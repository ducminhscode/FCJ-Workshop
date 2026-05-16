---
title : "Clean up resources"
date :  "`r Sys.Date()`" 
weight : 5
pre: <b> 5.5 </b>
chapter : false
---

## Overview

After completing the testing process or when the system is no longer needed, it is necessary to clean up all AWS resources to:

- Avoid unnecessary costs.
- Remove excessive IAM permissions.
- Ensure no APIs or credentials remain active unintentionally.
- Keep the AWS environment clean and manageable.

Recommendations:
- Perform cleanup in the correct dependency order.
- Do not delete production resources without backup or team confirmation.
- For UAT/SIT environments: snapshot or export logs before deletion.

## List of resources to delete

| Resource Group | Components |
|----------------|------------|
| API Layer | API Gateway, API Keys, Usage Plans |
| Compute | Lambda Functions, Lambda Layers |
| IAM | IAM Roles, Inline Policies |
| Storage | DynamoDB Tables |
| Identity Center | Access Groups, Group Assignments, Permission Sets |
| Security | Secrets Manager Secrets |
| Email | SES Identities, Configuration Sets |
| Monitoring | CloudWatch Log Groups, Alarms |

### Step 1 - Delete API Gateway

#### Delete REST API

1. Access **API Gateway**.
2. Select API: `pa-{env}-api-{region}`.
3. Select **Actions** > **Delete API**.
4. Enter: `delete`.
5. Confirm **Delete**.

#### Delete API Keys

1. Left menu > **API Keys**.
2. Select: `pa-{env}-jira-webhook-key-{region}`.
3. Select **Delete**.

#### Delete Usage Plans

1. Left menu > **Usage Plans**.
2. Select: `pa-{env}-usage-plan-{region}`.
3. Select **Delete**.

### Step 2 - Delete Lambda Functions

#### Delete Lambda Executor

1. Access **Lambda**.
2. Select function: `pa-{env}-executor-{region}`.
3. Select **Actions** > **Delete**.
4. Enter the function name to confirm.

#### Delete Lambda Expiry

Delete function: `pa-{env}-expiry-{region}`.

#### Delete Lambda Email Approval

Delete function: `pa-{env}-email-approval-{region}`.

### Step 3 - Delete Event Source Mappings

*Note: If the Event Source Mapping is not deleted, the DynamoDB Stream may continue invoking Lambda.*

1. **Lambda** > Select: `pa-{env}-expiry-{region}`.
2. Tab **Configuration** > **Triggers**.
3. Select the DynamoDB trigger.
4. Select **Delete**.

### Step 4 - Delete Lambda Layer

1. Access **Lambda** > **Layers**.
2. Select: `pa-{env}-dependencies-{region}`.
3. Select each version.
4. Select **Delete**.

*All versions must be deleted before the layer disappears completely.*

### Step 5 - Delete CloudWatch Logs & Alarms

#### Delete CloudWatch Alarms

Access: **CloudWatch** > **Alarms**.

Delete the alarms: `pa-{env}-executor-{region}-errors`, `pa-{env}-expiry-{region}-errors`, `pa-{env}-email-approval-{region}-errors`.

#### Delete CloudWatch Log Groups

Access: **CloudWatch** > **Log groups**.

Delete the log groups: `/aws/lambda/pa-{env}-executor-{region}`, `/aws/lambda/pa-{env}-expiry-{region}`, `/aws/lambda/pa-{env}-email-approval-{region}`.

### Step 6 - Delete IAM Roles & Policies

#### Delete Inline Policies

Before deleting roles, remove all inline policies.

**IAM** > **Roles** > Select role: `pa-{env}-lambda-executor-role-{region}`.

Tab **Permissions**: Remove all inline policies.

Repeat for: `pa-{env}-lambda-expiry-role`, `pa-{env}-email-approval-role-{region}`.

#### Delete IAM Roles

After removing policies:

1. Select the role.
2. Select **Delete**.
3. Confirm deletion.

### Step 7 - Delete Secrets Manager

Access: **Secrets Manager**.

Delete the secrets: `pa-{env}-jira-credentials-{region}`, `pa-{env}-webhook-auth-{region}`, `pa-{env}-access-group-mapping-{region}`, `pa-{env}-token-secret-{region}`.

#### Force Delete (Optional)

If you do not want to wait for the recovery period.

When deleting a secret: Disable recovery > Permanently delete.

*Cannot be restored after force deletion.*

### Step 8 - Delete DynamoDB Tables

#### Delete Access Sessions Table

1. **DynamoDB** > **Tables**.
2. Select: `pa-{env}-access-sessions-{region}`.
3. Select **Delete**.
4. Enter the table name to confirm.

#### Delete Approval Tokens Table

Delete table: `pa-{env}-approval-tokens-{region}`.

### Step 9 - Delete SES Configuration

#### Delete Configuration Set

**SES** > **Configuration sets**.

Delete: `pa-{env}-email-approval-{region}`.

#### Delete Verified Email Identity

**SES** > **Verified identities**.

Delete email identity: `SES_SENDER_EMAIL`.

*Only delete if the email is no longer used by another system.*

### Step 10 - Delete IAM Identity Center Assignments

#### Delete Group Assignments

1. **IAM Identity Center** > **AWS accounts**.
2. Select each AWS Account.
3. Remove all related assignments: `pa-{env}-application-*`, `pa-{env}-data-*`.

*Assignments must be removed before deleting groups or permission sets.*

### Step 11 - Delete Access Groups

1. **IAM Identity Center** > Groups.
2. Delete all groups:

```text
pa-{env}-application-ReadOnly-1h
pa-{env}-application-ReadOnly-4h
...
pa-{env}-data-Admin-8h
```

### Step 12 - Delete Permission Sets

1. **IAM Identity Center** > **Permission sets**.
2. Delete all:

```text
ProdAccessReadOnly1h
ProdAccessReadOnly4h
ProdAccessReadOnly8h

ProdAccessPowerUser1h
ProdAccessPowerUser4h
ProdAccessPowerUser8h

ProdAccessAdmin1h
ProdAccessAdmin4h
ProdAccessAdmin8h
```

#### Delete Legacy Permission Sets

Continue deleting:

```text
ProdAccessReadOnly
ProdAccessPowerUser
ProdAccessAdmin
```

## Conclusion

After completing all the steps above:

- The Production Access Request Portal system will be completely removed from AWS
- No temporary access permissions will remain in IAM Identity Center
- No resources generating background costs will remain
- The AWS environment will be restored to a clean state

Recommendations:
- For production environments, export CloudWatch Logs and back up DynamoDB before cleanup
- Permission Sets may be retained if redeployment is expected in the future
