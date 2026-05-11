---
title : "Blog 2"
date :  "`r Sys.Date()`" 
weight : 2 
pre: <b> 3.2 </b>
chapter : false
---

# Building multi-tenant SaaS applications with AWS Lambda’s new tenant isolation mode


**Original Article:** [Building multi-tenant SaaS applications with AWS Lambda’s new tenant isolation mode](https://aws.amazon.com/vi/blogs/compute/building-multi-tenant-saas-applications-with-aws-lambdas-new-tenant-isolation-mode/)

**Author:** Anton Aleksandrov (Principal Solutions Architect, Amazon Web Services) & Ayush Kulkarni

**Publication Date:** 20/11/2025

**Source:** AWS Compute Blog

**Translator:** Tran Nguyen Duc Minh – FCAJ Intern

**Translation Date:** 18/03/2026

**Estimated Reading Time:** 30 minutes

## Summary

This article introduces a new feature in AWS Lambda called tenant isolation mode, designed to simplify the development of multi-tenant SaaS applications in serverless architectures. Previously, developers had to implement tenant isolation manually, either by handling it within application logic or by deploying separate Lambda functions for each tenant. Both approaches increased architectural complexity and operational overhead.

With tenant isolation mode, AWS Lambda enables per-tenant execution environment isolation while still allowing multiple tenants to share the same function. Incoming requests are routed using a tenant ID, ensuring that each tenant runs in its own isolated execution environment without sharing data or state with others.

This approach enhances security and simplifies system design, while still benefiting from execution environment reuse (warm starts) at the tenant level. However, it also introduces trade-offs, such as an increase in cold starts and operational costs, since more execution environments may be required.

Overall, tenant isolation mode makes AWS Lambda a more effective solution for building scalable and secure multi-tenant SaaS systems, especially in scenarios where strong isolation between tenants is critical.

**Target Audience:** Cloud engineers, backend developers and solution architects who are building or designing multi-tenant SaaS applications on AWS, especially those working with serverless architectures

**Difficulty Level:** Intermediate

**Tags:** AWS Lambda, Serverless, Multi-tenant SaaS, Tenant Isolation, Cloud Architecture, Security, Scalability

## Table of Contents

- [Building multi-tenant SaaS applications with AWS Lambda’s new tenant isolation mode](#building-multi-tenant-saas-applications-with-aws-lambdas-new-tenant-isolation-mode)
  - [Summary](#summary)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Overview](#overview)
  - [Example scenario](#example-scenario)
  - [Getting Started with Lambda Tenant Isolation Mode](#getting-started-with-lambda-tenant-isolation-mode)
  - [Integrating with Amazon API Gateway](#integrating-with-amazon-api-gateway)
  - [Tenant-aware observability](#tenant-aware-observability)
  - [Considerations](#considerations)
  - [Best practices](#best-practices)
  - [Sample code](#sample-code)
  - [Conclusion](#conclusion)
  - [Contributions and Feedback](#contributions-and-feedback)

## Introduction

Today, AWS announced a new tenant isolation mode for [AWS Lambda](https://aws.amazon.com/vi/lambda/), that allows you to process function invocations in separate execution environments for each application end-user or tenant invoking your Lambda function. This capability simplifies building secure multi-tenant SaaS applications by managing tenant-level compute environment isolation and request routing for you. As a result, you can focus on your core business logic rather than implementing your own tenant-aware compute environment isolation.

## Overview

Lambda runs your function code in secure execution environments that leverage [Firecracker virtualization](https://firecracker-microvm.github.io/) to provide isolation. These execution environments never share or reuse virtual resources (such as vCPU, disk, or memory) across functions, or even across different versions of the same function. However, Lambda can reuse execution environments for multiple invocations of the same function version, as these execution environments are fully set-up and can therefore deliver faster request processing for your functions.

<img src="/images/figure-1-blog-2.png" alt="figure-1-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 1. Incoming invocations processed by a collection of execution environments that belong to a single function.</p>

Multi-tenant SaaS applications that handle sensitive tenant-specific data or execute code supplied dynamically by tenants may need a higher degree of isolation-at the individual application tenant level rather than at the function level-for secure code execution and to reduce the risk of cross-tenant data access.

Prior to today’s launch, developers would implement custom solutions, such as SDKs or application logic to manage isolation within function code. This approach was bug-prone, required more work from application development teams and didn’t ensure isolation at the compute environment level.

Alternatively, developers adopted the approach of creating separate functions per application tenant, replicating the same code across hundreds or thousands of tenants. This approach provided stronger compute environment isolation than sharing compute environments across multiple tenants of the same function, but increased implementation overhead and operational complexity as workloads grew to support a larger number of tenants over time.

<img src="/images/figure-2-blog-2.png" alt="figure-2-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 2. Using function-per-tenant model, each tenant’s requests are processed by a separate function.</p>

Starting today, AWS Lambda offers a new tenant isolation mode that lets you isolate execution environments used across different tenants of your multi-tenant SaaS applications, even when all of the tenants invoke the same function. When you enable the new tenant isolation mode, you include a tenant identifier with each function invocation. Lambda uses this identifier to route the request to the correct execution environment. As a result, each execution environment is reused only for invocations from the same tenant. This means you still get the performance benefits of warm execution environments, while ensuring that each tenant’s workloads remain isolated.

<img src="/images/figure-3-blog-2.png" alt="figure-3-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 3. With the new tenant isolation capability, Lambda creates separate execution environments per tenant for a single function.</p>

For organizations handling sensitive tenant-specific data or running untrusted code supplied dynamically by end-users, Lambda’s new tenant isolation mode provides the security benefits of per-tenant compute environment separation without the operational complexity of managing individual functions or infrastructure for each tenant.

## Example scenario

Consider building a multi-tenant serverless SaaS application. To optimize performance, your function handler can retrieve tenant-specific configuration and data, cache it in memory and reuse it for subsequent invocations from the same tenant. For example, you might cache tenant-specific database location, feature flags, or business rules that are frequently accessed during request processing. You may store this information within the application runtime process as global variables or as files in the `/tmp` directory. However, if the underlying execution environment is used to serve multiple tenants, this approach can potentially expose data across tenants.

With tenant isolation mode you can address this risk with much simpler architecture and configuration. This built-in capability makes Lambda an excellent choice for multi-tenant SaaS applications needing isolated compute environments for individual tenants.

## Getting Started with Lambda Tenant Isolation Mode

Use the new **tenancy-config** parameter to configure tenant isolation mode when you create your function. You can only apply this configuration at function creation time; it cannot be updated for existing functions. The following snippet creates a function with tenancy config using the [AWS CLI](https://aws.amazon.com/cli/).

```
aws lambda create-function \
   --function-name my-function1 \
   --runtime nodejs22.x \
   --zip-file fileb://my-function1.zip \
   --handler index.handler \
   --role arn:aws:iam:1234567890:role/my-function-role \
   --tenancy-config '{"TenantIsolationMode": "PER_TENANT"}'
```

After the function is created, you must provide the tenant ID parameter with each invocation. Lambda uses this identifier to ensure that the execution environment used for a particular tenant is never reused for other tenants. For subsequent invocations from the same tenant, Lambda may reuse the execution environment to optimize performance. Specify this **tenant-id** parameter as illustrated below:

```
aws lambda invoke \
   --function-name my-function \
   --tenant-id BlueTenant \
   response.json
```

The new **tenant-id** parameter is required for functions using the tenant isolation mode. Function invocations omitting this parameter will fail with an invocation error, as shown below:

```
aws lambda invoke --function-name multitenant-function out.json

An error occurred (InvalidParameterValueException) when calling the Invoke operation:
The invoked function is enabled with tenancy configuration. 
Add a valid tenant ID in your request and try again.
```

Lambda makes the tenant ID parameter available through your function handler’s context object. This allows you to access tenant-specific information in your code, for example if you wish to implement custom logic based on the tenant identity, as shown below:

```
exports.handler = async function (event, context) {
   const tenantId = context.tenantId;

   // Process tenant-specific logic

   return {
      statusCode: 200,
      body: `OK for tenantId=${tenantId}`
   };
};
```

The following table outlines differences between Lambda functions with and without tenant isolation mode enabled:

| Feature | Without the new tenant isolation mode | With the new tenant isolation mode |
|---------|---------------------------------------|------------------------------------|
| Execution environment isolation | Isolated per function version. | Isolated per end-user or tenant invoking a function version. |
| Execution environment reuse | Can be reused to process all invocations of a function version. | Can only be reused to process invocations from the same tenant invoking a function version. |
| Data stored on local disk and in-memory | Potentially accessible across all invocations of a function version. | Potentially accessible across invocations from the same tenant. Not accessible for invocations from other tenants. |
| Cold starts | Occur when there are no warm execution environments available to process incoming invocation. | Occur when there are no tenant-specific warm execution environments available to process incoming invocation. More cold starts expected due to tenant-specific execution environments. |

## Integrating with Amazon API Gateway

[Amazon API Gateway](https://aws.amazon.com/vi/api-gateway/) uses [Lambda’s Invoke API](https://docs.aws.amazon.com/lambda/latest/api/API_Invoke.html) to invoke Lambda functions. When using the Invoke API, Lambda expects the tenant ID parameter to be passed using the **X-Amz-Tenant-Id** HTTP header. You can configure API Gateway to inject this HTTP header into the Lambda invocation request with a value obtained from client request properties such as HTTP header, query parameter, or path parameter. When using [Lambda Authorizers](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html), you can obtain the value from authorization context information returned by the authorizer, such as principal ID or JWT claim. See [API Gateway documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-lambda-authorizer-output.html) to learn how you can return authorization information from Lambda authorizers to be used for the **X-Amz-Tenant-Id** header value.

<img src="/images/figure-4-blog-2.png" alt="figure-4-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 4. Obtaining X-Amz-Tenant-Id header value from authentication sources.</p>

The following screenshot illustrates API Gateway Lambda integration configuration, where the incoming request to API Gateway includes an **x-tenant-id** header that is mapped to the **X-Amz-Tenant-Id** request header to invoke a Lambda function using tenant isolation mode.

<img src="/images/figure-5-blog-2.png" alt="figure-5-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 5. Mapping client request header to Lambda tenant-id header.</p>

The following code snippet illustrates this configuration implemented with the AWS CDK.

```
const lambdaIntegration = new ApiGw.LambdaIntegration(fn, {
   requestParameters: {
      // This configures API Gateway to inject X-Amz-Tenant-Id header
      // into downstream requests. The header value is obtained from 
      // x-tenant-id header in the client request.
      'integration.request.header.X-Amz-Tenant-Id': 'method.request.header.x-tenant-id'
   }
});

resource.addMethod('GET', lambdaIntegration, {
   requestParameters: {
      // This enables API Gateway to use the x-tenant-id header value 
      // obtained from the client request. The header name is arbitrary.
      // you can use any other header name. 
      'method.request.header.x-tenant-id': true
   }
});
```

## Tenant-aware observability

For functions using tenant isolation, Lambda automatically includes the tenant ID in [function logs](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-logs.html) when you have [JSON logging enabled](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-cloudwatchlogs-logformat.html), making it easier to monitor and debug tenant-specific issues. Note that the **tenantId** property is available during function invocation, rather than during function initialization. The tenantId property is included for both [platform events](https://docs.aws.amazon.com/lambda/latest/dg/telemetry-schema-reference.html) (like **platform.start** and **platform.report**) and custom logs you print in your function code, as shown in the following screenshot:

<img src="/images/figure-6-blog-2.png" alt="figure-6-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 6. Lambda function logs with tenantId.</p>

Lambda creates a separate [CloudWatch log stream](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) for each execution environment. You can use [CloudWatch Log Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html) to find log streams that belong to a particular tenant by filtering by tenant Id:

```
fields @logStream, @message
| filter tenantId=='BlueTenant' or record.tenantId=='BlueTenant'
| stats count() as logCount by @logStream
| sort @timestamp desc
```

You can also retrieve tenant-specific logs across all log streams:

```
fields @message
| filter tenantId=='BlueTenant' or record.tenantId=='BlueTenant'
| limit 1000
```

Each log stream starts with function initialization logs followed by the invocation logs. This structure helps you to debug tenant-specific issues and understand the lifecycle of each tenant’s execution environments.

## Considerations

When using the new tenant isolation for Lambda functions, consider the following:
- Each tenant’s execution environments are isolated from other tenants so that tenant-specific data stored on disk or in memory remain separated from other tenants invoking the same Lambda function.
- All tenants share the function’s execution role. For more fine-grained permissions for individual tenants, consider propagating tenant-scoped credentials from the upstream application components invoking your Lambda function.
- Your application may experience higher percentage of cold starts, as Lambda processes requests in separate execution environments for each tenant invoking your functions.
- You pay a fee for each new tenant-specific execution environment created, depending on the memory configured for your function. See [Lambda pricing page](https://aws.amazon.com/vi/lambda/pricing/) for details.

## Best practices

When using the new tenant isolation mode for Lambda functions, AWS recommends the following best practices:
- Implement robust tenant ID validation at the application layer to prevent unauthorized access through tenant ID manipulation. Consider using a dedicated service or database to maintain valid tenant IDs.
- Monitor and audit tenant access patterns regularly to detect potential security anomalies or unauthorized cross-tenant access attempts.
- Be aware of [Lambda concurrency quotas](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) when building multi-tenant applications. You might need to request quota increases based on your tenant count and usage patterns.

## Sample code

Follow the instructions in [this GitHub repository](https://github.com/aws-samples/sample-lambda-tenant-isolation) to provision a sample project in your own account and see the new Lambda tenant isolation mode in action. The sample project illustrates how to integrate a function using the new tenant isolation mode with [Amazon API Gateway](https://aws.amazon.com/api-gateway/) and propagate tenant identity from client requests.

## Conclusion

The new tenant isolation mode for Lambda simplifies building serverless multi-tenant SaaS applications on AWS. By automatically managing application tenant-level compute environment isolation, this capability eliminates the need for custom isolation logic or separate tenant functions, allowing you to focus on the core business logic while AWS handles the complexities of tenant-aware compute environment isolation.

Combined with the existing security features in Lambda, rapid scaling and pay-per-use pricing, tenant isolation mode makes Lambda an even more compelling choice for modern SaaS applications, whether you’re building new solutions or enhancing existing ones.

To learn more, refer to the [documentation for tenant isolation](https://docs.aws.amazon.com/lambda/latest/dg/tenant-isolation.html). For details on pricing, refer to [Lambda’s pricing page](https://aws.amazon.com/vi/lambda/pricing/).

## Contributions and Feedback

This translation was completed as part of the **FCAJ Internship Program**.

**Contact:** ducmin76@gmail.com

**Feedback:** Any suggestions to improve the translation quality are welcome and can be sent to the email above.

**Updates:** The translation will be updated based on feedback from the community.

*© 2026 – Translation by Tran Nguyen Duc Minh. Please provide proper credit when using this content.*