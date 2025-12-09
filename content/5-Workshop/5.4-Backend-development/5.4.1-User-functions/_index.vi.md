---
title : "User Functions"
#date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.4.1. </b> "
---

Trong bài này, chúng ta sẽ xây dựng module đầu tiên: **Quản lý Hồ sơ Người dùng (User Profile)**.

Chúng ta sẽ đi theo quy trình chuẩn của Spring Boot:
1.  **Model:** Định nghĩa cấu trúc dữ liệu.
2.  **Repository:** Tương tác với DynamoDB.
3.  **Service:** Xử lý nghiệp vụ.
4.  **Function:** Tiếp nhận request từ API Gateway.

---

## 1. Model (Entity Layer)

Đầu tiên, chúng ta cần ánh xạ đối tượng Java với bảng trong DynamoDB.

Chúng ta sử dụng các Annotation của **AWS SDK v2 Enhanced Client**.

```java
@Data
@DynamoDbBean
@NoArgsConstructor
@AllArgsConstructor
public class UserEntity {
    private String pk; // Format: USER#<cognito_sub>
    private String sk; // Format: METADATA

    // Core fields
    private String email;
    private String name;
    private String imageUrl;
    private String industry; // Link tới IndustryInsight (Lưu dạng chuỗi text)

    // Profile fields
    private String bio;
    private Integer experience;
    private List<String> skills; // DynamoDB hỗ trợ List<String> tự động

    // Timestamps (Lưu dạng ISO String cho dễ đọc: "2025-11-30T10:00:00Z")
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

Chiến lược khóa:

* PK: USER#{userId} (Gom nhóm dữ liệu theo User).
* SK: METADATA (Định danh đây là thông tin profile).

## 2. Repository (Data Access Layer)
Lớp này chịu trách nhiệm "nói chuyện" với DynamoDB.

Kế thừa từ lớp cha AbstractDynamoRepository

```java

@Repository
public class UserRepository extends AbstractDynamoRepository<UserEntity> {

    public UserRepository(DynamoDbEnhancedClient client) {
        // Truyền Class type để Abstract Repository biết map vào object nào
        super(client, UserEntity.class);
    }

}
```
## 3. Service (Business Logic Layer)
Đây là nơi chứa logic nghiệp vụ. 

```java

@Service
public class UserService {

    private static final Logger logger = LoggerFactory.getLogger(UserService.class);

    // Định nghĩa các hằng số Key Pattern để tránh Hardcode rải rác
    private static final String USER_PK_PREFIX = "USER#";
    private static final String METADATA_SK = "METADATA";

    private final UserRepository userRepository;

    // Constructor Injection
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    /**
     * Lấy thông tin User Profile
     */
    public UserEntity getUserProfile(String userId) {
        // 1. Validator: Kiểm tra đầu vào
        validateUserId(userId);

        // 2. Tạo Key chuẩn Single Table Design
        String pk = USER_PK_PREFIX + userId;

        logger.debug("Fetching profile for user: {}", userId);

        // 3. Gọi Repository (Đã có sẵn hàm findById)
        return userRepository.findById(pk, METADATA_SK);
    }

