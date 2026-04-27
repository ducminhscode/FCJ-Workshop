---
title : "Worklog Week 6"
date :  "`r Sys.Date()`" 
weight : 6
pre: <b> 1.6 </b>
chapter : false
---

### Week 6 Objectives:

- Clearly understand the security model on AWS, especially the Shared Responsibility Model, to distinguish responsibilities between the service provider and the user in protecting the system.
- Master the concepts and mechanisms of AWS Identity and Access Management (IAM), including IAM Users, Groups, Roles, and Policies (JSON), along with the principle of least privilege.
- Understand the role and benefits of IAM Roles, particularly in granting temporary permissions and enhancing security for AWS services.
- Learn about user authentication and authorization through Amazon Cognito, including User Pools, Identity Pools, and integration with web/mobile applications.
- Gain a clear understanding of managing multi-account environments with AWS Organizations, including organizational structure, Organizational Units (OUs), and permission control mechanisms using Service Control Policies (SCPs).
- Explore AWS Identity Center (SSO) to centrally manage access and implement single sign-on across multiple accounts and applications.
- Understand encryption mechanisms and key management in AWS Key Management Service (KMS), including key types (CMK, Data Keys) and how they are used to protect data.
- Learn about AWS Security Hub, including how to aggregate, analyze, and evaluate security findings based on standards such as CIS Benchmark and AWS Foundational Security Best Practices.
- Practice hands-on labs related to:
  - Security analysis and assessment using AWS Security Hub.
  - Optimizing EC2 costs using AWS Lambda.
  - Managing resources with Tags and Resource Groups.
  - Controlling EC2 access via IAM and Resource Tags.
