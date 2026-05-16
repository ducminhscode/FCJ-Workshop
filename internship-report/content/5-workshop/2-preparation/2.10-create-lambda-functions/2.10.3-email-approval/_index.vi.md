---
title : "Email Approval"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.2.10.3 </b>
chapter : false
---

1. **Truy cập:**
- Vào giao diện **AWS Console** và tìm kiếm "Lambda" trên thanh tìm kiếm.

![5.2.9-1](/images/5.2.9-1.png)

- Chọn **Functions** từ menu bên trái > Chọn **Create function**.

![5.2.10-1](/images/5.2.10-1.png)

2. **Cấu hình:**
- Chọn **Author from scratch**.
- **Function name:** `pa-{env}-email-approval-{region}`.
- **Runtime:** Chọn **Python 3.12**.
- **Architecture:** Chọn **x86_64**.
- **Execution role:** Chọn **Use another role** > Select `pa-{env}-email-approval-role-{region}`.
- Chọn **Create function**.

![5.2.10.3-1](/images/5.2.10.3-1.png)

![5.2.10.3-2](/images/5.2.10.3-2.png)

3. **Tải lên package:**
- Trong lambda vừa tạo, vào tab **Code** > Chọn **Upload from** > Chọn **.zip file**.

![5.2.10.3-3](/images/5.2.10.3-3.png)

- Chọn file để tải lên đã tải xuống ở **bước 5.2.9** > Chọn tên file **email_approval.zip** > Chọn **Save**.

![5.2.10.3-4](/images/5.2.10.3-4.png)

4. **Đính kèm Lambda Layer:**
- Scroll xuống phần **Layers** > Chọn **Edit** > Chọn **Add a layer**.

![5.2.10.3-5](/images/5.2.10.3-5.png)

![5.2.10.3-7](/images/5.2.10.3-7.png)

- **Layers source:** Chọn **Custom layers**.
- **Custom layers:** Chọn `pa-{env}-dependencies-{region}`.
- **Version:** Chọn **1**.
- Chọn **Add**.

![5.2.10.3-6](/images/5.2.10.3-6.png)

- Chọn **Save**.

![5.2.10.3-8](/images/5.2.10.3-8.png)

5. **Runtime Settings:**
- Ở phần **Runtime settings**, chọn **Edit**.

![5.2.10.3-10](/images/5.2.10.3-10.png)

- **Handler:** `lambda_functions.email_approval.handler.lambda_handler`.
- Chọn **Save**.

![5.2.10.3-9](/images/5.2.10.3-9.png)

6. **General Configuration:**
- Vào tab **Configuration** > Chọn **General configuration** > Chọn **Edit**.

![5.2.10.3-11](/images/5.2.10.3-11.png)

- **Memory:** `256 MB`.
- **Timeout:** `30 seconds`.
- Chọn **Save**.

![5.2.10.3-12](/images/5.2.10.3-12.png)

7. **Environment Variables:**
- Vào tab **Configuration** > Chọn **Environment variables** > Chọn **Edit**.

![5.2.10.3-13](/images/5.2.10.3-13.png)

- Thêm các biến sau:

| Key | Value |
|-----|-------|
| ADMIN_EMAIL | Email admin nhận approval |
| SES_SENDER_EMAIL | Email gửi |
| DYNAMODB_TOKEN_TABLE | pa-{env}-approval-tokens-{region} |
| TOKEN_SECRET_NAME | pa-{env}-token-secret-{region}     |
| LOG_LEVEL | INFO |
| JIRA_SECRET_NAME | pa-{env}-jira-credentials-{region} |
| WEBHOOK_SECRET_NAME | pa-{env}-webhook-auth-{region} |
| SECRETS_REGION | {region} |
| SES_CONFIGURATION_SET | pa-{env}-email-approval-{region} |

- Chọn **Save**.

![5.2.10.3-14](/images/5.2.10.3-14.png)

![5.2.10.3-15](/images/5.2.10.3-15.png)