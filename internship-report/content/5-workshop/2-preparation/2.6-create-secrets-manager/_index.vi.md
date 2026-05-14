---
title : "Tạo Secrets Manager"
date :  "`r Sys.Date()`" 
weight : 6
pre: <b> 5.2.6 </b>
chapter : false
---

Hệ thống sử dụng AWS Secrets Manager để lưu trữ các thông tin nhạy cảm như Jira API Token, API Key xác thực webhook, HMAC signing secret và Access Group Mapping.

Việc sử dụng Secrets Manager giúp:

- Không hardcode credentials trong source code hoặc Lambda environment variables.
- Dễ dàng rotate secrets khi cần.
- Phân quyền truy cập secrets thông qua IAM Policies.
- Audit được việc truy cập secrets qua CloudTrail.

### Tổng quan về các Secret cần tạo

| STT | Secret Name | Mục đích |
|:---:|:-----------:|----------|
| 1 | pa-{env}-jira-credentials-{region} | Lưu trữ thông tin định danh Jira API |
| 2 | pa-{env}-webhook-auth-{region} | Xác thực API key cho các yêu cầu từ Webhook |
| 3 | pa-{env}-access-group-mapping-{region} | Ánh xạ giữa Group ID và Quyền hạn (Permission) |
| 4 | pa-{env}-token-secret-{region} | Mã khóa HMAC dùng để tạo/xác thực email approval tokens |

**Lưu ý:** Thay `{env}` bằng môi trường tương ứng (Ví dụ: `sit`, `uat`, `prod`) và `{region}` bằng region bạn đang deploy (Ví dụ: `ap-southeast-1`).

![5.2.6-2](/images/5.2.6-2.png)

Sau khi tạo xong từng secret:

- Click vào secret.
- Copy giá trị **Secret ARN**.
- Lưu lại để sử dụng ở bước **IAM Policies** và **Lambda Environment Variables**.

| Secret | ARN |
|--------|-----|
| Jira Credentials | arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-jira-credentials-{region}-XXXXXX |
| Webhook Auth | arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-webhook-auth-{region}-XXXXXX |
| Access Group Mapping | arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-access-group-mapping-{region}-XXXXXX |
| Token Secret | arn:aws:secretsmanager:{region}:{account_id}:secret:pa-{env}-token-secret-{region}-XXXXXX |

### Các Secret cần tạo

1. [Jira Credentials](2.6.1-jira-credentials/)
2. [Webhook Auth](2.6.2-webhook-auth/)
3. [Access Group Mapping](2.6.3-access-group-mapping/)
4. [Token Secret (Email Approval)](2.6.4-token-secret/)
