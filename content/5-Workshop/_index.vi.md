---
title: "Workshop"
#date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Xây dựng **AI Career Coach** - một ứng dụng Fullstack Serverless hiện đại giúp người dùng nâng cao cơ hội nghề nghiệp thông qua sức mạnh của trí tuệ nhân tạo

---

## 🎥 Demo Sản Phẩm

[Xem video](https://www.youtube.com/watch?v=I2NEzLo2n7s)

---

## 💡 Vấn đề & Giải pháp

### Vấn đề
Thị trường việc làm ngày càng cạnh tranh. Ứng viên thường gặp khó khăn trong việc:
* Viết một bản CV (Resume) chuẩn chỉnh, tối ưu cho hệ thống ATS.
* Viết thư xin việc (Cover Letter) cá nhân hóa cho từng công ty.
* Thiếu môi trường để luyện tập phỏng vấn và nhận phản hồi khách quan.

### Giải pháp: AI Career Coach
Dự án này giải quyết các vấn đề trên bằng cách sử dụng **Amazon Bedrock (Claude 3)** để đóng vai trò như một người huấn luyện viên ảo:
1.  **AI Resume Builder:** Hỗ trợ viết và chỉnh sửa nội dung CV chuyên nghiệp.
2.  **Cover Letter Generator:** Tự động tạo thư xin việc dựa trên mô tả công việc (JD) và hồ sơ người dùng.
3.  **Mock Interview:** Phỏng vấn thử với AI dựa trên từng ngành nghề cụ thể và chấm điểm câu trả lời.

---

## 🛠️ Tech Stack (Công nghệ sử dụng)

Dự án này áp dụng mô hình **Serverless Microservices**, giúp tối ưu chi phí và khả năng mở rộng.

### 1. Frontend
* **React (Vite):** Tốc độ phát triển nhanh, hiệu năng cao.
* **Tailwind CSS & Shadcn UI:** Xây dựng giao diện đẹp, hiện đại và chuẩn accessibility.
* **AWS Amplify:** Quản lý Authentication (Cognito) và kết nối API.

### 2. Backend (Java Serverless)
* **Java 17 & Spring Boot:** Nền tảng backend mạnh mẽ.
* **Spring Cloud Function:** Chuyển đổi logic Java thành các hàm Serverless.
* **AWS Lambda:** Chạy code mà không cần quản lý server. Gồm 5 functions riêng biệt:
    * `UserProfileFunction`
    * `ResumeFunction`
    * `CoverLetterFunction`
    * `InterviewFunction`
    * `IndustryInsightFunction`

### 3. Database & AI
* **Amazon DynamoDB:** Cơ sở dữ liệu NoSQL với thiết kế **Single Table Design** hiệu năng cao.
* **Amazon Bedrock:** Cổng kết nối tới các mô hình AI nền tảng (Foundation Models). Chúng ta sử dụng model **Claude 3 Haiku** của Anthropic.

### 4. Infrastructure & DevOps
* **AWS SAM (Serverless Application Model):** Định nghĩa toàn bộ hạ tầng bằng code (IaC).
* **Amazon S3 & CloudFront:** Lưu trữ và phân phối Frontend toàn cầu với bảo mật OAC.

---

## 🎯 Mục tiêu Workshop

Sau khi hoàn thành workshop này, bạn sẽ nắm được:
1.  Cách xây dựng một ứng dụng **Fullstack** hoàn chỉnh.
2.  Tư duy thiết kế hệ thống **Serverless** trên AWS.
3.  Kỹ thuật **Prompt Engineering** để tích hợp AI vào ứng dụng Java.
4.  Cách bảo mật ứng dụng với **JWT** và **Cognito**.
5.  Quy trình **CI/CD** cơ bản: Build, Deploy và Hosting.

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Hạ tầng với SAM](5.3-Infrastructure-as-code/)
4. [Phát triển Backend](5.4-Backend-development/)
5. [Phát triển Frontend](5.5-Frontend-development/)
6. [Deploy](5.6-Deploy/)
7. [Dọn dẹp tài nguyên](5.7-Clean/)