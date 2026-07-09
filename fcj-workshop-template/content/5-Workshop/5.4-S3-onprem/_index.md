---
title: "Tạo Lambda Functions"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Bước 2: Tạo Lambda Functions

## Tổng quan

Trong bước này, chúng ta sẽ viết các hàm Python Lambda để xử lý CRUD operations (Create, Read, Update, Delete) cho Todo API. Các hàm này sẽ tương tác với bảng DynamoDB mà chúng ta đã tạo trước đó.

---

## Kiến trúc Lambda Function

Chúng ta sẽ tạo 4 Lambda functions:

| Tên Function | HTTP Method | Thao tác | Mô tả |
|---|---|---|---|
| `CreateTodo` | POST | Tạo | Thêm mục todo mới |
| `GetTodos` | GET | Đọc | Lấy tất cả todos |
| `UpdateTodo` | PUT | Cập nhật | Chỉnh sửa todo hiện có |
| `DeleteTodo` | DELETE | Xóa | Xóa một mục todo |

---

## Function 1: CreateTodo

### Mục đích
Chấp nhận yêu cầu POST và tạo một mục todo mới trong DynamoDB.

### Mã Python

```python
import json
import boto3
import uuid
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('todos')

def lambda_handler(event, context):
    """
    Tạo một mục todo mới
    Body dự kiến: {
        "title": "string",
        "description": "string",
        "status": "pending" (tùy chọn)
    }
    """
    try:
        # Parse request body
        body = json.loads(event.get('body', '{}'))
        
        # Validate required fields
        if 'title' not in body:
            return {
                'statusCode': 400,
                'body': json.dumps({'error': 'Thiếu trường bắt buộc: title'})
            }
        
        # Generate todo ID và timestamps
        todo_id = str(uuid.uuid4())
        timestamp = int(datetime.now().timestamp())
        
        # Tạo item
        item = {
            'todoId': todo_id,
            'title': body['title'],
            'description': body.get('description', ''),
            'status': body.get('status', 'pending'),
            'createdAt': timestamp,
            'updatedAt': timestamp
        }
        
        # Lưu vào DynamoDB
        table.put_item(Item=item)
        
        return {
            'statusCode': 201,
            'headers': {
                'Content-Type': 'application/json'
            },
            'body': json.dumps(item)
        }
    
    except Exception as e:
        print(f'Lỗi: {str(e)}')
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

---

## Function 2: GetTodos

### Mục đích
Lấy tất cả các mục todo từ DynamoDB.

### Mã Python

```python
import json
import boto3
from decimal import Decimal

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('todos')

class DecimalEncoder(json.JSONEncoder):
    def default(self, o):
        if isinstance(o, Decimal):
            return int(o) if o % 1 == 0 else float(o)
        return super(DecimalEncoder, self).default(o)

def lambda_handler(event, context):
    """
    Lấy tất cả todos
    Truy vấn tùy chọn: ?status=pending
    """
    try:
        # Quét tất cả items
        response = table.scan()
        items = response.get('Items', [])
        
        return {
            'statusCode': 200,
            'headers': {
                'Content-Type': 'application/json'
            },
            'body': json.dumps(items, cls=DecimalEncoder)
        }
    
    except Exception as e:
        print(f'Lỗi: {str(e)}')
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

---

## Function 3: UpdateTodo

### Mục đích
Cập nhật một mục todo hiện có.

### Mã Python

