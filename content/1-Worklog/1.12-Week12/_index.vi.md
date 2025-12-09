---
title: "Worklog Tuần 12"
#date: "`r Sys.Date()`"
weight: 12
chapter: false
pre: " <b> 1.12 </b> "
---
### Mục tiêu tuần 12:
* Xây dựng hoàn thiện 5 Lambda Functions sử dụng Java Spring Cloud Function.
* Triển khai tầng Repository tương tác với DynamoDB (Enhanced Client).
* Tích hợp AWS SDK v2 để gọi Amazon Bedrock (Claude 3 Haiku) cho các tính năng AI.
* Xử lý các vấn đề kỹ thuật chuyên sâu: Prompt Engineering, JSON Serialization (ObjectMapper), Error Handling.
* Deploy Backend lên AWS và kiểm thử API qua Postman.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | --- | :---: | :---: | --- |
| 2 | <br>- Tạo UserEntity & UserRepository.<br>- Viết logic CRUD thông tin người dùng.<br>- Tích hợp logic xác thực JWT từ Cognito trong Lambda.<br>- Xử lý logic Onboarding cho người dùng mới.<br>- Deploy thử nghiệm Lambda đầu tiên với SAM. | 24/11/2025 | 24/11/2025 | |
| 3 | - Tạo ResumeEntity & ResumeRepository.<br>- Viết logic lưu/lấy nội dung Markdown từ DynamoDB.<br>- Cấu hình ObjectMapper thủ công để xử lý lỗi JSON Serialization (tránh lỗi object rỗng).<br>- Viết Unit Test cho tầng Service. | 25/11/2025 | 25/11/2025 |  |
| 4 | - Cấu hình BedrockRuntimeClient trong Java.<br>- Viết Service improveWithAI: Gửi text thô -> Nhận text đã sửa.<br>- Thực hiện Prompt Engineering để tối ưu kết quả từ Claude 3.<br>- Xử lý Exception và Timeout khi gọi AI. | 26/11/2025 | 26/11/2025 |  |
| 5 | - **CoverLetter:** Viết Prompt tạo thư xin việc dựa trên JD và Profile.<br>- **Interview:** Viết logic sinh câu hỏi trắc nghiệm (Generate Quiz).<br>- **Interview:** Xử lý Prompt để bắt AI trả về đúng định dạng JSON Array.<br>- **Interview:** Viết logic lưu điểm số (Save Result) vào DynamoDB. | 27/11/2025 | 27/11/2025 |  |
| 6 | - Xây dựng IndustryInsightFunction (Optional - Cache strategy).<br>- Review và Refactor code (Clean Code).<br>- Build toàn bộ project (`mvn clean package`).<br>- Chạy `sam deploy` để đẩy toàn bộ Backend lên AWS.<br>- Test toàn bộ API Endpoints bằng Postman/Curl. | 28/11/2025 | 28/11/2025 |  |

### Kết quả đạt được tuần 12:
* 5 Lambda Functions hoạt động ổn định trên AWS.
* API Gateway đã public các endpoints cần thiết.
* Tích hợp thành công Amazon Bedrock để sinh nội dung CV, Cover Letter và Quiz.
* Dữ liệu được lưu trữ và truy xuất chính xác từ DynamoDB.
* Khắc phục được các lỗi liên quan đến JSON Serialization trong môi trường Serverless.