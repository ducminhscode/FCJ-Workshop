---
title : "Bảng ProvisionAccess"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.5.2 </b>
chapter : false
---

Bảng này dùng để lưu lịch sử các yêu cầu đã được xử lý (provisioned).

1. **Truy cập:** 

- Vào giao diện **AWS Console** và tìm kiếm "DynamoDB" trên thanh tìm kiếm.
- Chọn **Tables** từ menu bên trái > Chọn **Create table**.

![5.2.5.2-1](/images/5.2.5.2-1.png)

2. **Cấu hình:**

- **Table name** (Ví dụ: `pa-{env}-approval-tokens-{region}`)
- **Partition key:** ID duy nhất (Ví dụ: `token_id ` - Type: String).
- **Table settings:** Chọn **Customize settings**.
- **Read/Write capacity settings:** Chọn **On-demand**.
- **Encryption at rest:** AWS managed key.
- Chọn **Create table**.

![5.2.5.2-2](/images/5.2.5.2-2.png)

![5.2.5.2-3](/images/5.2.5.2-3.png)

![5.2.5.2-4](/images/5.2.5.2-4.png)

3. **Cấu hình TTL (Time to Live):**

- Sau khi bảng đã được tạo, chọn bảng > Vào **Actions** > Chọn **Turn on TTL**.

![5.2.5.2-5](/images/5.2.5.2-5.png)

- Nhập **TTL attribute name:** `expires_at` > Chọn **Turn on TTL**.

*Lưu ý: KHÔNG cần bật Streams cho bảng này.*

![5.2.5.2-6](/images/5.2.5.2-6.png)

![5.2.5.2-7](/images/5.2.5.2-7.png)