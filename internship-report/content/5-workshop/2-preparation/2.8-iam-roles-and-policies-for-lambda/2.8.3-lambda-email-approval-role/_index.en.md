---
title : "Lambda Email Approval Role"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.2.8.3 </b>
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

![5.2.8.3-1](/images/5.2.8.3-1.png)

3. **Step 2:** Select **Next**.

![5.2.8.3-2](/images/5.2.8.3-2.png)

4. **Step 3:**
- **Role name:** `pa-{env}-email-approval-role-{region}`.
- **Description:** `IAM role for Lambda Email Approval function`.
- **Tags:**

| Key | Value |
|:---:|:-----:|
| Project | production-access-portal |
| Env     | {env}                    |

- Select **Create role**.

![5.2.8.3-3](/images/5.2.8.3-3.png)

Create the policies similarly to the steps performed in **Lambda Expiry Role** or **Lambda Executor Role**.

Policies:

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

After completing the 4 policies, the result will look like the image below:

![5.2.8.3-4](/images/5.2.8.3-4.png)

And save this ARN to use in the Lambda Functions step: `arn:aws:iam::{account_id}:role/pa-{env}-email-approval-role-{region}`.