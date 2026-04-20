---
title : "Worklog Week 6"
date :  "`r Sys.Date()`" 
weight : 6
pre: <b> 1.6 </b>
chapter : false
---

### Objectives of Week 6:

- Gain a clear understanding of AWS security, especially the Shared Responsibility Model, to distinguish responsibilities between the service provider and the user in securing the system.
- Master the concepts and mechanisms of AWS Identity and Access Management (IAM), including IAM Users, Groups, Roles, and Policies (JSON), along with the principle of least privilege.
- Understand the role and benefits of IAM Roles, particularly in providing temporary permissions and enhancing security for AWS services.
- Learn about user authentication and authorization using Amazon Cognito, including User Pools, Identity Pools, and integration with web/mobile applications.
- Understand how to manage multi-account environments using AWS Organizations, including organizational structure, Organizational Units (OU), and permission control using Service Control Policies (SCP).
- Learn AWS Identity Center (SSO) for centralized access management and implementing single sign-on across multiple accounts and applications.
- Understand encryption mechanisms and key management in AWS Key Management Service (KMS), including key types (CMK, Data Key) and their use in protecting data.
- Explore AWS Security Hub, including aggregation, analysis, and evaluation of security findings based on standards such as CIS Benchmark and AWS Foundational Security Best Practices.
- Practice hands-on labs related to:
  - Security analysis and evaluation using AWS Security Hub
  - Optimizing EC2 costs using AWS Lambda
  - Managing resources using Tags and Resource Groups
  - Controlling EC2 access using IAM and Resource Tags
- Develop a security-first mindset in cloud system design by integrating multiple AWS services to build secure and efficient systems.
- Improve technical reading skills, hands-on implementation using AWS Console and CLI, and develop a mindset for cost optimization and resource management.

### Tasks to be completed during this week:

| Day | Tasks | Start Date | End Date | Resources |
|:---:|------|:----------:|:--------:|-----------|
| Mon | - Learn about AWS security and the Shared Responsibility Model:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Shared Responsibility Model<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Key security services: IAM, Amazon Cognito, AWS Organizations & Identity Center, AWS KMS<br>- Manage identities and access in AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Root account concept<br>&nbsp;&nbsp;&nbsp;&nbsp;+ IAM Users and Groups<br>&nbsp;&nbsp;&nbsp;&nbsp;+ IAM Policies (JSON): evaluation logic and types<br>&nbsp;&nbsp;&nbsp;&nbsp;+ IAM Roles and their importance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Why use IAM Roles?<br>- Manage authentication and authorization with Amazon Cognito:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Concepts, features, and benefits<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Core components: User Pool and Identity Pool<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Architecture and authentication flow<br>- AWS Organizations:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Role and benefits (centralized management, risk reduction, automation)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Structure: Management Account, Organizational Units, Member Accounts<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Permission control using SCP<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Consolidated Billing<br>- AWS Identity Center:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Concept and architecture<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Multi-account access model<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Benefits: SSO, centralized control, enhanced security<br>- AWS Key Management Service:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Concept, features, and standards<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Key types: CMK, Data Key<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Working mechanism<br>- AWS Security Hub:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Concept and operation<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Scope of checks<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Security standards: PCI DSS, AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Reading results and examples<br>- Additional labs and research | 30/03/2026 | 30/03/2026 | (links) |
| Tue | - Practice AWS Security Hub (Free Tier limitation):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Learn security standards<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Enable Security Hub<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Analyze findings<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Evaluate compliance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 31/03/2026 | 31/03/2026 | (link) |
| Wed | - Optimize EC2 costs using AWS Lambda:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Infrastructure setup (VPC, EC2, Security Group, Slack webhook)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tagging strategy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ IAM Role setup<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Lambda functions (start/stop instances)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Testing and cleanup | 01/04/2026 | 01/04/2026 | (link) |
| Thu | - Resource management using Tags and Resource Groups:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tagging via Console and CLI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Creating Resource Groups<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cleanup | 02/04/2026 | 02/04/2026 | (link) |
| Fri | - Manage EC2 access using IAM and Resource Tags:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ IAM user setup<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Policy creation<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Role creation<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Policy testing<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cleanup | 03/04/2026 | 03/04/2026 | (link) |
| Sat | - Rest and preparation for next week | 04/04/2026 | 04/04/2026 | |
| Sun | - Rest and preparation for next week | 05/04/2026 | 05/04/2026 | |

### Achievements of Week 6:

- **Theoretical Knowledge:**
  - Clearly understood the Shared Responsibility Model and the division of security responsibilities between AWS and users.
  - Mastered IAM components:
    - Users, Groups, Roles, and Policies
    - Least privilege principle and policy evaluation logic
  - Understood IAM Roles and their use in temporary access delegation.
  - Learned authentication and authorization with Amazon Cognito:
    - User Pools vs Identity Pools
    - Authentication workflows
  - Understood AWS Organizations:
    - Structure and governance using SCP
    - Billing management
  - Learned AWS Identity Center (SSO) for centralized access control.
  - Understood encryption and key management using AWS KMS:
    - CMK vs Data Key
    - Envelope encryption
  - Learned AWS Security Hub:
    - Security findings aggregation and analysis
    - Compliance standards and evaluation

- **Hands-on Labs:**
  - **Lab 18:** AWS Security Hub setup and analysis
  - **Lab 22:** EC2 cost optimization using Lambda
  - **Lab 27:** Resource management with Tags and Resource Groups
  - **Lab 28:** IAM access control using Resource Tags
  - Performed testing and resource cleanup after each lab

- **Skills Development:**
  - Improved ability to design secure AWS systems following best practices
  - Strengthened IAM-based access control skills
  - Gained proficiency in resource tagging strategies
  - Enhanced ability to analyze security findings and compliance
  - Improved practical deployment skills using AWS Console

- **System Thinking:**
  - Developed a security-first mindset in cloud architecture
  - Learned to integrate multiple AWS services for comprehensive security
  - Understood trade-offs between security, cost, and performance

- **Resource Management:**
  - Consistently cleaned up resources to avoid unnecessary costs
  - Applied tagging strategies for efficient resource control
  - Maintained awareness of Free Tier usage limits