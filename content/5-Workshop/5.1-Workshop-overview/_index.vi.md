---
title : "Giới thiệu"
#date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

## 1. Ứng dụng "AI Career Coach" là gì?

**AI Career Coach** là một nền tảng hỗ trợ người tìm việc làm, tận dụng sức mạnh của **Generative AI** (AI tạo sinh) để laà nhanh các tác vụ tốn thời gian nhất trong quy trình ứng tuyển.

Dưới đây là 4 module tính năng chính mà chúng ta sẽ xây dựng:

### A. Dashboard (Bảng điều khiển)
Nơi người dùng có cái nhìn tổng quan về ngành nghề của mình

![dashboard](/images/5-Workshop/5.1-Workshop-overview/dashboard.png)

### B. AI Resume Builder (Trình tạo CV thông minh)
Trình soạn thảo Markdown tích hợp AI. Người dùng nhập thông tin thô, AI sẽ giúp viết lại (rephrase) các câu mô tả kinh nghiệm làm việc sao cho chuyên nghiệp và chuẩn ATS (Applicant Tracking System).

![resume builder form](/images/5-Workshop/5.1-Workshop-overview/resume-builder-form.jpeg)
![resume builder markdown](/images/5-Workshop/5.1-Workshop-overview/resume-builder-markdown.jpeg)

### C. Cover Letter Generator (Tạo thư xin việc)
Người dùng chỉ cần cung cấp: "Vị trí ứng tuyển", "Tên công ty" và "Mô tả công việc (JD)". Hệ thống sẽ gọi Claude 3 để viết một lá thư xin việc thuyết phục, cá nhân hóa theo JD đó.

![cover letter page](/images/5-Workshop/5.1-Workshop-overview/cover-letter-page.jpeg)
![cover letter form](/images/5-Workshop/5.1-Workshop-overview/cover-letter-form.jpeg)

### D. Mock Interview (Phỏng vấn thử)
Tính năng thú vị nhất. Hệ thống sẽ sinh ra các câu hỏi trắc nghiệm dựa trên ngành nghề của người dùng. Sau khi người dùng trả lời, hệ thống sẽ chấm điểm và đưa ra lời khuyên cải thiện.

![interview](/images/5-Workshop/5.1-Workshop-overview/interview-page.jpeg)

---

## 2. Kiến trúc Hệ thống (System Architecture)

Đây là phần "xương sống" của workshop. Chúng ta sẽ áp dụng kiến trúc **Serverless** hoàn toàn trên AWS.

Hãy nhìn vào sơ đồ luồng dữ liệu dưới đây:

![AI Career Coach Architecture](/images/2-Proposal/AI_Career_Coach_Architecture_v1.png)

### Phân tích luồng đi (Data Flow):

1.  **Frontend Hosting:** Mã nguồn React (đã build) được lưu trữ trên **Amazon S3** và phân phối toàn cầu qua **Amazon CloudFront** để đảm bảo tốc độ tải trang nhanh nhất.
2.  **Authentication:** Người dùng đăng nhập thông qua **Amazon Cognito**. Frontend sẽ nhận được JWT Token để xác thực các request gửi lên Server.
3.  **API Routing:** **Amazon API Gateway** đóng vai trò là cửa ngõ, nhận HTTP Request từ Frontend, kiểm tra Token (Authorizer) và điều hướng đến đúng Lambda Function xử lý.
4.  **Backend Logic (Compute):** Chúng ta sử dụng 5 hàm **AWS Lambda** viết bằng **Java 17 (Spring Cloud Function)**. Mỗi hàm đảm nhiệm một nghiệp vụ riêng biệt (Microservices pattern).
5.  **Database:** Dữ liệu được lưu trữ trong **Amazon DynamoDB** với thiết kế **Single Table Design** (dùng chung 1 bảng cho User, Resume, Interview,...), tối ưu cho hiệu năng đọc/ghi cực nhanh.
6.  **AI Integration:** Các Lambda Function cần tính năng thông minh sẽ gọi sang **Amazon Bedrock** để tương tác với mô hình **Claude 3 Haiku**.

---

## 3. Tại sao chọn Tech Stack này?

Tại sao chúng ta lại kết hợp Java Spring Boot với Serverless?

* **Java & Spring Cloud Function:** Java là ngôn ngữ doanh nghiệp phổ biến. Spring Cloud Function giúp các lập trình viên Java dễ dàng chuyển đổi code Spring Boot quen thuộc sang môi trường Serverless mà không cần học lại từ đầu.
* **AWS Lambda & SnapStart:** Trước đây Java bị chê là khởi động chậm (Cold Start). Với tính năng **Lambda SnapStart**, code Java của chúng ta sẽ khởi động cực nhanh, gần như tức thì.
* **DynamoDB Single Table:** Đây là kỹ thuật thiết kế DB nâng cao, giúp giảm chi phí và tăng tốc độ truy vấn bằng cách giảm thiểu việc join dữ liệu (vốn không có trong NoSQL).
* **Bedrock:** Thay vì phải quản lý server GPU đắt đỏ để chạy AI, Bedrock cung cấp API để gọi các model hàng đầu (như Claude, Llama) theo dạng Serverless (dùng bao nhiêu trả bấy nhiêu).

---

## 4. Cấu trúc Source Code

Dự án này được tổ chức theo mô hình **Monorepo** (chứa cả Frontend và Backend trong cùng một kho chứa) để tiện quản lý cho workshop.

![source code](/images/5-Workshop/5.1-Workshop-overview/source-code.png)

* `backend/`: Chứa mã nguồn Java Spring Boot.
* `frontend/`: Chứa mã nguồn React Vite.
* `template.yaml`: File định nghĩa hạ tầng (Infrastructure as Code) của AWS SAM.


