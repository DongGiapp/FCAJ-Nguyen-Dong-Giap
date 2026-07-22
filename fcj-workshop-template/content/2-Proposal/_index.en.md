---
title: "Solution Architecture"
date: 2026-07-22
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Serverless Todo API — Solution Architecture and Production Readiness

## 1. Executive Summary

| Field | Value |
|---|---|
| Project owner | Nguyen Dong Giap |
| Internship role | Cloud Engineer Intern |
| Project period | 17 April–10 July 2026 |
| Workload type | Serverless REST API |
| Primary Region | Asia Pacific (Singapore), `ap-southeast-1` |

The project demonstrates a cloud-native Todo API using Amazon API Gateway, AWS Lambda, and Amazon DynamoDB. The implemented workshop proves the core CRUD request path. This document separates that **implemented learning baseline** from the **target production architecture** required to operate the workload securely and reliably.

{{% notice warning %}}
The current repository is an internship workshop and documentation artifact. It must not be described as production-ready until the production readiness gates in Section 11 have objective evidence.
{{% /notice %}}

## 2. Business Problem and Scope

The workload provides authenticated users with an API to create, list, update, and delete their own tasks. A serverless design is appropriate because traffic is expected to be intermittent, the team should not manage servers, and managed services can scale independently.

### In scope

- RESTful CRUD operations for Todo items.
- User authentication and per-user data isolation in the target design.
- Automated infrastructure deployment and environment separation.
- Logging, metrics, alarms, backup, cost controls, and operational documentation.

### Out of scope

- Rich web or mobile user interface.
- Multi-Region active-active operation.
- Real-time collaboration and notifications.
- Regulated or highly sensitive personal data.

## 3. Current State Versus Target State

| Capability | Workshop baseline evidenced in this report | Production target |
|---|---|---|
| API | API Gateway REST resources and four CRUD routes | Versioned API, request validation, throttling, access logs, controlled CORS |
| Identity | Cognito was studied, but deployment evidence is not included | Cognito User Pool authorizer with JWT validation |
| Compute | Four Python Lambda examples | Versioned functions, aliases, bounded concurrency, structured logs, tracing |
| Data | DynamoDB table keyed only by `todoId`; list uses `Scan` | Per-user composite key and `Query`; PITR enabled |
| Permissions | IAM least privilege is stated | Route-specific execution roles verified by policy review |
| Deployment | Manual console instructions | AWS SAM/CloudFormation in source control with CI/CD and rollback |
| Operations | CloudWatch log and dashboard instructions | SLO dashboard, alarms, runbook, synthetic test and incident ownership |

This distinction prevents unsupported claims and gives reviewers a clear, auditable improvement path.

## 4. Target Architecture

{{<mermaid align="center">}}
flowchart LR
    U["Web, mobile, or API client"] -->|"1. Sign in"| C["Amazon Cognito User Pool"]
    C -->|"2. JWT"| U
    U -->|"3. HTTPS + JWT"| A["Amazon API Gateway REST API"]
    A -->|"4. Authorize and route"| L["AWS Lambda functions"]
    L -->|"5. Query or conditional write"| D["Amazon DynamoDB"]
    A -.->|"Access logs and metrics"| O["Amazon CloudWatch"]
    L -.->|"Structured logs, metrics, traces"| O
    I["AWS SAM / CloudFormation pipeline"] -->|"Versioned deployment"| A
    I -->|"Versioned deployment"| L
    I -->|"Managed configuration"| D
{{< /mermaid >}}

### Request flow

1. The user signs in through Amazon Cognito and receives a short-lived JWT.
2. The client calls API Gateway over HTTPS and sends the token.
3. API Gateway validates the token, applies request validation and throttling, then routes the request.
4. A route-specific Lambda function validates business input and derives `userId` from the verified token—not from a client-provided field.
5. Lambda reads or writes only that user's partition in DynamoDB.
6. API Gateway and Lambda publish operational telemetry to CloudWatch; alarms notify the workload owner.

