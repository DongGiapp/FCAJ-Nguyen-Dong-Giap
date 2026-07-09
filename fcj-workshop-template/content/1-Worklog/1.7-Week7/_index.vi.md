---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Code các Lambda function CRUD — tuần em "vật lộn" với code nhiều nhất.
* Áp dụng DynamoDB single-table design vào thực tế.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học *Building Serverless CRUD with Lambda and DynamoDB* <br> - Làm theo lab, so sánh với code nhóm | 01/06/2026 | 01/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Code `createWorkflow.ts`: nhận JSON, ghi DynamoDB <br> - Em quên parse body từ API Gateway, debug mất nửa buổi | 02/06/2026 | 02/06/2026 | <https://github.com/cheduc1234/duan> |
| 4 | - Code `getTodos.ts`: query theo `PK = USER#<userId>` <br> - Tìm hiểu DocumentClient của AWS SDK v3 | 03/06/2026 | 03/06/2026 | |
| 5 | - Code `updateTodo.ts` và `deleteTodo.ts` <br> - Xử lý trả về status code 404 khi không tìm thấy item | 04/06/2026 | 04/06/2026 | |
| 6 | - Test thủ công bằng curl trên SAM local <br> - Push code lên GitHub, tạo PR cho nhóm review | 05/06/2026 | 05/06/2026 | |

### Kết quả đạt được tuần 7:

* Hoàn thành 4 Lambda CRUD cơ bản — em tự tin hơn với TypeScript.
* Hiểu cách DynamoDB lưu item với `pk` và `sk` thay vì bảng SQL quen thuộc.
* Biết đọc log Lambda để debug (console.log + CloudWatch sau này).
* Gặp nhiều lỗi nhỏ (parse body, CORS) nhưng mỗi lỗi em học được thêm.
* API chạy được local — milestone quan trọng với em.
