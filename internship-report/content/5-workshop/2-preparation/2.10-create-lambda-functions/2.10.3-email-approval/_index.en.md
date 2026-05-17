---
title : "Email Approval"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.2.10.3 </b>
chapter : false
---

1. **Access:**
- Go to the **AWS Console** interface and search for "Lambda" in the search bar.

![5.2.9-1](/images/5.2.9-1.png)

- Select **Functions** from the left menu > Select **Create function**.

![5.2.10-1](/images/5.2.10-1.png)

2. **Configuration:**
- Select **Author from scratch**.
- **Function name:** `pa-{env}-email-approval-{region}`.
- **Runtime:** Select **Python 3.12**.
- **Architecture:** Select **x86_64**.
- **Execution role:** Select **Use another role** > Select `pa-{env}-email-approval-role-{region}`.
- Select **Create function**.

![5.2.10.3-1](/images/5.2.10.3-1.png)

![5.2.10.3-2](/images/5.2.10.3-2.png)

3. **Upload package:**
- In the newly created Lambda, go to the **Code** tab > Select **Upload from** > Select **.zip file**.

![5.2.10.3-3](/images/5.2.10.3-3.png)

- Select the file to upload that was downloaded in **step 5.2.9** > Select the file named **email_approval.zip** > Select **Save**.

![5.2.10.3-4](/images/5.2.10.3-4.png)

4. **Attach Lambda Layer:**
- Scroll down to the **Layers** section > Select **Edit** > Select **Add a layer**.

![5.2.10.3-5](/images/5.2.10.3-5.png)

![5.2.10.3-7](/images/5.2.10.3-7.png)

- **Layers source:** Select **Custom layers**.
- **Custom layers:** Select `pa-{env}-dependencies-{region}`.
- **Version:** Select **1**.
- Select **Add**.

![5.2.10.3-6](/images/5.2.10.3-6.png)

- Select **Save**.

![5.2.10.3-8](/images/5.2.10.3-8.png)

5. **Runtime Settings:**
- In the **Runtime settings** section, select **Edit**.

![5.2.10.3-10](/images/5.2.10.3-10.png)

- **Handler:** `lambda_functions.email_approval.handler.lambda_handler`.
- Select **Save**.

![5.2.10.3-9](/images/5.2.10.3-9.png)

6. **General Configuration:**
- Go to the **Configuration** tab > Select **General configuration** > Select **Edit**.

![5.2.10.3-11](/images/5.2.10.3-11.png)

- **Memory:** `256 MB`.
- **Timeout:** `30 seconds`.
- Select **Save**.

![5.2.10.3-12](/images/5.2.10.3-12.png)

7. **Environment Variables:**
- Go to the **Configuration** tab > Select **Environment variables** > Select **Edit**.

![5.2.10.3-13](/images/5.2.10.3-13.png)

- Add the following variables:

| Key | Value |
|-----|-------|
| ADMIN_EMAIL | Admin email to receive approval |
| SES_SENDER_EMAIL | Sender email |
| DYNAMODB_TOKEN_TABLE | pa-{env}-approval-tokens-{region} |
| TOKEN_SECRET_NAME | pa-{env}-token-secret-{region}     |
| LOG_LEVEL | INFO |
| JIRA_SECRET_NAME | pa-{env}-jira-credentials-{region} |
| WEBHOOK_SECRET_NAME | pa-{env}-webhook-auth-{region} |
| SECRETS_REGION | {region} |
| SES_CONFIGURATION_SET | pa-{env}-email-approval-{region} |

- Select **Save**.

![5.2.10.3-14](/images/5.2.10.3-14.png)

![5.2.10.3-15](/images/5.2.10.3-15.png)