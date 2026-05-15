---
title : "Tạo Lambda Functions"
date :  "`r Sys.Date()`" 
weight : 10
pre: <b> 5.2.10 </b>
chapter : false
---

Ở bước này, chúng ta sẽ triển khai 3 Lambda Functions chính cho hệ thống:

| Lambda Function | Chức năng |
|-----------------|-----------|
| Executor | Xử lý request cấp quyền truy cập AWS |
| Expiry | Tự động thu hồi quyền khi session hết hạn |
| Email Approval | Gửi email approval và xử lý approve/reject |

### Các function cần tạo

1. [Executor](2.10.1-executor/)
2. [Expiry](2.10.2-expiry/)
3. [Email Approval](2.10.3-email-approval/)
