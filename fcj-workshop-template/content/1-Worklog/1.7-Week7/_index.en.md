---
title: "Week 7 Worklog"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives

- Implement the Lambda CRUD functions.
- Apply a DynamoDB single-table access pattern in the group project.

### Work Performed This Week

| Day | Work performed | Start date | Completion date | Reference |
|---|---|---|---|---|
| Monday | Studied *Building Serverless CRUD with Lambda and DynamoDB*.<br>Completed the lab and compared it with the group implementation. | 01/06/2026 | 01/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tuesday | Implemented `createWorkflow.ts` to parse JSON and write to DynamoDB.<br>Debugged an API Gateway body-parsing issue. | 02/06/2026 | 02/06/2026 | <https://github.com/cheduc1234/duan> |
| Wednesday | Implemented `getTodos.ts` using `PK = USER#<userId>`.<br>Studied the AWS SDK for JavaScript v3 DynamoDB DocumentClient. | 03/06/2026 | 03/06/2026 | |
| Thursday | Implemented `updateTodo.ts` and `deleteTodo.ts`.<br>Added a `404` response when the target item could not be found. | 04/06/2026 | 04/06/2026 | |
| Friday | Performed manual tests with curl against the local SAM API.<br>Pushed the code and opened a pull request for team review. | 05/06/2026 | 05/06/2026 | |

### Week 7 Results

- Completed four basic CRUD Lambda handlers and improved TypeScript familiarity.
- Applied partition-key and sort-key concepts rather than a relational table model.
- Used execution logs to debug request parsing and handler behavior.
- Resolved issues involving body parsing and CORS.
- Reached the milestone of running the API locally and submitting the work for review.
