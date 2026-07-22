---
title: "Phạm vi và kiến trúc workshop"
date: 2026-07-22
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Workshop Serverless Todo API

## Mục đích

Workshop minh họa request path tối thiểu của một serverless CRUD API: API Gateway nhận HTTP request, Lambda xử lý business logic và DynamoDB lưu kết quả. Đây là baseline học tập có thể triển khai, kiểm thử, quan sát và dọn dẹp trong một AWS account được kiểm soát.

## Baseline được triển khai

{{<mermaid align="center">}}
flowchart LR
    C["Postman hoặc API client"] -->|"HTTPS request"| A["Amazon API Gateway REST API"]
    A -->|"Định tuyến theo method và path"| L["Bốn AWS Lambda functions"]
    L -->|"CRUD operations"| D["Amazon DynamoDB todos table"]
    A -.->|"Access metrics"| W["Amazon CloudWatch"]
    L -.->|"Execution logs và metrics"| W
{{< /mermaid >}}

| Thành phần | Trách nhiệm trong workshop |
|---|---|
| API Gateway | Cung cấp `POST /todos`, `GET /todos`, `PUT /todos/{todoId}` và `DELETE /todos/{todoId}` |
| Lambda | Tách handler tạo, liệt kê, cập nhật và xóa |
| DynamoDB | Lưu Todo với `todoId` làm khóa của workshop |
| IAM | Cho phép Lambda truy cập bảng và xuất log |
| CloudWatch | Thu thập Lambda log và service metric để xử lý sự cố |

## Kết quả học tập

Sau workshop, người học phải có khả năng:

- Giải thích luồng request đồng bộ và trách nhiệm của từng dịch vụ.
- Tạo và kiểm thử bốn CRUD route.
- Kiểm tra phản hồi thành công và thất bại bằng Postman.
- Tìm Lambda/API metric trong CloudWatch.
- Giải thích vì sao IAM role phải giới hạn theo action và resource cần thiết.
- Xóa toàn bộ tài nguyên workshop và xác nhận cleanup.

## Definition of Done của workshop

- [ ] Mỗi endpoint trả đúng status code trong happy path.
- [ ] Request sai hoặc thiếu tạo phản hồi 4xx có kiểm soát.
- [ ] Lambda log có request ID nhưng không làm lộ credential.
- [ ] IAM policy được review trước khi test.
- [ ] Bằng chứng test ghi request, response, thời gian và kết quả.
- [ ] Bước xác minh cleanup chứng minh tài nguyên tính phí đã được xóa.

## Giới hạn production quan trọng

Workshop đang dùng thao tác Console thủ công và key đơn giản. Với trạng thái này, tài liệu **không** chứng minh production readiness. Trước khi đưa vào production, bắt buộc phải:

1. Bắt buộc Cognito JWT authorization và kiểm tra quyền sở hữu theo user.
2. Thay DynamoDB `Scan` bằng data model `Query` theo user có pagination.
3. Đưa toàn bộ resource và IAM policy vào AWS SAM/CloudFormation.
4. Thêm input schema, error handling an toàn, CORS có kiểm soát, throttling, alarm và log retention hữu hạn.
5. Bật DynamoDB point-in-time recovery; kiểm thử restore và rollback.
6. Chạy tự động security, negative, performance và deployment test.

Xem [Kiến trúc giải pháp và Production Readiness](../../2-Proposal/) để đọc target design, Well-Architected assessment, risk, SLO và acceptance gate.

## Trình tự workshop

1. Kiểm tra prerequisite, an toàn tài khoản, permission và budget control.
2. Tạo DynamoDB workshop table.
3. Tạo và test Lambda handler.
4. Cấu hình API Gateway route và kiểm tra API.
5. Xem CloudWatch telemetry và lưu bằng chứng.
6. Xóa tài nguyên và xác minh cleanup.

Giá trị của workshop không nằm ở số lượng dịch vụ, mà ở khả năng giải thích quyết định thiết kế, tái tạo triển khai, kiểm thử failure behavior và xác định thay đổi cần thiết cho production.
