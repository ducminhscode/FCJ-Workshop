---
title : "Chuẩn bị tài nguyên"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2 </b>
chapter : false
---

Để thực hiện các bước tiếp theo, cần hoàn thành các yêu cầu sau:
- **Tài khoản AWS:** Đã có quyền truy cập vào AWS Management Console.
- **Quyền hạn IAM:** Tài khoản đang sử dụng phải có quyền AdministratorAccess hoặc tối thiểu là các quyền quản trị cho các dịch vụ: IAM, Lambda, DynamoDB, API Gateway, và SES.
- **Vùng làm việc (Region):** Xác định rõ Region sẽ triển khai (ví dụ: ap-southeast-1 - Singapore) và giữ nhất quán Region này trong suốt quá trình cài đặt.
- **Cấu hình AWS Organizations:** Đảm bảo tính năng AWS IAM Identity Center (SAML 2.0) đã được kích hoạt nếu bạn thực hiện thiết lập liên quan đến Group Assignments.

*Lưu ý: Tất cả các tên tài nguyên (Resource Name) trong tài liệu này nên được đặt theo một quy tắc thống nhất để dễ quản lý. Hãy sao chép chính xác các giá trị ARN hoặc ID phát sinh trong quá trình cài đặt để sử dụng cho các bước sau.*

### Các bước chuẩn bị

1. [Kích hoạt Identity Center](2.1-activate-identity-center/)
2. [Tạo Permission Sets](2.2-create-permission-sets/)
3. [Tạo Access Groups](2.3-create-access-group/)
4. [Tạo Group Assignments](2.4-create-group-assignments/)
5. [Tạo các bảng DynamoDB](2.5-create-dynamodb-tables/)
6. [Tạo Secrets Manager](2.6-create-secrets-manager/)
7. [Tạo Simple Email Service](2.7-create-simple-email-service/)
8. [IAM Roles & Policies cho Lambda](2.8-iam-roles-and-policies-for-lambda/)
9. [Tạo Lambda Layer](2.9-create-lambda-layer/)
10. [Tạo Lambda Function](2.10-create-lambda-functions/)
11. [Thiết lập API Gateway](2.11-setup-api-gateway/)
12. [Kết nối DynamoDB Stream đến Lambda Expiry](2.12-connect-dynamodb-stream-to-lambda-expiry/)
13. [Thiết lập CloudWatch Alarms](2.13-setup-cloudwatch-alarms/)
14. [Populate Access Group Mapping](2.14-populate-access-group-mapping/)