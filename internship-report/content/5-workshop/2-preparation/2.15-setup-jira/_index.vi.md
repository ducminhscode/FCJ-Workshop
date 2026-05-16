---
title : "Thiết lập Jira"
date :  "`r Sys.Date()`" 
weight : 15
pre: <b> 5.2.15 </b>
chapter : false
---

Truy cập vào workspace Atlassian (Ví dụ: https://your-domain.atlassian.net/jira/).

Ở thanh bên trái, nhấn vào **Spaces** > Chọn **Create space**.

![5.2.15-17](/images/5.2.15-17.png)

Confluence sẽ hiện nhiều template, chọn **Blank space**.

Đặt tên **Space name** và chọn quyền truy cập > Chọn **Create blank space**.

![5.2.15-1](/images/5.2.15-1.png)

![5.2.15-2](/images/5.2.15-2.png)

Chọn vào space vừa tạo, chọn **Space settings**.

![5.2.15-3](/images/5.2.15-3.png)

Ở thanh bên trái, chọn **Fields** > Chọn **Create a space field** > Thêm các field.

| Name | Field Type | Options |
|------------|------|-------|
| Accounts | Dropdown | sit-data, sit-app |
| Permissions | Dropdown | ReadOnly, PowerUser, Admin |
| Duration | Dropdown | 1, 4, 8 |

![5.2.15-4](/images/5.2.15-4.png)

Vào phần **Space settings** > **Request management** > **Request types** > Chọn một **Request types**.

Kéo các field đã tạo từ thanh bên phải sang **Customer request form**.

Sau đó, chọn **Edit workflow**.

![5.2.15-5](/images/5.2.15-5.png)

Chỉnh sửa workflow như hình bên dưới:

![5.2.15-6](/images/5.2.15-6.png)

Vào phần **Space settings** > **Automation** > **Create flow** > **Create from scratch**.

Tạo flow như hình bên dưới:

![5.2.15-7](/images/5.2.15-7.png)

![5.2.15-8](/images/5.2.15-8.png)

![5.2.15-9](/images/5.2.15-9.png)

![5.2.15-10](/images/5.2.15-10.png)

![5.2.15-11](/images/5.2.15-11.png)

Tương tự tạo thêm một flow như hình bên dưới:

![5.2.15-12](/images/5.2.15-12.png)

![5.2.15-13](/images/5.2.15-13.png)

![5.2.15-14](/images/5.2.15-14.png)

![5.2.15-15](/images/5.2.15-15.png)

![5.2.15-16](/images/5.2.15-16.png)

