---
title : "Worklog Week 4"
date :  "`r Sys.Date()`" 
weight : 4
pre: <b> 1.4 </b>
chapter : false
---

### Week 4 Objectives:

- Gain a solid understanding of AWS storage services, especially Amazon S3, including object storage architecture, storage classes, and cost optimization strategies.
- Master advanced S3 features such as Static Website Hosting, CORS, Access Control (IAM, Bucket Policy, ACL), Versioning, and VPC Endpoints.
- Explore long-term, low-cost data storage solutions such as Amazon S3 Glacier and its data retrieval mechanisms.
- Study data migration and hybrid storage solutions on AWS, including AWS Snow Family and AWS Storage Gateway.
- Understand Disaster Recovery (DR) strategies and centralized backup mechanisms using AWS Backup.
- Practice deploying and managing backup systems, including backup and restore operations across multiple AWS services (EBS, RDS, DynamoDB).
- Enhance knowledge of serverless and multi-tenant SaaS architectures, particularly tenant isolation in AWS Lambda through in-depth blog translation.
- Improve technical reading comprehension and translation skills for Cloud/AWS-related content.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Overview of AWS storage services:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Object Storage: Deep dive into Amazon S3 (Simple Storage Service), one of the most widely used AWS services<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Data transfer and hybrid solutions: Amazon Storage Gateway, AWS Snow Family<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cold Storage: Learn about low-cost storage solutions such as S3 Glacier<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Disaster Recovery: Designing resilient architectures on AWS to ensure business continuity<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Backup: Using AWS Backup to manage and automate backups across services<br>- Deep dive into S3 core concepts and architecture:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Object Storage concepts: storage units, WORM model<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Key features: scalability, durability, availability, replication, S3 event triggers<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Data access: HTTP/HTTPS (REST API), key-value structure<br>&nbsp;&nbsp;&nbsp;&nbsp;+ S3 Access Points for simplified access management<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Storage classes and cost optimization<br>- Advanced S3 and Glacier features:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Static Website Hosting and CORS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Access control: ACL, IAM, Bucket Policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ S3 VPC Endpoint<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Versioning and data protection<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Performance optimization and object key design<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon S3 Glacier: use cases, retrieval options, Glacier Vault Lock<br>- Data migration, hybrid storage, and DR strategies:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Snow Family: Snowball, Snowball Edge, Snowmobile<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Storage Gateway: Hybrid storage solution integrating on-premises and cloud<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Disaster Recovery on AWS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Backup | 16/03/2026 | 16/03/2026 | [Storage Services on AWS](https://youtu.be/hsCfP0IxoaM?si=0HbBG1CLbHeK-08Y)<br>[Amazon Simple Storage Service ( S3 ) - Access Point - Storage Class](https://youtu.be/_yunukwcAwc?si=T-PUjhOY_lwk_WbL)<br>[S3 Static Website & CORS - Control Access - Object Key & Performance - Glacier](https://youtu.be/mPBjB6Ltl_Q?si=G1x-LC4YsbRRat8G)<br>[Snow Family - Storage Gateway - Backup](https://youtu.be/YXn8Q_Hpsu4?si=DIOoYiT883xx-3Ya) |
| Tue | - Practice deploy AWS Backup ([Module 04 - Lab 13](https://drive.google.com/drive/folders/1xGjj8feG0K4l9q4sOejxJuxYpPim-pLR?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Setup infrastructure: S3, EC2, RDS, DynamoDB<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create Backup Plan and assign resources<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure notifications using Amazon SNS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Perform backup and restore testing<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 17/03/2026 | 17/03/2026 | [Deploy AWS Backup to the System](https://000013.awsstudygroup.com/) |
| Wed | - Study and translate blog: **Building multi-tenant SaaS applications with AWS Lambda’s new tenant isolation mode**<br>- Analyze multi-tenant SaaS architectures and isolation strategies (shared function vs function-per-tenant)<br>- Understand **AWS Lambda tenant isolation mode**: tenant-id routing, execution environment isolation, per-tenant reuse<br>- Translate benefits (security, simplicity) and trade-offs (cold start, cost)<br>- Refine terminology and improve translation quality | 18/03/2026 | 18/03/2026 | [Blog 02](https://aws.amazon.com/vi/blogs/compute/building-multi-tenant-saas-applications-with-aws-lambdas-new-tenant-isolation-mode/) |
| Thu | - Practice Backup & Restore with Amazon EBS ([Module 04 - Lab 14](https://drive.google.com/drive/folders/1lZFzO_z09Y4nyMCv14j3N0yw-LlWvfXi?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Setup EC2 and attach EBS volumes<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create EBS snapshots<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Automate backups using Data Lifecycle Manager (DLM)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Restore volumes and validate data<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 19/03/2026 | 19/03/2026 | [VM Import/Export](https://000014.awsstudygroup.com/) |
| Fri | - Practice AWS Storage Gateway (Free Tier limitation applies) ([Module 04 - Lab 24](https://drive.google.com/drive/folders/1XCUuNDfmr1AnilJTwEEONTN-XD7nTZhx?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Setup infrastructure (S3 + EC2 Gateway)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Activate and configure Gateway<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create File Share<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Mount and test from client<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 20/03/2026 | 20/03/2026 | [Using File Storage Gateway](https://000024.awsstudygroup.com/) |
| Sat | - Participated in the **AWS First Cloud AI Journey Community Day 2026** (kick-off of FCAJ Bootcamp 2026) to connect with cloud experts, explore new applications of Cloud and Generative AI, experience real-world demos, and expand networking within the technology ecosystem<br>- Attended in-person meetings with team members to discuss and align on project implementation approaches | 21/03/2026 | 21/03/2026 | ![team](/images/team.jpg) [AWS First Cloud AI Journey Community Day 2026](/4-events/event-2/) |
| Sun | - Rest and prepare for the upcoming week | 22/03/2026 | 22/03/2026 | |

### Week 4 Achievements:

- Theoretical Knowledge:
  - Gained a solid understanding of Amazon S3 object storage architecture, including key-value data organization, durability, availability, and scalability.
  - Learned how to select appropriate S3 storage classes for cost optimization.
  - Understood access control mechanisms in S3, including IAM Policies, Bucket Policies, ACLs, and Access Points.
  - Differentiated long-term storage options such as Amazon S3 Glacier and their retrieval tiers.
  - Explored hybrid storage solutions with AWS Storage Gateway and large-scale data transfer using AWS Snow Family.
  - Understood Disaster Recovery (DR) strategies and centralized backup management using AWS Backup.
  - Gained insights into multi-tenant serverless architectures, especially AWS Lambda tenant isolation mode.
  - Analyzed trade-offs between security, cost, and performance (e.g., cold starts, resource isolation).
- Hands-on Labs:
  - **Lab 13:** Successfully deployed AWS Backup, created backup plans, configured notifications, and tested restore processes.
  - **Lab 14:** Practiced EBS snapshot backup and restore, automated lifecycle management using DLM.
  - **Lab 24:** Explored hybrid storage with AWS Storage Gateway and connected on-premises simulation with S3.
  - Validated data and cleaned up resources after each lab to optimize costs.
- Skills Development:
  - Improved ability to deploy and manage AWS storage systems (object and hybrid storage).
  - Developed skills in designing and operating backup & recovery systems.
  - Strengthened system architecture analysis skills, especially in serverless and multi-tenant systems.
  - Enhanced technical English reading and AWS-related translation skills.
  - Built cost optimization and performance-oriented thinking in cloud architecture.
- Community & Teamwork:
  - Participated in AWS First Cloud AI Journey Community Day 2026, expanding professional network and learning about trends in Generative AI and Cloud Computing.
  - Collaborated with team members to align project implementation strategies and improve teamwork skills.
- Resource Management:
  - Effectively cleaned up AWS resources after each lab to avoid unnecessary costs.
  - Maintained awareness of resource optimization within Free Tier limits.