```python
import json
import boto3
from datetime import datetime
from decimal import Decimal

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('todos')

class DecimalEncoder(json.JSONEncoder):
    def default(self, o):
        if isinstance(o, Decimal):
            return int(o) if o % 1 == 0 else float(o)
        return super(DecimalEncoder, self).default(o)

def lambda_handler(event, context):
    """
    Cập nhật một todo hiện có
    Path parameter dự kiến: todoId
    Body dự kiến: {
        "title": "string" (tùy chọn),
        "description": "string" (tùy chọn),
        "status": "string" (tùy chọn)
    }
    """
    try:
        # Lấy todo ID từ path
        todo_id = event.get('pathParameters', {}).get('todoId')
        
        if not todo_id:
            return {
                'statusCode': 400,
                'body': json.dumps({'error': 'Thiếu todoId'})
            }
        
        # Parse request body
        body = json.loads(event.get('body', '{}'))
        
        # Xây dựng update expression
        update_expr = 'SET updatedAt = :now'
        expr_values = {':now': int(datetime.now().timestamp())}
        
        if 'title' in body:
            update_expr += ', title = :title'
            expr_values[':title'] = body['title']
        
        if 'description' in body:
            update_expr += ', description = :desc'
            expr_values[':desc'] = body['description']
        
        if 'status' in body:
            update_expr += ', #status = :status'
            expr_values[':status'] = body['status']
        
        # Cập nhật item trong DynamoDB
        response = table.update_item(
            Key={'todoId': todo_id},
            UpdateExpression=update_expr,
            ExpressionAttributeValues=expr_values,
            ExpressionAttributeNames={'#status': 'status'} if 'status' in body else {},
            ReturnValues='ALL_NEW'
        )
        
        return {
            'statusCode': 200,
            'headers': {
                'Content-Type': 'application/json'
            },
            'body': json.dumps(response['Attributes'], cls=DecimalEncoder)
        }
    
    except Exception as e:
        print(f'Lỗi: {str(e)}')
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

---

## Function 4: DeleteTodo

### Mục đích
Xóa một mục todo khỏi DynamoDB.

### Mã Python

```python
import json
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('todos')

def lambda_handler(event, context):
    """
    Xóa một todo
    Path parameter dự kiến: todoId
    """
    try:
        # Lấy todo ID từ path
        todo_id = event.get('pathParameters', {}).get('todoId')
        
        if not todo_id:
            return {
                'statusCode': 400,
                'body': json.dumps({'error': 'Thiếu todoId'})
            }
        
        # Xóa item từ DynamoDB
        table.delete_item(Key={'todoId': todo_id})
        
        return {
            'statusCode': 204,
            'body': ''
        }
    
    except Exception as e:
        print(f'Lỗi: {str(e)}')
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

---

## Tạo Lambda Functions trong Console

### Bước 1: Tạo Function

1. Đi tới **Lambda Console**
2. Nhấp **Create function**
3. Chọn **Author from scratch**
4. Tên function: `CreateTodo`
5. Runtime: **Python 3.11**
6. Nhấp **Create function**

### Bước 2: Thêm Code

1. Sao chép mã Python `CreateTodo`
2. Dán vào code editor
3. Nhấp **Deploy**

### Bước 3: Đặt IAM Role

1. Cuộn xuống **Execution role**
2. Nhấp tên role
3. Đi tới **Permissions**
4. Thêm inline policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:PutItem",
                "dynamodb:GetItem",
                "dynamodb:Scan",
                "dynamodb:Query",
                "dynamodb:UpdateItem",
                "dynamodb:DeleteItem"
            ],
            "Resource": "arn:aws:dynamodb:REGION:ACCOUNT:table/todos"
        }
    ]
}
```

Thay thế `REGION` và `ACCOUNT` bằng khu vực AWS và ID tài khoản của bạn.

### Lặp lại cho các Function khác

Tạo tương tự cho:
- `GetTodos`
- `UpdateTodo`
- `DeleteTodo`

---

## Kiểm tra Lambda Functions

### Kiểm tra CreateTodo

Test event:
```json
{
    "body": "{\"title\": \"Học AWS Lambda\", \"description\": \"Thành thạo serverless computing\"}"
}
```

---

## Bước tiếp theo

Tiếp tục với **Bước 3: Thiết lập API Gateway** để hiển thị các hàm này dưới dạng HTTP endpoints.
