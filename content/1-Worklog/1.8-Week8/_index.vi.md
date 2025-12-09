---
title: "Worklog Tuần 8"
#date: "`r Sys.Date()`"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Mục tiêu tuần 8:
* Hiểu và thực hành quản lý danh tính với IAM Identity Center và các API User/Group.  
* Nắm được cơ chế giới hạn quyền bằng IAM Permission Boundary và điều kiện khi chuyển Role (Role Switching).  
* Làm quen với các dịch vụ bảo mật: Security Hub, AWS WAF và các tiêu chuẩn đánh giá an ninh.  
* Tìm hiểu các dịch vụ giám sát – nhật ký – mã hóa như CloudTrail, KMS và Athena.  
* Biết cách sử dụng AWS Backup và SNS để xây dựng quy trình sao lưu và gửi thông báo.  

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------- | ---------------- | -------------- |
| 2 | - Thực hành: IAM Identity Center Identity Store APIs <br> &emsp; + Environment Setup <br> &emsp; + Test Setup <br> &emsp; + Use case Prerequisites <br> &emsp; + AWS IAM Identity Center User and Group API Operations <br> &emsp; + Create User and add to the group <br> &emsp; + Update User’s Group Membership <br> &emsp; + AWS IAM Identity Center User and Group Audit Operations <br> - Dịch Blog: <br> &emsp; + Announcing Amazon Quick Suite: your agentic teammate for answering questions and taking action <br> &emsp; + AWS Transfer Family SFTP connectors now support VPC-based connectivity <br> &emsp; + AWS Weekly Roundup: Amazon Quick Suite, Amazon EC2, Amazon EKS, and more (October 13, 2025) | 27/10/2025 | 27/10/2025 | https://cloudjourney.awsstudygroup.com/ và https://aws.amazon.com/vi/blogs/aws/reimagine-the-way-you-work-with-ai-agents-in-amazon-quick-suite/#:~:text=Today%2C%20we%E2%80%99re%20announcing%20Amazon%20Quick%20Suite%2C%20a%20new,and%20turns%20those%20insights%20into%20actions%20for%20you. và https://aws.amazon.com/vi/blogs/aws/aws-transfer-family-sftp-connectors-now-support-vpc-based-connectivity/ và https://aws.amazon.com/vi/blogs/aws/aws-weekly-roundup-amazon-quick-suite-amazon-ec2-amazon-eks-and-more-october-13-2025/ |
| 3 | - Tìm hiểu về IAM Permission Boundary <br> - Thực hành: Giới hạn Quyền của User với IAM Permission Boundary <br> &emsp; + Tạo Policy Giới hạn <br> &emsp; + Tạo IAM User Giới Hạn <br> &emsp; + Kiểm tra xem liệu user được gán quyền có bị giới hạn quyền bởi Permission Boundary <br> - Thực hành: Giới hạn chuyển Role theo Condition <br> &emsp; + Tạo IAM Group <br> &emsp; + Tạo các IAM User và kiểm tra quyền của chúng <br> &emsp; + Cấu hình Role Condition <br> &emsp;&emsp; + Tạo IAM Role có quyền Quản trị <br> &emsp;&emsp; + Cấu hình Switch role cho IAM User <br> &emsp;&emsp; + Giới hạn Thời gian được phép Switch role <br> &emsp;&emsp; + Giới hạn IP được phép Switch role | 28/10/2025 | 28/10/2025 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Tìm hiểu về AWS Security Hub, các tiêu chuẩn bảo mật <br> - Thực hành: Kích hoạt Security Hub thông qua console <br> - Thực hành: Kiểm tra đánh giá theo từng bộ tiêu chuẩn <br> - Tìm hiểu về AWS Web Application Firewall <br> - Thực hành: thực hiện tạo môi trường cho workshop bao gồm việc tạo S3 bucket và triển khai một Ứng dụng mẫu <br> &emsp; + Tạo S3 bucket <br> &emsp; + Triển khai một Ứng dụng OWASP Juice Shop <br> - Thực hành: Sử dụng AWS WAF <br> &emsp; + Triển khai Web ACLs với managed rules <br> &emsp; + Tạo Custom Rule <br> &emsp; + Định nghĩa WAF rule ở định dạng JSON. Các logic phức tạp cần phải được định nghĩa sử dụng các toán tử AND, OR và NOT <br> &emsp; + Kiểm thử Rule mới <br> &emsp; + Ghi nhật ký request | 29/10/2025 | 29/10/2025 | https://cloudjourney.awsstudygroup.com/ và https://aws.amazon.com/vi/waf/ |
| 5 | - Tìm hiểu về AWS Key Management Service, AWS CloudTrail, Amazon Athena <br> - Thực hành: Quản lý Khóa với dịch vụ Key Management Service (AWS KMS) <br> &emsp; + Tạo Policy, Role, Group và User <br> &emsp; + Tạo KMS <br> &emsp; + Tạo Bucket và tải dữ liệu lên S3 <br> &emsp; + Tạo CloudTrail và ghi nhật ký vào CloudTrail <br> &emsp; + Tạo Amazon Athena và truy xuất dữ liệu với Athena <br> &emsp; + Kiểm thử và chia sẻ dữ liệu mã hóa trên S3 | 30/10/2025 | 30/10/2025 | https://cloudjourney.awsstudygroup.com/ và https://docs.aws.amazon.com/kms/latest/developerguide/overview.html và https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html và https://docs.aws.amazon.com/athena/latest/ug/what-is.html |
| 6 | - Tìm hiểu về AWS Backup, AWS Simple Notification Service <br> - Thực hành: Triển khai kế hoạch sao lưu hệ thống với AWS Backup <br> &emsp; + Tạo S3 bucket <br> &emsp; + Sử dụng AWS Backup để tạo ra một kế hoạch sao lưu (backup plan) cho các tài nguyên đang hoạt động trên AWS <br> &emsp; + Tạo Backup Plan <br> &emsp; + Sử dụng dịch vụ AWS SNS (Simple Notification Service) để gửi thông báo liên quan đến các hoạt động sao lưu đang diễn ra <br> &emsp; + Kiểm tra hoạt động sao lưu | 31/10/2025 | 31/10/2025 | https://cloudjourney.awsstudygroup.com/ và https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html và https://aws.amazon.com/vi/sns/ |

### Kết quả đạt được tuần 8:
* Quản lý người dùng tập trung bằng IAM Identity Center: thiết lập environment, tạo user/group, cập nhật quyền và thực hiện các thao tác audit qua API.  
* Tăng cường kiểm soát quyền truy cập bằng Permission Boundary và Role Condition: giới hạn quyền user, giới hạn switch role theo thời gian/IP và thử nghiệm phân quyền thực tế.  
* Triển khai các giải pháp bảo mật lớp ứng dụng: kích hoạt Security Hub, đánh giá theo tiêu chuẩn bảo mật; triển khai AWS WAF với managed rules, custom rules, JSON rule logic và ghi nhật ký request.  
* Quản lý mã hóa, theo dõi và phân tích log: tạo KMS key, cấu hình CloudTrail, upload dữ liệu S3, truy vấn log bằng Athena và kiểm thử chia sẻ dữ liệu mã hóa.  
* Thiết lập quy trình sao lưu hệ thống với AWS Backup: tạo backup plan, cấu hình tài nguyên sao lưu, kết hợp SNS để gửi thông báo và kiểm tra kết quả sao lưu.  
