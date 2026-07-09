---
title: "Project Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Serverless Todo API - Project Proposal

---

## 1. Project Overview

**Project Name**: Serverless Todo Application API  
**Duration**: 12 weeks (April 17 - July 19, 2026)  
**Intern**: Phan Chế Đức  
**University**: HUTECH (Ho Chi Minh City University of Technology)

### What is This Project?

A modern, scalable Todo management system built entirely on AWS serverless services. Users can create, read, update, and delete todo items through a REST API, perfect for learning AWS architecture and best practices.

![alt text](</images/2-Proposal/Screenshot 2026-07-08 170037-1.png>) ![alt text](</images/2-Proposal/Screenshot 2026-07-08 165309-1.png>) ![alt text](</images/2-Proposal/Screenshot 2026-07-08 165926-1.png>) ![alt text](</images/2-Proposal/Screenshot 2026-07-08 170022-1.png>)
![alt text](/images/2-Proposal/image.png)

---

## 2. Problem Statement

### Current Challenges
- Traditional server-based applications require constant management
- Scaling issues with fixed infrastructure
- High operational costs from idle server resources
- Difficulty in learning cloud-native architecture

### Solution
Build a serverless solution that:
- Requires zero server management
- Auto-scales with demand
- Costs only for actual usage
- Demonstrates AWS best practices

---

## 3. Project Objectives

### Primary Objectives
1. **Learn AWS Services**: Hands-on experience with Lambda, DynamoDB, API Gateway
2. **Build Production-Ready Code**: Write clean, maintainable Python code
3. **Understand Serverless Architecture**: Know when and how to use serverless
4. **Implement Security**: Apply IAM least privilege principles
5. **Monitor & Troubleshoot**: Use CloudWatch for observability

### Measurable Outcomes
- Complete CRUD API with all endpoints working
- Zero errors in 100+ test requests
- Sub-100ms average response time
- Documentation for full deployment
- Cost < $1 USD for 3-hour workshop

---

## 4. Solution Architecture

```
                          [Clients]
                              ↓
                   [API Gateway]
                   (REST endpoints)
                              ↓
        ┌─────────────────────┼─────────────────────┐
        ↓                      ↓                      ↓
  [CreateTodo]          [GetTodos]          [UpdateTodo/Delete]
  (Lambda Python)       (Lambda Python)     (Lambda Python)
        ↓                      ↓                      ↓
        └─────────────────────┼─────────────────────┘
                              ↓
                       [DynamoDB Table]
                       (todos - Data Store)
                              ↓
                       [CloudWatch Logs]
                       (Monitoring)
```

### AWS Services Used (3 Core Services)

| Service | Purpose | Why Choose |
|---------|---------|-----------|
| **API Gateway** | REST API endpoints | Managed, scalable, easy integration |
| **Lambda** | Business logic execution | Pay-per-use, auto-scaling, fast |
| **DynamoDB** | Data persistence | Fully managed, infinite scalability |

### IAM Security
- Least Privilege principle: Each Lambda has minimal permissions
- Separate roles for each function
- No hardcoded credentials

---

## 5. Project Timeline

### Phase 1: Setup (Week 1-2) ✅
- AWS account and CLI configuration
- DynamoDB table creation
- Initial IAM role setup

### Phase 2: Development (Week 3-10) ✅
- Lambda functions (Create/Read/Update/Delete)
- API Gateway configuration
- Integration testing
- CloudWatch monitoring

### Phase 3: Documentation & Deployment (Week 11-12) ✅
- Complete bilingual documentation for the core content
- Infrastructure as Code (CloudFormation)
- Final testing and cleanup

---

## 6. Technical Requirements

### Web API Specification

#### POST /todos - Create Todo
```
Request: {
  "title": "string (required)",
  "description": "string",
  "status": "pending|completed"
}
Response: 201 {
  "todoId": "uuid",
  "createdAt": timestamp
}
```

#### GET /todos - Get All
```
Response: 200 [
  { "todoId": "...", ... }
]
```

#### PUT /todos/{id} - Update
```
Request: { "status": "completed" }
Response: 200 { ...updated item }
```

#### DELETE /todos/{id} - Delete
```
Response: 204 (No Content)
```

---

## 7. Success Criteria

✅ **Functional Requirements**
- All 4 CRUD endpoints working
- 100% test pass rate
- < 100ms average response time

✅ **Non-Functional Requirements**
- Zero unhandled errors
- Cost < $1 USD
- CloudWatch logs for debugging

✅ **Documentation**
- Core content available in both Vietnamese and English
- Step-by-step deployment guide
- Architecture diagrams

---

## 8. Risks & Mitigation

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|-----------|
| Lambda timeout | Low | High | Optimize code, increase timeout |
| DynamoDB throttling | Low | Medium | Use on-demand billing mode |
| API errors | Medium | Medium | Comprehensive error handling |
| Learning curve | Medium | High | Reference AWS documentation |

---

## 9. Benefits & Learning Outcomes

### For Professional Growth
- Industry-standard serverless architecture
- AWS certification preparation
- Portfolio-worthy project
- Cloud-native best practices

### For Organization
- Low-cost, maintainable infrastructure
- Scalable to millions of users
- Built-in monitoring and logging
- Easy to modify or extend

---

## 10. Budget & Resource Allocation

### AWS Resources
- API Gateway: Free Tier eligible
- Lambda: Free Tier (1M invocations)
- DynamoDB: Free Tier (25 GB)
- **Estimated Cost**: < $1 USD

### Development Resources
- Time: 40 hours (3 hours/week × 12 weeks)
- Tools: VS Code, AWS CLI, Postman
- Documentation: Markdown, Diagrams

---

**Proposal Status**: ✅ Approved for Implementation
