---
title : "Access Group Mapping"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.2.6.3 </b>
chapter : false
---

Đây là secret quan trọng nhất trong cơ chế **Group-Based Access Architecture**. Lambda Executor sẽ đọc secret này để xác định:

- User cần được add vào group nào.
- Group đó map tới account nào.
- Permission Set nào sẽ được cấp.
- Session duration bao lâu.

1. **Truy cập:**

- Vào giao diện **AWS Console** và tìm kiếm "Secrets Manager" trên thanh tìm kiếm.
- Chọn **Store a new secret**.

![5.2.6-1](/images/5.2.6-1.png)

2. **Cấu hình:**

- Ở bước đầu tiên:

**Secret type:** Chọn **Other type of secret**.

**Key/value pairs:** Chọn tab **Plaintext** và dán nội dung sau vào bên dưới:

```
{
  "access_groups": {}
}
```

*Ở bước này chỉ tạo placeholder trước.*

**Encryption key:** Giữ mặc định `aws/secretsmanager`.

Chọn **Next**.

![5.2.6.3-1](/images/5.2.6.3-1.png)

- Ở bước thứ 2:

**Secret name:** `pa-{env}-access-group-mapping-{region}`.

**Description:** `Access Group mapping for group-based JIT access`.

Chọn **Next**.

![5.2.6.3-2](/images/5.2.6.3-2.png)

- Ở bước thứ 3:

**Rotation:** Chọn **Disable automatic rotation**.

Chọn **Next**.

Kiểm tra lại những gì đã cấu hình và nhấn **Store**.

![5.2.6.3-3](/images/5.2.6.3-3.png)

![5.2.6.3-4](/images/5.2.6.3-4.png)