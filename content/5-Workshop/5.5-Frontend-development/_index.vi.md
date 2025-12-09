---
title : "Phát triển Frontend"
#date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

Frontend của **AI Career Coach** không chỉ cần đẹp mà còn phải xử lý logic phức tạp như Markdown Editor, Quiz Game và gọi API bảo mật.

Xem source code đầy đủ Frontend [tại đây](https://github.com/duykhanh07/ai-career-coach/tree/main/frontend)

Dưới đây tôi chỉ trình bày những nội dung chính và những gì cần lưu ý khi xây dựng frontend

## 1. Khởi tạo Dự án (Tech Stack Setup)

Chúng ta sử dụng **Vite** để khởi tạo dự án React vì tốc độ build cực nhanh.

### Các thư viện chính:
* **Tailwind CSS:** Framework CSS ưu tiên tiện ích (utility-first).
* **Shadcn UI:** Bộ thư viện component đẹp mắt (Button, Input, Card, Accordion...) giúp chúng ta không phải code CSS từ con số 0.
* **AWS Amplify:** Thư viện giúp kết nối React với Cognito dễ dàng.
* **React Router Dom:** Để điều hướng giữa các trang.

```bash
npm create vite@latest frontend -- --template react
cd frontend
npm install
```

**Thư viện cần cài đặt**

```bash
npm install react-router-dom aws-amplify @aws-amplify/ui-react lucide-react react-hook-form zod @hookform/resolvers sonner @uiw/react-md-editor recharts html2pdf.js react-spinners date-fns clsx tailwind-merge class-variance-authority
```
## 2. Cấu hình Authentication với AWS Amplify

Thay vì tự viết trang Login/Register phức tạp, chúng ta sử dụng component <Authenticator> của Amplify. Nó tự động sinh ra giao diện đăng nhập/đăng ký đầy đủ tính năng.

Cấu hình trong main.jsx: Bạn cần lấy UserPoolId và ClientId từ phần Output của lệnh sam deploy ở bài trước để điền vào đây.

```javaScript

import { Amplify } from 'aws-amplify';

Amplify.configure({
  Auth: {
    Cognito: {
      userPoolId: 'ap-southeast-1_XXXXXXX', // <-- Thay bằng ID của bạn
      userPoolClientId: 'xxxxxxxxxxxxxxxxx',  // <-- Thay bằng Client ID của bạn
      loginWith: { email: true }
    }
  }
});
```
![login](/images/5-Workshop/5.5-Frontend-development/login.jpeg)

## 3. apiClient
Đây là phần quan trọng nhất khi tích hợp (Integration).

**Vấn đề gặp phải:**

Khi Backend Java trả về dữ liệu, đôi khi nó trả về một chuỗi JSON lồng trong một chuỗi JSON khác (Double Serialization).
* Ví dụ Backend trả: { statusCode: 200, body: "{\"pk\": \"USER#1\", ...}" }
* Frontend nếu chỉ response.json() sẽ nhận được chuỗi string trong body, không thể truy cập data.pk được.

Giải pháp: Wrapper `apiClient`

Chúng ta viết một hàm wrapper để:

1. Tự động lấy JWT Token từ session hiện tại.
2. Gắn Token vào Header Authorization.
3. Tự động parse JSON 2 lần nếu phát hiện dữ liệu bị lồng.

```javaScript

// src/lib/api.js
export const apiClient = async (endpoint, options = {}) => {
    // 1. Lấy Token
    const session = await fetchAuthSession();
    const token = session.tokens?.idToken?.toString();

    // 2. Gọi API với Token
    const response = await fetch(`${BASE_URL}${endpoint}`, {
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json',
      },
      ...options,
    });

    // 3. Xử lý "Double JSON"
    const data = await response.json();
    if (data.body && typeof data.body === 'string') {
        return JSON.parse(data.body); // <-- "Bóc vỏ" lần 2
    }
    return data;
};
```
## 4. Xây dựng các trang chức năng (Core Features)
### A. Dashboard (Bảng điều khiển)

Trang tổng quan hiển thị thống kê.

* Thách thức: Xử lý bất đồng bộ khi gọi API lấy lịch sử hoạt độn
* Giải pháp: Sử dụng `useEffect` để fetch dữ liệu và hiển thị `BarLoader` (loading spinner) trong lúc chờ.

![dashboard](/images/5-Workshop/5.5-Frontend-development/dashboard.png)

### B. Resume Builder (Trình tạo CV)

Đây là tính năng phức tạp nhất về mặt UI.

* Giao diện: Sử dụng Tabs để chuyển đổi giữa chế độ Edit Form và Markdown Preview.
* Form: Sử dụng react-hook-form và zod để validate dữ liệu đầu vào.
* AI Feature: Khi người dùng bấm "Improve with AI", Frontend gửi đoạn text thô lên API /resume/improve và điền lại kết quả vào ô input.
* Accordion UI: Sử dụng component Accordion của Shadcn để gom nhóm các mục Kinh nghiệm/Học vấn cho gọn gàng.

![resume builder form](/images/5-Workshop/5.5-Frontend-development/resume-builder-form.jpeg)

![resume builder markdown](/images/5-Workshop/5.5-Frontend-development/resume-builder-markdown.jpeg)

### C. Mock Interview (Phỏng vấn thử)

Trang trắc nghiệm kiến thức.

* Logic tính điểm: So sánh đáp án người dùng chọn với đáp án đúng từ Backend.
* Lưu ý kỹ thuật: Backend trả về đáp án đúng là ký tự "B", nhưng Frontend hiển thị là "B. Nội dung...".
* Code xử lý: userAnswer.split(".")[0].trim() === correctAnswer.

![quiz](/images/5-Workshop/5.5-Frontend-development/quiz.jpeg)

![quiz result](/images/5-Workshop/5.5-Frontend-development/quiz-result.jpeg)

### D. Cover Letter Generator (Tạo thư xin việc)

Đây là tính năng giúp người dùng tạo thư xin việc tự động dựa trên mô tả công việc (JD).

* **Luồng xử lý:**
    1.  Người dùng nhập: *Tên công ty*, *Vị trí ứng tuyển*, *Mô tả công việc (JD)*.
    2.  Frontend gửi `POST /cover-letters` lên Backend.
    3.  Backend gọi Claude 3 để viết thư và lưu vào DB.
    4.  Frontend nhận kết quả và hiển thị cho người dùng.

* **Lưu ý kỹ thuật:**
    * **Loading State:** Vì AI mất khoảng 5-10 giây để viết xong một lá thư, Frontend **bắt buộc** phải có trạng thái chờ (Loading Spinner hoặc Skeleton) để người dùng biết hệ thống đang xử lý, tránh việc họ bấm nút nhiều lần.
    * **Hiển thị kết quả:** Nội dung trả về là văn bản thuần (hoặc Markdown), cần hiển thị trong `textarea` hoặc `MDEditor` để người dùng có thể chỉnh sửa lại trước khi tải về.

![cover letter form](/images/5-Workshop/5.5-Frontend-development/cover-letter-form.jpeg)
