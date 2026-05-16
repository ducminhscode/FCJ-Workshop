---
title : "Dọn dẹp tài nguyên"
date :  "`r Sys.Date()`" 
weight : 5
pre: <b> 5.5 </b>
chapter : false
---

## Tổng quan

Sau khi hoàn tất quá trình kiểm thử hoặc không còn nhu cầu sử dụng hệ thống, cần thực hiện dọn dẹp toàn bộ tài nguyên AWS để:

- Tránh phát sinh chi phí không cần thiết.
- Loại bỏ IAM permissions dư thừa.
- Đảm bảo không còn API hoặc credentials hoạt động ngoài ý muốn.
- Giữ môi trường AWS sạch và dễ quản lý.

Khuyến nghị:
- Thực hiện cleanup theo đúng thứ tự dependency.
- Không xóa tài nguyên production nếu chưa backup hoặc xác nhận với team.
- Với môi trường UAT/SIT: nên snapshot hoặc export logs trước khi xóa.

## Danh sách tài nguyên cần xóa

| Nhóm tài nguyên | Thành phần |
|-----------------|------------|
| API Layer | API Gateway, API Keys, Usage Plans |
| Compute | Lambda Functions, Lambda Layers |
| IAM | IAM Roles, Inline Policies |
| Storage | DynamoDB Tables |
| Identity Center | Access Groups, Group Assignments, Permission Sets |
| Security | Secrets Manager Secrets |
| Email | SES Identities, Configuration Sets |
| Monitoring | CloudWatch Log Groups, Alarms |

### Bước 1 - Xóa API Gateway

#### Xóa REST API

1. Truy cập **API Gateway**.
2. Chọn API: `pa-{env}-api-{region}`.
3. Chọn **Actions** > **Delete API**.
4. Nhập: `delete`.
5. Confirm **Delete**.

#### Xóa API Keys

1. Menu bên trái > **API Keys**.
2. Chọn: `pa-{env}-jira-webhook-key-{region}`.
3. Chọn **Delete**.

#### Xóa Usage Plans

1. Menu bên trái > **Usage Plans**.
2. Chọn: `pa-{env}-usage-plan-{region}`.
3. Chọn **Delete**.

### Bước 2 - Xóa Lambda Functions

#### Xóa Lambda Executor

1. Truy cập **Lambda**.
2. Chọn function: `pa-{env}-executor-{region}`.
3. Chọn **Actions** > **Delete**.
4. Nhập tên function để confirm.

#### Xóa Lambda Expiry

Xóa function: `pa-{env}-expiry-{region}`.

#### Xóa Lambda Email Approval

Xóa function: `pa-{env}-email-approval-{region}`.

### Bước 3 - Xóa Event Source Mappings

*Lưu ý: Nếu chưa xóa Event Source Mapping, DynamoDB Stream có thể tiếp tục cố invoke Lambda.*

1. **Lambda** > Chọn: `pa-{env}-expiry-{region}`.
2. Tab **Configuration** > **Triggers**.
3. Chọn DynamoDB trigger.
4. Chọn **Delete**.

### Bước 4 - Xóa Lambda Layer

1. Truy cập **Lambda** > **Layers**.
2. Chọn: `pa-{env}-dependencies-{region}`.
3. Chọn từng version.
4. Chọn **Delete**.

*Phải xóa toàn bộ versions trước khi layer biến mất hoàn toàn.*

### Bước 5 - Xóa CloudWatch Logs & Alarms

#### Xóa CloudWatch Alarms

Truy cập: **CloudWatch** > **Alarms**.

Xóa các alarms: `pa-{env}-executor-{region}-errors`, `pa-{env}-expiry-{region}-errors`, `pa-{env}-email-approval-{region}-errors`.

#### Xóa CloudWatch Log Groups

Truy cập: **CloudWatch** > **Log groups**.

