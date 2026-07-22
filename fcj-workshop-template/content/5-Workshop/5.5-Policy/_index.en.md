---
title: "Testing & Validation"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Step 3: Set up API Gateway & Testing

## Part A: Create API Gateway

### 1. Create REST API

1. Go to **API Gateway Console**
2. Click **Create API**
3. Choose **REST API**
4. API name: `TodoAPI`
5. Click **Create**

---

### 2. Create Resources & Methods

#### Create `/todos` Resource
1. Click **Actions > Create Resource**
2. Resource name: `todos`
3. Click **Create Resource**

#### Create POST Method (Create Todo)
1. Select `/todos` resource
2. Click **Actions > Create Method > POST**
3. Choose **Lambda Function**
4. Lambda Function: `CreateTodo`
5. Click **Save**

#### Create GET Method (Get All Todos)
1. Select `/todos` resource
2. Click **Actions > Create Method > GET**
3. Lambda Function: `GetTodos`
4. Click **Save**

---

#### Create `{todoId}` Resource
1. Right-click `/todos` resource
2. **Create Resource**
3. Path: `{todoId}`
4. Click **Create Resource**

#### Create PUT Method (Update Todo)
1. Select `/{todoId}` resource
2. **Create Method > PUT**
3. Lambda Function: `UpdateTodo`
4. Click **Save**

#### Create DELETE Method
1. Select `/{todoId}` resource
2. **Create Method > DELETE**
3. Lambda Function: `DeleteTodo`
4. Click **Save**

---

### 3. Deploy API

1. Click **Actions > Deploy API**
2. Deployment stage: **prod** (or **test**)
3. Click **Deploy**
4. Note the **Invoke URL** (e.g., `https://xxxxx.execute-api.us-east-1.amazonaws.com/prod`)

---

## Part B: Test Endpoints with Postman

### Setup Postman

1. Download [Postman](https://www.postman.com/downloads/)
2. Create new request collection: "Todo API"
3. Set base URL as environment variable: `{{API_URL}}`

---

### Test 1: Create Todo (POST)

**Request:**
```
POST {{API_URL}}/todos
Content-Type: application/json

{
  "title": "Learn AWS",
  "description": "Master Lambda, DynamoDB, API Gateway",
  "status": "pending"
}
```

**Expected Response (201):**
```json
{
  "todoId": "uuid-xxx",
  "title": "Learn AWS",
  "description": "Master Lambda, DynamoDB, API Gateway",
  "status": "pending",
  "createdAt": 1694564800,
  "updatedAt": 1694564800
}
```

---

### Test 2: Get All Todos (GET)

**Request:**
```
GET {{API_URL}}/todos
```

**Expected Response (200):**
```json
[
  {
    "todoId": "uuid-xxx",
    "title": "Learn AWS",
    ...
  },
  {
    "todoId": "uuid-yyy",
    "title": "Complete Workshop",
    ...
  }
]
```

---

### Test 3: Update Todo (PUT)

**Request:**
```
PUT {{API_URL}}/todos/uuid-xxx
Content-Type: application/json

{
  "status": "completed",
  "title": "Completed AWS Learning"
}
```

**Expected Response (200):**
```json
{
  "todoId": "uuid-xxx",
  "title": "Completed AWS Learning",
  "status": "completed",
  "updatedAt": 1694564900
}
```

---

### Test 4: Delete Todo (DELETE)

**Request:**
```
DELETE {{API_URL}}/todos/uuid-xxx
```

**Expected Response (204):**
```
No content
```

---

## Part C: Monitor with CloudWatch

### View Lambda Logs

1. Go to **CloudWatch Console**
2. Click **Log Groups**
3. Find `/aws/lambda/CreateTodo`
4. Click to view log streams
5. Check for errors or print statements

### Create CloudWatch Dashboard

1. **Dashboards > Create Dashboard**
2. Add widgets:
   - Lambda Invocations
   - Lambda Errors
   - Lambda Duration
   - DynamoDB ConsumedWriteCapacityUnits

---

## Part D: Test Results Summary

| Endpoint | Method | Status | Response |
|----------|--------|--------|----------|
| `/todos` | POST | 201 | Created todo with ID |
| `/todos` | GET | 200 | List of all todos |
| `/todos/{id}` | PUT | 200 | Updated todo |
| `/todos/{id}` | DELETE | 204 | No content |

---

## Metrics to Check

✅ **Lambda**
- Invocations: Should match API calls
- Duration: Record the measured baseline and investigate regressions or execution time approaching the configured timeout
- Errors: Should be 0

✅ **DynamoDB**
- ConsumedWriteCapacityUnits: Increases with inserts
- ConsumedReadCapacityUnits: Increases with reads
- No throttling errors

✅ **API Gateway**
- 2xx responses: Successful requests
- 4xx responses: Client errors (should be minimal)
- 5xx responses: Server errors (should be 0)

---

## Next Step

Proceed to **Step 4: Clean-up Resources** to remove AWS resources and avoid charges.
