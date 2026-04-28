---
title : "Blog 3"
date :  "`r Sys.Date()`" 
weight : 3
pre: <b> 3.3 </b>
chapter : false
---

# Thiết kế kiến trúc cho phát triển agentic AI trên AWS

**Bài viết gốc:** [Thiết kế kiến trúc cho phát triển agentic AI trên AWS](https://aws.amazon.com/vi/blogs/architecture/architecting-for-agentic-ai-development-on-aws/)

**Tác giả:** Alan Oberto Jimenez (Application Architect)

**Ngày xuất bản:** 26/03/2026

**Nguồn:** AWS Architecture Blog

**Người dịch:** Trần Nguyễn Đức Minh - FCAJ Intern

**Ngày dịch:** 10/04/2026

**Thời gian đọc:** 35 phút

## Tóm tắt

Bài blog này giải thích cách thiết kế hệ thống để phát triển agentic AI trên AWS, tức là các hệ thống AI có khả năng tự lập kế hoạch, hành động và tương tác với môi trường thay vì chỉ phản hồi một lần như mô hình truyền thống.

Nội dung tập trung vào việc chuyển từ mô hình AI "request–response" sang các kiến trúc có vòng lặp (loop), nơi agent có thể:
- Nhận mục tiêu
- Lập kế hoạch từng bước
- Gọi công cụ hoặc API bên ngoài
- Đánh giá kết quả và điều chỉnh hành động

Blog trình bày một kiến trúc tham chiếu trên AWS, thường bao gồm:
- LLM làm "bộ não" (decision engine)
- Orchestration layer để quản lý workflow và trạng thái
- Tool integrations (API, database, services)
- Memory (ngắn hạn và dài hạn)
- Monitoring và guardrails để kiểm soát hành vi

Một điểm quan trọng là nhấn mạnh việc tách rời các thành phần (modular design) để dễ mở rộng, thay thế model, và kiểm soát chi phí. AWS cũng gợi ý sử dụng các dịch vụ như orchestration workflows, storage, và compute để xây dựng hệ thống agent linh hoạt.

Ngoài ra, blog đề cập đến các thách thức thực tế:
- Quản lý trạng thái và context
- Đảm bảo độ tin cậy của agent
- Kiểm soát chi phí do vòng lặp nhiều bước
- Thiết lập guardrails để tránh hành vi ngoài ý muốn

Tổng thể, bài viết hướng dẫn cách xây dựng agent AI theo hướng production-ready, không chỉ demo, với trọng tâm là khả năng mở rộng, quan sát được (observability), và kiểm soát.

**Đối tượng đọc:** Developers và software engineers làm về AI/ML, solution architects thiết kế hệ thống trên cloud, product builders muốn xây dựng AI agents thực tế, người đã có kiến thức cơ bản về LLM hoặc AWS

**Độ khó:** Intermediate đến Advance

**Tags:** AI Agents, Agentic AI, AWS Architecture, LLM, Cloud Design, Distributed Systems, AI Engineering

## Mục lục

- [Thiết kế kiến trúc cho phát triển agentic AI trên AWS](#thiết-kế-kiến-trúc-cho-phát-triển-agentic-ai-trên-aws)
  - [Tóm tắt](#tóm-tắt)
  - [Mục lục](#mục-lục)
  - [Giới thiệu](#giới-thiệu)
  - [Vì sao kiến trúc truyền thống cản trở agentic AI](#vì-sao-kiến-trúc-truyền-thống-cản-trở-agentic-ai)
  - [Kiến trúc hệ thống cho các vòng phản hồi agentic nhanh](#kiến-trúc-hệ-thống-cho-các-vòng-phản-hồi-agentic-nhanh)
    - [Giả lập cục bộ (local emulation) như con đường phản hồi mặc định](#giả-lập-cục-bộ-local-emulation-như-con-đường-phản-hồi-mặc-định)
    - [Phát triển offline cho các workload dữ liệu và phân tích](#phát-triển-offline-cho-các-workload-dữ-liệu-và-phân-tích)
    - [Kiểm thử hybrid với tài nguyên cloud nhẹ](#kiểm-thử-hybrid-với-tài-nguyên-cloud-nhẹ)
    - [Preview environments và thiết kế contract-first](#preview-environments-và-thiết-kế-contract-first)
  - [Kiến trúc code base thân thiện với AI](#kiến-trúc-code-base-thân-thiện-với-ai)
    - [Cấu trúc domain-driven với ranh giới rõ ràng](#cấu-trúc-domain-driven-với-ranh-giới-rõ-ràng)
    - [Mã hóa ý đồ kiến trúc bằng project rules](#mã-hóa-ý-đồ-kiến-trúc-bằng-project-rules)
    - [Tests như các "đặc tả có thể thực thi" (executable specifications)](#tests-như-các-đặc-tả-có-thể-thực-thi-executable-specifications)
    - [Monorepo và tài liệu có thể đọc bởi máy (machine-readable documentation)](#monorepo-và-tài-liệu-có-thể-đọc-bởi-máy-machine-readable-documentation)
    - [Tích hợp AI agents một cách an toàn vào delivery pipelines](#tích-hợp-ai-agents-một-cách-an-toàn-vào-delivery-pipelines)
  - [Kết luận](#kết-luận)
  - [Đóng góp và Feedback](#đóng-góp-và-feedback)

## Giới thiệu

Nếu bạn đang thiết kế các hệ thống cloud cho phát triển AI trên AWS, có lẽ bạn đã nhận ra rằng các kiến trúc truyền thống tạo ra nhiều friction cho AI agents. Nhiều đội ngũ cloud đang thử nghiệm các AI coding assistants nhưng nhanh chóng nhận thấy khoảng cách giữa những gì các công cụ này hứa hẹn và những gì kiến trúc của họ thực sự cho phép. Khi một AI agent sinh ra code, thường phải mất vài phút-thậm chí vài giờ-trước khi bạn có thể validate xem thay đổi đó có thực sự hoạt động hay không. Chu kỳ deployment chậm, các service bị tightly coupled, và code base thiếu minh bạch khiến mỗi lần iteration trở thành một quá trình nhiều ma sát. Kết quả là, AI agents khó có thể vận hành một cách autonomous, và developers lại phải quay về các vòng lặp validation thủ công.

Bài viết này dành cho các cloud architects muốn loại bỏ những friction đó. Nội dung tập trung vào agentic development, một mô hình mà AI agent không chỉ gợi ý snippet-mà còn có thể viết, test, deploy và refine code thông qua các vòng phản hồi nhanh (rapid feedback cycles). Để làm được điều này, cả system architecture và code base architecture của bạn cần được thiết kế để hỗ trợ việc validation nhanh, iteration an toàn và thể hiện intent rõ ràng.

Trong bài viết này, chúng tôi sẽ trình bày cách thiết kế các hệ thống AWS giúp AI agents có thể iteration nhanh chóng thông qua các design patterns cho cả system architecture và cấu trúc code base. Trước tiên, chúng ta sẽ xem xét những vấn đề kiến trúc đang hạn chế agentic development hiện nay. Sau đó, bài viết sẽ đi qua các system architecture patterns hỗ trợ experimentation nhanh, tiếp theo là các codebase patterns giúp AI agents hiểu, chỉnh sửa và validate ứng dụng của bạn một cách tự tin.

## Vì sao kiến trúc truyền thống cản trở agentic AI

Hầu hết các kiến trúc cloud được thiết kế cho human-driven development. Chúng giả định môi trường tồn tại lâu dài (long-lived environments), testing thủ công, và deployment không thường xuyên. Nhưng trong một agentic workflow, những giả định này không còn phù hợp.

AI agents cần validate thay đổi liên tục. Khi mỗi lần test đều yêu cầu provisioning tài nguyên cloud, chờ pipeline chạy, hoặc debug các lỗi chỉ xuất hiện khi deployment, thì các vòng phản hồi (feedback loops) trở nên quá chậm. Sự tight coupling giữa business logic và cloud services càng làm việc testing cục bộ (local testing) trở nên phức tạp, trong khi cấu trúc project thiếu nhất quán khiến agent khó xác định chính xác nơi cần thay đổi.

Nếu không có sự hỗ trợ từ kiến trúc, agentic AI sẽ tạo ra nhiều rủi ro hơn là giá trị. Giải pháp không nằm ở việc viết prompt tốt hơn, mà là xây dựng một kiến trúc coi feedback nhanh và ranh giới rõ ràng (clear boundaries) là các yếu tố cốt lõi. Những friction trong kiến trúc này không chỉ gây bất tiện-chúng còn giới hạn trực tiếp hiệu quả của AI agents. Dưới đây là cách bạn có thể thiết kế lại kiến trúc để khai thác tối đa tiềm năng của agentic AI.

## Kiến trúc hệ thống cho các vòng phản hồi agentic nhanh

Agentic development phụ thuộc rất lớn vào tốc độ phản hồi (feedback speed). Agent càng nhanh chóng quan sát được tác động của một thay đổi, thì càng có thể refine output một cách hiệu quả. Ở đây, system architecture đóng vai trò quyết định.

<img src="/images/figure-1-blog-3.jpeg" alt="figure-1-blog-3" style="width:600px !important; max-width:600px !important;">
<p style="text-align:center; font-style:italic;">Hình 1: Kiến trúc tổng thể hỗ trợ agentic development: local test loops, ephemeral test stack, và pipeline CI/CD được AI kích hoạt.</p>

### Giả lập cục bộ (local emulation) như con đường phản hồi mặc định

Bất cứ khi nào có thể, kiến trúc của bạn nên cho phép AI agents test các thay đổi ngay trên môi trường local trước khi sử dụng tài nguyên cloud. AWS cung cấp nhiều công cụ giúp điều này trở nên khả thi.

Ví dụ, các ứng dụng serverless được xây dựng với [AWS Lambda](https://aws.amazon.com/vi/lambda/) và [Amazon API Gateway](https://aws.amazon.com/vi/api-gateway/) có thể được giả lập cục bộ bằng [AWS Serverless Application Model](https://aws.amazon.com/vi/serverless/sam/) (AWS SAM). Với lệnh `sam local start-api`, một AI agent có thể gọi Lambda functions thông qua API Gateway được giả lập trên local, quan sát phản hồi ngay lập tức và lặp (iterate) chỉ trong vài giây thay vì vài phút.

Containers cũng mang lại lợi ích tương tự cho các dịch vụ chạy trên [Amazon Elastic Container Service](https://aws.amazon.com/vi/ecs/) (Amazon ECS) hoặc [AWS Fargate](https://aws.amazon.com/vi/fargate/). Bằng cách build và chạy cùng một container image trên local, agent có thể validate hành vi của ứng dụng trước khi deploy lên cloud. Đối với lưu trữ dữ liệu, [Amazon DynamoDB](https://aws.amazon.com/vi/dynamodb/) Local cho phép agent test các thao tác create, read, update, delete (CRUD) trên một database local mô phỏng API của DynamoDB.

Lưu ý: Local emulation giúp giảm đáng kể thời gian iteration, cho phép code do AI sinh ra được validate chỉ trong vài giây, đồng thời giảm chi phí và rủi ro khi thử nghiệm.

### Phát triển offline cho các workload dữ liệu và phân tích

Nhiều workload phù hợp với mô hình test request-response, nhưng các data processing pipelines thường liên quan đến dữ liệu lớn và thực thi phân tán. Ngay cả trong trường hợp này, agentic workflows vẫn hưởng lợi đáng kể từ phản hồi cục bộ (local feedback).

[AWS Glue](https://aws.amazon.com/vi/glue/) cung cấp các Docker image cho phép chạy Glue jobs trên môi trường local cùng với thư viện AWS Glue ETL. Một AI agent có thể validate các bước transform trên các tập dữ liệu mẫu, kiểm tra kết quả trung gian, và chỉ chuyển lên cloud khi cần test ở quy mô lớn. Mô hình tương tự cũng áp dụng cho các workload về dữ liệu và machine learning (ML): tách biệt logic, test local với dữ liệu thu nhỏ, sau đó đưa code đã được kiểm chứng lên các managed services.

**Lưu ý:** Phát triển offline giúp rút ngắn feedback loop cho các workload dữ liệu và giảm số lần chạy cloud không cần thiết trong giai đoạn iteration ban đầu.

### Kiểm thử hybrid với tài nguyên cloud nhẹ

Một số dịch vụ AWS không thể được giả lập hoàn toàn trên môi trường local. Trong những trường hợp này, mục tiêu không phải là tránh sử dụng cloud, mà là giữ cho việc phản hồi từ cloud nhẹ và nhanh.

Đối với các hệ thống hướng sự kiện (event-driven) sử dụng [Amazon Simple Notification Service](https://aws.amazon.com/vi/sns/) (Amazon SNS) hoặc [Amazon Simple Queue Service](https://aws.amazon.com/vi/sqs/) (Amazon SQS), bạn có thể định nghĩa các môi trường development tối giản bằng các công cụ infrastructure as code (IaC) như [AWS CloudFormation](https://aws.amazon.com/vi/cloudformation/) hoặc [AWS Cloud Development Kit](https://aws.amazon.com/vi/cdk/) (AWS CDK). Một AI agent có thể deploy các tài nguyên nhỏ, tách biệt, gọi chúng thông qua AWS SDK, và validate hành vi mà không cần provisioning toàn bộ hệ thống.

Cách tiếp cận hybrid này xem cloud như một dependency trong quá trình test-được sử dụng một cách tiết kiệm và có kiểm soát.

Lưu ý: Hybrid testing giúp xác nhận hành vi thực tế của dịch vụ từ sớm, đồng thời giữ việc sử dụng cloud tập trung và hiệu quả.

### Preview environments và thiết kế contract-first

Phản hồi nhanh không chỉ dừng lại ở việc test trên local. Việc validate end-to-end vẫn rất quan trọng, đặc biệt khi có nhiều service tương tác với nhau.

Preview environments là các môi trường tạm thời (short-lived) được tạo theo nhu cầu để phục vụ validation. Được định nghĩa bằng IaC, chúng cho phép AI agent deploy toàn bộ ứng dụng, chạy các smoke test, và sau đó teardown toàn bộ tài nguyên khi hoàn tất. Khi kết hợp với contract-first design-nơi các API được định nghĩa trước bằng OpenAPI specifications-agents có thể validate tích hợp giữa các service ngay cả khi chưa triển khai đầy đủ tất cả thành phần.

**Lưu ý:** Preview environments giúp giảm rủi ro tích hợp (integration risk) và cho phép các thay đổi do AI tạo ra được kiểm chứng an toàn trước khi đưa vào production.

## Kiến trúc code base thân thiện với AI

System architecture giúp tăng tốc phản hồi, nhưng code base architecture mới là yếu tố quyết định liệu một AI agent có thể hiểu được những gì nó đang thay đổi hay không.

### Cấu trúc domain-driven với ranh giới rõ ràng

Chúng tôi khuyến nghị áp dụng agentic development khi repository của bạn phản ánh rõ ràng ý đồ kiến trúc. Một cấu trúc domain-driven lấy cảm hứng từ [Domain-Driven Design](https://docs.aws.amazon.com/prescriptive-guidance/latest/hexagonal-architectures/overview.html#ddd) (DDD) sẽ tách biệt business logic cốt lõi khỏi phần orchestration của ứng dụng và các yếu tố hạ tầng.

Trong thực tế, điều này thường có nghĩa là tổ chức code thành các layer rõ ràng như /domain, /application và /infrastructure, nơi mỗi phần đảm nhận một vai trò riêng biệt. Domain layer tập trung vào business logic cốt lõi và hoàn toàn không phụ thuộc vào các dịch vụ Amazon, giúp logic nghiệp vụ luôn thuần và dễ kiểm thử. Trong khi đó, infrastructure layer chịu trách nhiệm tích hợp với các dịch vụ như Amazon DynamoDB hoặc Amazon Simple Notification Service. Nhờ sự tách biệt này, AI agents có thể chỉnh sửa và kiểm tra business logic ngay trên môi trường local mà không cần chạm tới các thành phần phụ thuộc cloud, từ đó tăng tốc độ iteration và giảm rủi ro khi thay đổi.

Các pattern như [hexagonal architecture](https://docs.aws.amazon.com/prescriptive-guidance/latest/hexagonal-architectures/welcome.html) (kiến trúc lục giác) còn củng cố sự tách biệt này bằng cách xem các hệ thống bên ngoài như adapter thay vì dependency trực tiếp.

Lưu ý: Ranh giới rõ ràng giúp giảm các tác động ngoài ý muốn và khiến các thay đổi do AI tạo ra trở nên dễ hiểu, dễ kiểm thử hơn.

### Mã hóa ý đồ kiến trúc bằng project rules

Ngay cả khi repository đã được tổ chức tốt, việc có hướng dẫn rõ ràng và tường minh vẫn mang lại nhiều giá trị. [Kiro](https://kiro.dev/) hỗ trợ [steering files](https://kiro.dev/docs/cli/steering/)-các file Markdown được lưu trong thư mục `.kiro/steering/`-dùng để mô tả các [architectural constraints và coding conventions](https://kiro.dev/docs/cli/steering/#foundational-steering-files).

Ví dụ, một rule có thể quy định rằng mọi truy cập database phải đi qua các repository class trong infrastructure layer. AI agent sẽ tự động tham chiếu các rule này, giúp giảm nhu cầu phải lặp lại constraint trong mỗi prompt và đảm bảo code được sinh ra luôn tuân theo kiến trúc đã định.

**Lưu ý:** Project rules giúp giảm architectural drift và duy trì tính nhất quán khi AI agents hoạt động ngày càng autonomous.

### Tests như các "đặc tả có thể thực thi" (executable specifications)

Trong agentic workflows, tests không chỉ dùng để phát hiện lỗi hồi quy (regressions) mà còn đóng vai trò định nghĩa hành vi chấp nhận được của hệ thống. Một chiến lược testing theo nhiều lớp (layered) đặc biệt hiệu quả:
- **Unit tests** kiểm tra domain logic một cách độc lập và chạy rất nhanh, phù hợp cho các vòng lặp iteration liên tục do AI thực hiện.
- **Contract tests** xác minh rằng các service tuân thủ đúng interface đã thỏa thuận, giúp phát hiện sớm các thay đổi phá vỡ (breaking changes).
- **Smoke tests** chạy trên môi trường đã deploy để phát hiện các vấn đề về cấu hình hoặc quyền truy cập chỉ xuất hiện khi runtime, chẳng hạn như thiếu quyền trong [AWS Identity và Access Management](https://aws.amazon.com/vi/iam/) (IAM). Các tests được viết tốt cũng đóng vai trò như tài liệu. Khi một test thất bại, AI agent có thể suy ra hành vi mong đợi và điều chỉnh lại thay đổi của mình cho phù hợp.

**Lưu ý:** Tests cung cấp cơ chế validation nhanh, khách quan cho code do AI tạo ra và giúp giảm rủi ro các lỗi tích hợp khó phát hiện.

### Monorepo và tài liệu có thể đọc bởi máy (machine-readable documentation)

AI agents hoạt động hiệu quả hơn khi có ngữ cảnh rộng (broad context). Một monorepo cho phép agent di chuyển xuyên suốt các service, hiểu các pattern dùng chung và đánh giá tác động của thay đổi trên toàn hệ thống. Trong repository đó, tài liệu cần ngắn gọn, có cấu trúc rõ ràng. Các file như AGENT.md có thể mô tả nguyên tắc và ràng buộc kiến trúc, trong khi RUNBOOK.md và CONTRIBUTING.md trình bày quy trình vận hành và phát triển. Những định dạng machine-readable như YAML hoặc các file cấu hình sẽ dễ dàng cho agent phân tích hơn so với văn bản dài.

Kiro cũng có thể sử dụng các [foundational steering documents](https://kiro.dev/docs/cli/steering/#foundational-steering-files)-tóm tắt về cấu trúc, công nghệ và guideline sản phẩm—để giúp agent duy trì nhận thức bối cảnh (situational awareness) khi dự án phát triển.

**Lưu ý:** Việc chia sẻ ngữ cảnh đầy đủ giúp nâng cao chất lượng code do AI tạo ra và giảm nhu cầu chỉnh sửa thủ công.

### Tích hợp AI agents một cách an toàn vào delivery pipelines

Khi AI agents ngày càng trở nên mạnh mẽ hơn, governance vẫn là yếu tố không thể thiếu. Các pipeline CI/CD (continuous integration và continuous delivery) cần được thiết lập với các guardrails như bắt buộc chạy test, review tự động và cơ chế bảo vệ branch. Theo thời gian, khi mức độ tin cậy tăng lên, bạn có thể mở rộng quyền tự chủ của agent, đồng thời vẫn giữ con người trong vòng kiểm soát đối với các quyết định có tác động lớn. Cách tiếp cận cân bằng này cho phép AI tăng tốc các công việc lặp lại mà không làm gia tăng rủi ro vận hành.

## Kết luận

Agentic AI development không thể thành công một cách ngẫu nhiên. Nó đòi hỏi các kiến trúc được thiết kế với trọng tâm là feedback nhanh, ranh giới rõ ràng và ý đồ tường minh (explicit intent). Việc kết hợp local emulation, kiểm thử cloud nhẹ, và preview environments cùng với cấu trúc domain-driven, chiến lược testing nhiều lớp và tài liệu machine-readable sẽ tạo ra một môi trường nơi AI agents có thể hoạt động hiệu quả và an toàn. Các công cụ như Kiro giúp thu hẹp khoảng cách giữa các quyết định thiết kế của con người và việc thực thi tự động của AI. Khi kiến trúc được thiết kế phù hợp với agentic workflows, AI agents sẽ trở thành force multiplier thực sự—xử lý các vòng lặp phát triển nhanh chóng, trong khi đội ngũ của bạn có thể tập trung vào thiết kế cấp cao và đổi mới.

Để tìm hiểu thêm về cách AWS có thể hỗ trợ tổ chức của bạn triển khai các giải pháp agentic, hãy truy cập [AWS Agentic AI](https://aws.amazon.com/vi/ai/agentic-ai/).

## Đóng góp và Feedback

Bài dịch này được thực hiện trong khuôn khổ **FCAJ Internship Program**. 

**Liên hệ:** ducmin76@gmail.com

**Feedback:** Mọi góp ý để cải thiện chất lượng dịch thuật xin gửi về email trên

**Updates:** Bài dịch sẽ được cập nhật dựa trên feedback từ cộng đồng

*© 2026 - Bản dịch thuộc về Trần Nguyễn Đức Minh. Vui lòng credit khi sử dụng.*
