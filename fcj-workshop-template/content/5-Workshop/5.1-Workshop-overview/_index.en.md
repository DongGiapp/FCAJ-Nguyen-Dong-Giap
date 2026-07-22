---
title: "Workshop Scope and Architecture"
date: 2026-07-22
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Serverless Todo API Workshop

## Purpose

This workshop demonstrates the minimum request path of a serverless CRUD API: API Gateway receives an HTTP request, a Lambda function applies business logic, and DynamoDB persists the result. It is designed as a learning baseline that can be deployed, tested, observed, and removed within a controlled AWS account.

## Implemented Baseline

{{<mermaid align="center">}}
flowchart LR
    C["Postman or API client"] -->|"HTTPS request"| A["Amazon API Gateway REST API"]
    A -->|"Route by method and path"| L["Four AWS Lambda functions"]
    L -->|"CRUD operations"| D["Amazon DynamoDB todos table"]
    A -.->|"Access metrics"| W["Amazon CloudWatch"]
    L -.->|"Execution logs and metrics"| W
{{< /mermaid >}}

| Component | Workshop responsibility |
|---|---|
| API Gateway | Exposes `POST /todos`, `GET /todos`, `PUT /todos/{todoId}`, and `DELETE /todos/{todoId}` |
| Lambda | Separates create, list, update, and delete handlers |
| DynamoDB | Persists Todo items using `todoId` as the workshop key |
| IAM | Allows Lambda to access the table and publish logs |
| CloudWatch | Captures Lambda logs and service metrics for troubleshooting |

## Learning Outcomes

After completing the workshop, a learner should be able to:

- Explain the synchronous request flow and service responsibilities.
- Create and test four CRUD routes.
- Validate success and failure responses with Postman.
- Locate Lambda and API metrics in CloudWatch.
- Explain why IAM roles should be scoped to required actions and resources.
- Remove all workshop resources and confirm cleanup.

## Definition of Done for the Workshop

- [ ] Each endpoint returns the documented status code for its happy path.
- [ ] Invalid or incomplete requests produce controlled 4xx responses.
- [ ] Lambda logs contain a request identifier without exposing credentials.
- [ ] IAM policy scope is reviewed before testing.
- [ ] Test evidence records request, response, timestamp, and result.
- [ ] Cleanup verification confirms that billable workshop resources are removed.

## Important Production Limitations

The workshop instructions use manual console configuration and a simplified table key. As written, they do **not** prove production readiness. Before production use, the following changes are mandatory:

1. Enforce Cognito JWT authorization and per-user ownership.
2. Replace DynamoDB `Scan` with a paginated, per-user `Query` data model.
3. Move all resources and IAM policies into AWS SAM/CloudFormation.
4. Add input schemas, safe error handling, controlled CORS, throttling, alarms, and finite log retention.
5. Enable DynamoDB point-in-time recovery and test restore and rollback.
6. Run automated security, negative, performance, and deployment tests.

See [Solution Architecture and Production Readiness](../../2-Proposal/) for the target design, Well-Architected assessment, risks, SLOs, and acceptance gates.

## Workshop Sequence

1. Review prerequisites, account safety, permissions, and budget controls.
2. Create the DynamoDB workshop table.
3. Create and test the Lambda handlers.
4. Configure API Gateway routes and validate the API.
5. Inspect CloudWatch telemetry and record evidence.
6. Delete resources and verify cleanup.

The value of this workshop is not the number of services used; it is the ability to explain the design decisions, reproduce the deployment, test failure behavior, and identify what must change for production.
