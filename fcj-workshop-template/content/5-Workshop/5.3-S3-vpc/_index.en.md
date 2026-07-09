---
title: "Create DynamoDB Table"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# Step 1: Create DynamoDB Table

## Overview

Amazon DynamoDB is a fully managed, serverless NoSQL database with single-digit millisecond performance at any scale. In this step, we create the `todos` table that will store all todo items for our API.

---

## Table Schema

| Attribute | Type | Role |
|---|---|---|
| `todoId` | String | Partition Key (Primary Key) — auto-generated UUID |
| `title` | String | Todo title |
| `description` | String | Optional detail text |
| `status` | String | `pending` or `completed` |
| `createdAt` | Number | Unix timestamp (set on creation) |
| `updatedAt` | Number | Unix timestamp (updated on every change) |

---

## Create Table via AWS Console

### Step 1: Open DynamoDB Console

1. Sign in to the [AWS Management Console](https://console.aws.amazon.com/)
2. In the top search bar, type **DynamoDB** and click the service
3. In the left menu, click **Tables**

### Step 2: Create the Table

1. Click **Create table**
2. Fill in the table details:
   - **Table name**: `todos`
   - **Partition key**: `todoId` — type **String**
   - Leave **Sort key** empty
3. Under **Table settings**, select **Customize settings**
4. Under **Read/write capacity settings**, choose **On-demand**
   - On-demand charges only for what you use — no capacity planning needed
5. Leave all other settings as default
6. Click **Create table**

### Step 3: Verify the Table

1. Wait for the status to change from **Creating** → **Active** (usually < 30 seconds)
2. Click the `todos` table to open its details
3. Confirm the following:
   - **Table name**: `todos`
   - **Partition key**: `todoId (S)`
   - **Status**: Active
   - **Billing mode**: On-demand

---

## Create Table via AWS CLI

```bash
aws dynamodb create-table \
  --table-name todos \
  --attribute-definitions AttributeName=todoId,AttributeType=S \
  --key-schema AttributeName=todoId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region us-east-1
```

Verify the table is active:

```bash
aws dynamodb describe-table \
  --table-name todos \
  --query "Table.TableStatus" \
  --output text
```

Expected output:

```
ACTIVE
```

---

## Test with a Sample Item (Optional)

You can insert a test item directly to verify the table works:

```bash
aws dynamodb put-item \
  --table-name todos \
  --item '{
    "todoId":     {"S": "test-001"},
    "title":      {"S": "Test item"},
    "description":{"S": "Verify DynamoDB works"},
    "status":     {"S": "pending"},
    "createdAt":  {"N": "1700000000"},
    "updatedAt":  {"N": "1700000000"}
  }'
```

Then read it back:

```bash
aws dynamodb get-item \
  --table-name todos \
  --key '{"todoId": {"S": "test-001"}}'
```

Delete the test item before continuing:

```bash
aws dynamodb delete-item \
  --table-name todos \
  --key '{"todoId": {"S": "test-001"}}'
```

---

## Next Step

Proceed to **Step 2: Create Lambda Functions** to write the CRUD business logic that reads from and writes to this table.

