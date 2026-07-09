---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Tích hợp Cognito — phần em thấy khó nhất vì liên quan auth và JWT.
* API không còn public, cần token mới gọi được.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học *Cross-Domain Authentication with Amazon Cognito* <br> - Tạo User Pool trên Console, thử đăng ký user test | 08/06/2026 | 08/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Học *User Authentication with Amazon Cognito* (Book Store series) <br> - Hiểu flow: login → nhận JWT → gửi kèm header Authorization | 09/06/2026 | 09/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Thêm Cognito Authorizer vào API Gateway trong `template.yaml` <br> - Em đọc lại 3 lần docs SAM về Auth | 10/06/2026 | 10/06/2026 | |
| 5 | - Sửa Lambda: lấy `userId` từ `event.requestContext.authorizer.claims.sub` <br> - Test gọi API không token → nhận 401 như mong đợi | 11/06/2026 | 11/06/2026 | |
| 6 | - Test gọi API có token — em dùng Postman <br> - Fix lỗi CORS khi gọi từ frontend local | 12/06/2026 | 12/06/2026 | |

### Kết quả đạt được tuần 8:

* Tạo được Cognito User Pool và đăng nhập thử thành công.
* API yêu cầu JWT — em hiểu vì sao không nên để API public.
* Lambda biết user nào đang gọi nhờ `sub` trong token.
* Cognito vẫn còn nhiều cấu hình em chưa nắm hết, nhưng flow cơ bản đã chạy.
* Tuần này em hỏi mentor và bạn nhóm nhiều nhất.
