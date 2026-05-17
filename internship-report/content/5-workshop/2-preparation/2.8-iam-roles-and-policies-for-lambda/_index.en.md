---
title : "IAM Roles & Policies for Lambda"
date :  "`r Sys.Date()`" 
weight : 8
pre: <b> 5.2.8 </b>
chapter : false
---

In the system architecture, each Lambda function will use a separate IAM Role to ensure:

- Separation of access permissions by function.
- Compliance with the Least Privilege principle.
- Reduced risk when a Lambda is compromised.
- Easier auditing and troubleshooting.

The system uses a total of **3 IAM Roles**:

| Lambda Function | IAM Role |
|:---------------:|:--------:|
| pa-{env}-executor-{region} | pa-{env}-lambda-executor-role-{region} |
| pa-{env}-expiry-{region} | pa-{env}-lambda-expiry-role |
| pa-{env}-email-approval-{region} | pa-{env}-email-approval-role-{region} |

### IAM Roles to Create

1. [Lambda Executor Role](2.8.1-lambda-executor-role/)
2. [Lambda Expiry Role](2.8.2-lambda-expiry-role/)
3. [Lambda Email Approval Role](2.8.3-lambda-email-approval-role/)