- Develop skills in designing cloud systems with a security-first approach, combining multiple AWS services to build secure and efficient architectures.
- Improve technical documentation reading skills, hands-on implementation via AWS Console and CLI, and build a mindset for cost optimization and efficient resource management.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Learn about AWS security and the Shared Responsibility Model:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Shared Responsibility Model<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Key security services: IAM, Amazon Cognito, AWS Organizations & Identity Center, AWS KMS<br>- Manage identities and access in AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Root account concept<br>&nbsp;&nbsp;&nbsp;&nbsp;+ IAM Users and Groups<br>&nbsp;&nbsp;&nbsp;&nbsp;+ IAM Policies (JSON): evaluation logic and types<br>&nbsp;&nbsp;&nbsp;&nbsp;+ IAM Roles and their importance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Why use IAM Roles?<br>- Manage authentication and authorization with Amazon Cognito:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Concepts, features, and benefits<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Core components: User Pool and Identity Pool<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Architecture and authentication flow<br>- AWS Organizations:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Role and benefits (centralized management, risk reduction, automation)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Structure: Management Account, Organizational Units, Member Accounts<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Permission control using SCP<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Consolidated Billing<br>- AWS Identity Center:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Concept and architecture<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Multi-account access model<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Benefits: SSO, centralized control, enhanced security<br>- AWS Key Management Service:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Concept, features, and standards<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Key types: CMK, Data Key<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Working mechanism<br>- AWS Security Hub:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Concept and operation<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Scope of checks<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Security standards: PCI DSS, AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Reading results and examples<br>- Additional labs and research | 30/03/2026 | 30/03/2026 | [Share Responsibility Model](https://youtu.be/tsobAlSg19g?si=VRtW8Y1D1zuJUnFL)<br>[Amazon Identity and access management](https://youtu.be/N_vlJGAqZxo?si=h_taNgRzWo2OlED5)<br>[Amazon Cognito](https://youtu.be/pZ2fgEFK3Vs?si=0bjyUxZmq4EAPFQg)<br>[AWS Organization](https://youtu.be/5oQY8Rogz9Y?si=Xx2M1i3wiO7h5HNz)<br>[AWS Identity Center](https://youtu.be/NW1xrMkNMjU?si=Y0WEJoN_k6JAjbrR)<br>[Amazon Key Management Service](https://youtu.be/GMihNQojhZc?si=hkSfBPC9axfHKS0G)<br>[AWS Security Hub](https://youtu.be/clj2E0rNBEs?si=O9WI1Q_939tNxLKG)<br>[Hands-on and Additional research](https://youtu.be/0SdpD2GPYz4?si=yN_FWFNR3honqcu1) |
| Tue | - Practice getting familiar with AWS Security Hub (Free Tier accounts cannot perform this lab) ([Module 05 - Lab 18](https://drive.google.com/drive/folders/1V35rUoHCcNq5eqQzhATQSY5dBU8wcOiu?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Security standards: Learn about frameworks such as AWS Foundational Security Best Practices or the CIS AWS Foundations Benchmark<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Enable Security Hub<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Analyze security findings<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Evaluate compliance status<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 31/03/2026 | 31/03/2026 | [Getting Started with AWS Security Hub](https://000018.awsstudygroup.com/) |
| Wed | - Practice optimizing EC2 costs using AWS Lambda ([Module 05 - Lab 22](https://drive.google.com/drive/folders/1b4NZnxlqQlM4Ku0PZ6i4MzabPinZzPHP?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Infrastructure setup: Configure VPC, Security Groups, EC2 instances, and integrate webhooks with Slack to receive notifications<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tagging: Use resource tags to identify specific EC2 instances that Lambda will manage<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Permissions: Create an IAM Role that allows Lambda to perform StartInstances and StopInstances actions on EC2<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Build Lambda functions: Create functions to stop instances and start instances<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Test the results<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 01/04/2026 | 01/04/2026 | [Optimizing EC2 Costs with Lambda](https://000022.awsstudygroup.com/) |
| Thu | - Practice managing resources using Tags and Resource Groups on AWS ([Module 05 - Lab 27](https://drive.google.com/drive/folders/1ME_mYIb3V8FS36IrNMnCtosvYF_DLxw9?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Using Tags on the Console<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Using Tags via CLI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create Resource Groups: Allows grouping resources (such as EC2, S3, CloudFormation stacks) within the same Region based on query results<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 02/04/2026 | 02/04/2026 | [Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com/) |
| Fri | - Practice managing access to EC2 services using Resource Tags and IAM ([Module 05 - Lab 28](https://drive.google.com/drive/folders/1uNkrzbHSkqqGdMQkmgfyCVnLOAxnAJo7?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Preparation: Create IAM users and configure the required prerequisites<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create IAM Policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create IAM Role<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Test the policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 03/04/2026 | 03/04/2026 | [Manage access to EC2 Services with Resource Tags through IAM Services](https://000028.awsstudygroup.com/) |
| Sat | - Rest and prepare for the upcoming week | 04/04/2026 | 04/04/2026 | |
| Sun | - Rest and prepare for the upcoming week | 05/04/2026 | 05/04/2026 | |

### Week 6 Achievements:

- Theoretical Knowledge:
  - Clearly understand the Shared Responsibility Model, distinguishing security responsibilities between AWS and the user across layers (infrastructure, platform, application, data).
  - Master the components of IAM (Identity and Access Management):
    - IAM Users, Groups, Roles, and Policies (JSON)
    - The principle of least privilege and policy evaluation logic (explicit deny > allow)
  - Understand the role and usage of IAM Roles, especially for granting temporary permissions to services (EC2, Lambda).
  - Understand authentication and authorization mechanisms in Amazon Cognito:
    - Differentiate between User Pools (authentication) and Identity Pools (authorization)
    - Grasp the login flow and permission granting for web/mobile applications
  - Understand how to organize and manage multi-account environments with AWS Organizations:
    - Structure: Management Account, Organizational Units (OUs), Member Accounts
    - Permission control via Service Control Policies (SCPs)
    - Consolidated Billing
  - Understand how AWS Identity Center (SSO) works:
    - Centralized access management
    - Account-based permission assignment in multi-account environments
  - Understand encryption and key management using AWS Key Management Service (KMS):
    - Differentiate between Customer Managed Keys (CMK) and Data Keys
    - Understand the envelope encryption mechanism
  - Understand the role of AWS Security Hub:
    - Aggregating and analyzing security findings
    - Standards: CIS AWS Foundations Benchmark, AWS Foundational Security Best Practices, PCI DSS
    - How to interpret and evaluate security assessment results
- Hands-on Labs:
  - **Lab 18:**
    - Enable and explore AWS Security Hub (within Free Tier limits)
    - Analyze security findings
    - Evaluate compliance against security standards
  - **Lab 22:**
    - Build a Lambda function to automatically start/stop EC2 instances
    - Use Tags to identify target resources
    - Integrate notifications via webhook (Slack)
  - **Lab 27:**
    - Tag resources using both Console and CLI
    - Create Resource Groups for centralized management
  - **Lab 28:**
    - Build IAM Policies based on tag conditions
    - Create IAM Roles and test EC2 access permissions
  - Perform validation and clean up resources after each lab
- Skills Development:
  - Enhance ability to design secure systems on AWS following best practices
  - Build and manage flexible access control using IAM, Roles, and Policies
  - Become proficient in using Tags for resource management and control
  - Develop skills in analyzing security findings and evaluating compliance
  - Improve technical documentation reading and hands-on implementation using AWS Console
- System Thinking:
  - Develop a security-first mindset when designing cloud systems
  - Understand how to combine services (IAM, Cognito, Organizations, KMS) to build comprehensive security architectures
  - Recognize trade-offs between security, cost, and performance
- Resource Management:
  - Clean up resources after each lab to avoid unnecessary costs
  - Use Tags effectively to control and optimize resources
  - Be mindful of staying within Free Tier limits during practice