---
title : "Kiểm tra kết nối"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.3 </b>
chapter : false
---

### Kiểm tra kết nối API Gateway đến Lambda Executor
- Chạy Script trong CloudShell trên giao diện AWS Console:

```
curl -X POST \
  https://XXXXXXXXXX.execute-api.{region}.amazonaws.com/{env}/provision-access \
  -H "x-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"test": true}'
```

- Kết quả mong đợi:
  - API Gateway trả về response từ Lambda.
  - Có thể nhận HTTP: 200, 400 422.

### Kiểm tra Lambda Executor hoạt động
- Vào giao diện **AWS Console** và tìm kiếm "Lambda" trên thanh tìm kiếm.
- Chọn **Functions** từ menu bên trái > Chọn `pa-{env}-executor-{region}` > Vào tab **Test**.
- Tạo event mô phỏng:

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
- Chọn **Test**.
- Kết quả mong đợi:
  - Parse request body.
  - Validate payload.
  - Kết nối Secrets Manager.
  - Ghi dữ liệu vào DynamoDB.
  - Add user vào Identity Center Group.

### Kiểm tra DynamoDB Stream đến Lambda Expiry
- Vào giao diện **AWS Console** và tìm kiếm "DynamoDB" trên thanh tìm kiếm.
- Chọn **Tables** từ menu bên trái > Chọn `pa-{env}-access-sessions-{region}`.
- Tạo record test với:
  - **sessionId:** `TEST-SESSION-001`.
  - **requester:** `test@company.com`.
  - **ttl:** `Unix timestamp hiện tại + 2 phút`.
- Kết quả mong đợi: **Lambda Expiry** được invoke tự động.

### Kiểm tra Email Approval Workflow

- Chạy Script trong CloudShell trên giao diện AWS Console:

```
curl -X POST \
  https://XXXXXXXXXX.execute-api.{region}.amazonaws.com/{env}/email-approval/request \
  -H "x-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"requestId":"TEST-002","requester":"test@company.com"}'
```

- Kết quả mong đợi:
  - Generate approval token.
  - Lưu token vào DynamoDB.
  - Gửi email qua SES.
  - Sinh approve/reject link.