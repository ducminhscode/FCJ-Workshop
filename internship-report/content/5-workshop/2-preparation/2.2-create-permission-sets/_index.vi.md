---
title : "Tạo Permission Sets"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.2 </b>
chapter : false
---

**Permission Sets** quy định người dùng có quyền gì và thời lượng phiên làm việc kéo dài bao lâu. Trong kiến trúc mới, chúng ta cần tạo một Permission Set riêng biệt cho mỗi tổ hợp giữa **Loại quyền (Access Type)** và **Bậc thời gian (Duration Tier)**.

### Bảng Permission Sets cần tạo

Dưới đây là danh sách 9 Permission Sets tiêu chuẩn (dựa trên 3 loại quyền x 3 bậc thời gian):

| STT | Tên Permission Set | Access Type | Duration | AWS Managed Policy |
|:---:|:------------------:|:-----------:|:--------:|:------------------:|
| 1 | ProdAccessReadOnly1h | ReadOnly | 1h | ReadOnlyAccess |
| 2 | ProdAccessReadOnly4h | ReadOnly | 4h | ReadOnlyAccess |
| 3 | ProdAccessReadOnly8h | ReadOnly | 8h | ReadOnlyAccess |
| 4 | ProdAccessPowerUser1h | PowerUser | 1h | PowerUserAccess |
| 5 | ProdAccessPowerUser4h | PowerUser | 4h | PowerUserAccess |
| 6 | ProdAccessPowerUser8h | PowerUser | 8h | PowerUserAccess |
| 7 | ProdAccessAdmin1h | Admin | 1h | AdministratorAccess |
| 8 | ProdAccessAdmin4h | Admin | 4h | AdministratorAccess |
| 9 | ProdAccessAdmin8h | Admin | 8h | AdministratorAccess |

**Mẹo:** Nếu dự án yêu cầu nhiều bậc thời gian hơn (Ví dụ: thêm 2h, 12h), hãy tạo thêm các bộ tương ứng theo công thức trên.

### Hướng dẫn thực hiện chi tiết

Thực hiện lặp lại các bước sau cho từng Permission Set có trong bảng trên:

1. **Truy cập:** Vào **IAM Identity Center** > Chọn **Permission sets** từ menu bên trái > Click **Create permission set**.

![5.2.2-1](/images/5.2.2-1.png)

2. **Khởi tạo:** Chọn **Custom permission set** > Nhấn **Next**.

![5.2.2-2](/images/5.2.2-2.png)

3. **Gán Policy:**

- Tại bước 2 - **Specify policies and permissions boundary**, chọn tab AWS managed policies.
- Tìm và tick chọn policy tương ứng (Ví dụ: `ReadOnlyAccess`).
- Nhấn **Next**.

![5.2.2-3](/images/5.2.2-3.png)

4. **Cấu hình cơ bản:**

- **Permission set name:** Nhập chính xác tên theo bảng (Ví dụ: `ProdAccessReadOnly1h`).
- **Description:** Mô tả ngắn gọn (Ví dụ: `ReadOnly access with 1h session duration`).
- **Session duration:** Chọn thời gian tương ứng (Ví dụ: 1 hour, 4 hours,...).

![5.2.2-4](/images/5.2.2-4.png)

5. **Gán Tags (Khuyến nghị):** Để dễ dàng quản lý, hãy thêm các tag sau:

- **AccessType:** ReadOnly/PowerUser/Admin.
- **Duration:** 1h/4h/8h.
- **ManagedBy:** Manual.

6. **Hoàn tất:** Review lại thông tin và nhấn **Create**.

![5.2.2-5](/images/5.2.2-5.png)

7. **Ghi nhận thông tin:** Sau khi tạo xong mỗi Permission Set, copy và lưu lại ARN của chúng. Các giá trị này sẽ được sử dụng để cấu hình biến môi trường cho Lambda Function ở các bước sau. **Cấu trúc ARN mẫu:** `arn:aws:sso:::permissionSet/ssoins-XXXXXXXXXX/ps-XXXXXXXXXXXX`.

*Lặp lại bước này cho tất cả 9 Permission Sets và ghi lại ARN của từng cái.*

![5.2.2-6](/images/5.2.2-6.png)

Ngoài ra, cũng cần tạo **3 Legacy Permission Sets** (Dùng cho backward compatibility - sẽ là Environment variables cho Lambda):

| STT | Tên Legacy Permission Set | Duration | AWS Managed Policy |
|:---:|:-------------------------:|:--------:|:------------------:|
| 1 | ProdAccessReadOnly | 12h | ReadOnlyAccess |
| 2 | ProdAccessPowerUser | 12h | PowerUserAccess |
| 3 | ProdAccessAdmin | 12h | AdministratorAccess |
