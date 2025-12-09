---
title : "Cover Letter Functions"
#date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4.4. </b> "
---

In this lesson, we will build the **Cover Letter Generator** feature.

Unlike the Resume (CV), which is static data that rarely changes, the Cover Letter needs to be customized ("tailor") for each company and applied position. This is very time-consuming if done manually, but it is a perfect problem for AI.

### Data Flow
1.  **Input:** User provides Company Name, Position, and Job Description (JD).
2.  **Process:** Backend combines this information into a Prompt sent to Claude 3.
3.  **Output:** AI returns a sample letter. Backend saves to DB and returns to Frontend.

---

## 1. Model (Entity Layer)

Since a user can create multiple cover letters for different companies, the `SK` (Sort Key) will be unique for each letter (using UUID).

```java
@Data
@DynamoDbBean
public class CoverLetterEntity {
    private String pk; // Format: USER#<cognito_sub>
    private String sk; // Format: LETTER#<uuid>

    private String content; // Markdown do AI Bedrock viết
    private String jobDescription;
    private String companyName;
    private String jobTitle;
    private String status; // "draft", "completed"

    private String createdAt;
    private String updatedAt;

    @DynamoDbPartitionKey
    @DynamoDbAttribute("PK")
    public String getPk() { return pk; }

    @DynamoDbSortKey
    @DynamoDbAttribute("SK")
    public String getSk() { return sk; }
}
```

Key Strategy:

* PK: USER#{userId}.
* SK: LETTER#{uuid} (Example: LETTER#550e8400-e29b...).

---

## 2. Repository (Data Access Layer)

We need functions to Save, Retrieve the list, and Delete letters.

Inherits from the parent class AbstractDynamoRepository

```java

@Repository
public class CoverLetterRepository extends AbstractDynamoRepository<CoverLetterEntity> {

    public CoverLetterRepository(DynamoDbEnhancedClient client) {
        super(client, CoverLetterEntity.class);
    }

    // Tìm tất cả Cover Letter của một User
    // PK: USER#<userId>, SK bắt đầu bằng LETTER#
    // Vì AbstractRepository đã có findAllByPartitionKey, ta chỉ cần gọi nó
    public List<CoverLetterEntity> findAllByUserId(String userId) {
        String pk = "USER#" + userId;
        // Lưu ý: findAllByPartitionKey sẽ lấy cả UserEntity và ResumeEntity nếu chung PK
        // Tuy nhiên, vì ta map TableSchema với CoverLetterEntity class,
        // SDK sẽ tự động filter hoặc map, nhưng để an toàn nhất với Single Table Design,
        // Ta nên dùng query với điều kiện SK begins_with "LETTER#"
        // (Để đơn giản trong demo này, giả sử hàm findAllByPartitionKey của bạn lọc được type,
        // hoặc ta chấp nhận lấy về rồi filter ở Service).

        // Cách tốt nhất: Viết query cụ thể ở đây
        return findAllByPartitionKey(pk);
    }

    public CoverLetterEntity findById(String userId, String letterId) {
        return super.findById("USER#" + userId, "LETTER#" + letterId);
    }

    public void deleteById(String userId, String letterId) {
        super.delete("USER#" + userId, "LETTER#" + letterId);
    }
}
```

---

## 3. Service

This is the most important part: How to talk to AI.

Combine with BedrockService to implement the generateCoverLetter function using AI

