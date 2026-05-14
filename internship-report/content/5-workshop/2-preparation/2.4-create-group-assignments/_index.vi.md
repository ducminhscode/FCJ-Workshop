---
title : "Tạo Group Assignments"
date :  "`r Sys.Date()`" 
weight : 4
pre: <b> 5.2.4 </b>
chapter : false
---

**Group Assignment** là thao tác gán **vĩnh viễn (permanent)** mỗi Access Group tới tài khoản AWS tương ứng kèm theo Permission Set phù hợp. Đây được coi là "dây nối" cố định trong hệ thống IAM Identity Center - một khi user được thêm vào group, họ sẽ tự động có quyền truy cập vào account đó với đặc quyền đã định nghĩa.

### Bảng Assignments cần tạo

Trước khi thực hiện, hãy đảm bảo bạn đã có danh sách ánh xạ giữa các nhóm và quyền hạn:

| STT | Group | AWS Account | Permission Set |
|:---:|:-----:|:-----------:|:--------------:|
| 1 | pa-{env}-application-ReadOnly-1h | application | ProdAccessReadOnly1h | 
| 2 | pa-{env}-application-ReadOnly-4h | application | ProdAccessReadOnly4h | 
| 3 | pa-{env}-application-ReadOnly-8h | application | ProdAccessReadOnly8h | 
| 4 | pa-{env}-application-PowerUser-1h | application | ProdAccessPowerUser1h | 
| 5 | pa-{env}-application-PowerUser-4h | application | ProdAccessPowerUser4h | 
| 6 | pa-{env}-application-PowerUser-8h | application | ProdAccessPowerUser8h | 
| 7 | pa-{env}-application-Admin-1h | application | ProdAccessAdmin1h | 
| 8 | pa-{env}-application-Admin-4h | application | ProdAccessAdmin4h | 
| 9 | pa-{env}-application-Admin-8h | application | ProdAccessAdmin8h | 
| 10 | pa-{env}-data-ReadOnly-1h | data | ProdAccessReadOnly1h | 
| 11 | pa-{env}-data-ReadOnly-4h | data | ProdAccessReadOnly4h | 
| 12 | pa-{env}-data-ReadOnly-8h | data | ProdAccessReadOnly8h | 
| 13 | pa-{env}-data-PowerUser-1h | data | ProdAccessPowerUser1h | 
| 14 | pa-{env}-data-PowerUser-4h | data | ProdAccessPowerUser4h | 
| 15 | pa-{env}-data-PowerUser-8h | data | ProdAccessPowerUser8h | 
| 16 | pa-{env}-data-Admin-1h | data | ProdAccessAdmin1h | 
| 17 | pa-{env}-data-Admin-4h | data | ProdAccessAdmin4h | 
| 18 | pa-{env}-data-Admin-8h | data | ProdAccessAdmin8h | 

### Hướng dẫn thực hiện chi tiết

Thực hiện lặp lại cho mỗi dòng trong bảng trên:

1. **Truy cập:** Vào **IAM Identity Center** > Chọn **Groups** từ menu bên trái > Chọn group thực hiện.

![5.2.4-1](/images/5.2.4-1.png)

2. **Chỉ định tài khoản:** Đổi sang tab **AWS accounts** > Chọn **Assign accounts**.

![5.2.4-2](/images/5.2.4-2.png)

3. **Gán Permission sets:** Tìm và tick chọn **Permission sets** tương ứng cho AWS accounts của group > Chọn **Assign**.

![5.2.4-3](/images/5.2.4-3.png)

![5.2.4-4](/images/5.2.4-4.png)

*Lặp lại cho tất cả 18 assignments.*