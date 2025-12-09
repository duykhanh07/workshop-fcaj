---
title : "Industry Functions"
#date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.4.2. </b> "
---

In the final lesson of the Backend section, we will build the **Industry Insight** feature.

This feature allows users to enter a profession (e.g., "React Developer") and receive information such as:
* Average salary.
* Required skills.
* Recruitment trends.

### Strategy: Cache-First
Calling AI (Bedrock) incurs a cost for each token. Market information does not change every second. Therefore, we will apply the following strategy:
1.  User asks about "Java Developer".
2.  Check DynamoDB to see if this information already exists.
3.  If **YES**: Return immediately (Fast, Cheap).
4.  If **NO**: Call Bedrock for analysis -> Save to DB -> Return (Slightly slower, but only incurs cost once).

---

## 1. Model (Entity Layer)

Define the data structure to store market analysis results in DynamoDB.

```java

@Data
@DynamoDbBean
public class IndustryInsightEntity {
    private String pk; // Format: INDUSTRY#<tên_ngành> (Ví dụ: INDUSTRY#tech-software)
    private String sk; // Format: METADATA

    // AI Bedrock Generated Data
    private Float growthRate;
    private String demandLevel; // "High", "Medium", "Low"
    private String marketOutlook;

    private List<String> topSkills;
    private List<String> keyTrends;
    private List<String> recommendedSkills;

    // Nested JSON cho lương
    private List<SalaryRangeItem> salaryRanges;

    private String lastUpdated;
    private String nextUpdate;

    @DynamoDbPartitionKey
    @DynamoDbAttribute("PK")
    public String getPk() { return pk; }

    @DynamoDbSortKey
    @DynamoDbAttribute("SK")
    public String getSk() { return sk; }

    // Inner Class cho dải lương
    @Data
    @DynamoDbBean
    public static class SalaryRangeItem {
        private String role;
        private Float min;
        private Float max;
        private Float median;
        private String location;
    }
}
```
Key Naming Rules:

* PK: INSIGHT#{lowercase_industry_name} (Example: INSIGHT#java-developer).
* SK: METADATA.

---

## 2. Repository (Data Access Layer)
Interact with DynamoDB to search for and store Insights.

Inherits from the parent class AbstractDynamoRepository

```java

@Repository
public class IndustryInsightRepository extends AbstractDynamoRepository<IndustryInsightEntity> {

    public IndustryInsightRepository(DynamoDbEnhancedClient client) {
        super(client, IndustryInsightEntity.class);
    }

    /**
     * Tìm Insight dựa trên tên ngành.
     * PK: INDUSTRY#<industryName>
     * SK: METADATA
     */
    public IndustryInsightEntity findByIndustry(String industryName) {
        if (industryName == null || industryName.trim().isEmpty()) {
            return null;
        }
        // Chuẩn hóa key (ví dụ: lowercase hoặc slugify nếu cần thiết)
        // Ở đây giả định lưu nguyên văn chuỗi
        String pk = "INDUSTRY#" + industryName;
        String sk = "METADATA";

        return findById(pk, sk);
    }
}
```

---

## 3. Service (AI Integration Layer)

Where logic is processed in combination with BedrockService to perform AI-powered statistical functions.

```java

@Service
public class IndustryInsightService {

    private static final Logger logger = LoggerFactory.getLogger(IndustryInsightService.class);

    private final UserRepository userRepository;
    private final IndustryInsightRepository insightRepository;
    private final BedrockService bedrockService;

    public IndustryInsightService(UserRepository userRepository,
                                  IndustryInsightRepository insightRepository,
                                  BedrockService bedrockService) {
        this.userRepository = userRepository;
        this.insightRepository = insightRepository;
        this.bedrockService = bedrockService;
    }

    public IndustryInsightEntity getIndustryInsights(String userId) {
        // 1. Validate User ID
        if (userId == null || userId.isEmpty()) {
            throw new IllegalArgumentException("User ID is required");
        }

        // 2. Lấy thông tin User để biết họ làm ngành gì
        // PK: USER#<id>, SK: METADATA
        UserEntity user = userRepository.findById("USER#" + userId, "METADATA");

        if (user == null) {
            logger.error("User not found: {}", userId);
            throw new RuntimeException("User not found");
        }

        // 3. Kiểm tra xem User đã chọn ngành chưa
        String industry = user.getIndustry();
        if (industry == null || industry.trim().isEmpty()) {
            logger.warn("User {} has not selected an industry yet", userId);
            // Có thể throw lỗi hoặc trả về null tùy logic frontend
            throw new RuntimeException("User has not selected an industry");
        }

        // 4. Tìm Insight trong DB
        logger.info("Fetching insights for industry: {}", industry);
        IndustryInsightEntity insight = insightRepository.findByIndustry(industry);

        // 5. Nếu chưa có hoặc (Optional: đã hết hạn cache), gọi AI tạo mới
        // Ở đây logic gốc là "If no insights exist", tôi giữ nguyên logic đó.
        if (insight == null) {
            logger.info("Insight not found for '{}'. Generating via Bedrock AI...", industry);

            // Gọi AI
            insight = bedrockService.generateIndustryInsights(industry);

            // Bổ sung các thông tin quản lý DB
            insight.setPk("INDUSTRY#" + industry);
            insight.setSk("METADATA");
            insight.setLastUpdated(Instant.now().toString());
            // Set next update = now + 7 days
            insight.setNextUpdate(Instant.now().plus(7, ChronoUnit.DAYS).toString());

            // Lưu vào DB
            insightRepository.save(insight);
            logger.info("Saved new insights for '{}'", industry);
        } else {
            logger.info("Found existing insights for '{}' in DB", industry);
        }

        return insight;
    }
}
```

