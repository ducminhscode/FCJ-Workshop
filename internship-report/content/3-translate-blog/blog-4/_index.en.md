---
title : "Blog 4"
date :  "`r Sys.Date()`" 
weight : 4 
pre: <b> 3.4 </b>
chapter : false
---

# Handling sensitive log data using Amazon CloudWatch


**Original Article:** [Handling sensitive log data using Amazon CloudWatch](https://aws.amazon.com/vi/blogs/mt/handling-sensitive-log-data-using-amazon-cloudwatch/)

**Author:** Jyothi Madanlal and Pratima Singh

**Publication Date:** 30/10/2025

**Source:** AWS Cloud Operations Blog

**Translator:** Tran Nguyen Duc Minh – FCAJ Intern

**Translation Date:** 20/04/2026

**Estimated Reading Time:** 40 minutes

## Summary

The AWS blog explains how to securely handle sensitive data (especially personally identifiable information - PII) in application logs using Amazon CloudWatch.

Logs are essential for debugging, monitoring, and incident response, but they often contain sensitive data such as credit card numbers, emails, or personal details. This creates a trade-off between operational efficiency (fast troubleshooting) and data security/compliance.

To address this, the blog introduces a solution based on two key capabilities:
- Data masking (data protection policies): CloudWatch Logs can automatically detect and mask sensitive data using built-in identifiers (e.g., credit cards, emails) or custom patterns. This ensures that sensitive information is hidden while logs remain usable for debugging.
- Fine-grained access control with IAM: By default, masked data is shown to users. Only authorized users can "unmask" data using specific IAM permissions (logs:Unmask). This enforces least-privilege access and prevents unnecessary exposure.

The blog also describes:
- Using audit and de-identification operations to track and mask sensitive data.
- Implementing privilege escalation workflows for temporary access to raw logs.
- Leveraging CloudTrail auditing to monitor who accessed sensitive data and when.

Conclusion: By combining automated masking with strict access control, organizations can protect sensitive log data without increasing troubleshooting time (MTTR), achieving both security and operational efficiency.

**Target Audience:** Cloud engineers, DevOps engineers, security engineers and system administrators

**Difficulty Level:** Intermediate

**Tags:** Amazon CloudWatch, AWS Security, Log Management, Data Protection, PII, IAM, Observability, Compliance, DevOps, Monitoring

## Table of Contents

- [Handling sensitive log data using Amazon CloudWatch](#handling-sensitive-log-data-using-amazon-cloudwatch)
  - [Summary](#summary)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [What is personally identifiable information (PII)?](#what-is-personally-identifiable-information-pii)
  - [Reference Application](#reference-application)
  - [Scenario](#scenario)
    - [Masking personally identifiable information (PII)](#masking-personally-identifiable-information-pii)
  - [Solution](#solution)
  - [Privilege escalation workflow](#privilege-escalation-workflow)
  - [Conclusion](#conclusion)
  - [Contributions and Feedback](#contributions-and-feedback)

## Introduction

Efficient logging is crucial to building effective investigative and response workflows. Logs, metrics and traces offer critical value when investigating application issues, security events and debugging failures. Structured wide-event logs can provide a means to investigate application behaviour without requiring access to data stores. This level of verbosity in application logs increases the likelihood of security vulnerabilities like sensitive information exposure, thereby introducing friction between security and operational efficiency when analysing application logs during incidents and unexpected behaviour.

This post will help you identify common techniques to secure sensitive information stored in log data without impacting mean time to respond (MTTR) for application issues and security events. In this post, you will learn about effective strategies like data masking and access control capabilities offered by AWS services like [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) and [AWS Identity and Access Management (AWS IAM)](https://aws.amazon.com/vi/iam/) to provide a seamless and audited developer and operational experience while ensuring secure handling of PII in Application and Service Logs.

## What is personally identifiable information (PII)?

As defined in NIST Computer Security Resource Center, [personally identifiable information (PII)](https://csrc.nist.gov/glossary/term/PII) is *"any information about an individual maintained by an agency, including (1) any information that can be used to distinguish or trace an individual's identity, such as name, social security number, date and place of birth, mother's maiden name, or biometric records; and (2) any other information that is linked or linkable to an individual, such as medical, educational, financial, and employment information."*

Modern applications collect user data to personalize experiences and boost retention. Data collection varies by application type, industry, and domain. Features like in-app purchases require sensitive information such as names and credit card details, demanding high availability and low mean time to respond (MTTR) during incidents.

To meet these requirements, developers implement structured logging and cross-correlation of telemetry signals for faster troubleshooting. However, this increases the risk of PII exposure in logs.

Regulated environments mandate PII masking, creating a trade-off: reduced unauthorized exposure risk versus disconnected debugging experiences that increase investigation time and MTTR.

Through the next sections in this post, we will discuss how you can handle sensitive information without impacting MTTR, helping you improve application failure debugging, while continuing to secure PII and meet compliance mandates.

## Reference Application

We launched the [One Observability Demo Workshop](https://observability.workshop.aws/) in August 2020 that utilizes an application called PetAdoptions that is available on [GitHub](https://github.com/aws-samples/one-observability-demo/tree/main/PetAdoptions/petsite/petsite/Controllers). We will use that as the reference application to operate on the log data and showcase the data protection capability. It is built using a Microservice architecture, and different components of the application are deployed on various services, such as [Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/vi/eks/), [Amazon Elastic Container Service (Amazon ECS)](https://aws.amazon.com/ecs), [AWS Lambda](https://aws.amazon.com/vi/lambda/), [Amazon API Gateway](https://aws.amazon.com/vi/api-gateway/), [Amazon DynamoDB](https://aws.amazon.com/vi/dynamodb/), [Amazon Simple Queue Service (Amazon SQS)](https://aws.amazon.com/vi/sqs/), [Amazon Simple Notification Service (Amazon SNS)](https://aws.amazon.com/vi/sns/), and [AWS Step Functions](https://aws.amazon.com/vi/step-functions/). The application architecture is shown in the following diagram.

<img src="/images/figure-1-blog-4.png" alt="figure-1-blog-4" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 1 : Reference Application Architecture Diagram</p>

As illustrated in the diagram, the application is deployed on various services and written using different programming languages, such as Java, C#, Go, Python, and Node.js. The service components collect traces, metrics, and logs, which are then sent to CloudWatch and X-Ray.

## Scenario

The deployed application provides users with the option to make a payment when adopting a pet. The application hence captures credit card information along with other user details. There have been reports of failures in adoptions and the team has added the details of the user, including their credit card details in the logs to be able to quickly identify trends and the root cause.

### Masking personally identifiable information (PII)

Amazon CloudWatch is a fully managed monitoring service customers use to capture observability telemetry from cloud infrastructure and applications. In this pattern, we focus on application logging with CloudWatch Logs. AWS services are integrated with CloudWatch natively and support real-time logging. Regardless of where your application is deployed, you should be able to use the [unified CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) to stream application log data to CloudWatch service.

## Solution

Applications handling PII can contain sensitive information in log data. Data masking is an effective way to secure attributes containing sensitive information while still improving MTTR. This solution describes how customers can use [data protection policies](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/mask-sensitive-log-data.html) with the application logs to maintain data security and privacy and safeguard sensitive information. The main benefit of using the native capability to mask log data is the capability to use AWS Identity and Access Management (IAM) permissions to restrict access to the wider developer teams. With IAM-based controls, you can apply conditional permissions based on attributes like source IP address to restrict access from within the protected network for example VPN endpoints or corporate IP ranges.

CloudWatch data protection policies support preconfigured managed identifiers that customers can use to protect sensitive information like financial information and personal health information. To support bespoke and specific use cases, customers can also configure custom data identifiers to mask sensitive data.

Consider the following log event that contains sensitive information:

```
{
  "PetId": "002",
  "PetType": "puppy",
  "caller": "middlewares.go:60",
  "customer": {
    "ID": 1744785448587828200,
    "FullName": "Selim Zheng",
    "Address": "3333 Piedmont Road NE, Atlanta, GA 30305",
    "CreditCard": "4012000033330026",
    "Email": "selim@zheng.com"
  },
  "err": null,
  "method": "In CompleteAdoption",
  "took": "70.652636ms",
  "traceId": "71d5bd083fbbcbb9",
  "ts": "2025-04-16T06:37:28.587832004Z"
}
```

The data protection policy below is configured to capture occurrences of sensitive information using the **Audit** operation and sends the findings to the FindingDestination without interrupting the collection of log data. The **FindingDestination** can be a CloudWatch Logs group, a Kinesis Data Firehose or an S3 bucket. Customers can choose to mask sensitive information by creating a data protection policy with **Deidentify** type **Operation**.

Customers can use managed identifiers for fields that commonly contain sensitive information like credit card number, email address, date of birth and credentials. Data protection policy to mask such information would look like below:

```
{
  "Name": "data-protection-policy",
  "Description": "",
  "Version": "2021-06-01",
  "Statement": [
    {
      "Sid": "audit-policy",
      "DataIdentifier": [
        "arn:aws:dataprotection::aws:data-identifier/CreditCardNumber",
        "arn:aws:dataprotection::aws:data-identifier/CreditCardSecurityCode"
      ],
      "Operation": {
        "Audit": {
          "FindingsDestination": {
            "CloudWatchLogs": {
              "LogGroup": "dataprotection-log"
            }
          }
        }
      }
    },
    {
      "Sid": "redact-policy",
      "DataIdentifier": [
        "arn:aws:dataprotection::aws:data-identifier/CreditCardNumber",
        "arn:aws:dataprotection::aws:data-identifier/CreditCardSecurityCode"
      ],
      "Operation": {
        "Deidentify": {
          "MaskConfig": {}
        }
      }
    }
  ]
}
```

If no data protection policy is applied, the raw logs capture and present all sensitive information as shown below in Figure 2:

<img src="/images/figure-2-blog-4.png" alt="figure-2-blog-4" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 2: Raw sensitive data visible in logs</p>

As soon as the data protection policy is applied, the sensitive data is automatically masked(figure 3), without interrupting application behaviour.

<img src="/images/figure-3-blog-4.png" alt="figure-3-blog-4" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Figure 3: Sensitive data masked/redacted in logs per data protection policy defined</p>

Anyone who can access the logs in the CloudWatch Logs groups will see redacted information. This capability should be combined with identity controls that restrict log unmasking to users with appropriate privilege escalation. This can be achieved with AWS Identity and Access Management (AWS IAM) policies using the *logs:Unmask* permission. For business as usual activities, apply a deny policy for unmasking operation.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Deny_unmask_sensitive_information",
      "Effect": "Deny",
      "Action": [
        "logs:Unmask"
      ],
      "Resource": "*"
    }
  ]
}
```

The policy shown above denies access to unmask any information from any resource, adding an additional layer of preventative control to secure PII without restricting developer workflow. For a step-by-step walkthrough on how to use the data protection capability at scale, refer to [How Amazon CloudWatch Logs Data Protection can help detect and protect sensitive log data](https://aws.amazon.com/vi/blogs/mt/how-amazon-cloudwatch-logs-data-protection-can-help-detect-and-protect-sensitive-log-data/).

## Privilege escalation workflow

You can refer to sample implementations of privilege escalation workflows per one of the following blogs

- https://aws.amazon.com/vi/blogs/security/managing-temporary-elevated-access-to-your-aws-environment/
- https://aws.amazon.com/vi/blogs/security/temporary-elevated-access-management-with-iam-identity-center/

Once that is in place, you will need to create a new IAM role with a policy which includes an Allow for logs:Unmask permission and allow users to request access to that new IAM role via the privilege escalation workflow.

When a user requires access to the raw logs, the user would go through the privilege escalation workflow and assume a role with the logs:Unmask allow permission. Once the role is assumed, they can use the unmask() function to view the raw data in the logs. An example log insights query using the unmask() function is seen below.

`fields @timestamp, unmask(@message)`<br>
`| sort @timestamp desc`<br>
`| limit 20`

Every time logs data is accessed, a record is created in AWS CloudTrail auditing this action. The audit record captures two actions: If the unmask function was used, and the log record pointer, which can subsequently be used in the [GetLogRecord](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_GetLogRecord.html) call to determine which log record was accessed.

```
{
  "eventVersion": "1.11",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "EXAMPLEPRINCIPALID:Participant",
    "arn": "arn:aws:sts::ACCOUNTID:assumed-role/WSParticipantRole/Participant",
    "accountId": "ACCOUNTID",
    "accessKeyId": "EXAMPLEACCESSKEYID",
    "sessionContext": {
      "sessionIssuer": {
        "type": "Role",
        "principalId": "EXAMPLEPRINCIPALID",
        "arn": "arn:aws:iam::ACCOUNTID:role/WSParticipantRole",
        "accountId": "ACCOUNTID",
        "userName": "WSParticipantRole"
      },
      "attributes": {
        "creationDate": "2025-04-25T19:12:49Z",
        "mfaAuthenticated": "false"
      }
    }
  },
  "eventTime": "2025-04-26T05:11:30Z",
  "eventSource": "logs.amazonaws.com",
  "eventName": "GetLogRecord",
  "awsRegion": "us-east-2",
  "sourceIPAddress": "118.93.208.116",
  "userAgent": "AWS Internal",
  "requestParameters": {
    "logRecordPointer": "CnEKNAogMDc5ODgyNjc0ODY5Oi9lY3MvUGF5Rm9yQWRvcHRpb24QBiIOCJj+8/7mMhCYlYeE5zISNRoYAgaAQZwzAAAAAHVj9ggABoDGpbAAAAYyIAEov8P+g+cyMJDjgoTnMjgbQI0oSIIdUI0WGAAgARAYGAE=",
    "unmask": true,
    "dryRun": false
  },
  "responseElements": null,
  "requestID": "8e32a9f7-f4fa-43bc-b955-4d6556fda100",
  "eventID": "643c50e1-8ac8-473b-937c-50e3f682fbef",
  "readOnly": true,
  "eventType": "AwsApiCall",
  "apiVersion": "20140328",
  "managementEvent": true,
  "recipientAccountId": "ACCOUNTID",
  "eventCategory": "Management",
  "sessionCredentialFromConsole": "true"
}
```

## Conclusion

In this post, you learned how to and why there is a need to secure sensitive information while continuing to operate effectively without impacting mean time to respond. Automated detection and masking capabilities with CloudWatch help reduce the overhead of maintaining a data masking workflow within teams operating on sensitive data. Observing IAM best practices and implementing least privilege access with fine-grained controls can further secure access patterns to protected information.

## Contributions and Feedback

This translation was completed as part of the **FCAJ Internship Program**.

**Contact:** ducmin76@gmail.com

**Feedback:** Any suggestions to improve the translation quality are welcome and can be sent to the email above.

**Updates:** The translation will be updated based on feedback from the community.

*© 2026 – Translation by Tran Nguyen Duc Minh. Please provide proper credit when using this content.*
