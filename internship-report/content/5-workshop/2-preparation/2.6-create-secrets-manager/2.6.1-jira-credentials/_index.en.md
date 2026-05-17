---
title : "Jira Credentials"
date :  "`r Sys.Date()`" 
weight : 1
pre: <b> 5.2.6.1 </b>
chapter : false
---

This secret is used by Lambda to call the Jira REST API in order to: 
- Retrieve ticket information.
- Update request status.
- Comment on provisioning results.
- Synchronize workflows between Jira and AWS.

1. **Access:**
- Go to the **AWS Console** interface and search for "Secrets Manager" in the search bar.

![5.2.6-1](/images/5.2.6-1.png)

- Select **Store a new secret**.

![5.2.6.1-1](/images/5.2.6.1-1.png)

2. **Configuration:**
- In the first step:

**Secret type:** Select **Other type of secret**.

**Key/value pairs:** Select the **Plaintext** tab and paste the following content below:

```
{
  "base_url": "https://your-company.atlassian.net",
  "api_token": "YOUR_JIRA_API_TOKEN",
  "user_email": "jira-service-account@company.com"
}
```

**Explanation:** `base_url`: Jira Cloud URL, `api_token`: API Token generated from an Atlassian Account, `user_email`: Service account email used to call the Jira API.

**Encryption key:** Keep the default value `aws/secretsmanager`.

Select **Next**.

![5.2.6.1-2](/images/5.2.6.1-2.png)

- In the second step:

**Secret name:** `pa-{env}-jira-credentials-{region}`.

**Description:** `Jira API credentials for Production Access Portal`.

**Add Tags:**

| Key | Value |
|:---:|:-----:|
| Project | production-access-portal |
| Env | {env} |
| Rotation | Manual |

Select **Next**.

![5.2.6.1-3](/images/5.2.6.1-3.png)

- In the third step:

**Rotation:** Select **Disable automatic rotation**.

Select **Next**.

Review the configured settings and click **Store**.

![5.2.6.1-4](/images/5.2.6.1-4.png)