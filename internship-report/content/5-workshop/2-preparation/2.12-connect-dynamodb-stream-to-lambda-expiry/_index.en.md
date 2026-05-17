---
title : "Connect DynamoDB Stream to Lambda Expiry"
date :  "`r Sys.Date()`" 
weight : 12
pre: <b> 5.2.12 </b>
chapter : false
---

1. **Access:**
- Go to the **AWS Console** interface and search for "Lambda" in the search bar.

![5.2.12-1](/images/5.2.12-1.png)

- Select **Functions** from the left menu > Select `pa-{env}-expiry-{region}`.

![5.2.12-2](/images/5.2.12-2.png)

- Go to the **Configuration** tab > Select **Triggers** > Select **Add trigger**.

![5.2.12-3](/images/5.2.12-3.png)

2. **Configuration:**
- **Trigger configuration:** Select **DynamoDB**.
- **DynamoDB table:** Select `pa-{env}-access-sessions-{region}`.
- **Batch size:** `100`.
- **Starting position:** Select **Latest**.
- **Batch window:** `10 seconds`.
- **Retry attempts:** `3`.
- **On failure destination:** None.
- **Add Filter criteria:** 

```
{
  "eventName": ["REMOVE"],
  "userIdentity": {
    "type": ["Service"],
    "principalId": ["dynamodb.amazonaws.com"]
  }
}
```

*This filter ensures that the Expiry Lambda is only triggered when the DynamoDB TTL service automatically deletes a record (not when someone manually deletes it).*

- Select **Add**.

![5.2.12-4](/images/5.2.12-4.png)

![5.2.12-5](/images/5.2.12-5.png)

![5.2.12-6](/images/5.2.12-6.png)

![5.2.12-7](/images/5.2.12-7.png)