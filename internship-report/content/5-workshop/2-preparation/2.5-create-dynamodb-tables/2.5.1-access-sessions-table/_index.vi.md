---
title : "Bảng AccessSessions"
date :  "`r Sys.Date()`" 
weight : 1
pre: <b> 5.2.5.1 </b>
chapter : false
---

Bảng này dùng để quản lý các phiên truy cập của người dùng vào hệ thống.

1. **Truy cập:** 
- Vào giao diện **AWS Console** và tìm kiếm "DynamoDB" trên thanh tìm kiếm.

![5.2.5.1-1](/images/5.2.5.1-1.png)

- Chọn **Tables** từ menu bên trái > Chọn **Create table**.

![5.2.5.1-2](/images/5.2.5.1-2.png)

2. **Cấu hình:**
- **Table name** (Ví dụ: `pa-{env}-access-sessions-{region}`).
- **Partition key:** ID duy nhất của phiên truy cập (Ví dụ: `sessionId` - Type: String).
- **Table settings:** Chọn **Customize settings**.
- **Read/Write capacity settings:** Chọn **On-demand**.
- **Secondary indexes:**
  - Chọn **Create global index** (lần 1):
    - **Index name:** `requestId-index`.
    - **Partition key:** `requestId` - **Attribute**, `String` - **Data type**.
    - **Attribute projections:** All.
  - Chọn **Create global index** (lần 2):
    - **Index name:** `requester-index`.
    - **Partition key:** `requester` - **Attribute**, `String` - **Data type**.
    - **Attribute projections:** All.
- **Encryption at rest:** AWS managed key.
- Chọn **Create table**.

![5.2.5.1-3](/images/5.2.5.1-3.png)

![5.2.5.1-4](/images/5.2.5.1-4.png)

![5.2.5.1-5](/images/5.2.5.1-5.png)

![5.2.5.1-6](/images/5.2.5.1-6.png)

![5.2.5.1-7](/images/5.2.5.1-7.png)

![5.2.5.1-8](/images/5.2.5.1-8.png)

![5.2.5.1-20](/images/5.2.5.1-20.png)

![5.2.5.1-9](/images/5.2.5.1-9.png)

3. **Cấu hình TTL (Time to Live):**
- Sau khi bảng đã được tạo, chọn bảng > Vào **Actions** > Chọn **Turn on TTL**.

![5.2.5.1-10](/images/5.2.5.1-10.png)

![5.2.5.1-11](/images/5.2.5.1-11.png)

- Nhập **TTL attribute name:** `ttl` > Chọn **Turn on TTL**.

*Lưu ý: Điều này giúp hệ thống tự động dọn dẹp các phiên làm việc đã hết hạn để tiết kiệm dung lượng lưu trữ.*

![5.2.5.1-12](/images/5.2.5.1-12.png)

![5.2.5.1-13](/images/5.2.5.1-13.png)

- Sau khi bảng đã được tạo, chọn bảng > Vào tab **Exports and streams**.

![5.2.5.1-14](/images/5.2.5.1-14.png)

- Ở phần **DynamoDB stream details**, chọn **Turn on**.

![5.2.5.1-15](/images/5.2.5.1-15.png)

- Cấu hình **View type:** New and old images > Chọn **Turn on stream**.

![5.2.5.1-16](/images/5.2.5.1-16.png)

![5.2.5.1-17](/images/5.2.5.1-17.png)

- Bật **Point-in-Time Recovery:** Vào tab **Backups** > Chọn **Edit** (Point-in-time recovery (PITR)) > Tick vào **Turn on point-in-time recovery** > Chọn **Save changes**.

![5.2.5.1-18](/images/5.2.5.1-18.png)

![5.2.5.1-19](/images/5.2.5.1-19.png)

4. **Ghi nhận thông tin:** Sau khi tạo thành công, click vào tên bảng vừa tạo. Copy và lưu lại **Amazon Resource Name (ARN)** (Ví dụ: `arn:aws:dynamodb:{region}:{account_id}:table/pa-{env}-access-sessions-{region}`), **Latest stream ARN** (Ví dụ: `arn:aws:dynamodb:{region}:{account_id}:table/pa-{env}-access-sessions-{region}/stream/...`).

![5.2.5.1-21](/images/5.2.5.1-21.png)

![5.2.5.1-22](/images/5.2.5.1-22.png)