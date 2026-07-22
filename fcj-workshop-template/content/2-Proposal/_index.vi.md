---
title: "Kiến trúc giải pháp"
date: 2026-07-22
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Serverless Todo API — Kiến trúc giải pháp và mức độ sẵn sàng Production

## 1. Tóm tắt điều hành

| Hạng mục | Giá trị |
|---|---|
| Chủ dự án | Nguyễn Đông Giáp |
| Vị trí thực tập | Cloud Engineer Intern |
| Thời gian | 17/04–10/07/2026 |
| Loại workload | Serverless REST API |
| Region chính | Asia Pacific (Singapore), `ap-southeast-1` |

Dự án minh họa một Todo API cloud-native sử dụng Amazon API Gateway, AWS Lambda và Amazon DynamoDB. Workshop hiện tại chứng minh được luồng CRUD cơ bản. Tài liệu này phân tách rõ **baseline học tập đã thể hiện** và **kiến trúc production mục tiêu** cần có để vận hành an toàn, tin cậy.

{{% notice warning %}}
Repository hiện là sản phẩm workshop và tài liệu thực tập. Không nên gọi hệ thống là production-ready cho đến khi có đủ bằng chứng cho các cổng kiểm duyệt ở Mục 11.
{{% /notice %}}

## 2. Bài toán và phạm vi

Workload cung cấp API để người dùng đã xác thực tạo, xem, cập nhật và xóa công việc của chính họ. Serverless phù hợp vì lưu lượng dự kiến không liên tục, nhóm không cần quản lý máy chủ và từng dịch vụ managed có thể mở rộng độc lập.

### Trong phạm vi

- Các thao tác CRUD RESTful cho Todo.
- Xác thực và cô lập dữ liệu theo người dùng trong kiến trúc mục tiêu.
- Triển khai hạ tầng tự động và tách biệt môi trường.
- Log, metric, alarm, backup, kiểm soát chi phí và tài liệu vận hành.

### Ngoài phạm vi

- Giao diện web/mobile hoàn chỉnh.
- Active-active đa Region.
- Cộng tác thời gian thực và notification.
- Dữ liệu cá nhân nhạy cảm hoặc workload chịu quy định chuyên ngành.

## 3. Hiện trạng và trạng thái mục tiêu

| Năng lực | Baseline workshop có bằng chứng | Mục tiêu production |
|---|---|---|
| API | API Gateway REST và bốn route CRUD | API có version, validation, throttling, access log, CORS có kiểm soát |
| Danh tính | Đã học Cognito nhưng chưa có bằng chứng triển khai trong repository | Cognito User Pool authorizer xác thực JWT |
| Compute | Bốn ví dụ Lambda Python | Function có version/alias, bounded concurrency, structured log và tracing |
| Dữ liệu | DynamoDB chỉ dùng `todoId`; thao tác list dùng `Scan` | Composite key theo user, dùng `Query`, bật PITR |
| Quyền | Có nêu nguyên tắc least privilege | Execution role riêng theo route và được review policy |
| Triển khai | Hướng dẫn thao tác Console | AWS SAM/CloudFormation trong source control, CI/CD và rollback |
| Vận hành | Hướng dẫn xem log/dashboard | SLO dashboard, alarm, runbook, synthetic test và người chịu trách nhiệm |

Việc phân biệt này giúp báo cáo không tuyên bố vượt quá bằng chứng và cho reviewer thấy lộ trình cải tiến có thể kiểm tra.

## 4. Kiến trúc mục tiêu

![Sơ đồ kiến trúc AWS Enterprise Multi-AZ với phân đoạn mạng zero-trust](/images/2-Proposal/aws-enterprise-reference-architecture.png)

*Hình 1 — Kiến trúc triển khai production tham chiếu: Route 53, CloudFront, WAF và Shield bảo vệ lớp biên; ALB phân phối lưu lượng đến ECS Fargate trên hai Availability Zone; Aurora và ElastiCache nằm trong các subnet dữ liệu cô lập; log, mã hóa, secrets và private endpoints được quản lý bằng các dịch vụ AWS.*

### Luồng request

1. Người dùng đăng nhập qua Cognito và nhận JWT có thời hạn ngắn.
2. Client gọi API Gateway qua HTTPS và gửi token.
3. API Gateway xác thực token, kiểm tra request, áp dụng throttling và định tuyến.
4. Lambda theo từng route kiểm tra input và lấy `userId` từ token đã xác thực, không tin trường do client tự gửi.
5. Lambda chỉ đọc/ghi partition của đúng người dùng trong DynamoDB.
6. API Gateway và Lambda gửi telemetry tới CloudWatch; alarm thông báo cho người phụ trách workload.

### Ranh giới tin cậy

