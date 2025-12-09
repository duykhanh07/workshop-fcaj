---
title : "Các bước chuẩn bị"
#date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

Để xây dựng và triển khai ứng dụng **AI Career Coach**, chúng ta cần thiết lập một môi trường phát triển mạnh mẽ. Hãy đảm bảo bạn cài đặt đầy đủ các công cụ dưới đây trước khi bước vào phần code.

{{% notice note %}}
ℹ️ **Lưu ý quan trọng:** Dự án này yêu cầu bạn phải có tài khoản AWS (Amazon Web Services). Nếu chưa có, hãy đăng ký một tài khoản [tại đây](https://aws.amazon.com/free/). Tài khoản Free Tier là đủ để thực hiện workshop này.
{{% /notice %}}

---

## 1. Cài đặt Runtime (Ngôn ngữ lập trình)

### A. Java 17 (JDK)
Backend của chúng ta sử dụng Spring Boot 3, yêu cầu tối thiểu là Java 17.

1.  **Tải xuống:** Truy cập [Oracle JDK 17](https://www.oracle.com/java/technologies/downloads/#java17) hoặc [Amazon Corretto 17](https://aws.amazon.com/corretto/).
2.  **Cài đặt:** Chạy file cài đặt theo hệ điều hành của bạn (Windows/Mac/Linux).
3.  **Kiểm tra:** Mở Terminal (hoặc CMD/PowerShell) và gõ lệnh:

```bash
java -version
```
Kết quả mong đợi: Bạn sẽ thấy phiên bản hiển thị là 17.x.x.

### B. Node.js & npm
Frontend React (Vite) cần môi trường Node.js.

1. **Tải xuống:** Truy cập Node.js và tải bản LTS (Long Term Support) (ví dụ: v18 hoặc v20).
2. **Kiểm tra:** Mở Terminal (hoặc CMD/PowerShell) và gõ lệnh:
```bash
node -v
npm -v
```

---

## 2. Cài đặt bộ công cụ AWS
Đây là những công cụ giúp chúng ta giao tiếp với AWS Cloud và triển khai ứng dụng Serverless.

### A. AWS CLI (Command Line Interface)
Công cụ dòng lệnh chính thức của AWS.

* Windows: Tải file MSI installer [tại đây.](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* MacOS: Tải file PKG [tại đây.](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* Linux: Xem hướng dẫn chi tiết [tại đây.](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Kiểm tra cài đặt:

```bash
aws --version
```
### B. AWS SAM CLI (Serverless Application Model)
Công cụ giúp xây dựng, test và deploy ứng dụng Serverless (Lambda, DynamoDB...) dễ dàng hơn.

* Hướng dẫn cài đặt: [Xem tài liệu chính thức của AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)
* Kiểm tra cài đặt:
```
sam --version
```

---

## 3. Cấu hình AWS Credentials
Sau khi cài AWS CLI, bạn cần kết nối nó với tài khoản AWS của mình thông qua Access Key.

**Bước 1: Tạo Access Key**
* Đăng nhập vào AWS Console.
* Tìm dịch vụ IAM -> Chọn Users -> Chọn User của bạn (hoặc tạo mới).
* Vào tab Security credentials -> Bấm Create access key.
* Chọn Use case: Command Line Interface (CLI).
* Tải file .csv chứa Access Key ID và Secret Access Key về máy.

**Bước 2: Cấu hình trên máy**

Mở Terminal và chạy lệnh:

```
aws configure
```
Nhập lần lượt các thông tin:

* AWS Access Key ID: [Paste Key ID của bạn]
* AWS Secret Access Key: [Paste Secret Key của bạn]
* Default region name: ap-southeast-1 (Singapore) (Hoặc region gần bạn nhất)
* Default output format: json

---

## 4. Kích hoạt Model AI (Quan trọng)
Dự án sử dụng mô hình Claude 3 Haiku của Anthropic thông qua Amazon Bedrock. Mặc định, quyền truy cập này bị tắt.

1. Đăng nhập AWS Console, tìm dịch vụ Amazon Bedrock.
2. Đảm bảo bạn đang ở đúng Region đã cấu hình (ví dụ: Singapore).
3. Ở menu bên trái, chọn Model catalog.
4. Tìm nhà cung cấp Anthropic.
{{% notice note  %}}
ℹ️ **Lưu ý:**  Bạn cần nộp form "Use Case Details" trước. Điền thông tin là "Personal Project" cho mục đích học tập
{{% /notice %}}
5. Tick chọn Claude 3 Haiku.
6. Nếu **Open in playground** có màu cam thì đã sử dụng được.

![claude 3](/images/5-Workshop/5.2-Prerequisite/claude-3.png)

---

## 5. IDE & Extensions (Khuyên dùng)
Để code hiệu quả nhất, bạn nên sử dụng Visual Studio Code (VS Code) hoặc IntelliJ IDEA.

Nếu dùng VS Code, hãy cài đặt các Extensions sau:

* Extension Pack for Java: Hỗ trợ code Java/Spring Boot.
* ES7+ React/Redux/React-Native snippets: Hỗ trợ code React nhanh.
* AWS Toolkit: Quản lý tài nguyên AWS ngay trong VS Code.
* YAML: Hỗ trợ highlight cú pháp cho file template.yaml.

---

✅ Checklist kiểm tra
Trước khi sang bài tiếp theo, hãy chắc chắn bạn đã tích đủ các mục sau:
* Đã cài Java 17 (`java -version`).
* Đã cài Node.js (`node -v`).
* Đã cài AWS SAM CLI (`sam --version`).
* Đã chạy aws configure thành công.
* Đã thấy Open in playground cho Claude 3 Haiku trên AWS Bedrock.
