---
title : "Tạo Simple Email Service"
date :  "`r Sys.Date()`" 
weight : 7
pre: <b> 5.2.7 </b>
chapter : false
---

Hệ thống sử dụng Amazon Simple Email Service để gửi email phục vụ các chức năng:
- Gửi email approval request tới admin.
- Gửi email thông báo cấp quyền thành công.
- Gửi email thông báo revoke quyền truy cập.
- Gửi approval link chứa signed token.
- Theo dõi trạng thái gửi email qua CloudWatch metrics.

### Xác thực Email gửi

Để AWS SES có quyền gửi email thay mặt mình, địa chỉ email người gửi cần được xác minh chính chủ.

1. **Truy cập:**
- Vào giao diện **AWS Console** và tìm kiếm "Amazon SES" trên thanh tìm kiếm.

![5.2.7-1](/images/5.2.7-1.png)

- Chọn **Identities** từ menu bên trái > Chọn **Create identity**.

![5.2.7-2](/images/5.2.7-2.png)

2. **Cấu hình:**
- **Identity type:** Chọn **Email address**.
- **Email address:** `SES_SENDER_EMAIL` (Ví dụ: `noreply@company.com`).
- **Assign a default configuration set:** None.
- **Assign to a tenant:** None.
- Chọn **Create identity**.

![5.2.7-3](/images/5.2.7-3.png)

Sau khi tạo identity, **AWS SES** sẽ gửi email xác nhận. Hãy mở inbox của email sender và click vào link để **Verify this email address**.

Sau khi verify thành công, trạng thái của identity (Identity status) sẽ chuyển thành **Verified**.

![5.2.7-4](/images/5.2.7-4.png)

### Tạo Configuration Set

**Configuration Set** giúp theo dõi các chỉ số gửi email và quản lý việc thực thi các quy định về gửi mail (reputation).

1. **Truy cập:**
- Vào giao diện **AWS Console** và tìm kiếm "Amazon SES" trên thanh tìm kiếm.
- Chọn **Configuration sets** từ menu bên trái > Chọn **Create set**.

![5.2.7-5](/images/5.2.7-5.png)

2. **Cấu hình:**
- **Configuration set name:** `pa-{env}-email-approval-{region}`.
- **Reputation metrics:** Chọn **Enable**.
- Chọn **Create set**.

![5.2.7-6](/images/5.2.7-6.png)

### Thêm Event Destination

**Event Destination** cho phép SES gửi email events tới CloudWatch để monitoring.

1. Click vào configuration set vừa tạo > Vào tab **Event destinations** > Chọn **Add destination**.

![5.2.7-7](/images/5.2.7-7.png)

![5.2.7-8](/images/5.2.7-8.png)

2. **Cấu hình:**
- Ở bước 1, chọn tất cả **Event types** > Chọn **Next**.

![5.2.7-9](/images/5.2.7-9.png)

- Ở bước 2:
  - **Destination type:** Chọn **Amazon CloudWatch**.
  - **Dimension name:** `ses:configuration-set`.
  - **Default value:** `default`.
  - **Value source:** `Message tag`.
  - Chọn **Next**.

![5.2.7-10](/images/5.2.7-10.png)

- Ở bước 3, kiểm tra lại thông tin và nhấn **Add destination**.

![5.2.7-11](/images/5.2.7-11.png)

![5.2.7-12](/images/5.2.7-12.png)

***Lưu ý về Sandbox Mode:** Nếu tài khoản AWS của bạn đang ở chế độ SES Sandbox, bạn chỉ có thể gửi email đến các địa chỉ đã được verified (xác thực). Để gửi email cho người dùng bất kỳ trong công ty, bạn cần gửi yêu cầu "Production Access" cho dịch vụ SES tới AWS Support.*