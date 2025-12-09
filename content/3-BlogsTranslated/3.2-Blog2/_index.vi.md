---
title: "Blog 2"
#date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# Tổng hợp hàng tuần của AWS: Amazon Quick Suite, Amazon EC2, Amazon EKS và nhiều hơn nữa (ngày 13 tháng 10 năm 2025)

Tuần này tôi đã tham dự [buổi gặp mặt đầu tiên AWS AI in Practice của AWS User Group UK](https://www.meetup.com/awsuguk-ai-in-practice/). Phát triển phần mềm có hỗ trợ AI và các agent là trọng tâm của buổi tối hôm đó! Tuần tới tôi sẽ đến Ý để tham dự [Codemotion](https://conferences.codemotion.com/milan2025/agenda/) (Milan) và một [buổi gặp mặt AWS User Group](https://www.meetup.com/amazon-web-services-rome/events/311302816) (Rome). Các buổi chia sẻ của tôi tại đó sẽ nói về các AI agent và kỹ thuật ngữ cảnh. Tôi cũng rất háo hức được [trải nghiệm Amazon Quick Suite mới](https://aws.amazon.com/blogs/aws/reimagine-the-way-you-work-with-ai-agents-in-amazon-quick-suite/) — bộ công cụ mang khả năng nghiên cứu, phân tích kinh doanh và tự động hóa dựa trên AI vào một không gian làm việc duy nhất.

---

## Các ra mắt trong tuần trước

Dưới đây là những tính năng ra mắt đã thu hút sự chú ý của tôi trong tuần này:

- [Amazon Quick Suite](https://aws.amazon.com/quicksuite/) – Một đồng đội tự động mới giúp trả lời nhanh các câu hỏi của bạn trong công việc và chuyển những hiểu biết đó thành hành động cho bạn. [Đọc thêm trong bài đăng ra mắt của Esra](https://aws.amazon.com/blogs/aws/reimagine-the-way-you-work-with-ai-agents-in-amazon-quick-suite/).
- [Amazon EC2](https://aws.amazon.com/ec2/) – [Các phiên bản M8a](https://aws.amazon.com/blogs/aws/new-general-purpose-amazon-ec2-m8a-instances-are-now-available/) chạy bằng bộ xử lý AMD EPYC thế hệ thứ 5 (tên mã Turin) và [các phiên bản C8i và C8i-flex](https://aws.amazon.com/blogs/aws/introducing-new-compute-optimized-amazon-ec2-c8i-and-c8i-flex-instances/) chạy bằng bộ xử lý Intel Xeon 6 tùy chỉnh hiện đã có sẵn.
- [Amazon EKS](https://aws.amazon.com/eks/) – EKS và  [EKS Distro](https://aws.amazon.com/eks/eks-distro/) [hiện đã hỗ trợ Kubernetes phiên bản 1.34](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-eks-distro-kubernetes-version-1-34/) với nhiều cải tiến.
- [AWS IAM Identity Center](https://aws.amazon.com/iam/identity-center/) – Các khóa AWS Key Management Service hiện có thể được sử dụng [để mã hóa dữ liệu danh tính được lưu trữ trong các instance IAM Identity Center của tổ chức](https://aws.amazon.com/blogs/aws/aws-iam-identity-center-now-supports-customer-managed-kms-keys-for-encryption-at-rest/).
- [Amazon VPC Lattice](https://aws.amazon.com/vpc/lattice/) – Giờ đây bạn có thể cấu hình số lượng địa chỉ IPv4 được gán cho [elastic network interfaces (ENIs)](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-vpc-lattice-configurable-ip-resource-gateway/) của resource gateway. Các địa chỉ IPv4 này được dùng cho dịch địa chỉ mạng và xác định số lượng kết nối IPv4 đồng thời tối đa đến một tài nguyên.
- [Amazon Q Developer](https://aws.amazon.com/q/developer/) – Amazon Q Developer có thể giúp bạn [lấy thông tin về giá, khả năng và thuộc tính của các sản phẩm AWS](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-q-developer-understand-service-prices-estimate-workload-costs/), giúp bạn dễ dàng chọn đúng tài nguyên và ước tính chi phí khối lượng công việc bằng ngôn ngữ tự nhiên. [Thông tin chi tiết có trong bài viết trên blog](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-q-developer-understand-service-prices-estimate-workload-costs/).
- [Amazon RDS for Db2](https://aws.amazon.com/rds/db2/) – Giờ đây bạn có [thể thực hiện sao lưu cơ sở dữ liệu ở cấp độ gốc](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-rds-for-db2-native-database-backup/), cung cấp tính linh hoạt cao hơn trong quản lý và di chuyển cơ sở dữ liệu.
- [AWS Service Quotas](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) – Nhận thông báo về việc sử dụng hạn mức với [tự động quản lý hạn mức](https://aws.amazon.com/about-aws/whats-new/2025/10/automatic-quota-management-service-quotas/). Có thể cấu hình kênh thông báo ưa thích như email, SMS, hoặc Slack. Thông báo cũng có sẵn trong [AWS Health](https://docs.aws.amazon.com/health/latest/ug/what-is-aws-health.html), và bạn có thể đăng ký các sự kiện [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) liên quan cho quy trình tự động hóa.
- [Amazon Connect](https://aws.amazon.com/connect/) – Giờ đây bạn có thể [làm giàu dữ liệu hồ sơ một cách lập trình với các API hồ sơ mới](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-connect-cases-api-link-search/) để liên kết các hồ sơ liên quan, thêm các mục tùy chỉnh liên quan, và tìm kiếm trên chúng. Bạn cũng có thể [tùy chỉnh cách tính mức dịch vụ](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-connect-enables-service-level-calculation-configuration/) theo nhu cầu cụ thể. Các khả năng mới vừa được giới thiệu bao gồm [sao chép và chỉnh sửa hàng loạt cấu hình lịch trình của agent](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-connect-copy-bulk-edit-agent-scheduling/) và [thông báo về việc tuân thủ lịch trình của agent](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-connect-agent-adherence-notifications/).
- [AWS Client VPN](https://aws.amazon.com/vpn/) – Giờ đây [hỗ trợ MacOS Tahoe](https://aws.amazon.com/about-aws/whats-new/2025/10/aws-client-vpn-macos-tahoe/).

---

## Cập nhật bổ sung

Dưới đây là một số dự án, bài viết blog và tin tức khác mà tôi thấy thú vị:

- [Serverless ICYMI Q3 2025](https://aws.amazon.com/blogs/compute/serverless-icymi-q3-2025/) – Bản tóm tắt hàng quý về các tin tức serverless, trong trường hợp bạn đã bỏ lỡ.
- [Best practices for migrating from Apache Airflow 2.x to Apache Airflow 3.x on Amazon MWAA](https://aws.amazon.com/blogs/big-data/best-practices-for-migrating-from-apache-airflow-2-x-to-apache-airflow-3-x-on-amazon-mwaa/) – Hướng dẫn giúp bạn tận dụng các lợi ích của phiên bản mới.
- [Building self-managed RAG applications with Amazon EKS and Amazon S3 Vectors](https://aws.amazon.com/blogs/storage/building-self-managed-rag-applications-with-amazon-eks-and-amazon-s3-vectors/) – Kiến trúc tham chiếu để xây dựng và triển khai ứng dụng RAG tự quản lý bằng các công cụ mã nguồn mở như [Ray](https://docs.ray.io/en/latest/ray-overview/index.html), [Hugging Face](https://huggingface.co/) và [LangChain](https://www.langchain.com/).
- [BBVA: Building a multi-region, multi-country global Data and ML Platform at scale](https://aws.amazon.com/blogs/industries/part-1-bbva-building-a-multi-region-multi-country-global-data-and-ml-platform-at-scale/) – Chuỗi bài viết gồm sáu phần mô tả hành trình chuyển đổi toàn bộ hạ tầng phân tích dữ liệu của [BBVA](https://www.bbva.com/en/), với một trong những dự án di chuyển lên đám mây lớn và phức tạp nhất trong ngành ngân hàng.
- [Customizing text content moderation with Amazon Nova](https://aws.amazon.com/blogs/machine-learning/customizing-text-content-moderation-with-amazon-nova/) – Tinh chỉnh Amazon Nova cho các tác vụ kiểm duyệt nội dung văn bản phù hợp với yêu cầu của bạn, bằng cách sử dụng dữ liệu huấn luyện theo lĩnh vực và hướng dẫn kiểm duyệt riêng của tổ chức.

---

## Sự kiện AWS sắp tới

Hãy kiểm tra lịch của bạn để đăng ký tham gia các sự kiện sắp diễn ra này:

- [AWS AI Agent Global Hackathon](https://info.devpost.com/blog/aws-ai-agent-global-hackathon?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) – Cơ hội để bạn khám phá sâu ngăn xếp AI sinh (generative AI stack) mạnh mẽ của AWS và tạo ra điều gì đó thật ấn tượng. Từ ngày 8 tháng 9 đến 20 tháng 10, bạn có thể tạo AI agents bằng bộ dịch vụ AI của AWS, cạnh tranh cho giải thưởng hơn 45.000 USD cùng cơ hội tiếp cận thị trường độc quyền.
- [AWS Gen AI Lofts](https://aws.amazon.com/startups/lp/aws-gen-ai-lofts?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) – Bạn có thể tìm hiểu về các sản phẩm và dịch vụ AI của AWS thông qua các buổi học độc quyền, gặp gỡ các chuyên gia hàng đầu trong ngành và kết nối với nhà đầu tư cùng đồng nghiệp. Đăng ký tại thành phố gần bạn: [Paris](https://aws.amazon.com/startups/lp/aws-gen-ai-loft-paris?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) (ngày 7–21 tháng 10), [London](https://aws.amazon.com/startups/lp/aws-gen-ai-loft-london?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) (ngày 13–21 tháng 10), và [Tel Aviv](https://aws.amazon.com/startups/lp/aws-gen-ai-loft-tel-aviv?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) (ngày 11–19 tháng 11).
- [AWS Community Days](https://aws.amazon.com/events/community-day/) – Tham gia các hội nghị do cộng đồng dẫn dắt, với các buổi thảo luận kỹ thuật, workshop và phòng lab thực hành do những người dùng AWS chuyên nghiệp và các nhà lãnh đạo trong ngành trình bày: [Budapest](https://awscommunity.eu/) (ngày 16 tháng 10).

Tham gia [AWS Builder Center](https://builder.aws.com/?trk=e61dee65-4ce8-4738-84db-75305c9cd4fe&sc_channel=el) để học hỏi, xây dựng và kết nối với các nhà phát triển trong cộng đồng AWS. Tại đây, bạn có thể duyệt qua các [sự kiện trực tiếp sắp tới](https://aws.amazon.com/events/explore-aws-events/?trk=e61dee65-4ce8-4738-84db-75305c9cd4fe&sc_channel=el), [sự kiện dành cho lập trình viên](https://aws.amazon.com/developer/events/?trk=e61dee65-4ce8-4738-84db-75305c9cd4fe&sc_channel=el), và [sự kiện dành cho startup](https://aws.amazon.com/startups/events?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el).

Đó là tất cả cho tuần này. Hãy quay lại vào thứ Hai tới để xem [bản tổng hợp hàng tuần](https://aws.amazon.com/blogs/aws/tag/week-in-review/?trk=7c8639c6-87c6-47d6-9bd0-a5812eecb848&sc_channel=el) mới!
— [Danilo](https://x.com/danilop)
