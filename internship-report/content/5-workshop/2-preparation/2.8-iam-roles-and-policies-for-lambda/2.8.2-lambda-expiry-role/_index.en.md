---
title : "Lambda Expiry Role"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.8.2 </b>
chapter : false
---


1. **Access:**
- Open the **AWS Console** interface and search for "IAM" in the search bar.

![5.2.8-1](/images/5.2.8-1.png)

- Select **Roles** from the left menu > Select **Create role**.

![5.2.8-2](/images/5.2.8-2.png)

2. **Step 1:**
- **Trusted entity type:** Select **AWS service**.
- **Use case:** Select **Lambda**.
- Select **Next**.

![5.2.8.2-1](/images/5.2.8.2-1.png)

3. **Step 2:** Select **Next**.

![5.2.8.2-2](/images/5.2.8.2-2.png)

4. **Step 3:**
- **Role name:** `pa-{env}-lambda-expiry-role`.
- **Description:** `IAM role for Lambda Expiry function`.
- **Tags:**

| Key | Value |
|:---:|:-----:|
| Project | production-access-portal |
| Env     | {env}                    |

- Select **Create role**.

![5.2.8.2-3](/images/5.2.8.2-3.png)

After creating the role, select the newly created role > Go to the **Permissions** tab > Select **Add permissions** > Select **Create inline policy**.

![5.2.8.2-4](/images/5.2.8.2-4.png)

Select the **JSON** tab and paste the policy below:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:DescribeStream",
        "dynamodb:GetRecords",
        "dynamodb:GetShardIterator",
        "dynamodb:ListStreams"
      ],
      "Resource": "arn:aws:dynamodb:{region}:{account_id}:table/pa-{env}-access-sessions-{region}/stream/*"
    }
  ]
}
```
Select **Next** > Set **Policy name** to `pa-{env}-lambda-expiry-streams` > Select **Create policy**.

![5.2.8.2-5](/images/5.2.8.2-5.png)

Repeat the same steps with **Policy name** set to `pa-{env}-lambda-expiry-secrets`.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue"
      ],
      "Resource": "arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-jira-credentials-{region}-*"
    }
  ]
}
```

![5.2.8.2-6](/images/5.2.8.2-6.png)

![5.2.8.2-7](/images/5.2.8.2-7.png)

Repeat the same steps with **Policy name** set to `pa-{env}-lambda-expiry-logs`.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:{region}:{account_id}:log-group:/aws/lambda/pa-{env}-expiry-{region}:*"
    }
  ]
}
```

![5.2.8.2-8](/images/5.2.8.2-8.png)

Repeat the same steps with **Policy name** set to `pa-{env}-lambda-expiry-ses-{region}`.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ses:SendEmail",
        "ses:SendRawEmail"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ses:FromAddress": "YOUR_SES_SENDER_EMAIL"
        }
      }
    }
  ]
}
```

![5.2.8.2-9](/images/5.2.8.2-9.png)

Repeat the same steps with **Policy name** set to `pa-{env}-lambda-expiry-jit-revoke`.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RevokeAccountAssignments",
      "Effect": "Allow",
      "Action": [
        "sso:DeleteAccountAssignment",
        "sso:DescribeAccountAssignmentDeletionStatus"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ReadIdentityStore",
      "Effect": "Allow",
      "Action": [
        "identitystore:ListUsers",
        "identitystore:DescribeUser",
        "identitystore:ListGroups",
        "identitystore:DescribeGroup",
        "identitystore:ListGroupMemberships",
        "identitystore:DescribeGroupMembership"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ManageGroupMemberships",
      "Effect": "Allow",
      "Action": [
        "identitystore:DeleteGroupMembership"
      ],
      "Resource": "*"
    }
  ]
}
```

**Policy name:** `pa-{env}-lambda-identity-store-update`.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "identitystore:UpdateUser",
        "identitystore:DescribeUser"
      ],
      "Resource": "*"
    }
  ]
}
```

After completing the 6 policies, the result will look like the image below:

![5.2.8.2-10](/images/5.2.8.2-10.png)

And save this ARN to use in the Lambda Functions step: `arn:aws:iam::{account_id}:role/pa-{env}-lambda-expiry-role`.