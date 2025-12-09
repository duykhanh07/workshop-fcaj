---
title: "Worklog Tuần 2"
#date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Hiểu và thực hành các dịch vụ hạ tầng cốt lõi của AWS, bao gồm VPC, EC2 và Site-to-Site VPN.  
* Biết cách xây dựng, cấu hình và bảo mật mạng VPC cơ bản.  
* Triển khai và quản lý EC2 Instance trong môi trường Public và Private Subnet.  
* Nắm khái niệm kết nối mạng giữa on-premises và AWS Cloud thông qua Site-to-Site VPN.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------- |
| 2   | - Tìm hiểu cơ bản về VPC: <br>&emsp; + Subnets <br>&emsp; + Route Table <br>&emsp; + Internet Gateway <br>&emsp; + NAT Gateway <br>&emsp; + Security Groups <br>&emsp; + Network Access Control Lists <br> - **Thực hành:** Xây dựng môi trường VPC với các thành phần mạng cơ bản (VPC, Subnets, Route Table, Internet Gateway, Security Groups, VPC Flow Logs) và thiết lập cấu trúc mạng an toàn, có khả năng mở rộng. | 15/09/2025   | 15/09/2025      | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) và YouTube của AWS Study Group |
| 3   | - Tìm hiểu cơ bản về EC2: <br>&emsp; + Khái niệm, tính năng và cách hoạt động của EC2 <br>&emsp; + Các loại instance (General Purpose, Compute Optimized, Memory Optimized, …) <br>&emsp; + Khái niệm AMI, EBS, Security Group và Key Pair <br>&emsp; + Cấu hình mạng, lưu trữ và quyền truy cập cho EC2 Instance <br>&emsp; + Quy trình khởi tạo, kết nối và quản lý vòng đời instance (Start, Stop, Reboot, Terminate) | 16/09/2025   | 16/09/2025      | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 4   | - Tìm hiểu về VPC Reachability Analyzer, EC2 Instance Connect Endpoint, AWS Systems Manager Session Manager, CloudWatch Monitoring <br> - **Thực hành:** Triển khai Amazon EC2 Instances <br>&emsp; + Tạo EC2 instance trong Public và Private Subnet <br>&emsp; + Kết nối SSH <br>&emsp; + Tạo Elastic IP <br>&emsp; + Tạo NAT Gateway <br>&emsp; + Cấu hình Route Table cho Private Subnet <br>&emsp; + Phân tích kết nối với VPC Reachability Analyzer <br>&emsp; + Triển khai EIC Endpoint | 17/09/2025   | 18/09/2025      | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 5   | - Tìm hiểu về AWS Site-to-Site VPN: <br>&emsp; + Virtual Private Gateway (VPG) <br>&emsp; + Customer Gateway (CGW)                                                                                                                               | 18/09/2025   | 18/09/2025      | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6   | - **Thực hành:** <br>&emsp; + Thiết lập VPC cho Site-to-Site VPN <br>&emsp; + Triển khai EC2 Instance cho Customer Gateway <br>&emsp; + Tạo Virtual Private Gateway <br>&emsp; + Tạo Customer Gateway <br>&emsp; + Thiết lập kết nối VPN giữa VPC và môi trường on-premises <br>&emsp; + Cấu hình Customer Gateway <br>&emsp; + Chỉnh AWS VPN Tunnel | 19/09/2025   | 19/09/2025      | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Kết quả đạt được tuần 2:

* Hiểu rõ cấu trúc và nguyên lý hoạt động của VPC trên AWS, bao gồm:  
  Subnet, Route Table, Internet Gateway, NAT Gateway, Security Group và Network ACL.  
* Tự xây dựng được mô hình VPC cơ bản có khả năng mở rộng và đảm bảo an toàn truy cập giữa các subnet.  
* Nắm vững kiến thức cơ bản về Amazon EC2, bao gồm:
  * Phân loại instance và lựa chọn AMI phù hợp  
  * Cấu hình lưu trữ (EBS), bảo mật (Security Group, Key Pair) và mạng (Elastic IP, Route Table)  
  * Quản lý vòng đời instance (khởi tạo, kết nối, dừng, xóa)  
* Thực hành triển khai EC2 Instance trong Public và Private Subnet, kết nối qua SSH và quản lý truy cập bằng Instance Connect Endpoint.  
* Biết sử dụng VPC Reachability Analyzer và CloudWatch để giám sát và phân tích kết nối mạng.  
* Hiểu khái niệm, thành phần và quy trình thiết lập Site-to-Site VPN giữa hệ thống on-premises và AWS Cloud.  
* Tự thực hành tạo kết nối VPN an toàn giữa Customer Gateway và Virtual Private Gateway, giúp mở rộng mạng nội bộ lên đám mây AWS.
