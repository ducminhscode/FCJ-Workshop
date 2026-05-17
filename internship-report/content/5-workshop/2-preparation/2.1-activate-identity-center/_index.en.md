---
title : "Activate Identity Center"
date :  "`r Sys.Date()`" 
weight : 1
pre: <b> 5.2.1 </b>
chapter : false
---

Before starting, we need to ensure that the service is ready in the correct Region where our project is being deployed (For example: `ap-southeast-1` for **Singapore** or `us-east-1` for **N. Virginia**).

1. **Log in:** Access the [AWS Management Console](https://console.aws.amazon.com/) using the **Management Account** (the organization's root account).

![5.2.1-1](/images/5.2.1-1.png)

2. **Access the service:** Search for "IAM Identity Center" in the search bar and select the service.

![5.2.1-2](/images/5.2.1-2.png)

3. **Activate:** If the service is not yet enabled, click **Enable** and choose **AWS Organizations mode** for the most optimal multi-account management capability.

4. **Store information:** After successful activation, copy and save the following two values into the project configuration file.
- **Identity Center Instance ARN** (Example: `arn:aws:sso:::instance/ssoins-XXXXXXXXXX`).
- **Identity Store ID** (Example: `d-XXXXXXXXXX`).

![5.2.1-3](/images/5.2.1-3.png)

**Search tip:** You can find these values directly in the **Settings** tab within the IAM Identity Center administration interface.
