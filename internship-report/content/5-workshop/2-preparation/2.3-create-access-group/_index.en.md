---
title : "Create Access Groups"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 5.2.3 </b>
chapter : false
---

**Access Groups** are groups in IAM Identity Center. In the v2.0 architecture, each group represents a unique combination of **Account × Access Type × Duration Tier**. 
- **When granting access:** Lambda only needs to add the user to the corresponding group > The user automatically receives access permissions to the Account. 
- **When revoking access:** Lambda removes the user from the group > All current credentials of the user will become invalid within a maximum of 60 seconds.

### Access Groups Table to Create

Based on the standard configuration (2 accounts × 3 access types × 3 duration tiers), a total of 18 groups need to be created:

 | STT | Group Name  | Account | Access Type | Duration | 
 |:---:|:---------:|:-------:|:-----------:|:--------:| 
 | 1 | pa-{env}-application-ReadOnly-1h | application | ReadOnly | 1h | 
 | 2 | pa-{env}-application-ReadOnly-4h | application | ReadOnly | 4h | 
 | 3 | pa-{env}-application-ReadOnly-8h | application | ReadOnly | 8h | 
 | 4 | pa-{env}-application-PowerUser-1h | application | PowerUser | 1h | 
 | 5 | pa-{env}-application-PowerUser-4h | application | PowerUser | 4h | 
 | 6 | pa-{env}-application-PowerUser-8h | application | PowerUser | 8h | 
 | 7 | pa-{env}-application-Admin-1h | application | Admin | 1h | 
 | 8 | pa-{env}-application-Admin-4h | application | Admin | 4h | 
 | 9 | pa-{env}-application-Admin-8h | application | Admin | 8h | 
 | 10 | pa-{env}-data-ReadOnly-1h | data | ReadOnly | 1h | 
 | 11 | pa-{env}-data-ReadOnly-4h | data | ReadOnly | 4h | 
 | 12 | pa-{env}-data-ReadOnly-8h | data | ReadOnly | 8h | 
 | 13 | pa-{env}-data-PowerUser-1h | data | PowerUser | 1h | 
 | 14 | pa-{env}-data-PowerUser-4h | data | PowerUser | 4h | 
 | 15 | pa-{env}-data-PowerUser-8h | data | PowerUser | 8h | 
 | 16 | pa-{env}-data-Admin-1h | data | Admin | 1h | 
 | 17 | pa-{env}-data-Admin-4h | data | Admin | 4h | 
 | 18 | pa-{env}-data-Admin-8h | data | Admin | 8h | 

 **Note:** Replace `{env}` with the environment name (Example: `sit`, `uat`, `prod`).

 ### Detailed Implementation Guide

 Perform the following steps for each group in the list above:

1. **Access:** Go to **IAM Identity Center** > Select **Groups** from the left menu > Click **Create group**.

![5.2.3-1](/images/5.2.3-1.png)

2. **Configuration:**
- **Group name:** Enter the exact name according to the convention (Example: `pa-sit-application-ReadOnly-1h`).
- **Description:** Enter a description for easier management (Example: `JIT Access: application - ReadOnly - 1h`).

![5.2.3-2](/images/5.2.3-2.png)

3. **Members:** DO NOT add any users to the group at this stage. The group must remain empty.

4. **Complete:** Click **Create group**.

5. **Record Information:** After successful creation, click on the newly created group name. Copy and save the Group ID (in UUID format such as: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`).

![5.2.3-3](/images/5.2.3-3.png)

*Repeat for all 18 groups and record the Group ID of each one.*

![5.2.3-4](/images/5.2.3-4.png)
