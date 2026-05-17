---
title : "Webhook Auth"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.6.2 </b>
chapter : false
---

This secret contains the API Key used to authenticate requests from Jira Automation or external systems calling the API Gateway.

1. **Access:**
- Go to the **AWS Console** interface and search for "Secrets Manager" in the search bar.

![5.2.6-1](/images/5.2.6-1.png)

- Select **Store a new secret**.

![5.2.6.2-1](/images/5.2.6.2-1.png)

2. **Configuration:**
- In the first step:

**Secret type:** Select **Other type of secret**.

**Key/value pairs:** Select the **Plaintext** tab and paste the following content below:

```
{
  "api_key": "YOUR_SECURE_API_KEY_HERE"
}
```

**How to generate** `api_key`**:** You can generate it using Python with the command below.

```
python -c "import secrets; print(secrets.token_urlsafe(48))"
```

**Recommendations:**
- Minimum 32 characters.
- Do not use simple passwords.
- Do not commit the API key to a Git repository.

**Encryption key:** Keep the default value `aws/secretsmanager`.

Select **Next**.

![5.2.6.2-2](/images/5.2.6.2-2.png)

- In the second step:

**Secret name:** `pa-{env}-webhook-auth-{region}`.

**Description:** `Webhook authentication secrets for API Gateway`.

**Add Tags:**

| Key | Value |
|:---:|:-----:|
| Project | production-access-portal |
| Env | {env} |
| Rotation | Manual |

Select **Next**.

![5.2.6.2-3](/images/5.2.6.2-3.png)

- In the third step:

**Rotation:** Select **Disable automatic rotation**.

Select **Next**.

Review the configured settings and click **Store**.

![5.2.6.2-4](/images/5.2.6.2-4.png)

![5.2.6.2-5](/images/5.2.6.2-5.png)