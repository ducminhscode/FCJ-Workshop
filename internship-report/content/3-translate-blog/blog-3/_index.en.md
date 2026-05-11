---
title : "Blog 3"
date :  "`r Sys.Date()`" 
weight : 3 
pre: <b> 3.3 </b>
chapter : false
---

# Architecting for agentic AI development on AWS


**Original Article:** [Architecting for agentic AI development on AWS](https://aws.amazon.com/vi/blogs/architecture/architecting-for-agentic-ai-development-on-aws/)

**Author:** Alan Oberto Jimenez (Application Architect)

**Publication Date:** 26/03/2026

**Source:** AWS Architecture Blog

**Translator:** Tran Nguyen Duc Minh – FCAJ Intern

**Translation Date:** 10/04/2026

**Estimated Reading Time:** 35 minutes

## Summary

This blog explains how to design systems for building agentic AI on AWS—AI systems that can plan, take actions and iteratively interact with their environment, rather than simply responding to a single prompt.

It highlights the shift from traditional "request–response" AI to loop-based architectures, where an agent can:
- Understand a goal
- Break it into steps
- Call external tools or APIs
- Evaluate results and adjust its actions

The post introduces a reference architecture on AWS that typically includes:
- An LLM as the core decision-making engine
- An orchestration layer to manage workflows and state
- Tool integrations such as APIs, databases and services
- Memory components (short-term and long-term)
- Monitoring and guardrails to control behavior
  
A key emphasis is on modular system design, allowing teams to scale, swap models and optimize costs more easily. The blog also outlines how AWS services can be used to support orchestration, storage and compute for flexible agent-based systems.

It also discusses real-world challenges, including:
- Managing state and context across multiple steps
- Ensuring reliability of agent decisions
- Controlling costs from iterative loops
- Implementing guardrails to prevent unintended behavior

Overall, the blog provides guidance on building production-ready AI agents, focusing on scalability, observability and control rather than just prototypes.

**Target Audience:** AI/ML developers and software engineers, cloud and solution architects, product builders working with AI agents, readers with basic knowledge of LLMs or AWS

**Difficulty Level:** Intermediate to Advanced

**Tags:** AI Agents, Agentic AI, AWS Architecture, LLM, Cloud Design, Distributed Systems, AI Engineering

## Table of Contents

- [Architecting for agentic AI development on AWS](#architecting-for-agentic-ai-development-on-aws)
  - [Summary](#summary)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Why traditional architectures hinder agentic AI](#why-traditional-architectures-hinder-agentic-ai)
  - [System architecture for fast agentic feedback loops](#system-architecture-for-fast-agentic-feedback-loops)
    - [Local emulation as the default feedback path](#local-emulation-as-the-default-feedback-path)
    - [Offline development for data and analytics workloads](#offline-development-for-data-and-analytics-workloads)
    - [Hybrid testing with lightweight cloud resources](#hybrid-testing-with-lightweight-cloud-resources)
    - [Preview environments and contract-first design](#preview-environments-and-contract-first-design)
  - [Code base architecture for AI-friendly development](#code-base-architecture-for-ai-friendly-development)
    - [Domain-driven structure with explicit boundaries](#domain-driven-structure-with-explicit-boundaries)
    - [Encoding architectural intent with project rules](#encoding-architectural-intent-with-project-rules)
    - [Tests as executable specifications](#tests-as-executable-specifications)
    - [Monorepos and machine-readable documentation](#monorepos-and-machine-readable-documentation)
    - [Integrating agents safely into delivery pipelines](#integrating-agents-safely-into-delivery-pipelines)
  - [Conclusion](#conclusion)
  - [Contributions and Feedback](#contributions-and-feedback)

## Introduction

If you’re architecting cloud systems for AI development on AWS, you’ve likely discovered that traditional architectures create friction for AI agents. Many cloud teams are experimenting with AI coding assistants but quickly discover a gap between what these tools promise and what their architectures allow. When an AI agent generates code, it often takes minutes-or hours-before you can validate whether that change actually works. Slow deployment cycles, tightly coupled services and opaque code bases turn every iteration into a high-friction exercise. As a result, AI agents struggle to operate autonomously and developers are forced back into manual validation loops.

This article is written for cloud architects who want to remove that friction. It focuses on agentic development, a model where an AI agent does more than suggest snippets-it writes, tests, deploys and refines code through rapid feedback cycles. To make that possible, both your system architecture and your code base architecture must be designed to support fast validation, safe iteration and clear intent.

In this post, we demonstrate how to architect AWS systems that enable AI agents to iterate rapidly through design patterns for both system architecture and code base structure. We first examine the architectural problems that limit agentic development today. We then walk through system architecture patterns that support rapid experimentation, followed by codebase patterns that help AI agents understand, modify and validate your applications with confidence.

## Why traditional architectures hinder agentic AI

Most cloud architectures were designed for human-driven development. They assume long-lived environments, manual testing and infrequent deployments. In an agentic workflow, those assumptions break down.

AI agents must validate changes continuously. When every test requires provisioning cloud resources, waiting for pipelines, or debugging deployment-only failures, feedback loops become too slow. Tight coupling between business logic and cloud services further complicates local testing, while inconsistent project structures make it difficult for an agent to understand where changes belong.

Without architectural support, agentic AI produces more risk than value. The solution is not better prompts, it’s an architecture that treats fast feedback and clear boundaries as first-class concerns. This architectural friction isn’t only inconvenient, it fundamentally limits AI agent effectiveness. Here’s how to redesign your architecture to help unlock the potential of agentic AI.

## System architecture for fast agentic feedback loops

Agentic development depends on feedback speed. The faster an agent can observe the impact of a change, the more effectively it can refine its output. System architecture plays a decisive role here.

<img src="/images/figure-1-blog-3.jpeg" alt="figure-1-blog-3" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 1: High-level architecture enabling agentic development: local test loops, ephemeral test stack and continuous integration and continuous delivery (CI/CD) pipeline triggered by AI</p>

### Local emulation as the default feedback path

Whenever possible, your architecture should allow AI agents to test changes locally before touching cloud resources. AWS provides several tools that make this practical.

For example, serverless applications built with [AWS Lambda](https://aws.amazon.com/vi/lambda/) and [Amazon API Gateway](https://aws.amazon.com/vi/api-gateway/) can be emulated locally using the [AWS Serverless Application Model](https://aws.amazon.com/vi/serverless/sam/) (AWS SAM). With the `sam local start-api` command, an AI agent can invoke Lambda functions through a locally emulated API Gateway, observe responses immediately and iterate in seconds rather than minutes.

Containers offer similar benefits for services that run on [Amazon Elastic Container Service](https://aws.amazon.com/vi/ecs/) (Amazon ECS) or [AWS Fargate](https://aws.amazon.com/vi/fargate/). By building and running the same container images locally, an agent can validate application behavior before deploying to the cloud. For data persistence, [Amazon DynamoDB](https://aws.amazon.com/vi/dynamodb/) Local allows the agent to test create, read, update and delete (CRUD) operations against a local database that mirrors the DynamoDB API.

Note: Local emulation reduces iteration time, allowing AI-generated code to be validated in seconds and potentially reducing the cost and risk of experimentation.

### Offline development for data and analytics workloads

Many workloads fit neatly into request-response testing, but data processing pipelines often involve large datasets and distributed execution. Even here, agentic workflows benefit from local feedback.

[AWS Glue](https://aws.amazon.com/vi/glue/) provides Docker images that allow AWS Glue jobs to run locally with the AWS Glue ETL libraries. An AI agent can validate transformations against sample datasets, inspect intermediate results and only move to the cloud for scale testing. The same pattern applies to other data and machine learning (ML) workloads: isolate logic, test locally with reduced data and promote validated code to managed services later.

**Note:** Offline development shortens feedback loops for data workloads and reduces unnecessary cloud runs during early iteration.

### Hybrid testing with lightweight cloud resources

Some AWS services cannot be fully emulated locally. In these cases, the goal is not to avoid the cloud, but to keep cloud feedback lightweight.

For event-driven systems using [Amazon Simple Notification Service](https://aws.amazon.com/vi/sns/) (Amazon SNS) or [Amazon Simple Queue Service](https://aws.amazon.com/vi/sqs/) (Amazon SQS), you can define minimal development stacks using infrastructure as code (IaC) tools such as [AWS CloudFormation](https://aws.amazon.com/vi/cloudformation/) or the [AWS Cloud Development Kit](https://aws.amazon.com/vi/cdk/) (AWS CDK). An AI agent can deploy small, isolated resources, invoke them through the AWS SDK and validate behavior without provisioning full environments.

This hybrid approach treats the cloud as another test dependency-used sparingly and predictably.

Note: Hybrid testing confirms real service behavior early while keeping cloud usage focused and controlled.

### Preview environments and contract-first design

Fast feedback does not stop at local testing. End-to-end validation still matters, especially when multiple services interact.

Preview environments are short-lived stacks deployed on demand for validation. Defined through IaC, they allow an AI agent to deploy a complete application, run smoke tests and tear everything down when finished. When combined with contract-first design-where APIs are defined upfront using OpenAPI specifications-agents can validate integrations even before all services are implemented.

**Note**: Preview environments can reduce integration risk and allow AI-generated changes to be validated safely before reaching production.

## Code base architecture for AI-friendly development

System architecture accelerates feedback, but code base architecture determines whether an AI agent can make sense of what it is changing.

### Domain-driven structure with explicit boundaries

We recommend agentic development when your repository reflects clear architectural intent. A domain-driven structure inspired by [Domain-Driven Design](https://docs.aws.amazon.com/prescriptive-guidance/latest/hexagonal-architectures/overview.html#ddd) (DDD) separates core business logic from application orchestration and infrastructure concerns.

Trong thực tế, điều này thường có nghĩa là tổ chức code thành các layer rõ ràng như /domain, /application và /infrastructure, nơi mỗi phần đảm nhận một vai trò riêng biệt. Domain layer tập trung vào business logic cốt lõi và hoàn toàn không phụ thuộc vào các dịch vụ Amazon, giúp logic nghiệp vụ luôn thuần và dễ kiểm thử. Trong khi đó, infrastructure layer chịu trách nhiệm tích hợp với các dịch vụ như Amazon DynamoDB hoặc Amazon Simple Notification Service. Nhờ sự tách biệt này, AI agents có thể chỉnh sửa và kiểm tra business logic ngay trên môi trường local mà không cần chạm tới các thành phần phụ thuộc cloud, từ đó tăng tốc độ iteration và giảm rủi ro khi thay đổi.

Patterns like [hexagonal architecture](https://docs.aws.amazon.com/prescriptive-guidance/latest/hexagonal-architectures/welcome.html) reinforce this separation by treating external systems as adapters rather than dependencies.

Note: Clear boundaries can reduce unintended side effects and make AI-generated changes more straightforward to reason about and test.

### Encoding architectural intent with project rules

Even well-structured repositories benefit from explicit guidance. [Kiro](https://kiro.dev/) supports [steering files](https://kiro.dev/docs/cli/steering/)-Markdown files stored under `.kiro/steering/`-that describe [architectural constraints and coding conventions](https://kiro.dev/docs/cli/steering/#foundational-steering-files).

For example, a rule might state that database access must go through repository classes in the infrastructure layer. The agent consults these rules automatically, reducing the need to restate constraints in every prompt and helping to keep generated code aligned with your architecture.

**Note:** Project rules reduce architectural drift and help maintain consistency as AI agents operate more autonomously.

### Tests as executable specifications

In agentic workflows, tests do more than catch regressions, they define acceptable behavior. A layered testing strategy works particularly well:
- **Unit** tests validate domain logic in isolation and run quickly, making them ideal for frequent AI-driven iterations.
- **Contract** tests verify that services honor agreed interfaces, catching breaking changes early.
- **Smoke** tests run against deployed environments to surface configuration or permission issues that only appear at runtime, such as missing [AWS Identity and Access Management](https://aws.amazon.com/vi/iam/) (IAM) permissions.
Well-written tests also act as documentation. When a test fails, the agent can infer what behavior is expected and refine its changes accordingly.

**Note:** Tests provide fast, objective validation of AI-generated code and reduce the risk of subtle integration failures.

### Monorepos and machine-readable documentation

AI agents work more effectively when they have broad context. A monorepo allows the agent to navigate across services, understand shared patterns and evaluate the impact of changes system-wide. Within that repository, concise and structured documentation is essential. Files such as AGENT.md can explain architectural principles and constraints, while RUNBOOK.md and CONTRIBUTING.md describe operational and development workflows. Machine-readable formats, such as YAML or configuration files, are more straightforward for agents to interpret than lengthy prose.

Kiro can use [foundational steering documents](https://kiro.dev/docs/cli/steering/#foundational-steering-files)-summaries of structure, technology and product guidelines—to help the agent maintain situational awareness as the project evolves.

**Note:** Shared context improves the quality of AI-generated changes and reduces the need for manual correction.

### Integrating agents safely into delivery pipelines

As AI agents become more capable, governance remains essential. Continuous integration and continuous deliver (CI/CD) pipelines should include guardrails such as required test execution, automated reviews and branch protections. Over time, as confidence grows, you can expand the agent’s autonomy while keeping humans in the loop for high-impact decisions. This balance allows AI to accelerate routine work without increasing operational risk.

## Conclusion

Agentic AI development does not succeed by accident. It requires architectures that prioritize fast feedback, clear boundaries and explicit intent. Combining local emulation, lightweight cloud testing and preview environments with domain-driven structure, layered testing and machine-readable documentation creates an environment where AI agents can operate effectively and safely. Tools like Kiro help bridge the gap between human design decisions and autonomous AI execution. When architecture aligns with agentic workflows, AI agents become true force multipliers, handling iterative development at speed while your team focuses on higher-level design and innovation.

To learn more about how AWS can help your organization implement agentic solutions, visit [AWS Agentic AI](https://aws.amazon.com/vi/ai/agentic-ai/).

## Contributions and Feedback

This translation was completed as part of the **FCAJ Internship Program**.

**Contact:** ducmin76@gmail.com

**Feedback:** Any suggestions to improve the translation quality are welcome and can be sent to the email above.

**Updates:** The translation will be updated based on feedback from the community.

*© 2026 – Translation by Tran Nguyen Duc Minh. Please provide proper credit when using this content.*
