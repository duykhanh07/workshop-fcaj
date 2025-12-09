---
title : "Dọn dẹp tài nguyên"
#date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

Sau khi hoàn thành workshop, điều cực kỳ quan trọng là bạn phải xóa các tài nguyên đã tạo để tránh bị AWS tính phí "oan" vào cuối tháng.

Quy trình dọn dẹp gồm 2 bước chính.

## Bước 1: Xóa Frontend Bucket (Bắt buộc) 🗑️

Lệnh `sam delete` sẽ **thất bại** khi cố xóa S3 Bucket nếu bucket đó vẫn còn chứa file (code frontend bạn đã upload). Do đó, chúng ta cần xóa bucket này thủ công trước.

1.  Mở Terminal.
2.  Chạy lệnh sau để xóa sạch file và xóa luôn bucket (thay tên bucket của bạn vào):

```bash
aws s3 rb s3://career-coach-frontend-123456789 --force
```

Giải thích:

* rb: Remove Bucket
* --force: Ép buộc xóa kể cả khi bucket còn chứa file (nguy hiểm nhưng tiện lợi cho workshop).

## Bước 2: Xóa Stack Backend (SAM Delete)

Sau khi Bucket đã xóa, giờ chúng ta có thể yên tâm xóa toàn bộ hạ tầng còn lại (Lambda, DynamoDB, API Gateway, Cognito...).

Tại thư mục gốc của dự án (nơi có file template.yaml), chạy lệnh:

```bash
sam delete
```

Xác nhận hành động:

* Are you sure you want to delete the stack [career-coach-stack]? -> Nhập y (Yes).
* Are you sure you want to delete the folder [career-coach-stack]...? -> Nhập y.

Quá trình này sẽ mất khoảng 2-3 phút. SAM sẽ dọn dẹp sạch sẽ mọi thứ được định nghĩa trong template.yaml.