### Trust boundaries

- The internet-facing API is an untrusted boundary; every request is authenticated, authorized, validated, and rate limited.
- Lambda execution roles are workload identities. They receive only the DynamoDB actions and resource scope needed by each function.
- DynamoDB is not exposed directly to clients.
- Credentials are never stored in code or documentation.

## 5. Data and API Design

### DynamoDB access patterns

Production design starts from access patterns rather than a generic table scan.

| Access pattern | Key condition |
|---|---|
| Create a Todo for a user | `PK = USER#<sub>`, `SK = TODO#<uuid>` |
| List a user's Todos | `Query PK = USER#<sub>` with pagination |
| Get, update, or delete one Todo | Exact `PK` and `SK` match |
| Filter by status at scale | Optional sparse GSI only when justified by measurements |

Suggested item attributes are `title`, `description`, `status`, `createdAt`, `updatedAt`, and `version`. Conditional expressions prevent updates or deletes of nonexistent items and support optimistic locking. API list responses use a bounded page size and an opaque continuation token.

### API contract

| Method and resource | Success | Main validation |
|---|---:|---|
| `POST /v1/todos` | `201` | Non-empty title, allowed status, payload-size limit |
| `GET /v1/todos` | `200` | Bounded `limit`, valid continuation token |
| `GET /v1/todos/{todoId}` | `200` | Valid identifier and ownership |
| `PUT /v1/todos/{todoId}` | `200` | Allowed mutable fields and ownership |
| `DELETE /v1/todos/{todoId}` | `204` | Ownership and conditional existence check |

Errors use a consistent body containing `code`, `message`, and `requestId`. Internal exception messages and stack traces are never returned to clients.

## 6. AWS Well-Architected Review

| Pillar | Design decisions and required evidence |
|---|---|
| **Operational Excellence** | Infrastructure as Code, separate `dev`/`prod` parameters, automated tests, structured JSON logs, dashboard, alarms, deployment and incident runbooks |
| **Security** | Cognito JWT authorizer, least-privilege IAM, TLS, DynamoDB encryption at rest, controlled CORS, no secrets in code, CloudTrail for account-level audit |
| **Reliability** | Managed Regional services, Lambda retries considered per invocation type, conditional writes, DynamoDB PITR, rollback procedure, quota and failure-path testing |
| **Performance Efficiency** | DynamoDB `Query` instead of `Scan`, pagination, Lambda memory tuning based on measurements, API latency and throttling metrics |
| **Cost Optimization** | On-demand capacity for uncertain low traffic, log retention, budgets and anomaly alerts, cost-allocation tags, removal of unused environments |
| **Sustainability** | Managed serverless services, right-sized Lambda memory and duration, minimal data retention, removal of unnecessary resources and duplicate processing |

The review follows the AWS Well-Architected Framework and its Serverless Applications Lens. The six pillars are a decision framework, not a certification label.

## 7. Security Baseline

- Deny unauthenticated access to all Todo routes.
- Use Cognito `sub` as the stable user identifier and enforce tenant isolation in every DynamoDB key.
- Grant `CreateTodo` only `dynamodb:PutItem`; read functions receive read actions; update and delete functions receive only their respective actions.
- Restrict policies to the production table ARN and avoid wildcard resources where the service supports tighter scope.
- Validate payload schema, length, enum values, identifiers, and pagination parameters.
- Configure explicit CORS origins for the production client; do not use `*` with credentials.
- Set API Gateway throttling and consider AWS WAF when the public workload's threat model justifies it.
- Mask sensitive values in logs and define a finite CloudWatch Logs retention period.
- Enable DynamoDB point-in-time recovery for production and test restoration.

## 8. Reliability and Operations

### Service level objectives

These are targets to validate, not achieved results.

