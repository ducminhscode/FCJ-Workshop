---
title : "Worklog Week 4"
date :  "`r Sys.Date()`" 
weight : 4
pre: <b> 1.4 </b>
chapter : false
---

### Week 4 Objectives:

- Gain a solid understanding of Amazon S3, including object storage architecture, data access methods (REST API), S3 Access Points, and cost optimization using storage classes.
- Master advanced S3 features such as Static Website Hosting, CORS, Access Control (IAM, Bucket Policy, ACL), Versioning, and VPC Endpoints.
- Explore long-term, low-cost data storage solutions such as Amazon S3 Glacier and its data retrieval mechanisms.
- Learn and practice the VM Import/Export process between on-premises environments and AWS.
- Study and practice hybrid storage solutions on AWS, especially AWS Storage Gateway to connect on-premises systems with S3 via File Share.
- Understand Disaster Recovery (DR) strategies and centralized backup mechanisms using AWS Backup.
- Practice deploying AWS Backup to automate backup and restore operations across multiple services (EBS, RDS, DynamoDB, EFS), including Backup Plan configuration and notifications via Amazon SNS.
- Enhance knowledge of serverless and multi-tenant SaaS architectures, particularly tenant isolation in AWS Lambda through in-depth blog translation.
- Improve technical reading comprehension and translation skills for Cloud/AWS-related content.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Overview of AWS storage services:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Object Storage: Deep dive into Amazon S3 (Simple Storage Service), one of the most widely used AWS services<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Data transfer and hybrid solutions: Amazon Storage Gateway, AWS Snow Family<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cold Storage: Learn about low-cost storage solutions such as S3 Glacier<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Disaster Recovery: Designing resilient architectures on AWS to ensure business continuity<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Backup: Using AWS Backup to manage and automate backups across services<br>- Deep dive into S3 core concepts and architecture:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Object Storage concepts: storage units, WORM model<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Key features: scalability, durability, availability, replication, S3 event triggers<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Data access: HTTP/HTTPS (REST API), key-value structure<br>&nbsp;&nbsp;&nbsp;&nbsp;+ S3 Access Points for simplified access management<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Storage classes and cost optimization<br>- Advanced S3 and Glacier features:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Static Website Hosting and CORS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Access control: ACL, IAM, Bucket Policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ S3 VPC Endpoint<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Versioning and data protection<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Performance optimization and object key design<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon S3 Glacier: use cases, retrieval options, Glacier Vault Lock<br>- Approach data migration solutions, hybrid storage, and disaster recovery strategies on AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Snow Family: Snowball, Snowball Edge, Snowmobile<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Storage Gateway: A hybrid storage solution combining on-premises infrastructure and the cloud<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Disaster Recovery on AWS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Backup | 16/03/2026 | 16/03/2026 | [Storage Services on AWS](https://youtu.be/hsCfP0IxoaM?si=0HbBG1CLbHeK-08Y)<br>[Amazon Simple Storage Service ( S3 ) - Access Point - Storage Class](https://youtu.be/_yunukwcAwc?si=T-PUjhOY_lwk_WbL)<br>[S3 Static Website & CORS - Control Access - Object Key & Performance - Glacier](https://youtu.be/mPBjB6Ltl_Q?si=G1x-LC4YsbRRat8G)<br>[Snow Family - Storage Gateway - Backup](https://youtu.be/YXn8Q_Hpsu4?si=DIOoYiT883xx-3Ya) |
| Tue | - Hands-on practice: Using AWS Backup to centrally manage and automate data protection for services such as EBS, RDS, DynamoDB and EFS ([Module 04 - Lab 13](https://drive.google.com/drive/folders/1xGjj8feG0K4l9q4sOejxJuxYpPim-pLR?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare infrastructure: Create S3 Bucket and deploy infrastructure<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create a Backup Plan: Initialize a backup plan (frequency, retention period), assign resources<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure notifications using Amazon SNS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Test operations: Run manual backups, verify restoration, confirm notifications<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 17/03/2026 | 17/03/2026 | [Deploy AWS Backup to the System](https://000013.awsstudygroup.com/) |
| Wed | - Study and translate blog: **Building multi-tenant SaaS applications with AWS Lambda’s new tenant isolation mode**<br>- Analyze multi-tenant SaaS architectures and isolation strategies (shared function vs function-per-tenant)<br>- Understand **AWS Lambda tenant isolation mode**: tenant-id routing, execution environment isolation, per-tenant reuse<br>- Translate benefits (security, simplicity) and trade-offs (cold start, cost)<br>- Refine terminology and improve translation quality | 18/03/2026 | 18/03/2026 | [Blog 02](https://aws.amazon.com/vi/blogs/compute/building-multi-tenant-saas-applications-with-aws-lambdas-new-tenant-isolation-mode/) |
| Thu | - Hands-on practice: VM Import/Export to migrate virtual machines between on-premises environments and AWS Cloud ([Module 04 - Lab 14](https://drive.google.com/drive/folders/1lZFzO_z09Y4nyMCv14j3N0yw-LlWvfXi?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare the application server: Initialize a server or prepare a VM image file, configure basic settings<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare storage and permissions: Create an S3 Bucket, configure IAM Roles<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Upload VM image to AWS using AWS CLI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Perform the import process<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Launch an instance from the AMI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Export the virtual machine<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 19/03/2026 | 19/03/2026 | [VM Import/Export](https://000014.awsstudygroup.com/) |
| Fri | - Hands-on practice: Configuring AWS Storage Gateway to build a storage bridge between on-premises environments and AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare infrastructure: Create S3 Bucket and deploy Gateway server (EC2)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Initialize AWS Storage Gateway (not available in Free Tier accounts): Create a gateway, configure local cache storage<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create File Shares: Set up an S3 File Share connected directly to the created S3 bucket, configure access control to define who can read/write files<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Connect from on-premises machines: Mount the file share, verify data<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 20/03/2026 | 20/03/2026 | [Using File Storage Gateway](https://000024.awsstudygroup.com/) |
| Sat | - Participated in the **AWS First Cloud AI Journey Community Day 2026** (kick-off of FCAJ Bootcamp 2026) to connect with cloud experts, explore new Cloud and Generative AI applications, experience real-world demos, and expand networking<br>- Attended in-person meetings with team members to discuss and align project implementation approaches | 21/03/2026 | 21/03/2026 | ![team](/images/team.jpg) [AWS First Cloud AI Journey Community Day 2026](/4-events/event-2/) |
| Sun | - Rest and prepare for the upcoming week | 22/03/2026 | 22/03/2026 | |

### Week 4 Achievements:

- Theoretical Knowledge:
  - Gained a solid understanding of Amazon S3 object storage architecture, including key-value model, REST API, S3 Access Points, and storage classes for cost optimization.
  - Understood access control mechanisms in S3, including IAM Policies, Bucket Policies, ACLs, and Access Points.
  - Differentiated long-term storage solutions such as Amazon S3 Glacier and their retrieval tiers.
  - Explored hybrid storage solutions using AWS Storage Gateway and large-scale data transfer using AWS Snow Family.
  - Understood Disaster Recovery (DR) strategies and centralized backup management using AWS Backup.
  - Learned the VM migration process from on-premises to AWS using VM Import/Export, including VM image conversion, S3 storage, and AMI deployment.
  - Gained insights into multi-tenant serverless architectures, especially AWS Lambda tenant isolation mode.
  - Analyzed trade-offs between security, cost, and performance (e.g., cold starts, resource isolation).
- Hands-on Labs:
  - **Lab 13:** Configured centralized backup using AWS Backup with Backup Plans and resource assignments.
  - **Lab 14:** Practiced VM Import/Export, including uploading VM images to S3, importing as AMI, launching EC2 instances, and exporting VMs back to external environments.
  - **Lab 24:** Explored hybrid storage architecture using AWS Storage Gateway connecting simulated on-premises environments with S3.
  - Verified data and cleaned up resources after each lab to optimize costs.
- Skills Development:
  - Improved ability to deploy and manage AWS storage systems, from object storage to hybrid storage.
  - Developed practical skills in backup, hybrid storage, and VM migration in AWS environments.
  - Strengthened system architecture analysis skills, especially in serverless and multi-tenant systems.
  - Enhanced technical English reading and AWS-related translation skills.
  - Developed cost optimization and performance-oriented thinking in cloud architecture.
- Community & Teamwork:
  - Participated in AWS First Cloud AI Journey Community Day 2026, expanding professional network and learning about trends in Generative AI and Cloud Computing.
  - Collaborated with team members to align project implementation strategies and improve teamwork skills.
- Resource Management:
  - Effectively cleaned up AWS resources after each lab to avoid unnecessary costs.
  - Maintained awareness of resource optimization within Free Tier limits.