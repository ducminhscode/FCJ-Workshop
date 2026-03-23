---
title : "Worklog Week 3"
date :  "`r Sys.Date()`" 
weight : 3 
pre: <b> 1.3 </b>
chapter : false
---

### Week 3 Objectives:

- Clearly understand how to connect multiple VPCs using AWS Transit Gateway, thereby gaining knowledge of a centralized network architecture that helps manage connections between multiple systems on AWS efficiently and in a scalable manner.
- Gain solid knowledge about Compute services on AWS, especially Amazon EC2, including its architecture, instance types, instance initialization mechanisms, and ways to optimize performance and cost.
- Understand AWS data storage solutions, including block storage, file storage, and object storage through services such as Amazon EBS, Amazon EFS, and Amazon S3.
- Study and practice automation and scalability mechanisms, especially Amazon EC2 Auto Scaling, to ensure systems can automatically adjust resources based on real workload demand.
- Explore and implement data protection and backup solutions using AWS Backup to centrally manage backups for multiple AWS services.
- Learn about hybrid storage integration solutions between on-premises environments and AWS through AWS Storage Gateway.
- Practice deploying a static website on the cloud, using Amazon S3 combined with Amazon CloudFront to accelerate content delivery and enhance security.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Practice using AWS Transit Gateway to connect multiple VPCs ([Module 02 - Lab 20](https://drive.google.com/drive/folders/1R942J-0GyxGhZm2NpvKkhhXmjS6LDLpA?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare environment: Create Keypair, initialize infrastructure with CloudFormation<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create Transit Gateway<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create Transit Gateway Attachments<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure Transit Gateway Route Tables<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Update VPC Route Tables<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Verify results: Ping Private IP between EC2 instances in different VPCs<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 09/03/2026 | 09/03/2026 | [Set up AWS Transit Gateway](https://000020.awsstudygroup.com/) |
| Tue | - Study Compute Virtual Machine services and related storage services on AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon EC2 - the primary virtual server service of AWS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon EBS - block storage service for EC2 instances<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon Lightsail - simplified virtual server service suitable for lightweight applications<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Network File Storage services: Amazon EFS, Amazon FSx<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Application Migration Service (MGN): Migration and disaster recovery<br>- Study EC2 in detail, focusing on Instance Types and EC2 architecture:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Overview of Amazon EC2: Definition and advantages compared to traditional servers<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Instance Types concept<br>&nbsp;&nbsp;&nbsp;&nbsp;+ EC2 instance architecture: Hardware Node, Placement Options, Hypervisor, AMI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Important considerations<br>- Understand three key concepts:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon Machine Image (AMI): Definition, types, components<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Backup and Snapshot: Definition and mechanisms<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Key Pair: Definition and usage differences depending on operating systems<br>- Amazon Elastic Block Store (EBS):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Overview of Amazon EBS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Main disk types: HDD and SSD<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Architecture and key features: Independence, EBS optimized instances, Multi-Attach<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Backup and Snapshot<br>- EC2 Instance Store:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Overview and performance characteristics<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Ephemeral storage properties and data persistence behavior<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Use cases: Caching, buffering, swap space, logs, reproducible data<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Comparison with EBS<br>- EC2 User Data:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Definition and purpose<br>&nbsp;&nbsp;&nbsp;&nbsp;+ How it works on Linux and Windows<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Benefits of using User Data<br>- EC2 Metadata:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Definition and types of information provided<br>&nbsp;&nbsp;&nbsp;&nbsp;+ How to access metadata<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Key differences and use cases<br>- EC2 Auto Scaling:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Overview and purpose<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Components and operation mechanisms<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Integration with Elastic Load Balancing<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cost optimization<br>- Study advanced compute and storage services:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ EC2 Auto Scaling architecture and workflow<br>&nbsp;&nbsp;&nbsp;&nbsp;+ EC2 pricing models: On-Demand, Reserved Instances, Savings Plans, Spot Instances<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon Lightsail<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Network storage: Amazon EFS and Amazon FSx<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Application Migration Service (MGN) | 10/03/2026 | 10/03/2026 | [Compute VM on AWS](https://youtu.be/-t5h4N6vfBs?si=_uHrDvmYkZWo5Ee6)<br>[EC2 - Instance type](https://youtu.be/e7XeKdOVq40?si=mp8iLSeckT-TIt-v)<br>[EC2 - AMI / Backup / Key Pair](https://youtu.be/yAR6QRT3N1k?si=5NxD2rNDyl8xMKMk)<br>[EC2 - Elastic block store](https://youtu.be/hKr_TfGP7NY?si=kNm3qW_HVNoJmpCp)<br>[EC2 - Instance store](https://youtu.be/6IHNDJ85aoQ?si=LcEs0U5Rx4QHo4HW)<br>[EC2 - User Data](https://youtu.be/_v_43Wi7zjo?si=sdqegshjBNsSSuBp)<br>[EC2 - Metadata](https://youtu.be/Ew3QRaKJQSA?si=yM01QVi1IuojA0gC)<br>[EC2 - EC2 Auto Scaling](https://youtu.be/bbLcPitXJSY?si=DsU5EQwRYKAP8-bO)<br>[EC2 Autoscaling - EFS/FSx - Lightsail - MGN](https://youtu.be/hFVYG8WqfU0?si=9fXq-GMnIMQJE4Jx) |
| Wed | - Practice using AWS Backup to centrally manage and automate data protection for services such as EBS, RDS, DynamoDB and EFS ([Module 03 - Lab 13](https://drive.google.com/drive/folders/1_NbVp3i8nkD5_d_7Pn1kcKNVzwn-k0wW?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare infrastructure: Create S3 Bucket and deploy infrastructure<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create Backup Plan (frequency and retention settings)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure notifications using Amazon SNS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Test operations: Manual backup and restore verification<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 11/03/2026 | 11/03/2026 | [Deploy AWS Backup to the System](https://000013.awsstudygroup.com/) |
| Thu | - Practice configuring AWS Storage Gateway to build a storage bridge between on-premises environments and AWS ([Module 03 - Lab 24](https://drive.google.com/drive/folders/1TyWhEC1vw_FAnbV3gADB7i12fE-FeBqa?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare infrastructure: Create S3 Bucket and deploy Gateway server (EC2)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Initialize Storage Gateway (Free Tier limitation noted)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create File Shares connected to S3 Bucket<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure access control permissions<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Mount file share from on-premises environment<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 12/03/2026 | 12/03/2026 | [Using File Storage Gateway](https://000024.awsstudygroup.com/) |
| Fri | - Practice Amazon S3 Static Website Hosting ([Module 03 - Lab 57](https://drive.google.com/drive/folders/1GSPhu8eI7ZMk5CDyN9GyKfHlhatrHdwK?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare data: Create S3 bucket and upload website files<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure Static Website Hosting<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure security and permissions<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Integrate Amazon CloudFront for performance and security<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Manage bucket lifecycle and versioning<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 13/03/2026 | 13/03/2026 | [Starting with Amazon S3](https://000057.awsstudygroup.com/) |
| Sat | - Rest and prepare for the upcoming week | 14/03/2026 | 14/03/2026 | |
| Sun | - Rest and prepare for the upcoming week | 15/03/2026 | 15/03/2026 | |

### Week 3 Achievements:

- Theoretical Knowledge:
  - Gained understanding of Hub-and-Spoke networking architecture using AWS Transit Gateway to connect multiple VPCs.
  - Understood the working mechanism of Amazon EC2, including instance types, AMI, hypervisor, placement options, and how to choose configurations for different workloads.
  - Distinguished between AWS storage types:
    - Block storage: Amazon EBS
    - File storage: Amazon EFS and Amazon FSx
    - Object storage: Amazon S3
  - Understood differences between EBS and Instance Store in terms of performance, durability, and use cases.
  - Learned how EC2 Auto Scaling enables automatic resource scaling and integrates with load balancing systems.
  - Understood centralized backup and restore processes with AWS Backup.
  - Learned how to deploy and optimize static website hosting with Amazon S3 and CloudFront CDN.
- Hands-on Labs:
  - **Lab 20:** Successfully deployed a multi-VPC architecture using AWS Transit Gateway and verified connectivity between EC2 instances via private IP.
  - **Lab 13:** Configured centralized backup using AWS Backup with Backup Plans and resource assignments.
  - **Lab 24:** Explored hybrid storage architecture using AWS Storage Gateway connecting simulated on-premises environments with S3.
  - **Lab 57:** Successfully deployed static website hosting on Amazon S3 and integrated it with CloudFront CDN.
- System Deployment Skills:
  - Able to deploy and manage EC2 instances, including instance creation, storage configuration, and access management using Key Pairs.
  - Learned how to automate server configuration using EC2 User Data and retrieve system information using EC2 Metadata.
  - Practiced setting up Hybrid Storage and Cloud Storage architectures.
  - Learned the process of deploying and operating a static website on the cloud.
- Resource Management:
  - Properly cleaned up resources after each lab to ensure no unnecessary AWS costs during the learning and experimentation process.