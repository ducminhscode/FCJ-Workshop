---
title : "Worklog Week 9"
date :  "`r Sys.Date()`" 
weight : 9
pre: <b> 1.9 </b>
chapter : false
---

### Week 9 Objectives:

- Understand how to process and protect sensitive data in logging systems, especially data masking mechanisms and access control in Amazon CloudWatch.
- Learn how to balance security and operability (reducing incident resolution time) when working with real-world logs.
- Build and understand the overall architecture of a Data Lake on AWS, from data ingestion and storage to analysis and visualization.
- Get familiar with data analytics services such as Amazon Athena and Amazon QuickSight, as well as the process of building a Data Catalog and processing data.
- Improve DynamoDB data design and optimization skills, including how to build appropriate data models for different use cases.
- Complete the access management system by developing a revocation flow for permission control.
- Automate access revocation using DynamoDB Streams and TTL mechanisms, minimizing manual operations.
- Integrate Amazon SES notifications to ensure users are promptly informed when access permissions change.
- Continue strengthening the mindset of building a complete serverless system, combining data processing, security and automation on AWS.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Research and translate the blog post **Handling sensitive log data using Amazon CloudWatch**: Focus on key topics such as data masking mechanisms using data protection policies, methods for detecting and masking sensitive information (PII), access control with IAM using the logs:Unmask permission, as well as temporary privilege escalation workflows and auditing with AWS CloudTrail. Through this, gain a deeper understanding of how to balance data security and operational efficiency (MTTR) when handling logs in real-world systems. | 20/04/2026 | 20/04/2026 | [Blog 04](https://aws.amazon.com/vi/blogs/mt/handling-sensitive-log-data-using-amazon-cloudwatch/) |
| Tue | - Practice building a complete Data Lake system:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare the environment: Create IAM roles and configure initial settings so that AWS services can communicate with each other<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Collect and store data<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create a Data Catalog<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Transform data<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Analyze data using Amazon Athena<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Model data and build dashboards with Amazon QuickSight<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 21/04/2026 | 21/04/2026 | [Data Lake on AWS](https://000035.awsstudygroup.com/) |
| Wed | - Practice becoming familiar with designing, managing and optimizing data on DynamoDB through real-world scenarios. The hands-on exercise is divided into different learning tracks: Beginner Track (Getting familiar with the AWS Console and CLI), Advanced Data Design (Learning NoSQL data modeling), Real-world Applications & AI. | 22/04/2026 | 22/04/2026 | [Amazon DynamoDB Immersion Day](https://000039.awsstudygroup.com/) |
| Thu | - Build Lambda Revocation:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Develop the revoke-access function: Implement logic to remove users from IAM Identity Center Groups<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Integrate the SES notification module to inform users when their access permissions expire | 23/04/2026 | 23/04/2026 |  |
| Fri | - Build Lambda Revocation:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure DynamoDB Streams processing: Implement logic for Lambda to automatically trigger when an item in the AccessSessions table is deleted due to expiration (TTL) | 24/04/2026 | 24/04/2026 |  |
| Sat | - Rest and prepare for the upcoming week | 25/04/2026 | 25/04/2026 | |
| Sun | - Rest and prepare for the upcoming week | 26/04/2026 | 26/04/2026 | |

### Week 9 Achievements:

- Theoretical Knowledge:
  - Understand the mechanisms for processing and protecting sensitive data in logging systems with CloudWatch, especially how data protection policies are used to detect and mask PII data.
  - Learn how to control log access using IAM (logs:Unmask permission), as well as the concept of temporary privilege escalation and auditing via AWS CloudTrail.
  - Understand the trade-off between data security and operational efficiency (MTTR) when handling real-world incidents.
  - Gain a solid understanding of the overall architecture of a Data Lake on AWS, including storage, cataloging, processing and analytics components.
  - Understand how services such as Amazon Athena (data querying) and Amazon QuickSight (data visualization) work and their roles in the ecosystem.
  - Strengthen NoSQL data modeling knowledge in DynamoDB, with a focus on access-pattern-driven design.
- Hands-on Labs:
  - **Lab 35:** Build a complete Data Lake system on AWS, including data ingestion, Data Catalog creation, data processing, querying with Athena and visualization with QuickSight.
  - **Lab 39:** Practice DynamoDB data design and optimization, exploring multiple approaches from basic to advanced using CLI, Console and data modeling techniques.
- System Development:
  - Build a complete Lambda Revocation function to remove user access from IAM Identity Center Groups.
  - Integrate DynamoDB Streams and TTL to automatically trigger permission revocation when sessions expire.
  - Connect the system with Amazon SES to notify users when their access permissions are revoked.
  - Complete the full lifecycle of access control (provision → usage → expiration → revocation) in the system.
- Skills Development:
  - Improve the ability to design and implement AWS data processing systems based on the Data Lake architecture.
  - Enhance DynamoDB skills, especially schema design optimized for performance and cost efficiency.
  - Strengthen security thinking when handling logs and sensitive data in cloud systems.
  - Develop skills in building fully automated serverless systems.
  - Improve the ability to read technical documentation and apply it in real-world scenarios.
- Resource Management:
  - Perform resource cleanup after each lab to ensure cost control.
  - Maintain awareness of monitoring and optimizing resources when working with data and analytics services.