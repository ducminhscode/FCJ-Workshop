---
title : "Create Secrets Manager"
date :  "`r Sys.Date()`" 
weight : 6
pre: <b> 5.2.6 </b>
chapter : false
---

The system uses AWS Secrets Manager to store sensitive information such as Jira API Token, webhook authentication API Key, HMAC signing secret and Access Group Mapping.

Using Secrets Manager helps:

- Avoid hardcoding credentials in source code or Lambda environment variables.
- Easily rotate secrets when needed.
- Control access permissions to secrets through IAM Policies.
- Audit secret access through CloudTrail.

### Overview of the Secrets to Create

| No. | Secret Name | Purpose |
|:---:|:-----------:|----------|
| 1 | pa-{env}-jira-credentials-{region} | Store Jira API credential information |
| 2 | pa-{env}-webhook-auth-{region} | Authenticate API key for requests from Webhook |
| 3 | pa-{env}-access-group-mapping-{region} | Mapping between Group ID and Permissions |
| 4 | pa-{env}-token-secret-{region} | HMAC key used to generate/validate email approval tokens |

**Note:** Replace `{env}` with the corresponding environment (Example: `sit`, `uat`, `prod`) and `{region}` with the region you are deploying to (Example: `ap-southeast-1`).

![5.2.6-2](/images/5.2.6-2.png)

After creating each secret:

- Click on the secret.
- Copy the **Secret ARN** value.
- Save it for use in the **IAM Policies** and **Lambda Environment Variables** steps.

| Secret | ARN |
|--------|-----|
| Jira Credentials | arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-jira-credentials-{region}-XXXXXX |
| Webhook Auth | arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-webhook-auth-{region}-XXXXXX |
| Access Group Mapping | arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-access-group-mapping-{region}-XXXXXX |
| Token Secret | arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-token-secret-{region}-XXXXXX |

### Secrets to Create

1. [Jira Credentials](2.6.1-jira-credentials/)
2. [Webhook Auth](2.6.2-webhook-auth/)
3. [Access Group Mapping](2.6.3-access-group-mapping/)
4. [Token Secret (Email Approval)](2.6.4-token-secret/)
