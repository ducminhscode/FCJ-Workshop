---
title : "Blog 4"
date :  "`r Sys.Date()`" 
weight : 4
pre: <b> 3.4 </b>
chapter : false
---

# Xử lý dữ liệu log nhạy cảm bằng Amazon CloudWatch

**Bài viết gốc:** [Xử lý dữ liệu log nhạy cảm bằng Amazon CloudWatch](https://aws.amazon.com/vi/blogs/mt/handling-sensitive-log-data-using-amazon-cloudwatch/)

**Tác giả:** Jyothi Madanlal và Pratima Singh

**Ngày xuất bản:** 30/10/2025

**Nguồn:** AWS Cloud Operations Blog

**Người dịch:** Trần Nguyễn Đức Minh - FCAJ Intern

**Ngày dịch:** 20/04/2026

**Thời gian đọc:** 40 phút

## Tóm tắt

Bài blog của AWS trình bày cách xử lý dữ liệu nhạy cảm (đặc biệt là PII - thông tin định danh cá nhân) trong log ứng dụng bằng Amazon CloudWatch.

Log rất quan trọng cho việc debug, giám sát và xử lý sự cố, nhưng thường chứa dữ liệu nhạy cảm như số thẻ tín dụng, email hoặc thông tin cá nhân. Điều này tạo ra sự đánh đổi giữa hiệu quả vận hành (debug nhanh) và bảo mật/tuân thủ.

Giải pháp được đề xuất dựa trên hai cơ chế chính:
- Masking dữ liệu (data protection policies): CloudWatch Logs có thể tự động phát hiện và che (mask) dữ liệu nhạy cảm bằng các mẫu có sẵn (ví dụ: thẻ tín dụng, email) hoặc định nghĩa custom. Điều này giúp bảo vệ dữ liệu nhưng vẫn giữ log hữu ích cho việc phân tích.
- Kiểm soát truy cập chi tiết với IAM: Mặc định người dùng chỉ thấy dữ liệu đã bị che. Chỉ những người có quyền đặc biệt mới có thể "unmask" thông qua permission logs:Unmask. Điều này đảm bảo nguyên tắc least privilege và hạn chế lộ dữ liệu.

Ngoài ra, blog còn đề cập: 
- Sử dụng Audit và Deidentify để phát hiện và che dữ liệu nhạy cảm.
- Xây dựng quy trình cấp quyền tạm thời (privilege escalation) để truy cập log gốc.
- Dùng CloudTrail để audit việc truy cập dữ liệu nhạy cảm.

Kết luận: Kết hợp giữa masking tự động và kiểm soát truy cập giúp doanh nghiệp bảo vệ dữ liệu nhạy cảm trong log mà không làm tăng thời gian xử lý sự cố (MTTR), cân bằng giữa bảo mật và hiệu quả vận hành.

**Đối tượng đọc:** Cloud engineers, DevOps engineers, security engineers và system administrators

**Độ khó:** Intermediate

**Tags:** Amazon CloudWatch, AWS Security, Log Management, Data Protection, PII, IAM, Observability, Compliance, DevOps, Monitoring

## Mục lục

- [Xử lý dữ liệu log nhạy cảm bằng Amazon CloudWatch](#xử-lý-dữ-liệu-log-nhạy-cảm-bằng-amazon-cloudwatch)
  - [Tóm tắt](#tóm-tắt)
  - [Mục lục](#mục-lục)
  - [Giới thiệu](#giới-thiệu)
  - [Thông tin nhận dạng cá nhân (PII) là gì?](#thông-tin-nhận-dạng-cá-nhân-pii-là-gì)
  - [Ứng dụng tham chiếu](#ứng-dụng-tham-chiếu)
  - [Kịch  bản](#kịch--bản)
    - [Ẩn thông tin nhận dạng cá nhân (PII)](#ẩn-thông-tin-nhận-dạng-cá-nhân-pii)
  - [Giải pháp](#giải-pháp)
  - [Quy trình nâng cấp đặc quyền](#quy-trình-nâng-cấp-đặc-quyền)
  - [Kết luận](#kết-luận)
  - [Đóng góp và Feedback](#đóng-góp-và-feedback)

## Giới thiệu

Ghi chép hiệu quả rất quan trọng để xây dựng các quy trình điều tra và phản ứng hiệu quả. Nhật ký, số liệu và dấu vết cung cấp giá trị quan trọng khi điều tra các sự cố ứng dụng, sự kiện bảo mật và gỡ lỗi lỗi. Nhật ký sự kiện rộng có cấu trúc có thể cung cấp một phương tiện để điều tra hành vi ứng dụng mà không cần truy cập vào kho dữ liệu. Mức độ chi tiết này trong nhật ký ứng dụng tăng khả năng xảy ra các lỗ hổng bảo mật như tiết lộ thông tin nhạy cảm, do đó tạo ra sự cản trở giữa bảo mật và hiệu quả vận hành khi phân tích nhật ký ứng dụng trong các sự cố và hành vi bất thường.

Bài viết này sẽ giúp bạn xác định các kỹ thuật phổ biến để bảo vệ thông tin nhạy cảm được lưu trữ trong dữ liệu nhật ký mà không ảnh hưởng đến thời gian phản hồi trung bình (MTTR) đối với các sự cố ứng dụng và sự kiện bảo mật. Trong bài viết này, bạn sẽ tìm hiểu về các chiến lược hiệu quả như che dấu dữ liệu và khả năng kiểm soát truy cập do các dịch vụ AWS cung cấp, chẳng hạn như [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) và [AWS Identity and Access Management (AWS IAM)](https://aws.amazon.com/vi/iam/), nhằm mang lại trải nghiệm phát triển và vận hành liền mạch, có kiểm toán, đồng thời đảm bảo xử lý an toàn các thông tin nhận dạng cá nhân (PII) trong nhật ký ứng dụng và dịch vụ.

## Thông tin nhận dạng cá nhân (PII) là gì?

Theo định nghĩa tại Trung tâm Tài nguyên An ninh Máy tính NIST, [thông tin nhận dạng cá nhân (PII)](https://csrc.nist.gov/glossary/term/PII) là *"bất kỳ thông tin nào về một cá nhân được quản lý bởi một cơ quan, bao gồm (1) bất kỳ thông tin nào có thể sử dụng để phân biệt hoặc truy vết danh tính của cá nhân đó, chẳng hạn như tên, số an sinh xã hội, ngày và nơi sinh, tên con gái của mẹ, hoặc hồ sơ sinh trắc học; và (2) bất kỳ thông tin nào khác có liên kết hoặc có thể liên kết với cá nhân đó, chẳng hạn như thông tin y tế, giáo dục, tài chính và việc làm."*

Các ứng dụng hiện đại thu thập dữ liệu người dùng để cá nhân hóa trải nghiệm và tăng cường khả năng giữ chân người dùng. Việc thu thập dữ liệu thay đổi tùy theo loại ứng dụng, ngành nghề và lĩnh vực. Các tính năng như mua trong ứng dụng đòi hỏi thông tin nhạy cảm như tên và chi tiết thẻ tín dụng, yêu cầu độ khả dụng cao và thời gian trung bình để phản hồi (MTTR) thấp khi xảy ra sự cố.

Để đáp ứng các yêu cầu này, các nhà phát triển triển khai ghi chép có cấu trúc và đối chiếu chéo các tín hiệu thu thập được để xử lý sự cố nhanh hơn. Tuy nhiên, điều này làm tăng nguy cơ lộ thông tin PII trong nhật ký.

Các môi trường được quản lý yêu cầu che giấu PII, tạo ra một sự đánh đổi: giảm rủi ro tiếp xúc trái phép đổi lấy trải nghiệm gỡ lỗi bị gián đoạn, làm tăng thời gian điều tra và MTTR.

Trong các phần tiếp theo của bài viết này, chúng tôi sẽ thảo luận cách bạn có thể xử lý thông tin nhạy cảm mà không ảnh hưởng đến MTTR, giúp cải thiện việc gỡ lỗi sự cố ứng dụng, đồng thời tiếp tục bảo mật PII và đáp ứng các yêu cầu tuân thủ.

## Ứng dụng tham chiếu

Chúng tôi đã ra mắt [One Observability Demo Workshop](https://observability.workshop.aws/) vào tháng 8 năm 2020, sử dụng một ứng dụng có tên là PetAdoptions, được cung cấp trên [GitHub](https://github.com/aws-samples/one-observability-demo/tree/main/PetAdoptions/petsite/petsite/Controllers). Trong tài liệu này, chúng tôi sẽ sử dụng ứng dụng đó làm ứng dụng tham chiếu để thao tác với dữ liệu log và minh họa khả năng bảo vệ dữ liệu. Ứng dụng được xây dựng theo kiến trúc microservices, trong đó các thành phần khác nhau được triển khai trên nhiều dịch vụ, bao gồm: [Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/vi/eks/), [Amazon Elastic Container Service (Amazon ECS)](https://aws.amazon.com/ecs), [AWS Lambda](https://aws.amazon.com/vi/lambda/), [Amazon API Gateway](https://aws.amazon.com/vi/api-gateway/), [Amazon DynamoDB](https://aws.amazon.com/vi/dynamodb/), [Amazon Simple Queue Service (Amazon SQS)](https://aws.amazon.com/vi/sqs/), [Amazon Simple Notification Service (Amazon SNS)](https://aws.amazon.com/vi/sns/), và [AWS Step Functions](https://aws.amazon.com/vi/step-functions/). Kiến trúc của ứng dụng được minh họa trong sơ đồ dưới đây.

<img src="/images/figure-1-blog-4.png" alt="figure-1-blog-4" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 1 : Sơ đồ kiến trúc ứng dụng tham chiếu</p>

Như minh họa trong sơ đồ, ứng dụng được triển khai trên nhiều dịch vụ khác nhau và được viết bằng các ngôn ngữ lập trình khác nhau, chẳng hạn như Java, C#, Go, Python và Node.js. Các thành phần dịch vụ thu thập dấu vết, số liệu và nhật ký, sau đó được gửi đến CloudWatch và X-Ray.

## Kịch  bản

Ứng dụng được triển khai cung cấp cho người dùng tùy chọn thanh toán khi nhận nuôi thú cưng. Do đó, ứng dụng thu thập thông tin thẻ tín dụng cùng với các chi tiết khác của người dùng. Đã có báo cáo về sự cố trong việc nhận nuôi và nhóm phát triển đã thêm chi tiết của người dùng, bao gồm cả thông tin thẻ tín dụng, vào nhật ký để có thể nhanh chóng xác định xu hướng và nguyên nhân gốc rễ.

### Ẩn thông tin nhận dạng cá nhân (PII)

Amazon CloudWatch là một dịch vụ giám sát được quản lý hoàn toàn, giúp khách hàng thu thập dữ liệu telemetry phục vụ quan sát (observability) từ hạ tầng đám mây và ứng dụng. Trong mô hình này, chúng ta tập trung vào việc ghi log ứng dụng với CloudWatch Logs. Các dịch vụ AWS được tích hợp sẵn với CloudWatch và hỗ trợ ghi log theo thời gian thực. Bất kể ứng dụng của bạn được triển khai ở đâu, bạn đều có thể sử dụng [CloudWatch Agent hợp nhất](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) để truyền (stream) dữ liệu log ứng dụng tới dịch vụ CloudWatch.

## Giải pháp

Các ứng dụng xử lý thông tin nhận dạng cá nhân (PII) có thể chứa dữ liệu nhạy cảm trong log. Việc che dữ liệu (data masking) là một cách hiệu quả để bảo vệ các thuộc tính chứa thông tin nhạy cảm, đồng thời vẫn giúp cải thiện MTTR (Mean Time to Resolution - thời gian xử lý sự cố). Giải pháp này mô tả cách khách hàng có thể sử dụng [các chính sách bảo vệ dữ liệu](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/mask-sensitive-log-data.html) (data protection policies) với log ứng dụng để duy trì bảo mật và quyền riêng tư, đồng thời bảo vệ thông tin nhạy cảm. Lợi ích chính của việc sử dụng khả năng che dữ liệu log có sẵn là cho phép áp dụng quyền truy cập thông qua AWS Identity and Access Management (IAM) để hạn chế quyền truy cập đối với các nhóm phát triển rộng hơn. Với cơ chế kiểm soát dựa trên IAM, bạn có thể áp dụng các quyền có điều kiện dựa trên những thuộc tính như địa chỉ IP nguồn, nhằm giới hạn truy cập chỉ từ các mạng được bảo vệ, ví dụ như endpoint VPN hoặc dải IP nội bộ của doanh nghiệp.

Các chính sách bảo vệ dữ liệu của Amazon CloudWatch hỗ trợ các định danh được quản lý sẵn (managed identifiers) mà khách hàng có thể sử dụng để bảo vệ thông tin nhạy cảm như dữ liệu tài chính hoặc thông tin sức khỏe cá nhân. Để đáp ứng các nhu cầu đặc thù và riêng biệt, khách hàng cũng có thể cấu hình các định danh dữ liệu tùy chỉnh (custom data identifiers) nhằm che (mask) dữ liệu nhạy cảm.

Hãy xem xét sự kiện log sau đây có chứa thông tin nhạy cảm:

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

Chính sách bảo vệ dữ liệu dưới đây được cấu hình để phát hiện sự xuất hiện của thông tin nhạy cảm bằng thao tác **Audit**, đồng thời gửi các phát hiện (findings) đến FindingDestination mà không làm gián đoạn quá trình thu thập dữ liệu log. **FindingDestination** có thể là: Một log group trong Amazon CloudWatch, Amazon Kinesis Data Firehose, Hoặc một bucket trong Amazon S3. Khách hàng cũng có thể lựa chọn che (mask) thông tin nhạy cảm bằng cách tạo chính sách bảo vệ dữ liệu với thao tác **Deidentify**. 

Khách hàng có thể sử dụng các managed identifiers cho những trường thường chứa thông tin nhạy cảm như số thẻ tín dụng, địa chỉ email, ngày sinh và thông tin xác thực (credentials). Một chính sách bảo vệ dữ liệu để che các thông tin này sẽ có dạng như sau:

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

Nếu không áp dụng chính sách bảo vệ dữ liệu, các log thô (raw logs) sẽ ghi lại và hiển thị toàn bộ thông tin nhạy cảm như minh họa trong Hình 2 dưới đây:

<img src="/images/figure-2-blog-4.png" alt="figure-2-blog-4" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 2: Dữ liệu nhạy cảm thô hiển thị trong log</p>

Ngay khi chính sách bảo vệ dữ liệu được áp dụng, dữ liệu nhạy cảm sẽ tự động được che (mask) (Hình 3) mà không làm gián đoạn hoạt động của ứng dụng.

<img src="/images/figure-3-blog-4.png" alt="figure-3-blog-4" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 3: Dữ liệu nhạy cảm được che/ẩn (mask/redact) trong log theo chính sách bảo vệ dữ liệu đã định nghĩa</p>

Bất kỳ ai có quyền truy cập vào log trong các log group của Amazon CloudWatch đều sẽ chỉ thấy thông tin đã được che (redacted). Tính năng này nên được kết hợp với các cơ chế kiểm soát danh tính nhằm hạn chế việc "bỏ che" (unmask) dữ liệu chỉ dành cho những người có quyền phù hợp. Điều này có thể thực hiện thông qua các chính sách của AWS Identity and Access Management (IAM) bằng cách sử dụng quyền *logs:Unmask*. Trong các hoạt động vận hành thông thường, bạn nên áp dụng chính sách deny đối với thao tác unmask để đảm bảo dữ liệu nhạy cảm luôn được bảo vệ.

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

Chính sách được trình bày ở trên từ chối quyền bỏ che (unmask) bất kỳ thông tin nào từ mọi tài nguyên, qua đó bổ sung thêm một lớp kiểm soát phòng ngừa nhằm bảo vệ PII mà không làm gián đoạn quy trình làm việc của đội ngũ phát triển. Để có hướng dẫn chi tiết từng bước về cách sử dụng khả năng bảo vệ dữ liệu ở quy mô lớn, hãy tham khảo tài liệu: [How Amazon CloudWatch Logs Data Protection can help detect and protect sensitive log data](https://aws.amazon.com/vi/blogs/mt/how-amazon-cloudwatch-logs-data-protection-can-help-detect-and-protect-sensitive-log-data/).

## Quy trình nâng cấp đặc quyền

Bạn có thể tham khảo các triển khai mẫu về quy trình nâng cấp đặc quyền (privilege escalation workflow) trong một trong các bài blog sau:

- https://aws.amazon.com/vi/blogs/security/managing-temporary-elevated-access-to-your-aws-environment/
- https://aws.amazon.com/vi/blogs/security/temporary-elevated-access-management-with-iam-identity-center/

Sau khi thiết lập xong, bạn cần tạo một IAM role mới với chính sách cho phép (Allow) quyền logs:Unmask, và cho phép người dùng yêu cầu quyền truy cập vào IAM role này thông qua quy trình nâng cấp đặc quyền (privilege escalation workflow).

Khi người dùng cần truy cập vào log thô (raw logs), họ sẽ thực hiện quy trình nâng cấp đặc quyền và assume role có quyền logs:Unmask. Sau khi đảm nhận (assume) role này, họ có thể sử dụng hàm unmask() để xem dữ liệu gốc trong log. Một ví dụ về truy vấn log insights sử dụng hàm unmask() được minh họa bên dưới.

`fields @timestamp, unmask(@message)`<br>
`| sort @timestamp desc`<br>
`| limit 20`

Mỗi lần dữ liệu log được truy cập, một bản ghi sẽ được tạo trong AWS CloudTrail để ghi nhận (audit) hành động này. Bản ghi audit sẽ ghi lại hai thông tin chính: Liệu hàm unmask có được sử dụng hay không, con trỏ (pointer) đến bản ghi log, có thể được sử dụng sau đó trong lệnh [GetLogRecord](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_GetLogRecord.html) để xác định chính xác bản ghi log nào đã được truy cập.

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

## Kết luận

Trong bài viết này, bạn đã hiểu cách thức và lý do cần bảo vệ thông tin nhạy cảm, đồng thời vẫn đảm bảo vận hành hiệu quả mà không ảnh hưởng đến thời gian phản hồi sự cố (MTTR). Các khả năng phát hiện và che dữ liệu tự động của Amazon CloudWatch giúp giảm đáng kể chi phí và công sức trong việc duy trì quy trình che dữ liệu đối với các nhóm làm việc với dữ liệu nhạy cảm. Bên cạnh đó, việc tuân thủ các best practices của AWS Identity and Access Management (IAM) và triển khai nguyên tắc quyền tối thiểu (least privilege) với các kiểm soát chi tiết sẽ giúp tăng cường bảo mật cho các mô hình truy cập vào dữ liệu được bảo vệ.

## Đóng góp và Feedback

Bài dịch này được thực hiện trong khuôn khổ **FCAJ Internship Program**. 

**Liên hệ:** ducmin76@gmail.com

**Feedback:** Mọi góp ý để cải thiện chất lượng dịch thuật xin gửi về email trên

**Updates:** Bài dịch sẽ được cập nhật dựa trên feedback từ cộng đồng

*© 2026 - Bản dịch thuộc về Trần Nguyễn Đức Minh. Vui lòng credit khi sử dụng.*