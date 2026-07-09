---
title: "Prerequisites"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Prerequisites

## AWS Account Setup

You need an AWS account with appropriate permissions to create API Gateway, Lambda, and DynamoDB resources.

### Required Permissions
- API Gateway: Create and manage REST APIs
- Lambda: Create and manage functions
- DynamoDB: Create and manage tables
- IAM: Create roles and policies
- CloudWatch: View logs

{{% notice info %}}
If you don't have an AWS account, sign up for [AWS Free Tier](https://aws.amazon.com/free/) to get started. Most of this workshop fits within the free tier limits.
{{% /notice %}}

---

## Local Requirements

### Required
- **AWS CLI**: Version 2.x or higher
  - [Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
  - Verify: `aws --version`
  
- **AWS Account Credentials**: Configured locally
  - Run: `aws configure`
  - Enter AWS Access Key ID and Secret Access Key
  - Default region: `us-east-1` (recommended)

- **Text Editor or IDE**: VS Code, PyCharm, or similar

### Optional but Recommended
- **Python 3.8+**: For testing Lambda functions locally
- **Postman** or **curl**: For testing API endpoints
- **git**: For version control

---

## AWS Services Used in This Workshop

1. **API Gateway** - REST API endpoints
2. **Lambda** - Serverless compute for business logic
3. **DynamoDB** - NoSQL database for data storage

### Additional Services Used
- **IAM**: For access control and security
- **CloudWatch**: For monitoring and logs
- **AWS CloudFormation** (optional): For Infrastructure as Code

---

## Estimated Costs

This workshop should cost **less than $1 USD** if:
- Completed within 2-3 hours
- Resources are cleaned up at the end
- You're within AWS Free Tier limits (eligible accounts)

### Free Tier Coverage (12 months)
- Lambda: 1,000,000 free requests/month
- DynamoDB: 25 GB storage, 25 RCU, 25 WCU
- API Gateway: 1,000,000 API calls/month (first year)

**Important**: Always delete resources after testing to avoid unexpected charges.

---

## Time Required

**Total: 2-3 hours**
- Setup & Prerequisites: 15-20 minutes
- Creating DynamoDB Table: 10 minutes
- Writing Lambda Functions: 40-50 minutes
- Setting up API Gateway: 30-40 minutes
- Testing & Debugging: 20-30 minutes
- Cleanup: 10 minutes

---

## Next Steps

1. Create an AWS Account or ensure your existing account has the necessary permissions
2. Install and configure AWS CLI
3. Test your setup with: `aws s3 ls`
4. Proceed to [Setup DynamoDB Table](../5.3-S3-vpc/)

---

{{% notice warning %}}
Do NOT commit AWS credentials to version control. Use AWS CLI configuration files in `~/.aws/credentials` instead.
{{% /notice %}}

