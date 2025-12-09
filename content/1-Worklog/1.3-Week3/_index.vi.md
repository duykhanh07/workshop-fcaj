---
title: "Worklog Tuần 3"
#date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Hiểu và thực hành các cấu hình nâng cao của VPN và EC2 trên AWS.  
* Biết cách thiết lập VPN nâng cao với StrongSwan, Transit Gateway và BGP Dynamic Routing để mở rộng kết nối mạng giữa các vùng và hệ thống.  
* Nắm vững quản lý và vận hành EC2 nâng cao: thay đổi loại instance, tạo AMI tùy chỉnh, phục hồi truy cập khi mất Key Pair.  
* Triển khai ứng dụng thực tế (Node.js và User Management) trên Amazon Linux 2 và Windows Server 2022.  
* Quản lý quyền truy cập và giới hạn sử dụng tài nguyên bằng IAM Policy để tăng cường bảo mật hệ thống.  

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ---------- | ------------- | ---------------- | --------------- |
| 2 | - Tìm hiểu cơ bản về cấu hình VPN nâng cao: <br>&emsp; + Cấu hình StrongSwan thay thế <br>&emsp; + Cấu hình bảo mật nâng cao <br>&emsp; + Cấu hình BGP Dynamic Routing <br>&emsp; + Giám sát và Troubleshooting nâng cao <br>&emsp; + Checklist triển khai Production <br> - Tìm hiểu các vấn đề thường gặp trong quá trình thiết lập VPN và giải pháp cho từng hệ điều hành <br> - **Thực hành:** Cấu hình VPN bằng StrongSwan với Transit Gateway <br>&emsp; + Tạo Customer Gateway <br>&emsp; + Tạo Transit Gateway <br>&emsp; + Tạo kết nối VPN <br>&emsp; + Tạo Transit Gateway Attachment <br>&emsp; + Cấu hình Route Tables cho Transit Gateway và VPC <br>&emsp; + Cấu hình Customer Gateway | 22/09/2025 | 22/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Ôn tập kiến thức cơ bản về EC2: <br>&emsp; + Khái niệm, tính năng và cách hoạt động <br>&emsp; + Các loại instance (General Purpose, Compute Optimized, Memory Optimized, …) <br>&emsp; + AMI, EBS, Security Group, Key Pair <br>&emsp; + Cấu hình mạng, lưu trữ, quyền truy cập <br>&emsp; + Quy trình quản lý vòng đời (Start, Stop, Reboot, Terminate) <br> - **Thực hành:** <br>&emsp; + Tạo VPC, Security Group cho Linux và Windows <br>&emsp; + Khởi tạo Windows Server 2022 instance <br>&emsp; + Kết nối từ máy tính đến Windows Server 2022 <br>&emsp; + Tạo Amazon Linux 2 instance <br>&emsp; + Kết nối đến Amazon Linux 2 bằng MobaXterm và PuTTY | 23/09/2025 | 23/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 4 | - **Thực hành:** Làm việc nâng cao với Amazon EC2 và các thành phần liên quan <br>&emsp; + Thay đổi EC2 Instance Type <br>&emsp; + Tạo Snapshot <br>&emsp; + Tạo Custom AMI <br>&emsp; + Tạo instance từ Custom AMI <br>&emsp; + Phục hồi truy cập khi mất Key Pair (Linux - User Data, Windows - SSM) <br> - Tìm hiểu: <br>&emsp; + Giao diện Desktop cho EC2 Ubuntu 22.04 <br>&emsp; + EBS Snapshots Archive <br>&emsp; + Chia sẻ AMI | 24/09/2025 | 24/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 5 | - **Thực hành:** Triển khai ứng dụng Node.js trên Amazon Linux 2 <br>&emsp; + Chuẩn bị LAMP Web Server <br>&emsp; + Kiểm tra LAMP Web Server <br>&emsp; + Cấu hình bảo mật Database Server <br>&emsp; + Cài đặt phpMyAdmin <br>&emsp; + Triển khai ứng dụng trên Linux Instance <br> - **Thực hành:** Triển khai ứng dụng AWS User Management trên Amazon EC2 (Windows Server 2022) <br>&emsp; + Cài đặt XAMPP trên Windows Server 2022 Base <br>&emsp; + Triển khai ứng dụng trên Windows Server 2022 instance | 25/09/2025 | 26/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Tìm hiểu giới hạn sử dụng tài nguyên bằng dịch vụ IAM <br> - **Thực hành:** <br>&emsp; + Cho phép sử dụng EC2 tại Region Singapore <br>&emsp; + Giới hạn EC2 theo Instance Family, Instance Type <br>&emsp; + Giới hạn EBS Volume Storage Type <br>&emsp; + Giới hạn quyền xóa EC2 theo địa chỉ IP doanh nghiệp <br>&emsp; + Giới hạn quyền xóa EC2 theo thời gian | 26/09/2025 | 26/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Kết quả đạt được tuần 3:

* Hiểu sâu hơn về cấu hình và bảo mật VPN nâng cao: StrongSwan, Transit Gateway, BGP Dynamic Routing, giám sát và xử lý lỗi.  
* Thực hành thiết lập VPN kết nối nhiều mạng VPC với cấu trúc linh hoạt và độ tin cậy cao.  
* Củng cố kiến thức và kỹ năng triển khai EC2 trên cả Linux và Windows:  
  * Tạo, cấu hình và kết nối thành công đến EC2 instance.  
  * Quản lý tài nguyên (EBS, Security Group, AMI).  
  * Phục hồi truy cập khi mất Key Pair bằng User Data và SSM Session Manager.  
  * Quản lý AMI tùy chỉnh, sao lưu và khôi phục EC2 bằng Snapshot.  
* Triển khai ứng dụng thực tế (Node.js và User Management) trên Amazon Linux 2 và Windows Server 2022.  
* Áp dụng IAM Policy để giới hạn sử dụng tài nguyên, tăng cường bảo mật và kiểm soát chi phí hiệu quả.  
* Hoàn thiện kỹ năng vận hành, triển khai và quản trị hạ tầng điện toán đám mây AWS ở mức thực hành nâng cao.  
