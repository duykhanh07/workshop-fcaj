---
title : "Assessment Functions"
#date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.4.5. </b> "
---

Hàm `AssessmentFunctions` đóng vai trò là người phỏng vấn ảo. Nó thực hiện hai nhiệm vụ chính:
1.  **Generate Quiz:** Tạo ra bộ câu hỏi trắc nghiệm "tươi mới" mỗi lần, dựa trên ngành nghề của người dùng.
2.  **Save & History:** Lưu lại kết quả làm bài để người dùng theo dõi sự tiến bộ qua biểu đồ.

---

## 1. Model (Entity Layer)

Chúng ta cần lưu trữ kết quả của mỗi lần phỏng vấn.

```java
@Data
@DynamoDbBean
public class AssessmentEntity {
    private String pk; // Format: USER#<cognito_sub>
    private String sk; // Format: ASSESS#<uuid>

    private Double quizScore;
    private String category; // "Technical", "Behavioral"
    private String improvementTip; // AI Bedrock generated tip

    // Nested Object List (JSON)
    private List<QuestionItem> questions;

    private String createdAt;
    private String updatedAt;

    @DynamoDbPartitionKey
    @DynamoDbAttribute("PK")
    public String getPk() { return pk; }

    @DynamoDbSortKey
    @DynamoDbAttribute("SK")
    public String getSk() { return sk; }

    // Inner Class cho cấu trúc câu hỏi
    @Data
    @DynamoDbBean
    public static class QuestionItem {
        private String question;
        private String answer; // Đáp án mẫu
        private String userAnswer; // Câu trả lời của user
        private Boolean isCorrect;
        private String explanation; // AI giải thích tại sao đúng/sai
    }
}
```

Chiến lược khóa:

* PK: USER#{userId}.
* SK: ASSESSMENT#{timestamp} (Sử dụng timestamp để sắp xếp lịch sử theo thời gian).

---

## 2. Repository (Data Access Layer)

Kế thừa từ lớp cha AbstractDynamoRepository

```java

@Repository
public class AssessmentRepository extends AbstractDynamoRepository<AssessmentEntity> {

    public AssessmentRepository(DynamoDbEnhancedClient client) {
        super(client, AssessmentEntity.class);
    }

    // Lấy lịch sử làm bài của User
    public List<AssessmentEntity> findAllByUserId(String userId) {
        // Query theo PK = USER#<id>, sau đó lọc SK bắt đầu bằng ASSESS#
        // (Cách tối ưu hơn là dùng Query Conditional BeginsWith trong SDK,
        // nhưng dùng findAllByPartitionKey lọc mềm cũng ổn với số lượng ít)
        List<AssessmentEntity> allItems = findAllByPartitionKey("USER#" + userId);

        return allItems.stream()
                .filter(item -> item.getSk() != null && item.getSk().startsWith("ASSESS#"))
                // Sắp xếp theo ngày tạo (Mới nhất lên đầu) - Logic Java
                .sorted((a, b) -> b.getCreatedAt().compareTo(a.getCreatedAt()))
                .collect(Collectors.toList());
    }
}
```

----

## 3. Service
   
Kết hợp với BedrockService để thực hiện chức năng generateQuiz bằng AI

