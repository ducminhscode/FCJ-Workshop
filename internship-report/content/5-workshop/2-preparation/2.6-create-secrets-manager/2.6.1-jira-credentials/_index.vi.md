---
title : "Jira Credentials"
date :  "`r Sys.Date()`" 
weight : 1
pre: <b> 5.2.6.1 </b>
chapter : false
---

Secret này được Lambda sử dụng để gọi Jira REST API nhằm: 
- Lấy thông tin ticket
- Cập nhật trạng thái request
- Comment kết quả provisioning
- Đồng bộ workflow giữa Jira và AWS.

1. **Truy cập:**

- Vào giao diện **AWS Console** và tìm kiếm "Secrets Manager" trên thanh tìm kiếm.

![5.2.6-1](/images/5.2.6-1.png)

- Chọn **Store a new secret**.

![5.2.6.1-1](/images/5.2.6.1-1.png)

2. **Cấu hình:**

- Ở bước đầu tiên:

**Secret type:** Chọn **Other type of secret**.

**Key/value pairs:** Chọn tab **Plaintext** và dán nội dung sau vào bên dưới:

```
{
  "base_url": "https://your-company.atlassian.net",
  "api_token": "YOUR_JIRA_API_TOKEN",
  "user_email": "jira-service-account@company.com"
}
```

**Giải thích:** `base_url`: Jira Cloud URL, `api_token`: API Token tạo từ Atlassian Account, `user_email`: Email service account dùng gọi Jira API.

**Encryption key:** Giữ mặc định `aws/secretsmanager`.

Chọn **Next**.

![5.2.6.1-2](/images/5.2.6.1-2.png)

- Ở bước thứ 2:

**Secret name:** `pa-{env}-jira-credentials-{region}`.

**Description:** `Jira API credentials for Production Access Portal`.

**Thêm Tags:**

| Key | Value |
|:---:|:-----:|
| Project | production-access-portal |
| Env | {env} |
| Rotation | Manual |

Chọn **Next**.

![5.2.6.1-3](/images/5.2.6.1-3.png)

- Ở bước thứ 3:

**Rotation:** Chọn **Disable automatic rotation**.

Chọn **Next**.

Kiểm tra lại những gì đã cấu hình và nhấn **Store**.

![5.2.6.1-4](/images/5.2.6.1-4.png)