---
title : "Tạo Access Groups"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.2.3 </b>
chapter : false
---

**Access Groups** là các nhóm trong IAM Identity Center. Trong kiến trúc v2.0, mỗi group đại diện cho một tổ hợp duy nhất giữa **Account × Access Type × Duration Tier**. 
- **Khi cấp quyền:** Lambda chỉ cần thêm user vào group tương ứng > User tự động nhận quyền truy cập vào Account. 
- **Khi thu hồi:** Lambda xóa user khỏi group > Mọi credentials hiện hành của user sẽ bị vô hiệu hóa (invalid) trong vòng tối đa 60 giây.

### Bảng Access Groups cần tạo

Dựa trên cấu hình chuẩn (2 accounts × 3 loại quyền × 3 bậc thời gian), cần tạo tổng cộng 18 groups:

 | STT | Tên Group  | Account | Access Type | Duration | 
 |:---:|:---------:|:-------:|:-----------:|:--------:| 
 | 1 | pa-{env}-application-ReadOnly-1h | application | ReadOnly | 1h | 
 | 2 | pa-{env}-application-ReadOnly-4h | application | ReadOnly | 4h | 
 | 3 | pa-{env}-application-ReadOnly-8h | application | ReadOnly | 8h | 
 | 4 | pa-{env}-application-PowerUser-1h | application | PowerUser | 1h | 
 | 5 | pa-{env}-application-PowerUser-4h | application | PowerUser | 4h | 
 | 6 | pa-{env}-application-PowerUser-8h | application | PowerUser | 8h | 
 | 7 | pa-{env}-application-Admin-1h | application | Admin | 1h | 
 | 8 | pa-{env}-application-Admin-4h | application | Admin | 4h | 
 | 9 | pa-{env}-application-Admin-8h | application | Admin | 8h | 
 | 10 | pa-{env}-data-ReadOnly-1h | data | ReadOnly | 1h | 
 | 11 | pa-{env}-data-ReadOnly-4h | data | ReadOnly | 4h | 
 | 12 | pa-{env}-data-ReadOnly-8h | data | ReadOnly | 8h | 
 | 13 | pa-{env}-data-PowerUser-1h | data | PowerUser | 1h | 
 | 14 | pa-{env}-data-PowerUser-4h | data | PowerUser | 4h | 
 | 15 | pa-{env}-data-PowerUser-8h | data | PowerUser | 8h | 
 | 16 | pa-{env}-data-Admin-1h | data | Admin | 1h | 
 | 17 | pa-{env}-data-Admin-4h | data | Admin | 4h | 
 | 18 | pa-{env}-data-Admin-8h | data | Admin | 8h | 

 **Lưu ý:** Thay `{env}` bằng tên môi trường (Ví dụ: `sit`, `uat`, `prod`).

 ### Hướng dẫn thực hiện chi tiết

 Thực hiện các bước sau cho từng group trong danh sách trên:
 
1. **Truy cập:** Vào **IAM Identity Center** > Chọn **Groups** từ menu bên trái > Click **Create group**.

![5.2.3-1](/images/5.2.3-1.png)

2. **Cấu hình:**
- **Group name:** Nhập chính xác theo quy ước (Ví dụ: `pa-sit-application-ReadOnly-1h`).
- **Description:** Nhập mô tả để dễ quản lý (Ví dụ: `JIT Access: application - ReadOnly - 1h`).

![5.2.3-2](/images/5.2.3-2.png)

3. **Thành viên:** KHÔNG thêm bất kỳ user nào vào group tại thời điểm này. Group phải để trống.
   
4. **Hoàn tất:** Click **Create group**.

5. **Ghi nhận thông tin:** Sau khi tạo thành công, click vào tên group vừa tạo. Copy và lưu lại Group ID (có định dạng UUID như: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`).

![5.2.3-3](/images/5.2.3-3.png)

*Lặp lại cho tất cả 18 groups và ghi lại Group ID.*

![5.2.3-4](/images/5.2.3-4.png)

