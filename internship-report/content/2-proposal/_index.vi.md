---
title : "Bản đề xuất dự án"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre: <b> 2. </b>
---

# Production Access Request Portal

## Mục lục

- [Production Access Request Portal](#production-access-request-portal)
  - [Mục lục](#mục-lục)
  - [Tổng quan dự án](#tổng-quan-dự-án)
    - [Tóm tắt](#tóm-tắt)
    - [Đối tượng sử dụng](#đối-tượng-sử-dụng)
    - [Nguyên tắc thiết kế](#nguyên-tắc-thiết-kế)
  - [Vấn đề cần giải quyết](#vấn-đề-cần-giải-quyết)
    - [Mô tả vấn đề](#mô-tả-vấn-đề)
    - [Giải pháp](#giải-pháp)
    - [So sánh hai kiến trúc](#so-sánh-hai-kiến-trúc)
  - [Kiến trúc giải pháp](#kiến-trúc-giải-pháp)
    - [Sơ đồ kiến trúc hệ thống](#sơ-đồ-kiến-trúc-hệ-thống)
    - [Các dịch vụ AWS được sử dụng](#các-dịch-vụ-aws-được-sử-dụng)
    - [Thành phần hệ thống](#thành-phần-hệ-thống)
      - [Jira Service Management Portal](#jira-service-management-portal)
      - [API Gateway Layer](#api-gateway-layer)
      - [Compute Layer - AWS Lambda](#compute-layer---aws-lambda)
        - [Lambda Executor](#lambda-executor)
        - [Lambda Expiry](#lambda-expiry)
        - [Lambda Email Approval](#lambda-email-approval)
      - [Data Layer - DynamoDB](#data-layer---dynamodb)
        - [AccessSessions Table](#accesssessions-table)
        - [ApprovalTokens Table](#approvaltokens-table)
      - [Identity Management Layer - AWS IAM Identity Center](#identity-management-layer---aws-iam-identity-center)
      - [Secrets Management Layer - AWS Secrets Manager](#secrets-management-layer---aws-secrets-manager)
      - [Notification Layer - Amazon SES](#notification-layer---amazon-ses)
      - [Monitoring \& Audit Layer - CloudWatch và CloudTrail](#monitoring--audit-layer---cloudwatch-và-cloudtrail)
    - [Quy trình vận hành](#quy-trình-vận-hành)
  - [Lộ trình \& Mốc triển khai](#lộ-trình--mốc-triển-khai)
  - [Ước tính ngân sách](#ước-tính-ngân-sách)
  - [Đánh giá rủi ro](#đánh-giá-rủi-ro)
  - [Kết quả kỳ vọng](#kết-quả-kỳ-vọng)

## Tổng quan dự án

### Tóm tắt

Production Access Request Portal là một hệ thống quản lý quyền truy cập tạm thời (Just-in-Time Access) được xây dựng trên kiến trúc serverless hoàn toàn (AWS Lambda, API Gateway, DynamoDB), giúp kiểm soát việc truy cập vào các tài khoản AWS Production một cách tự động và bảo mật. Quy trình bắt đầu khi người dùng gửi yêu cầu qua Jira Service Management, sau đó hệ thống sẽ điều phối việc phê duyệt qua email bằng Amazon SES và sử dụng AWS IAM Identity Center để cấp quyền. Điểm cải tiến vượt trội của phiên bản v2.0 chính là cơ chế Group-Based Access, cho phép hệ thống tự động thu hồi quyền truy cập (nằm trong Access Groups) chỉ trong vòng 60 giây ngay khi hết thời gian hiệu lực (TTL), khắc phục hoàn toàn giới hạn trễ lên đến 12 giờ của các phương thức truyền thống. Toàn bộ hoạt động đều được giám sát chặt chẽ qua CloudWatch và CloudTrail, đảm bảo tính minh bạch và tuân thủ tuyệt đối cho môi trường vận hành nhạy cảm.

### Đối tượng sử dụng

Trong hệ thống Production Access Request Portal có nhiều đối tượng tham gia vào quá trình yêu cầu, phê duyệt, cấp phát và giám sát quyền truy cập vào môi trường Production. Mỗi vai trò đảm nhiệm một chức năng riêng, góp phần đảm bảo hệ thống vận hành an toàn, hiệu quả và tuân thủ các chính sách bảo mật.

| Vai trò | Mô tả | Tương tác với hệ thống |
|---------|-------|------------------------|
| End User (Developer/Engineer) | Người có nhu cầu truy cập vào môi trường Production để thực hiện các công việc như triển khai (deploypment), xử lý sự cố (troubleshooting) hoặc kiểm tra hệ thống. | Người dùng tương tác với hệ thống thông qua Jira Service Management Portal để gửi yêu cầu truy cập. Sau khi được phê duyệt, họ sử dụng AWS IAM Identity Center để đăng nhập và nhận quyền truy cập tạm thời. |
| Approver (Team Lead/Manager) | Người chịu trách nhiệm xem xét và phê duyệt các yêu cầu truy cập Production. Đóng vai trò kiểm soát, đảm bảo rằng chỉ những yêu cầu hợp lệ và cần thiết mới được cấp quyền. | Việc phê duyệt được thực hiện trực tiếp trên giao diện Jira hoặc thông qua email với các liên kết xác nhận (approve/decline).|
| Platform/DevOps Engineer | Người chịu trách nhiệm thiết kế, triển khai và vận hành hệ thống. | Quản lý toàn bộ hạ tầng thông qua Terraform (Infrastructure as Code), phát triển và bảo trì các Lambda functions cũng như giám sát hiệu năng hệ thống, xử lý sự cố và thực hiện các cải tiến kiến trúc khi cần thiết để đảm bảo hệ thống hoạt động ổn định. |
| Security/Compliance Team | Người chịu trách nhiệm đảm bảo hệ thống tuân thủ các tiêu chuẩn và chính sách bảo mật của tổ chức. | Theo dõi các hoạt động truy cập thông qua audit logs, kiểm tra các chính sách phân quyền và đánh giá rủi ro bảo mật. |

### Nguyên tắc thiết kế

Hệ thống Production Access Request Portal được thiết kế dựa trên các nguyên tắc bảo mật và kiến trúc hiện đại nhằm đảm bảo việc quản lý truy cập vào môi trường Production được thực hiện một cách an toàn, minh bạch và hiệu quả. Các nguyên tắc cốt lõi bao gồm:

- **Zero Standing Privileges:** 
  - Nguyên tắc này quy định rằng hệ thống không cho phép tồn tại bất kỳ quyền truy cập Production nào mang tính lâu dài hoặc thường trực. Thay vào đó thì mọi quyền truy cập đều phải được yêu cầu thông qua hệ thống, trải qua quy trình phê duyệt và chỉ tồn tại trong một khoảng thời gian giới hạn. 
  - Sau khi hết thời gian được cấp, quyền truy cập sẽ tự động bị thu hồi mà không cần sự can thiệp thủ công. Cách tiếp cận này giúp giảm thiểu đáng kể nguy cơ lộ thông tin xác thực (credentials) cũng như hạn chế khả năng lạm dụng quyền truy cập trong hệ thống.
- **Least Privilege:** 
  - Nguyên tắc đảm bảo rằng mỗi người dùng chỉ được cấp quyền tối thiểu cần thiết để thực hiện nhiệm vụ của mình. Hệ thống định nghĩa ba cấp độ quyền truy cập chính bao gồm ReadOnly, PowerUser và Admin tương ứng với các mức độ thao tác khác nhau trên tài nguyên AWS.
  - Mỗi cấp quyền được ánh xạ tới các chính sách (policies) cụ thể, đồng thời được kiểm soát thông qua các cơ chế giới hạn phạm vi truy cập. Nhờ đó mà hệ thống giảm thiểu rủi ro trong trường hợp tài khoản bị xâm phạm, đồng thời ngăn chặn các hành vi thao tác vượt quá quyền hạn cho phép.
- **Defense in Depth:**
  - Nguyên tắc được áp dụng nhằm xây dựng nhiều lớp bảo mật chồng lấn trong toàn bộ hệ thống. Cụ thể, hệ thống triển khai xác thực người dùng thông qua SSO tại Jira Portal, kết hợp với cơ chế xác thực API Gateway bằng API key và giới hạn tốc độ truy cập (throttling).
  - Ngoài ra, các webhook được bảo vệ bằng chữ ký số HMAC-SHA256 nhằm đảm bảo tính toàn vẹn của dữ liệu. Hệ thống cũng áp dụng chính sách phân quyền chặt chẽ thông qua IAM cùng với cơ chế ghi log và giám sát toàn diện thông qua CloudWatch và CloudTrail.
  - Việc triển khai nhiều lớp bảo mật giúp đảm bảo rằng ngay cả khi một lớp bị vượt qua, các lớp còn lại vẫn có thể bảo vệ hệ thống khỏi các mối đe dọa.
- **Fail-Safe Defaults:** 
  - Nguyên tắc quy định rằng hệ thống luôn mặc định từ chối mọi yêu cầu truy cập nếu không có đủ điều kiện hợp lệ. Quyền truy cập chỉ được cấp khi tất cả các bước trong quy trình bao gồm xác thực, phê duyệt và cấp phát quyền đều được thực hiện thành công.
  - Trong trường hợp xảy ra lỗi hoặc không xác định được trạng thái hợp lệ hệ thống sẽ không cấp quyền truy cập. Điều này giúp đảm bảo rằng không có quyền truy cập nào được cấp một cách ngoài ý muốn hoặc không kiểm soát.
- **Complete Auditability:** Nguyên tắc đảm bảo rằng mọi hoạt động trong hệ thống đều được ghi nhận và có thể truy vết đầy đủ. Thông tin về các yêu cầu truy cập, quá trình phê duyệt, cấp quyền và thu hồi quyền đều được lưu trữ tại nhiều hệ thống như Jira, CloudWatch, CloudTrail và DynamoDB. Nhờ vậy mà hệ thống có khả năng cung cấp đầy đủ thông tin phục vụ cho việc kiểm toán, tuân thủ các quy định bảo mật, cũng như hỗ trợ điều tra khi xảy ra sự cố. Việc đảm bảo tính minh bạch và truy vết này là yếu tố quan trọng trong các hệ thống quản lý truy cập Production.

## Vấn đề cần giải quyết

### Mô tả vấn đề

Trước khi hệ thống này ra đời, quy trình quản lý quyền truy cập vào môi trường Production trong tổ chức còn tồn tại nhiều hạn chế, tiềm ẩn rủi ro cao về bảo mật và gây khó khăn trong vận hành. Các vấn đề chính được xác định như sau:

| Vấn đề | Tác động | Mức độ rủi ro |
|--------|----------|:-------------:|
| Cấp quyền thủ công và thiếu nhất quán | Quy trình cấp quyền truy cập Production chủ yếu được thực hiện thủ công, thông qua việc tạo hoặc chỉnh sửa IAM User cho từng yêu cầu. Cách làm này không chỉ tốn nhiều thời gian mà còn dễ xảy ra sai sót trong quá trình cấu hình, dẫn đến việc cấp sai quyền hoặc thiếu kiểm soát. Việc thiếu một quy trình chuẩn hóa cũng khiến cho các thao tác giữa các nhóm không đồng nhất, gây khó khăn trong quản lý và mở rộng hệ thống. | Cao |
| Không có cơ chế tự động hết hạn quyền truy cập | Một trong những vấn đề nghiêm trọng là quyền truy cập Production không được tự động thu hồi sau khi hoàn thành công việc. Điều này dẫn đến việc tồn tại các credentials lâu dài (long-lived credentials) trong hệ thống. Việc duy trì quyền truy cập trong thời gian dài làm tăng nguy cơ bị khai thác nếu thông tin xác thực bị lộ, đồng thời vi phạm các nguyên tắc bảo mật hiện đại như Zero Standing Privileges. | Rất cao |
| Thiếu khả năng truy vết và kiểm toán (Audit Trail) | Hệ thống cũ không cung cấp đầy đủ thông tin để theo dõi và truy vết các hoạt động truy cập. Việc không biết chính xác ai đã truy cập, vào thời điểm nào và thực hiện hành động gì gây khó khăn trong quá trình kiểm toán và điều tra sự cố. Điều này đặc biệt nghiêm trọng trong các môi trường yêu cầu tuân thủ cao, nơi mà audit trail là bắt buộc. | Rất cao |
| Quy trình phê duyệt rời rạc và kém hiệu quả | Quy trình phê duyệt truy cập thường diễn ra thông qua nhiều kênh khác nhau như email, tin nhắn hoặc trao đổi trực tiếp, thiếu sự tập trung và tự động hóa. Điều này làm cho thời gian xử lý yêu cầu kéo dài, không có SLA rõ ràng hay khó theo dõi trạng thái yêu cầu hiện tại. Hệ quả là làm giảm hiệu suất làm việc của đội ngũ kỹ thuật và làm tăng độ trễ trong xử lý sự cố Production. | Trung bình |
| Không có cơ chế thu hồi quyền khẩn cấp | Trong trường hợp phát hiện rủi ro bảo mật hoặc tài khoản bị xâm phạm, hệ thống không cung cấp khả năng thu hồi quyền truy cập ngay lập tức. Việc này khiến tổ chức không thể phản ứng nhanh với các sự cố, làm gia tăng mức độ ảnh hưởng và thiệt hại tiềm ẩn. | Nghiêm trọng |

### Giải pháp

Để khắc phục các hạn chế trong quy trình quản lý truy cập Production, hệ thống Production Access Request Portal được xây dựng như một giải pháp tự động hóa toàn diện, áp dụng các nguyên tắc bảo mật hiện đại và tận dụng kiến trúc serverless trên nền tảng AWS. Các giải pháp chính như sau:
- **Tự động hóa quy trình cấp quyền truy cập:** 
  - Thay vì thực hiện thủ công, hệ thống chuẩn hóa và tự động hóa toàn bộ quy trình cấp quyền theo mô hình:
    ```
    Request → Approval → Provisioning → Expiry → Revocation
    ```
  - Người dùng gửi yêu cầu thông qua Jira Service Management, sau đó hệ thống tự động xử lý các bước tiếp theo thông qua API Gateway và AWS Lambda. Việc này giúp giảm thiểu sai sót do con người, đồng thời đảm bảo tính nhất quán trong toàn bộ hệ thống.
- **Áp dụng cơ chế Just-in-Time Access:** 
  - Hệ thống triển khai mô hình Just-in-Time (JIT) Access, trong đó quyền truy cập chỉ được cấp khi cần thiết và tồn tại trong một khoảng thời gian giới hạn.
  - Mỗi yêu cầu truy cập đều được gắn với một thời gian cụ thể (duration), sau đó được ánh xạ vào các bậc thời gian tiêu chuẩn (duration tiers). Khi hết thời gian, hệ thống tự động thu hồi quyền truy cập mà không cần thao tác thủ công.
  - Giải pháp này giúp loại bỏ hoàn toàn việc tồn tại các quyền truy cập lâu dài trong môi trường production.
- **Sử dụng Group-Based Access thay cho cấp quyền trực tiếp:** Một cải tiến quan trọng của hệ thống là chuyển từ mô hình cấp quyền trực tiếp Direct Assignment sang Group-Based Access.
  -  Thay vì gán quyền trực tiếp cho từng user, hệ thống sẽ:
     -  Tạo sẵn các access group tương ứng với từng loại quyền và thời gian.
     -  Thêm user vào group khi được cấp quyền.
     -  Xóa user khỏi group khi hết hạn.
  - Cách tiếp cận này giúp rút ngắn thời gian cấp quyền xuống dưới 5 giây, thu hồi quyền gần như ngay lập tức, giảm đáng kể độ phức tạp trong quản lý.
- **Tích hợp cơ chế phê duyệt tập trung và linh hoạt:** Hệ thống tích hợp chặt chẽ với Jira Service Management để quản lý toàn bộ quy trình phê duyệt. Ngoài ra còn hỗ trợ phê duyệt thông qua email với các liên kết xác nhận (approve/decline) giúp tăng tính linh hoạt cho người quản lý. Tất cả các yêu cầu đều được theo dõi trạng thái rõ ràng, đảm bảo minh bạch và có thể kiểm soát theo SLA.
- **Thiết lập cơ chế thu hồi quyền tự động và khẩn cấp:** Hệ thống sử dụng DynamoDB TTL kết hợp với Lambda để tự động phát hiện và thu hồi quyền khi hết hạn. Khi một session hết hạn, Lambda sẽ tự động xóa user khỏi access group, khiến credentials bị vô hiệu hóa trong vòng 60 giây. Hệ thống cũng hỗ trợ cơ chế thu hồi khẩn cấp (Emergency Revocation), cho phép xóa toàn bộ quyền truy cập của một user trong thời gian rất ngắn khi phát hiện rủi ro bảo mật.
- **Tăng cường khả năng giám sát và kiểm toán:** Để giải quyết vấn đề thiếu Audit Trail, hệ thống ghi nhận toàn bộ hoạt động tại nhiều lớp:
  - Jira: Lưu trữ request và approval.
  - CloudWatch: Log thực thi hệ thống.
  - CloudTrail: Ghi nhận các API call trên AWS.
  - DynamoDB: Lưu metadata của session.
- **Áp dụng kiến trúc serverless để tối ưu vận hành:** Hệ thống được xây dựng hoàn toàn trên kiến trúc serverless với các dịch vụ như AWS Lambda, API Gateway và DynamoDB giúp:
  - Tự động mở rộng theo nhu cầu sử dụng.
  - Không cần quản lý hạ tầng máy chủ.
  - Tối ưu chi phí vận hành (pay-per-use).
  - Tăng độ tin cậy và khả năng sẵn sàng của hệ thống.

### So sánh hai kiến trúc

| Tiêu chí | Direct Assignment (v1.0) | Group-Based Access (v2.0) |
|----------|:------------------------:|:-------------------------:|
| Cơ chế cấp quyền | Gán trực tiếp user vào account | Thêm user vào group đã được gán sẵn |
| Thời gian cấp quyền | 15-30 giây (async operation) | < 5 giây (sync operation, gần như real-time) |
| Thời gian thu hồi quyền | Tối đa 12 giờ | Trong vòng 60 giây |
| Hiệu lực credentials sau revoke| Vẫn còn hiệu lực trong một khoảng thời gian | Bị vô hiệu hóa gần như ngay lập tức |
| Độ phức tạp vận hành | Cao - Phải tạo/xóa assignment mỗi lần | Thấp - chỉ quản lý membership |
| Khả năng mở rộng | Hạn chế khi số lượng user tăng | Dễ mở rộng nhờ reuse group |
| Phản ứng sự cố bảo mật | Chậm, khó kiểm soát | Nhanh, có thể revoke hàng loạt |

## Kiến trúc giải pháp

### Sơ đồ kiến trúc hệ thống

<img src="/images/figure-1-proposal.png" alt="figure-1-proposal" style="width:600px !important; max-width:900px !important;">
<p style="text-align:center; font-style:italic;">Hình 1. Sơ đồ kiến trúc hệ thống Production Access Request Portal</p>

### Các dịch vụ AWS được sử dụng

| Dịch vụ AWS | Vai trò trong hệ thống | Lý do lựa chọn |
|-------------|------------------------|----------------|
| AWS Lambda | Xử lý toàn bộ logic nghiệp vụ của hệ thống như provisioning access, revoke, xử lý email approval và auto expiry. | Kiến trúc serverless giúp không cần quản lý server, auto scaling, chi phí thấp theo số lần chạy (pay-per-use), phù hợp workload event-driven. |
| Amazon API Gateway | Cung cấp REST API endpoint để nhận webhook từ Jira. | Managed API service có hỗ trợ authentication, throttling, rate limit, usage plans và tích hợp native với Lambda. |
| AWS IAM Identity Center | Quản lý quyền truy cập AWS thông qua Permission Sets, Access Groups và Group Memberships. Cấp phát và thu hồi quyền truy cập nhanh chóng (group-based). | Đây là giải pháp SSO chính thức của AWS, hỗ trợ centralized access management và revoke credentials gần như tức thời ( trong vòng 60s). |
| Amazon DynamoDB | Lưu trữ session metadata (Sessions Table), approval tokens (Approval Tokens Table), hỗ trợ TTL auto-expiry và trigger revoke workflow thông qua DynamoDB Streams. | NoSQL serverless có độ trễ thấp, hỗ trợ TTL auto-expiry và DynamoDB Streams. |
| AWS Secrets Manager | Lưu trữ bảo mật Jira credentials, webhook API key, token secret và access group mapping. | Bảo mật secrets tốt hơn hardcode/config file, có encryption at rest và automatic rotation support. |
| Amazon SES | Gửi email approval, email notification khi cấp quyền và revoke quyền. | Chi phí thấp, dễ tích hợp với Lambda. |
| Amazon CloudWatch | Logging (structured JSON logs), monitoring metrics, alarming cho Lambda, API Gateway, DynamoDB... | Native integration với Lambda và các dịch vụ AWS khác. |
| AWS CloudTrail | Ghi audit trail cho tất cả API calls liên quan đến Identity Center, Lambda và các thao tác cấp quyền. | Đáp ứng compliance requirements (đảm bảo complete auditability). |

### Thành phần hệ thống

Hệ thống Production Access Request Portal được tổ chức theo mô hình nhiều lớp, trong đó mỗi thành phần đảm nhận một vai trò riêng nhưng phối hợp chặt chẽ để tạo thành một quy trình cấp quyền truy cập Production hoàn toàn tự động, có kiểm soát và có khả năng kiểm toán đầy đủ. Về mặt logic, hệ thống bao gồm các nhóm thành phần chính sau: giao diện yêu cầu (Jira Service Management), lớp tiếp nhận API, lớp xử lý nghiệp vụ bằng Lambda, lớp lưu trữ dữ liệu, lớp quản lý danh tính và quyền truy cập, lớp gửi thông báo, và lớp giám sát/vận hành. Thiết kế này phù hợp với kiến trúc serverless đã mô tả trong tài liệu, đồng thời hỗ trợ cơ chế Group-Based Access cho việc cấp và thu hồi quyền gần như theo thời gian thực.

| Thành phần | Chức năng chính | Vai trò trong luồng xử lý |
|------------|-----------------|---------------------------|
| Jira Service Management Portal | Tiếp nhận yêu cầu truy cập từ người dùng, hiển thị trạng thái request, workflow phê duyệt | Điểm bắt đầu của toàn bộ quy trình |
| API Gateway | Tiếp nhận webhook từ Jira, xác thực request và chuyển tiếp vào Lambda | Cửa vào của backend serverless |
| Lambda Executor | Xử lý provisioning access sau khi request được duyệt | Thực thi logic cấp quyền chính |
| Lambda Email Approval | Tạo email phê duyệt, xử lý approve/decline từ email | Hỗ trợ phê duyệt linh hoạt qua email |
| Lambda Expiry | Theo dõi session hết hạn và tự động thu hồi quyền | Đảm bảo quyền truy cập chỉ tồn tại trong thời gian cho phép |
| DynamoDB | Lưu session, approval token, TTL và metadata phục vụ revoke | Nền tảng lưu trữ trạng thái hệ thống |
| Secrets Manager | Lưu secrets, mapping group, token secret, Jira credentials | Bảo vệ dữ liệu nhạy cảm |
| IAM Identity Center | Quản lý permission sets, access groups, group membership | Thành phần cấp phát và thu hồi quyền truy cập |
| Amazon SES | Gửi email approval, notification, expiry alert | Kênh giao tiếp với approver và requester |
| CloudWatch/CloudTrail | Ghi log, metrics, audit trail | Giám sát, truy vết và tuân thủ |

#### Jira Service Management Portal

Jira Service Management Portal là lớp giao tiếp chính giữa người dùng và hệ thống. Người dùng khởi tạo yêu cầu truy cập Production thông qua form chuẩn hóa, trong đó các thông tin như tài khoản AWS, loại quyền truy cập và thời gian yêu cầu được ghi nhận và chuyển thành ticket. Jira đồng thời đóng vai trò quản lý vòng đời của request: từ trạng thái khởi tạo, chờ phê duyệt, đã cấp quyền, đến hết hạn hoặc bị thu hồi.

Ngoài ra, Jira còn là nguồn phát sinh webhook để kích hoạt các quy trình tự động phía AWS. Khi một request được phê duyệt, Jira sẽ gọi sang API Gateway để khởi động Lambda Executor. Khi request bị từ chối hoặc hết hạn, Jira cũng được cập nhật trạng thái tương ứng để đảm bảo người dùng và đội vận hành luôn theo dõi được tình trạng thực tế của request.

#### API Gateway Layer

API Gateway đóng vai trò là lớp tiếp nhận và bảo vệ các endpoint công khai của hệ thống. Trong kiến trúc này, API Gateway không chỉ nhận webhook từ Jira mà còn cung cấp các endpoint phục vụ email approval flow. Mỗi request đi vào đều phải được xác thực bằng API key và chịu giới hạn bởi throttling, usage plan và quota để giảm nguy cơ bị lạm dụng hoặc gửi request bất thường.

Các endpoint chính bao gồm:
- POST /provision-access: kích hoạt Lambda Executor sau khi request được duyệt.
- POST /email-approval/request: tạo luồng gửi email phê duyệt.
- GET /email-approval/action: xử lý hành động approve/decline từ email.

Thiết kế này giúp toàn bộ luồng tự động hóa được gom về một lớp truy cập thống nhất, dễ kiểm soát, dễ áp dụng bảo mật và dễ quan sát log vận hành.

#### Compute Layer - AWS Lambda

Compute Layer là trung tâm xử lý nghiệp vụ của hệ thống và được triển khai bằng ba Lambda functions chính. Tất cả đều sử dụng Python 3.12 runtime và chia sẻ một Lambda Layer chung để tái sử dụng logic dùng chung, giảm trùng lặp mã nguồn và rút ngắn thời gian deploy.

##### Lambda Executor

Lambda Executor là function quan trọng nhất của hệ thống. Function này tiếp nhận webhook từ Jira, xác thực request, ánh xạ duration sang duration tier, tra cứu group phù hợp trong mapping, thêm user vào access group của IAM Identity Center, sau đó ghi session vào DynamoDB và cập nhật trạng thái Jira.

Vai trò của Lambda Executor gồm:
- Xác thực webhook và kiểm tra dữ liệu đầu vào.
- Tìm user theo email trong Identity Store.
- Thêm user vào access group tương ứng.
- Ghi nhận session để phục vụ TTL và revoke.
- Gửi notification cho requester sau khi cấp quyền thành công.

##### Lambda Expiry

Lambda Expiry được kích hoạt từ DynamoDB Stream khi record session bị TTL xóa. Đây là cơ chế cốt lõi bảo đảm hệ thống tự động thu hồi quyền truy cập ngay khi phiên làm việc hết hạn. Function này đọc dữ liệu cũ từ stream, xác định membership cần xóa, thực hiện remove khỏi group trong IAM Identity Center và cập nhật trạng thái request trong Jira.

Cơ chế này tạo ra lợi thế lớn so với mô hình Direct Assignment trước đây vì quyền truy cập không còn phụ thuộc vào quá trình revoke thủ công hoặc đồng bộ chậm. Theo thiết kế trong tài liệu, credentials có thể bị vô hiệu hóa trong vòng khoảng 60 giây sau khi session hết hạn.

##### Lambda Email Approval

Lambda Email Approval phục vụ quy trình phê duyệt qua email. Function này tạo token có chữ ký HMAC-SHA256, lưu metadata của token vào DynamoDB với TTL, render email HTML chứa nút Approve/Decline và gửi qua SES đến approver. Khi approver click vào link, function sẽ kiểm tra token, xác nhận tính hợp lệ, đánh dấu token đã dùng và gọi Jira Approval API tương ứng.

#### Data Layer - DynamoDB

DynamoDB là lớp lưu trữ trạng thái của toàn bộ hệ thống. Trong tài liệu, hệ thống sử dụng ít nhất hai bảng chính: AccessSessions và ApprovalTokens. Đây là lựa chọn phù hợp với kiến trúc serverless vì có độ trễ thấp, hỗ trợ TTL và tích hợp tốt với Streams để kích hoạt luồng revoke tự động.

##### AccessSessions Table

Bảng này lưu toàn bộ metadata của một session truy cập, bao gồm requestId, requester, account, access type, duration, group membership id, thời điểm cấp quyền, thời điểm hết hạn và trạng thái hiện tại. Trường ttl được dùng để DynamoDB tự động xóa record khi session kết thúc. Khi record bị xóa, stream event sẽ kích hoạt Lambda Expiry để thu hồi quyền.

##### ApprovalTokens Table

Bảng này dùng để quản lý token cho workflow phê duyệt email. Mỗi token là single-use, có thời hạn sống ngắn và được đánh dấu used ngay sau khi được kích hoạt. Cách làm này giúp hệ thống chống replay attack và tránh việc một liên kết phê duyệt bị dùng lại nhiều lần.

#### Identity Management Layer - AWS IAM Identity Center

IAM Identity Center là nơi thực thi quyền truy cập thực tế vào các AWS target accounts. Hệ thống không cấp quyền trực tiếp cho từng user theo kiểu truyền thống mà sử dụng mô hình group-based access. Mỗi group tương ứng với một tổ hợp account + access_type + duration_tier, từ đó ánh xạ tới permission set phù hợp. Khi user được thêm vào group, quyền được cấp gần như ngay lập tức; khi bị xóa khỏi group, credentials sẽ bị vô hiệu hóa trong thời gian rất ngắn.

Thành phần này gồm ba khối logic:
- Permission Sets: định nghĩa mức quyền như ReadOnly, PowerUser, Admin.
- Access Groups: nhóm người dùng đại diện cho một tổ hợp quyền cụ thể.
- Group Assignments: gán group vào target account và permission set tương ứng.

Thiết kế này giúp giảm số lượng thao tác quản trị, dễ mở rộng khi thêm tài khoản mới và đặc biệt phù hợp với mục tiêu revoke gần như tức thời của hệ thống.

#### Secrets Management Layer - AWS Secrets Manager

Secrets Manager được sử dụng để lưu trữ các thông tin nhạy cảm như Jira credentials, API key của webhook, token secret và access group mapping. Việc dùng Secrets Manager thay cho hardcode hoặc file cấu hình giúp giảm nguy cơ lộ bí mật và hỗ trợ kiểm soát truy cập tốt hơn.

Đặc biệt, mapping giữa account, access type và duration tier được lưu trong Secrets Manager để Lambda Executor có thể tra cứu group đúng theo request. Theo thiết kế trong tài liệu, Lambda còn áp dụng cache ngắn hạn để giảm số lần truy vấn secret, từ đó tối ưu hiệu năng và chi phí.

#### Notification Layer - Amazon SES

Amazon SES là kênh thông báo chính của hệ thống. Thành phần này chịu trách nhiệm gửi email phê duyệt đến approver, gửi email xác nhận cấp quyền cho requester và gửi cảnh báo khi access sắp hết hạn hoặc đã hết hạn. Việc tách notification ra khỏi logic chính giúp quy trình cấp quyền không phụ thuộc hoàn toàn vào email, đồng thời vẫn duy trì được tính minh bạch cho người dùng cuối.

Trong email approval flow, SES còn đóng vai trò truyền tải các liên kết approve/decline đã được ký và có TTL. Điều này giúp approver có thể xử lý nhanh mà không cần vào Jira, nhưng vẫn đảm bảo token được xác thực ở phía backend trước khi thực hiện thay đổi trạng thái request.

#### Monitoring & Audit Layer - CloudWatch và CloudTrail

Hệ thống đặt yêu cầu rất cao về giám sát và kiểm toán, do đó CloudWatch và CloudTrail được sử dụng như lớp quan sát bắt buộc. CloudWatch đảm nhiệm việc ghi log thực thi của Lambda, theo dõi metric và kích hoạt cảnh báo khi có lỗi, timeout hoặc bất thường về lưu lượng. CloudTrail ghi nhận các API call liên quan đến Identity Center và những thao tác quản trị AWS để phục vụ audit trail.

Sự kết hợp giữa CloudWatch, CloudTrail, Jira và DynamoDB giúp hệ thống đạt được mức truy vết đầy đủ từ lúc request được tạo, phê duyệt, cấp quyền, sử dụng, cho đến khi bị thu hồi hoặc hết hạn. Đây là nền tảng quan trọng để đáp ứng các yêu cầu tuân thủ và điều tra sự cố sau này.

### Quy trình vận hành

## Lộ trình & Mốc triển khai

## Ước tính ngân sách

## Đánh giá rủi ro

## Kết quả kỳ vọng
