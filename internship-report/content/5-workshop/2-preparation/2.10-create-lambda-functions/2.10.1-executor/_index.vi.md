---
title : "Executor"
date :  "`r Sys.Date()`" 
weight : 1
pre: <b> 5.2.10.1 </b>
chapter : false
---

1. **Truy cập:**

- Vào giao diện **AWS Console** và tìm kiếm "Lambda" trên thanh tìm kiếm.

![5.2.9-1](/images/5.2.9-1.png)

- Chọn **Functions** từ menu bên trái > Chọn **Create function**.

![5.2.10-1](/images/5.2.10-1.png)

2. **Cấu hình:**

- Chọn **Author from scratch**.
- **Function name:** `pa-{env}-executor-{region}`.
- **Runtime:** Chọn **Python 3.12**.
- **Architecture:** Chọn **x86_64**.
- **Execution role:** Chọn **Use another role** > Select `pa-{env}-lambda-executor-role-{region}`.
- Chọn **Create function**.

![5.2.10.1-1](/images/5.2.10.1-1.png)

![5.2.10.1-2](/images/5.2.10.1-2.png)

3. **Tải lên package:**

- Trong lambda vừa tạo, vào tab **Code** > Chọn **Upload from** > Chọn **.zip file**.

![5.2.10.1-3](/images/5.2.10.1-3.png)

![5.2.10.1-4](/images/5.2.10.1-4.png)

- Chọn file để tải lên đã tải xuống ở **bước 5.2.9** > Chọn tên file **executor.zip** > Chọn **Save**.

![5.2.10.1-5](/images/5.2.10.1-5.png)

![5.2.10.1-6](/images/5.2.10.1-6.png)

4. **Đính kèm Lambda Layer:**

- Scroll xuống phần **Layers** > Chọn **Edit** > Chọn **Add a layer**.

![5.2.10.1-7](/images/5.2.10.1-7.png)

![5.2.10.1-8](/images/5.2.10.1-8.png)

- **Layers source:** Chọn **Custom layers**.
- **Custom layers:** Chọn `pa-{env}-dependencies-{region}`.
- **Version:** Chọn **1**.
- Chọn **Add**.

![5.2.10.1-9](/images/5.2.10.1-9.png)

![5.2.10.1-10](/images/5.2.10.1-10.png)

- Chọn **Save**.

![5.2.10.1-11](/images/5.2.10.1-11.png)

5. **Runtime Settings:**

- Ở phần **Runtime settings**, chọn **Edit**.

![5.2.10.1-12](/images/5.2.10.1-12.png)

- **Handler:** `lambda_functions.executor.handler.lambda_handler`.
- Chọn **Save**.

![5.2.10.1-13](/images/5.2.10.1-13.png)

6. **General Configuration:**

- Vào tab **Configuration** > Chọn **General configuration** > Chọn **Edit**.

![5.2.10.1-14](/images/5.2.10.1-14.png)

- **Memory:** `256 MB`.
- **Timeout:** `30 seconds`.
- Chọn **Save**.

![5.2.10.1-15](/images/5.2.10.1-15.png)

7. **Environment Variables:**

- Vào tab **Configuration** > Chọn **Environment variables** > Chọn **Edit**.

![5.2.10.1-16](/images/5.2.10.1-16.png)

- Thêm các biến sau:

| Key | Value |
|-----|-------|
| ENVIRONMENT | {env} |
| LOG_LEVEL | INFO |
| DYNAMODB_TABLE_NAME | pa-{env}-access-sessions-{region} |
| JIRA_SECRET_NAME | pa-{env}-jira-credentials-{region} |
| WEBHOOK_SECRET_NAME | pa-{env}-webhook-auth-{region} |
| ACCESS_GROUP_MAPPING_SECRET_NAME | pa-{env}-access-group-mapping-{region} |
| SECRETS_REGION | {region} |
| IDENTITY_CENTER_INSTANCE_ARN | Giá trị từ **bước 5.2.1** |
| IDENTITY_STORE_ID | Giá trị từ **bước 5.2.1** |
| IDENTITY_CENTER_PORTAL_URL | https://{identity_store_id}.awsapps.com/start |
| READONLY_PERMISSION_SET_ARN | ARN legacy ProdAccessReadOnly |
| POWERUSER_PERMISSION_SET_ARN | ARN legacy ProdAccessPowerUser |
| ADMIN_PERMISSION_SET_ARN | ARN legacy ProdAccessAdmin |
| USE_GROUP_BASED_ACCESS | true |
| SES_SENDER_EMAIL | Email đã verify trong SES |
| ACCOUNT_MAPPING | {"sit-app":"xxxxxxxxxxxx","sit-data":"xxxxxxxxxxxx"} |
| ACCOUNT_NAME_MAPPING | {"sit-app":"application","sit-data":"data"} |

- Chọn **Save**.

![5.2.10.1-17](/images/5.2.10.1-17.png)

![5.2.10.1-18](/images/5.2.10.1-18.png)