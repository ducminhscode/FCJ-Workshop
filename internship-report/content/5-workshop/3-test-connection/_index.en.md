---
title : "Test Connection"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.3 </b>
chapter : false
---

### Test API Gateway Connection to Lambda Executor
- Run the script in CloudShell on the AWS Console interface:

```
curl -X POST \
  https://XXXXXXXXXX.execute-api.{region}.amazonaws.com/{env}/provision-access \
  -H "x-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"test": true}'
```

- Expected results:
  - API Gateway returns a response from Lambda.
  - May receive HTTP: 200, 400, 422.

### Test Lambda Executor Operation
- Go to the **AWS Console** interface and search for "Lambda" in the search bar.
- Select **Functions** from the left menu > Select `pa-{env}-executor-{region}` > Go to the **Test** tab.
- Create a mock event:

```
{
  "httpMethod": "POST",
  "path": "/provision-access",
  "headers": {
    "Content-Type": "application/json",
    "x-api-key": "test-key"
  },
  "body": "{\"requestId\":\"TEST-001\",\"requester\":\"test@company.com\",\"awsAccount\":\"application\",\"accessType\":\"ReadOnly\",\"durationHours\":1}"
}
```

- Select **Test**.
- Expected results:
  - Parse request body.
  - Validate payload.
  - Connect to Secrets Manager.
  - Write data to DynamoDB.
  - Add user to Identity Center Group.

### Test DynamoDB Stream to Lambda Expiry
- Go to the **AWS Console** interface and search for "DynamoDB" in the search bar.
- Select **Tables** from the left menu > Select `pa-{env}-access-sessions-{region}`.
- Create a test record with:
  - **sessionId:** `TEST-SESSION-001`.
  - **requester:** `test@company.com`.
  - **ttl:** `Current Unix timestamp + 2 minutes`.
- Expected result: **Lambda Expiry** is invoked automatically.

### Test Email Approval Workflow

- Run the script in CloudShell on the AWS Console interface:

```
curl -X POST \
  https://XXXXXXXXXX.execute-api.{region}.amazonaws.com/{env}/email-approval/request \
  -H "x-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"requestId":"TEST-002","requester":"test@company.com"}'
```

- Expected results:
  - Generate approval token.
  - Save token to DynamoDB.
  - Send email via SES.
  - Generate approve/reject links.
