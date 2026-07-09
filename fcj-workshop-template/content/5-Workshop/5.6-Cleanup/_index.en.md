---
title: "Clean up Resources"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Step 4: Clean up Resources

{{% notice warning %}}
Always delete AWS resources after testing to avoid unexpected charges to your account.
{{% /notice %}}

## Cleanup Checklist

- [ ] Delete API Gateway (TodoAPI)
- [ ] Delete Lambda Functions (CreateTodo, GetTodos, UpdateTodo, DeleteTodo)
- [ ] Delete DynamoDB Table (todos)
- [ ] Delete IAM Roles created for Lambda
- [ ] Delete CloudWatch Log Groups

---

## 1. Delete API Gateway

### Via Console

1. Go to [API Gateway Console](https://console.aws.amazon.com/apigateway/)
2. Click on **TodoAPI**
3. Click **Actions** → **Delete API**
4. Type the API name to confirm, then click **Delete**

### Via CLI

```bash
API_ID=$(aws apigateway get-rest-apis \
  --query 'items[?name==`TodoAPI`].id' \
  --output text)

aws apigateway delete-rest-api --rest-api-id $API_ID
```

---

## 2. Delete Lambda Functions

### Via Console

1. Go to [Lambda Console](https://console.aws.amazon.com/lambda/)
2. Tick the checkbox next to each of the 4 functions:
   - `CreateTodo`
   - `GetTodos`
   - `UpdateTodo`
   - `DeleteTodo`
3. Click **Actions** → **Delete**
4. Type `delete` to confirm, then click **Delete**

### Via CLI

```bash
aws lambda delete-function --function-name CreateTodo
aws lambda delete-function --function-name GetTodos
aws lambda delete-function --function-name UpdateTodo
aws lambda delete-function --function-name DeleteTodo
```

---

## 3. Delete DynamoDB Table

### Via Console

1. Go to [DynamoDB Console](https://console.aws.amazon.com/dynamodb/)
2. Click **Tables** in the left menu
3. Select the `todos` table
4. Click **Actions** → **Delete table**
5. Check **Delete all CloudWatch alarms for this table**
6. Type `confirm` and click **Delete**

### Via CLI

```bash
aws dynamodb delete-table --table-name todos
```

---

## 4. Delete IAM Roles

1. Go to [IAM Console](https://console.aws.amazon.com/iam/)
2. Click **Roles** in the left menu
3. Search for roles created during the workshop (e.g., names containing `todo` or `lambda`)
4. Select each role and click **Delete**
5. Type the role name to confirm, then click **Delete**

---

## 5. Delete CloudWatch Log Groups

### Via Console

1. Go to [CloudWatch Console](https://console.aws.amazon.com/cloudwatch/)
2. Click **Log groups** in the left menu
3. Search for `/aws/lambda/`
4. Select the 4 log groups:
   - `/aws/lambda/CreateTodo`
   - `/aws/lambda/GetTodos`
   - `/aws/lambda/UpdateTodo`
   - `/aws/lambda/DeleteTodo`
5. Click **Actions** → **Delete log group(s)**
6. Click **Delete** to confirm

### Via CLI

```bash
aws logs delete-log-group --log-group-name /aws/lambda/CreateTodo
aws logs delete-log-group --log-group-name /aws/lambda/GetTodos
aws logs delete-log-group --log-group-name /aws/lambda/UpdateTodo
aws logs delete-log-group --log-group-name /aws/lambda/DeleteTodo
```

---

## 6. Verify Cleanup

Run the following to confirm all resources have been removed:

```bash
# Should return empty list
aws lambda list-functions \
  --query "Functions[?contains(FunctionName,'Todo')].FunctionName"

# Should return empty list
aws dynamodb list-tables \
  --query "TableNames[?contains(@,'todos')]"

# Should return empty list
aws apigateway get-rest-apis \
  --query "items[?name=='TodoAPI'].id"
```

All commands should return `[]`.

---

**Congratulations! You have successfully completed the Serverless Todo API workshop.**

