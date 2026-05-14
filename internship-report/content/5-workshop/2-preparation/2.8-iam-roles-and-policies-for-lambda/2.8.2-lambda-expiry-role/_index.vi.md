---
title : "Lambda Expiry Role"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.8.2 </b>
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

![5.2.8.2-1](/images/5.2.8.2-1.png)

3. **Bước 2:** Chọn **Next**.

![5.2.8.2-2](/images/5.2.8.2-2.png)

4. **Bước 3:**

- **Role name:** `pa-{env}-lambda-expiry-role`.
- **Description:** `IAM role for Lambda Expiry function`.
- **Tags:**

| Key | Value |
|:---:|:-----:|
| Project | production-access-portal |
| Env     | {env}                    |

- Chọn **Create role**.

![5.2.8.2-3](/images/5.2.8.2-3.png)

Sau khi tạo role, chọn role vừa tạo > Vào tab **Permissions** > Chọn **Add permissions** > Chọn **Create inline policy**.

![5.2.8.2-4](/images/5.2.8.2-4.png)

Chọn tab **JSON** và dán policy bên dưới vào:

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
Chọn **Next** > Đặt **Policy name** là `pa-{env}-lambda-expiry-streams` > Chọn **Create policy**.

![5.2.8.2-5](/images/5.2.8.2-5.png)

Làm tương tự với **Policy name** là `pa-{env}-lambda-expiry-secrets`.

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

Làm tương tự với **Policy name** là `pa-{env}-lambda-expiry-logs`.

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

Làm tương tự với **Policy name** là `pa-{env}-lambda-expiry-ses-{region}`.

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

Làm tương tự với các **Policy name** là `pa-{env}-lambda-expiry-jit-revoke`.

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

Sau khi hoàn tất 6 policy ta sẽ được như bên hình dưới:

![5.2.8.2-10](/images/5.2.8.2-10.png)

Và lưu ARN này để dùng ở bước Lambda Functions: `arn:aws:iam::{account_id}:role/pa-{env}-lambda-expiry-role`.