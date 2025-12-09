---
title: "Worklog Tuần 3"
#date: "`r Sys.Date()`"
weight: 13
chapter: false
pre: " <b> 1.13 </b> "
---
### Mục tiêu tuần 13:
* Xây dựng Frontend React hoàn chỉnh với Vite, Tailwind CSS và Shadcn UI.
* Tích hợp AWS Amplify để xử lý Đăng nhập/Đăng ký.
* Kết nối Frontend với Backend API Gateway (Integration).
* Hoàn thiện các tính năng cốt lõi: Resume Builder, Quiz Game, Dashboard.
* Build, Deploy Frontend lên S3/CloudFront và kiểm thử toàn hệ thống (UAT).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | --- | :---: | :---: | --- |
| 2 | - Cài đặt Tailwind CSS, Shadcn UI components (Button, Card, Input...).<br>- Cấu hình AWS Amplify (Amplify.configure) với UserPoolId từ Backend.<br>- Tạo trang Login/Register sử dụng component Authenticator.<br>- Tạo Layout chính (Sidebar, Header, Protected Routes). | 01/12/2025 | 01/12/2025 | Tài liệu AWS Amplify UI |
| 3 | - Code trang Dashboard: Fetch data lịch sử và hiển thị thống kê.<br>- Code trang Resume Builder: Tích hợp Markdown Editor (@uiw/react-md-editor).<br>- Xây dựng Form nhập liệu CV với React Hook Form & Zod Validation.<br>- Tích hợp tính năng xuất PDF (html2pdf.js). | 02/12/2025 | 02/12/2025 | Tài liệu React Hook Form |
| 4 | - Viết `apiClient` wrapper: Tự động gắn Token và xử lý "Double JSON parsing".<br>- Kết nối nút "Improve with AI" trong Resume Builder với API Backend.<br>- Xây dựng giao diện Cover Letter Generator: Form nhập JD -> Hiển thị kết quả AI.<br>- Xử lý loading state (Skeleton/Spinner) khi chờ AI trả lời. | 03/12/2025 | 03/12/2025 | |
| 5 | - Code giao diện làm bài trắc nghiệm (Radio buttons).<br>- Xử lý logic tính điểm và hiển thị kết quả đúng/sai (Logic so sánh chuỗi).<br>- Vẽ biểu đồ tiến độ (Performance Chart) dùng Recharts.<br>- Review giao diện tổng thể, fix lỗi CSS, Responsive Mobile. | 04/12/2025 | 04/12/2025 |  |
| 6 | - Chạy `npm run build` để đóng gói Frontend.<br>- Đồng bộ code lên S3 (`aws s3 sync`).<br>- Invalidate Cache CloudFront để cập nhật code mới nhất.<br>- Kiểm thử luồng đi trọn vẹn (End-to-End Testing) trên môi trường Production. | 05/12/2025 | 05/12/2025 |  |

### Kết quả đạt được tuần 13:
* Website AI Career Coach hoàn chỉnh, hoạt động mượt mà trên môi trường Internet.
* Người dùng có thể đăng nhập, tạo CV, viết Cover Letter và luyện phỏng vấn.
* Frontend kết nối thành công với Backend Serverless và AI Bedrock.
* Hệ thống được triển khai tự động hóa một phần qua CLI.
* Sẵn sàng cho việc demo.