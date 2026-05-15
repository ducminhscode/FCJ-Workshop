---
title : "Workshop"
date :  "`r Sys.Date()`" 
weight : 5
pre: <b> 5. </b>
chapter : false
---

# Production Access Request Portal

### Overview

This project is designed to establish a centralized management system that allows users to request and obtain approval for access to critical resources on the AWS (Amazon Web Services) platform in a secure and controlled manner. Instead of granting permanent permissions, the system focuses on Just-in-Time (JIT) access management, helping enhance security for the organization’s Production environment.

This document provides detailed guidance for the manual setup process of the entire infrastructure on the AWS Console. The main contents include:
- Configuring IAM Identity Center, creating and configuring permission sets, access groups and group assignments.
- Deploying storage-related components such as DynamoDB, Secrets Manager and SES.
- Building and assigning permissions for Lambda functions.
- Configuring API Gateway.
- Integrating these services into a complete operational workflow for the temporary access management system.

The document also presents the deployment sequence, resource naming conventions, prerequisite values and important considerations to ensure the setup process is performed correctly, completely and consistently within the AWS Console environment.

Below is the deployment architecture:

<img src="/images/figure-proposal.png" alt="figure-proposal" style="width:600px !important; max-width:900px !important;">

### Target Users

The system is designed to support multiple user groups involved in managing and controlling access to the AWS production environment:

- **End Users (Developers/Engineers):** Individuals who require temporary access to the production environment to perform tasks such as system deployment, incident troubleshooting, log inspection, or service maintenance.
- **Approvers (Team Leads/Managers):** Responsible for reviewing, approving, or rejecting access requests to ensure permissions are granted to the right individuals for the right purposes.
- **Platform/DevOps Engineers:** Responsible for deploying, configuring, operating and monitoring the entire system infrastructure on AWS, while ensuring all services operate securely and reliably.
- **Security/Compliance Teams:** Monitor access logs, review audit trails and ensure the system complies with the organization’s security, governance and access control policies.

### Content

1. [Introduction](1-introduction/)
2. [Preparation](2-preparation/)
3. [Test Connection](3-test-connection/)
4. [Product Demonstration](4-product-demonstration/)
5. [Clean up resources](5-clean-up-resources/)