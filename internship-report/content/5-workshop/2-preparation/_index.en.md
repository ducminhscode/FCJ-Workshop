---
title : "Preparation"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2 </b>
chapter : false
---

To perform the following steps, the following requirements must be completed:
- **AWS Account:** Have access to the AWS Management Console.
- **IAM Permissions:** The account being used must have AdministratorAccess permission or at minimum administrative permissions for the following services: IAM, Lambda, DynamoDB, API Gateway and SES.
- **Working Region:** Clearly determine the Region to be deployed (For example: ap-southeast-1 - Singapore) and keep this Region consistent throughout the installation process.
- **AWS Organizations Configuration:** Ensure that the AWS IAM Identity Center (SAML 2.0) feature has been enabled if you are performing configurations related to Group Assignments.

*Note: All resource names in this document should follow a consistent naming convention for easier management. Please copy the ARN or ID values generated during the installation process exactly for use in the following steps.*

### Preparation Steps

1. [Activate Identity Center](2.1-activate-identity-center/)
2. [Create Permission Sets](2.2-create-permission-sets/)
3. [Create Access Groups](2.3-create-access-group/)
4. [Create Group Assignments](2.4-create-group-assignments/)
5. [Create DynamoDB Tables](2.5-create-dynamodb-tables/)
6. [Create Secrets Manager](2.6-create-secrets-manager/)
7. [Create Simple Email Service](2.7-create-simple-email-service/)
8. [IAM Roles & Policies for Lambda](2.8-iam-roles-and-policies-for-lambda/)
9. [Create Lambda Layer](2.9-create-lambda-layer/)
10. [Create Lambda Function](2.10-create-lambda-functions/)
11. [Set Up API Gateway](2.11-setup-api-gateway/)
12. [Connect DynamoDB Stream to Lambda Expiry](2.12-connect-dynamodb-stream-to-lambda-expiry/)
13. [Set Up CloudWatch Alarms](2.13-setup-cloudwatch-alarms/)
14. [Populate Access Group Mapping](2.14-populate-access-group-mapping/)
15. [Set Up Jira](2.15-setup-jira/)
