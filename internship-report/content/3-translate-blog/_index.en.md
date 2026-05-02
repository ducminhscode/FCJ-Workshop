---
title : "Translated Blogs"
date :  "`r Sys.Date()`" 
weight : 3 
pre: <b> 3. </b>
chapter : false
---

**Blog 1:** [Blog 1 - Building an AI-powered defense-in-depth security architecture for serverless microservices](./blog-1)

This article presents an approach to building a defense-in-depth security architecture for serverless microservices applications on AWS, integrating AI/ML to protect against modern threats. As the attack surface expands with numerous APIs and functional components, the architecture establishes seven layers of protection from the outside in-ranging from blocking malicious traffic, authenticating users, controlling API access, and isolating networks, to securing the compute environment, managing credentials, protecting data, and continuously monitoring the system with AI-driven threat detection and response.

**Blog 2:** [Blog 2 - Building multi-tenant SaaS applications with AWS Lambda’s new tenant isolation mode](./blog-2)

The blog introduces a new **tenant isolation mode** for **AWS Lambda**, designed to help build more secure multi-tenant SaaS applications. This feature ensures that each tenant runs in its own execution environment and does not share it with other tenants, reducing the risk of data leakage. At the same time, the environment can still be reused for the same tenant to help minimize cold start latency.

**Blog 3:** [Blog 3 - Architecting for agentic AI development on AWS](./blog-3)

This blog explains how to design both system architecture and code base structure to support **agentic AI development** on AWS. It focuses on reducing friction through fast feedback mechanisms such as local emulation, hybrid testing, and preview environments. In addition, it highlights code organization principles like domain-driven design, layered testing, monorepos, and machine-readable documentation to help AI agents understand, modify, and validate systems effectively. The goal is to create an environment where AI agents can iteratively develop, test, and refine code safely and efficiently.

**Blog 4:** [Blog 4 - Handling sensitive log data using Amazon CloudWatch](./blog-4)

The article focuses on how to protect sensitive data (especially PII) in application logs when using Amazon CloudWatch, while still maintaining effective debugging and operational efficiency. The main content revolves around using **data protection policies** to automatically detect and mask sensitive information such as emails and credit card numbers, as well as applying fine-grained access control with AWS Identity and Access Management to restrict who can view raw (unmasked) data. In addition, the article discusses combining Audit and Deidentify operations to monitor and protect data, building a privilege escalation workflow for temporary access to full logs when necessary, and using AWS CloudTrail to audit all access activities. This solution helps balance security, compliance, and incident response efficiency (MTTR) in modern systems.