    /**
     * Cập nhật (hoặc tạo mới) User Profile
     */
    public UserEntity updateUserProfile(String userId, String email, UpdateUserRequest request) {
        // 1. Validator
        validateUserId(userId);
        if (request == null) {
            throw new IllegalArgumentException("Update request cannot be null");
        }

        logger.info("Processing profile update for user: {}", userId);

        // 2. Lấy user cũ để kiểm tra tồn tại
        UserEntity user = getUserProfile(userId);

        // 3. Logic: Create if not exists (Upsert)
        if (user == null) {
            logger.info("User not found via ID {}, creating new profile...", userId);
            if (email == null || email.isEmpty()) {
                logger.warn("Creating new user but Email is missing!");
            }

            user = new UserEntity();
            user.setPk(USER_PK_PREFIX + userId);
            user.setSk(METADATA_SK);
            user.setCreatedAt(Instant.now().toString());
            user.setEmail(email); // Chỉ set email khi tạo mới
        }

        // 4. Mapping dữ liệu từ DTO sang Entity (Chỉ update nếu có dữ liệu)
        boolean isUpdated = false;

        if (hasValue(request.getIndustry())) {
            user.setIndustry(request.getIndustry());
            isUpdated = true;
        }
        if (hasValue(request.getBio())) {
            user.setBio(request.getBio());
            isUpdated = true;
        }
        if (request.getExperience() != null) {
            user.setExperience(request.getExperience());
            isUpdated = true;
        }
        if (request.getSkills() != null && !request.getSkills().isEmpty()) {
            user.setSkills(request.getSkills());
            isUpdated = true;
        }

        // 5. Lưu xuống DB
        user.setUpdatedAt(Instant.now().toString());

        // Dùng hàm save của Repository (nó sẽ log debug bên trong)
        userRepository.save(user);

        logger.info("Profile updated successfully for user: {}", userId);
        return user;
    }

    // Thêm hàm này vào UserService.java

    public Map<String, Boolean> checkOnboardingStatus(String userId) {
        // 1. Validator input
        if (userId == null || userId.trim().isEmpty()) {
            logger.error("CheckOnboardingStatus failed: UserId is null/empty");
            throw new IllegalArgumentException("User ID required");
        }

        // 2. Tạo Key chuẩn
        String pk = "USER#" + userId;
        String sk = "METADATA";

        logger.info("Checking onboarding status for User: {}", userId);

        // 3. Gọi Repo lấy User
        UserEntity user = userRepository.findById(pk, sk);

        // 4. Logic kiểm tra (Giống hệt logic if(!user) throw Error trong JS của bạn)
        if (user == null) {
            logger.warn("User not found in DB: {}", userId);
            throw new RuntimeException("User not found");
        }

        // 5. Logic xác định isOnboarded (Có industry = đã onboard)
        boolean isOnboarded = user.getIndustry() != null && !user.getIndustry().trim().isEmpty();

        logger.info("User {} onboarding status: {}", userId, isOnboarded);

        return Map.of("isOnboarded", isOnboarded);
    }

    // --- Helper Methods ---

    private void validateUserId(String userId) {
        if (userId == null || userId.trim().isEmpty()) {
            logger.error("Validation failed: UserId is null or empty");
            throw new IllegalArgumentException("UserId cannot be null or empty");
        }
    }

    private boolean hasValue(String value) {
        return value != null && !value.trim().isEmpty();
    }
}
```
## 4. Function (Controller Layer)
Cuối cùng, chúng ta viết Lambda Handler để nhận HTTP Request từ API Gateway, trích xuất thông tin và gọi Service.

```Java

@Configuration
public class UserFunctions {

    private static final Logger logger = LoggerFactory.getLogger(UserFunctions.class);

    private final UserService userService;
    private final ObjectMapper objectMapper;

