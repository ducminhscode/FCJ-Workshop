---
title : "Executor"
date :  "`r Sys.Date()`" 
weight : 1
pre: <b> 5.2.10.1 </b>
chapter : false
---

1. **Access:**
- Go to the **AWS Console** interface and search for "Lambda" in the search bar.

![5.2.9-1](/images/5.2.9-1.png)

- Select **Functions** from the left menu > Select **Create function**.

![5.2.10-1](/images/5.2.10-1.png)

2. **Configuration:**
- Select **Author from scratch**.
- **Function name:** `pa-{env}-executor-{region}`.
- **Runtime:** Select **Python 3.12**.
- **Architecture:** Select **x86_64**.
- **Execution role:** Select **Use another role** > Select `pa-{env}-lambda-executor-role-{region}`.
- Select **Create function**.

![5.2.10.1-1](/images/5.2.10.1-1.png)

![5.2.10.1-2](/images/5.2.10.1-2.png)

3. **Upload package:**
- In the newly created Lambda, go to the **Code** tab > Select **Upload from** > Select **.zip file**.

![5.2.10.1-3](/images/5.2.10.1-3.png)

![5.2.10.1-4](/images/5.2.10.1-4.png)

- Select the file to upload that was downloaded in **step 5.2.9** > Select the file named **executor.zip** > Select **Save**.

![5.2.10.1-5](/images/5.2.10.1-5.png)

![5.2.10.1-6](/images/5.2.10.1-6.png)

4. **Attach Lambda Layer:**
- Scroll down to the **Layers** section > Select **Edit** > Select **Add a layer**.

![5.2.10.1-7](/images/5.2.10.1-7.png)

![5.2.10.1-8](/images/5.2.10.1-8.png)

- **Layers source:** Select **Custom layers**.
- **Custom layers:** Select `pa-{env}-dependencies-{region}`.
- **Version:** Select **1**.
- Select **Add**.

![5.2.10.1-9](/images/5.2.10.1-9.png)

![5.2.10.1-10](/images/5.2.10.1-10.png)

- Select **Save**.

![5.2.10.1-11](/images/5.2.10.1-11.png)

5. **Runtime Settings:**
- In the **Runtime settings** section, select **Edit**.

![5.2.10.1-12](/images/5.2.10.1-12.png)

- **Handler:** `lambda_functions.executor.handler.lambda_handler`.
- Select **Save**.

![5.2.10.1-13](/images/5.2.10.1-13.png)

6. **General Configuration:**
- Go to the **Configuration** tab > Select **General configuration** > Select **Edit**.

![5.2.10.1-14](/images/5.2.10.1-14.png)

- **Memory:** `256 MB`.
- **Timeout:** `30 seconds`.
- Select **Save**.

![5.2.10.1-15](/images/5.2.10.1-15.png)

7. **Environment Variables:**
- Go to the **Configuration** tab > Select **Environment variables** > Select **Edit**.

![5.2.10.1-16](/images/5.2.10.1-16.png)

- Add the following variables:

| Key | Value |
|-----|-------|
| ENVIRONMENT | {env} |
| LOG_LEVEL | INFO |
| DYNAMODB_TABLE_NAME | pa-{env}-access-sessions-{region} |
| JIRA_SECRET_NAME | pa-{env}-jira-credentials-{region} |
| WEBHOOK_SECRET_NAME | pa-{env}-webhook-auth-{region} |
| ACCESS_GROUP_MAPPING_SECRET_NAME | pa-{env}-access-group-mapping-{region} |
| SECRETS_REGION | {region} |
| IDENTITY_CENTER_INSTANCE_ARN | Value from **step 5.2.1** |
| IDENTITY_STORE_ID | Value from **step 5.2.1** |
| IDENTITY_CENTER_PORTAL_URL | https://{identity_store_id}.awsapps.com/start |
| READONLY_PERMISSION_SET_ARN | ARN legacy ProdAccessReadOnly |
| POWERUSER_PERMISSION_SET_ARN | ARN legacy ProdAccessPowerUser |
| ADMIN_PERMISSION_SET_ARN | ARN legacy ProdAccessAdmin |
| USE_GROUP_BASED_ACCESS | true |
| SES_SENDER_EMAIL | Email verified in SES |
| ACCOUNT_MAPPING | {"sit-app":"xxxxxxxxxxxx","sit-data":"xxxxxxxxxxxx"} |
| ACCOUNT_NAME_MAPPING | {"sit-app":"application","sit-data":"data"} |

- Select **Save**.

![5.2.10.1-17](/images/5.2.10.1-17.png)

![5.2.10.1-18](/images/5.2.10.1-18.png)