---
title : "AccessSessions Table"
date :  "`r Sys.Date()`" 
weight : 1
pre: <b> 5.2.5.1 </b>
chapter : false
---

This table is used to manage user access sessions in the system.

1. **Access:** 
- Go to the **AWS Console** interface and search for "DynamoDB" in the search bar.

![5.2.5.1-1](/images/5.2.5.1-1.png)

- Select **Tables** from the left menu > Select **Create table**.

![5.2.5.1-2](/images/5.2.5.1-2.png)

2. **Configuration:**
- **Table name** (Example: `pa-{env}-access-sessions-{region}`).
- **Partition key:** Unique ID of the access session (Example: `sessionId` - Type: String).
- **Table settings:** Select **Customize settings**.
- **Read/Write capacity settings:** Select **On-demand**.
- **Secondary indexes:**
  - Select **Create global index** (1st time):
    - **Index name:** `requestId-index`.
    - **Partition key:** `requestId` - **Attribute**, `String` - **Data type**.
    - **Attribute projections:** All.
  - Select **Create global index** (2nd time):
    - **Index name:** `requester-index`.
    - **Partition key:** `requester` - **Attribute**, `String` - **Data type**.
    - **Attribute projections:** All.
- **Encryption at rest:** AWS managed key.
- Select **Create table**.

![5.2.5.1-3](/images/5.2.5.1-3.png)

![5.2.5.1-4](/images/5.2.5.1-4.png)

![5.2.5.1-5](/images/5.2.5.1-5.png)

![5.2.5.1-6](/images/5.2.5.1-6.png)

![5.2.5.1-7](/images/5.2.5.1-7.png)

![5.2.5.1-8](/images/5.2.5.1-8.png)

![5.2.5.1-20](/images/5.2.5.1-20.png)

![5.2.5.1-9](/images/5.2.5.1-9.png)

3. **Configure TTL (Time to Live):**
- After the table has been created, select the table > Go to **Actions** > Select **Turn on TTL**.

![5.2.5.1-10](/images/5.2.5.1-10.png)

![5.2.5.1-11](/images/5.2.5.1-11.png)

- Enter **TTL attribute name:** `ttl` > Select **Turn on TTL**.

*Note: This helps the system automatically clean up expired sessions to save storage space.*

![5.2.5.1-12](/images/5.2.5.1-12.png)

![5.2.5.1-13](/images/5.2.5.1-13.png)

- After the table has been created, select the table > Go to the **Exports and streams** tab.

![5.2.5.1-14](/images/5.2.5.1-14.png)

- In the **DynamoDB stream details** section, select **Turn on**.

![5.2.5.1-15](/images/5.2.5.1-15.png)

- Configure **View type:** New and old images > Select **Turn on stream**.

![5.2.5.1-16](/images/5.2.5.1-16.png)

![5.2.5.1-17](/images/5.2.5.1-17.png)

- Enable **Point-in-Time Recovery:** Go to the **Backups** tab > Select **Edit** (Point-in-time recovery (PITR)) > Tick **Turn on point-in-time recovery** > Select **Save changes**.

![5.2.5.1-18](/images/5.2.5.1-18.png)

![5.2.5.1-19](/images/5.2.5.1-19.png)

4. **Record information:** After successful creation, click on the newly created table name. Copy and save the **Amazon Resource Name (ARN)** (Example: `arn:aws:dynamodb:{region}:{account_id}:table/pa-{env}-access-sessions-{region}`), **Latest stream ARN** (Example: `arn:aws:dynamodb:{region}:{account_id}:table/pa-{env}-access-sessions-{region}/stream/...`).

![5.2.5.1-21](/images/5.2.5.1-21.png)

![5.2.5.1-22](/images/5.2.5.1-22.png)