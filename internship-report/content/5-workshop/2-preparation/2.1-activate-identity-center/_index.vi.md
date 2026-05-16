---
title : "Kích hoạt Identity Center"
date :  "`r Sys.Date()`" 
weight : 1
pre: <b> 5.2.1 </b>
chapter : false
---

Trước khi bắt đầu, chúng ta cần đảm bảo dịch vụ đã được sẵn sàng đúng với vùng mà dự án của chúng ta đang triển khai (Ví dụ: `ap-southeast-1` cho **Singapore** hoặc `us-east-1` cho **N. Virginia**).

1. **Đăng nhập:** Truy cập vào [AWS Management Console](https://console.aws.amazon.com/) bằng **Management Account** (tài khoản gốc của tổ chức).

![5.2.1-1](/images/5.2.1-1.png)

2. **Truy cập dịch vụ:** Tìm kiếm "IAM Identity Center" trên thanh tìm kiếm và chọn dịch vụ.

![5.2.1-2](/images/5.2.1-2.png)

3. **Kích hoạt:** Nếu dịch vụ chưa được bật, nhấn **Enable** và chọn chế độ **AWS Organizations mode** để có khả năng quản lý đa tài khoản tối ưu nhất.

4. **Lưu trữ thông tin:** Sau khi kích hoạt thành công, hãy copy và lưu lại hai giá trị sau vào file cấu hình dự án.
- **Identity Center Instance ARN** (Ví dụ: `arn:aws:sso:::instance/ssoins-XXXXXXXXXX`).
- **Identity Store ID** (Ví dụ: `d-XXXXXXXXXX`).

![5.2.1-3](/images/5.2.1-3.png)

**Mẹo tìm kiếm:** Bạn có thể tìm thấy các giá trị này ngay tại tab **Settings** trong giao diện quản trị của IAM Identity Center.