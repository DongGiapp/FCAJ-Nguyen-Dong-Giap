---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Học Infrastructure as Code và bắt đầu viết `template.yaml` cho nhóm.
* Hiểu SAM khác CloudFormation thế nào (em tìm hiểu vì thấy cả hai trên Cloud Journey).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học *Infrastructure as Code with AWS CloudFormation* (phần khái niệm) <br> - Hiểu stack, template, resource | 25/05/2026 | 25/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Học *Deployment Automation with AWS SAM* (Book Store series) <br> - Làm theo lab tạo project SAM mẫu | 26/05/2026 | 26/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Chỉnh `template.yaml`: thêm DynamoDB table `todos-v2` <br> - Thêm API Gateway và event mapping cho Lambda | 27/05/2026 | 27/05/2026 | <https://github.com/cheduc1234/duan> |
| 5 | - Chạy `sam build` lần đầu — em bị lỗi esbuild, nhờ bạn fix giúp <br> - Ghi lại lỗi để lần sau tự xử lý | 28/05/2026 | 28/05/2026 | |
| 6 | - Chạy `sam local start-api` thử trên máy (cần Docker) <br> - Gọi thử endpoint GET `/workflows` | 29/05/2026 | 29/05/2026 | |

### Kết quả đạt được tuần 6:

* Biết SAM là extension của CloudFormation, viết IaC gọn hơn cho serverless.
* `template.yaml` có DynamoDB table và API Gateway cơ bản.
* `sam build` chạy được sau khi sửa lỗi.
* Test local qua SAM — em thấy hay vì không cần deploy lên AWS mỗi lần sửa nhỏ.
* Vẫn chưa hiểu hết mọi dòng trong template, nhưng biết chỗ nào cần sửa khi thêm function.
