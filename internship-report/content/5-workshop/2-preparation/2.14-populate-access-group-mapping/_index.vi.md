---
title : "Populate Access Group Mapping"
date :  "`r Sys.Date()`" 
weight : 14
pre: <b> 5.2.14 </b>
chapter : false
---

Truy cập vào [đường dẫn](https://gitlab.com/ducminhscode/production-access-request-portal.git) GitLab để clone dự án về, sau đó thực hiện câu lệnh trên Terminal hoặc Command Prompt/PowerShell:
```
cd production-access-request-portal/infrastructure/scripts
``` 

Chạy Script sau:

```
python3 populate_access_group_mapping.py --environment {env} --region {region}
```

*Thay `{env}` bằng môi trường phát triển, `{region}` là vùng hiện tại.*

### Các điều kiện cần thiết trên máy để chạy được

Để lệnh chạy thành công và đẩy được dữ liệu lên AWS Secrets Manager, máy cần có:

- **Python:** Đã cài đặt Python 3.
- **Thư viện Boto3:** Đây là thư viện để Python "nói chuyện" được với AWS. Cài bằng lệnh: pip install boto3.
- **AWS CLI & Credentials:** Máy phải được cấu hình quyền truy cập AWS. Cần chạy lệnh aws configure và nhập:
  - **AWS Access Key ID** và **AWS Secret Access Key** (của một tài khoản IAM có quyền ghi vào Secrets Manager).
  - Default region name (Ví dụ: ap-southeast-1).