---
title : "Set up CloudWatch Alarms"
date :  "`r Sys.Date()`" 
weight : 13
pre: <b> 5.2.13 </b>
chapter : false
---

### Alarm for Lambda Executor Errors
- Go to the **AWS Console** interface and search for "CloudWatch" in the search bar.

![5.2.13-1](/images/5.2.13-1.png)

- Select **Create alarm**.

![5.2.13-2](/images/5.2.13-2.png)

- Select **Select metric**.

![5.2.13-3](/images/5.2.13-3.png)

- Follow this path: **Lambda** > **By Function Name** > Select the metric with **FunctionName:** `pa-{env}-executor-{region}` and **Metric name:** Errors > Select **Select metric**.

![5.2.13-4](/images/5.2.13-4.png)

![5.2.13-5](/images/5.2.13-5.png)

![5.2.13-6](/images/5.2.13-6.png)

- Configure Metric:
  - **Statistic:** `Sum`.
  - **Period:** Select **5 minutes**.

![5.2.13-7](/images/5.2.13-7.png)

- Configure Threshold:
  - **Threshold type:** Select **Static**.
  - **Condition:** Select **Greater**.
  - **than...:** `5`.
  - **Datapoints to alarm:** `2` out of `2`.
  - **Missing data treatment:** Select **Treat missing data as good (not breaching threshold)**.
  - Select **Next**.

![5.2.13-8](/images/5.2.13-8.png)

- Select **Next**.

![5.2.13-9](/images/5.2.13-9.png)

- **Alarm name:** `pa-{env}-executor-{region}-errors`.
- Select **Next**.

![5.2.13-10](/images/5.2.13-10.png)

- Review and select **Create alarm**.

![5.2.13-11](/images/5.2.13-11.png)

![5.2.13-12](/images/5.2.13-12.png)

*Do the same to create alarms `pa-{env}-expiry-{region}-errors` and `pa-{env}-email-approval-{region}-errors` for `pa-{env}-expiry-{region}` and `pa-{env}-email-approval-{region}`.*

![5.2.13-13](/images/5.2.13-13.png)