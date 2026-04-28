---
title : "Blog 2"
date :  "`r Sys.Date()`" 
weight : 2 
pre: <b> 3.2 </b>
chapter : false
---

# Xây dựng ứng dụng multi-tenant SaaS với chế độ tenant isolation mới của AWS Lambda


**Bài viết gốc:** [Xây dựng ứng dụng multi-tenant SaaS với chế độ tenant isolation mới của AWS Lambda](https://aws.amazon.com/vi/blogs/compute/building-multi-tenant-saas-applications-with-aws-lambdas-new-tenant-isolation-mode/)

**Tác giả:** Anton Aleksandrov (Principal Solutions Architect, Amazon Web Services) và Ayush Kulkarni

**Ngày xuất bản:** 20/11/2025

**Nguồn:** AWS Compute Blog

**Người dịch:** Trần Nguyễn Đức Minh - FCAJ Intern

**Ngày dịch:** 18/03/2026  

**Thời gian đọc:** 30 phút

## Tóm tắt 

Bài viết giới thiệu một tính năng mới của AWS Lambda gọi là tenant isolation mode, giúp đơn giản hóa việc xây dựng các ứng dụng multi-tenant SaaS trên kiến trúc serverless. Trước đây, để đảm bảo cách ly giữa các tenant, developers thường phải tự triển khai logic trong code hoặc tạo riêng một Lambda function cho mỗi tenant, dẫn đến độ phức tạp và chi phí vận hành cao.

Với tenant isolation mode, AWS Lambda cho phép tách biệt execution environment theo từng tenant, ngay cả khi tất cả cùng sử dụng một function. Khi gọi Lambda, hệ thống sử dụng tenant ID để định tuyến request đến đúng môi trường thực thi, đảm bảo rằng dữ liệu và trạng thái của mỗi tenant không bị chia sẻ với tenant khác.

Giải pháp này giúp tăng cường bảo mật và đơn giản hóa kiến trúc, đồng thời vẫn tận dụng được khả năng tái sử dụng môi trường (warm start) cho từng tenant. Tuy nhiên, nó cũng đi kèm một số trade-offs như tăng số lượng cold start và chi phí do cần nhiều execution environments hơn.

Nhìn chung, tenant isolation mode giúp AWS Lambda trở thành một lựa chọn hiệu quả hơn cho các hệ thống SaaS yêu cầu mức độ cách ly cao giữa các tenant.

**Đối tượng đọc:** Cloud engineers, backend developers và solution architects đang xây dựng hoặc thiết kế các hệ thống SaaS multi-tenant trên AWS, đặc biệt là những người làm việc với kiến trúc serverless

**Độ khó:** Intermediate

**Tags:** AWS Lambda, Serverless, Multi-tenant SaaS, Tenant Isolation, Cloud Architecture, Security, Scalability

## Mục lục

- [Xây dựng ứng dụng multi-tenant SaaS với chế độ tenant isolation mới của AWS Lambda](#xây-dựng-ứng-dụng-multi-tenant-saas-với-chế-độ-tenant-isolation-mới-của-aws-lambda)
  - [Tóm tắt](#tóm-tắt)
  - [Mục lục](#mục-lục)
  - [Giới thiệu](#giới-thiệu)
  - [Tổng quan](#tổng-quan)
  - [Kịch bản ví dụ](#kịch-bản-ví-dụ)
  - [Bắt đầu với Lambda Tenant Isolation Mode](#bắt-đầu-với-lambda-tenant-isolation-mode)
  - [Tích hợp với Amazon API Gateway](#tích-hợp-với-amazon-api-gateway)
  - [Khả năng quan sát (Observability)](#khả-năng-quan-sát-observability)
  - [Các lưu ý](#các-lưu-ý)
  - [Best practices](#best-practices)
  - [Code mẫu](#code-mẫu)
  - [Kết luận](#kết-luận)
  - [Đóng góp và Feedback](#đóng-góp-và-feedback)

## Giới thiệu

Hôm nay, AWS đã công bố một chế độ cô lập tenant (tenant isolation mode) mới cho [AWS Lambda](https://aws.amazon.com/vi/lambda/), cho phép bạn xử lý các lần gọi hàm (function invocations) trong các môi trường thực thi (execution environments) riêng biệt cho từng end-user hoặc tenant của ứng dụng khi họ invoke Lambda function. Khả năng này giúp đơn giản hóa việc xây dựng các ứng dụng SaaS multi-tenant bảo mật bằng cách AWS tự động quản lý việc cô lập môi trường compute ở cấp tenant (tenant-level isolation) và định tuyến request (request routing) cho bạn. Nhờ đó, bạn có thể tập trung vào business logic cốt lõi thay vì phải tự triển khai cơ chế cô lập môi trường compute theo tenant.

## Tổng quan

AWS Lambda chạy code function của bạn trong các execution environments bảo mật, sử dụng công nghệ [Firecracker virtualization](https://firecracker-microvm.github.io/) để đảm bảo isolation. Các execution environment này không bao giờ chia sẻ hoặc tái sử dụng tài nguyên ảo (như vCPU, disk, hoặc memory) giữa các function, thậm chí giữa các version khác nhau của cùng một function. Tuy nhiên, Lambda có thể tái sử dụng execution environment cho nhiều lần invocation của cùng một version function. Điều này là do các environment này đã được thiết lập sẵn (fully initialized), từ đó giúp tăng tốc độ xử lý request (faster request processing) cho function của bạn.

<img src="/images/figure-1-blog-2.png" alt="figure-1-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 1. Các invocations được xử lý bởi một tập hợp các execution environments thuộc về một function duy nhất.</p>

Các ứng dụng SaaS multi-tenant xử lý dữ liệu nhạy cảm theo từng tenant, hoặc thực thi code được cung cấp động bởi tenant, thường yêu cầu mức độ isolation cao hơn - cụ thể là ở tenant level thay vì chỉ ở function level - nhằm đảm bảo thực thi code an toàn và giảm thiểu rủi ro truy cập dữ liệu chéo giữa các tenant (cross-tenant data access).

Trước khi tính năng này được ra mắt, các developer thường phải tự triển khai các giải pháp custom, ví dụ như sử dụng SDK hoặc viết application logic để quản lý isolation ngay trong function code. Cách tiếp cận này dễ phát sinh lỗi (bug-prone), làm tăng khối lượng công việc cho team phát triển, và không đảm bảo isolation ở cấp compute environment.

Một phương án khác là tạo function riêng cho từng tenant, đồng nghĩa với việc replicate cùng một code cho hàng trăm hoặc hàng nghìn tenant. Cách này cung cấp mức độ isolation mạnh hơn ở compute environment so với việc share environment giữa nhiều tenant trong cùng một function. Tuy nhiên, nó lại làm tăng implementation overhead và operational complexity, đặc biệt khi hệ thống scale để phục vụ số lượng tenant ngày càng lớn theo thời gian.

<img src="/images/figure-2-blog-2.png" alt="figure-2-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 2. Mô hình function-per-tenant, mỗi yêu cầu của tenant được xử lý bởi một hàm riêng biệt.</p>

Bắt đầu từ hôm nay, AWS Lambda cung cấp một tenant isolation mode mới, cho phép bạn cô lập các execution environments giữa các tenant khác nhau trong ứng dụng SaaS multi-tenant - ngay cả khi tất cả tenant đều invoke cùng một function. Khi bạn bật chế độ tenant isolation mới này, bạn sẽ truyền vào một tenant identifier trong mỗi lần function invocation. Lambda sẽ sử dụng identifier này để route request đến đúng execution environment tương ứng. Kết quả là, mỗi execution environment chỉ được reuse cho các invocation thuộc cùng một tenant. Điều này giúp bạn vẫn tận dụng được lợi ích hiệu năng của warm execution environments, đồng thời đảm bảo workload của từng tenant luôn được cô lập hoàn toàn (isolated).

<img src="/images/figure-3-blog-2.png" alt="figure-3-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 3. Với khả năng cô lập mới, Lambda tạo ra các môi trường thực thi riêng biệt cho từng tenant trong cùng một hàm.</p>

Đối với các tổ chức xử lý dữ liệu nhạy cảm theo từng tenant hoặc thực thi untrusted code được cung cấp động bởi end-user, chế độ tenant isolation mode mới của AWS Lambda mang lại lợi ích bảo mật từ việc tách biệt compute environment theo từng tenant, mà không phải chịu sự phức tạp trong vận hành khi phải quản lý function riêng lẻ hoặc infrastructure riêng cho từng tenant.

## Kịch bản ví dụ

Hãy xem xét việc xây dựng một ứng dụng SaaS serverless multi-tenant. Để tối ưu hiệu năng, function handler của bạn có thể lấy các cấu hình và dữ liệu riêng theo từng tenant, sau đó cache chúng trong memory và tái sử dụng cho các lần invocation tiếp theo từ cùng tenant. Ví dụ, bạn có thể cache các thông tin như database location theo tenant, feature flags, hoặc business rules thường xuyên được truy cập trong quá trình xử lý request. Những dữ liệu này có thể được lưu trong runtime process của ứng dụng dưới dạng global variables hoặc dưới dạng file trong thư mục `/tmp`. Tuy nhiên, nếu execution environment bên dưới được dùng để phục vụ nhiều tenant khác nhau, cách tiếp cận này có thể dẫn đến rủi ro rò rỉ dữ liệu giữa các tenant (cross-tenant data exposure).

Với tenant isolation mode, bạn có thể giải quyết vấn đề này bằng một kiến trúc và cấu hình đơn giản hơn nhiều. Tính năng built-in này của AWS Lambda giúp Lambda trở thành một lựa chọn rất phù hợp cho các ứng dụng SaaS multi-tenant cần compute environment được cô lập riêng cho từng tenant.

## Bắt đầu với Lambda Tenant Isolation Mode

Sử dụng tham số mới **tenancy-config** để cấu hình tenant isolation mode khi bạn tạo function. Lưu ý rằng cấu hình này chỉ có thể được áp dụng tại thời điểm tạo function, và không thể cập nhật lại cho các function đã tồn tại. Đoạn snippet dưới đây minh họa cách tạo một function với tenancy config bằng [AWS CLI](https://aws.amazon.com/cli/).

```
aws lambda create-function \
   --function-name my-function1 \
   --runtime nodejs22.x \
   --zip-file fileb://my-function1.zip \
   --handler index.handler \
   --role arn:aws:iam:1234567890:role/my-function-role \
   --tenancy-config '{"TenantIsolationMode": "PER_TENANT"}'
```

Sau khi function được tạo, bạn phải cung cấp tham số tenant ID trong mỗi lần invocation. AWS Lambda sẽ sử dụng identifier này để đảm bảo rằng execution environment dành cho một tenant cụ thể không bao giờ được tái sử dụng cho tenant khác. Đối với các lần invocation tiếp theo từ cùng một tenant, Lambda có thể reuse execution environment nhằm tối ưu hiệu năng. Bạn có thể truyền tham số **tenant-id** như minh họa bên dưới:

```
aws lambda invoke \
   --function-name my-function \
   --tenant-id BlueTenant \
   response.json
```

Tham số mới **tenant-id** là bắt buộc đối với các function sử dụng tenant isolation mode trong AWS Lambda. Nếu một function invocation không cung cấp tham số này, request sẽ bị fail và trả về invocation error, như minh họa bên dưới:

```
aws lambda invoke --function-name multitenant-function out.json

An error occurred (InvalidParameterValueException) when calling the Invoke operation:
The invoked function is enabled with tenancy configuration. 
Add a valid tenant ID in your request and try again.
```

AWS Lambda cung cấp tham số tenant ID thông qua context object trong function handler của bạn. Điều này cho phép bạn truy cập thông tin theo từng tenant trực tiếp trong code. Nhờ đó, bạn có thể implement các custom logic dựa trên tenant identity, ví dụ như xử lý khác nhau cho từng tenant, như minh họa bên dưới:

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

Bảng dưới đây mô tả sự khác biệt giữa các function của AWS Lambda khi không sử dụng và có sử dụng tenant isolation mode:

| Feature | Without the new tenant isolation mode | With the new tenant isolation mode |
|---------|---------------------------------------|------------------------------------|
| Execution environment isolation | Được cô lập theo từng function version. | Được cô lập theo từng end-user hoặc tenant invoke một function version. |
| Execution environment reuse | Có thể được reuse để xử lý tất cả các invocation của một function version. | Chỉ được reuse cho các invocation đến từ cùng một tenant invoke function version đó. |
| Data stored on local disk và in-memory | Có khả năng được truy cập bởi tất cả các invocation của một function version. | Chỉ có thể truy cập bởi các invocation thuộc cùng tenant. Không thể truy cập từ tenant khác. |
| Cold starts | Xảy ra khi không có warm execution environment để xử lý request đến. | Xảy ra khi không có tenant-specific warm execution environment. Có thể xảy ra nhiều cold start hơn do environment được tách riêng theo từng tenant. |

## Tích hợp với Amazon API Gateway

[Amazon API Gateway](https://aws.amazon.com/vi/api-gateway/) sử dụng [Lambda’s Invoke API](https://docs.aws.amazon.com/lambda/latest/api/API_Invoke.html) để gọi các function của AWS Lambda. Khi sử dụng Invoke API, Lambda yêu cầu tham số tenant ID phải được truyền thông qua HTTP header **X-Amz-Tenant-Id**. Bạn có thể cấu hình API Gateway để tự động inject header này vào request gọi Lambda, với giá trị được lấy từ các thuộc tính của client request như HTTP header, query parameter hoặc path parameter. Ngoài ra, khi sử dụng [Lambda Authorizers](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html), bạn cũng có thể lấy tenant ID từ authorization context do authorizer trả về, chẳng hạn như principal ID hoặc JWT claim, và xem [tài liệu API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-lambda-authorizer-output.html) để biết cách mà bạn có thể sử dụng các giá trị này để gán cho header **X-Amz-Tenant-Id** khi gửi request đến Lambda.

<img src="/images/figure-4-blog-2.png" alt="figure-4-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 4. Lấy giá trị X-Amz-Tenant-Id từ các nguồn xác thực.</p>

Ảnh minh họa dưới đây cho thấy cấu hình tích hợp giữa Amazon API Gateway và AWS Lambda, trong đó request gửi đến API Gateway chứa header **x-tenant-id**. Header này được map sang **X-Amz-Tenant-Id** trong request gọi Lambda, nhằm kích hoạt function với tenant isolation mode.

<img src="/images/figure-5-blog-2.png" alt="figure-5-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 5. Ánh xạ (mapping) HTTP header từ client request sang header tenant-id của AWS Lambda.</p>

Đoạn code dưới đây minh họa cách cấu hình này được triển khai bằng AWS CDK.

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

## Khả năng quan sát (Observability)

Đối với các function sử dụng tenant isolation, AWS Lambda sẽ tự động include tenant ID vào Lambda [function logs](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-logs.html) khi bạn [bật JSON logging](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-cloudwatchlogs-logformat.html), giúp việc monitoring và debug các vấn đề theo từng tenant trở nên dễ dàng hơn. Lưu ý rằng thuộc tính **tenantId** chỉ khả dụng trong quá trình function invocation, chứ không có trong giai đoạn function initialization. Thuộc tính này được include trong cả Lambda [platform events](https://docs.aws.amazon.com/lambda/latest/dg/telemetry-schema-reference.html) (như **platform.start** và **platform.report**) cũng như trong các custom logs do bạn in ra từ function code, như minh họa trong hình bên dưới:

<img src="/images/figure-6-blog-2.png" alt="figure-6-blog-2" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 6. Log của AWS Lambda hiển thị thuộc tính tenantId trong quá trình thực thi function.</p>

AWS Lambda tạo một [CloudWatch log stream](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) riêng biệt cho mỗi execution environment. Bạn có thể sử dụng [CloudWatch Log Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html) để tìm các log stream thuộc về một tenant cụ thể bằng cách filter theo tenantId:

```
fields @logStream, @message
| filter tenantId=='BlueTenant' or record.tenantId=='BlueTenant'
| stats count() as logCount by @logStream
| sort @timestamp desc
```

Bạn cũng có thể truy xuất (retrieve) các log theo từng tenant trên tất cả log stream, bằng cách filter theo tenantId trong CloudWatch Logs Insights khi làm việc với AWS Lambda:

```
fields @message
| filter tenantId=='BlueTenant' or record.tenantId=='BlueTenant'
| limit 1000
```

Mỗi CloudWatch log stream bắt đầu với các log của function initialization, sau đó là các log của function invocation. Cấu trúc này giúp bạn dễ dàng debug các vấn đề theo từng tenant và hiểu rõ lifecycle của từng execution environment trong AWS Lambda.

## Các lưu ý

Khi sử dụng tenant isolation mode cho AWS Lambda, bạn nên lưu ý các điểm sau:
- Execution environment của mỗi tenant được cô lập hoàn toàn với tenant khác, do đó dữ liệu riêng của tenant (lưu trên disk hoặc in-memory) sẽ không bị truy cập chéo khi nhiều tenant cùng invoke một Lambda function.
- Tất cả tenant vẫn chia sẻ cùng một execution role của function. Nếu cần phân quyền chi tiết hơn theo từng tenant, bạn nên propagate tenant-scoped credentials từ các upstream service gọi đến Lambda.
- Ứng dụng của bạn có thể gặp tỷ lệ cold start cao hơn, vì Lambda phải xử lý request trong các execution environment riêng biệt cho từng tenant.
- Bạn sẽ trả phí cho mỗi execution environment riêng theo tenant được tạo ra, tùy thuộc vào cấu hình memory của function. Bạn có thể tham khảo chi tiết tại [Lambda pricing page](https://aws.amazon.com/vi/lambda/pricing/).

## Best practices

Khi sử dụng tenant isolation mode cho AWS Lambda, AWS khuyến nghị một số best practices sau:
- Triển khai cơ chế validate tenant ID chặt chẽ ở tầng application để ngăn chặn truy cập trái phép thông qua việc giả mạo hoặc thao túng tenant ID. Bạn nên sử dụng một service hoặc database riêng để quản lý danh sách tenant ID hợp lệ.
- Thường xuyên monitor và audit access pattern của từng tenant để phát hiện sớm các dấu hiệu bất thường hoặc các nỗ lực truy cập cross-tenant trái phép.
- Lưu ý đến [Lambda concurrency quotas](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) khi xây dựng ứng dụng multi-tenant. Bạn có thể cần yêu cầu tăng quota tùy theo số lượng tenant và pattern sử dụng thực tế.

## Code mẫu

Bạn có thể làm theo hướng dẫn trong [GitHub repository](https://github.com/aws-samples/sample-lambda-tenant-isolation) mẫu để triển khai (provision) một project demo trên tài khoản của mình và trải nghiệm trực tiếp tenant isolation mode mới của AWS Lambda. Project mẫu này minh họa cách tích hợp Lambda với [Amazon API Gateway](https://aws.amazon.com/api-gateway/), đồng thời truyền (propagate) tenant identity từ request của client.

## Kết luận

Tính năng tenant isolation mode giúp đơn giản hóa việc xây dựng các ứng dụng SaaS multi-tenant serverless trên AWS. Bằng cách tự động quản lý việc cô lập compute environment ở cấp tenant, Lambda loại bỏ nhu cầu phải viết custom isolation logic hoặc tạo function riêng cho từng tenant, từ đó cho phép bạn tập trung hoàn toàn vào business logic cốt lõi trong khi AWS xử lý các phức tạp liên quan đến tenant-aware isolation.

Khi kết hợp với các tính năng bảo mật sẵn có, khả năng scale nhanh và mô hình tính phí pay-per-use, tenant isolation mode giúp Lambda trở thành lựa chọn hấp dẫn hơn cho các ứng dụng SaaS hiện đại, cho dù bạn có đang xây dựng hệ thống mới hay nâng cấp hệ thống hiện có. 

Để tìm hiểu thêm, bạn có thể tham khảo [tài liệu chính thức về tenant isolation](https://docs.aws.amazon.com/lambda/latest/dg/tenant-isolation.html) và [trang pricing của Lambda](https://aws.amazon.com/vi/lambda/pricing/).

## Đóng góp và Feedback

Bài dịch này được thực hiện trong khuôn khổ **FCAJ Internship Program**. 

**Liên hệ:** ducmin76@gmail.com

**Feedback:** Mọi góp ý để cải thiện chất lượng dịch thuật xin gửi về email trên

**Updates:** Bài dịch sẽ được cập nhật dựa trên feedback từ cộng đồng

*© 2026 - Bản dịch thuộc về Trần Nguyễn Đức Minh. Vui lòng credit khi sử dụng.*