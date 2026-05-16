---
title : "Lambda Executor Role"
date :  "`r Sys.Date()`" 
weight : 1
pre: <b> 5.2.8.1 </b>
chapter : false
---

1. **Truy cập:**
- Vào giao diện **AWS Console** và tìm kiếm "IAM" trên thanh tìm kiếm.

![5.2.8-1](/images/5.2.8-1.png)

- Chọn **Roles** từ menu bên trái > Chọn **Create role**.

![5.2.8-2](/images/5.2.8-2.png)

2. **Bước 1:**
- **Trusted entity type:** Chọn **AWS service**.
- **Use case:** Chọn **Lambda**.
- Chọn **Next**.

![5.2.8.1-1](/images/5.2.8.1-1.png)

3. **Bước 2:** Chọn **Next**.

![5.2.8.1-2](/images/5.2.8.1-2.png)

4. **Bước 3:**
- **Role name:** `pa-{env}-lambda-executor-role-{region}`.
- **Description:** `IAM role for Lambda Executor function`.
- **Tags:**

| Key | Value |
|:---:|:-----:|
| Project | production-access-portal |
| Env     | {env}                    |

- Chọn **Create role**.

![5.2.8.1-3](/images/5.2.8.1-3.png)

![5.2.8.1-4](/images/5.2.8.1-4.png)

Sau khi tạo role, chọn role vừa tạo > Vào tab **Permissions** > Chọn **Add permissions** > Chọn **Create inline policy**.

![5.2.8.1-5](/images/5.2.8.1-5.png)

Chọn tab **JSON** và dán policy bên dưới vào:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue"
      ],
      "Resource": [
        "arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-jira-credentials-{region}-*",
        "arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-webhook-auth-{region}-*",
        "arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-access-group-mapping-{region}-*"
      ]
    }
  ]
}
```

Chọn **Next** > Đặt **Policy name** là `pa-{env}-lambda-executor-secrets-{region}` > Chọn **Create policy**.

![5.2.8.1-6](/images/5.2.8.1-6.png)

Làm tương tự với **Policy name** là `pa-{env}-lambda-executor-dynamodb`.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem"
      ],
      "Resource": "arn:aws:dynamodb:{region}:{account_id}:table/pa-{env}-access-sessions-{region}"
    }
  ]
}
```

![5.2.8.1-7](/images/5.2.8.1-7.png)

Làm tương tự với **Policy name** là `pa-{env}-lambda-executor-logs`.

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
      "Resource": "arn:aws:logs:{region}:{account_id}:log-group:/aws/lambda/pa-{env}-executor-{region}:*"
    }
  ]
}
```

![5.2.8.1-8](/images/5.2.8.1-8.png)

![5.2.8.1-9](/images/5.2.8.1-9.png)

Làm tương tự với các **Policy name** là `pa-{env}-lambda-executor-ses-{region}`.

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

**Policy name:** `pa-{env}-lambda-executor-jit-access`.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ManageAccountAssignments",
      "Effect": "Allow",
      "Action": [
        "sso:CreateAccountAssignment",
        "sso:DescribeAccountAssignmentCreationStatus",
        "sso:DeleteAccountAssignment",
        "sso:ListAccountAssignments",
        "sso:ListPermissionSets"
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
        "identitystore:CreateGroupMembership",
        "identitystore:DeleteGroupMembership"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ReadSAMLProvider",
      "Effect": "Allow",
      "Action": [
        "iam:GetSAMLProvider",
        "iam:GetRole",
        "iam:ListAttachedRolePolicies",
        "iam:ListRolePolicies"
      ],
      "Resource": [
        "arn:aws:iam::*:saml-provider/AWSSSO_*",
        "arn:aws:iam::*:role/aws-reserved/sso.amazonaws.com/*/AWSReservedSSO_*"
      ]
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
      "Sid": "UpdateUserAttributes",
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

Sau khi hoàn tất 6 policy ta sẽ được như bên hình dưới:

![5.2.8.1-10](/images/5.2.8.1-10.png)

Và lưu ARN này để dùng ở bước Lambda Functions: `arn:aws:iam::{account_id}:role/pa-{env}-lambda-executor-role-{region}`.