Xóa các log groups: `/aws/lambda/pa-{env}-executor-{region}`, `/aws/lambda/pa-{env}-expiry-{region}`, `/aws/lambda/pa-{env}-email-approval-{region}`.

### Bước 6 - Xóa IAM Roles & Policies

#### Xóa Inline Policies

Trước khi xóa role, cần remove toàn bộ inline policies.

**IAM** > **Roles** > Chọn role: `pa-{env}-lambda-executor-role-{region}`.

Tab **Permissions**: Remove tất cả inline policies.

Lặp lại cho: `pa-{env}-lambda-expiry-role`, `pa-{env}-email-approval-role-{region}`.

#### Xóa IAM Roles

Sau khi remove policies:

1. Chọn role.
2. Chọn **Delete**.
3. Confirm delete.

### Bước 7 - Xóa Secrets Manager

Truy cập: **Secrets Manager**.

Xóa các secrets: `pa-{env}-jira-credentials-{region}`, `pa-{env}-webhook-auth-{region}`, `pa-{env}-access-group-mapping-{region}`, `pa-{env}-token-secret-{region}`.

#### Force Delete (Tùy chọn)

Nếu không muốn chờ recovery period.

Khi delete secret: Disable recovery > Permanently delete.

*Không thể khôi phục sau khi force delete.*

### Bước 8 - Xóa DynamoDB Tables
 
#### Xóa Access Sessions Table

1. **DynamoDB** > **Tables**.
2. Chọn: `pa-{env}-access-sessions-{region}`.
3. Chọn **Delete**.
4. Nhập tên table để confirm.

#### Xóa Approval Tokens Table

Xóa table: `pa-{env}-approval-tokens-{region}`.

### Bước 9 - Xóa SES Configuration

#### Xóa Configuration Set

**SES** > **Configuration sets**.

Xóa: `pa-{env}-email-approval-{region}`.

#### Xóa Verified Email Identity

**SES** > **Verified identities**.

Xóa email identity: `SES_SENDER_EMAIL`.

*Chỉ xóa nếu email không còn được sử dụng bởi hệ thống khác.*

### Bước 10 - Xóa IAM Identity Center Assignments

#### Xóa Group Assignments

1. **IAM Identity Center** > **AWS accounts**.
2. Chọn từng AWS Account.
3. Remove toàn bộ assignments liên quan: `pa-{env}-application-*`, `pa-{env}-data-*`.

*Bắt buộc remove assignments trước khi xóa groups hoặc permission sets.*

### Bước 11 - Xóa Access Groups

1. **IAM Identity Center** > Groups.
2. Xóa toàn bộ groups:

```text
pa-{env}-application-ReadOnly-1h
pa-{env}-application-ReadOnly-4h
...
pa-{env}-data-Admin-8h
```

### Bước 12 - Xóa Permission Sets

1. **IAM Identity Center** > **Permission sets**.
2. Xóa toàn bộ:

```text
ProdAccessReadOnly1h
ProdAccessReadOnly4h
ProdAccessReadOnly8h

ProdAccessPowerUser1h
ProdAccessPowerUser4h
ProdAccessPowerUser8h

ProdAccessAdmin1h
ProdAccessAdmin4h
ProdAccessAdmin8h
```

#### Xóa Legacy Permission Sets

Tiếp tục xóa:

```text
ProdAccessReadOnly
ProdAccessPowerUser
ProdAccessAdmin
```

## Kết luận

Sau khi hoàn tất toàn bộ các bước trên:
- Hệ thống Production Access Request Portal sẽ được gỡ bỏ hoàn toàn khỏi AWS.
- Không còn quyền truy cập tạm thời tồn tại trong IAM Identity Center.
- Không còn tài nguyên phát sinh chi phí nền.
- Môi trường AWS được trả về trạng thái sạch.

Khuyến nghị:
- Với môi trường production, nên export CloudWatch Logs và backup DynamoDB trước khi cleanup.
- Có thể giữ lại Permission Sets nếu dự kiến triển khai lại trong tương lai.
