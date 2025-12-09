---
title : "Deploy"
#date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

Chúng ta đã hoàn thành việc viết code (Dev) và thiết kế hạ tầng (Ops). Giờ là lúc kết hợp chúng lại (DevOps) để đưa ứng dụng ra thế giới thực.

Quy trình triển khai gồm 3 giai đoạn chính:
1.  **Backend Deployment:** Đẩy API và Database lên AWS.
2.  **Frontend Configuration:** Cập nhật thông tin kết nối (API URL, Cognito ID) vào code React.
3.  **Frontend Deployment:** Build và đẩy Static Web lên S3.

---

## Giai đoạn 1: Triển khai Backend (AWS SAM)

### Bước 1: Build mã nguồn Java

Trước khi deploy, chúng ta cần đóng gói mã nguồn Java thành file `.jar` có thể chạy được.

Mở Terminal tại thư mục gốc của dự án (nơi có file `pom.xml` cha hoặc thư mục `backend/`):

```bash
cd backend
mvn clean package -DskipTests
```

Giải thích:

* clean: Xóa các file build cũ.
* package: Đóng gói thành file JAR (nằm trong target/).
* -DskipTests: Bỏ qua chạy unit test để tiết kiệm thời gian cho workshop.

### Bước 2: Deploy lên AWS

Quay lại thư mục gốc (nơi chứa template.yaml) và chạy lệnh:

```bash
cd ..
sam deploy --guided
```

Làm theo hướng dẫn trên màn hình (chỉ cần Enter để chọn mặc định cho hầu hết các câu hỏi):

* Stack Name: career-coach-stack (hoặc tên tùy ý).
* AWS Region: ap-southeast-1 (Singapore).
* Confirm changes before deploy: Y.
* Allow SAM CLI IAM role creation: Y.
* Save arguments to configuration file: Y.

Quá trình này sẽ mất khoảng 5-7 phút để CloudFormation tạo Database, Lambda, API Gateway và CloudFront.

### Bước 3: Lưu lại thông tin Output

Sau khi deploy thành công, màn hình sẽ hiện ra bảng Outputs. Hãy copy lại các thông số sau vào Notepad, chúng ta sẽ cần chúng ngay lập tức:

* ApiEndpoint: URL của API Gateway (Ví dụ: https://xyz.execute-api...).
* CognitoUserPoolId: ID của User Pool.
* CognitoClientId: ID của App Client.
* S3BucketName: Tên Bucket để chứa web.
* WebsiteURL: Đường dẫn trang web (CloudFront).

## Giai đoạn 2: Cấu hình Frontend

Frontend cần biết phải gọi API nào và đăng nhập vào đâu.

### Bước 1: Cập nhật API Client

Mở file frontend/src/lib/api.js:

```javaScript

// Thay thế bằng ApiEndpoint bạn vừa copy
const BASE_URL = "https://xxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com"; 
```
### Bước 2: Cập nhật Auth Config

Mở file frontend/src/main.jsx:

```javaScript

Amplify.configure({
  Auth: {
    Cognito: {
      userPoolId: 'ap-southeast-1_XXXXXXX', // Paste CognitoUserPoolId
      userPoolClientId: 'xxxxxxxxxxxxxxxxx',  // Paste CognitoClientId
      loginWith: { email: true }
    }
  }
});
```
## Giai đoạn 3: Triển khai Frontend (S3 & CloudFront)

### Bước 1: Build dự án React (Vite)

Chuyển vào thư mục frontend và tiến hành build (biên dịch):

```bash
cd frontend
npm run build
```

Kết quả: Một thư mục tên là dist sẽ được tạo ra. Đây là phiên bản "tinh gọn" của web, đã được tối ưu hóa dung lượng.

### Bước 2: Đẩy code lên S3

Sử dụng AWS CLI để đồng bộ thư mục dist lên S3 Bucket (dùng tên bucket bạn đã lưu ở Giai đoạn 1):

```Bash
aws s3 sync dist s3://career-coach-frontend-123456789 --delete
``` 

Tham số --delete: Tự động xóa các file cũ trên S3 nếu trong thư mục dist không còn tồn tại (giúp dọn rác).

### Bước 3: Xóa Cache CloudFront (Invalidation) 

Đây là bước bắt buộc mỗi khi update code frontend. Nếu không làm bước này, người dùng vẫn sẽ thấy phiên bản web cũ do CloudFront lưu cache.

Lấy ID của CloudFront Distribution:

```bash
aws cloudfront list-distributions --query "DistributionList.Items[*].{ID:Id, Domain:DomainName}" --output table
```

Chạy lệnh xóa cache:

```bash
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths
```