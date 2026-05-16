---
title : "Expiry"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.10.2 </b>
chapter : false
---

1. **Truy cập:**
- Vào giao diện **AWS Console** và tìm kiếm "Lambda" trên thanh tìm kiếm.

![5.2.9-1](/images/5.2.9-1.png)

- Chọn **Functions** từ menu bên trái > Chọn **Create function**.

![5.2.10-1](/images/5.2.10-1.png)

![5.2.10.2-1](/images/5.2.10.2-1.png)

2. **Cấu hình:**
- Chọn **Author from scratch**.
- **Function name:** `pa-{env}-expiry-{region}`.
- **Runtime:** Chọn **Python 3.12**.
- **Architecture:** Chọn **x86_64**.
- **Execution role:** Chọn **Use another role** > Select `pa-{env}-lambda-expiry-role`.
- Chọn **Create function**.

![5.2.10.2-2](/images/5.2.10.2-2.png)

![5.2.10.2-3](/images/5.2.10.2-3.png)

3. **Tải lên package:**
- Trong lambda vừa tạo, vào tab **Code** > Chọn **Upload from** > Chọn **.zip file**.

![5.2.10.2-4](/images/5.2.10.2-4.png)

- Chọn file để tải lên đã tải xuống ở **bước 5.2.9** > Chọn tên file **expiry.zip** > Chọn **Save**.

![5.2.10.2-5](/images/5.2.10.2-5.png)

4. **Đính kèm Lambda Layer:**
- Scroll xuống phần **Layers** > Chọn **Edit** > Chọn **Add a layer**.

![5.2.10.2-6](/images/5.2.10.2-6.png)

![5.2.10.2-7](/images/5.2.10.2-7.png)

- **Layers source:** Chọn **Custom layers**.
- **Custom layers:** Chọn `pa-{env}-dependencies-{region}`.
- **Version:** Chọn **1**.
- Chọn **Add**.

![5.2.10.2-8](/images/5.2.10.2-8.png)

- Chọn **Save**.

![5.2.10.2-9](/images/5.2.10.2-9.png)

5. **Runtime Settings:**
- Ở phần **Runtime settings**, chọn **Edit**.

![5.2.10.2-10](/images/5.2.10.2-10.png)

- **Handler:** `lambda_functions.expiry.handler.lambda_handler`.
- Chọn **Save**.

![5.2.10.2-11](/images/5.2.10.2-11.png)

![5.2.10.2-12](/images/5.2.10.2-12.png)

6. **General Configuration:**
- Vào tab **Configuration** > Chọn **General configuration** > Chọn **Edit**.

![5.2.10.2-13](/images/5.2.10.2-13.png)

- **Memory:** `256 MB`.
- **Timeout:** `30 seconds`.
- Chọn **Save**.

![5.2.10.2-14](/images/5.2.10.2-14.png)

7. **Environment Variables:**
- Vào tab **Configuration** > Chọn **Environment variables** > Chọn **Edit**.

![5.2.10.2-15](/images/5.2.10.2-15.png)

- Thêm các biến sau:

| Key | Value |
|-----|-------|
| ENVIRONMENT | {env} |
| LOG_LEVEL | INFO |
| JIRA_SECRET_NAME | pa-{env}-jira-credentials-{region} |
| SECRETS_REGION | {region} |
| IDENTITY_CENTER_INSTANCE_ARN | Giá trị từ **bước 5.2.1** |
| IDENTITY_STORE_ID | Giá trị từ **bước 5.2.1** |
| READONLY_PERMISSION_SET_ARN | ARN legacy ProdAccessReadOnly |
| POWERUSER_PERMISSION_SET_ARN | ARN legacy ProdAccessPowerUser |
| ADMIN_PERMISSION_SET_ARN | ARN legacy ProdAccessAdmin |
| USE_GROUP_BASED_ACCESS | true |
| SES_SENDER_EMAIL | Email đã verify trong SES |

- Chọn **Save**.

![5.2.10.2-16](/images/5.2.10.2-16.png)

![5.2.10.2-17](/images/5.2.10.2-17.png)