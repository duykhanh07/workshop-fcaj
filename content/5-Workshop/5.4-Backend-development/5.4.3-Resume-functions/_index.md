---
title : "Resume Functions"
#date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.4.3. </b> "
---

In this lesson, we will build the **Resume Function**. This is not just a place to store CV content but also a "writing assistant" for users.

### Key Features:
1.  **Storage:** Save the entire CV content in **Markdown** format to DynamoDB. Why Markdown? Because it is lightweight, easy to format, and easy to convert to PDF.
2.  **Improvement:** Send raw experience descriptions to AI (Claude 3) to rewrite them more professionally.

---

## 1. Model (Entity Layer)

Define the structure for storing CVs in DynamoDB.

```java
@Data
@DynamoDbBean
public class ResumeEntity {
    private String pk; // Format: USER#<cognito_sub>
    private String sk; // Format: RESUME

    private String content; // Markdown text
    private Double atsScore; // Điểm số ATS
    private String feedback; // Feedback từ Bedrock AI

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

* PK: USER#{userId} (Linked to the user).
* SK: RESUME (Each user has only 1 main CV draft in this context).

---

## 2. Repository (Data Access Layer)

Inherits from the parent class AbstractDynamoRepository.

```java

@Repository
public class ResumeRepository extends AbstractDynamoRepository<ResumeEntity> {

    public ResumeRepository(DynamoDbEnhancedClient client) {
        super(client, ResumeEntity.class);
    }

    // Tìm Resume theo UserID (Quan hệ 1-1)
    // PK: USER#<userId>, SK: RESUME
    public ResumeEntity findByUserId(String userId) {
        String pk = "USER#" + userId;
        String sk = "RESUME";
        return findById(pk, sk);
    }
}
```

---

## 3. Service

This is the most interesting part. The improveWithAI function will call Amazon Bedrock.

Combine with BedrockService to implement the improveWithAI function using AI

```java

@Service
public class ResumeService {

    private static final Logger logger = LoggerFactory.getLogger(ResumeService.class);

    private final ResumeRepository resumeRepository;
    private final UserRepository userRepository;
    private final BedrockService bedrockService;

    public ResumeService(ResumeRepository resumeRepository, UserRepository userRepository, BedrockService bedrockService) {
        this.resumeRepository = resumeRepository;
        this.userRepository = userRepository;
        this.bedrockService = bedrockService;
    }

    // 1. Save Resume (Upsert)
    public ResumeEntity saveResume(String userId, String content) {
        if (content == null || content.trim().isEmpty()) {
            throw new IllegalArgumentException("Resume content cannot be empty");
        }

        // Kiểm tra user có tồn tại không
        // (Tùy chọn: nếu tin tưởng token thì có thể bỏ qua bước này để tiết kiệm 1 RCU)

        ResumeEntity resume = resumeRepository.findByUserId(userId);

        if (resume == null) {
            logger.info("Creating new resume for user: {}", userId);
            resume = new ResumeEntity();
            resume.setPk("USER#" + userId);
            resume.setSk("RESUME");
            resume.setCreatedAt(Instant.now().toString());
        } else {
            logger.info("Updating existing resume for user: {}", userId);
        }

        resume.setContent(content);
        resume.setUpdatedAt(Instant.now().toString());

        resumeRepository.save(resume);
        return resume;
    }

    // 2. Get Resume
    public ResumeEntity getResume(String userId) {
        logger.debug("Fetching resume for user: {}", userId);
        return resumeRepository.findByUserId(userId);
    }

    // 3. Improve Content with AI
    public String improveWithAI(String userId, String currentContent, String type) {
        // Validation
        if (currentContent == null || currentContent.isEmpty()) throw new IllegalArgumentException("Current content is required");
        if (type == null || type.isEmpty()) throw new IllegalArgumentException("Type is required");

        // Lấy thông tin User để biết Industry
        UserEntity user = userRepository.findById("USER#" + userId, "METADATA");
        if (user == null) throw new RuntimeException("User not found");

        String industry = user.getIndustry();
        if (industry == null) industry = "General Professional"; // Fallback nếu chưa có ngành

        logger.info("Improving resume section '{}' for industry '{}'", type, industry);

        // Tạo Prompt cho Bedrock (Claude 3)
        String prompt = String.format("""
            As an expert resume writer, improve the following %s description for a %s professional.
            Make it more impactful, quantifiable, and aligned with industry standards.
            Current content: "%s"

            Requirements:
            1. Use action verbs
            2. Include metrics and results where possible
            3. Highlight relevant technical skills
            4. Keep it concise but detailed
            5. Focus on achievements over responsibilities
            6. Use industry-specific keywords
            
            Format the response as a single paragraph without any additional text or explanations.
            """, type, industry, currentContent);

        // Gọi AI
        return bedrockService.generateTextCorrection(prompt);
    }
}
```

---

## 4. Function (Controller Layer)

This module needs to handle 3 main flows: Get CV, Save CV, and Call AI.

```java

