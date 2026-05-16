---
title : "Lambda Email Approval Role"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.2.8.3 </b>
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

![5.2.8.3-1](/images/5.2.8.3-1.png)

3. **Bước 2:** Chọn **Next**.

![5.2.8.3-2](/images/5.2.8.3-2.png)

4. **Bước 3:**
- **Role name:** `pa-{env}-email-approval-role-{region}`.
- **Description:** `IAM role for Lambda Email Approval function`.
- **Tags:**

| Key | Value |
|:---:|:-----:|
| Project | production-access-portal |
| Env     | {env}                    |

- Chọn **Create role**.

![5.2.8.3-3](/images/5.2.8.3-3.png)

Thực hiện tạo các policy tương tự như các bước đã làm ở **Lambda Expiry Role** hoặc **Lambda Executor Role**.

Các policy:

- **Policy name:** `pa-{env}-lambda-email-approval-role-{region}`.

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
      "Resource": "*"
    }
  ]
}
```

- **Policy name:** `pa-{env}-dynamodb`.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem",
        "dynamodb:GetItem",
        "dynamodb:UpdateItem",
        "dynamodb:Query"
      ],
      "Resource": "arn:aws:dynamodb:{region}:{account_id}:table/pa-{env}-approval-tokens-{region}"
    }
  ]
}
```

- **Policy name:** `pa-{env}-secrets-manager`.

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
        "arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-token-secret-{region}-*",
        "arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-webhook-auth-{region}-*"
      ]
    }
  ]
}
```

- **Policy name:** `pa-{env}-cloudwatch-logs`.

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
      "Resource": "arn:aws:logs:{region}:{account_id}:log-group:/aws/lambda/pa-{env}-email-approval-{region}:*"
    }
  ]
}
```

Sau khi hoàn tất 4 policy ta sẽ được như bên hình dưới:

![5.2.8.3-4](/images/5.2.8.3-4.png)

Và lưu ARN này để dùng ở bước Lambda Functions: `arn:aws:iam::{account_id}:role/pa-{env}-email-approval-role-{region}`.