---
title : "Create Simple Email Service"
date :  "`r Sys.Date()`" 
weight : 7
pre: <b> 5.2.7 </b>
chapter : false
---

The system uses Amazon Simple Email Service to send emails for the following functions:
- Send approval request emails to admins.
- Send successful permission grant notification emails.
- Send access revoke notification emails.
- Send approval links containing signed tokens.
- Monitor email delivery status through CloudWatch metrics.

### Verify Sender Email

To allow AWS SES to send emails on your behalf, the sender email address must be verified as owned by you.

1. **Access:**
- Go to the **AWS Console** interface and search for "Amazon SES" in the search bar.

![5.2.7-1](/images/5.2.7-1.png)

- Select **Identities** from the left menu > Select **Create identity**.

![5.2.7-2](/images/5.2.7-2.png)

2. **Configuration:**
- **Identity type:** Select **Email address**.
- **Email address:** `SES_SENDER_EMAIL` (Example: `noreply@company.com`).
- **Assign a default configuration set:** None.
- **Assign to a tenant:** None.
- Select **Create identity**.

![5.2.7-3](/images/5.2.7-3.png)

After creating the identity, **AWS SES** will send a confirmation email. Open the sender email inbox and click the link to **Verify this email address**.

After successful verification, the **identity status** will change to **Verified**.

![5.2.7-4](/images/5.2.7-4.png)

### Create Configuration Set

A **Configuration Set** helps monitor email sending metrics and manage email sending reputation policies.

1. **Access:**
- Go to the **AWS Console** interface and search for "Amazon SES" in the search bar.
- Select **Configuration sets** from the left menu > Select **Create set**.

![5.2.7-5](/images/5.2.7-5.png)

2. **Configuration:**
- **Configuration set name:** `pa-{env}-email-approval-{region}`.
- **Reputation metrics:** Select **Enable**.
- Select **Create set**.

![5.2.7-6](/images/5.2.7-6.png)

### Add Event Destination

An **Event Destination** allows SES to send email events to CloudWatch for monitoring.

1. Click the configuration set you just created > Go to the **Event destinations** tab > Select **Add destination**.

![5.2.7-7](/images/5.2.7-7.png)

![5.2.7-8](/images/5.2.7-8.png)

2. **Configuration:**
- In step 1, select all **Event types** > Select **Next**.

![5.2.7-9](/images/5.2.7-9.png)

- In step 2:
  - **Destination type:** Select **Amazon CloudWatch**.
  - **Dimension name:** `ses:configuration-set`.
  - **Default value:** `default`.
  - **Value source:** `Message tag`.
  - Select **Next**.

![5.2.7-10](/images/5.2.7-10.png)

- In step 3, review the information and click **Add destination**.

![5.2.7-11](/images/5.2.7-11.png)

![5.2.7-12](/images/5.2.7-12.png)

***Note about Sandbox Mode:** If your AWS account is in SES Sandbox mode, you can only send emails to verified email addresses. To send emails to any users within the company, you need to request "Production Access" for the SES service through AWS Support.*