```java

@Service
public class AssessmentService {

    private static final Logger logger = LoggerFactory.getLogger(AssessmentService.class);

    private final AssessmentRepository assessmentRepository;
    private final UserRepository userRepository;
    private final BedrockService bedrockService;
    private final ObjectMapper objectMapper;

    public AssessmentService(AssessmentRepository assessmentRepository, UserRepository userRepository,
                             BedrockService bedrockService, ObjectMapper objectMapper) {
        this.assessmentRepository = assessmentRepository;
        this.userRepository = userRepository;
        this.bedrockService = bedrockService;
        this.objectMapper = objectMapper;
    }

    // 1. Generate Quiz
    public List<QuizQuestion> generateQuiz(String userId) {
        // Lấy thông tin User
        UserEntity user = userRepository.findById("USER#" + userId, "METADATA");
        if (user == null) throw new RuntimeException("User not found");

        String industry = user.getIndustry();
        String skills = user.getSkills() != null ? String.join(", ", user.getSkills()) : "";

        // Gọi AI
        String jsonResponse = bedrockService.generateQuizJson(industry, skills);

        // Parse JSON trả về List Questions
        try {
            // Claude có thể trả về text kèm markdown, cần clean
            String cleanedJson = jsonResponse.replaceAll("```json", "").replaceAll("```", "").trim();
            JsonNode root = objectMapper.readTree(cleanedJson);
            JsonNode questionsNode = root.get("questions");

            return Arrays.asList(objectMapper.treeToValue(questionsNode, QuizQuestion[].class));
        } catch (Exception e) {
            logger.error("Failed to parse Quiz JSON", e);
            throw new RuntimeException("Failed to parse AI response");
        }
    }

    // 2. Save Result & Generate Tip
    public AssessmentEntity saveQuizResult(String userId, SaveAssessmentRequest request) {
        UserEntity user = userRepository.findById("USER#" + userId, "METADATA");
        if (user == null) throw new RuntimeException("User not found");

        List<QuestionItem> questionResults = new ArrayList<>();
        List<String> wrongAnswersText = new ArrayList<>();

        // Map DTO sang Entity và tìm câu sai
        for (int i = 0; i < request.getQuestions().size(); i++) {
            QuizQuestion q = request.getQuestions().get(i);
            String userAnswer = request.getUserAnswers().get(i);
            boolean isCorrect = q.getCorrectAnswer().equals(userAnswer);

            QuestionItem item = new QuestionItem();
            item.setQuestion(q.getQuestion());
            item.setAnswer(q.getCorrectAnswer());
            item.setUserAnswer(userAnswer);
            item.setIsCorrect(isCorrect);
            item.setExplanation(q.getExplanation());

            questionResults.add(item);

            if (!isCorrect) {
                wrongAnswersText.add(String.format("Question: \"%s\"\nCorrect: \"%s\"\nUser Answer: \"%s\"",
                        q.getQuestion(), q.getCorrectAnswer(), userAnswer));
            }
        }

        // Tạo Improvement Tip nếu có câu sai
        String improvementTip = null;
        if (!wrongAnswersText.isEmpty()) {
            String prompt = String.format("""
                The user got the following %s technical interview questions wrong:
                %s
                
                Based on these mistakes, provide a concise, specific improvement tip.
                Focus on knowledge gaps. Keep it under 2 sentences. Encouraging tone.
                """, user.getIndustry(), String.join("\n\n", wrongAnswersText));

            improvementTip = bedrockService.generateTextCorrection(prompt); // Tái sử dụng hàm sinh text
        }

        // Lưu DB
        AssessmentEntity entity = new AssessmentEntity();
        entity.setPk("USER#" + userId);
        entity.setSk("ASSESS#" + UUID.randomUUID().toString());
        entity.setQuizScore(request.getScore());
        entity.setCategory("Technical");
        entity.setImprovementTip(improvementTip);
        entity.setQuestions(questionResults); // DynamoDB Enhanced tự convert List sang JSON
        entity.setCreatedAt(Instant.now().toString());
        entity.setUpdatedAt(Instant.now().toString());

        assessmentRepository.save(entity);
        return entity;
    }

    // 3. Get History
    public List<AssessmentEntity> getAssessments(String userId) {
        return assessmentRepository.findAllByUserId(userId);
    }
}
```

---

## 4. Function (Router Layer)

Hàm này xử lý 3 endpoints khác nhau cho quy trình phỏng vấn.

