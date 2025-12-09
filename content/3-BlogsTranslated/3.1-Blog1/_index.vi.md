---
title: "Blog 1"
#date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AWS Transfer Family SFTP connectors hiện nay đã hỗ trợ khả năng kết nối dựa trên VPC

Nhiều tổ chức phụ thuộc vào [Secure File Transfer Protocol (SFTP)](https://aws.amazon.com/what-is/sftp/) như một tiêu chuẩn trong ngành để trao đổi các dữ liệu kinh doanh quan trọng. Trước đây, việc kết nối an toàn đến các máy chủ SFTP riêng thường đòi hỏi phải tự xây dựng hạ tầng, viết các đoạn script thủ công, hoặc mở các endpoint ra internet công cộng.

Hiện nay, [AWS Transfer Family](https://aws.amazon.com/aws-transfer-family/) [SFTP connectors](https://docs.aws.amazon.com/transfer/latest/userguide/creating-connectors.html) đã hỗ trợ kết nối tới các máy chủ SFTP từ xa thông qua môi trường [Amazon Virtual Private Cloud (Amazon VPC)](https://aws.amazon.com/vi/vpc/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el). Bạn có thể truyền tệp giữa [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/vi/s3/?nc2=type_a&?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=elhttps://aws.amazon.com/vi/s3/?nc2=type_a&?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) và các máy chủ SFTP riêng tư hoặc công khai, đồng thời áp dụng các biện pháp kiểm soát bảo mật và cấu hình mạng đã được xác định sẵn trong VPC của bạn. Tính năng này giúp bạn tích hợp các nguồn dữ liệu từ môi trường on-premises, máy chủ riêng do đối tác lưu trữ, hoặc các endpoint hướng ra internet, trong khi vẫn duy trì sự đơn giản trong vận hành nhờ sử dụng một dịch vụ [Amazon Web Services (AWS)](https://aws.amazon.com/vi/) được quản lý hoàn toàn.

---

## Các khả năng mới với SFTP connectors

Dưới đây là các cải tiến chính:
- **Kết nối tới các máy chủ SFTP riêng tư** – Các SFTP connector giờ đây có thể truy cập đến các endpoint chỉ khả dụng trong kết nối AWS VPC của bạn. Điều này bao gồm các máy chủ được lưu trữ trong VPC của bạn hoặc VPC được chia sẻ, các hệ thống on-premises được kết nối qua [AWS Direct Connect](https://aws.amazon.com/vi/directconnect/?nc2=type_a&?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el), và các máy chủ do đối tác lưu trữ được kết nối thông qua đường hầm VPN.
- **Bảo mật và tuân thủ** – Tất cả các quá trình truyền tệp đều được định tuyến thông qua các biện pháp kiểm soát bảo mật đã được áp dụng trong VPC của bạn, chẳng hạn như [AWS Network Firewall](https://aws.amazon.com/vi/network-firewall/?nc2=h_prod_se_netf&?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) hoặc hệ thống kiểm tra tập trung lưu lượng vào và ra. Các máy chủ SFTP riêng tư vẫn được giữ riêng tư và không cần phải mở ra internet. Bạn cũng có thể sử dụng địa chỉ Elastic IP tĩnh hoặc mang địa chỉ IP của riêng bạn (BYOIP) để đáp ứng các yêu cầu trong danh sách cho phép của đối tác.
- **Hiệu năng và sự đơn giản** – Bằng cách sử dụng các tài nguyên mạng của riêng bạn như NAT gateway, AWS Direct Connect hoặc kết nối VPN, các connector có thể tận dụng băng thông cao hơn cho việc truyền tệp quy mô lớn. Bạn có thể cấu hình các connector trong vài phút thông qua [AWS Management Console](https://console.aws.amazon.com/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el), [AWS Command Line Interface (AWS CLI)](https://aws.amazon.com/vi/cli/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) hoặc [AWS SDKs](https://builder.aws.com/build/tools?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) mà không cần xây dựng các script tùy chỉnh hay sử dụng công cụ của bên thứ ba.

---

## Cách hoạt động của các kết nối SFTP dựa trên VPC

Các SFTP connector sử dụng các tài nguyên [Amazon VPC Lattice](https://aws.amazon.com/vi/vpc/lattice/) để thiết lập kết nối bảo mật thông qua VPC của bạn. Các cấu trúc chính bao gồm **cấu hình tài nguyên** và **cổng tài nguyên**. Cấu hình tài nguyên đại diện cho máy chủ SFTP đích, mà bạn chỉ định bằng địa chỉ IP riêng tư hoặc tên miền DNS công khai. Cổng tài nguyên cung cấp quyền truy cập cho SFTP connector tới các cấu hình này, cho phép việc truyền tệp đi qua VPC của bạn và các biện pháp kiểm soát bảo mật của nó.

Sơ đồ kiến trúc sau minh họa cách luồng dữ liệu di chuyển giữa Amazon S3 và các máy chủ SFTP từ xa.
![Hình ảnh sơ đồ luồng dữ liệu di chuyển giữa Amazon S3 và các máy chủ SFTP từ xa](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/10/14/Screenshot-2025-10-14-at-6.01.04%E2%80%AFPM.png)

Như được thể hiện trong kiến trúc, lưu lượng di chuyển từ Amazon S3 thông qua SFTP connector vào VPC của bạn. Cổng tài nguyên là điểm vào xử lý các kết nối đến từ connector tới các tài nguyên trong VPC của bạn. Lưu lượng đi ra được định tuyến thông qua đường ra mà bạn đã cấu hình, sử dụng Amazon VPC NAT gateway với Elastic IP cho các máy chủ công khai hoặc AWS Direct Connect và kết nối VPN cho các máy chủ riêng tư. Bạn có thể sử dụng các địa chỉ IP hiện có từ dải CIDR của VPC, giúp đơn giản hóa danh sách cho phép của máy chủ đối tác. Tường lửa tập trung trong VPC thực thi các chính sách bảo mật, và NAT gateway do khách hàng sở hữu cung cấp băng thông cao hơn cho các truyền tải quy mô lớn.

---

## Khi nào nên sử dụng tính năng này

Với khả năng này, các nhà phát triển và quản trị viên CNTT có thể đơn giản hóa quy trình làm việc trong khi vẫn đáp ứng các yêu cầu về bảo mật và tuân thủ trong nhiều tình huống khác nhau:
- **Môi trường lai** – Truyền tệp giữa Amazon S3 và các máy chủ SFTP on-premises bằng cách sử dụng AWS Direct Connect hoặc [AWS Site-to-Site VPN](https://aws.amazon.com/vi/vpn/site-to-site-vpn/), mà không cần mở các endpoint ra internet.
- **Tích hợp với đối tác** – Kết nối với máy chủ SFTP của đối tác kinh doanh chỉ khả dụng thông qua đường hầm VPN riêng hoặc VPC được chia sẻ. Cách này giúp tránh việc phải xây dựng script tùy chỉnh hoặc quản lý công cụ của bên thứ ba, giảm bớt sự phức tạp trong vận hành.
- **Các ngành được quản lý** – Định tuyến việc truyền tệp thông qua tường lửa tập trung và các điểm kiểm tra trong VPC để tuân thủ các yêu cầu bảo mật trong ngành tài chính, chính phủ hoặc y tế.
- **Truyền tải thông lượng cao** – Sử dụng cấu hình mạng riêng của bạn như NAT gateway, AWS Direct Connect hoặc kết nối VPN với Elastic IP hoặc BYOIP để xử lý các truyền tải quy mô lớn, băng thông cao, đồng thời vẫn giữ nguyên địa chỉ IP đã nằm trong danh sách cho phép của đối tác.
- **Giải pháp truyền tệp thống nhất** – Chuẩn hóa việc sử dụng Transfer Family cho cả kết nối SFTP nội bộ và bên ngoài, giúp giảm sự phân mảnh giữa các công cụ truyền tệp.

---

## Bắt đầu xây dựng với SFTP connectors

Để bắt đầu truyền tệp với các SFTP connector thông qua môi trường VPC của tôi, tôi thực hiện các bước sau:

Trước tiên, tôi cấu hình các tài nguyên VPC Lattice của mình. Trong [Amazon VPC console](https://us-east-1.console.aws.amazon.com/vpcconsole/home/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el), dưới mục **PrivateLink and Lattice** ở ngăn điều hướng, tôi chọn **Resource gateways**, chọn **Create resource gateway** để tạo một cổng hoạt động như điểm vào (ingress point) trong VPC của tôi.

![Hình ảnh bước 1](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/09/30/Create-or-select-a-resource-gateway-1.png)

Tiếp theo, dưới **PrivateLink and Lattice** ở ngăn điều hướng, tôi chọn **Resource configuration** và chọn **Create resource configuration** để tạo cấu hình tài nguyên cho máy chủ SFTP mục tiêu của mình. Chỉ định địa chỉ IP riêng hoặc tên miền DNS công khai, và cổng (thường là 22).

![Hình ảnh bước 2](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/09/30/create-resouce-configurations.png)

Sau đó, tôi cấu hình quyền [AWS Identity and Access Management (IAM)](https://aws.amazon.com/vi/iam/?nc2=type_a&?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el). Tôi đảm bảo rằng vai trò IAM được sử dụng để tạo connector có các quyền <span style="color: red; background-color: #f0f0f0">transfer:* </span> và các quyền **VPC Lattice** ( <span style="color: red; background-color: #f0f0f0">vpc-lattice:CreateServiceNetworkResourceAssociation, vpc-lattice:GetResourceConfiguration, vpc-lattice:AssociateViaAWSService</span>). Tôi cập nhật chính sách tin cậy trên vai trò IAM để chỉ định <span style="color: red; background-color: #f0f0f0">transfer.amazonaws.com</span> là principal được tin cậy. Điều này cho phép AWS Transfer Family đảm nhận vai trò này khi tạo và quản lý các SFTP connector của tôi.

Tiếp theo, tôi tạo một SFTP connector thông qua [AWS Transfer Family console](https://console.aws.amazon.com/transfer/home?refid=30641bb5-5f59-4f87-9a27-a89f5ad26ab6). Tôi chọn **SFTP Connectors** rồi chọn **Create SFTP connector.**

![Hình ảnh bước 3](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/09/30/create-SFTP-connector-1.png)

Trong phần **Connector configuration**, tôi chọn **VPC Lattice** làm loại egress, sau đó cung cấp Amazon Resource Name (ARN) của **Resource Configuration**, **Access role**, và **thông tin xác thực của Connector**. Tùy chọn, có thể bao gồm khóa máy chủ đáng tin cậy để tăng cường bảo mật, hoặc ghi đè cổng mặc định nếu máy chủ SFTP của tôi sử dụng một cổng không tiêu chuẩn.

![Hình ảnh bước 4](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/09/30/configure-SFTP-connector-1.png)

Tiếp theo, tôi kiểm tra kết nối. Trên menu **Actions**, tôi chọn **Test connection** để xác nhận rằng connector có thể kết nối đến máy chủ SFTP mục tiêu.

![Hình ảnh bước 5](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/10/03/test-SFTP-connector-2.png)

Cuối cùng, sau khi trạng thái của connector là **ACTIVE**, tôi có thể bắt đầu các thao tác tệp với máy chủ SFTP từ xa của mình thông qua mã bằng cách gọi các API của Transfer Family như <span style="color: red; background-color: #f0f0f0">StartDirectoryListing</span>, <span style="color: red; background-color: #f0f0f0">StartFileTransfer</span>, <span style="color: red; background-color: #f0f0f0">StartRemoteDelete</span>, hoặc <span style="color: red; background-color: #f0f0f0">StartRemoteMove</span>. Tất cả lưu lượng đều được định tuyến thông qua VPC của tôi bằng cách sử dụng các tài nguyên đã cấu hình như NAT gateway, AWS Direct Connect, hoặc kết nối VPN, cùng với địa chỉ IP và các biện pháp kiểm soát bảo mật của tôi.

Để biết đầy đủ các tùy chọn và quy trình nâng cao, hãy tham khảo [AWS Transfer Family document](https://docs.aws.amazon.com/transfer/).


---

## Hiện đã có sẵn

Các SFTP connector với kết nối dựa trên VPC hiện đã có sẵn tại 21 [AWS Regions](https://docs.aws.amazon.com/glossary/latest/reference/glos-chap.html#region). Hãy kiểm tra [AWS Services by Region](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/?utm_source=chatgpt.com/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) để biết các vùng AWS được hỗ trợ mới nhất. Giờ đây, bạn có thể kết nối an toàn các AWS Transfer Family SFTP connector tới các máy chủ riêng tư, on-premises hoặc máy chủ hướng internet, bằng cách sử dụng các tài nguyên VPC của riêng bạn như NAT gateway, Elastic IP, và network firewall.

— [Betty](https://www.linkedin.com/in/zhengyubin714/)
