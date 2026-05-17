---
title : "Access Group Mapping"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.2.6.3 </b>
chapter : false
---

This is the most important secret in the **Group-Based Access Architecture** mechanism. The Lambda Executor will read this secret to determine:
- Which group the user should be added to.
- Which account the group maps to.
- Which Permission Set will be granted.
- The session duration.

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
  "access_groups": {}
}
```

*At this step, only create a placeholder first.*

**Encryption key:** Keep the default value `aws/secretsmanager`.

Select **Next**.

![5.2.6.3-1](/images/5.2.6.3-1.png)

- In the second step:

**Secret name:** `pa-{env}-access-group-mapping-{region}`.

**Description:** `Access Group mapping for group-based JIT access`.

Select **Next**.

![5.2.6.3-2](/images/5.2.6.3-2.png)

- In the third step:

**Rotation:** Select **Disable automatic rotation**.

Select **Next**.

Review the configured settings and click **Store**.

![5.2.6.3-3](/images/5.2.6.3-3.png)

![5.2.6.3-4](/images/5.2.6.3-4.png)