| Indicator | Initial target | Measurement |
|---|---:|---|
| Successful API requests | ≥ 99.5% per month | API Gateway 5xx and Lambda errors |
| API latency | p95 ≤ 500 ms | API Gateway integration latency |
| Recovery point objective | ≤ 24 hours | DynamoDB backup/PITR policy |
| Recovery time objective | ≤ 4 hours | Documented restore and redeploy exercise |

### Minimum alarms

- API Gateway 5xx rate and high p95 latency.
- Lambda errors, throttles, duration near timeout, and concurrency pressure.
- DynamoDB throttled requests and system errors.
- AWS Budget threshold and Cost Anomaly Detection notification.

Each alarm must have an owner, severity, response action, and runbook link. Logging alone is not observability.

## 9. Deployment Strategy

1. Define API Gateway, Cognito, Lambda, DynamoDB, IAM, log retention, alarms, and outputs in AWS SAM/CloudFormation.
2. Run linting, unit tests, template validation, and security checks in CI.
3. Deploy automatically to `dev`; run contract and smoke tests.
4. Require approval before production deployment.
5. Publish Lambda versions and shift traffic through an alias; roll back on alarm.
6. Record the deployed stack, commit SHA, test evidence, endpoint, and deployment time.

Manual console setup remains useful for learning, but it is not the production deployment mechanism.

## 10. Verification Plan

| Test group | Required checks |
|---|---|
| Functional | CRUD happy paths, invalid payloads, missing items, pagination |
| Security | Missing/expired token, cross-user access, overlong input, CORS, IAM policy review |
| Reliability | Lambda failure, throttling response, conditional-write conflict, restore exercise |
| Performance | Defined load profile, p50/p95/p99 latency, error rate, concurrency and consumed capacity |
| Deployment | Repeatable clean deployment, smoke test, rollback, environment isolation |
| Cost | Cost estimate with assumptions, budget alarm, tag coverage, log-retention check |

Screenshots may support the evidence, but exported test results, source-controlled templates, CloudWatch metrics, and deployment logs are stronger evidence.

## 11. Production Readiness Gates

The workload can be described as production-ready only when all mandatory gates pass:

- [ ] Source-controlled SAM/CloudFormation template deploys a clean environment.
- [ ] Cognito authorization and cross-user isolation tests pass.
- [ ] IAM roles pass least-privilege review.
- [ ] Automated functional and negative tests pass in CI.
- [ ] Dashboard, alarms, log retention, and incident runbook exist.
- [ ] DynamoDB PITR is enabled and a restore exercise is documented.
- [ ] Load-test results meet the stated SLO under the declared traffic profile.
- [ ] Rollback is tested and deployment evidence is retained.
- [ ] Production endpoint and deployment status are published without exposing credentials.
- [ ] Cost estimate, budget alarm, resource tags, and cleanup ownership are verified.

**Current assessment:** workshop baseline demonstrated; production readiness not yet evidenced in this repository.

## 12. Risks and Remediation Priority

| Priority | Risk | Consequence | Remediation |
|---|---|---|---|
| P0 | Public API without enforced authentication | Unauthorized data access or modification | Add Cognito authorizer and ownership tests |
| P0 | `Scan` returns all Todo records | Data leakage, cost and latency growth | Use per-user composite keys and paginated `Query` |
| P1 | Manual infrastructure configuration | Drift and non-repeatable recovery | Add SAM/CloudFormation and CI/CD |
| P1 | No backup/restore evidence | Irrecoverable accidental deletion | Enable PITR and execute restore drill |
| P1 | No alerting or SLO evidence | Failures remain undetected | Add dashboard, alarms, owners and runbooks |
| P2 | Unsupported latency and cost claims | Misleading assessment | Report measured results with assumptions |

## 13. References

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [AWS Serverless Applications Lens](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/welcome.html)
- [RESTful microservices reference architecture](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/restful-microservices.html)
- [DynamoDB security best practices](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices-security-preventative.html)
- [DynamoDB partition-key design](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-partition-key-design.html)
- [DynamoDB backup and point-in-time recovery](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Backup-and-Restore.html)
