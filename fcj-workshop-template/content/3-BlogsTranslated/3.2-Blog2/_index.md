---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

**Người dịch:** Nguyễn Đông Giáp

# Tối ưu hóa chi phí vận hành backend qua kiến trúc serverless với AWS Lambda

![Kiến trúc serverless: Client → API Gateway → Lambda → DynamoDB](/images/3-BlogsTranslated/3.2-Blog2/serverless-architecture.png)

---

## 1. Bài toán chi phí của kiến trúc truyền thống

Một sai lầm phổ biến khi triển khai ứng dụng backend là duy trì các máy chủ EC2 hoạt động 24/7 bất kể lưu lượng thực tế.

Trong nhiều hệ thống nội bộ hoặc startup giai đoạn đầu, CPU thường chỉ sử dụng dưới 10–20% nhưng doanh nghiệp vẫn phải trả tiền cho toàn bộ tài nguyên đã cấp phát.

Điều này tạo ra hiện tượng:

- Trả tiền cho thời gian nhàn rỗi (**Idle Cost**)
- Tốn công quản trị máy chủ
- Khó mở rộng khi lượng truy cập tăng đột biến
- Chi phí vận hành tăng theo số lượng server

**AWS Lambda** được thiết kế để giải quyết chính xác bài toán này bằng mô hình **Serverless Compute**: chỉ chạy code khi có yêu cầu và tự động scale theo lưu lượng thực tế.

---

## 2. Bản chất kỹ thuật của AWS Lambda

AWS Lambda là dịch vụ **Function-as-a-Service (FaaS)**. Thay vì quản lý server, kỹ sư chỉ cần upload code.

Khi có sự kiện phát sinh:

- HTTP Request từ API Gateway
- Upload file lên S3
- Tin nhắn từ SQS
- Event từ EventBridge
- Thay đổi dữ liệu DynamoDB

Lambda sẽ tự động khởi tạo môi trường thực thi, chạy code và giải phóng tài nguyên sau khi hoàn thành.

---

## 3. Phân tích hiệu quả kinh tế

### Mô hình EC2 truyền thống

Ví dụ:

- 1 EC2 `t3.medium` chạy 24/7
- Backend có 500 người dùng/ngày

**Chi phí:** Trả tiền cả tháng — dù có request hay không.

### Mô hình Lambda

Lambda chỉ tính phí:

- Số lần gọi hàm (**Invocations**)
- Thời gian thực thi (**Duration**)

**Không có request → chi phí gần như bằng 0.**

AWS xác nhận Lambda tự động scale từ 0 request lên hàng nghìn request mỗi giây mà không cần quản trị hạ tầng.

---

## 4. Ứng dụng thực tế cho AI Agent và Chatbot

Đây là phần rất hợp với đồ án AI hiện nay.

**Thay vì:**

```text
EC2
└── FastAPI
    └── AI Chatbot
```

**Có thể chuyển sang:**

```text
API Gateway
    │
    ▼
Lambda
    │
    ▼
Amazon Bedrock
```

**Lợi ích:**

- Không cần bật server liên tục
- Tự động scale khi nhiều người dùng chat
- Chỉ trả tiền khi chatbot được sử dụng
- Giảm đáng kể chi phí cho các dự án sinh viên hoặc startup

Lambda hiện còn hỗ trợ tích hợp trực tiếp với các workflow AI thông qua **MCP** và **Bedrock**.

---

## 5. Kết hợp Event-Driven Architecture

Một ưu điểm lớn khác là khả năng xây dựng hệ thống hướng sự kiện.

**Ví dụ website bán hàng:**

```text
Khách đặt hàng
    │
    ▼
DynamoDB
    │
    ▼
EventBridge
    │
    ┌────┴────┐
    ▼         ▼
Lambda      Lambda
Gửi mail    Cập nhật kho
```

Mỗi Lambda chỉ thực hiện một nhiệm vụ nhỏ. AWS khuyến nghị chia nhỏ Lambda theo từng chức năng thay vì xây dựng **"Lambda Monolith"** để tăng khả năng mở rộng và giảm chi phí bảo trì.

---

## 6. Những hạn chế cần lưu ý

### Cold Start

Khi Lambda không được sử dụng trong một khoảng thời gian, lần gọi tiếp theo có thể phát sinh độ trễ khởi động. Đây là vấn đề phổ biến trong kiến trúc serverless.

### Khó debug hơn Monolith

Một hệ thống có 20 Lambda, 10 Queue, 5 Event Source sẽ phức tạp hơn đáng kể so với một backend đơn khối.

### Không phù hợp với workload chạy liên tục

Các tác vụ sau thường không phải ứng viên tốt nhất cho Lambda:

- Streaming thời gian thực
- Video Processing kéo dài
- Hệ thống giao dịch siêu thấp độ trễ

---

## Kết luận

AWS Lambda chuyển tư duy thiết kế backend từ **"luôn bật server"** sang **"chỉ chạy khi có request"** — phù hợp cho backend nhỏ, chatbot, AI agent và dự án có lưu lượng không đều. Kết hợp với API Gateway, DynamoDB, EventBridge và Amazon Bedrock, đây là hướng đi thực tế để giảm chi phí vận hành mà em áp dụng trong dự án workshop nhóm.
