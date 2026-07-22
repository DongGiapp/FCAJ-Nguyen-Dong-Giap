---
title: "Week 9 Worklog"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives

- Deploy the group project to AWS for the first time while monitoring cost exposure.
- Add tests and inspect operational logs in CloudWatch.

### Work Performed This Week

| Day | Work performed | Start date | Completion date | Reference |
|---|---|---|---|---|
| Monday | Studied *Monitoring Serverless Applications*.<br>Reviewed Lambda Duration and Errors metrics in CloudWatch. | 15/06/2026 | 15/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tuesday | Added unit tests under `tests/` and ran `npm test`.<br>Used `aws-sdk-client-mock` to learn how to mock DynamoDB calls. | 16/06/2026 | 16/06/2026 | <https://github.com/cheduc1234/duan> |
| Wednesday | Ran `sam build` and `sam deploy --guided`.<br>Recorded the API endpoint from the stack outputs. | 17/06/2026 | 17/06/2026 | |
| Thursday | Called the deployed API using curl and Postman.<br>Created a test workflow and verified the item in the DynamoDB console. | 18/06/2026 | 18/06/2026 | |
| Friday | Inspected Lambda logs for a `500` response in CloudWatch.<br>Corrected a defect and deployed the update. | 19/06/2026 | 19/06/2026 | |

### Week 9 Results

- Deployed the group workload to AWS and recorded a CloudFormation stack completion screenshot.
- Called an AWS-hosted endpoint rather than only the local API.
- Ran unit tests successfully and gained confidence in changing the handlers.
- Used CloudWatch Logs to investigate an application failure.
- Kept the exercise within the budget configured during Week 1.

{{% notice warning %}}
The deployment evidence belongs to the external group project. A current production endpoint, deployment log, source-controlled infrastructure, and reproducible test report are still required before making a production claim in this report.
{{% /notice %}}
