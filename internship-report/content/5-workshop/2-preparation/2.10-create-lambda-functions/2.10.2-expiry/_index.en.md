---
title : "Expiry"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.10.2 </b>
chapter : false
---

1. **Access:**
- Go to the **AWS Console** interface and search for "Lambda" in the search bar.

![5.2.9-1](/images/5.2.9-1.png)

- Select **Functions** from the left menu > Select **Create function**.

![5.2.10-1](/images/5.2.10-1.png)

![5.2.10.2-1](/images/5.2.10.2-1.png)

2. **Configuration:**
- Select **Author from scratch**.
- **Function name:** `pa-{env}-expiry-{region}`.
- **Runtime:** Select **Python 3.12**.
- **Architecture:** Select **x86_64**.
- **Execution role:** Select **Use another role** > Select `pa-{env}-lambda-expiry-role`.
- Select **Create function**.

![5.2.10.2-2](/images/5.2.10.2-2.png)

![5.2.10.2-3](/images/5.2.10.2-3.png)

3. **Upload package:**
- In the newly created Lambda, go to the **Code** tab > Select **Upload from** > Select **.zip file**.

![5.2.10.2-4](/images/5.2.10.2-4.png)

- Select the file to upload that was downloaded in **step 5.2.9** > Select the file named **expiry.zip** > Select **Save**.

![5.2.10.2-5](/images/5.2.10.2-5.png)

4. **Attach Lambda Layer:**
- Scroll down to the **Layers** section > Select **Edit** > Select **Add a layer**.

![5.2.10.2-6](/images/5.2.10.2-6.png)

![5.2.10.2-7](/images/5.2.10.2-7.png)

- **Layers source:** Select **Custom layers**.
- **Custom layers:** Select `pa-{env}-dependencies-{region}`.
- **Version:** Select **1**.
- Select **Add**.

![5.2.10.2-8](/images/5.2.10.2-8.png)

- Select **Save**.

![5.2.10.2-9](/images/5.2.10.2-9.png)

5. **Runtime Settings:**
- In the **Runtime settings** section, select **Edit**.

![5.2.10.2-10](/images/5.2.10.2-10.png)

- **Handler:** `lambda_functions.expiry.handler.lambda_handler`.
- Select **Save**.

![5.2.10.2-11](/images/5.2.10.2-11.png)

![5.2.10.2-12](/images/5.2.10.2-12.png)

6. **General Configuration:**
- Go to the **Configuration** tab > Select **General configuration** > Select **Edit**.

![5.2.10.2-13](/images/5.2.10.2-13.png)

- **Memory:** `256 MB`.
- **Timeout:** `30 seconds`.
- Select **Save**.

![5.2.10.2-14](/images/5.2.10.2-14.png)

7. **Environment Variables:**
- Go to the **Configuration** tab > Select **Environment variables** > Select **Edit**.

![5.2.10.2-15](/images/5.2.10.2-15.png)

- Add the following variables:

| Key | Value |
|-----|-------|
| ENVIRONMENT | {env} |
| LOG_LEVEL | INFO |
| JIRA_SECRET_NAME | pa-{env}-jira-credentials-{region} |
| SECRETS_REGION | {region} |
| IDENTITY_CENTER_INSTANCE_ARN | Value from **step 5.2.1** |
| IDENTITY_STORE_ID | Value from **step 5.2.1** |
| READONLY_PERMISSION_SET_ARN | ARN legacy ProdAccessReadOnly |
| POWERUSER_PERMISSION_SET_ARN | ARN legacy ProdAccessPowerUser |
| ADMIN_PERMISSION_SET_ARN | ARN legacy ProdAccessAdmin |
| USE_GROUP_BASED_ACCESS | true |
| SES_SENDER_EMAIL | Email verified in SES |

- Select **Save**.

![5.2.10.2-16](/images/5.2.10.2-16.png)

![5.2.10.2-17](/images/5.2.10.2-17.png)