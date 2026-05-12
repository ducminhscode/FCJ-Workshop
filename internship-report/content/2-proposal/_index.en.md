---
title : "Project Proposal"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre: <b> 2. </b>
---

# Production Access Request Portal

## Table of Contents

- [Production Access Request Portal](#production-access-request-portal)
  - [Table of Contents](#table-of-contents)
  - [Project Overview](#project-overview)
    - [Summary](#summary)
    - [Target Users](#target-users)
    - [Design Principles](#design-principles)
  - [Problems to Solve](#problems-to-solve)
    - [Problem Description](#problem-description)
    - [Solution](#solution)
    - [Architecture Comparison](#architecture-comparison)
  - [Solution Architecture](#solution-architecture)
    - [System Architecture Diagram](#system-architecture-diagram)
    - [AWS Services Used](#aws-services-used)
    - [System Components](#system-components)
    - [Operational Workflow](#operational-workflow)
      - [Standard Grant Flow](#standard-grant-flow)
      - [Email Approval Flow](#email-approval-flow)
      - [Auto Expiry and Access Revocation Flow](#auto-expiry-and-access-revocation-flow)
      - [Emergency Revocation Flow](#emergency-revocation-flow)
      - [Operational Principles and Exception Handling](#operational-principles-and-exception-handling)
  - [Roadmap \& Implementation Milestones](#roadmap--implementation-milestones)
    - [Phase 1: Requirements Analysis and Solution Design (Weeks 2-4)](#phase-1-requirements-analysis-and-solution-design-weeks-2-4)
    - [Phase 2: Foundation Infrastructure Deployment (Weeks 5-6)](#phase-2-foundation-infrastructure-deployment-weeks-5-6)
    - [Phase 3: System Business Logic Development (Weeks 7–8)](#phase-3-system-business-logic-development-weeks-78)
    - [Phase 4: Testing and User Acceptance (Weeks 9-10)](#phase-4-testing-and-user-acceptance-weeks-9-10)
    - [Phase 5: Production Deployment and Operational Handover (Week 11)](#phase-5-production-deployment-and-operational-handover-week-11)
    - [Overall Implementation Plan](#overall-implementation-plan)
    - [Post-Deployment Expansion Plan](#post-deployment-expansion-plan)
  - [Budget Estimation](#budget-estimation)
  - [Risk Assessment](#risk-assessment)
    - [Risk Assessment Matrix](#risk-assessment-matrix)
    - [Incident Response Plan](#incident-response-plan)
  - [Expected Outcomes](#expected-outcomes)

## Project Overview

### Summary

Production Access Request Portal is a temporary access management system (Just-in-Time Access) built on a fully serverless architecture (AWS Lambda, API Gateway, DynamoDB), designed to securely and automatically control access to AWS Production accounts. The process begins when users submit requests through Jira Service Management, after which the system orchestrates the approval workflow via email using Amazon SES and grants permissions through AWS IAM Identity Center. The most significant improvement introduced in version 2.0 is the Group-Based Access mechanism, which enables the system to automatically revoke access permissions (within Access Groups) within just 60 seconds immediately after the access expiration time (TTL), completely overcoming the traditional delay limitation of up to 12 hours found in conventional approaches. All activities are continuously monitored through CloudWatch and CloudTrail, ensuring transparency and strict compliance for sensitive production environments.

### Target Users

Within the Production Access Request Portal system, multiple stakeholders participate in the process of requesting, approving, provisioning and monitoring access to the Production environment. Each role is responsible for a specific function, contributing to the secure, efficient and policy-compliant operation of the system.

| Role | Description | Interaction with the System |
|------|-------------|-----------------------------|
| **End User (Developer/Engineer)** | A person who needs access to the Production environment to perform tasks such as deployment, troubleshooting, or system verification. | Users interact with the system through the Jira Service Management Portal to submit access requests. Once approved, they use AWS IAM Identity Center to log in and receive temporary access permissions. |
| **Approver (Team Lead/Manager)** | A person responsible for reviewing and approving Production access requests. This role acts as a control point to ensure that only valid and necessary requests are granted access. | Approvals are performed directly through the Jira interface or via email using confirmation links (approve/decline). |
| **Platform/DevOps Engineer** | A person responsible for designing, deploying and operating the system. | Manage the entire infrastructure using Terraform (Infrastructure as Code), develop and maintain Lambda functions, monitor system performance, handle incidents and implement architectural improvements when necessary to ensure system stability. |
| **Security/Compliance Team** | A team responsible for ensuring that the system complies with the organization's security standards and policies. | Monitor access activities through audit logs, review access control policies and assess security risks. |

### Design Principles

The Production Access Request Portal system is designed based on modern security and architectural principles to ensure that access management for the Production environment is performed securely, transparently and efficiently. The core principles include:
- **Zero Standing Privileges:**  
  - This principle states that the system does not allow any permanent or standing access to the Production environment. Instead, all access permissions must be requested through the system, go through an approval process and only remain valid for a limited period of time.  
  - Once the granted duration expires, access permissions are automatically revoked without requiring manual intervention. This approach significantly reduces the risk of credential exposure and minimizes the possibility of privilege misuse within the system.
- **Least Privilege:**  
  - This principle ensures that each user is granted only the minimum level of access necessary to perform their tasks. The system defines three main access levels: ReadOnly, PowerUser and Admin, corresponding to different levels of interaction with AWS resources.  
  - Each permission level is mapped to specific IAM policies and controlled through scope limitation mechanisms. As a result, the system minimizes risks in the event of account compromise and prevents actions that exceed authorized privileges.
- **Defense in Depth:**  
  - This principle is applied to establish multiple overlapping layers of security throughout the system. Specifically, the system implements user authentication through SSO at the Jira Portal, combined with API Gateway authentication using API keys and request throttling mechanisms.  
  - In addition, webhooks are protected using HMAC-SHA256 digital signatures to ensure data integrity. The system also applies strict access control policies through IAM, along with comprehensive logging and monitoring using CloudWatch and CloudTrail.  
  - Implementing multiple security layers ensures that even if one layer is bypassed, the remaining layers can still protect the system against threats.
- **Fail-Safe Defaults:**  
  - This principle dictates that the system denies all access requests by default unless all required conditions are satisfied. Access is granted only when every step in the workflow, including authentication, approval and permission provisioning, is successfully completed.  
  - In cases where errors occur or the system cannot determine a valid state, access will not be granted. This ensures that no permissions are unintentionally or improperly assigned.
- **Complete Auditability:** This principle ensures that all activities within the system are fully recorded and traceable. Information related to access requests, approvals, permission grants and revocations is stored across multiple systems such as Jira, CloudWatch, CloudTrail and DynamoDB. As a result, the system can provide comprehensive audit information for compliance requirements, security investigations and incident analysis. Ensuring transparency and traceability is a critical factor in Production access management systems.

## Problems to Solve

### Problem Description

Before this system was introduced, the process of managing access to the Production environment within the organization had many limitations, posing significant security risks and operational challenges. The key issues identified are as follows:

| Problem | Impact | Risk Level |
|--------|----------|:-------------:|
| **Manual and Inconsistent Access Provisioning** | The process of granting Production access was primarily performed manually through the creation or modification of IAM Users for each request. This approach was not only time-consuming but also highly prone to configuration errors, potentially resulting in excessive permissions or insufficient access control. The lack of a standardized process also caused inconsistencies across teams, making system management and scalability more difficult. | High |
| **No Automatic Access Expiration Mechanism** | One of the most critical issues was that Production access permissions were not automatically revoked after tasks were completed. This resulted in the existence of long-lived credentials within the system. Maintaining access permissions for extended periods significantly increased the risk of exploitation if credentials were leaked, while also violating modern security principles such as Zero Standing Privileges. | Very High |
| **Lack of Auditability and Traceability (Audit Trail)** | The legacy system did not provide sufficient information to monitor and trace access activities. The inability to accurately determine who accessed the system, when the access occurred and what actions were performed created major difficulties during auditing and incident investigations. This issue was especially severe in highly regulated environments where maintaining a complete audit trail is mandatory. | Very High |
| **Fragmented and Inefficient Approval Process** | The access approval process was often handled through multiple disconnected channels such as email, messaging applications, or direct communication, lacking centralization and automation. As a result, request processing times were prolonged, SLAs were unclear and tracking the current status of requests became difficult. This reduced the productivity of engineering teams and increased delays in resolving Production incidents. | Medium |
| **No Emergency Access Revocation Mechanism** | In cases involving detected security risks or compromised accounts, the system did not provide the capability to immediately revoke access permissions. This limitation prevented the organization from responding quickly to security incidents, increasing both the potential impact and associated risks. | Critical |

### Solution

To address the limitations in the Production access management process, the Production Access Request Portal was developed as a comprehensive automation solution that applies modern security principles and leverages a serverless architecture on AWS. The main solutions are as follows:
- **Automating the Access Provisioning Workflow:**  
  - Instead of relying on manual operations, the system standardizes and automates the entire access provisioning process following the model:
    ```
    Request → Approval → Provisioning → Expiry → Revocation
    ```
  - Users submit requests through Jira Service Management, after which the system automatically processes the remaining steps using API Gateway and AWS Lambda. This approach minimizes human error while ensuring consistency across the entire system.
- **Applying the Just-in-Time Access Model:**  
  - The system implements a Just-in-Time (JIT) Access model, where access permissions are granted only when needed and remain valid for a limited period of time.
  - Each access request is associated with a specific duration and mapped into standardized duration tiers. Once the duration expires, the system automatically revokes access permissions without requiring manual intervention.
  - This solution completely eliminates the existence of long-lived access permissions within the Production environment.
- **Using Group-Based Access Instead of Direct Permission Assignment:** One of the key improvements of the system is the transition from a Direct Assignment model to a Group-Based Access model.
  - Instead of assigning permissions directly to individual users, the system will:
     - Pre-create access groups corresponding to specific permission levels and durations.
     - Add users to groups when access is granted.
     - Remove users from groups when access expires.
  - This approach reduces provisioning time to under 5 seconds, enables near real-time access revocation and significantly decreases management complexity.
- **Integrating a Centralized and Flexible Approval Mechanism:** The system tightly integrates with Jira Service Management to manage the entire approval workflow. In addition, it supports email-based approvals through confirmation links (approve/decline), increasing flexibility for managers. All requests are tracked with clear statuses, ensuring transparency and SLA-based monitoring.
- **Establishing Automatic and Emergency Access Revocation Mechanisms:** The system uses DynamoDB TTL combined with AWS Lambda to automatically detect and revoke expired access permissions. When a session expires, Lambda automatically removes the user from the access group, causing credentials to become invalid within 60 seconds. The system also supports an Emergency Revocation mechanism, allowing all user access permissions to be removed within a very short period when security risks are detected.
- **Enhancing Monitoring and Auditability:** To address the lack of audit trails, the system records activities across multiple layers:
    - Jira: Stores requests and approval information.
    - CloudWatch: Captures system execution logs.
    - CloudTrail: Records AWS API calls.
    - DynamoDB: Stores session metadata.
- **Applying a Serverless Architecture for Operational Optimization:** The system is built entirely on a serverless architecture using services such as AWS Lambda, API Gateway and DynamoDB, which provides:
    - Automatic scaling based on usage demand.
    - No need for server infrastructure management.
    - Optimized operational costs through a pay-per-use model.
    - Improved system reliability and availability.

### Architecture Comparison

| Criteria | Direct Assignment (v1.0) | Group-Based Access (v2.0) |
|----------|:------------------------:|:-------------------------:|
| **Access Provisioning Mechanism** | Directly assign users to accounts | Add users to pre-assigned access groups |
| **Provisioning Time** | 15-30 seconds (asynchronous operation) | < 5 seconds (synchronous operation, near real-time) |
| **Access Revocation Time** | Up to 12 hours | Within 60 seconds |
| **Credential Validity After Revocation** | Credentials may remain valid for a period of time | Credentials are invalidated almost immediately |
| **Operational Complexity** | High - requires creating/removing assignments each time | Low - only group membership management is required |
| **Scalability** | Limited as the number of users increases | Easily scalable through reusable groups |
| **Security Incident Response** | Slow and difficult to control | Fast, supports bulk revocation |

## Solution Architecture

### System Architecture Diagram

<img src="/images/figure-proposal.png" alt="figure-proposal" style="width:600px !important; max-width:900px !important;">
<p style="text-align:center; font-style:italic;">Figure 1. Production Access Request Portal System Architecture Diagram</p>

### AWS Services Used

| AWS Service | Role in the System | Reason for Selection |
|-------------|--------------------|----------------------|
| **AWS Lambda** | Handles the entire business logic of the system, including access provisioning, revocation, email approval processing and automatic expiry handling. | The serverless architecture eliminates the need for server management, provides auto scaling, offers low operational costs based on execution (pay-per-use) and is well suited for event-driven workloads. |
| **Amazon API Gateway** | Provides REST API endpoints to receive webhooks from Jira. | A managed API service that supports authentication, throttling, rate limiting, usage plans and native integration with Lambda. |
| **AWS IAM Identity Center** | Manages AWS access permissions through Permission Sets, Access Groups and Group Memberships. Enables fast access provisioning and revocation using a group-based approach. | It is AWS’s official SSO solution, supporting centralized access management and near real-time credential revocation (within 60 seconds). |
| **Amazon DynamoDB** | Stores session metadata (Sessions Table) and approval tokens (Approval Tokens Table), while supporting TTL auto-expiry and triggering revoke workflows through DynamoDB Streams. | A low-latency serverless NoSQL database that supports TTL auto-expiry and DynamoDB Streams. |
| **AWS Secrets Manager** | Securely stores Jira credentials, webhook API keys, token secrets and access group mappings. | Provides better security than hardcoded secrets or configuration files, with encryption at rest and automatic rotation support. |
| **Amazon SES** | Sends approval emails, access provisioning notifications and access revocation notifications. | Low cost and easy integration with Lambda. |
| **Amazon CloudWatch** | Provides logging (structured JSON logs), monitoring metrics and alarms for Lambda, API Gateway, DynamoDB and other services. | Native integration with Lambda and other AWS services. |
| **AWS CloudTrail** | Records audit trails for all API calls related to Identity Center, Lambda and access provisioning operations. | Helps satisfy compliance requirements by ensuring complete auditability. |

### System Components

The Production Access Request Portal system is organized using a multi-layered architecture, where each component serves a specific responsibility while closely coordinating with others to create a fully automated, controlled and auditable Production access provisioning workflow. From a logical perspective, the system consists of the following major component groups: the request interface layer (Jira Service Management), the API ingestion layer, the business logic processing layer using AWS Lambda, the data storage layer, the identity and access management layer, the notification layer and the monitoring/operations layer. This design aligns with the serverless architecture described in the documentation while also supporting the Group-Based Access mechanism for near real-time access provisioning and revocation.

| Component | Main Function | Role in the Processing Flow |
|-----------|----------------|------------------------------|
| **Jira Service Management Portal** | Receives access requests from users, displays request status and manages the approval workflow | Starting point of the entire process |
| **API Gateway** | Receives webhooks from Jira, validates requests and forwards them to Lambda | Entry point of the serverless backend |
| **Lambda Executor** | Handles access provisioning after requests are approved | Executes the core access provisioning logic |
| **Lambda Email Approval** | Generates approval emails and processes approve/decline actions from email | Supports flexible approval through email |
| **Lambda Expiry** | Monitors expired sessions and automatically revokes access permissions | Ensures access permissions only exist within the allowed duration |
| **DynamoDB** | Stores sessions, approval tokens, TTL values and metadata for revocation workflows | State storage layer of the system |
| **Secrets Manager** | Stores secrets, group mappings, token secrets and Jira credentials | Protects sensitive information |
| **IAM Identity Center** | Manages permission sets, access groups and group memberships | Core component for provisioning and revoking access permissions |
| **Amazon SES** | Sends approval emails, notifications and expiry alerts | Communication channel for approvers and requesters |
| **CloudWatch/CloudTrail** | Provides logs, metrics and audit trails | Supports monitoring, traceability and compliance |

### Operational Workflow

The operational workflow of the Production Access Request Portal is designed as an end-to-end automation model, starting from the moment a user submits a request through Jira Service Management until access permissions are granted, monitored for expiration and automatically revoked once they expire. The entire lifecycle of each request is fully recorded in Jira, DynamoDB, CloudWatch and CloudTrail to ensure auditability and traceability.

#### Standard Grant Flow

The standard access provisioning process is triggered when an End User submits a Production access request through the Jira Service Management Portal. Once the request is created, the Jira workflow transitions the request into the approval waiting state and simultaneously triggers a webhook to AWS API Gateway to initiate the provisioning process. Lambda Executor is the component responsible for handling the main access provisioning logic, including webhook validation, mapping lookup, adding users to groups and storing session information in DynamoDB.

The standard processing flow is as follows:
1. **User submits an access request:**  
   The End User fills out a request form on the Jira Service Management Portal with details such as the target account, access type and desired access duration.
2. **Jira transitions the request to the approval state:**  
   Jira creates a ticket and triggers an automation rule to send a webhook request to the backend system.
3. **API Gateway receives the webhook:**  
   API Gateway receives the request from Jira, validates the API key and forwards the payload to Lambda Executor.
4. **Lambda Executor validates and processes the request:**  
   Lambda validates the payload, verifies the webhook signature, retrieves the required data and maps the requested duration into a standardized duration tier.
5. **Lookup the corresponding access group:**  
   The system uses Secrets Manager to retrieve the mapping between the AWS account, access type and duration tier, then determines the appropriate access group within IAM Identity Center. This approach aligns with the Group-Based Access model described in the documentation.
6. **Add the user to the access group:**  
   Lambda Executor calls the Identity Center API to create a group membership for the user. Once the membership is successfully created, the corresponding access permissions are granted almost immediately.
7. **Store the session in DynamoDB:**  
   A session record is created in the AccessSessions table, containing information such as the request ID, requester, group ID, membership ID, provisioning time and expiration time. A TTL value is configured to trigger the automatic revocation workflow.
8. **Update the status in Jira:**  
   The Jira ticket is transitioned to the Approved state and serves as the primary tracking source for both users and approvers.
9. **Send notification email to the requester:**  
   The system sends an email notification indicating that access has been successfully granted, along with the required information to access the IAM Identity Center Portal.
10. **User logs in and uses temporary access:**  
    The End User logs into the Identity Center Portal to use AWS access permissions within the approved time duration.

#### Email Approval Flow

In addition to direct approval through Jira, the system also supports email-based approvals to provide greater flexibility for Approvers. When a new request is created, Jira automation can invoke the Lambda Email Approval function to generate emails containing Approve/Decline links. This process uses HMAC-SHA256 signed tokens with a 24-hour expiration time and single-use validation to minimize the risk of forgery or replay attacks.

The processing flow is as follows:
1. **Jira triggers the email approval request:** When a request enters the pending approval state, Jira calls the `/email-approval/request` endpoint.
2. **Lambda generates a secure approval token:** The token is generated from `token_id`, `expiry_timestamp` and `hmac_signature`, then its metadata is stored in the Approval Tokens table within DynamoDB.
3. **Send email to the Approver:** Lambda renders an HTML email containing `Approve` and `Decline` buttons and sends it through Amazon SES.
4. **Approver selects an action:** When the approver clicks one of the buttons, the request is sent to the `/email-approval/action` endpoint.
5. **The system validates the token:** Lambda verifies the HMAC signature, expiration status and `used` flag stored in DynamoDB. If the token is valid, the system marks the token as used.
6. **Update the result back to Jira:** Lambda calls the Jira Service Desk Approval API to update the request status as approved or declined.
7. **Return a confirmation page:** The system displays a confirmation page for the Approver and simultaneously records logs for auditing purposes.

#### Auto Expiry and Access Revocation Flow

When a session reaches its expiration time, DynamoDB TTL automatically removes the session record. This deletion event is published through DynamoDB Streams and triggers Lambda Expiry to revoke the associated permissions. This mechanism is the key component that enables the system to revoke credentials within approximately 60 seconds.

The processing flow is as follows:
1. **DynamoDB TTL removes expired sessions:** When the session `ttl` value expires, DynamoDB automatically deletes the record.
2. **DynamoDB Streams generates a REMOVE event:** Lambda Expiry receives the deletion event from the stream and determines that it is a valid expiration event.
3. **Identify the membership to revoke:** Lambda reads the old session data to retrieve the corresponding `membership_id` and group name.
4. **Remove the user from the access group:** The system calls `DeleteGroupMembership` in IAM Identity Center to revoke the user’s access permissions.
5. **Credentials become invalidated:** Once the membership is removed, the user’s credentials become invalid within a very short period, achieving near real-time revocation.
6. **Update Jira and send notifications:** The Jira ticket is transitioned to the Expired state and the requester receives an email notification indicating that the access has expired.

#### Emergency Revocation Flow

In emergency situations such as credential leaks, suspicious activities, or security team requests, the system supports an emergency revocation mechanism that can remove all access permissions of a user within a short period of time. Lambda can enumerate all group memberships associated with the user and remove them to invalidate all active sessions.

The process includes the following steps:
1. **Trigger an emergency revocation request:** An operator or monitoring system submits an immediate revocation request.
2. **Retrieve all user memberships:** Lambda retrieves all group memberships associated with the user in IAM Identity Center.
3. **Remove the user from all groups:** All memberships related to the user are deleted to ensure that no active access permissions remain.
4. **Record the revocation event:** The system updates the status in DynamoDB, writes logs to CloudWatch and stores audit records for traceability purposes.

#### Operational Principles and Exception Handling

To ensure stable system operation, the following principles are consistently applied throughout the workflow:
- **No access is granted unless all conditions are satisfied:** Requests are processed only when the webhook is valid, the approver is authorized and the corresponding group mapping exists.
- **Each request can only have one final state:** Approved, Declined, Expired, or Revoked.
- **Email tokens are single-use only:** Prevents reopening old links to perform unintended actions.
- **All errors must be logged:** Authentication failures, API errors and provisioning failures must be recorded for investigation purposes.
- **Fail-safe is prioritized:** If the system cannot determine a valid state, access permissions will not be granted.

## Roadmap & Implementation Milestones

### Phase 1: Requirements Analysis and Solution Design (Weeks 2-4)

The objective of this phase is to clearly define the system scope, business requirements, access control model and overall architecture design.

Main tasks:
- Gather requirements from stakeholders (DevOps, Security, Engineering).
- Identify the list of AWS Accounts to be managed.
- Build the access control matrix (ReadOnly, PowerUser, Admin).
- Define standard duration tiers.
- Design the Jira Service Management workflow.
- Design the system architecture diagram.
- Design the DynamoDB data model.

**Milestone 1 - Architecture Approved**

### Phase 2: Foundation Infrastructure Deployment (Weeks 5-6)

The objective of this phase is to deploy the entire AWS infrastructure using Terraform to establish the foundation for the system.

Main tasks:
- Build Terraform modules: API Gateway, Lambda Functions, DynamoDB Tables, IAM Roles, Secrets Manager, SES Configuration.
- Create Permission Sets in IAM Identity Center.
- Create Access Groups for each account.
- Configure Group Assignments.
- Set up Terraform Backend (S3 + DynamoDB Lock).

**Milestone 2 - Infrastructure Ready**

### Phase 3: System Business Logic Development (Weeks 7–8)

This phase focuses on developing the core business logic of the system.

Main tasks:
- Develop Lambda Executor.
- Develop Lambda Email Approval.
- Develop Lambda Expiry.
- Build shared Lambda Layer.
- Integrate Jira APIs.
- Integrate IAM Identity Center APIs.
- Integrate Amazon SES.
- Implement structured logging.
- Integrate Secrets Manager cache.

**Milestone 3 - Core Features Completed**

### Phase 4: Testing and User Acceptance (Weeks 9-10)

This phase focuses on validating system stability, security and operational readiness in real-world scenarios.

Main tasks:
- Unit Testing for Lambda functions.
- Integration Testing for the entire workflow.
- Security Testing: API authentication, webhook signature validation, token replay prevention.
- Load Testing for API Gateway.
- TTL Expiry Testing.
- Emergency Revocation Testing.

**Milestone 4 - UAT Passed**

### Phase 5: Production Deployment and Operational Handover (Week 11)

The final phase focuses on deploying the system into the production environment and handing it over for operation.

Main tasks:
- Deploy the Production environment.
- Verify all resources.
- Populate Access Group Mapping.
- Verify Jira webhook integration.
- Verify SES production mode.
- Configure CloudWatch alarms.
- Set up monitoring dashboards.

**Milestone 5 - Production Go-Live**

### Overall Implementation Plan

| Phase | Timeline | Deliverables | Milestone |
|-------|-----------|--------------|-----------|
| Analysis & Design | Weeks 2-4 | Architecture, workflow, data model | Architecture Approved |
| Infrastructure Deployment | Weeks 5-6 | Terraform infrastructure | Infrastructure Ready |
| Business Logic Development | Weeks 7-8 | Lambda functions, API integration | Core Features Completed |
| Testing & User Acceptance | Weeks 9-10 | Testing reports, UAT | UAT Passed |
| Production Deployment | Week 11 | Production system live | Production Go-Live |

### Post-Deployment Expansion Plan

After the system is deployed and operating stably, future improvements may include:
- Support for Slack Approval Workflow.
- Real-time dashboard for monitoring active sessions.
- SIEM integration for the Security Team.
- Risk-based Access Approval.
- Machine Learning anomaly detection for access patterns.
- Multi-region deployment to improve high availability.

## Budget Estimation

| Service | Pricing | Assumptions | Estimated Monthly Cost |
|---------|---------|-------------|-------------------------|
| **AWS Lambda** | $0.2/million requests and $0.0000166667 per GB-second duration | Approximately 3000 invocations, 256 MB memory, average execution time around 5 seconds | ~ $0.06 |
| **API Gateway** | $3.5/million requests | Approximately 2000 requests | ~ $0.01 |
| **DynamoDB** | On-demand pricing | Approximately 4000 writes and 2000 reads | ~ $0.01 |
| **Secrets Manager** | $0.4/secret/month and $0.05/10000 API calls | 4 secrets, approximately 10000 API calls | ~ $1.6 |
| **Amazon SES** | $0.1/1000 emails | Approximately 2000 emails (grant + expiry) | ~ $0.02 |
| **CloudWatch Logs** | $0.5/GB ingested | Approximately 0.5 GB log ingestion | ~ $0.25 |
| **S3 (Terraform State)** | $0.023/GB | Less than 1 MB | ~ $0.00 |

The total estimated cost for the entire system is approximately **~$2/month**.

## Risk Assessment

Although the Production Access Request Portal is built on a serverless architecture with multiple security layers and automation mechanisms, the system still contains potential risks related to external service dependencies, operational failures, configuration issues, or security incidents. Conducting a risk assessment helps proactively establish mitigation strategies and ensure system availability in the Production environment.

### Risk Assessment Matrix

The risks are evaluated based on three main criteria: likelihood of occurrence, level of impact and priority level for mitigation.

| Risk | Description | Likelihood | Impact Level | Mitigation Priority |
|------|-------------|:-----------:|:------------:|:-------------------:|
| Jira Service Management outage | Unable to create new requests or update approval status | Medium | High | High |
| AWS IAM Identity Center failure or service degradation | Unable to grant or revoke access permissions | Low | Very High | High |
| DynamoDB TTL processing delay | Sessions expire but are not revoked at the expected time | Medium | High | High |
| Lambda function runtime failure | Provisioning or expiry workflows are interrupted | Medium | High | High |
| SES email delivery failure | Approvers or requesters do not receive notifications | Medium | Medium | Medium |
| Secrets Manager unavailable | Lambda cannot retrieve secrets to process requests | Low | High | Medium |
| Forged webhook or modified payload | Could lead to unauthorized provisioning if validation fails | Low | Very High | High |
| Incorrect access group mapping configuration | Users may receive incorrect permissions or access to the wrong account | Medium | High | High |
| CloudWatch logging failure or missing logs | Loss of investigation capability or incomplete audit trail | Low | Medium | Medium |
| Emergency revoke failure | Unable to revoke access immediately during a security incident | Low | Very High | High |

### Incident Response Plan

To minimize risks when incidents occur, the system should have clear operational runbooks.

| Situation | Immediate Response |
|-----------|--------------------|
| Lambda provisioning failure | Retry or manually trigger the workflow |
| Identity Center API failure | Retry and escalate to AWS Support |
| SES failure | Retry email delivery or use Jira UI approval |
| TTL revoke failure | Perform manual revoke through an operator |
| Security incident | Emergency revoke all active sessions |
| Webhook authentication failure | Reject the request and alert the Security Team |

## Expected Outcomes

- **Standardize and automate the access provisioning process:** The system fully replaces the manual access provisioning workflow with an automated, standardized and end-to-end controllable process. From request submission, approval, access provisioning, to revocation, every step is orchestrated through Jira Service Management, AWS Lambda and IAM Identity Center. This significantly reduces processing time and minimizes human errors.
- **Enhance Production environment security:** By adopting the Just-in-Time Access, Zero Standing Privileges and Group-Based Access models, the system grants access only for the required duration and automatically revokes it upon expiration. This minimizes the risk of long-lived credentials while improving the protection of the Production environment against credential leakage or access abuse.
- **Reduce access provisioning and revocation time:** Compared to the previous Direct Assignment architecture, the new version is expected to reduce provisioning time to under 5 seconds and revoke access within approximately 60 seconds after TTL expiration. This enables engineering teams to gain access more quickly when handling incidents, while allowing the security team to revoke permissions almost instantly when risks are detected.
- **Improve auditability and traceability:** All system activities are recorded through Jira, CloudWatch, CloudTrail and DynamoDB, creating a complete audit trail for events such as request submission, approval, provisioning, expiration and revocation. This serves as a critical foundation for compliance requirements, auditing processes and security incident investigations.
- **Optimize operational costs and maintenance effort:** With a serverless architecture on AWS, the system requires no server management, automatically scales based on demand and incurs costs only according to actual usage. This reduces the operational burden on the DevOps team while maintaining low deployment costs suitable for organizational needs.
- **Improve user experience and approval workflow:** Users can submit access requests through the Jira Portal and receive clear email notifications throughout the request lifecycle. Approvers can also process requests more efficiently via the Jira UI or email approval links, making the workflow more flexible, transparent and convenient for both requesters and approvers.
- **Enable future scalability:** The modular design and Infrastructure as Code approach allow the system to scale easily across multiple AWS accounts, support new access types, or integrate additional approval channels such as Slack without requiring major architectural changes.

In summary, the project is expected to deliver a modern, secure and highly scalable Production access management platform that effectively balances security, processing speed, auditability and operational cost.
