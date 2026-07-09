---
title: "Giới thiệu"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Serverless Todo API trên AWS

## Tổng quan

Trong workshop này, bạn sẽ xây dựng một **API Ứng dụng Todo Serverless** sử dụng các dịch vụ AWS. Đây là một bài tập thực tế để minh họa cách tạo một backend hiện đại, có thể mở rộng mà không cần quản lý máy chủ.

### Bạn sẽ xây dựng cái gì?

Một API RESTful hoàn chỉnh cho ứng dụng Todo, nơi người dùng có thể:
- Tạo các mục todo mới
- Lấy tất cả todos
- Cập nhật trạng thái todo
- Xóa todos

Tất cả các thao tác được lưu trữ trong cơ sở dữ liệu và truy cập qua các điểm cuối HTTP.

### Trường hợp sử dụng thực tế

Kiến trúc này được sử dụng cho:
- **Microservices**: Xây dựng các dịch vụ độc lập, có thể mở rộng
- **Web/Mobile Backends**: Phục vụ dữ liệu cho ứng dụng
- **IoT Platforms**: Thu thập và phục vụ dữ liệu cảm biến
- **E-commerce**: Quản lý danh mục sản phẩm

---

## Kiến trúc tổng quan

```
Client (Postman/Browser)
    ↓
API Gateway (HTTP Endpoint)
    ↓
Lambda Functions (Business Logic)
    ↓
DynamoDB (Data Storage)
```

### 3 Dịch vụ AWS chính

1. **API Gateway**: Tạo các điểm cuối REST API mà client có thể gọi
2. **Lambda**: Các hàm serverless xử lý logic yêu cầu (CRUD operations)
3. **DynamoDB**: Cơ sở dữ liệu NoSQL để lưu trữ các mục todo

---

## Tại sao chọn stack này?

| Dịch vụ | Tại sao sử dụng |
|---------|-----------|
| **API Gateway** | Cung cấp các điểm cuối HTTP công khai, định tuyến yêu cầu |
| **Lambda** | Trả tiền theo lượng sử dụng, tự động mở rộng, không quản lý máy chủ |
| **DynamoDB** | Cơ sở dữ liệu NoSQL được quản lý hoàn toàn, tự động mở rộng, độ trễ mili giây |

---

## Bạn sẽ học được gì?

- ✅ Tạo bảng DynamoDB với schema khóa phù hợp
- ✅ Viết các hàm Lambda Python cho CRUD operations
- ✅ Thiết lập API Gateway để định tuyến yêu cầu HTTP tới Lambda
- ✅ Kiểm tra các điểm cuối API bằng Postman
- ✅ Giám sát bằng CloudWatch Logs
- ✅ Triển khai IAM roles và Least Privilege security
- ✅ Deploy sử dụng Infrastructure as Code (CloudFormation)
- ✅ Dọn dẹp tài nguyên một cách thích hợp

---

## Kết quả dự kiến

Sau khi hoàn thành workshop, bạn sẽ có:
- Một API Todo Serverless hoạt động đầy đủ trên AWS
- Hiểu biết về lợi ích của kiến trúc serverless
- Kinh nghiệm thực tế với 3 dịch vụ AWS cốt lõi
- Kiến thức về nguyên tắc thiết kế REST API
- Một ứng dụng hoạt động, có thể mở rộng

