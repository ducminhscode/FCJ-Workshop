---
title : "Worklog Week 5"
date :  "`r Sys.Date()`" 
weight : 5
pre: <b> 1.5 </b>
chapter : false
---

### Week 5 Objectives:

- Master the deployment and administration of storage services on AWS, including Amazon FSx and Amazon S3, through hands-on lab exercises.
- Understand and apply the architecture of high-performance shared file systems on Windows Server in a cloud environment.
- Gain experience in deploying static websites using Amazon S3 integrated with CloudFront, ensuring security and optimized performance.
- Build a foundation for serverless backend systems with AWS Lambda, including project organization, code reuse (shared logic), and dependency management using Lambda Layers.
- Develop integration modules with external services (Jira API) and AWS services (DynamoDB) to support workflow management solutions.
- Design and implement approval flow logic with HMAC-SHA256 token security mechanisms, ensuring data integrity and security.
- Gradually strengthen the skills required to design, develop, and operate a real-world serverless system on AWS.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Hands-on practice: Setting up a high-performance shared file system based on Windows Server:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Infrastructure preparation: Create VPC, Active Directory (AWS Managed Microsoft AD), launch Windows Server (EC2)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ FSx setup: SSD Multi-AZ file system, HDD Multi-AZ file system<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Connect and create File Share<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Storage optimization: Data Deduplication, Shadow Copies<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Administration & monitoring: Session management, User Quotas, Continuously Available Shares<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Scaling: Increase throughput and storage capacity<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Performance monitoring and evaluation<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 23/03/2026 | 23/03/2026 | [Amazon FSx for Windows File Server](https://000025.awsstudygroup.com/) |
| Tue | - Hands-on practice: Amazon S3 Static Website Hosting ([Module 04 - Lab 57](https://drive.google.com/drive/folders/1wwixlCyGcefwCB5F8Gq094LTd1t_eScY?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Data preparation: Create an S3 bucket, upload data<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure Static Website Hosting: Enable static website hosting on S3, specify the index document and error document<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Set up access control (Security and Permissions): Configure Block Public Access, set up Bucket Policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Enhance performance and security with CloudFront: Use Amazon CloudFront, configure OAC/OAI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Manage lifecycle and data: Enable bucket versioning, perform data migration and copying<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 24/03/2026 | 24/03/2026 | [Starting with Amazon S3](https://000057.awsstudygroup.com/) |
| Wed | - Environment Setup and Shared Logic Initialization:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Set up the project directory structure for AWS Lambda<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Build shared libraries for common functionalities (logging, exception handling)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Develop the `dynamodb_session.py` module to interact with session and token tables | 25/03/2026 | 25/03/2026 | [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)<br>[Python Virtualenv](https://docs.python.org/3/library/venv.html) |
| Thu | - Build Lambda Layer and Jira Client:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Package external libraries (requests, boto3) into a Lambda Layer<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Develop `jira_client.py` to integrate Jira APIs for updating ticket statuses and retrieving User/Group information from Jira Service Management | 26/03/2026 | 26/03/2026 | [Boto3](https://docs.aws.amazon.com/boto3/latest/)<br>[AWS SSO Admin API](https://docs.aws.amazon.com/boto3/latest/reference/services/sso-admin.html)<br>[Jira Service Management](https://developer.atlassian.com/cloud/jira/service-desk/webhooks/)<br>[Atlassian](https://developer.atlassian.com/cloud/jira/platform/webhooks/) |
| Fri | - Develop Approval Logic:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Implement the generate_token function for approvals: Generate secure tokens based on the HMAC-SHA256 algorithm<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Develop a Lambda function to process email responses: Validate tokens and update approval statuses in DynamoDB | 27/03/2026 | 27/03/2026 | [AWS DynamoDB](https://aws.amazon.com/vi/dynamodb/) |
| Sat | - Rest and prepare for the upcoming week | 28/03/2026 | 28/03/2026 | |
| Sun | - Rest and prepare for the upcoming week | 29/03/2026 | 29/03/2026 | |

### Week 5 Achievements:

- Theoretical Knowledge:
  - Gain a solid understanding of the architecture and deployment of high-performance shared file systems on Windows Server within the AWS environment.
  - Understand how Amazon FSx for Windows File Server works, including Multi-AZ deployment, SSD/HDD storage mechanisms, and optimization features such as deduplication and Shadow Copies.
  - Learn how to deploy static websites on Amazon S3 and understand the role of CloudFront in content acceleration and security.
  - Understand S3 access control mechanisms such as Bucket Policies, Block Public Access, and Versioning.
  - Strengthen knowledge of serverless architecture using AWS Lambda and reusable code organization practices.
- Hands-on Labs:
  - **Lab 25:** Deploy a file-sharing system with Amazon FSx, including file system creation, connection setup, access permission configuration, and performance monitoring.
  - **Lab 57:** Build a static website on Amazon S3, configure hosting, manage access permissions, and integrate with CloudFront.
- System Development:
  - Set up a modular and scalable project structure for AWS Lambda.
  - Build shared libraries for common functionalities such as logging and exception handling.
  - Develop the `dynamodb_session.py` module to manage sessions and tokens using DynamoDB.
  - Create Lambda Layers to package and reuse external libraries such as requests and boto3.
  - Develop `jira_client.py` to integrate with Jira APIs for ticket management.
  - Implement approval flow logic, including secure token generation using HMAC-SHA256 and email response handling.
  - Integrate with DynamoDB to store and update approval statuses.
- Skills Development:
  - Enhance the ability to deploy and manage storage systems on AWS.
  - Improve serverless application development skills using AWS Lambda and related services.
  - Strengthen system design thinking with a focus on modularization and code reusability.
  - Develop skills in integrating external APIs (Jira) into backend systems.
  - Improve security handling in business workflows, especially token-based approval mechanisms.
- Resource Management:
  - Perform resource cleanup after each lab to avoid unnecessary costs.
  - Build awareness of monitoring and optimizing AWS storage and serverless resources during deployment.
