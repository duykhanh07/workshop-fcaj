---
title: "Bản đề xuất"
#date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# AI Career Coach   

### 1. Tóm tắt điều hành  
Nền tảng AI Career Coach được thiết kế dành cho người tìm việc và sinh viên mới ra trường nhằm tối ưu hóa quy trình ứng tuyển và nâng cao cơ hội việc làm. Hệ thống hoạt động như một trợ lý ảo thông minh, hỗ trợ từ việc xây dựng hồ sơ (CV), viết thư xin việc (Cover Letter) đến luyện tập phỏng vấn. Nền tảng tận dụng sức mạnh của AWS Serverless để đảm bảo khả năng mở rộng tự động và Amazon Bedrock (Generative AI) để cá nhân hóa nội dung sâu sắc. Hệ thống đảm bảo bảo mật thông tin cá nhân người dùng thông qua Amazon Cognito. 

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Người tìm việc hiện nay tốn quá nhiều thời gian để chỉnh sửa CV thủ công cho từng vị trí ứng tuyển. Việc viết thư xin việc thường mang tính rập khuôn, thiếu điểm nhấn. Ngoài ra, cung cấp cho ứng viên môi trường để luyện tập kiến thức chuyên môn và phân tích thông tin chi tiết xu hướng về nghề nghiệp bản thân (Industry Insights) một cách nhanh nhất.  

*Giải pháp*  
Nền tảng sử dụng kiến trúc AWS Serverless toàn diện để giải quyết các vấn đề trên:
* Amazon S3 & CloudFront: Lưu trữ và phân phối giao diện web (SPA) với tốc độ cao.
* Amazon Cognito: Quản lý định danh và xác thực người dùng an toàn.
* Amazon API Gateway & AWS Lambda (Java Spring Cloud Function): Xử lý logic nghiệp vụ theo kiến trúc Microservices (User, Resume, Cover Letter, Interview).
* Amazon DynamoDB: Lưu trữ dữ liệu hồ sơ, bài đánh giá và thông tin ngành nghề với độ trễ thấp.
* Amazon Bedrock: Trái tim của hệ thống, cung cấp khả năng nâng cấp CV, soạn thảo Cover Letter và sinh câu hỏi phỏng vấn theo ngữ cảnh thực tế.

Nền tảng tập trung sâu vào tính năng "Cá nhân hóa bằng AI", người dùng không chỉ lưu trữ CV mà còn được AI hỗ trợ. Các tính năng chính bao gồm: nâng cấp CV, viết thư xin việc dựa trên JD bằng AI, làm quiz ôn tập kiến thức, báo cáo xu hướng ngành nghề.  

*Lợi ích và hoàn vốn đầu tư (ROI)*  
Giảm nhiều thời gian viết cover letter và chỉnh sửa CV. Tận dụng AWS Free Tier cho Lambda, DynamoDB và Cognito, chi phí chủ yếu đến từ việc gọi API Bedrock. Ước tính chi phí vận hành hàng tháng khoảng 3-5 USD cho nhu cầu sử dụng cá nhân hoặc quy mô nhỏ (dưới 1000 request AI/tháng). Giá trị lớn nhất không nằm ở tiền mặt trực tiếp mà ở việc rút ngắn viết cover letter và chỉnh sửa CV. Không có chi phí phần cứng ban đầu. 

### 3. Kiến trúc giải pháp  
Nền tảng áp dụng kiến trúc AWS Serverless hoàn toàn (Full Serverless) để tối ưu hóa khả năng mở rộng và chi phí bảo trì. Giao diện người dùng được phân phối toàn cầu thông qua Amazon CloudFront và S3. Hệ thống backend sử dụng kiến trúc Microservices với AWS Lambda (Java Spring Cloud Function) để xử lý nghiệp vụ và Amazon Bedrock để tích hợp trí tuệ nhân tạo. Dữ liệu hồ sơ và đánh giá được quản lý tập trung bởi Amazon DynamoDB, đảm bảo hiệu năng cao và bảo mật.  

![AI Career Coach Architecture](/images/2-Proposal/AI_Career_Coach_Architecture_v1.png)

