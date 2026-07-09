---
title: "Introduction"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Serverless Todo API on AWS

## Overview

In this workshop, you will build a **Serverless Todo Application API** using AWS services. This is a practical exercise that demonstrates how to create a modern, scalable backend without managing servers.

### What You'll Build

A complete RESTful API for a Todo application where users can:
- Create new todo items
- Retrieve all todos
- Update todo status
- Delete todos

All operations are persisted in a database and accessed through HTTP endpoints.

### Real-World Use Cases

This architecture is used for:
- **Microservices**: Building independent, scalable services
- **Content Management Systems**: APIs for blogs, newsletters
- **Mobile Backends**: Serving data to mobile applications
- **IoT Dashboards**: Collecting and serving sensor data
- **E-commerce**: Product catalogs, order management

---

## Architecture at a Glance

```
Client (Postman/Browser)
    ↓
API Gateway (HTTP Endpoint)
    ↓
Lambda Functions (Business Logic)
    ↓
DynamoDB (Data Storage)
```

### 3 Key AWS Services

1. **API Gateway**: Creates REST API endpoints that clients can call
2. **Lambda**: Serverless functions that handle request logic (CRUD operations)
3. **DynamoDB**: NoSQL database for storing todo items

---

## Why This Stack?

| Service | Why Use It |
|---------|-----------|
| **API Gateway** | Provides public HTTP endpoints, handles routing, manages traffic |
| **Lambda** | Pay-per-use pricing, automatic scaling, no server management |
| **DynamoDB** | Fully managed NoSQL database, auto-scaling, millisecond latency |

---

## What You'll Learn

- ✅ Create a DynamoDB table with proper key schema
- ✅ Write Python Lambda functions for CRUD operations
- ✅ Set up API Gateway to route HTTP requests to Lambda
- ✅ Test API endpoints using Postman
- ✅ Monitor with CloudWatch Logs
- ✅ Implement IAM roles and Least Privilege security
- ✅ Deploy using Infrastructure as Code (CloudFormation)
- ✅ Clean up resources properly

---

## Expected Outcomes

After this workshop, you will have:
- A fully functional Serverless Todo API on AWS
- Understanding of serverless architecture benefits
- Hands-on experience with 3 AWS core services
- Knowledge of REST API design principles
- A working, scalable application 