```java

@Configuration
public class AssessmentFunctions {

    private static final Logger logger = LoggerFactory.getLogger(AssessmentFunctions.class);

    private final AssessmentService assessmentService;
    private final ObjectMapper objectMapper;

    public AssessmentFunctions(AssessmentService assessmentService) {
        this.assessmentService = assessmentService;

        // --- CẤU HÌNH JACKSON THỦ CÔNG ---
        // Đảm bảo parse được mọi loại object, kể cả private fields
        this.objectMapper = new ObjectMapper();
        this.objectMapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY);
        this.objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
    }

    /**
     * HÀM TỔNG (ROUTER) CHO ASSESSMENT
     */
    @Bean
    public Function<Map<String, Object>, Map<String, Object>> assessmentHandler() {
        return event -> {
            try {
                // 1. Trích xuất thông tin Request
                String path = extractPath(event);
                String method = extractHttpMethod(event);
                logger.info("Assessment Router -> Path: [{}], Method: [{}]", path, method);

                // 2. Xác thực User (Auth)
                Map<String, String> headers = normalizeHeaders(event);
                String userId = extractUserIdOrThrow(headers);

                // 3. Phân luồng xử lý (Routing)

                // Case 1: POST /interview/generate (Generate Quiz)
                if (path.endsWith("/interview/generate") && "POST".equalsIgnoreCase(method)) {
                    return handleGenerateQuiz(userId);
                }

                // Case 2: POST /interview/save (Save Result)
                if (path.endsWith("/interview/save") && "POST".equalsIgnoreCase(method)) {
                    return handleSaveQuizResult(userId, event);
                }

                // Case 3: GET /interview/history (Get List)
                if (path.endsWith("/interview/history") && "GET".equalsIgnoreCase(method)) {
                    return handleGetAssessmentHistory(userId);
                }

                return buildResponse(404, Map.of("error", "Route not found: " + path));

            } catch (SecurityException e) {
                logger.warn("Auth Error: {}", e.getMessage());
                return buildResponse(401, Map.of("error", e.getMessage()));
            } catch (IllegalArgumentException e) {
                logger.warn("Validation Error: {}", e.getMessage());
                return buildResponse(400, Map.of("error", e.getMessage()));
            } catch (Exception e) {
                logger.error("System Error", e);
                return buildResponse(500, Map.of("error", e.getMessage()));
            }
        };
    }

    // =========================================================================
    // LOGIC CON (SUB-HANDLERS)
    // =========================================================================

    private Map<String, Object> handleGenerateQuiz(String userId) {
        logger.info("Generating interview quiz for user: {}", userId);

        // Gọi Service (Có thể mất thời gian do gọi AI)
        List<QuizQuestion> quiz = assessmentService.generateQuiz(userId);

        logger.info("Generated {} questions successfully.", quiz.size());
        return buildResponse(200, Map.of("questions", quiz));
    }

    private Map<String, Object> handleSaveQuizResult(String userId, Map<String, Object> event) throws Exception {
        String bodyString = extractBodyContent(event);

        if (bodyString == null || bodyString.trim().isEmpty()) {
            throw new IllegalArgumentException("Request body is required for saving result");
        }

        logger.debug("Saving quiz result payload: {}", bodyString);

        // Parse JSON sang DTO
        SaveAssessmentRequest req = objectMapper.readValue(bodyString, SaveAssessmentRequest.class);

        // Validate cơ bản DTO
        if (req.getQuestions() == null || req.getQuestions().isEmpty()) {
            throw new IllegalArgumentException("Questions list cannot be empty");
        }

        // Gọi Service
        AssessmentEntity result = assessmentService.saveQuizResult(userId, req);

        logger.info("Saved assessment result ID: {}", result.getSk());
        return buildResponse(200, result);
    }

    private Map<String, Object> handleGetAssessmentHistory(String userId) {
        logger.info("Fetching assessment history for user: {}", userId);

        List<AssessmentEntity> list = assessmentService.getAssessments(userId);

        logger.info("Found {} past assessments.", list.size());
        return buildResponse(200, list);
    }

    // =========================================================================
    // HELPERS (Tiện ích - Copy y hệt từ các file trước)
    // =========================================================================

    private String extractPath(Map<String, Object> event) {
        return event.get("rawPath") != null ? event.get("rawPath").toString() : "";
    }

    private String extractHttpMethod(Map<String, Object> event) {
        try {
            Map req = (Map) event.get("requestContext");
            if (req != null) {
                Map http = (Map) req.get("http");
                if (http != null) return http.get("method").toString();
            }
        } catch (Exception e) { /* ignore */ }
        return "GET";
    }

    private Map<String, String> normalizeHeaders(Map<String, Object> event) {
        Map<String, String> headers = new HashMap<>();
        Object headersObj = event.get("headers");
        if (headersObj instanceof Map) {
            Map<?, ?> rawMap = (Map<?, ?>) headersObj;
            for (Map.Entry<?, ?> entry : rawMap.entrySet()) {
                if (entry.getKey() != null && entry.getValue() != null) {
                    headers.put(entry.getKey().toString().toLowerCase(), entry.getValue().toString());
                }
            }
        }
        return headers;
    }

    private String extractUserIdOrThrow(Map<String, String> headers) {
        String authHeader = headers.get("authorization");
        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            throw new SecurityException("Missing or invalid Authorization header");
        }
        try {
            String token = authHeader.substring(7);
            String[] parts = token.split("\\.");
            if (parts.length < 2) throw new SecurityException("Invalid JWT format");

            // Dùng getUrlDecoder cho JWT chuẩn
            String payloadJson = new String(Base64.getUrlDecoder().decode(parts[1]));
            return objectMapper.readTree(payloadJson).get("sub").asText();
        } catch (Exception e) {
            logger.error("Token decode error", e);
            throw new SecurityException("Token validation failed");
        }
    }

    private String extractBodyContent(Map<String, Object> event) {
        Object bodyObj = event.get("body");
        if (bodyObj == null) return null;

        String bodyString;
        if (bodyObj instanceof String) {
            bodyString = (String) bodyObj;
        } else {
            try {
                bodyString = objectMapper.writeValueAsString(bodyObj);
            } catch (Exception e) { return "{}"; }
        }

        Object isBase64Obj = event.get("isBase64Encoded");
        if (isBase64Obj != null && Boolean.parseBoolean(isBase64Obj.toString())) {
            try {
                bodyString = new String(Base64.getDecoder().decode(bodyString));
            } catch (Exception e) { throw new RuntimeException("Invalid Base64 body"); }
        }
        return bodyString;
    }

    private Map<String, Object> buildResponse(int statusCode, Object body) {
        Map<String, Object> response = new HashMap<>();
        response.put("statusCode", statusCode);
        response.put("headers", Map.of("Content-Type", "application/json"));
        try {
            // Dùng objectMapper thủ công để serialize
            response.put("body", objectMapper.writeValueAsString(body));
        } catch (Exception e) {
            response.put("statusCode", 500);
            response.put("body", "{\"error\": \"JSON Serialization Error\"}");
        }
        return response;
    }
}
```