---
title : "Token Secret (Email Approval)"
date :  "`r Sys.Date()`" 
weight : 4
pre: <b> 5.2.6.4 </b>
chapter : false
---

This secret contains the HMAC signing key used to:
- Sign email approval tokens.
- Validate approval links.
- Prevent approval request forgery.

1. **Access:**
- Go to the **AWS Console** interface and search for "Secrets Manager" in the search bar.
- Select **Store a new secret**.

![5.2.6-1](/images/5.2.6-1.png)

2. **Configuration:**
- In the first step:

**Secret type:** Select **Other type of secret**.

**Key/value pairs:** Select the **Plaintext** tab and paste the following content below:

```
{
  "secret_key": "YOUR_HMAC_SECRET_KEY_HERE"
}
```

**How to generate** `secret_key`**:** You can generate it using Python with the command below.

```
python -c "import secrets; print(secrets.token_urlsafe(64))"
```

**Recommendations:**
- Do not use short secrets.
- Do not reuse old JWT secrets.
- Do not share secrets through chat/email.

**Encryption key:** Keep the default `aws/secretsmanager`.

Select **Next**.

![5.2.6.4-1](/images/5.2.6.4-1.png)

- In the second step:

**Secret name:** `pa-{env}-token-secret-{region}`.

**Description:** `HMAC secret key for email approval workflow token generation`.

Select **Next**.

![5.2.6.4-2](/images/5.2.6.4-2.png)

- In the third step:

**Rotation:** Select **Disable automatic rotation**.

Select **Next**.

Review the configured information and click **Store**.

![5.2.6.4-3](/images/5.2.6.4-3.png)

![5.2.6.4-4](/images/5.2.6.4-4.png)
