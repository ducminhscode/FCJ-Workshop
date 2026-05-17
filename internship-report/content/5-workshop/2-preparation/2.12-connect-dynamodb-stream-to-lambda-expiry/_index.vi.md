---
title : "Kết nối DynamoDB Stream đến Lambda Expiry"
date :  "`r Sys.Date()`" 
weight : 12
pre: <b> 5.2.12 </b>
chapter : false
---

1. **Truy cập:**
- Vào giao diện **AWS Console** và tìm kiếm "Lambda" trên thanh tìm kiếm.

![5.2.12-1](/images/5.2.12-1.png)

- Chọn **Functions** từ menu bên trái > Chọn `pa-{env}-expiry-{region}`.

![5.2.12-2](/images/5.2.12-2.png)

- Vào tab **Configuration** > Chọn **Triggers** > Chọn **Add trigger**.

![5.2.12-3](/images/5.2.12-3.png)

2. **Cấu hình:**
- **Trigger configuration:** Chọn **DynamoDB**.
- **DynamoDB table:** Chọn `pa-{env}-access-sessions-{region}`.
- **Batch size:** `100`.
- **Starting position:** Chọn **Latest**.
- **Batch window:** `10 seconds`.
- **Retry attempts:** `3`.
- **On failure destination:** None.
- **Thêm Filter criteria:** 

```
{
  "eventName": ["REMOVE"],
  "userIdentity": {
    "type": ["Service"],
    "principalId": ["dynamodb.amazonaws.com"]
  }
}
```

*Filter này đảm bảo Lambda Expiry chỉ được trigger khi DynamoDB TTL service tự động xóa record (không phải khi ai đó xóa thủ công).*

- Chọn **Add**.

![5.2.12-4](/images/5.2.12-4.png)

![5.2.12-5](/images/5.2.12-5.png)

![5.2.12-6](/images/5.2.12-6.png)

![5.2.12-7](/images/5.2.12-7.png)