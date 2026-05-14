---
title : "Tạo Lambda Layer"
date :  "`r Sys.Date()`" 
weight : 9
pre: <b> 5.2.9 </b>
chapter : false
---

Truy cập vào [đường dẫn](https://drive.google.com/drive/folders/1d4NfTtJsv90yf4kwihjYupCDF__Mlauh?usp=sharing) sau để lấy 4 file chứa **Lambda Packages**.

1. **Truy cập:**

- Vào giao diện **AWS Console** và tìm kiếm "Lambda" trên thanh tìm kiếm.

![5.2.9-1](/images/5.2.9-1.png)

- Chọn **Layers** từ menu bên trái > Chọn **Create layer**.

![5.2.9-2](/images/5.2.9-2.png)

2. **Cấu hình:**

- **Name:** `pa-{env}-dependencies-{region}`.
- **Description:** `Shared dependencies for Production Access Portal Lambda functions`.
- **Upload:** Upload file **dependencies-layer.zip**.
- **Compatible runtimes:** Chọn **Python 3.12**.
- Chọn **Create**.

![5.2.9-3](/images/5.2.9-3.png)

Sau khi tạo thành công, chọn layer và copy **Version ARN** (Ví dụ: `arn:aws:lambda:{region}:{account_id}:layer:pa-{env}-dependencies-{region}:1`).

![5.2.9-4](/images/5.2.9-4.png)