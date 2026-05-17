---
title : "Create Permission Sets"
date :  "`r Sys.Date()`" 
weight : 2
pre: <b> 5.2.2 </b>
chapter : false
---

**Permission Sets** define what permissions users have and how long their session duration lasts. In the new architecture, we need to create a separate Permission Set for each combination of **Access Type** and **Duration Tier**.

### Permission Sets Table to Create

Below is the list of 9 standard Permission Sets (based on 3 access types x 3 duration tiers):

| STT | Permission Set Name | Access Type | Duration | AWS Managed Policy |
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

**Tip:** If the project requires more duration tiers (For example: adding 2h, 12h), create additional sets according to the formula above.

### Detailed Implementation Guide

Repeat the following steps for each Permission Set listed in the table above:

1. **Access:** Go to **IAM Identity Center** > Select **Permission sets** from the left menu > Click **Create permission set**.

![5.2.2-1](/images/5.2.2-1.png)

2. **Initialize:** Select **Custom permission set** > Click **Next**.

![5.2.2-2](/images/5.2.2-2.png)

3. **Assign Policy:**
- In step 2 - **Specify policies and permissions boundary**, select the AWS managed policies tab.
- Search for and check the corresponding policy (Example: `ReadOnlyAccess`).
- Click **Next**.

![5.2.2-3](/images/5.2.2-3.png)

4. **Basic Configuration:**
- **Permission set name:** Enter the exact name according to the table (Example: `ProdAccessReadOnly1h`).
- **Description:** A short description (Example: `ReadOnly access with 1h session duration`).
- **Session duration:** Select the corresponding duration (Example: 1 hour, 4 hours,...).

![5.2.2-4](/images/5.2.2-4.png)

5. **Assign Tags (Recommended):** To make management easier, add the following tags:
- **AccessType:** ReadOnly/PowerUser/Admin.
- **Duration:** 1h/4h/8h.
- **ManagedBy:** Manual.

6. **Complete:** Review the information and click **Create**.

![5.2.2-5](/images/5.2.2-5.png)

7. **Record Information:** After creating each Permission Set, copy and save its ARN. These values will be used to configure environment variables for the Lambda Function in later steps. **Sample ARN structure:** `arn:aws:sso:::permissionSet/ssoins-XXXXXXXXXX/ps-XXXXXXXXXXXX`.

*Repeat this step for all 9 Permission Sets and record the ARN of each one.*

![5.2.2-6](/images/5.2.2-6.png)

Additionally, you also need to create **3 Legacy Permission Sets** (Used for backward compatibility - will be Environment variables for Lambda):

| STT | Legacy Permission Set Name | Duration | AWS Managed Policy |
|:---:|:-------------------------:|:--------:|:------------------:|
| 1 | ProdAccessReadOnly | 12h | ReadOnlyAccess |
| 2 | ProdAccessPowerUser | 12h | PowerUserAccess |
| 3 | ProdAccessAdmin | 12h | AdministratorAccess |
