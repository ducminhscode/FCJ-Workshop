---
title : "Token Secret (Email Approval)"
date :  "`r Sys.Date()`" 
weight : 4
pre: <b> 5.2.6.4 </b>
chapter : false
---

Secret này chứa HMAC signing key dùng để:

- Ký email approval token.
- Validate approval links.
- Ngăn giả mạo approval request.

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
  "secret_key": "YOUR_HMAC_SECRET_KEY_HERE"
}
```

**Cách tạo** `secret_key`**:** Có thể generate bằng Python với câu lệnh bên dưới.

```
python -c "import secrets; print(secrets.token_urlsafe(64))"
```

**Khuyến nghị:**

- Không dùng secret ngắn.
- Không tái sử dụng JWT secret cũ.
- Không chia sẻ secret qua chat/email.

**Encryption key:** Giữ mặc định `aws/secretsmanager`.

Chọn **Next**.

![5.2.6.4-1](/images/5.2.6.4-1.png)

- Ở bước thứ 2:

**Secret name:** `pa-{env}-token-secret-{region}`.

**Description:** `HMAC secret key for email approval workflow token generation`.

Chọn **Next**.

![5.2.6.4-2](/images/5.2.6.4-2.png)

- Ở bước thứ 3:

**Rotation:** Chọn **Disable automatic rotation**.

Chọn **Next**.

Kiểm tra lại những gì đã cấu hình và nhấn **Store**.

![5.2.6.4-3](/images/5.2.6.4-3.png)

![5.2.6.4-4](/images/5.2.6.4-4.png)