*Dịch vụ AWS sử dụng*
- *Amazon CloudFront & S3*: Lưu trữ và phân phối giao diện Web với độ trễ thấp.
- *Amazon Cognito*: Quản lý định danh, đăng ký/đăng nhập và xác thực người dùng an toàn.
- *Amazon API Gateway*: Cổng REST API quản lý traffic và định tuyến yêu cầu đến đúng Lambda Service.
- *AWS Lambda*: Xử lý logic nghiệp vụ (5 services: User, Resume, Cover Letter, Interview, Industry) trên môi trường Java Spring.
- *Amazon DynamoDB*: Cơ sở dữ liệu NoSQL lưu trữ thông tin người dùng, resume và lịch sử đánh giá (1 bảng).
- *Amazon Bedrock*: Cung cấp các mô hình nền tảng (Claude 3 Haiku/Sonnet) để phân tích và tạo nội dung. 

*Thiết kế thành phần*
- *Giao diện người dùng (Frontend)*: Ứng dụng Next.js Single Page Application (SPA) tương tác với backend thông qua RESTful API.
- *Quản lý truy cập*: Amazon Cognito (User Pool) xác thực người dùng và cấp JWT Token cho các API request.
- *Xử lý trung tâm*: AWS Lambda thực thi các hàm Spring Cloud Function, kết nối với Bedrock để xử lý các tác vụ thông minh (tạo quiz, sửa CV).
- *Lớp dữ liệu*: DynamoDB được tổ chức theo mô hình "một bảng" (single-table) để tối ưu hoá việc tổ chức dữ liệu và cho phép truy vấn hiệu quả.
- *Trí tuệ nhân tạo*: Amazon Bedrock nhận ngữ cảnh từ Lambda, thực hiện suy luận (Inference) và trả về kết quả tư vấn hoặc nội dung văn bản. 

### 4. Triển khai kỹ thuật
*Các giai đoạn triển khai*
Dự án được chia thành 2 giai đoạn lớn, tập trung vào việc thiết kế kiến trúc Serverless tối ưu cho Java và tích hợp Generative AI:
1. *Nghiên cứu, Thiết kế và Đánh giá khả thi*: Thiết kế sơ đồ luồng dữ liệu AWS Serverless, xác định chiến lược chia tách Microservices với Spring Cloud Function. Lựa chọn model AI phù hợp (Claude 3 Haiku) dựa trên benchmark về độ chính xác và tốc độ. Sử dụng AWS Pricing Calculator để ước tính chi phí Lambda (thời gian chạy Java), DynamoDB (Read/Write capacity) và quan trọng nhất là chi phí Token của Amazon Bedrock. Thiết lập ngân sách dự kiến (AWS Budget) để đảm bảo dự án nằm trong giới hạn Free Tier hoặc chi phí thấp nhất. (Tháng thứ 2)
2. *Phát triển, Tối ưu hóa và Vận hành*: Xây dựng các hàm Lambda bằng Java (Spring Boot 3), cấu hình DynamoDB Multi-table, và viết Prompt Engineering cho Bedrock để tối ưu kết quả đầu ra cho CV/Interview. Tích hợp Frontend (Next.js) với API Gateway và Cognito, thực hiện kiểm thử tích hợp (Integration Test) toàn hệ thống trước khi Go-live.(Tháng thứ 3)

*Yêu cầu kỹ thuật*
- *Backend Services (Java Serverless):* Thành thạo Java 17/21 và Spring Cloud Function để viết code theo mô hình hướng chức năng. Hiểu sâu về cơ chế AWS Lambda SnapStart để tối ưu hóa thời gian khởi động ứng dụng Java (giảm độ trễ từ hàng giây xuống mili-giây).
- *Generative AI & Data:* Kỹ năng Prompt Engineering nâng cao để điều khiển model Claude 3 trên Amazon Bedrock trả về định dạng JSON chuẩn xác. Thiết kế schema DynamoDB hiệu quả (Partition Key/Sort Key) cho các truy vấn lịch sử đánh giá và hồ sơ người dùng.
- *Infrastructure & Security:* Sử dụng Amazon Cognito để quản lý User Pool và xác thực JWT. Cấu hình Amazon API Gateway để map request và xử lý CORS cho Frontend. Triển khai Frontend (Next.js) lên Amazon S3 và phân phối qua CloudFront.

