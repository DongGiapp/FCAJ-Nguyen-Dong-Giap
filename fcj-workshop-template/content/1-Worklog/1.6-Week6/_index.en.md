---
title: "Week 6 Worklog"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives

- Study Infrastructure as Code and begin editing the group's `template.yaml`.
- Understand the relationship between AWS SAM and AWS CloudFormation.

### Work Performed This Week

| Day | Work performed | Start date | Completion date | Reference |
|---|---|---|---|---|
| Monday | Studied the concepts in *Infrastructure as Code with AWS CloudFormation*.<br>Reviewed stacks, templates, and resources. | 25/05/2026 | 25/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tuesday | Studied *Deployment Automation with AWS SAM* in the Book Store series.<br>Followed the lab to create a sample SAM project. | 26/05/2026 | 26/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wednesday | Edited `template.yaml` to add the `todos-v2` DynamoDB table.<br>Added API Gateway and Lambda event mappings. | 27/05/2026 | 27/05/2026 | <https://github.com/cheduc1234/duan> |
| Thursday | Ran `sam build` and encountered an esbuild error.<br>Resolved the issue with assistance and documented it for future troubleshooting. | 28/05/2026 | 28/05/2026 | |
| Friday | Ran `sam local start-api` with Docker.<br>Called the local `GET /workflows` endpoint. | 29/05/2026 | 29/05/2026 | |

### Week 6 Results

- Understood that AWS SAM extends CloudFormation with concise serverless resource syntax.
- Added a DynamoDB table and basic API configuration to `template.yaml` in the group repository.
- Completed `sam build` after correcting the build issue.
- Tested the API locally without deploying every code change to AWS.
- Identified sections of the template requiring further study before independent maintenance.
