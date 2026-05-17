---
title : "Set up Jira"
date :  "`r Sys.Date()`" 
weight : 15
pre: <b> 5.2.15 </b>
chapter : false
---

Access your Atlassian workspace (Example: https://your-domain.atlassian.net/jira/).

On the left sidebar, click **Spaces** > Select **Create space**.

![5.2.15-17](/images/5.2.15-17.png)

Confluence will show many templates, select **Blank space**.

Set the **Space name** and choose access permissions > Select **Create blank space**.

![5.2.15-1](/images/5.2.15-1.png)

![5.2.15-2](/images/5.2.15-2.png)

Select the newly created space, then select **Space settings**.

![5.2.15-3](/images/5.2.15-3.png)

On the left sidebar, select **Fields** > Select **Create a space field** > Add the following fields.

| Name | Field Type | Options |
|------------|------|-------|
| Accounts | Dropdown | sit-data, sit-app |
| Permissions | Dropdown | ReadOnly, PowerUser, Admin |
| Duration | Dropdown | 1, 4, 8 |

![5.2.15-4](/images/5.2.15-4.png)

Go to **Space settings** > **Request management** > **Request types** > Select a **Request type**.

Drag the created fields from the right sidebar to the **Customer request form**.

Then, select **Edit workflow**.

![5.2.15-5](/images/5.2.15-5.png)

Edit the workflow as shown in the image below:

![5.2.15-6](/images/5.2.15-6.png)

Go to **Space settings** > **Automation** > **Create flow** > **Create from scratch**.

Create the flow as shown in the images below:

![5.2.15-7](/images/5.2.15-7.png)

![5.2.15-8](/images/5.2.15-8.png)

![5.2.15-9](/images/5.2.15-9.png)

![5.2.15-10](/images/5.2.15-10.png)

![5.2.15-11](/images/5.2.15-11.png)

Similarly, create another flow as shown in the images below:

![5.2.15-12](/images/5.2.15-12.png)

![5.2.15-13](/images/5.2.15-13.png)

![5.2.15-14](/images/5.2.15-14.png)

![5.2.15-15](/images/5.2.15-15.png)

![5.2.15-16](/images/5.2.15-16.png)