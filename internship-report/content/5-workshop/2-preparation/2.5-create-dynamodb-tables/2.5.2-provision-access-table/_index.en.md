---
title : "ProvisionAccess Table"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.5.2 </b>
chapter : false
---

This table is used to store the history of processed (provisioned) requests.

1. **Access:** 
- Go to the **AWS Console** interface and search for "DynamoDB" in the search bar.
- Select **Tables** from the left menu > Select **Create table**.

![5.2.5.2-1](/images/5.2.5.2-1.png)

2. **Configuration:**
- **Table name** (Example: `pa-{env}-approval-tokens-{region}`).
- **Partition key:** Unique ID (Example: `token_id ` - Type: String).
- **Table settings:** Select **Customize settings**.
- **Read/Write capacity settings:** Select **On-demand**.
- **Encryption at rest:** AWS managed key.
- Select **Create table**.

![5.2.5.2-2](/images/5.2.5.2-2.png)

![5.2.5.2-3](/images/5.2.5.2-3.png)

![5.2.5.2-4](/images/5.2.5.2-4.png)

3. **Configure TTL (Time to Live):**
- After the table has been created, select the table > Go to **Actions** > Select **Turn on TTL**.

![5.2.5.2-5](/images/5.2.5.2-5.png)

- Enter **TTL attribute name:** `expires_at` > Select **Turn on TTL**.

*Note: It is NOT necessary to enable Streams for this table.*

![5.2.5.2-6](/images/5.2.5.2-6.png)

![5.2.5.2-7](/images/5.2.5.2-7.png)