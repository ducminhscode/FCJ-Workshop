---
title : "Thiết lập API Gateway"
date :  "`r Sys.Date()`" 
weight : 11
pre: <b> 5.2.11 </b>
chapter : false
---

| Endpoint | Method | Lambda | Mục đích |
|----------|--------|--------|----------|
| /provision-access | POST | Executor Lambda | Cấp quyền truy cập |
| /email-approval/request | POST | Email Approval Lambda | Gửi email approval |
| /email-approval/action | GET | Email Approval Lambda | Approve/Reject qua email link |

### Tạo REST API

1. **Truy cập:**
- Vào giao diện **AWS Console** và tìm kiếm "API Gateway" trên thanh tìm kiếm.

![5.2.11-1](/images/5.2.11-1.png)

- Chọn **Create an API**.

![5.2.11-2](/images/5.2.11-2.png)

- Tìm đến mục **REST API** > Chọn **Build**.

![5.2.11-3](/images/5.2.11-3.png)

2. **Cấu hình:**
- **API name:** `pa-{env}-api-{region}`.
- **Description:** `API Gateway for {env} Access Request Portal`.
- **Endpoint Type:** Chọn **Regional**.
- Chọn **Create API**.

![5.2.11-4](/images/5.2.11-4.png)

### Tạo Resource & Method - Provision Access

1. **Tạo Resource:**
- Trong API vừa tạo, chọn **Create resource**.

![5.2.11-5](/images/5.2.11-5.png)

- **Cấu hình:**
  - **Resource path:** `/`.
  - **Resource name:** `provision-access`.
- Chọn **Create resource**.

![5.2.11-6](/images/5.2.11-6.png)

2. **Tạo POST Method:**
- Chọn resource `/provision-access` > Chọn **Create method**.

![5.2.11-7](/images/5.2.11-7.png)

- **Cấu hình:**
  - **Method type:** Chọn **POST**.
  - **Integration type:** Chọn **Lambda function**.
  - **Lambda proxy integration:** Bật **Enabled**.
  - **Lambda function:** Chọn vùng > Chọn `arn:aws:lambda:{region}:{account_id}:function:pa-{env}-executor-{region}`.
  - Tick vào ô **API key required**.
- Chọn **Create method**.

![5.2.11-8](/images/5.2.11-8.png)

![5.2.11-9](/images/5.2.11-9.png)

### Tạo Resources & Methods - Email Approval

1. **Tạo Resource:**
- Trong API vừa tạo, chọn **Create resource**.

![5.2.11-10](/images/5.2.11-10.png)

- **Cấu hình:**
  - **Resource path:** `/`.
  - **Resource name:** `email-approval`.
- Chọn **Create resource**.
- Tiếp tục **Create resource** tương tự với:
  - **Resource path:** `/email-approval`.
  - **Resource name:** `request`.

![5.2.11-11](/images/5.2.11-11.png)

2. **Tạo POST Method:**
- Chọn resource `/email-approval/request` > Chọn **Create method**.

![5.2.11-12](/images/5.2.11-12.png)

- **Cấu hình:**
  - **Method type:** Chọn **POST**.
  - **Integration type:** Chọn **Lambda function**.
  - **Lambda proxy integration:** Bật **Enabled**.
  - **Lambda function:** Chọn vùng > Chọn `arn:aws:lambda:{region}:{account_id}:function:pa-{env}-email-approval-{region}`.
  - Tick vào ô **API key required**.
- Chọn **Create method**.

![5.2.11-13](/images/5.2.11-13.png)

![5.2.11-14](/images/5.2.11-14.png)

3. **Tạo GET Method:**
- Tiếp tục **Create resource** tương tự với:
  - **Resource path:** `/email-approval`.
  - **Resource name:** `action`.

![5.2.11-15](/images/5.2.11-15.png)

![5.2.11-16](/images/5.2.11-16.png)

- Chọn resource `/email-approval/action` > Chọn **Create method**.

![5.2.11-17](/images/5.2.11-17.png)

