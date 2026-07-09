---
title: "Thiết lập API Gateway & Kiểm tra"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Bước 3: Thiết lập API Gateway & Kiểm tra

## Phần A: Tạo API Gateway

### 1. Tạo REST API

1. Đi tới **API Gateway Console**
2. Nhấp **Create API**
3. Chọn **REST API**
4. Tên API: `TodoAPI`
5. Nhấp **Create**

---

### 2. Tạo Resources & Methods

#### Tạo Resource `/todos`
1. Nhấp **Actions > Create Resource**
2. Tên resource: `todos`
3. Nhấp **Create Resource**

#### Tạo POST Method (Create Todo)
1. Chọn resource `/todos`
2. Nhấp **Actions > Create Method > POST**
3. Chọn **Lambda Function**
4. Lambda Function: `CreateTodo`
5. Nhấp **Save**

#### Tạo GET Method (Get All Todos)
1. Chọn resource `/todos`
2. Nhấp **Actions > Create Method > GET**
3. Lambda Function: `GetTodos`
4. Nhấp **Save**

---

#### Tạo Resource `{todoId}`
1. Nhấp phải `/todos` resource
2. **Create Resource**
3. Path: `{todoId}`
4. Nhấp **Create Resource**

#### Tạo PUT Method (Update Todo)
1. Chọn resource `/{todoId}`
2. **Create Method > PUT**
3. Lambda Function: `UpdateTodo`
4. Nhấp **Save**

#### Tạo DELETE Method
1. Chọn resource `/{todoId}`
2. **Create Method > DELETE**
3. Lambda Function: `DeleteTodo`
4. Nhấp **Save**

---

### 3. Deploy API

1. Nhấp **Actions > Deploy API**
2. Deployment stage: **prod** (hoặc **test**)
3. Nhấp **Deploy**
4. Ghi chép **Invoke URL** (ví dụ: `https://xxxxx.execute-api.us-east-1.amazonaws.com/prod`)

---

## Phần B: Kiểm tra Endpoints với Postman

### Cài đặt Postman

1. Tải xuống [Postman](https://www.postman.com/downloads/)
2. Tạo bộ sưu tập yêu cầu mới: "Todo API"
3. Đặt URL cơ sở làm biến môi trường: `{{API_URL}}`

---

### Test 1: Tạo Todo (POST)

**Request:**
```
POST {{API_URL}}/todos
Content-Type: application/json

{
  "title": "Học AWS",
  "description": "Thành thạo Lambda, DynamoDB, API Gateway",
  "status": "pending"
}
```

**Phản hồi dự kiến (201):**
```json
{
  "todoId": "uuid-xxx",
  "title": "Học AWS",
  "status": "pending"
}
```

---

### Test 2: Lấy All Todos (GET)

**Request:**
```
GET {{API_URL}}/todos
```

**Phản hồi dự kiến (200):**
```json
[
  {
    "todoId": "uuid-xxx",
    "title": "Học AWS"
  }
]
```

---

### Test 3: Cập nhật Todo (PUT)

**Request:**
```
PUT {{API_URL}}/todos/uuid-xxx
Content-Type: application/json

{
  "status": "completed"
}
```

**Phản hồi dự kiến (200):**
```json
{
  "todoId": "uuid-xxx",
  "status": "completed"
}
```

---

### Test 4: Xóa Todo (DELETE)

**Request:**
```
DELETE {{API_URL}}/todos/uuid-xxx
```

**Phản hồi dự kiến (204):**
```
Không có nội dung
```

---

## Phần C: Giám sát với CloudWatch

### Xem Lambda Logs

1. Đi tới **CloudWatch Console**
2. Nhấp **Log Groups**
3. Tìm `/aws/lambda/CreateTodo`
4. Nhấp để xem log streams
5. Kiểm tra lỗi hoặc print statements

### Tạo CloudWatch Dashboard

1. **Dashboards > Create Dashboard**
2. Thêm widgets:
   - Lambda Invocations
   - Lambda Errors
   - Lambda Duration
   - DynamoDB ConsumedWriteCapacityUnits

---

## Tóm tắt Kết quả Kiểm tra

| Endpoint | Method | Status | Response |
|----------|--------|--------|----------|
| `/todos` | POST | 201 | Tạo todo với ID |
| `/todos` | GET | 200 | Danh sách tất cả todos |
| `/todos/{id}` | PUT | 200 | Cập nhật todo |
| `/todos/{id}` | DELETE | 204 | Không có nội dung |

---

## Bước tiếp theo

Tiếp tục với **Bước 4: Dọn dẹp Tài nguyên** để xóa tài nguyên AWS và tránh phí.
