---
title : "Tạo các bảng DynamoDB"
date :  "`r Sys.Date()`" 
weight : 5
pre: <b> 5.2.5 </b>
chapter : false
---

Trong bước này, chúng ta sẽ cấu hình các bảng **Amazon DynamoDB** để lưu trữ trạng thái phiên làm việc (sessions) và các yêu cầu truy cập. DynamoDB được chọn vì khả năng mở rộng linh hoạt và độ trễ thấp.

### Các bảng

1. [Bảng AccessSessions](2.5.1-access-sessions-table/)
2. [Bảng ProvisionAccess](2.5.2-provision-access-table/)