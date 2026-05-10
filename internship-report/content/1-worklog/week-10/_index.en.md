---
title : "Worklog Week 10"
date :  "`r Sys.Date()`" 
weight : 10
pre: <b> 1.10 </b>
chapter : false
---

### Week 10 Objectives:

- Understand and practice cost and performance analysis on AWS using Amazon Glue and Amazon Athena.
- Get familiar with building and operating a serverless data pipeline on AWS following a modern Data Lake architecture approach.
- Strengthen knowledge and hands-on skills with NoSQL databases (DynamoDB) using AWS SDK for Python (boto3).
- Understand how to organize data, automate ETL workflows, and visualize data using Amazon QuickSight within a serverless architecture.
- Complete the Email Token feature in the access management system, including handling approve/reject actions directly from email.
- Integrate automatic status updates to Jira tickets to synchronize the access approval workflow.
- Improve system stability and fault tolerance by implementing retry logic for AWS and Jira API calls.
- Handle system edge cases such as expired tokens, non-existent users, or deleted groups.
- Continue enhancing system design thinking for serverless architectures combining automation, monitoring, and fault tolerance on AWS.
- Strengthen real-world implementation skills, resource management, and cost optimization when working with AWS data services.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Practice setting up a cost and performance analytics system using Amazon Glue and Amazon Athena:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Introduction to Amazon Glue and Amazon Athena<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare the environment: Create an S3 Bucket, configure Cost and Usage Reports, and set up IAM permissions<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Analyze cost and performance: Use AWS Glue Crawlers and query data with Amazon Athena<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 27/04/2026 | 27/04/2026 | [Cost and performance analysis with AWS Glue and Amazon Athena](https://000040.awsstudygroup.com/) |
| Tue | - Practice getting familiar with AWS NoSQL databases through Python ([Module 07 - Lab 60](https://drive.google.com/drive/folders/19P-MVrC5ksfOOYbEyYGOz9-zro-sqTK2?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Introduction to Amazon DynamoDB<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare the environment: Configure AWS Credentials and install the boto3 library<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Getting started with the AWS SDK: Create tables, insert data, query data, update, and delete records<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 28/04/2026 | 28/04/2026 | [Work with Amazon DynamoDB](https://000060.awsstudygroup.com/) |
| Wed | - Practice building a serverless data pipeline:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare the environment: Create the required IAM permissions, create S3 Buckets, and configure the working Availability Zone<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare data: Upload sample datasets to Amazon S3<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Ingest data with AWS Glue<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Build the Data Pipeline<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Query data using Amazon Athena<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Visualize data with Amazon QuickSight<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 29/04/2026 | 29/04/2026 | [Building a Datalake with Your Data](https://000070.awsstudygroup.com/) |
| Thu | - Complete Email Token Logic:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Develop functions to process approval/rejection links from emails<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Implement logic to update Jira ticket status directly when a Manager clicks the Approve/Reject button | 30/04/2026 | 30/04/2026 |  |
| Fri | - Error Handling and Edge Cases:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Implement retry logic for failed AWS/Jira API calls<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Handle special cases such as deleted users, non-existent groups, or expired token errors | 01/05/2026 | 01/05/2026 |  |
| Sat | - Rest and prepare for the upcoming week | 02/05/2026 | 02/05/2026 | |
| Sun | - Rest and prepare for the upcoming week | 03/05/2026 | 03/05/2026 | |

### Week 10 Achievements:

- Theoretical Knowledge:
  - Understand the process of building a cost and performance analytics system on AWS by combining Amazon Glue, Amazon Athena, and AWS Cost & Usage Report.
  - Learn the role of AWS Glue in crawling data, generating metadata catalogs, and enabling queries via Athena.
  - Understand the serverless Data Pipeline model on AWS, including storage, ETL, querying, and data visualization layers.
  - Strengthen knowledge of DynamoDB and how to interact with NoSQL databases using AWS SDK for Python (boto3).
  - Gain a deeper understanding of error handling mechanisms, retry strategies, and common edge cases in distributed systems.
- Hands-on Labs:
  - **Lab 40:** Build a cost and performance analytics system using AWS Glue and Amazon Athena, including Cost & Usage Report configuration, Glue Crawler setup, and data querying.
  - **Lab 60:** Work with Amazon DynamoDB using Python and boto3, including table creation, data insertion, querying, updating, and deletion.
  - **Lab 70:** Build a complete serverless data pipeline on AWS using S3, Glue, Athena, and QuickSight for data processing and visualization.
- System Development:
  - Complete Email Token logic for approve/reject access workflows directly from email.
  - Implement automatic Jira ticket status updates when a Manager approves or rejects a request.
  - Add retry logic for AWS and Jira API calls to improve system stability under transient failures.
  - Handle critical edge cases such as expired tokens, missing users, or deleted groups.
  - Improve fault tolerance and automation in the access management workflow.
- Skills Development:
  - Enhance the ability to design and operate serverless data systems on AWS.
  - Improve practical skills with DynamoDB and AWS SDK using Python.
  - Develop system design thinking focused on resilience, error handling, and exception management.
  - Strengthen the ability to integrate multiple AWS services into a unified real-world system.
  - Improve technical documentation reading, lab execution, and applied engineering skills.
- Resource Management:
  - Perform resource cleanup after each lab to avoid unexpected costs.
  - Monitor and optimize resource usage across Glue, Athena, S3, and QuickSight.
  - Maintain cost-awareness and performance optimization mindset when building AWS data analytics systems.