---
title : "Thiết lập CloudWatch Alarms"
date :  "`r Sys.Date()`" 
weight : 13
pre: <b> 5.2.13 </b>
chapter : false
---

### Alarm cho Lambda Executor Errors

- Truy cập vào giao diện **AWS Console** và tìm kiếm "CloudWatch" trên thanh tìm kiếm.

![5.2.13-1](/images/5.2.13-1.png)

- Chọn **Create alarm**.

![5.2.13-2](/images/5.2.13-2.png)

- Chọn **Select metric**.

![5.2.13-3](/images/5.2.13-3.png)

- Đi theo đường dẫn: **Lambda** > **By Function Name** > Chọn metric có **FunctionName:** `pa-{env}-executor-{region}` và **Metric name:** Errors > Chọn **Select metric**.

![5.2.13-4](/images/5.2.13-4.png)

![5.2.13-5](/images/5.2.13-5.png)

![5.2.13-6](/images/5.2.13-6.png)

- Cấu hình Metric:
  - **Statistic:** `Sum`.
  - **Period:** Chọn **5 minutes**.

![5.2.13-7](/images/5.2.13-7.png)

- Cấu hình Threshold:
  - **Threshold type:** Chọn **Static**.
  - **Condition:** Chọn **Greater**.
  - **than...:** `5`.
  - **Datapoints to alarm:** `2` out of `2`.
  - **Missing data treatment:** Chọn **Treat missing data as good (not breaching threshold)**.
  - Chọn **Next**.

![5.2.13-8](/images/5.2.13-8.png)

- Chọn **Next**.

![5.2.13-9](/images/5.2.13-9.png)

- **Alarm name:** `pa-{env}-executor-{region}-errors`.
- Chọn **Next**.

![5.2.13-10](/images/5.2.13-10.png)

- Kiểm tra lại và chọn **Create alarm**.

![5.2.13-11](/images/5.2.13-11.png)

![5.2.13-12](/images/5.2.13-12.png)

*Làm tương tự để tạo alarm `pa-{env}-expiry-{region}-errors` và `pa-{env}-email-approval-{region}-errors` cho `pa-{env}-expiry-{region}` và `pa-{env}-email-approval-{region}`.*

![5.2.13-13](/images/5.2.13-13.png)