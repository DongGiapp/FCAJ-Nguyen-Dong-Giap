---
title: "Dọn dẹp Tài nguyên"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Bước 4: Dọn dẹp Tài nguyên

{{% notice warning %}}
Luôn xóa các tài nguyên AWS sau khi kiểm thử để tránh phát sinh chi phí không mong muốn.
{{% /notice %}}

## Danh Sách Kiểm Tra

- [ ] Xóa API Gateway (TodoAPI)
- [ ] Xóa Lambda Functions (CreateTodo, GetTodos, UpdateTodo, DeleteTodo)
- [ ] Xóa DynamoDB Table (todos)
- [ ] Xóa IAM Roles đã tạo cho Lambda
- [ ] Xóa CloudWatch Log Groups

---

## 1. Xóa API Gateway

### Qua Console

1. Truy cập [API Gateway Console](https://console.aws.amazon.com/apigateway/)
2. Click vào **TodoAPI**
3. Click **Actions** → **Delete API**
4. Nhập tên API để xác nhận, sau đó click **Delete**

### Qua CLI

```bash
API_ID=$(aws apigateway get-rest-apis \
  --query 'items[?name==`TodoAPI`].id' \
  --output text)

aws apigateway delete-rest-api --rest-api-id $API_ID
```

---

## 2. Xóa Lambda Functions

### Qua Console

1. Truy cập [Lambda Console](https://console.aws.amazon.com/lambda/)
2. Đánh dấu chọn cả 4 function:
   - `CreateTodo`
   - `GetTodos`
   - `UpdateTodo`
   - `DeleteTodo`
3. Click **Actions** → **Delete**
4. Nhập `delete` để xác nhận, sau đó click **Delete**

### Qua CLI

```bash
aws lambda delete-function --function-name CreateTodo
aws lambda delete-function --function-name GetTodos
aws lambda delete-function --function-name UpdateTodo
aws lambda delete-function --function-name DeleteTodo
```

---

## 3. Xóa DynamoDB Table

### Qua Console

1. Truy cập [DynamoDB Console](https://console.aws.amazon.com/dynamodb/)
2. Click **Tables** ở menu bên trái
3. Chọn bảng `todos`
4. Click **Actions** → **Delete table**
5. Đánh dấu **Delete all CloudWatch alarms for this table**
6. Nhập `confirm` và click **Delete**

### Qua CLI

```bash
aws dynamodb delete-table --table-name todos
```

---

## 4. Xóa IAM Roles

1. Truy cập [IAM Console](https://console.aws.amazon.com/iam/)
2. Click **Roles** ở menu bên trái
3. Tìm kiếm các role đã tạo trong workshop (ví dụ: tên chứa `todo` hoặc `lambda`)
4. Chọn từng role và click **Delete**
5. Nhập tên role để xác nhận, sau đó click **Delete**

---

## 5. Xóa CloudWatch Log Groups

### Qua Console

1. Truy cập [CloudWatch Console](https://console.aws.amazon.com/cloudwatch/)
2. Click **Log groups** ở menu bên trái
3. Tìm kiếm `/aws/lambda/`
4. Chọn 4 log groups:
   - `/aws/lambda/CreateTodo`
   - `/aws/lambda/GetTodos`
   - `/aws/lambda/UpdateTodo`
   - `/aws/lambda/DeleteTodo`
5. Click **Actions** → **Delete log group(s)**
6. Click **Delete** để xác nhận

### Qua CLI

```bash
aws logs delete-log-group --log-group-name /aws/lambda/CreateTodo
aws logs delete-log-group --log-group-name /aws/lambda/GetTodos
aws logs delete-log-group --log-group-name /aws/lambda/UpdateTodo
aws logs delete-log-group --log-group-name /aws/lambda/DeleteTodo
```

---

## 6. Xác Minh Đã Dọn Xong

Chạy các lệnh sau để xác nhận tất cả tài nguyên đã được xóa:

```bash
# Phải trả về danh sách rỗng
aws lambda list-functions \
  --query "Functions[?contains(FunctionName,'Todo')].FunctionName"

# Phải trả về danh sách rỗng
aws dynamodb list-tables \
  --query "TableNames[?contains(@,'todos')]"

# Phải trả về danh sách rỗng
aws apigateway get-rest-apis \
  --query "items[?name=='TodoAPI'].id"
```

Tất cả lệnh phải trả về `[]`.

---


