---
title : "Webhook Auth"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.6.2 </b>
chapter : false
---

Secret này chứa API Key dùng để xác thực request từ Jira Automation hoặc các external systems gọi vào API Gateway.

1. **Truy cập:**
- Vào giao diện **AWS Console** và tìm kiếm "Secrets Manager" trên thanh tìm kiếm.

![5.2.6-1](/images/5.2.6-1.png)

- Chọn **Store a new secret**.

![5.2.6.2-1](/images/5.2.6.2-1.png)

2. **Cấu hình:**
- Ở bước đầu tiên:

**Secret type:** Chọn **Other type of secret**.

**Key/value pairs:** Chọn tab **Plaintext** và dán nội dung sau vào bên dưới:

```
{
  "api_key": "YOUR_SECURE_API_KEY_HERE"
}
```

**Cách tạo** `api_key`**:** Có thể generate bằng Python với câu lệnh bên dưới.

```
python -c "import secrets; print(secrets.token_urlsafe(48))"
```

**Khuyến nghị:**
- Tối thiểu 32 ký tự.
- Không sử dụng password đơn giản.
- Không commit API key vào Git repository.

**Encryption key:** Giữ mặc định `aws/secretsmanager`.

Chọn **Next**.

![5.2.6.2-2](/images/5.2.6.2-2.png)

- Ở bước thứ 2:

**Secret name:** `pa-{env}-webhook-auth-{region}`.

**Description:** `Webhook authentication secrets for API Gateway`.

**Thêm Tags:**

| Key | Value |
|:---:|:-----:|
| Project | production-access-portal |
| Env | {env} |
| Rotation | Manual |

Chọn **Next**.

![5.2.6.2-3](/images/5.2.6.2-3.png)

- Ở bước thứ 3:

**Rotation:** Chọn **Disable automatic rotation**.

Chọn **Next**.

Kiểm tra lại những gì đã cấu hình và nhấn **Store**.

![5.2.6.2-4](/images/5.2.6.2-4.png)

![5.2.6.2-5](/images/5.2.6.2-5.png)