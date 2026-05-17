---
title : "Create Lambda Functions"
date :  "`r Sys.Date()`" 
weight : 10
pre: <b> 5.2.10 </b>
chapter : false
---

In this step, we will deploy 3 main Lambda Functions for the system:

| Lambda Function | Function |
|-----------------|----------|
| Executor | Process AWS access permission requests |
| Expiry | Automatically revoke permissions when session expires |
| Email Approval | Send approval email and handle approve/reject |

### Functions to create

1. [Executor](2.10.1-executor/)
2. [Expiry](2.10.2-expiry/)
3. [Email Approval](2.10.3-email-approval/)