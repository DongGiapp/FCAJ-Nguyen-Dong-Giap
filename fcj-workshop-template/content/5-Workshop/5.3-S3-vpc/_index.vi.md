---
title: "Tạo DynamoDB Table"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# Bước 1: Tạo DynamoDB Table

## Tổng quan

Amazon DynamoDB là dịch vụ cơ sở dữ liệu NoSQL serverless được quản lý hoàn toàn, với hiệu suất hàng chục mili giây ở mọi quy mô. Trong bước này, chúng ta sẽ tạo bảng `todos` để lưu trữ tất cả các mục todo cho API.

---

## Cấu trúc Bảng

| Thuộc tính | Kiểu | Vai trò |
|---|---|---|
| `todoId` | String | Partition Key (Khóa chính) — UUID tự động sinh |
| `title` | String | Tiêu đề todo |
| `description` | String | Nội dung mô tả (tùy chọn) |
| `status` | String | `pending` hoặc `completed` |
| `createdAt` | Number | Unix timestamp (đặt khi tạo) |
| `updatedAt` | Number | Unix timestamp (cập nhật mỗi lần thay đổi) |

---

## Tạo Bảng qua AWS Console

### Bước 1: Mở DynamoDB Console

1. Đăng nhập vào [AWS Management Console](https://console.aws.amazon.com/)
2. Trong thanh tìm kiếm, nhập **DynamoDB** và click vào dịch vụ
3. Ở menu bên trái, click **Tables**

### Bước 2: Tạo Bảng

1. Click **Create table**
2. Điền thông tin bảng:
   - **Table name**: `todos`
   - **Partition key**: `todoId` — kiểu **String**
   - Để trống **Sort key**
3. Trong **Table settings**, chọn **Customize settings**
4. Trong **Read/write capacity settings**, chọn **On-demand**
   - On-demand chỉ tính phí cho lượng sử dụng thực tế, không cần lên kế hoạch dung lượng
5. Giữ nguyên các cài đặt khác
6. Click **Create table**

### Bước 3: Xác Minh Bảng

1. Chờ trạng thái chuyển từ **Creating** → **Active** (thường dưới 30 giây)
2. Click vào bảng `todos` để xem chi tiết
3. Xác nhận:
   - **Table name**: `todos`
   - **Partition key**: `todoId (S)`
   - **Status**: Active
   - **Billing mode**: On-demand

---

## Tạo Bảng qua AWS CLI

```bash
aws dynamodb create-table \
  --table-name todos \
  --attribute-definitions AttributeName=todoId,AttributeType=S \
  --key-schema AttributeName=todoId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region us-east-1
```

Kiểm tra bảng đã Active chưa:

```bash
aws dynamodb describe-table \
  --table-name todos \
  --query "Table.TableStatus" \
  --output text
```

Kết quả mong đợi:

```
ACTIVE
```

---

## Kiểm Tra Bằng Item Mẫu (Tùy Chọn)

Bạn có thể thêm một item thử để xác minh bảng hoạt động:

```bash
aws dynamodb put-item \
  --table-name todos \
  --item '{
    "todoId":     {"S": "test-001"},
    "title":      {"S": "Item thử nghiệm"},
    "description":{"S": "Kiểm tra DynamoDB hoạt động"},
    "status":     {"S": "pending"},
    "createdAt":  {"N": "1700000000"},
    "updatedAt":  {"N": "1700000000"}
  }'
```

Đọc lại item:

```bash
aws dynamodb get-item \
  --table-name todos \
  --key '{"todoId": {"S": "test-001"}}'
```

Xóa item thử trước khi tiếp tục:

```bash
aws dynamodb delete-item \
  --table-name todos \
  --key '{"todoId": {"S": "test-001"}}'
```

---

## Bước Tiếp Theo

Tiếp tục với **Bước 2: Tạo Lambda Functions** để viết logic CRUD đọc và ghi vào bảng này.

