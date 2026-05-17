---
title : "Set up API Gateway"
date :  "`r Sys.Date()`" 
weight : 11
pre: <b> 5.2.11 </b>
chapter : false
---

| Endpoint | Method | Lambda | Purpose |
|----------|--------|--------|---------|
| /provision-access | POST | Executor Lambda | Grant access permissions |
| /email-approval/request | POST | Email Approval Lambda | Send approval email |
| /email-approval/action | GET | Email Approval Lambda | Approve/Reject via email link |

### Create REST API

1. **Access:**
- Go to the **AWS Console** interface and search for "API Gateway" in the search bar.

![5.2.11-1](/images/5.2.11-1.png)

- Select **Create an API**.

![5.2.11-2](/images/5.2.11-2.png)

- Navigate to **REST API** > Select **Build**.

![5.2.11-3](/images/5.2.11-3.png)

2. **Configuration:**
- **API name:** `pa-{env}-api-{region}`.
- **Description:** `API Gateway for {env} Access Request Portal`.
- **Endpoint Type:** Select **Regional**.
- Select **Create API**.

![5.2.11-4](/images/5.2.11-4.png)

### Create Resource & Method - Provision Access

1. **Create Resource:**
- In the newly created API, select **Create resource**.

![5.2.11-5](/images/5.2.11-5.png)

- **Configuration:**
  - **Resource path:** `/`.
  - **Resource name:** `provision-access`.
- Select **Create resource**.

![5.2.11-6](/images/5.2.11-6.png)

2. **Create POST Method:**
- Select the `/provision-access` resource > Select **Create method**.

![5.2.11-7](/images/5.2.11-7.png)

- **Configuration:**
  - **Method type:** Select **POST**.
  - **Integration type:** Select **Lambda function**.
  - **Lambda proxy integration:** Enable **Enabled**.
  - **Lambda function:** Select region > Select `arn:aws:lambda:{region}:{account_id}:function:pa-{env}-executor-{region}`.
  - Tick the **API key required** checkbox.
- Select **Create method**.

![5.2.11-8](/images/5.2.11-8.png)

![5.2.11-9](/images/5.2.11-9.png)

### Create Resources & Methods - Email Approval

1. **Create Resource:**
- In the newly created API, select **Create resource**.

![5.2.11-10](/images/5.2.11-10.png)

- **Configuration:**
  - **Resource path:** `/`.
  - **Resource name:** `email-approval`.
- Select **Create resource**.
- Continue to **Create resource** similarly with:
  - **Resource path:** `/email-approval`.
  - **Resource name:** `request`.

![5.2.11-11](/images/5.2.11-11.png)

2. **Create POST Method:**
- Select the `/email-approval/request` resource > Select **Create method**.

![5.2.11-12](/images/5.2.11-12.png)

- **Configuration:**
  - **Method type:** Select **POST**.
  - **Integration type:** Select **Lambda function**.
  - **Lambda proxy integration:** Enable **Enabled**.
  - **Lambda function:** Select region > Select `arn:aws:lambda:{region}:{account_id}:function:pa-{env}-email-approval-{region}`.
  - Tick the **API key required** checkbox.
- Select **Create method**.

![5.2.11-13](/images/5.2.11-13.png)

![5.2.11-14](/images/5.2.11-14.png)

3. **Create GET Method:**
- Continue to **Create resource** similarly with:
  - **Resource path:** `/email-approval`.
  - **Resource name:** `action`.

![5.2.11-15](/images/5.2.11-15.png)

![5.2.11-16](/images/5.2.11-16.png)

- Select the `/email-approval/action` resource > Select **Create method**.

![5.2.11-17](/images/5.2.11-17.png)

- **Configuration:**
  - **Method type:** Select **GET**.
  - **Integration type:** Select **Lambda function**.
  - **Lambda proxy integration:** Enable **Enabled**.
  - **Lambda function:** Select region > Select `arn:aws:lambda:{region}:{account_id}:function:pa-{env}-email-approval-{region}`.
  - Do NOT tick the **API key required** checkbox.
- Select **Create method**.

![5.2.11-18](/images/5.2.11-18.png)

![5.2.11-19](/images/5.2.11-19.png)

### Deploy API
- Select **Deploy API**.

![5.2.11-20](/images/5.2.11-20.png)

- **Stage:** Select **New Stage**.
- **Stage name:** `{env}`.
- Select **Deploy**.

![5.2.11-21](/images/5.2.11-21.png)

After deployment is complete, AWS will generate the **Invoke URL**. Please note it down: `https://XXXXXXXXXX.execute-api.{region}.amazonaws.com/{env}`.

![5.2.11-22](/images/5.2.11-22.png)

### Create API Key
- Select **API keys** from the left menu > Select **Create API key**.

![5.2.11-23](/images/5.2.11-23.png)

- **Name:** `pa-{env}-jira-webhook-key-{region}`.
- Select **Save**.

![5.2.11-24](/images/5.2.11-24.png)

- Select the newly created API key, choose Show **API key** > Copy and save the key value.

![5.2.11-25](/images/5.2.11-25.png)

### Create Usage Plan
- Select **Usage plans** from the left menu > Select **Create usage plan**.

![5.2.11-26](/images/5.2.11-26.png)

- **Name:** `pa-{env}-usage-plan-{region}`.
- **Description:** `Usage plan for {env} Access Portal`.
- **Throttling - Rate:** `10 requests/second`.
- **Throttling - Burst:** `20`.
- **Quota:** `1000` Per day.
- Select **Create usage plan**.

![5.2.11-27](/images/5.2.11-27.png)

- Select the newly created usage plan.

![5.2.11-28](/images/5.2.11-28.png)

- Go to the **Associated API stages** tab > Select **Add API stage**.

![5.2.11-29](/images/5.2.11-29.png)

- **API:** `pa-{env}-api-{region}`.
- **Stage:** `{env}`.
- Select **Add to usage plan**.

![5.2.11-30](/images/5.2.11-30.png)

![5.2.11-31](/images/5.2.11-31.png)

- Go to the **Associated API keys** tab > Select **Add API key**.

![5.2.11-32](/images/5.2.11-32.png)

- **API key:** `pa-{env}-jira-webhook-key-{region}`.
- Select **Add API key**.

![5.2.11-33](/images/5.2.11-33.png)

![5.2.11-34](/images/5.2.11-34.png)

### Configure Lambda Permission

1. **Permission for Executor Lambda:**

Open CloudShell or a local terminal with AWS CLI and run the script:

```
aws lambda add-permission \
  --function-name pa-{env}-executor-{region} \
  --statement-id AllowAPIGatewayInvoke \
  --action lambda:InvokeFunction \
  --principal apigateway.amazonaws.com \
  --source-arn "arn:aws:execute-api:{region}:{account_id}:API_ID/*/*"
```

2. **Permission for Email Approval Lambda:**

Open CloudShell or a local terminal with AWS CLI and run the script:

```
aws lambda add-permission \
  --function-name pa-{env}-email-approval-{region} \
  --statement-id AllowAPIGatewayInvokeEmailApproval \
  --action lambda:InvokeFunction \
  --principal apigateway.amazonaws.com \
  --source-arn "arn:aws:execute-api:{region}:{account_id}:API_ID/*/*"
```


***API_ID** is the beginning part of the **Invoke URL**.*

*(Example: **Invoke URL:** `https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/sit` > **API_ID:** `abc123xyz`).*

![5.2.11-35](/images/5.2.11-35.png)