---

## 4. Function (Controller Layer)

The final Lambda function to build the API.

```java

@Configuration
public class IndustryFunctions {

    private static final Logger logger = LoggerFactory.getLogger(IndustryFunctions.class);

    private final IndustryInsightService insightService;
    private final ObjectMapper objectMapper;

    public IndustryFunctions(IndustryInsightService insightService) {
        this.insightService = insightService;
        this.objectMapper = new ObjectMapper();
        this.objectMapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY);
        this.objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
    }

    /**
     * HÀM TỔNG (DISPATCHER) CHO INDUSTRY
     * Quản lý tất cả route liên quan đến /industry-insights
     */
    @Bean
    public Function<Map<String, Object>, Map<String, Object>> industryInsightHandler() {
        return event -> {
            try {
                String path = extractPath(event);
                String method = extractHttpMethod(event);
                logger.info("Industry Handler -> Path: {}, Method: {}", path, method);

                // Chuẩn hóa Header & Auth
                Map<String, String> headers = normalizeHeaders(event);
                String userId = extractUserIdOrThrow(headers);

                // --- ROUTING LOGIC ---
                // Route 1: GET /industry-insights
                if (path.endsWith("/industry-insights") && "GET".equalsIgnoreCase(method)) {
                    return handleGetIndustryInsights(userId);
                }

                // Route 2 (Ví dụ tương lai): POST /industry-insights/refresh (Force update AI)
                // if (path.endsWith("/refresh") ...) { return handleRefresh(userId); }

                return buildResponse(404, Map.of("error", "Route not found in Industry Function"));

            } catch (SecurityException e) {
                return buildResponse(401, Map.of("error", e.getMessage()));
            } catch (Exception e) {
                logger.error("System Error", e);
                return buildResponse(500, Map.of("error", e.getMessage()));
            }
        };
    }

    // --- LOGIC CON ---
    private Map<String, Object> handleGetIndustryInsights(String userId) {
        IndustryInsightEntity result = insightService.getIndustryInsights(userId);
        return buildResponse(200, result);
    }

    // --- CÁC HÀM HELPER (Copy từ UserFunctions sang hoặc gom vào 1 file Utils dùng chung) ---
    // (Để code ngắn gọn ở đây mình ví dụ các hàm quan trọng, bạn copy phần decode token từ UserFunctions sang nhé)

    private String extractPath(Map<String, Object> event) {
        return event.get("rawPath") != null ? event.get("rawPath").toString() : "";
    }

    private String extractHttpMethod(Map<String, Object> event) {
        Map<String, Object> req = (Map) event.get("requestContext");
        if (req != null) {
            Map<String, Object> http = (Map) req.get("http");
            if (http != null) return http.get("method").toString();
        }
        return "GET";
    }

    private Map<String, String> normalizeHeaders(Map<String, Object> event) {
        Map<String, String> headers = new HashMap<>();
        Object headersObj = event.get("headers");
        if (headersObj instanceof Map) {
            Map<?, ?> rawMap = (Map<?, ?>) headersObj;
            for (Map.Entry<?, ?> entry : rawMap.entrySet()) {
                headers.put(entry.getKey().toString().toLowerCase(), entry.getValue().toString());
            }
        }
        return headers;
    }

    private String extractUserIdOrThrow(Map<String, String> headers) {
        // ... Logic extract token giống hệt UserFunctions ...
        // (Lưu ý: Bạn nên tạo class JwtUtils để tái sử dụng đoạn này đỡ phải copy paste)
        String authHeader = headers.get("authorization");
        if (authHeader != null && authHeader.startsWith("Bearer ")) {
            try {
                String token = authHeader.substring(7);
                String[] parts = token.split("\\.");
                String payloadJson = new String(Base64.getUrlDecoder().decode(parts[1])); // Nhớ dùng Base64UrlDecoder
                return new ObjectMapper().readTree(payloadJson).get("sub").asText();
            } catch (Exception e) { logger.error("Token error", e); }
        }
        throw new SecurityException("Invalid Token");
    }

    private Map<String, Object> buildResponse(int statusCode, Object body) {
        Map<String, Object> response = new HashMap<>();
        response.put("statusCode", statusCode);
        Map<String, String> headers = new HashMap<>();
        headers.put("Content-Type", "application/json");
        response.put("headers", headers);
        try {
            response.put("body", objectMapper.writeValueAsString(body));
        } catch (Exception e) {
            response.put("statusCode", 500);
            response.put("body", "{}");
        }
        return response;
    }
}
```