---
title: "Worklog Tuần 7"
#date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu tuần 7:
* Hiểu và thực hành AWS Systems Manager – Session Manager, gồm kết nối EC2 public/private, thiết lập Endpoint và quản lý session logs.  
* Nắm được các khái niệm và thực hành cơ bản & nâng cao của AWS CloudFormation, bao gồm tạo template, tạo stack, StackSets và Drift Detection.  
* Biết sử dụng AWS Cloud9 để làm việc với CloudFormation và môi trường AWS.  
* Hiểu và thiết lập AWS IAM Identity Center (AWS SSO) trong AWS Organization: Users, Groups, Permission Sets và phân quyền truy cập.  
* Áp dụng Time-based Access Control và Customer Managed Policies để thiết lập các Permission Set nâng cao cho từng nhóm người dùng trong Organization.  

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------- | ---------------- | -------------- |
| 2 | - Tìm hiểu về Session Manager <br> - Thực hành: làm việc với Amazon System Manager - Session Manager <br> &emsp; + Tạo VPC, Public subnet, Private subnet, security group, máy chủ Linux public và Windows private <br> &emsp; + Tạo IAM Role <br> &emsp; + Tạo kết nối đến máy chủ EC2 Public <br> &emsp; + Tạo kết nối đến máy chủ EC2 Private <br> &emsp;&emsp; + Kích hoạt DNS hostnames <br> &emsp;&emsp; + Tạo Endpoint ssm <br> &emsp;&emsp; + Tạo Endpoint ssmmessages <br> &emsp;&emsp; + Tạo Endpoint ec2messages <br> &emsp;&emsp; + Gán IAM role và restart EC2 instance <br> &emsp; + Quản lý session logs <br> &emsp;&emsp; + Cập nhật IAM Role <br> &emsp;&emsp; + Tạo S3 bucket và S3 Gateway endpoint <br> &emsp;&emsp; + Theo dõi session logs <br> &emsp;&emsp; + Kiểm tra Session logs trong S3 <br> &emsp; + Port Forwarding <br> &emsp;&emsp; + Tạo IAM User có quyền kết nối SSM <br> &emsp;&emsp; + Cài đặt và cấu hình AWS CLI và Session Manager Plugin <br> &emsp;&emsp; + Thực hiện Portforwarding | 20/10/2025 | 20/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Tìm hiểu về AWS CloudFormation và AWS Cloud9 <br> - Thực hành: tạo một CloudFormation template và một vài tính năng cơ bản của CloudFormation, và cách kiểm tra template <br> &emsp; + Tạo IAM User, IAM Role <br> &emsp; + Tạo Workspace <br> &emsp; + Tạo CloudFormation Template | 21/10/2025 | 21/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/), https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html, https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-template-resource-type-ref.html |
| 4 | - Tìm hiểu về StackSets, Drift Detection, Simple Queue Service <br> - Thực hành: CloudFormation nâng cao <br> &emsp; + Tạo Lambda Function <br> &emsp; + Tạo Stack trong CloudFormation <br> &emsp; + Kết nối EC2 Instance <br> &emsp; + Ánh xạ và Stacksets <br> &emsp; + Thay đổi cấu hình tài nguyên với Drift | 22/10/2025 | 22/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) và https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html |
| 5 | - Thực hành: Thiết lập Single Sign-On (Amazon SSO) cho Organization <br> &emsp; + Kích hoạt AWS Organizations <br> &emsp; + Kích hoạt IAM Identity Center <br> &emsp; + Tạo User và Groups trong IAM Identity Center <br> &emsp; + Tạo Permission Sets <br> &emsp; + Cung cấp Permission Sets <br> &emsp; + Kiểm tra quyền truy cập dựa trên người dùng, nhóm và permission set <br> &emsp;&emsp; + Triển khai CloudFormation Template <br> &emsp;&emsp; + Xác thực quyền truy cập <br> &emsp;&emsp; + Kiểm tra quyền Administrator <br> &emsp;&emsp; + Kiểm tra quyền ReadOnly <br> &emsp; + Cấu hình AWS CLI - Làm mới thông tin xác thực thủ công <br> &emsp; + Cấu hình AWS CLI - Làm mới thông tin xác thực tự động | 23/10/2025 | 23/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Thực hành: Thiết lập Single Sign-On (Amazon SSO) cho Organization <br> &emsp; + Sử dụng Time-based access control cung cấp quyền truy cập tạm thời vào AWS accounts cho Security Auditor <br> &emsp;&emsp; + Tạo Permission Set với quyền truy cập cần thiết cho Security Auditor dưới dạng inline policy với các điều kiện time-based access control <br> &emsp;&emsp; + Tạo Group cho Security Auditors <br> &emsp;&emsp; + Gán người dùng vào Group Security Auditors <br> &emsp;&emsp; + Gán quyền truy cập vào AWS Account cho nhóm Security Auditors với permission set mới tạo <br> &emsp; + Tạo CMP, tạo permission set bằng cách gắn CMP đã tạo và cuối cùng là cung cấp permission set cho AWS account <br> &emsp;&emsp; + Tạo Customer Managed IAM Policy <br> &emsp;&emsp; + Tạo permission Set với Customer Managed policy <br> &emsp;&emsp; + Tạo Group và tạo user và thêm vào Group <br> &emsp;&emsp; + Gán Permission set cho AWS Account và xác minh quyền truy cập | 24/10/2025 | 24/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Kết quả đạt được tuần 7:
* Làm chủ Session Manager trong AWS Systems Manager: tạo môi trường VPC public/private, gán IAM Role, thiết lập các endpoint SSM, EC2Messages, SSMMessages và kết nối thành công đến cả EC2 Public và Private.  
* Quản lý và theo dõi Session Logs: cấu hình S3 bucket, S3 Gateway Endpoint, cập nhật IAM Role để ghi log và xác minh log hoạt động đúng trong S3.  
* Thực hiện Port Forwarding qua SSM: tạo IAM User, cấu hình AWS CLI + SSM Plugin, thực hiện port forwarding an toàn mà không cần mở port trong Security Group.  
* Tìm hiểu và sử dụng AWS CloudFormation: tạo Workspace Cloud9, tạo IAM User/Role, viết CloudFormation template và kiểm tra tính hợp lệ của template.  
* Triển khai CloudFormation nâng cao: tạo Lambda Function, xây dựng Stack, thiết lập StackSets, ánh xạ tài nguyên, kết nối EC2 và kiểm tra Drift Detection khi thay đổi cấu hình.  
* Thiết lập Single Sign-On với IAM Identity Center cho toàn bộ AWS Organization: tạo Users/Groups, Permission Sets, cấp quyền Administrator/ReadOnly và xác thực quyền theo từng user/group.  
* Cấu hình AWS CLI với IAM Identity Center: thử nghiệm làm mới thông tin đăng nhập thủ công và tự động, đảm bảo nhận diện đúng permission set.  
* Áp dụng Time-based Access Control để cấp quyền truy cập tạm thời cho Security Auditor: tạo Permission Set có điều kiện theo thời gian, phân nhóm Security Auditor, gán quyền và xác thực hoạt động đúng.  
* Tạo Customer Managed Policy (CMP) và kết hợp CMP vào Permission Set, gán cho AWS Account và kiểm tra quyền truy cập thực tế của user.
