---
title: "Worklog Tuần 5"
#date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Mục tiêu tuần 5:
* Làm quen với các khái niệm về mở rộng hệ thống (Load Balancer, Auto Scaling) và biết cách triển khai thực tế trên AWS.  
* Nắm được quy trình triển khai ứng dụng và cơ sở dữ liệu bằng RDS, quản lý ảnh máy (AMI) và tạo Launch Template.  
* Hiểu và ứng dụng CloudWatch để giám sát tài nguyên, theo dõi logs, tạo metric, alarm, và dashboard.  
* Tìm hiểu cơ bản về Route 53, DNS resolver và tích hợp với Microsoft AD trong môi trường cloud.  

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------- | ---------------- | -------------- |
| 2 | - Thực hành <br> &emsp; + Triển khai Akaunting Instance <br> &emsp;&emsp; + Tạo instance <br> &emsp;&emsp; + Cấu hình Networking <br> &emsp;&emsp; + Triển khai Prestashop E-Commerce <br> &emsp; + Bảo mật ứng dụng Wordpress <br> &emsp; + Tạo bản Snapshot <br> &emsp; + Dịch chuyển sang instance lớn hơn <br> &emsp; + Tạo cảnh báo để thông báo cho quản trị viên nếu một sự kiện cụ thể xảy ra <br> - Tìm hiểu về Auto Scaling Group, AMIs và Launch Template <br> - Thực hành <br> &emsp; + Thiết lập hạ tầng mạng cơ bản cho ứng dụng FCJ Management <br> &emsp;&emsp; + VPC và Subnet <br> &emsp;&emsp; + Internet Gateway <br> &emsp;&emsp; + Route Table <br> &emsp;&emsp; + Security Groups <br> &emsp; + Tạo EC2 Instance <br> &emsp; + Tạo DB Subnet Group cho Amazon RDS <br> &emsp; + Tạo Amazon RDS Database Instance <br> &emsp; + Cài đặt dữ liệu cho Database <br> &emsp; + Triển khai máy chủ web <br> &emsp; + Chuẩn bị dữ liệu cho Predictive scaling <br> &emsp; + Tạo Amazon Machine Images (AMIs) từ EC2 <br> &emsp; + Tạo Launch Templates | 06/10/2025 | 06/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Tìm hiểu về Elastic Load Balancing (ELB) <br> &emsp; + Target Group <br> &emsp; + Application Load Balancer <br> &emsp; + Network Load Balancer <br> &emsp; + Classic Load Balancer <br> &emsp; + Gateway Load Balancer <br> - Tìm hiểu về Các giải pháp / kỹ thuật scaling <br> &emsp; + Manual scaling <br> &emsp; + Scheduled scaling <br> &emsp; + Dynamic scaling <br> &emsp; + Predictive scaling <br> - Thực hành: <br> &emsp; + Tạo Target Group <br> &emsp; + Tạo Application Load Balancer <br> &emsp; + Kiểm tra kết quả triển khai Elastic Load Balancing (ELB) <br> &emsp; + Tạo Auto Scaling Group <br> &emsp; + kết nối ASG với Load Balancer <br> &emsp; + Thiết lập Size và Scaling Policies <br> &emsp; + Thiết lập thông báo <br> - Thực hành Các giải pháp / kỹ thuật scaling <br> &emsp; + Manual scaling <br> &emsp; + Scheduled scaling <br> &emsp; + Dynamic scaling <br> &emsp; + Predictive scaling | 07/10/2025 | 07/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) và youtube của AWS Study Group |
| 4 | - Tìm hiểu về AWS CloudWatch <br> &emsp; + Tính năng <br> &emsp; + Container Insights <br> &emsp; + Lợi ích chính <br> - Tìm hiểu về CloudWatch Metric, CloudWatch Logs, CloudWatch Alarms, CloudWatch Dashboards | 08/10/2025 | 08/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 5 | - Thực hành: <br> &emsp; + Triển khai CloudFormation Stack <br> &emsp; + Xem các Metrics <br> &emsp; + Thao tác với biểu đồ trong CloudWatch <br> &emsp; + Thực hiện các phép tìm kiếm Metrics <br> &emsp; + Thực hiện các phép toán học <br> &emsp; + Tạo dynamic labels <br> &emsp; + Xem CloudWatch Logs <br> &emsp; + Tạo logs từ một ứng dụng, sau đó là sẽ query những logs này ở trong CloudWatch Logs Insights <br> &emsp; + Trích xuất dữ liệu có giá trị từ logs và chuyển đổi chúng thành các metrics bằng CloudWatch Metric Filters <br> &emsp; + Thiết lập Alarm cho Error Log Metric <br> &emsp; + Tạo một Dashboard đơn giản để tập trung quản lý các Metrics và Alarms | 09/10/2025 | 10/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Tìm hiểu về Route 53 <br> &emsp; + Outbound Endpoints <br> &emsp; + Inbound Endpoints <br> &emsp; + Route 53 Resolver Rules <br> - Tìm hiểu về AWS Quick Starts, AWS CloudFormation, AWS Directory Service <br> - Thực hành: <br> &emsp; + Tạo Key Pair, khởi tạo CloudFormation Template, cấu hình Security Group <br> &emsp; + Kết nối đến RDGW <br> &emsp; + Triển khai Microsoft AD <br> &emsp; + Thiết lập DNS <br> &emsp;&emsp; + Tạo Route 53 Outbound Endpoint <br> &emsp;&emsp; + Tạo Route 53 Resolver Rules <br> &emsp;&emsp; + Tạo Route 53 Inbound Endpoints <br> &emsp;&emsp; + Thử nghiệm kết quả | 10/10/2025 | 10/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Kết quả đạt được tuần 5:
* Triển khai và quản lý các ứng dụng web trên EC2/Lightsail, bao gồm Akaunting, WordPress và Prestashop, kèm theo cấu hình bảo mật, snapshot và nâng cấp instance.  
* Thiết lập Auto Scaling Group, Launch Template và AMI để mở rộng hệ thống, áp dụng các phương pháp scaling: manual, scheduled, dynamic, predictive.  
* Tạo và quản lý Load Balancer (ALB, NLB) kết nối với Auto Scaling Group để cân bằng tải và đảm bảo hệ thống ổn định.  
* Giám sát hệ thống bằng CloudWatch: Metrics, Logs, Alarms, Dashboard và trích xuất dữ liệu từ logs để theo dõi hiệu suất.  
* Triển khai cơ sở dữ liệu với Amazon RDS, thiết lập DB Subnet Group và kết nối EC2 với RDS để ứng dụng hoạt động hoàn chỉnh.  
* Tìm hiểu và thực hành Route 53, triển khai DNS nội bộ, Microsoft AD, cấu hình Resolver Rules và kiểm tra kết nối.  

