---
title: "Worklog Tuần 4"
#date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Hiểu và thực hành các dịch vụ lưu trữ và phân phối nội dung cốt lõi của AWS, bao gồm S3, CloudFront, Cloud9, RDS và Lightsail.  
* Biết cách triển khai, quản lý và bảo mật dữ liệu trong Amazon S3 (bao gồm Website Hosting, Versioning và Cross-Region Replication).  
* Nắm được quy trình triển khai ứng dụng Web tĩnh và động sử dụng S3 + CloudFront + RDS + EC2.  
* Làm quen với môi trường phát triển đám mây AWS Cloud9 và hiểu về AWS Well-Architected Framework để thiết kế hệ thống tối ưu.  
* Biết cách triển khai nhanh ứng dụng mã nguồn mở (WordPress, PrestaShop) bằng Amazon Lightsail.  

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | ---------- | ------------- | ---------------- | --------------- |
| 2 | - Tìm hiểu cơ bản về AWS Cloud9 và thực hành: <br>&emsp; + Tạo Cloud9 instance <br>&emsp; + Sử dụng command line <br>&emsp; + Làm việc với file text <br>&emsp; + Sử dụng AWS CLI trên AWS Cloud9 <br> - Tìm hiểu cơ bản về AWS S3 <br>&emsp; + Khái niệm và mục đích sử dụng dịch vụ lưu trữ đối tượng (Object Storage) <br>&emsp; + Cấu trúc Bucket và Object <br>&emsp; + Các lớp lưu trữ (Storage Classes) phổ biến <br>&emsp; + Quản lý quyền truy cập và chia sẻ dữ liệu (Public Access, Bucket Policy) <br> - **Thực hành:** <br>&emsp; + Tạo S3 bucket <br>&emsp; + Tải dữ liệu lên S3 bucket <br>&emsp; + Bật tính năng lưu trữ website tĩnh <br>&emsp; + Cấu hình Block Public Access <br>&emsp; + Cấu hình truy cập đối tượng công khai <br>&emsp; + Kiểm tra Website đã host | 29/09/2025 | 29/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Tìm hiểu cơ bản về Amazon CloudFront <br>&emsp; + Khái niệm dịch vụ CDN và vai trò của CloudFront trong phân phối nội dung <br>&emsp; + Cấu trúc cơ bản: Origin, Distribution, Edge Location <br>&emsp; + Cách CloudFront tăng tốc độ truy cập nội dung từ S3 <br> - Tìm hiểu về S3 Versioning <br>&emsp; + Lợi ích chính của Versioning <br>&emsp; + Cách Versioning hoạt động <br>&emsp; + Trạng thái Versioning <br> - **Thực hành:** <br>&emsp; + Chặn tất cả truy cập công cộng vào S3 <br>&emsp; + Tạo CloudFront Distribution phục vụ S3 Bucket <br>&emsp; + Kiểm tra Amazon CloudFront <br>&emsp; + Bật tính năng Versioning cho Bucket và thay đổi nội dung <br>&emsp; + Kiểm tra tính năng Versioning trên S3 và CloudFront | 30/09/2025 | 30/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) và [aws.amazon.com/cloudfront](https://aws.amazon.com/cloudfront/) |
| 4 | - Tìm hiểu về Amazon S3 Cross-Region Replication (CRR): <br>&emsp; + Lợi ích chính <br>&emsp; + Cách CRR hoạt động <br> - Tìm hiểu về AWS Well-Architected Framework: <br>&emsp; + Khái niệm và sáu trụ cột kiến trúc tối ưu của AWS <br>&emsp; + Các ống kính kiến trúc tối ưu (Well-Architected Lenses) <br> - **Thực hành:** <br>&emsp; + Di chuyển Object sang Bucket mới <br>&emsp; + Sao chép S3 Object sang Region khác <br> - Tìm hiểu về thực hành tốt nhất & khắc phục sự cố AWS S3: <br>&emsp; + Bảo mật <br>&emsp; + Hiệu suất <br>&emsp; + Tối ưu chi phí <br>&emsp; + Lỗi thường gặp và cách khắc phục | 01/10/2025 | 01/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) và [aws.amazon.com/architecture/well-architected](https://aws.amazon.com/architecture/well-architected/) |
| 5 | - Tìm hiểu cơ bản về Amazon RDS: <br>&emsp; + Giới thiệu dịch vụ cơ sở dữ liệu quan hệ (Relational Database Service) <br>&emsp; + Các loại cơ sở dữ liệu hỗ trợ (MySQL, PostgreSQL, MariaDB, v.v.) <br>&emsp; + Cấu trúc cơ bản: DB Instance, Subnet Group, Security Group <br> - **Thực hành:** <br>&emsp; + Tạo VPC và SG cho EC2, RDS <br>&emsp; + Tạo DB Subnet Group <br>&emsp; + Tạo EC2 instance <br>&emsp; + Tạo RDS database instance <br>&emsp; + Triển khai ứng dụng <br>&emsp; + Backup và Restore trong Amazon RDS | 02/10/2025 | 03/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - **Thực hành:** Triển khai ứng dụng mã nguồn mở trên Amazon Lightsail và cấu hình tài nguyên: <br>&emsp; + Triển khai cơ sở dữ liệu trên Lightsail <br>&emsp; + Triển khai máy chủ WordPress <br>&emsp; + Cấu hình Ubuntu <br>&emsp; + Cấu hình Networking <br>&emsp; + Cấu hình WordPress <br>&emsp; + Triển khai PrestaShop E-Commerce Instance | 03/10/2025 | 03/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Kết quả đạt được tuần 4:

* Làm quen và thao tác thành thạo trên AWS Cloud9, biết cách sử dụng CLI và quản lý tài nguyên qua môi trường phát triển đám mây.  
* Hiểu rõ cách hoạt động của Amazon S3, biết tạo và cấu hình Bucket, triển khai Website tĩnh, Block Public Access và Versioning.  
* Biết cách phân phối nội dung toàn cầu với Amazon CloudFront, tích hợp CloudFront với S3 để tăng tốc và bảo mật Website.  
* Hiểu và thực hành được Cross-Region Replication (CRR) trong S3, áp dụng best practices về bảo mật, hiệu suất và chi phí.  
* Triển khai được Amazon RDS và kết nối với EC2 Instance, thực hiện Backup/Restore cơ sở dữ liệu.  
* Làm quen với AWS Well-Architected Framework, nắm được sáu trụ cột kiến trúc tối ưu của AWS.  
* Triển khai và cấu hình thành công ứng dụng WordPress và PrestaShop trên Amazon Lightsail, hiểu cách quản lý tài nguyên và bảo mật cơ bản.  
