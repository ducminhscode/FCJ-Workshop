---
title : "Worklog Week 7"
date :  "`r Sys.Date()`" 
weight : 7
pre: <b> 1.7 </b>
chapter : false
---

### Week 7 Objectives:

- Master the use of Permission Boundaries in AWS IAM to restrict user permissions and understand when they should be applied for more effective access control compared to standard IAM policies.
- Understand and practice protecting sensitive data through encryption at rest, using AWS KMS for key management and access control.
- Gain proficiency in implementing flexible IAM access control using Conditions (IP address, time, tags) instead of relying solely on static permissions.
- Learn how to securely grant applications access to AWS resources through IAM Roles, avoiding the direct use of Access Keys.
- Explore modern system design approaches by studying architectures that support agentic AI development on AWS, including code organization, testing strategies and development environments.
- Reinforce best practices in hands-on learning, validation and resource cleanup after each lab to optimize costs and maintain a clean AWS environment.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Practice restricting user permissions using IAM Permission Boundaries ([Module 05 - Lab 30](https://drive.google.com/drive/folders/1v_kjFM7quYAqDmXRfyDXxrWMCtlG03lh?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Introduction to Permission Boundaries<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare the AWS account environment<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create a Permission Boundary policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create an IAM Limited User, assign permissions and apply the Permission Boundary created above<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Verify permission restrictions<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 06/04/2026 | 06/04/2026 | [Limitation of user rights with IAM Permission Boundary](https://000030.awsstudygroup.com/) |
| Tue | - Practice protecting sensitive data by encrypting data at rest on AWS ([Module 05 - Lab 33](https://drive.google.com/drive/folders/1JpPL7tYB1Hw8QPkuWPGp0sPLZZ6BhAJC?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create a Customer Managed Key (CMK)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Encrypt storage resources: Amazon EBS and Amazon S3<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Verify encryption transparency and access permissions<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Monitor activities using AWS CloudTrail<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 07/04/2026 | 07/04/2026 | [Encrypt at rest with AWS KMS](https://000033.awsstudygroup.com/) |
| Wed | - Practice implementing more flexible and secure access control instead of relying solely on static permissions ([Module 05 - Lab 44](https://drive.google.com/drive/folders/1qQYDh2hPUwYENRNucp0RAx7_hyl3F0Rc?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Introduction to IAM<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create an IAM Group<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create an IAM User<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Use Conditions to restrict access based on IP address<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Apply restrictions based on time and tags<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Test and verify access control<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 08/04/2026 | 08/04/2026 | [IAM Role & Condition](https://000044.awsstudygroup.com/) |
| Thu | - Practice granting applications access to AWS services using IAM Roles ([Module 05 - Lab 48](https://drive.google.com/drive/folders/1YOydEc2uf3lXutnHmRAUXd7A_KoSkZcp?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Prepare the environment: Launch an EC2 Instance and create an S3 Bucket<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Use Access Keys: Create an IAM User and generate Access Keys<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Use IAM Roles on EC2: Create an IAM Role and attach it to the EC2 Instance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 09/04/2026 | 09/04/2026 | [Granting authorization for an application to access AWS services with an IAM role](https://000048.awsstudygroup.com/) |
| Fri | - Study and translate blog **Architecting for agentic AI development on AWS**: Focusing on key concepts such as system architecture for fast feedback loops, local emulation, hybrid testing, preview environments and code base design principles (domain-driven design, layered testing, monorepo). Gained a clearer understanding of how to design architectures that enable AI agents to autonomously develop, test and refine code efficiently. | 10/04/2026 | 10/04/2026 | [Blog 03](https://aws.amazon.com/vi/blogs/architecture/architecting-for-agentic-ai-development-on-aws/) |
| Sat | - Rest and prepare for the upcoming week | 11/04/2026 | 11/04/2026 | |
| Sun | - Rest and prepare for the upcoming week | 12/04/2026 | 12/04/2026 | |

### Week 7 Achievements:

- Theoretical Knowledge:
  - Gain a thorough understanding of IAM Permission Boundaries:
    - How they function as a maximum permission boundary for IAM Users/Roles.
    - The differences between Permission Boundaries and standard IAM Policies.
    - When to use them for access control in large-scale environments.
  - Master the principles of data encryption at rest on AWS:
    - The role of AWS KMS in key management.
    - Differences between Customer Managed Keys and AWS Managed Keys.
    - How services such as Amazon EBS and Amazon S3 integrate with KMS.
  - Understand advanced access control mechanisms using IAM Conditions:
    - Restricting access based on IP address.
    - Implementing time-based access control.
    - Managing access control using resource tags.
  - Understand how to grant permissions to applications using IAM Roles:
    - Comparison between IAM Roles and Access Keys.
    - The mechanism of temporary credentials.
    - Security benefits of using IAM Roles for EC2 instances.
  - Explore agentic AI development architectures on AWS:
    - Design principles that support rapid feedback and automation.
    - Concepts such as local emulation, hybrid testing and preview environments.
    - Organizing code using domain-driven design and monorepo approaches.
- Hands-on Labs:
  - **Lab 30:**
    - Create and apply Permission Boundaries for IAM Users.
    - Test and verify access restrictions.
  - **Lab 33:**
    - Create a Customer Managed Key using AWS KMS.
    - Encrypt data on Amazon EBS and Amazon S3.
    - Monitor activities using AWS CloudTrail.
  - **Lab 44:**
    - Create IAM Users and Groups.
    - Apply Conditions to restrict access based on IP address, time and tags.
    - Verify policy effectiveness.
  - **Lab 48:**
    - Create IAM Roles and assign them to EC2 Instances.
    - Test S3 access through IAM Roles.
    - Compare this approach with using Access Keys.
    - Perform validation and clean up resources after each lab.
- Skills Development:
  - Enhance advanced access control management skills in AWS IAM.
  - Become proficient in implementing data encryption and key management.
  - Develop the ability to design flexible permissions based on real-world conditions.
  - Gain practical understanding and application of IAM Roles in production environments.
  - Improve skills in reading architectural documentation and system analysis.
- System Thinking:
  - Develop a defense-in-depth mindset for multi-layered access control.
  - Understand how to combine IAM, KMS and conditional access mechanisms to strengthen security.
  - Adopt system design thinking that supports automation and AI-driven development.
  - Gain deeper awareness of balancing security, flexibility and developer experience.
- Resource Management:
  - Maintain the habit of cleaning up resources after hands-on practice.
  - Proactively monitor and control resource usage within reasonable limits.
  - Increase awareness of cost optimization when using AWS services.