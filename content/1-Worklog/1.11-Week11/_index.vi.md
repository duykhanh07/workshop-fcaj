---
title: "Worklog Tuần 11"
#date: "`r Sys.Date()`"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
### Mục tiêu tuần 11:
* Hoàn thiện thiết kế kiến trúc hệ thống Serverless trên AWS (S3, CloudFront, API Gateway, Lambda, DynamoDB).
* Thiết kế cơ sở dữ liệu DynamoDB theo mô hình Single Table Design (Access Patterns, PK/SK Design).
* Thiết kế giao diện (UI/UX) cho các trang chức năng chính: Dashboard, Resume Builder, Mock Interview.
* Khởi tạo cấu trúc dự án Monorepo (Backend Java Spring Cloud & Frontend React Vite).
* Thiết lập Infrastructure as Code (IaC) với AWS SAM (template.yaml) và cấu hình quyền IAM/Bedrock.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | --- | :---: | :---: | --- |
| 2 |- Vẽ sơ đồ luồng dữ liệu (Data Flow): User -> CloudFront -> API Gateway -> Lambda.<br>- Xác định các dịch vụ AWS cần thiết: Bedrock (Claude 3), Cognito, DynamoDB.<br>- Phân tích yêu cầu nghiệp vụ cho 4 module chính: Resume, Cover Letter, Interview, Industry Insight.<br>- Lên danh sách các API Endpoints cần thiết. | 17/11/2025 | 17/11/2025 |  |
| 3 | - Định nghĩa các Entity: User, Resume, Assessment, CoverLetter.<br>- Xác định Access Patterns (Cách query dữ liệu).<br>- Thiết kế Partition Key (PK) và Sort Key (SK) cho từng Entity (VD: USER#{id}, METADATA, RESUME, LETTER#uuid).<br>- Thiết kế Global Secondary Index (GSI) nếu cần. | 18/11/2025 | 18/11/2025 | |
| 4 | - Thiết kế luồng đăng nhập/đăng ký với Cognito.<br>- Phác thảo giao diện Dashboard thống kê.<br>- Thiết kế giao diện Resume Builder (chia đôi màn hình: Form & Preview).<br>- Thiết kế giao diện Mock Interview (Quiz form).<br>- Lựa chọn bộ thư viện UI: Tailwind CSS, Shadcn UI, Lucide Icons. | 19/11/2025 | 19/11/2025 |  |
| 5 | - Cài đặt môi trường: Java 17, Maven, Node.js, AWS CLI, SAM CLI.<br>- Khởi tạo cấu trúc Backend: Spring Boot 3 + Spring Cloud Function.<br>- Khởi tạo cấu trúc Frontend: React + Vite.<br>- Cấu hình Git Repository và cấu trúc thư mục Monorepo. | 20/11/2025 | 20/11/2025 | |
| 6 | - Tạo file template.yaml.<br>- Khai báo tài nguyên DynamoDB Table (PAY_PER_REQUEST).<br>- Khai báo Cognito User Pool & Client.<br>- Khai báo S3 Bucket & CloudFront Origin Access Control (OAC).<br>- Cấu hình IAM Policies cho Lambda (quyền gọi Bedrock & DynamoDB).<br>- Đăng ký Model Access (Claude 3 Haiku) trên AWS Bedrock. | 21/11/2025 | 21/11/2025 |  |

### Kết quả đạt được tuần 11:
* Bản thiết kế kiến trúc hệ thống Serverless hoàn chỉnh.
* Schema thiết kế DynamoDB Single Table sẵn sàng cho việc code.
* Môi trường phát triển (Dev Environment) đã được cài đặt đầy đủ.
* File template.yaml của AWS SAM đã sẵn sàng để deploy hạ tầng cơ bản.
* Quyền truy cập model AI Claude 3 trên AWS Bedrock đã được kích hoạt.

