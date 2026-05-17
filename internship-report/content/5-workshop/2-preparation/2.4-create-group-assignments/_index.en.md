---
title : "Create Group Assignments"
date :  "`r Sys.Date()`" 
weight : 4
pre: <b> 5.2.4 </b>
chapter : false
---

**Group Assignment** is the operation of assigning each Access Group **permanently** to the corresponding AWS account along with the appropriate Permission Set. This is considered the fixed "connection wire" in the IAM Identity Center system - once a user is added to a group, they will automatically gain access to that account with the defined privileges.

### Assignments Table to Create

Before proceeding, ensure that you already have the mapping list between groups and permissions:

| STT | Group | AWS Account | Permission Set |
|:---:|:-----:|:-----------:|:--------------:|
| 1 | pa-{env}-application-ReadOnly-1h | application | ProdAccessReadOnly1h | 
| 2 | pa-{env}-application-ReadOnly-4h | application | ProdAccessReadOnly4h | 
| 3 | pa-{env}-application-ReadOnly-8h | application | ProdAccessReadOnly8h | 
| 4 | pa-{env}-application-PowerUser-1h | application | ProdAccessPowerUser1h | 
| 5 | pa-{env}-application-PowerUser-4h | application | ProdAccessPowerUser4h | 
| 6 | pa-{env}-application-PowerUser-8h | application | ProdAccessPowerUser8h | 
| 7 | pa-{env}-application-Admin-1h | application | ProdAccessAdmin1h | 
| 8 | pa-{env}-application-Admin-4h | application | ProdAccessAdmin4h | 
| 9 | pa-{env}-application-Admin-8h | application | ProdAccessAdmin8h | 
| 10 | pa-{env}-data-ReadOnly-1h | data | ProdAccessReadOnly1h | 
| 11 | pa-{env}-data-ReadOnly-4h | data | ProdAccessReadOnly4h | 
| 12 | pa-{env}-data-ReadOnly-8h | data | ProdAccessReadOnly8h | 
| 13 | pa-{env}-data-PowerUser-1h | data | ProdAccessPowerUser1h | 
| 14 | pa-{env}-data-PowerUser-4h | data | ProdAccessPowerUser4h | 
| 15 | pa-{env}-data-PowerUser-8h | data | ProdAccessPowerUser8h | 
| 16 | pa-{env}-data-Admin-1h | data | ProdAccessAdmin1h | 
| 17 | pa-{env}-data-Admin-4h | data | ProdAccessAdmin4h | 
| 18 | pa-{env}-data-Admin-8h | data | ProdAccessAdmin8h | 

### Detailed Implementation Guide

Repeat the following steps for each row in the table above:

1. **Access:** Go to **IAM Identity Center** > Select **Groups** from the left menu > Select the target group.

![5.2.4-1](/images/5.2.4-1.png)

2. **Assign Account:** Switch to the **AWS accounts** tab > Select **Assign accounts**.

![5.2.4-2](/images/5.2.4-2.png)

3. **Assign Permission Sets:** Search for and check the corresponding **Permission sets** for the group's AWS accounts > Select **Assign**.

![5.2.4-3](/images/5.2.4-3.png)

![5.2.4-4](/images/5.2.4-4.png)

*Repeat for all 18 assignments.*
