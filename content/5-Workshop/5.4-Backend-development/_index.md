---
title : "Backend Development"
#date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

In this section, we will write the source code for the Backend using **Java 17** and the **Spring Cloud Function** framework.

The goal is to transform business logic into independent functions that can run on AWS Lambda.

View the backend source code [here](https://github.com/duykhanh07/ai-career-coach/tree/main/backend)

## 1. Project Structure

The project is built based on Maven. Below is the standard directory structure:

![backend](/images/5-Workshop/5.4-Backend-development/backend.png)

### Main Packages:
* `model`: Defines Entities mapped to DynamoDB (e.g., `UserEntity`, `ResumeEntity`).
* `repository`: Interface for communicating with DynamoDB (using AWS SDK v2 Enhanced Client).
* `service`: Contains business logic and code to call AI Bedrock.
* `functions`: Contains Spring Cloud Function Beans (Lambda entry points).
* `dto`: Data Transfer Objects to receive requests from the Frontend.

---

## 2. Important Lesson: ObjectMapper Configuration

During development, we encountered a serious error: **The Backend returns an empty object `{}` or a `500` error even though the logic runs correctly.**

### Cause
By default, Jackson's `ObjectMapper` requires classes to have standard Getters/Setters (Java Bean standard) or fields must be Public. If our Entity uses Private fields and is missing Getters, or has a complex structure, Jackson will not be able to convert (serialize) it to JSON.

### Solution: Manual Configuration
Instead of using Spring's default ObjectMapper, we manually initialize and configure it more "aggressively" in the Constructor of the Function classes:

```java
public ResumeFunctions(ResumeService resumeService) {
    this.resumeService = resumeService;
    
    // --- CẤU HÌNH JACKSON THỦ CÔNG ---
    this.objectMapper = new ObjectMapper();
    
    // 1. Cho phép đọc/ghi trực tiếp vào field (kể cả private) mà không cần Getter/Setter
    this.objectMapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY);
    
    // 2. Bỏ qua lỗi nếu JSON đầu vào có trường thừa mà Java class không có
    this.objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
}
```
### 3. AbstractDynamoRepository<T>

* Generic Repository base class for DynamoDB.
* Provides standard CRUD operations, logging, and validation.
* @param <T> Entity Type (Example: UserEntity)

```java

public abstract class AbstractDynamoRepository<T> {

    // Logger chuẩn cho môi trường Production
    protected final Logger logger = LoggerFactory.getLogger(getClass());

    protected final DynamoDbTable<T> table;
    protected final String tableName;

    public AbstractDynamoRepository(DynamoDbEnhancedClient client, Class<T> type) {
        this.tableName = System.getenv("TABLE_NAME");

        // 1. Validate Config ngay khi khởi động
        if (this.tableName == null || this.tableName.isEmpty()) {
            logger.error("CRITICAL: Biến môi trường TABLE_NAME chưa được cấu hình!");
            throw new RuntimeException("Missing TABLE_NAME environment variable");
        }

        this.table = client.table(tableName, TableSchema.fromBean(type));
        logger.info("Initialized Repository for entity {} with table {}", type.getSimpleName(), tableName);
    }

    // ==================================================================================
    // 1. CREATE / UPDATE (Upsert)
    // ==================================================================================

    /**
     * Lưu hoặc cập nhật một Item.
     * Trong DynamoDB, putItem sẽ ghi đè toàn bộ item nếu PK/SK trùng.
     */
    public void save(T item) {
        if (item == null) {
            throw new IllegalArgumentException("Entity to save cannot be null");
        }

        try {
            logger.debug("Saving item to table {}: {}", tableName, item);
            table.putItem(item);
            logger.info("Successfully saved item.");
        } catch (DynamoDbException e) {
            logger.error("Failed to save item to DynamoDB: {}", e.getMessage(), e);
            throw new RuntimeException("Database Error: Could not save item", e);
        }
    }

    /**
     * Cập nhật item (Chỉ cập nhật các trường có giá trị, giữ nguyên các trường khác).
     * Yêu cầu Entity phải có đủ PK và SK.
     */
    public T update(T item) {
        if (item == null) {
            throw new IllegalArgumentException("Entity to update cannot be null");
        }
        try {
            logger.debug("Updating item in table {}: {}", tableName, item);
            // updateItem sẽ trả về item đã được update
            T updatedItem = table.updateItem(item);
            logger.info("Successfully updated item.");
            return updatedItem;
        } catch (DynamoDbException e) {
            logger.error("Failed to update item: {}", e.getMessage(), e);
            throw new RuntimeException("Database Error: Could not update item", e);
        }
    }

    // ==================================================================================
    // 2. READ (Find)
    // ==================================================================================

    public T findById(String pk, String sk) {
        // Validate input
        if (pk == null || pk.isEmpty() || sk == null || sk.isEmpty()) {
            logger.warn("FindById called with empty keys. PK: {}, SK: {}", pk, sk);
            return null; // Hoặc throw Exception tùy logic nghiệp vụ
        }

        try {
            Key key = Key.builder().partitionValue(pk).sortValue(sk).build();
            logger.debug("Fetching item with PK: {}, SK: {}", pk, sk);

            T item = table.getItem(key);

            if (item == null) {
                logger.info("Item not found for PK: {}, SK: {}", pk, sk);
            } else {
                logger.debug("Item found: {}", item);
            }
            return item;
        } catch (DynamoDbException e) {
            logger.error("Failed to find item: {}", e.getMessage(), e);
            throw new RuntimeException("Database Error: Could not fetch item", e);
        }
    }

    /**
     * Tìm tất cả các Item có cùng Partition Key (PK).
     * Đây là thao tác QUERY (Hiệu năng cao, rẻ tiền).
     * Ví dụ: Lấy tất cả Resume, Letter, Assessment của User X (PK=USER#X)
     */
    public List<T> findAllByPartitionKey(String pk) {
        if (pk == null || pk.isEmpty()) return new ArrayList<>();

        try {
            QueryConditional queryConditional = QueryConditional.keyEqualTo(
                    Key.builder().partitionValue(pk).build()
            );

            logger.debug("Querying items with PK: {}", pk);

            // SDK trả về Iterable (Lazy load), ta convert sang List để dễ dùng
            PageIterable<T> result = table.query(queryConditional);
            List<T> items = result.items().stream().collect(Collectors.toList());

            logger.info("Found {} items for PK: {}", items.size(), pk);
            return items;
        } catch (DynamoDbException e) {
            logger.error("Failed to query items by PK: {}", e.getMessage(), e);
            throw new RuntimeException("Database Error: Could not query items", e);
        }
    }

    /**
     * ⚠️ CẢNH BÁO: Quét toàn bộ bảng (SCAN).
     * Rất tốn kém Read Capacity Unit (RCU) và chậm nếu bảng lớn.
     * Chỉ dùng cho các bảng nhỏ (như Industry) hoặc debug.
     */
    public List<T> findAll() {
        try {
            logger.warn("PERFORMANCE WARNING: Executing full table SCAN on {}", tableName);
            return table.scan().items().stream().collect(Collectors.toList());
        } catch (DynamoDbException e) {
            logger.error("Failed to scan table: {}", e.getMessage(), e);
            throw new RuntimeException("Database Error: Could not scan table", e);
        }
    }

    // ==================================================================================
    // 3. DELETE
    // ==================================================================================

    public T delete(String pk, String sk) {
        if (pk == null || sk == null) {
            throw new IllegalArgumentException("Keys cannot be null for deletion");
        }

        try {
            Key key = Key.builder().partitionValue(pk).sortValue(sk).build();
            logger.info("Deleting item with PK: {}, SK: {}", pk, sk);

            // deleteItem trả về item cũ trước khi xóa (nếu có)
            return table.deleteItem(key);
        } catch (DynamoDbException e) {
            logger.error("Failed to delete item: {}", e.getMessage(), e);
            throw new RuntimeException("Database Error: Could not delete item", e);
        }
    }
}

```

---

### 4. BedrockService

This service class specializes in handling AI-related tasks.

```java

@Service
public class BedrockService {

    private static final Logger logger = LoggerFactory.getLogger(BedrockService.class);

    private final BedrockRuntimeClient bedrockClient;
    private final ObjectMapper objectMapper;

    // Model ID lấy từ biến môi trường (Config trong template.yaml)
    private final String modelId = System.getenv("BEDROCK_MODEL_ID");

    public BedrockService(ObjectMapper objectMapper) {
        this.bedrockClient = BedrockRuntimeClient.builder().build(); // Tự lấy region từ môi trường
        this.objectMapper = objectMapper;
    }

    public IndustryInsightEntity generateIndustryInsights(String industry) {
        logger.info("Calling AWS Bedrock to analyze industry: {}", industry);

        String prompt = """
             Analyze the current state of the %s industry and provide insights in ONLY the following JSON format without any additional notes or explanations:
             {
               "salaryRanges": [
                 { "role": "string", "min": number, "max": number, "median": number, "location": "string" }
               ],
               "growthRate": number,
               "demandLevel": "High" | "Medium" | "Low",
               "topSkills": ["skill1", "skill2"],
               "marketOutlook": "Positive" | "Neutral" | "Negative",
               "keyTrends": ["trend1", "trend2"],
               "recommendedSkills": ["skill1", "skill2"]
             }
             
             IMPORTANT: Return ONLY the JSON. No additional text, notes, or markdown formatting.
             Include at least 5 common roles for salary ranges.
             Growth rate should be a percentage float (e.g., 5.5).
             Include at least 5 skills and trends.
             """.formatted(industry);

        // Cấu trúc Body cho Claude 3
        Map<String, Object> payload = Map.of(
                "anthropic_version", "bedrock-2023-05-31",
                "max_tokens", 2000,
                "messages", java.util.List.of(
                        Map.of("role", "user", "content", prompt)
                )
        );

        try {
            String payloadJson = objectMapper.writeValueAsString(payload);

            InvokeModelRequest request = InvokeModelRequest.builder()
                    .modelId(modelId)
                    .body(SdkBytes.fromUtf8String(payloadJson))
                    .contentType("application/json")
                    .accept("application/json")
                    .build();

            InvokeModelResponse response = bedrockClient.invokeModel(request);
            String responseBody = response.body().asString(StandardCharsets.UTF_8);

            // Parse response của Claude để lấy phần text
            // Structure: { "content": [ { "text": "..." } ] }
            var jsonNode = objectMapper.readTree(responseBody);
            String aiText = jsonNode.get("content").get(0).get("text").asText();

            // Clean text (remove markdown ```json ... ```)
            String cleanedJson = aiText.replaceAll("```json", "").replaceAll("```", "").trim();
            logger.debug("Cleaned AI JSON: {}", cleanedJson);

            // Parse thành Entity
            return objectMapper.readValue(cleanedJson, IndustryInsightEntity.class);

        } catch (Exception e) {
            logger.error("Failed to generate AI insights", e);
            throw new RuntimeException("AI Generation Failed: " + e.getMessage());
        }
    }

    /**
     * Hàm gọi AI trả về Text thuần (dùng cho Resume improvement)
     */
    public String generateTextCorrection(String prompt) {
        logger.info("Calling Bedrock for Text Generation...");

        // Cấu trúc Body cho Claude 3
        Map<String, Object> payload = Map.of(
                "anthropic_version", "bedrock-2023-05-31",
                "max_tokens", 1000,
                "messages", java.util.List.of(
                        Map.of("role", "user", "content", prompt)
                )
        );

        try {
            String payloadJson = objectMapper.writeValueAsString(payload);

            InvokeModelRequest request = InvokeModelRequest.builder()
                    .modelId(modelId)
                    .body(SdkBytes.fromUtf8String(payloadJson))
                    .contentType("application/json")
                    .accept("application/json")
                    .build();

            InvokeModelResponse response = bedrockClient.invokeModel(request);
            String responseBody = response.body().asString(StandardCharsets.UTF_8);

            // Parse response: { "content": [ { "text": "..." } ] }
            var jsonNode = objectMapper.readTree(responseBody);
            String aiText = jsonNode.get("content").get(0).get("text").asText();

            return aiText.trim();

        } catch (Exception e) {
            logger.error("Bedrock Text Generation Failed", e);
            throw new RuntimeException("AI Service Unavailable");
        }
    }


    // Hàm mới: Tạo Quiz Questions
    public String generateQuizJson(String industry, String skills) {
        logger.info("Generating Quiz for {} with skills {}", industry, skills);

        String prompt = String.format("""
            Generate 10 technical interview questions for a %s professional with expertise in %s.
            Each question must be multiple choice with 4 options.
            
            Return the response in this JSON format only, no additional text, no markdown:
            {
              "questions": [
                {
                  "question": "string",
                  "options": ["string", "string", "string", "string"],
                  "correctAnswer": "string",
                  "explanation": "string"
                }
              ]
            }
            """, industry, skills);

        return callBedrock(prompt);
    }

    // Hàm tái sử dụng logic gọi Bedrock
    private String callBedrock(String prompt) {
        // Logic giống hệt hàm generateTextCorrection cũ, chỉ thay prompt
        // (Bạn có thể refactor code cũ để dùng chung hàm này cho gọn)
        Map<String, Object> payload = Map.of(
                "anthropic_version", "bedrock-2023-05-31",
                "max_tokens", 4000, // Tăng token vì JSON quiz khá dài
                "messages", java.util.List.of(Map.of("role", "user", "content", prompt))
        );
        try {
            String payloadJson = objectMapper.writeValueAsString(payload);
            InvokeModelRequest request = InvokeModelRequest.builder()
                    .modelId(modelId)
                    .body(SdkBytes.fromUtf8String(payloadJson))
                    .contentType("application/json").accept("application/json").build();
            InvokeModelResponse response = bedrockClient.invokeModel(request);
            String responseBody = response.body().asString(StandardCharsets.UTF_8);
            var jsonNode = objectMapper.readTree(responseBody);
            return jsonNode.get("content").get(0).get("text").asText().trim();
        } catch (Exception e) {
            logger.error("Bedrock Error", e);
            throw new RuntimeException("AI Error");
        }
    }

}

```

---

## Content of 5 Lambda Functions

- [UserFunctions](5.4.1-User-functions/)
- [IndustryFunctions](5.4.2-Industry-functions/)
- [ResumeFuunctions](5.4.3-Resume-functions/)
- [CoverLetterFunctions](5.4.4-Cover-letter-functions/)
- [AssessmentFunctions](5.4.5-Assessment-functions/)