```java

@Service
public class CoverLetterService {

    private static final Logger logger = LoggerFactory.getLogger(CoverLetterService.class);

    private final CoverLetterRepository coverLetterRepository;
    private final UserRepository userRepository;
    private final BedrockService bedrockService;

    public CoverLetterService(CoverLetterRepository coverLetterRepository,
                              UserRepository userRepository,
                              BedrockService bedrockService) {
        this.coverLetterRepository = coverLetterRepository;
        this.userRepository = userRepository;
        this.bedrockService = bedrockService;
    }

    // 1. Generate Cover Letter (Create)
    public CoverLetterEntity generateCoverLetter(String userId, CoverLetterRequest request) {
        // Validate
        if (request.getJobTitle() == null || request.getCompanyName() == null || request.getJobDescription() == null) {
            throw new IllegalArgumentException("Missing required fields (jobTitle, companyName, jobDescription)");
        }

        // Lấy thông tin User để AI viết cho chuẩn
        UserEntity user = userRepository.findById("USER#" + userId, "METADATA");
        if (user == null) throw new RuntimeException("User not found");

        String skills = user.getSkills() != null ? String.join(", ", user.getSkills()) : "Not specified";

        // Prompt Engineering (Copy từ logic cũ của bạn)
        String prompt = String.format("""
            Write a professional cover letter for a %s position at %s.
            
            About the candidate:
            - Industry: %s
            - Years of Experience: %d
            - Skills: %s
            - Professional Background: %s
            
            Job Description:
            %s
            
            Requirements:
            1. Use a professional, enthusiastic tone
            2. Highlight relevant skills and experience
            3. Show understanding of the company's needs
            4. Keep it concise (max 400 words)
            5. Use proper business letter formatting in markdown
            6. Include specific examples of achievements
            7. Relate candidate's background to job requirements
            
            Format the letter in markdown. Do not include any preamble or postscript.
            """,
                request.getJobTitle(), request.getCompanyName(),
                user.getIndustry(), user.getExperience(), skills, user.getBio(),
                request.getJobDescription()
        );

        // Gọi Bedrock
        String aiContent = bedrockService.generateTextCorrection(prompt);

        // Tạo Entity lưu xuống DB
        CoverLetterEntity entity = new CoverLetterEntity();
        String letterId = UUID.randomUUID().toString();

        entity.setPk("USER#" + userId);
        entity.setSk("LETTER#" + letterId); // SK unique
        entity.setContent(aiContent);
        entity.setJobTitle(request.getJobTitle());
        entity.setCompanyName(request.getCompanyName());
        entity.setJobDescription(request.getJobDescription());
        entity.setStatus("completed");
        entity.setCreatedAt(Instant.now().toString());
        entity.setUpdatedAt(Instant.now().toString());

        coverLetterRepository.save(entity);
        logger.info("Generated cover letter {} for user {}", letterId, userId);

        return entity;
    }

    // 2. Get All
    public List<CoverLetterEntity> getAllCoverLetters(String userId) {
        // Lấy về tất cả item của user, sau đó lọc ra những cái là Cover Letter
        // (Trong môi trường production nên dùng Query SK begins_with "LETTER#" để tối ưu)
        List<CoverLetterEntity> allItems = coverLetterRepository.findAllByUserId(userId);

        return allItems.stream()
                .filter(item -> item.getSk().startsWith("LETTER#"))
                .collect(Collectors.toList());
    }

    // 3. Get One
    public CoverLetterEntity getCoverLetter(String userId, String letterId) {
        return coverLetterRepository.findById(userId, letterId);
    }

    // 4. Delete
    public void deleteCoverLetter(String userId, String letterId) {
        coverLetterRepository.deleteById(userId, letterId);
        logger.info("Deleted cover letter {} for user {}", letterId, userId);
    }
}
```

---

## 4. Function (Router Layer)

This function needs to handle more API Endpoints (List, Get One, Create, Delete).