- API tiếp xúc Internet là ranh giới không tin cậy; mọi request phải được xác thực, phân quyền, kiểm tra và giới hạn tốc độ.
- Lambda execution role là danh tính của workload, chỉ nhận action DynamoDB cần thiết cho từng function.
- Client không truy cập trực tiếp DynamoDB.
- Không lưu credential trong code hoặc tài liệu.

## 5. Thiết kế dữ liệu và API

### DynamoDB access patterns

Thiết kế production bắt đầu từ access pattern, không dựa vào thao tác quét toàn bảng.

| Access pattern | Điều kiện khóa |
|---|---|
| Tạo Todo cho một user | `PK = USER#<sub>`, `SK = TODO#<uuid>` |
| Liệt kê Todo của user | `Query PK = USER#<sub>` kèm phân trang |
| Lấy, sửa hoặc xóa một Todo | Khớp chính xác `PK` và `SK` |
| Lọc theo trạng thái ở quy mô lớn | Chỉ thêm sparse GSI khi số liệu chứng minh cần thiết |

Các thuộc tính đề xuất gồm `title`, `description`, `status`, `createdAt`, `updatedAt` và `version`. Conditional expression ngăn cập nhật/xóa item không tồn tại và hỗ trợ optimistic locking. API list phải giới hạn page size và dùng continuation token không để lộ cấu trúc nội bộ.

### Hợp đồng API

| Method và resource | Thành công | Kiểm tra chính |
|---|---:|---|
| `POST /v1/todos` | `201` | Title không rỗng, status hợp lệ, giới hạn payload |
| `GET /v1/todos` | `200` | `limit` có giới hạn, continuation token hợp lệ |
| `GET /v1/todos/{todoId}` | `200` | ID hợp lệ và đúng chủ sở hữu |
| `PUT /v1/todos/{todoId}` | `200` | Chỉ trường được phép sửa và đúng chủ sở hữu |
| `DELETE /v1/todos/{todoId}` | `204` | Đúng chủ sở hữu và item tồn tại |

Lỗi dùng body nhất quán gồm `code`, `message` và `requestId`. Không trả internal exception hoặc stack trace cho client.

## 6. Đánh giá AWS Well-Architected

| Trụ cột | Quyết định thiết kế và bằng chứng bắt buộc |
|---|---|
| **Operational Excellence** | Infrastructure as Code, tham số `dev`/`prod` riêng, test tự động, JSON log, dashboard, alarm, deployment/incident runbook |
| **Security** | Cognito JWT authorizer, IAM least privilege, TLS, mã hóa DynamoDB, CORS có kiểm soát, không để secret trong code, CloudTrail cho audit cấp tài khoản |
| **Reliability** | Dịch vụ Regional managed, xem xét retry theo loại invocation, conditional write, DynamoDB PITR, quy trình rollback, kiểm thử quota và failure path |
| **Performance Efficiency** | DynamoDB `Query` thay `Scan`, pagination, tuning Lambda bằng số liệu, theo dõi latency và throttling |
| **Cost Optimization** | On-demand capacity cho tải thấp khó dự đoán, log retention, budget/anomaly alert, cost-allocation tag, xóa môi trường không dùng |
| **Sustainability** | Managed serverless, tối ưu memory/duration Lambda, thời gian lưu dữ liệu phù hợp, loại bỏ tài nguyên và xử lý trùng lặp |

Đánh giá dựa trên AWS Well-Architected Framework và Serverless Applications Lens. Sáu trụ cột là khung ra quyết định, không phải nhãn chứng nhận.

## 7. Security baseline

- Chặn truy cập chưa xác thực tới toàn bộ Todo route.
- Dùng Cognito `sub` làm user ID ổn định và đưa định danh tenant vào mọi DynamoDB key.
- `CreateTodo` chỉ có `dynamodb:PutItem`; function đọc, cập nhật và xóa chỉ nhận action tương ứng.
- Giới hạn policy theo ARN của bảng production; tránh wildcard resource khi dịch vụ hỗ trợ scope chặt hơn.
- Kiểm tra schema, độ dài, enum, identifier và tham số phân trang.
- CORS chỉ cho phép origin của client production; không dùng `*` cùng credential.
- Cấu hình API Gateway throttling; cân nhắc AWS WAF theo threat model của API public.
- Che dữ liệu nhạy cảm trong log và cấu hình thời gian lưu CloudWatch Logs hữu hạn.
- Bật DynamoDB PITR cho production và kiểm thử restore.

## 8. Reliability và vận hành

### Service level objectives

Đây là mục tiêu phải đo, không phải kết quả đã đạt.

