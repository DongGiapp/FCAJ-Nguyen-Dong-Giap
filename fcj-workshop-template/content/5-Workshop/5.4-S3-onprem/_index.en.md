---
title: "Create Lambda Functions"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Step 2: Create Lambda Functions

## Overview

In this step, we'll write Python Lambda functions to handle CRUD operations (Create, Read, Update, Delete) for our Todo API. These functions will interact with the DynamoDB table we created earlier.

---

## Lambda Function Architecture

We'll create 4 Lambda functions:

| Function Name | HTTP Method | Operation | Description |
|---|---|---|---|
| `CreateTodo` | POST | Create | Add new todo item |
| `GetTodos` | GET | Read | Retrieve all todos |
| `UpdateTodo` | PUT | Update | Modify existing todo |
| `DeleteTodo` | DELETE | Delete | Remove a todo item |

---

## Function 1: CreateTodo

### Purpose
Accept a POST request and create a new todo item in DynamoDB.

### Python Code

```python
import json
import boto3
import uuid
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('todos')

def lambda_handler(event, context):
    """
    Create a new todo item
    Expected body: {
        "title": "string",
        "description": "string",
        "status": "pending" (optional)
    }
    """
    try:
        # Parse request body
        body = json.loads(event.get('body', '{}'))
        
        # Validate required fields
        if 'title' not in body:
            return {
                'statusCode': 400,
                'body': json.dumps({'error': 'Missing required field: title'})
            }
        
        # Generate todo ID and timestamps
        todo_id = str(uuid.uuid4())
        timestamp = int(datetime.now().timestamp())
        
        # Create item
        item = {
            'todoId': todo_id,
            'title': body['title'],
            'description': body.get('description', ''),
            'status': body.get('status', 'pending'),
            'createdAt': timestamp,
            'updatedAt': timestamp
        }
        
        # Save to DynamoDB
        table.put_item(Item=item)
        
        return {
            'statusCode': 201,
            'headers': {
                'Content-Type': 'application/json'
            },
            'body': json.dumps(item)
        }
    
    except Exception as e:
        print(f'Error: {str(e)}')
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

---

## Function 2: GetTodos

### Purpose
Retrieve all todo items from DynamoDB.

### Python Code

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
    Retrieve all todos
    Optional query: ?status=pending
    """
    try:
        # Scan all items
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
        print(f'Error: {str(e)}')
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

---

## Function 3: UpdateTodo

### Purpose
Update an existing todo item.

### Python Code

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
    Update an existing todo
    Expected path parameter: todoId
    Expected body: {
        "title": "string" (optional),
        "description": "string" (optional),
        "status": "string" (optional)
    }
    """
    try:
        # Get todo ID from path
        todo_id = event.get('pathParameters', {}).get('todoId')
        
        if not todo_id:
            return {
                'statusCode': 400,
                'body': json.dumps({'error': 'Missing todoId'})
            }
        
        # Parse request body
        body = json.loads(event.get('body', '{}'))
        
        # Build update expression
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
        
        # Update item in DynamoDB
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
        print(f'Error: {str(e)}')
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

---

## Function 4: DeleteTodo

### Purpose
Delete a todo item from DynamoDB.

### Python Code

```python
import json
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('todos')

def lambda_handler(event, context):
    """
    Delete a todo
    Expected path parameter: todoId
    """
    try:
        # Get todo ID from path
        todo_id = event.get('pathParameters', {}).get('todoId')
        
        if not todo_id:
            return {
                'statusCode': 400,
                'body': json.dumps({'error': 'Missing todoId'})
            }
        
        # Delete item from DynamoDB
        table.delete_item(Key={'todoId': todo_id})
        
        return {
            'statusCode': 204,
            'body': ''
        }
    
    except Exception as e:
        print(f'Error: {str(e)}')
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

---

## Creating Lambda Functions in Console

### Step 1: Create Function

1. Go to **Lambda Console**
2. Click **Create function**
3. Choose **Author from scratch**
4. Function name: `CreateTodo`
5. Runtime: **Python 3.11**
6. Click **Create function**

### Step 2: Add Code

1. Copy the `CreateTodo` Python code
2. Paste into code editor
3. Click **Deploy**

### Step 3: Set IAM Role

1. Scroll to **Execution role**
2. Click role name
3. Go to **Permissions**
4. Add inline policy:

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

Replace `REGION` and `ACCOUNT` with your AWS region and account ID.

### Repeat for Other Functions

Create the same for:
- `GetTodos`
- `UpdateTodo`
- `DeleteTodo`

---

## Testing Lambda Functions

### Test CreateTodo

Test event:
```json
{
    "body": "{\"title\": \"Learn AWS Lambda\", \"description\": \"Master serverless computing\"}"
}
```

Expected response:
```json
{
    "todoId": "uuid-here",
    "title": "Learn AWS Lambda",
    "description": "Master serverless computing",
    "status": "pending",
    "createdAt": 1694564800,
    "updatedAt": 1694564800
}
```

---

## Next Step

Proceed to **Step 3: Set up API Gateway** to expose these functions as HTTP endpoints.
