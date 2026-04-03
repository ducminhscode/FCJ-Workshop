---
title : "Worklog Week 5"
date :  "`r Sys.Date()`" 
weight : 5
pre: <b> 1.5 </b>
chapter : false
---

### Week 5 Objectives:

- Master the setup and administration of a high-performance shared file system on Amazon FSx for Windows File Server.
- Clearly understand and proficiently implement Static Website Hosting on Amazon S3 combined with Amazon CloudFront.
- Study technical documentation and understand the data flow between Jira Service Management (JSM) and AWS.
- Set up the development environment and initialize the source code structure.
- Define the standard data structure (Payload) for communication between system components.
- Complete the `secrets.py` module with a caching mechanism.
- Develop the Just-In-Time core logic in `jit_access.py`: time mapping and Assignment/Group management.
- Build an Emergency Revoke mechanism.
- Develop the `jira_client.py` module to connect the automation system with Jira Service Management.
- Implement Retry and Exponential Backoff mechanisms to ensure API call reliability.
- Implement ticket state transitions and integrate approval/rejection via the Service Desk API (with fallback mechanism).

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
|:---:|------|:----------:|:---------------:|--------------------|
| Mon | - Hands-on practice: Setting up a high-performance shared file system based on Windows Server ([Module 04 - Lab 25](https://drive.google.com/drive/folders/1gp6idSIk6RIaVF3olLtOnx2_w-ontByp?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Infrastructure preparation: Create VPC, Active Directory (AWS Managed Microsoft AD), launch Windows Server (EC2)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ FSx setup: SSD Multi-AZ file system, HDD Multi-AZ file system<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Connect and create File Share<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Storage optimization: Data Deduplication, Shadow Copies<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Administration & monitoring: Session management, User Quotas, Continuously Available Shares<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Scaling: Increase throughput and storage capacity<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Performance monitoring and evaluation<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 23/03/2026 | 23/03/2026 | [Amazon FSx for Windows File Server](https://000025.awsstudygroup.com/) |
| Tue | - Hands-on practice: Amazon S3 Static Website Hosting ([Module 04 - Lab 57](https://drive.google.com/drive/folders/1wwixlCyGcefwCB5F8Gq094LTd1t_eScY?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Data preparation: Create an S3 bucket, upload data<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure Static Website Hosting: Enable static website hosting on S3, specify the index document and error document<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Set up access control (Security and Permissions): Configure Block Public Access, set up Bucket Policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Enhance performance and security with CloudFront: Use Amazon CloudFront, configure OAC/OAI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Manage lifecycle and data: Enable bucket versioning, perform data migration and copying<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Clean up resources | 24/03/2026 | 24/03/2026 | [Starting with Amazon S3](https://000057.awsstudygroup.com/) |
| Wed | **Logic Analysis & Project Initialization**<br>- Research documentation for group project:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Study AWS IAM Identity Center API (`boto3`, `sso-admin`) for provisioning/deprovisioning access<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Study Jira Service Management Webhook structure to extract ticket data<br>- Environment setup:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Initialize Git repository for backend<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Set up Python Virtualenv and install dependencies: `boto3`, `requests`, `pytest`<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure AWS CLI with appropriate IAM permissions for local Lambda testing<br>- Build project structure following AWS Lambda best practices:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ `/src/lambda_functions/`: Lambda source code (Executor, Expiry, Email)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ `/src/shared/`: Shared modules (Jira client, AWS secrets)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ `/scripts/`: Supporting scripts (data population)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ `/tests/`: Unit tests<br>- Design system logic:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Draw detailed sequence diagram for Executor function | 25/03/2026 | 25/03/2026 | [AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)<br>[Boto3](https://docs.aws.amazon.com/boto3/latest/)<br>[AWS SSO Admin API](https://docs.aws.amazon.com/boto3/latest/reference/services/sso-admin.html)<br>[Jira Service Management](https://developer.atlassian.com/cloud/jira/service-desk/webhooks/)<br>[Atlassian](https://developer.atlassian.com/cloud/jira/platform/webhooks/)<br>[AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)<br>[Python Virtualenv](https://docs.python.org/3/library/venv.html)<br>[AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) |
| Thu | **Build Lambda Layer (Common Utilities)**<br>- Develop `secrets.py` module:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Implement `get_secret(secret_name, region_name, use_cache)` with global caching<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Create wrappers: `get_jira_credentials` and `get_webhook_auth_secrets`<br>- Develop `jit_access.py` module:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Implement `map_duration_to_tier(duration_hours)` with rounding-up logic (1h, 2h, 4h, 8h, 12h)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Build `JITAccessManager` class supporting Account Assignment and Access Group<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Handle permission revocation: Implement the `immediate_revoke_with_user_tag` logic to immediately disable credentials by applying user tags or deactivating the account<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Emergency Revoke: Develop the `emergency_revoke_all_access` function to revoke all existing user access in case of a security incident | 26/03/2026 | 26/03/2026 |  |
| Fri | **Build Lambda Layer (Jira Integration)**<br>- Build connection infrastructure:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Implement the `_create_session_with_retry` function using `urllib3.util.retry`<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Configure retry strategy: 3 retries, backoff factor of 1, focusing on HTTP errors 500, 502, 503, 504<br>- Develop status update logic:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Implement the `transition_jira_status` function to dynamically find transition IDs based on names for better flexibility<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Develop the `update_jira_status` function as a wrapper to map business statuses (Executed, Failed, Expired) to corresponding steps in the Jira workflow<br>- Integrate Jira Service Desk API:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Implement the `approve_service_desk_request` function using the dedicated Service Desk endpoint instead of the standard Jira Core API<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Build a fallback mechanism: If a specific approval is not found, the system will automatically fall back to using the Transition API to approve the request (`_approve_via_transition`)<br>- Authentication and security: Successfully integrate with the `secrets.py` module to securely retrieve credentials (`base_url`, `api_token`, `user_email`) | 27/03/2026 | 27/03/2026 |  |
| Sat | - Rest and prepare for the upcoming week | 28/03/2026 | 28/03/2026 | |
| Sun | - Rest and prepare for the upcoming week | 29/03/2026 | 29/03/2026 | |

### Week 5 Achievements:

- Hands-on Labs:
  - **Lab 25:** Successfully deployed a high-performance shared file system on Amazon FSx (Multi-AZ, SSD & HDD, Data Deduplication, Shadow Copies, User Quotas, Continuously Available Shares).
  - **Lab 57:** Successfully deployed Static Website Hosting on Amazon S3 integrated with CloudFront (OAC, Bucket Policy, Versioning).
- Group Project:
  - Development environment fully set up locally.
  - Well-structured codebase following AWS Lambda best practices.
  - `secrets.py` module works reliably with global caching for Jira API Token, Webhook Secret, and AWS config.
  - `jit_access.py` module: Implement accurate Just-In-Time (JIT) time calculation logic. 
  - Successfully integrated the following methods:
    - Accurate time-to-tier mapping (1h, 2h, 4h, 8h, 12h).
    - Integrated Account Assignment and Access Group management.
    - Emergency revoke mechanism via user tagging.
  - `jira_client.py` completed with robust retry mechanism.
  - System capable of fully automating Jira workflows (approval, completion, failure handling).
  - Compatible with both Jira Software and Jira Service Management.
  - Challenges & Solutions:
    - Timezone issue: AWS Lambda uses UTC while Vietnam uses GMT+7 → risk of incorrect expiration time. **Solution:** Store all timestamps in UTC in DynamoDB, convert to Asia/Ho_Chi_Minh only when displaying or sending notifications.
    - AWS IAM Identity Center complexity: Multiple identifiers (ARN, Instance ID, Permission Set ID). **Solution:** Use a one-time populate script and store metadata in AWS Secrets Manager.
    - Difference between Jira Software and JSM. **Solution:** Check `canAnswerApproval` → prioritize Service Desk API → fallback to Transition API.
- Skills Gained:
  - Enhance hands-on skills with AWS infrastructure: managing Amazon FSx for Windows File Server and optimizing static websites on S3 and CloudFront.
  - Improve proficiency in using boto3 with AWS IAM Identity Center (SSO Admin API) and handling complex identifiers (ARNs, Permission Sets, Account Assignments).
  - Develop stronger skills in building clean, reusable Python modules (Lambda Layers) with professional caching, retry, and error handling mechanisms.
  - Gain proficiency in integrating third-party APIs (Jira Service Management & Service Desk API), including handling webhooks, transitions, approval workflows, and fallback mechanisms.
  - Improve the ability to design Just-In-Time (JIT) Access Control logic, manage access revocation timing, and handle emergency security incidents.
  - Strengthen debugging and problem-solving skills for real-world issues related to time zones, API rate limits, and differences between Jira Core and Jira Service Management.
  - Enhance project organization skills for large-scale codebases, using Virtualenv, Git, and preparing for serverless environments (AWS Lambda).