| Chỉ số | Mục tiêu ban đầu | Nguồn đo |
|---|---:|---|
| Request API thành công | ≥ 99,5% mỗi tháng | API Gateway 5xx và Lambda errors |
| Độ trễ API | p95 ≤ 500 ms | API Gateway integration latency |
| Recovery point objective | ≤ 24 giờ | Chính sách backup/PITR DynamoDB |
| Recovery time objective | ≤ 4 giờ | Bài thực hành restore và redeploy có tài liệu |

### Alarm tối thiểu

- Tỷ lệ API Gateway 5xx và p95 latency cao.
- Lambda errors, throttles, duration gần timeout và áp lực concurrency.
- DynamoDB throttled requests và system errors.
- AWS Budget threshold và Cost Anomaly Detection.

Mỗi alarm phải có owner, severity, hành động phản hồi và đường dẫn runbook. Chỉ có logging chưa đồng nghĩa với observability.

## 9. Chiến lược triển khai

1. Định nghĩa API Gateway, Cognito, Lambda, DynamoDB, IAM, log retention, alarm và output bằng AWS SAM/CloudFormation.
2. Chạy lint, unit test, template validation và security check trong CI.
3. Tự động deploy lên `dev`, sau đó chạy contract test và smoke test.
4. Yêu cầu phê duyệt trước khi deploy production.
5. Publish Lambda version và chuyển traffic qua alias; rollback khi alarm kích hoạt.
6. Lưu stack, commit SHA, bằng chứng test, endpoint và thời điểm triển khai.

Thao tác Console phù hợp để học, nhưng không phải cơ chế triển khai production.

## 10. Kế hoạch kiểm thử

| Nhóm test | Kiểm tra bắt buộc |
|---|---|
| Chức năng | CRUD happy path, payload sai, item không tồn tại, pagination |
| Bảo mật | Thiếu/hết hạn token, truy cập chéo user, input quá dài, CORS, review IAM |
| Độ tin cậy | Lambda lỗi, phản hồi throttling, xung đột conditional write, restore drill |
| Hiệu năng | Load profile xác định, p50/p95/p99, error rate, concurrency, consumed capacity |
| Triển khai | Clean deployment lặp lại được, smoke test, rollback, tách biệt môi trường |
| Chi phí | Ước tính có giả định, budget alarm, tag coverage, kiểm tra log retention |

Screenshot có thể hỗ trợ, nhưng kết quả test xuất ra file, template trong source control, CloudWatch metric và deployment log là bằng chứng mạnh hơn.

## 11. Cổng kiểm duyệt Production Readiness

Chỉ gọi workload là production-ready khi toàn bộ mục bắt buộc đã đạt:

- [ ] SAM/CloudFormation trong source control deploy được môi trường sạch.
- [ ] Cognito authorization và test cô lập chéo user đều pass.
- [ ] IAM role vượt qua least-privilege review.
- [ ] Functional test và negative test tự động pass trong CI.
- [ ] Có dashboard, alarm, log retention và incident runbook.
- [ ] DynamoDB PITR đã bật và có tài liệu restore drill.
- [ ] Load test đạt SLO dưới traffic profile đã công bố.
- [ ] Rollback đã test và lưu bằng chứng triển khai.
- [ ] Công bố endpoint/status production mà không lộ credential.
- [ ] Xác minh cost estimate, budget alarm, resource tag và trách nhiệm cleanup.

**Đánh giá hiện tại:** đã thể hiện baseline workshop; repository chưa cung cấp đủ bằng chứng production readiness.

## 12. Rủi ro và ưu tiên xử lý

| Ưu tiên | Rủi ro | Hậu quả | Hướng xử lý |
|---|---|---|---|
| P0 | API public chưa bắt buộc xác thực | Truy cập hoặc sửa dữ liệu trái phép | Thêm Cognito authorizer và ownership test |
| P0 | `Scan` trả toàn bộ Todo | Lộ dữ liệu, tăng chi phí và latency | Composite key theo user và `Query` có pagination |
| P1 | Cấu hình hạ tầng thủ công | Drift, không thể phục hồi lặp lại | Thêm SAM/CloudFormation và CI/CD |
| P1 | Chưa có bằng chứng backup/restore | Mất dữ liệu do xóa nhầm | Bật PITR và thực hiện restore drill |
| P1 | Chưa có alarm hoặc bằng chứng SLO | Không phát hiện lỗi kịp thời | Dashboard, alarm, owner và runbook |
| P2 | Tuyên bố latency/chi phí không có số đo | Báo cáo thiếu tin cậy | Chỉ báo cáo số liệu đã đo kèm giả định |

## 13. Tài liệu tham khảo

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [AWS Serverless Applications Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html)
- [RESTful microservices reference architecture](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/restful-microservices.html)
- [DynamoDB security best practices](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices-security-preventative.html)
- [DynamoDB partition-key design](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-design.html)
- [DynamoDB backup and point-in-time recovery](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Backup-and-Restore.html)
