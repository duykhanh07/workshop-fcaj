---
title: "Worklog Tuần 6"
#date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu tuần 6:
* Hiểu quy trình xây dựng AMI, Launch Template và triển khai WordPress trong môi trường có khả năng mở rộng.  
* Sử dụng Lambda Function để tối ưu chi phí, tự động bật/tắt EC2 Instance theo nhu cầu.  
* Làm quen với Grafana để giám sát dữ liệu và hệ thống.  
* Biết cách quản lý tài nguyên bằng Tag, Resource Groups và áp dụng kiểm soát truy cập bằng IAM.  
* Tìm hiểu AWS Systems Manager để chạy lệnh từ xa và quản lý bản vá (Patch Manager).  

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------- | ---------------- | -------------- |
| 2 | - Thực hành: triển khai ứng dụng Wordpress với Auto Scaling Group nhằm đảm bảo khả năng co giãn của ứng dụng đó theo nhu cầu của người truy cập <br> &emsp; + Chuẩn bị VPC và subnet, tạo Security Group cho EC2, Database và khởi tạo EC2, Database <br> &emsp; + Cài đặt wordpress trên EC2 <br> &emsp; + Thực hiện tạo Autoscaling cho wordpress Instance <br> &emsp;&emsp; + Khởi tạo AMI từ Webserver Instance <br> &emsp;&emsp; + Khởi tạo Launch Template <br> &emsp;&emsp; + Khởi tạo Target Group <br> &emsp;&emsp; + Khởi tạo Load Balancer <br> &emsp;&emsp; + Khởi tạo Auto Scaling Group <br> &emsp; + Sao lưu và phục hồi database <br> &emsp; + Khởi tạo Cloudfront cho Web Server | 13/10/2025 | 13/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Tìm hiểu về Lambda function <br> - Thực hành: sử dụng Lambda function để tối ưu hóa chi phí cho hệ thống của bạn trên môi trường AWS <br> &emsp; + Tạo VPC, Security Group, EC2 <br> &emsp; + Incoming Web-hooks slack <br> &emsp; + Tạo tag cho instance <br> &emsp; + Tạo Role cho Lambda <br> &emsp; + Tạo Lambda Function thực hiện chức năng Stop instances <br> &emsp; + Tạo Lambda Function thực hiện chức năng Start instances <br> &emsp; + Kiểm tra kết quả | 14/10/2025 | 14/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 4 | - Tìm hiểu về Grafana <br> - Thực hành: <br> &emsp; + Tạo VPC và Subnet, Security Group, Linux EC2 Instance <br> &emsp; + Tạo IAM User, IAM Role, gán IAM Role cho EC2 Instance <br> &emsp; + Cài đặt Grafana <br> &emsp; + Thực hiện kết nối EC2 bằng MobaXterm <br> &emsp; + Giám sát với Grafana | 15/10/2025 | 15/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) và [grafana.com/grafana](https://grafana.com/grafana) |
| 5 | - Tìm hiểu về quản lý tài nguyên bằng Tag và Resource Groups <br> - Thực hành: <br> &emsp; + Tạo EC2 Instance có tag <br> &emsp; + Thêm hoặc xóa tag trên các tài nguyên đơn lẻ và trên các nhóm các tài nguyên <br> &emsp; + Lọc tài nguyên theo tag <br> &emsp; + Tạo một Resource Group <br> - Thực hành: quản lý truy cập vào dịch vụ EC2 Resource Tag với AWS IAM <br> &emsp; + Tạo IAM User <br> &emsp; + Tạo IAM Policy <br> &emsp; + Tạo IAM Role <br> &emsp; + Chuyển đổi Role <br> &emsp; + Kiểm tra IAM Policy <br> &emsp;&emsp; + Tiến hành truy cập EC2 console ở AWS Region - Tokyo <br> &emsp;&emsp; + Tiến hành truy cập EC2 console ở AWS Region - North Virginia <br> &emsp;&emsp; + Tiến hành tạo EC2 instance khi không có và có Tags thỏa điều kiện <br> &emsp;&emsp; + Chỉnh sửa Resource Tag trên EC2 Instance <br> &emsp;&emsp; + Kiểm tra chính sách | 16/10/2025 | 16/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Tìm hiểu về AWS Systems Manager <br> - Thực hành: quản lý Patch và chạy câu lệnh trên nhiều máy chủ với AWS System Manager <br> &emsp; + Tạo VPC, Subnet, EC2 Instance, IAM Role và gán IAM Role <br> &emsp; + Thiết lập Patch Manager <br> &emsp; + Run Command | 17/10/2025 | 17/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Kết quả đạt được tuần 6:
* Triển khai WordPress theo mô hình mở rộng gồm: tạo VPC/Subnet/SG, cài Webserver, tạo AMI, Launch Template, Target Group, Load Balancer và Auto Scaling Group; đồng thời sao lưu – phục hồi database và cấu hình CloudFront.  
* Xây dựng giải pháp tự động tối ưu chi phí bằng Lambda, gồm tạo Role, gán tag, tạo Lambda Function Stop/Start EC2 và kiểm tra kích hoạt qua Slack Webhook.  
* Triển khai Grafana để giám sát hệ thống, gồm tạo EC2 Linux, IAM User/Role, cài Grafana và kết nối để theo dõi dữ liệu.  
* Quản lý tài nguyên bằng Tag và Resource Groups, đồng thời áp dụng IAM để kiểm soát truy cập theo Tag và thử nghiệm ở nhiều Region.  
* Sử dụng AWS Systems Manager để quản lý bản vá bằng Patch Manager và chạy lệnh từ xa với Run Command.  