```java

@Configuration
public class CoverLetterFunctions {

    private static final Logger logger = LoggerFactory.getLogger(CoverLetterFunctions.class);
    private final CoverLetterService coverLetterService;
    private final ObjectMapper objectMapper;

    public CoverLetterFunctions(CoverLetterService coverLetterService) {
        this.coverLetterService = coverLetterService;

        // --- CẤU HÌNH JACKSON THỦ CÔNG (QUAN TRỌNG) ---
        // Giúp serialize được các object không có getter/setter chuẩn hoặc field private
        this.objectMapper = new ObjectMapper();
        this.objectMapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY);
        this.objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
    }

    /**
     * HÀM TỔNG (ROUTER)
     * Phân phối request dựa trên Path và Method
     */
    @Bean
    public Function<Map<String, Object>, Map<String, Object>> coverLetterHandler() {
        return event -> {
            try {
                // 1. Trích xuất thông tin Request
                String path = extractPath(event);
                String method = extractHttpMethod(event);
                logger.info("CoverLetter Router -> Path: [{}], Method: [{}]", path, method);

                // 2. Xác thực User (Auth)
                Map<String, String> headers = normalizeHeaders(event);
                String userId = extractUserIdOrThrow(headers);

                // 3. Phân luồng xử lý (Routing)

                // Case 1: GET /cover-letters (List All)
                if (path.endsWith("/cover-letters") && "GET".equalsIgnoreCase(method)) {
                    return handleListCoverLetters(userId);
                }

                // Case 2: POST /cover-letters (Generate)
                if (path.endsWith("/cover-letters") && "POST".equalsIgnoreCase(method)) {
                    return handleGenerateCoverLetter(userId, event);
                }

                // Case 3: GET /cover-letters/{id} (Get One)
                if (path.contains("/cover-letters/") && "GET".equalsIgnoreCase(method)) {
                    String id = extractId(event, path);
                    return handleGetOneCoverLetter(userId, id);
                }

                // Case 4: DELETE /cover-letters/{id}
                if (path.contains("/cover-letters/") && "DELETE".equalsIgnoreCase(method)) {
                    String id = extractId(event, path);
                    return handleDeleteCoverLetter(userId, id);
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

    private Map<String, Object> handleListCoverLetters(String userId) {
        logger.info("Fetching all cover letters for user: {}", userId);
        List<CoverLetterEntity> list = coverLetterService.getAllCoverLetters(userId);
        return buildResponse(200, list);
    }

    private Map<String, Object> handleGenerateCoverLetter(String userId, Map<String, Object> event) throws Exception {
        String bodyString = extractBodyContent(event);
        if (bodyString == null || bodyString.trim().isEmpty()) {
            throw new IllegalArgumentException("Request body is required");
        }

        logger.debug("Generating cover letter with body: {}", bodyString);
        CoverLetterRequest req = objectMapper.readValue(bodyString, CoverLetterRequest.class);

        CoverLetterEntity created = coverLetterService.generateCoverLetter(userId, req);

        logger.info("Successfully generated cover letter. ID: {}", created.getSk());
        return buildResponse(200, created);
    }

    private Map<String, Object> handleGetOneCoverLetter(String userId, String letterId) {
        if (letterId == null || letterId.isEmpty()) throw new IllegalArgumentException("ID is missing");

        logger.info("Fetching cover letter ID: {}", letterId);
        CoverLetterEntity item = coverLetterService.getCoverLetter(userId, letterId);

        if (item == null) {
            return buildResponse(404, Map.of("error", "Cover letter not found"));
        }
        return buildResponse(200, item);
    }

    private Map<String, Object> handleDeleteCoverLetter(String userId, String letterId) {
        if (letterId == null || letterId.isEmpty()) throw new IllegalArgumentException("ID is missing");

        logger.info("Deleting cover letter ID: {}", letterId);
        coverLetterService.deleteCoverLetter(userId, letterId);

        return buildResponse(200, Map.of("status", "deleted", "id", letterId));
    }

    // =========================================================================
    // HELPERS (Tiện ích)
    // =========================================================================

    private String extractId(Map<String, Object> event, String path) {
        // Ưu tiên lấy từ Path Parameters của API Gateway
        if (event.get("pathParameters") instanceof Map) {
            Map<?, ?> params = (Map<?, ?>) event.get("pathParameters");
            if (params != null && params.get("id") != null) {
                return params.get("id").toString();
            }
        }
        // Fallback: Cắt chuỗi URL
        String[] parts = path.split("/");
        return parts.length > 0 ? parts[parts.length - 1] : null;
    }

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

            // Dùng getUrlDecoder cho JWT
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
            // Dùng objectMapper thủ công để serialize (tránh lỗi ClassCastException)
            response.put("body", objectMapper.writeValueAsString(body));
        } catch (Exception e) {
            response.put("statusCode", 500);
            response.put("body", "{\"error\": \"JSON Serialization Error\"}");
        }
        return response;
    }
}
```