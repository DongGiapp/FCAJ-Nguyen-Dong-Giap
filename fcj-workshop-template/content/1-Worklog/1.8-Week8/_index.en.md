---
title: "Week 8 Worklog"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives

- Integrate Amazon Cognito and understand JWT-based authentication.
- Require a valid token instead of leaving the group API publicly accessible.

### Work Performed This Week

| Day | Work performed | Start date | Completion date | Reference |
|---|---|---|---|---|
| Monday | Studied *Cross-Domain Authentication with Amazon Cognito*.<br>Created a User Pool and registered a test user. | 08/06/2026 | 08/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Tuesday | Studied *User Authentication with Amazon Cognito* in the Book Store series.<br>Documented the sign-in → JWT → Authorization header flow. | 09/06/2026 | 09/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Wednesday | Added a Cognito authorizer to API Gateway in `template.yaml`.<br>Reviewed the AWS SAM authentication documentation several times to understand the configuration. | 10/06/2026 | 10/06/2026 | |
| Thursday | Updated Lambda to obtain `userId` from `event.requestContext.authorizer.claims.sub`.<br>Verified that a request without a token returned `401`. | 11/06/2026 | 11/06/2026 | |
| Friday | Tested an authenticated API request with Postman.<br>Corrected a CORS issue encountered by the local frontend. | 12/06/2026 | 12/06/2026 | |

### Week 8 Results

- Created a Cognito User Pool and completed a test sign-in.
- Configured the group API to require a JWT in the tested flow.
- Derived the caller identity from the token's `sub` claim.
- Established the basic authentication flow while identifying advanced Cognito configuration as an area for further practice.
- Requested mentor and team support for the most difficult authentication issues.

{{% notice warning %}}
These activities refer to the external group-project repository. The internship report repository does not currently contain the SAM template or automated authorization tests needed to reproduce this result independently.
{{% /notice %}}