    public UserFunctions(UserService userService) {
        this.userService = userService;
        this.objectMapper = new ObjectMapper();
        this.objectMapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY);
        this.objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
    }

    //Hàm tổng (ROUTER FUNCTION) để điều phối
    @Bean
    public Function<Map<String, Object>, Map<String, Object>> profileHandler() {
        return event -> {
            try {
                // 1. Lấy thông tin cơ bản
                String httpMethod = extractHttpMethod(event);
                String path = extractPath(event); // <--- Hàm mới để lấy path

                logger.info("Routing Request -> Path: {}, Method: {}", path, httpMethod);

                Map<String, String> headers = normalizeHeaders(event);
                String userId = extractUserIdOrThrow(headers);
                String email = extractEmail(headers);

                // 2. ROUTING LOGIC (Bộ điều phối)
                // TH1: Check Onboarding Status
                if (path.endsWith("/onboarding") && "GET".equalsIgnoreCase(httpMethod)) {
                    return handleGetOnboardingStatus(userId);
                }

                // TH2: Get Profile
                if (path.endsWith("/profile") && "GET".equalsIgnoreCase(httpMethod)) {
                    return handleGetProfile(userId);
                }

                // TH3: Update Profile
                if (path.endsWith("/profile") && "POST".equalsIgnoreCase(httpMethod)) {
                    return handleUpdateProfile(userId, email, event);
                }

                return buildResponse(404, Map.of("error", "Route not found: " + path));

            } catch (SecurityException e) {
                return buildResponse(401, Map.of("error", e.getMessage()));
            } catch (IllegalArgumentException e) { // Bắt lỗi user not found từ service
                return buildResponse(400, Map.of("error", e.getMessage()));
            } catch (Exception e) {
                logger.error("System Error", e);
                return buildResponse(500, Map.of("error", e.getMessage()));
            }
        };
    }
    // Các hàm nghiệp vụ

    private Map<String, Object> handleGetProfile(String userId) {
        logger.info("Executing GET logic for User: {}", userId);
        UserEntity user = userService.getUserProfile(userId);

        if (user == null) {
            return buildResponse(404, Map.of("error", "Profile not found"));
        }
        return buildResponse(200, user);
    }

    private Map<String, Object> handleUpdateProfile(String userId, String email, Map<String, Object> event) throws Exception {
        logger.info("Executing POST logic for User: {}", userId);

        String bodyString = extractBodyContent(event);
        if (bodyString == null || bodyString.trim().isEmpty()) {
            return buildResponse(400, Map.of("error", "Request body is empty"));
        }

        logger.debug("Request Body: {}", bodyString);

        UpdateUserRequest request = objectMapper.readValue(bodyString, UpdateUserRequest.class);
        UserEntity updatedUser = userService.updateUserProfile(userId, email, request);

        return buildResponse(200, updatedUser);
    }

    private Map<String, Object> handleGetOnboardingStatus(String userId) {
        try {
            Map<String, Boolean> result = userService.checkOnboardingStatus(userId);
            return buildResponse(200, result);
        } catch (RuntimeException e) {
            // Nếu lỗi là "User not found", trả về 404 cho đúng chuẩn REST
            if ("User not found".equals(e.getMessage())) {
                return buildResponse(404, Map.of("error", "User not found"));
            }
            throw e; // Ném tiếp để handler chung xử lý 500
        }
    }

    // --- CÁC HÀM TIỆN ÍCH (HELPERS) ---

    private String extractHttpMethod(Map<String, Object> event) {
        // Trong API Gateway V2, method nằm ở: requestContext -> http -> method
        try {
            Map<String, Object> requestContext = (Map<String, Object>) event.get("requestContext");
            if (requestContext != null) {
                Map<String, Object> http = (Map<String, Object>) requestContext.get("http");
                if (http != null && http.get("method") != null) {
                    return http.get("method").toString();
                }
            }
            // Fallback cho một số trường hợp test local hoặc format khác
            if (event.get("httpMethod") != null) {
                return event.get("httpMethod").toString();
            }
        } catch (Exception e) {
            logger.warn("Could not extract HTTP method from event", e);
        }
        return "UNKNOWN";
    }

    private String extractPath(Map<String, Object> event) {
        if (event.get("rawPath") != null) {
            return event.get("rawPath").toString();
        }
        // Fallback nếu event cấu trúc khác
        return "/unknown";
    }

    // =========================================================================
    // HELPER METHODS (PRIVATE)
    // =========================================================================

    /**
     * Chuẩn hóa Header về dạng Map<String, String> và lowercase key để dễ tìm
     */
    private Map<String, String> normalizeHeaders(Map<String, Object> event) {
        Map<String, String> headers = new HashMap<>();
        Object headersObj = event.get("headers");

        logger.info("DEBUG RAW HEADERS TYPE: {}", (headersObj != null ? headersObj.getClass().getName() : "null"));
        logger.info("DEBUG RAW HEADERS CONTENT: {}", headersObj);

        if (headersObj instanceof Map) {
            Map<?, ?> rawMap = (Map<?, ?>) headersObj;
            for (Map.Entry<?, ?> entry : rawMap.entrySet()) {
                if (entry.getKey() != null && entry.getValue() != null) {
                    headers.put(entry.getKey().toString().toLowerCase(), entry.getValue().toString());
                }
            }
        }

        logger.info("DEBUG NORMALIZED HEADERS: {}", headers);

        return headers;
    }

    /**
     * Trích xuất và giải mã Body từ Event (xử lý cả Base64)
     */
    private String extractBodyContent(Map<String, Object> event) {
        Object bodyObj = event.get("body");
        if (bodyObj == null) return null;

        String bodyString;
        if (bodyObj instanceof String) {
            bodyString = (String) bodyObj;
        } else {
            // Trường hợp Spring đã lỡ parse thành Map
            try {
                bodyString = objectMapper.writeValueAsString(bodyObj);
            } catch (Exception e) {
                logger.warn("Failed to convert body object to string", e);
                return "{}";
            }
        }

        // Xử lý Base64 nếu AWS API Gateway bật tính năng này
        Object isBase64Obj = event.get("isBase64Encoded");
        if (isBase64Obj != null && Boolean.parseBoolean(isBase64Obj.toString())) {
            try {
                bodyString = new String(Base64.getDecoder().decode(bodyString));
            } catch (Exception e) {
                logger.error("Failed to decode Base64 body", e);
                throw new RuntimeException("Invalid Base64 body");
            }
        }
        return bodyString;
    }

    /**
     * Tạo Response chuẩn cho API Gateway
     */
    private Map<String, Object> buildResponse(int statusCode, Object body) {
        Map<String, Object> response = new HashMap<>();
        response.put("statusCode", statusCode);

        Map<String, String> headers = new HashMap<>();
        headers.put("Content-Type", "application/json");
        // CORS Headers (Nếu cần thiết, dù API Gateway thường đã xử lý)
        headers.put("Access-Control-Allow-Origin", "*");
        response.put("headers", headers);

        try {
            String jsonBody = objectMapper.writeValueAsString(body);
            response.put("body", jsonBody);
        } catch (Exception e) {
            response.put("statusCode", 500);
            response.put("body", "{\"error\": \"JSON Serialization Error\"}");
        }
        return response;
    }

    // --- SECURITY HELPERS ---

    private String extractUserIdOrThrow(Map<String, String> headers) {
        String sub = extractClaim(headers, "sub");
        if (sub == null) {
            throw new SecurityException("Missing 'sub' claim in token or Invalid Token");
        }
        return sub;
    }

    private String extractEmail(Map<String, String> headers) {
        return extractClaim(headers, "email");
    }

    private String extractClaim(Map<String, String> headers, String claimKey) {
        // Tìm header Authorization (đã được normalize về chữ thường)
        String authHeader = headers.get("authorization");

        logger.info("DEBUG AUTH HEADER FOUND: {}", authHeader);

        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            logger.warn("Authorization header missing or invalid format");
            // Thay vì trả về test ID, ta trả về null để hàm gọi ném lỗi 401
            return null;
        }

        try {
            String token = authHeader.substring(7);
            String[] parts = token.split("\\.");
            if (parts.length < 2) return null;

            String payloadJson = new String(Base64.getUrlDecoder().decode(parts[1]));
            JsonNode node = objectMapper.readTree(payloadJson);

            if (node.has(claimKey)) {
                return node.get(claimKey).asText();
            }
        } catch (Exception e) {
            logger.error("Token decoding failed: {}", e.getMessage());
        }
        return null;
    }
}
```