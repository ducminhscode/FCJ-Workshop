---
title : "Worklog Week 8"
date :  "`r Sys.Date()`" 
weight : 8
pre: <b> 1.8 </b>
chapter : false
---

### Week 8 Objectives:

- Master the core concepts of databases, including relational models, NoSQL, optimization mechanisms and the differences between OLTP and OLAP.
- Gain a clear understanding of the architecture, characteristics and use cases of AWS database services such as Amazon RDS, Aurora, Redshift and ElastiCache.
- Compare database services to select the most suitable solution for different real-world scenarios.
- Practice migrating databases to AWS using AWS DMS, understand the migration workflow and learn how to handle common migration issues.
- Deploy a complete database system on AWS, from networking infrastructure to application connectivity.
- Continue developing the serverless system by building a Lambda Executor to process requests from API Gateway or webhooks.
- Integrate AWS services such as Secrets Manager, IAM Identity Center and Amazon SES into workflows to automate access provisioning.
- Complete request processing logic, input validation and error-handling mechanisms within the system.
- Improve the ability to design and deploy real-world backend systems by combining databases, serverless architecture and related AWS services.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Overview of fundamental database concepts before diving into specific AWS database services:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Basic concepts of databases<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Key components in relational databases: Primary Key, Foreign Key, normalization, indexing and partitioning<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Database operation and optimization mechanisms: Execution Plans, Database Logs and Buffers<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Database classifications: RDBMS, NoSQL, OLTP vs OLAP<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS services covered this week: Amazon RDS, Amazon Aurora, Amazon ElastiCache and Amazon Redshift<br>- Introduction to two major relational database services on AWS: Amazon RDS and Amazon Aurora<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon RDS: Concepts, benefits and key features<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon Aurora: Concepts and unique architecture<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Comparison and practical use cases<br>- Introduction to two important AWS data services: Amazon Redshift and Amazon ElastiCache<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon Redshift: MPP architecture, columnar storage and cost optimization<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon ElastiCache: Supported engines (Redis and Memcached) and benefits<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Hands-on practice roadmap and additional reference materials | 13/04/2026 | 13/04/2026 | [Database Concepts review](https://youtu.be/OOD2RwWuLRw?si=4sj2X9mO-rr-6Tog)<br>[Amazon RDS & Amazon Aurora](https://youtu.be/qbrobQZrokY?si=jrfgL-muOLWaAoDX)<br>[Redshift - Elasticache](https://youtu.be/UvdiRW34aNI?si=fsusJsJ6ziH5ufaS) |
| Tue | - Practice migrating databases from source database systems to AWS destinations:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Set up Source and Target endpoints for AWS DMS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Deploy serverless replication<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Monitor and troubleshoot issues during the migration process<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 14/04/2026 | 14/04/2026 | [Database Schema Conversion & Migration](https://000043.awsstudygroup.com/) |
| Wed | - Practice mastering the deployment of a complete database system ([Module 06 - Lab 05](https://drive.google.com/drive/folders/1-4v8yKN1CbKdEZ_K0_nngmkcv6G6i__G?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare the environment: Create a VPC, configure Subnets and Route Tables and set up Security Groups<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Launch an EC2 Instance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create an RDS Database Instance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Deploy the application: Connect the application running on EC2 to RDS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Manage backups and restore operations<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 15/04/2026 | 15/04/2026 | [Amazon Relational Database Service (Amazon RDS)](https://000005.awsstudygroup.com/) |
| Thu | - Develop Lambda Executor:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Implement core logic for provision-access function: Receive payload from API Gateway or Jira Webhook<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Validate request and retrieve Group Mapping configuration from AWS Secrets Manager | 16/04/2026 | 16/04/2026 | [AWS Secrets Manager](https://aws.amazon.com/vi/secrets-manager/) |
| Fri | - Develop Lambda Executor:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Use AWS SDK to add a User to an IAM Identity Center Group<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Handle errors (error handling) and send result notifications via Amazon SES to the requester | 17/04/2026 | 17/04/2026 | [AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)<br>[AWS SES](https://aws.amazon.com/vi/ses/) |
| Sat | - Rest and prepare for the upcoming week | 18/04/2026 | 18/04/2026 | |
| Sun | - Rest and prepare for the upcoming week | 19/04/2026 | 19/04/2026 | |

### Week 8 Achievements:

- Theoretical Knowledge:
  - Master fundamental database concepts, including relational models, NoSQL, Primary Keys, Foreign Keys, normalization, indexing and partitioning.
  - Understand the internal mechanisms of database management systems, such as Execution Plans, Database Logs and Buffers.
  - Clearly distinguish between OLTP and OLAP and understand how they are applied in different real-world scenarios.
  - Understand the architecture, characteristics and use cases of AWS database services such as Amazon RDS, Aurora, Redshift and ElastiCache.
  - Compare database services to select the most suitable solution based on performance, cost and business requirements.
- Hands-on Labs:
  - **Lab 43:** Practice database migration using AWS DMS, including configuring source/target databases, setting up replication and monitoring the migration process.
  - **Lab 05:** Deploy a complete database system on AWS, including configuring VPCs, Subnets, Security Groups, creating RDS instances and connecting applications running on EC2.
- System Development:
  - Build a Lambda Executor to process requests from API Gateway or Jira Webhooks.
  - Integrate AWS Secrets Manager to securely and flexibly manage configuration information such as group mappings.
  - Use the AWS SDK to add users to IAM Identity Center Groups.
  - Implement error-handling and response-processing mechanisms.
  - Integrate Amazon SES to send access provisioning result notifications to users.
- Skills Development:
  - Enhance the ability to design and deploy database systems on AWS.
  - Improve skills in working with various database services and selecting the appropriate solution for different use cases.
  - Strengthen the ability to analyze and handle real-world data migration challenges.
  - Develop serverless backend development skills by integrating multiple AWS services.
  - Improve the ability to read technical documentation and apply it effectively in practical implementations.
- Resource Management:
  - Perform resource cleanup after each lab to avoid unnecessary costs.
  - Maintain awareness of monitoring and optimizing resources when working with AWS database services.