---
title : "Worklog Week 11"
date :  "`r Sys.Date()`" 
weight : 11
pre: <b> 1.11 </b>
chapter : false
---

### Week 11 Objectives:

- Complete and test the entire integration flow of the access management system from Jira to AWS Identity Center.
- Ensure that the access provisioning and revocation mechanisms operate stably, automatically and follow the designed lifecycle.
- Enhance system observability by implementing advanced logging and monitoring using Amazon CloudWatch.
- Optimize AWS Lambda performance and cost by tuning runtime configuration and memory allocation appropriately.
- Finalize technical documentation and architecture diagrams to support system handover and maintenance.
- Develop presentation and system demo skills through final operation and review sessions with the team.
- Continue expanding knowledge of the AWS data analytics ecosystem, especially the Serverless Data Lake model.
- Understand the end-to-end data processing flow from ingestion, cataloging, transformation, to analysis and visualization on AWS.
- Get familiar with building interactive and visual dashboards using Amazon QuickSight.
- Improve system design skills for serverless architectures combining data analytics, event-driven processing and visualization.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Integration Testing:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Collaborate with team members to test the workflow: Jira Webhook → API Gateway → Lambda Executor → Identity Center<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Verify the automated access revocation flow based on configured time intervals | 04/05/2026 | 04/05/2026 |  |
| Tue | - Optimization & Logging:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure advanced CloudWatch Logs to monitor the number of successful and failed requests<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Optimize Lambda runtime execution and memory allocation to reduce operational costs | 05/05/2026 | 05/05/2026 | [Amazon CloudWatch](https://aws.amazon.com/vi/cloudwatch/) |
| Wed | - Packaging and Handover:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Complete the documentation for the codebase and logic diagrams of Lambda functions<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Conduct the final demo session and operate the project together with team members | 06/05/2026 | 06/05/2026 |  |
| Thu | - Practice getting familiar with data analytics services in the AWS ecosystem:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Data Pipeline: Build a complete Serverless Data Lake architecture, including stages such as **Ingest & Store** (using Amazon S3 as the primary storage foundation for the Data Lake and Amazon Kinesis for real-time streaming data processing) and **Catalog & Transform** (using AWS Glue Crawlers to automatically scan and generate data schemas (Catalog))<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Analysis & Visualization: After the data has been processed and stored, learn how to extract value from it through **Querying** (Amazon Athena), building a **Data Warehouse** (Amazon Redshift), **Visualization** (Amazon QuickSight) and **Event Processing** (AWS Lambda) | 07/05/2026 | 07/05/2026 | [Analytics on AWS workshop](https://000072.awsstudygroup.com/) |
| Fri | - Practice building a dashboard to visualize data:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare resources: Sign in to Amazon QuickSight, select the working Region, prepare and connect data sources (such as Amazon S3 and Athena) and create datasets<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Build the Dashboard: Create an Analysis and add Visuals to represent the data<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Improve the Dashboard: Customize colors, number formats and labels; use advanced features to highlight important metrics<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create an interactive Dashboard: Configure filters and controls, set parameters for flexible view changes and create actions so that clicking one chart automatically updates related charts<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Publish and share<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 08/05/2026 | 08/05/2026 | [Get started with Quick Sight](https://000073.awsstudygroup.com/) |
| Sat | - Participated in the **FCAJ HCM Knowledge Sharing** workshop, where team speakers shared learning methods, thinking frameworks and personal development experiences. | 09/05/2026 | 09/05/2026 | |
| Sun | - Rest and prepare for the upcoming week | 10/05/2026 | 10/05/2026 | |

### Week 11 Achievements:

- Theoretical Knowledge:
  - Understand the integration testing process in a multi-service serverless system on AWS.
  - Learn how to use Amazon CloudWatch for logging, metrics and monitoring system health and performance.
  - Understand how to optimize AWS Lambda performance by tuning memory allocation and runtime execution.
  - Strengthen knowledge of Serverless Data Lake architecture and data processing flows in AWS Analytics ecosystems.
  - Understand the roles of services such as Amazon Kinesis, AWS Glue, Amazon Athena, Amazon Redshift, Amazon QuickSight and AWS Lambda in a data analytics pipeline.
  - Learn how to design interactive and effective data visualization dashboards using Amazon QuickSight.
- Hands-on Labs:
  - **Lab 72:** Build a complete Serverless Data Lake architecture using S3, Kinesis, Glue, Athena, Redshift, QuickSight and Lambda.
  - **Lab 73:** Build interactive dashboards with Amazon QuickSight, including dataset creation, visualization, dashboard interactivity and report sharing.
- System Development:
  - Perform end-to-end testing of the Jira Webhook → API Gateway → Lambda Executor → AWS Identity Center flow to ensure system stability.
  - Verify that the automated revoke mechanism works correctly according to the designed TTL lifecycle.
  - Configure CloudWatch Logs and monitor success/failure request metrics for better observability and troubleshooting.
  - Optimize Lambda functions to improve system stability and reduce operational costs.
  - Complete technical documentation, architecture diagrams and deployment guides for project handover.
  - Conduct system demo and operational testing with team members.
- Skills Development:
  - Improve integration testing skills for complex AWS multi-service systems.
  - Enhance monitoring, logging and troubleshooting capabilities in serverless environments.
  - Develop cost and performance optimization skills in cloud systems.
  - Strengthen data visualization and dashboard design skills for analytics use cases.
  - Improve teamwork, system presentation and real-world project delivery skills.
  - Reinforce mindset for building automated and event-driven systems on AWS.
- Resource Management:
  - Perform resource cleanup after each lab to avoid unnecessary costs.
  - Monitor resource usage across Lambda, Athena, Kinesis and QuickSight for cost optimization.
  - Maintain strong awareness of resource governance and performance monitoring in AWS analytics systems.