- **Cấu hình:**
  - **Method type:** Chọn **GET**.
  - **Integration type:** Chọn **Lambda function**.
  - **Lambda proxy integration:** Bật **Enabled**.
  - **Lambda function:** Chọn vùng > Chọn `arn:aws:lambda:{region}:{account_id}:function:pa-{env}-email-approval-{region}`.
  - KHÔNG chọn vào ô **API key required**.
- Chọn **Create method**.

![5.2.11-18](/images/5.2.11-18.png)

![5.2.11-19](/images/5.2.11-19.png)

### Deploy API
- Chọn **Deploy API**.

![5.2.11-20](/images/5.2.11-20.png)

- **Stage:** Chọn **New Stage**.
- **Stage name:** `{env}`.
- Chọn **Deploy**.

![5.2.11-21](/images/5.2.11-21.png)

Sau khi deploy xong, AWS sẽ tạo **Invoke URL** và hãy ghi lại nó: `https://XXXXXXXXXX.execute-api.{region}.amazonaws.com/{env}`.

![5.2.11-22](/images/5.2.11-22.png)

### Tạo API Key
- Chọn **API keys** từ menu bên trái > Chọn **Create API key**.

![5.2.11-23](/images/5.2.11-23.png)

- **Name:** `pa-{env}-jira-webhook-key-{region}`.
- Chọn **Save**.

![5.2.11-24](/images/5.2.11-24.png)

- Chọn API key vừa tạo, chọn Show **API key** > Copy và lưu lại giá trị key.

![5.2.11-25](/images/5.2.11-25.png)

### Tạo Usage Plan
- Chọn **Usage plans** từ menu bên trái > Chọn **Create usage plan**.

![5.2.11-26](/images/5.2.11-26.png)

- **Name:** `pa-{env}-usage-plan-{region}`.
- **Description:** `Usage plan for {env} Access Portal`.
- **Throttling - Rate:** `10 requests/second`.
- **Throttling - Burst:** `20`.
- **Quota:** `1000` Per day.
- Chọn **Create usage plan**.

![5.2.11-27](/images/5.2.11-27.png)

- Chọn usage plan vừa tạo.

![5.2.11-28](/images/5.2.11-28.png)

- Vào tab **Associated API stages** > Chọn **Add API stage**.

![5.2.11-29](/images/5.2.11-29.png)

- **API:** `pa-{env}-api-{region}`.
- **Stage:** `{env}`.
- Chọn **Add to usage plan**.

![5.2.11-30](/images/5.2.11-30.png)

![5.2.11-31](/images/5.2.11-31.png)

- Vào tab **Associated API keys** > Chọn **Add API key**.

![5.2.11-32](/images/5.2.11-32.png)

- **API key:** `pa-{env}-jira-webhook-key-{region}`.
- Chọn **Add API key**.

![5.2.11-33](/images/5.2.11-33.png)

![5.2.11-34](/images/5.2.11-34.png)

### Cấu hình Lambda Permission

1. **Permission cho Executor Lambda:**

Mở CloudShell hoặc local terminal có AWS CLI và chạy Script:

```
aws lambda add-permission \
  --function-name pa-{env}-executor-{region} \
  --statement-id AllowAPIGatewayInvoke \
  --action lambda:InvokeFunction \
  --principal apigateway.amazonaws.com \
  --source-arn "arn:aws:execute-api:{region}:{account_id}:API_ID/*/*"
```

2. **Permission cho Email Approval Lambda:**

Mở CloudShell hoặc local terminal có AWS CLI và chạy Script:

```
aws lambda add-permission \
  --function-name pa-{env}-email-approval-{region} \
  --statement-id AllowAPIGatewayInvokeEmailApproval \
  --action lambda:InvokeFunction \
  --principal apigateway.amazonaws.com \
  --source-arn "arn:aws:execute-api:{region}:{account_id}:API_ID/*/*"
```

***API_ID** là phần đầu của **Invoke URL**.*

*(Ví dụ: **Invoke URL:** `https://abc123xyz.execute-api.ap-southeast-1.amazonaws.com/sit` > **API_ID:** `abc123xyz`).*

![5.2.11-35](/images/5.2.11-35.png)

