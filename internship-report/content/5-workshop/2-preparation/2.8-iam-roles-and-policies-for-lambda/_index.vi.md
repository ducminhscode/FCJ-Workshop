---
title : "IAM Roles & Policies cho Lambda"
date :  "`r Sys.Date()`" 
weight : 8
pre: <b> 5.2.8 </b>
chapter : false
---

Trong kiến trúc của hệ thống, mỗi Lambda function sẽ sử dụng một IAM Role riêng nhằm đảm bảo:

- Phân tách quyền truy cập theo chức năng.
- Tuân thủ nguyên tắc Least Privilege.
- Giảm rủi ro khi một Lambda bị compromise.
- Dễ audit và troubleshooting.

Hệ thống sử dụng tổng cộng **3 IAM Roles**:

| Lambda Function | IAM Role |
|:---------------:|:--------:|
| pa-{env}-executor-{region} | pa-{env}-lambda-executor-role-{region} |
| pa-{env}-expiry-{region} | pa-{env}-lambda-expiry-role |
| pa-{env}-email-approval-{region} | pa-{env}-email-approval-role-{region} |

### Các IAM cần tạo

1. [Lambda Executor Role](2.8.1-lambda-executor-role/)
2. [Lambda Expiry Role](2.8.2-lambda-expiry-role/)
3. [Lambda Email Approval Role](2.8.3-lambda-email-approval-role/)