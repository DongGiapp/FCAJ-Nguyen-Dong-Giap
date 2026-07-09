---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Deploy dự án lên AWS thật lần đầu — em hơi hồi hộp vì sợ tốn tiền.
* Viết test và xem log trên CloudWatch.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học *Monitoring Serverless Applications* <br> - Xem CloudWatch metrics của Lambda (Duration, Errors) | 15/06/2026 | 15/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Viết unit test trong `tests/`, chạy `npm test` <br> - Dùng aws-sdk-client-mock — em học cách mock DynamoDB | 16/06/2026 | 16/06/2026 | <https://github.com/cheduc1234/duan> |
| 4 | - Chạy `sam build` và `sam deploy --guided` <br> - Lưu API endpoint từ Outputs | 17/06/2026 | 17/06/2026 | |
| 5 | - Gọi API trên AWS thật bằng curl/Postman <br> - Tạo workflow test, kiểm tra trên DynamoDB Console | 18/06/2026 | 18/06/2026 | |
| 6 | - Xem log Lambda trên CloudWatch khi có lỗi 500 <br> - Sửa bug nhỏ và deploy lại | 19/06/2026 | 19/06/2026 | |

### Kết quả đạt được tuần 9:

* Deploy thành công lên AWS — em chụp màn hình CloudFormation stack COMPLETE.
* API chạy trên URL thật, không chỉ local nữa.
* Unit test pass — em yên tâm hơn khi sửa code.
* Biết xem CloudWatch Logs để tìm lỗi thay vì đoán mò.
* Chi phí thấp, trong budget đã set tuần 1.