@Configuration
public class ResumeFunctions {

    private static final Logger logger = LoggerFactory.getLogger(ResumeFunctions.class);

    private final ResumeService resumeService;
    private final ObjectMapper objectMapper;

    public ResumeFunctions(ResumeService resumeService) {
        this.resumeService = resumeService;

        // --- CẤU HÌNH JACKSON THỦ CÔNG (THEO YÊU CẦU) ---
        this.objectMapper = new ObjectMapper();
        // Cho phép đọc/ghi trực tiếp vào field (kể cả private)
        this.objectMapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY);
        // Bỏ qua lỗi nếu JSON có trường thừa
        this.objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
    }

    /**
     * HÀM TỔNG (ROUTER) CHO RESUME
     */
    @Bean
    public Function<Map<String, Object>, Map<String, Object>> resumeHandler() {
        return event -> {
            try {
                String path = extractPath(event);
                String method = extractHttpMethod(event);
                logger.info("Resume Handler -> Path: {}, Method: {}", path, method);

                Map<String, String> headers = normalizeHeaders(event);
                String userId = extractUserIdOrThrow(headers);

                // --- ROUTING ---

                // 1. GET /resume
                if (path.endsWith("/resume") && "GET".equalsIgnoreCase(method)) {
                    return handleGetResume(userId);
                }

                // 2. POST /resume (Save)
                if (path.endsWith("/resume") && "POST".equalsIgnoreCase(method)) {
                    return handleSaveResume(userId, event);
                }

                // 3. POST /resume/improve (AI Improve)
                if (path.endsWith("/resume/improve") && "POST".equalsIgnoreCase(method)) {
                    return handleImproveResume(userId, event);
                }

                return buildResponse(404, Map.of("error", "Route not found"));

            } catch (SecurityException e) {
                return buildResponse(401, Map.of("error", e.getMessage()));
            } catch (IllegalArgumentException e) {
                return buildResponse(400, Map.of("error", e.getMessage()));
            } catch (Exception e) {
                logger.error("System Error", e);
                return buildResponse(500, Map.of("error", e.getMessage()));
            }
        };
    }

    // --- LOGIC CON ---

    private Map<String, Object> handleGetResume(String userId) {
        ResumeEntity resume = resumeService.getResume(userId);
        // Nếu chưa có resume, trả về null (frontend sẽ hiển thị form trống)
        return buildResponse(200, resume);
    }

    private Map<String, Object> handleSaveResume(String userId, Map<String, Object> event) throws Exception {
        String bodyString = extractBodyContent(event);

        // Parse thủ công để lấy field 'content'
        JsonNode node = objectMapper.readTree(bodyString);
        if (!node.has("content")) {
            throw new IllegalArgumentException("Field 'content' is required");
        }
        String content = node.get("content").asText();

        ResumeEntity saved = resumeService.saveResume(userId, content);
        return buildResponse(200, saved);
    }

    private Map<String, Object> handleImproveResume(String userId, Map<String, Object> event) throws Exception {
        String bodyString = extractBodyContent(event);
        JsonNode node = objectMapper.readTree(bodyString);

        if (!node.has("current") || !node.has("type")) {
            throw new IllegalArgumentException("Fields 'current' and 'type' are required");
        }
        String current = node.get("current").asText();
        String type = node.get("type").asText();

        String improvedContent = resumeService.improveWithAI(userId, current, type);

        // Trả về JSON đơn giản
        return buildResponse(200, Map.of("improvedContent", improvedContent));
    }

    // --- HELPERS (Đã được chuẩn hóa và format đẹp) ---

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

            // QUAN TRỌNG: Dùng Base64UrlDecoder
            String payloadJson = new String(Base64.getUrlDecoder().decode(parts[1]));
            return objectMapper.readTree(payloadJson).get("sub").asText();
        } catch (Exception e) {
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
            response.put("body", objectMapper.writeValueAsString(body));
        } catch (Exception e) {
            response.put("statusCode", 500);
            response.put("body", "{\"error\": \"JSON Serialization Error\"}");
        }
        return response;
    }
}
```