### 5. Lộ trình & Mốc triển khai   
- *Thực tập (Tháng 1–3)*:  
    - Tháng 1: Học AWS và nâng cấp phần cứng.  
    - Tháng 2: Thiết kế và điều chỉnh kiến trúc.  
    - Tháng 3: Triển khai, kiểm thử, đưa vào sử dụng.  
- *Sau triển khai*: Nghiên cứu thêm để mở rộng thêm nhiều chức năng.  

### 6. Ước tính ngân sách  
Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=16a1acd5f6d2fe1cf2414547f30fdfb0504f8c0d)    

*Chi phí hạ tầng (Khu vực: Asia Pacific - Singapore)*
- Amazon Bedrock: 2.70 USD/tháng (Mô hình AI, xử lý ~1,000 token input/output mỗi request).
- Amazon CloudFront: 0.85 USD/tháng (Truyền tải dữ liệu ra internet 10 GB).
- Amazon DynamoDB: 0.28 USD/tháng (Lưu trữ 1 GB, chế độ Standard).
- AWS Lambda: 0.13 USD/tháng (4,000 request, 512 MB bộ nhớ tạm).
- Amazon S3: 0.05 USD/tháng (Lưu trữ 1 GB, 4,000 request PUT/GET).
*Tổng cộng*: 4.01 USD/tháng, tương đương 48.12 USD/12 tháng.

### 7. Đánh giá rủi ro
*Ma trận rủi ro*
- Ảo giác AI (Hallucination): Ảnh hưởng Cao, Xác suất Trung bình. (AI đưa ra lời khuyên sai lệch hoặc bịa đặt thông tin trong CV).
- Chi phí vượt ngân sách: Ảnh hưởng Trung bình, Xác suất Thấp. (Do phí token của Bedrock nếu lượng request tăng đột biến).
- Độ trễ hệ thống (Cold Start): Ảnh hưởng Trung bình, Xác suất Thấp. (Do đặc thù của Java Lambda function khi khởi động).

*Chiến lược giảm thiểu*
- AI chính xác: Tối ưu hóa Prompt Engineering với ngữ cảnh cụ thể, thiết lập tham số `Temperature` thấp (0.1 - 0.2) để giảm tính ngẫu nhiên. Thêm cảnh báo miễn trừ trách nhiệm (Disclaimer) cho người dùng.
- Kiểm soát chi phí: Thiết lập AWS Budgets để cảnh báo khi chi phí đạt 80% ngưỡng cho phép.
- Hiệu năng: Kích hoạt tính năng AWS Lambda SnapStart để giảm thời gian khởi động Java từ hàng giây xuống mili-giây.

*Kế hoạch dự phòng*
- Sự cố dịch vụ AI: Thiết kế hệ thống để "fail gracefully" (thông báo lỗi thân thiện) hoặc trả về các template mẫu có sẵn nếu Amazon Bedrock bị gián đoạn.
- Khôi phục dữ liệu: Kích hoạt tính năng Point-in-Time Recovery (PITR) của DynamoDB để khôi phục dữ liệu user trong trường hợp thao tác xóa nhầm hoặc lỗi ứng dụng.

### 8. Kết quả kỳ vọng
*Cải tiến kỹ thuật:* Tự động hóa 90% quy trình chuẩn bị hồ sơ ứng tuyển (từ sửa CV đến viết thư xin việc), thay thế hoàn toàn việc soạn thảo thủ công tốn thời gian.  Hệ thống đạt độ trễ thấp (mili-giây) nhờ tối ưu hóa Java SnapStart, có khả năng chịu tải cao mà không cần can thiệp hạ tầng.
*Giá trị dài hạn:* Xây dựng thành công bộ khung kiến trúc mẫu cho các ứng dụng Java Serverless kết hợp GenAI trên AWS, có thể tái sử dụng cho các dự án doanh nghiệp. Tạo ra môi trường thực tế để thử nghiệm và tinh chỉnh các kỹ thuật Prompt Engineering phức tạp, phục vụ cho hướng phát triển chuyên sâu về AI Engineer.