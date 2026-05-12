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
    - [Quy trình vận hành](#quy-trình-vận-hành)
      - [Quy trình cấp quyền tiêu chuẩn (Standard Grant Flow)](#quy-trình-cấp-quyền-tiêu-chuẩn-standard-grant-flow)
      - [Quy trình phê duyệt qua email (Email Approval Flow)](#quy-trình-phê-duyệt-qua-email-email-approval-flow)
      - [Quy trình tự động hết hạn và thu hồi quyền (Auto Expiry Flow)](#quy-trình-tự-động-hết-hạn-và-thu-hồi-quyền-auto-expiry-flow)
      - [Quy trình thu hồi khẩn cấp (Emergency Revocation)](#quy-trình-thu-hồi-khẩn-cấp-emergency-revocation)
      - [Nguyên tắc vận hành và xử lý ngoại lệ](#nguyên-tắc-vận-hành-và-xử-lý-ngoại-lệ)
  - [Lộ trình \& Mốc triển khai](#lộ-trình--mốc-triển-khai)
    - [Giai đoạn 1: Phân tích yêu cầu và thiết kế giải pháp (Tuần 2-4)](#giai-đoạn-1-phân-tích-yêu-cầu-và-thiết-kế-giải-pháp-tuần-2-4)
    - [Giai đoạn 2: Xây dựng hạ tầng nền tảng (Tuần 5-6)](#giai-đoạn-2-xây-dựng-hạ-tầng-nền-tảng-tuần-5-6)
    - [Giai đoạn 3: Phát triển nghiệp vụ hệ thống (Tuần 7-8)](#giai-đoạn-3-phát-triển-nghiệp-vụ-hệ-thống-tuần-7-8)
    - [Giai đoạn 4: Kiểm thử và nghiệm thu (Tuần 9-10)](#giai-đoạn-4-kiểm-thử-và-nghiệm-thu-tuần-9-10)
    - [Giai đoạn 5: Triển khai Production và chuyển giao vận hành (Tuần 11)](#giai-đoạn-5-triển-khai-production-và-chuyển-giao-vận-hành-tuần-11)
    - [Bảng kế hoạch triển khai tổng quan](#bảng-kế-hoạch-triển-khai-tổng-quan)
    - [Kế hoạch mở rộng sau triển khai](#kế-hoạch-mở-rộng-sau-triển-khai)
  - [Ước tính ngân sách](#ước-tính-ngân-sách)
  - [Đánh giá rủi ro](#đánh-giá-rủi-ro)
    - [Ma trận đánh giá rủi ro](#ma-trận-đánh-giá-rủi-ro)
    - [Kế hoạch ứng phó sự cố](#kế-hoạch-ứng-phó-sự-cố)
  - [Kết quả kỳ vọng](#kết-quả-kỳ-vọng)

## Tổng quan dự án

### Tóm tắt

Production Access Request Portal là một hệ thống quản lý quyền truy cập tạm thời (Just-in-Time Access) được xây dựng trên kiến trúc serverless hoàn toàn (AWS Lambda, API Gateway, DynamoDB), giúp kiểm soát việc truy cập vào các tài khoản AWS Production một cách tự động và bảo mật. Quy trình bắt đầu khi người dùng gửi yêu cầu qua Jira Service Management, sau đó hệ thống sẽ điều phối việc phê duyệt qua email bằng Amazon SES và sử dụng AWS IAM Identity Center để cấp quyền. Điểm cải tiến vượt trội của phiên bản v2.0 chính là cơ chế Group-Based Access, cho phép hệ thống tự động thu hồi quyền truy cập (nằm trong Access Groups) chỉ trong vòng 60 giây ngay khi hết thời gian hiệu lực (TTL), khắc phục hoàn toàn giới hạn trễ lên đến 12 giờ của các phương thức truyền thống. Toàn bộ hoạt động đều được giám sát chặt chẽ qua CloudWatch và CloudTrail, đảm bảo tính minh bạch và tuân thủ tuyệt đối cho môi trường vận hành nhạy cảm.

### Đối tượng sử dụng

Trong hệ thống Production Access Request Portal có nhiều đối tượng tham gia vào quá trình yêu cầu, phê duyệt, cấp phát và giám sát quyền truy cập vào môi trường Production. Mỗi vai trò đảm nhiệm một chức năng riêng, góp phần đảm bảo hệ thống vận hành an toàn, hiệu quả và tuân thủ các chính sách bảo mật.

| Vai trò | Mô tả | Tương tác với hệ thống |
|---------|-------|------------------------|
| **End User (Developer/Engineer)** | Người có nhu cầu truy cập vào môi trường Production để thực hiện các công việc như triển khai (deploypment), xử lý sự cố (troubleshooting) hoặc kiểm tra hệ thống. | Người dùng tương tác với hệ thống thông qua Jira Service Management Portal để gửi yêu cầu truy cập. Sau khi được phê duyệt, họ sử dụng AWS IAM Identity Center để đăng nhập và nhận quyền truy cập tạm thời. |
| **Approver (Team Lead/Manager)** | Người chịu trách nhiệm xem xét và phê duyệt các yêu cầu truy cập Production. Đóng vai trò kiểm soát, đảm bảo rằng chỉ những yêu cầu hợp lệ và cần thiết mới được cấp quyền. | Việc phê duyệt được thực hiện trực tiếp trên giao diện Jira hoặc thông qua email với các liên kết xác nhận (approve/decline).|
| **Platform/DevOps Engineer** | Người chịu trách nhiệm thiết kế, triển khai và vận hành hệ thống. | Quản lý toàn bộ hạ tầng thông qua Terraform (Infrastructure as Code), phát triển và bảo trì các Lambda functions cũng như giám sát hiệu năng hệ thống, xử lý sự cố và thực hiện các cải tiến kiến trúc khi cần thiết để đảm bảo hệ thống hoạt động ổn định. |
| **Security/Compliance Team** | Người chịu trách nhiệm đảm bảo hệ thống tuân thủ các tiêu chuẩn và chính sách bảo mật của tổ chức. | Theo dõi các hoạt động truy cập thông qua audit logs, kiểm tra các chính sách phân quyền và đánh giá rủi ro bảo mật. |

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
| **Cấp quyền thủ công và thiếu nhất quán** | Quy trình cấp quyền truy cập Production chủ yếu được thực hiện thủ công, thông qua việc tạo hoặc chỉnh sửa IAM User cho từng yêu cầu. Cách làm này không chỉ tốn nhiều thời gian mà còn dễ xảy ra sai sót trong quá trình cấu hình, dẫn đến việc cấp sai quyền hoặc thiếu kiểm soát. Việc thiếu một quy trình chuẩn hóa cũng khiến cho các thao tác giữa các nhóm không đồng nhất, gây khó khăn trong quản lý và mở rộng hệ thống. | Cao |
| **Không có cơ chế tự động hết hạn quyền truy cập** | Một trong những vấn đề nghiêm trọng là quyền truy cập Production không được tự động thu hồi sau khi hoàn thành công việc. Điều này dẫn đến việc tồn tại các credentials lâu dài (long-lived credentials) trong hệ thống. Việc duy trì quyền truy cập trong thời gian dài làm tăng nguy cơ bị khai thác nếu thông tin xác thực bị lộ, đồng thời vi phạm các nguyên tắc bảo mật hiện đại như Zero Standing Privileges. | Rất cao |
| **Thiếu khả năng truy vết và kiểm toán (Audit Trail)** | Hệ thống cũ không cung cấp đầy đủ thông tin để theo dõi và truy vết các hoạt động truy cập. Việc không biết chính xác ai đã truy cập, vào thời điểm nào và thực hiện hành động gì gây khó khăn trong quá trình kiểm toán và điều tra sự cố. Điều này đặc biệt nghiêm trọng trong các môi trường yêu cầu tuân thủ cao, nơi mà audit trail là bắt buộc. | Rất cao |
| **Quy trình phê duyệt rời rạc và kém hiệu quả** | Quy trình phê duyệt truy cập thường diễn ra thông qua nhiều kênh khác nhau như email, tin nhắn hoặc trao đổi trực tiếp, thiếu sự tập trung và tự động hóa. Điều này làm cho thời gian xử lý yêu cầu kéo dài, không có SLA rõ ràng hay khó theo dõi trạng thái yêu cầu hiện tại. Hệ quả là làm giảm hiệu suất làm việc của đội ngũ kỹ thuật và làm tăng độ trễ trong xử lý sự cố Production. | Trung bình |
| **Không có cơ chế thu hồi quyền khẩn cấp** | Trong trường hợp phát hiện rủi ro bảo mật hoặc tài khoản bị xâm phạm, hệ thống không cung cấp khả năng thu hồi quyền truy cập ngay lập tức. Việc này khiến tổ chức không thể phản ứng nhanh với các sự cố, làm gia tăng mức độ ảnh hưởng và thiệt hại tiềm ẩn. | Nghiêm trọng |

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
| **Cơ chế cấp quyền** | Gán trực tiếp user vào account | Thêm user vào group đã được gán sẵn |
| **Thời gian cấp quyền** | 15-30 giây (async operation) | < 5 giây (sync operation, gần như real-time) |
| **Thời gian thu hồi quyền** | Tối đa 12 giờ | Trong vòng 60 giây |
| **Hiệu lực credentials sau revoke** | Vẫn còn hiệu lực trong một khoảng thời gian | Bị vô hiệu hóa gần như ngay lập tức |
| **Độ phức tạp vận hành** | Cao - Phải tạo/xóa assignment mỗi lần | Thấp - chỉ quản lý membership |
| **Khả năng mở rộng** | Hạn chế khi số lượng user tăng | Dễ mở rộng nhờ reuse group |
| **Phản ứng sự cố bảo mật** | Chậm, khó kiểm soát | Nhanh, có thể revoke hàng loạt |

## Kiến trúc giải pháp

### Sơ đồ kiến trúc hệ thống

<img src="/images/figure-proposal.png" alt="figure-proposal" style="width:600px !important; max-width:900px !important;">
<p style="text-align:center; font-style:italic;">Hình 1. Sơ đồ kiến trúc hệ thống Production Access Request Portal</p>

### Các dịch vụ AWS được sử dụng

| Dịch vụ AWS | Vai trò trong hệ thống | Lý do lựa chọn |
|-------------|------------------------|----------------|
| **AWS Lambda** | Xử lý toàn bộ logic nghiệp vụ của hệ thống như provisioning access, revoke, xử lý email approval và auto expiry. | Kiến trúc serverless giúp không cần quản lý server, auto scaling, chi phí thấp theo số lần chạy (pay-per-use), phù hợp workload event-driven. |
| **Amazon API Gateway** | Cung cấp REST API endpoint để nhận webhook từ Jira. | Managed API service có hỗ trợ authentication, throttling, rate limit, usage plans và tích hợp native với Lambda. |
| **AWS IAM Identity Center** | Quản lý quyền truy cập AWS thông qua Permission Sets, Access Groups và Group Memberships. Cấp phát và thu hồi quyền truy cập nhanh chóng (group-based). | Đây là giải pháp SSO chính thức của AWS, hỗ trợ centralized access management và revoke credentials gần như tức thời ( trong vòng 60s). |
| **Amazon DynamoDB** | Lưu trữ session metadata (Sessions Table), approval tokens (Approval Tokens Table), hỗ trợ TTL auto-expiry và trigger revoke workflow thông qua DynamoDB Streams. | NoSQL serverless có độ trễ thấp, hỗ trợ TTL auto-expiry và DynamoDB Streams. |
| **AWS Secrets Manager** | Lưu trữ bảo mật Jira credentials, webhook API key, token secret và access group mapping. | Bảo mật secrets tốt hơn hardcode/config file, có encryption at rest và automatic rotation support. |
| **Amazon SES** | Gửi email approval, email notification khi cấp quyền và revoke quyền. | Chi phí thấp, dễ tích hợp với Lambda. |
| **Amazon CloudWatch** | Logging (structured JSON logs), monitoring metrics, alarming cho Lambda, API Gateway, DynamoDB... | Native integration với Lambda và các dịch vụ AWS khác. |
| **AWS CloudTrail** | Ghi audit trail cho tất cả API calls liên quan đến Identity Center, Lambda và các thao tác cấp quyền. | Đáp ứng compliance requirements (đảm bảo complete auditability). |

### Thành phần hệ thống

Hệ thống Production Access Request Portal được tổ chức theo mô hình nhiều lớp, trong đó mỗi thành phần đảm nhận một vai trò riêng nhưng phối hợp chặt chẽ để tạo thành một quy trình cấp quyền truy cập Production hoàn toàn tự động, có kiểm soát và có khả năng kiểm toán đầy đủ. Về mặt logic, hệ thống bao gồm các nhóm thành phần chính sau: giao diện yêu cầu (Jira Service Management), lớp tiếp nhận API, lớp xử lý nghiệp vụ bằng Lambda, lớp lưu trữ dữ liệu, lớp quản lý danh tính và quyền truy cập, lớp gửi thông báo, và lớp giám sát/vận hành. Thiết kế này phù hợp với kiến trúc serverless đã mô tả trong tài liệu, đồng thời hỗ trợ cơ chế Group-Based Access cho việc cấp và thu hồi quyền gần như theo thời gian thực.

| Thành phần | Chức năng chính | Vai trò trong luồng xử lý |
|------------|-----------------|---------------------------|
| **Jira Service Management Portal** | Tiếp nhận yêu cầu truy cập từ người dùng, hiển thị trạng thái request, workflow phê duyệt | Điểm bắt đầu của toàn bộ quy trình |
| **API Gateway** | Tiếp nhận webhook từ Jira, xác thực request và chuyển tiếp vào Lambda | Cửa vào của backend serverless |
| **Lambda Executor** | Xử lý provisioning access sau khi request được duyệt | Thực thi logic cấp quyền chính |
| **Lambda Email Approval** | Tạo email phê duyệt, xử lý approve/decline từ email | Hỗ trợ phê duyệt linh hoạt qua email |
| **Lambda Expiry** | Theo dõi session hết hạn và tự động thu hồi quyền | Đảm bảo quyền truy cập chỉ tồn tại trong thời gian cho phép |
| **DynamoDB** | Lưu session, approval token, TTL và metadata phục vụ revoke | Nền tảng lưu trữ trạng thái hệ thống |
| **Secrets Manager** | Lưu secrets, mapping group, token secret, Jira credentials | Bảo vệ dữ liệu nhạy cảm |
| **IAM Identity Center** | Quản lý permission sets, access groups, group membership | Thành phần cấp phát và thu hồi quyền truy cập |
| **Amazon SES** | Gửi email approval, notification, expiry alert | Kênh giao tiếp với approver và requester |
| **CloudWatch/CloudTrail** | Ghi log, metrics, audit trail | Giám sát, truy vết và tuân thủ |

### Quy trình vận hành

Quy trình vận hành của Production Access Request Portal được thiết kế theo mô hình tự động hóa end-to-end, bắt đầu từ lúc người dùng gửi yêu cầu trên Jira Service Management cho đến khi quyền truy cập được cấp, theo dõi thời hạn, và tự động thu hồi khi hết hiệu lực. Toàn bộ vòng đời của một request đều được ghi nhận đầy đủ trên Jira, DynamoDB, CloudWatch và CloudTrail nhằm đảm bảo khả năng kiểm toán và truy vết.

#### Quy trình cấp quyền tiêu chuẩn (Standard Grant Flow)

Quy trình cấp quyền tiêu chuẩn được kích hoạt khi End User gửi request truy cập Production trên Jira Service Management Portal. Sau khi request được tạo, Jira workflow sẽ chuyển trạng thái sang bước chờ phê duyệt và đồng thời phát sinh webhook đến AWS API Gateway để bắt đầu quá trình provisioning. Lambda Executor là thành phần chịu trách nhiệm xử lý logic cấp quyền chính, bao gồm xác thực webhook, tra cứu mapping, thêm user vào group và ghi nhận session vào DynamoDB.

Luồng xử lý tiêu chuẩn như sau:
1. **Người dùng gửi yêu cầu truy cập:** End User điền form trên Jira Service Management Portal với các thông tin như tài khoản đích, loại quyền truy cập và thời gian sử dụng mong muốn.
2. **Jira chuyển yêu cầu sang trạng thái chờ duyệt:** Jira tạo ticket và kích hoạt automation rule để gọi webhook tới hệ thống backend.
3. **API Gateway tiếp nhận webhook:** API Gateway nhận request từ Jira, kiểm tra API key và chuyển tiếp payload đến Lambda Executor.
4. **Lambda Executor xác thực và xử lý request:** Lambda kiểm tra tính hợp lệ của payload, xác thực chữ ký webhook, đọc dữ liệu cần thiết và ánh xạ duration của request sang duration tier chuẩn.
5. **Tra cứu access group tương ứng:** Hệ thống sử dụng Secrets Manager để lấy mapping giữa account, access type và duration tier, sau đó xác định access group phù hợp trong IAM Identity Center. Cách làm này đúng với mô hình Group-Based Access đã mô tả trong tài liệu.
6. **Thêm user vào access group:** Lambda Executor gọi API của Identity Center để tạo group membership cho user. Khi membership được tạo thành công, quyền truy cập tương ứng sẽ được cấp gần như ngay lập tức.
7. **Ghi nhận session vào DynamoDB:** Một bản ghi session được tạo trong bảng AccessSessions, bao gồm thông tin request ID, requester, group ID, membership ID, thời gian cấp và thời gian hết hạn. TTL được thiết lập để kích hoạt luồng thu hồi tự động.
8. **Cập nhật trạng thái trên Jira:** Jira ticket được chuyển sang trạng thái Approved và được dùng như nguồn theo dõi chính cho người dùng và approver.
9. **Gửi email thông báo cho requester:** Hệ thống gửi email thông báo rằng quyền đã được cấp thành công, kèm theo thông tin cần thiết để truy cập IAM Identity Center Portal.
10. **Người dùng đăng nhập và sử dụng quyền tạm thời:** End User đăng nhập vào Identity Center Portal để sử dụng quyền truy cập AWS trong khoảng thời gian đã được phê duyệt.

#### Quy trình phê duyệt qua email (Email Approval Flow)

Ngoài quy trình phê duyệt trực tiếp trên Jira, hệ thống còn hỗ trợ phê duyệt qua email nhằm tăng tính linh hoạt cho Approver. Khi một request mới được tạo, Jira automation có thể gọi Lambda Email Approval để sinh email chứa các liên kết Approve/Decline. Quy trình này sử dụng token có chữ ký HMAC-SHA256, thời hạn 24 giờ và chỉ dùng một lần để hạn chế rủi ro giả mạo hoặc replay attack.

Luồng xử lý như sau:
1. **Jira kích hoạt yêu cầu phê duyệt email:** Khi request ở trạng thái chờ duyệt, Jira gọi endpoint `/email-approval/request`.
2. **Lambda tạo token phê duyệt an toàn:** Token được sinh ra từ `token_id`, `expiry_timestamp` và `hmac_signature`, sau đó lưu metadata vào bảng Approval Tokens trong DynamoDB.
3. **Gửi email đến Approver:** Lambda render email HTML chứa hai nút `Approve` và `Decline` rồi gửi qua Amazon SES.
4. **Approver chọn hành động:** Khi người phê duyệt nhấp vào một trong hai nút, request sẽ được chuyển đến endpoint `/email-approval/action`.
5. **Hệ thống xác thực token:** Lambda kiểm tra chữ ký HMAC, trạng thái hết hạn và cờ `used` trong DynamoDB. Nếu token hợp lệ, hệ thống đánh dấu token đã sử dụng.
6. **Cập nhật kết quả về Jira:** Lambda gọi Jira Service Desk Approval API để cập nhật trạng thái approve hoặc decline.
7. **Trả về trang xác nhận:** Hệ thống hiển thị trang xác nhận cho Approver và đồng thời ghi nhận log phục vụ kiểm toán.

#### Quy trình tự động hết hạn và thu hồi quyền (Auto Expiry Flow)

Khi session đến thời điểm hết hạn, DynamoDB TTL sẽ tự động xóa bản ghi session. Sự kiện xóa này được đẩy qua DynamoDB Streams và kích hoạt Lambda Expiry để thực hiện revoke quyền. Đây là cơ chế then chốt giúp hệ thống đạt được mục tiêu thu hồi credentials trong vòng khoảng 60 giây.

Luồng xử lý như sau:
1. **DynamoDB TTL xóa session hết hạn:** Khi `ttl` của session đã quá hạn, DynamoDB tự động loại bỏ record.
2. **DynamoDB Streams phát sinh event REMOVE:** Lambda Expiry nhận sự kiện xóa từ stream và xác định đây là một sự kiện hết hạn hợp lệ.
3. **Xác định membership cần thu hồi:** Lambda đọc dữ liệu cũ của session để lấy `membership_id` và group name tương ứng.
4. **Xóa user khỏi access group:** Hệ thống gọi `DeleteGroupMembership` trên IAM Identity Center để loại bỏ quyền của user.
5. **Credentials bị vô hiệu hóa:** Sau khi membership bị xóa, credentials của user sẽ không còn hiệu lực trong thời gian rất ngắn, đáp ứng mục tiêu revoke gần như tức thời.
6. **Cập nhật Jira và gửi thông báo:** Ticket được chuyển sang trạng thái Expired, đồng thời requester nhận email thông báo quyền đã hết hạn.

#### Quy trình thu hồi khẩn cấp (Emergency Revocation)

Trong các tình huống khẩn cấp như phát hiện lộ credentials, hành vi bất thường hoặc yêu cầu từ nhóm bảo mật, hệ thống hỗ trợ cơ chế thu hồi khẩn cấp để loại bỏ toàn bộ quyền truy cập của một user trong thời gian ngắn. Lambda có thể liệt kê tất cả group memberships của user và xóa chúng để vô hiệu hóa toàn bộ session đang hoạt động.

Các bước thực hiện gồm:
1. **Kích hoạt yêu cầu thu hồi khẩn cấp:** Operator hoặc hệ thống giám sát gửi yêu cầu revoke ngay lập tức.
2. **Tra cứu toàn bộ membership của user:** Lambda lấy danh sách tất cả group memberships của user trong Identity Center.
3. **Xóa user khỏi toàn bộ groups:** Mọi membership liên quan đến user đều bị xóa để đảm bảo không còn quyền truy cập nào tồn tại.
4. **Ghi nhận sự kiện thu hồi:** Hệ thống cập nhật trạng thái trong DynamoDB, ghi log lên CloudWatch và lưu dấu vết phục vụ audit trail.

#### Nguyên tắc vận hành và xử lý ngoại lệ

Để đảm bảo hệ thống vận hành ổn định, một số nguyên tắc sau được áp dụng xuyên suốt toàn bộ workflow:
- **Không cấp quyền nếu chưa đủ điều kiện**: Request chỉ được xử lý khi webhook hợp lệ, approver hợp lệ và mapping group tương ứng tồn tại.
- **Mỗi request chỉ có một trạng thái cuối cùng:** Approved, Declined, Expired hoặc Revoked.
- **Token email chỉ dùng một lần:** Ngăn chặn việc mở lại liên kết cũ để thực hiện thao tác ngoài ý muốn.
- **Mọi lỗi đều phải được log:** Các lỗi xác thực, lỗi API hoặc lỗi provisioning cần được ghi nhận để phục vụ điều tra.
- **Ưu tiên fail-safe:** Nếu hệ thống không xác định được trạng thái hợp lệ, quyền truy cập sẽ không được cấp.

## Lộ trình & Mốc triển khai

### Giai đoạn 1: Phân tích yêu cầu và thiết kế giải pháp (Tuần 2-4)

Mục tiêu của giai đoạn này là xác định rõ phạm vi hệ thống, các yêu cầu nghiệp vụ, mô hình phân quyền và thiết kế kiến trúc tổng thể.

Các công việc chính:
- Thu thập yêu cầu từ các bên liên quan (DevOps, Security, Engineering).
- Xác định danh sách AWS Accounts cần quản lý.
- Xây dựng ma trận quyền truy cập (ReadOnly, PowerUser, Admin).
- Xác định duration tiers tiêu chuẩn.
- Thiết kế workflow Jira Service Management.
- Thiết kế sơ đồ kiến trúc hệ thống.
- Thiết kế mô hình dữ liệu DynamoDB.

**Milestone 1 - Architecture Approved**

### Giai đoạn 2: Xây dựng hạ tầng nền tảng (Tuần 5-6)

Mục tiêu của giai đoạn này là triển khai toàn bộ hạ tầng AWS bằng Terraform để tạo nền tảng cho hệ thống.

Các công việc chính:
- Xây dựng Terraform modules: API Gateway, Lambda Functions, DynamoDB Tables, IAM Roles, Secrets Manager, SES Configuration.
- Tạo Permission Sets trong IAM Identity Center.
- Tạo Access Groups cho từng account.
- Thiết lập Group Assignments.
- Thiết lập Terraform Backend (S3 + DynamoDB Lock).

**Milestone 2 - Infrastructure Ready**

### Giai đoạn 3: Phát triển nghiệp vụ hệ thống (Tuần 7-8)

Đây là giai đoạn phát triển phần logic cốt lõi của hệ thống.

Các công việc chính:
- Phát triển Lambda Executor.
- Phát triển Lambda Email Approval.
- Phát triển Lambda Expiry.
- Xây dựng shared Lambda Layer.
- Tích hợp Jira APIs.
- Tích hợp IAM Identity Center APIs.
- Tích hợp Amazon SES.
- Triển khai structured logging.
- Tích hợp Secrets Manager cache.

**Milestone 3 - Core Features Completed**

### Giai đoạn 4: Kiểm thử và nghiệm thu (Tuần 9-10)

Giai đoạn này tập trung kiểm tra tính ổn định, độ bảo mật và khả năng vận hành thực tế.

Các công việc chính:
- Unit Testing cho Lambda functions.
- Integration Testing toàn bộ workflow.
- Security Testing: API authentication, webhook signature validation, token replay prevention.
- Load Testing API Gateway.
- TTL Expiry Testing.
- Emergency Revocation Testing.

**Milestone 4 - UAT Passed**

### Giai đoạn 5: Triển khai Production và chuyển giao vận hành (Tuần 11)

Giai đoạn cuối cùng đưa hệ thống vào vận hành chính thức.

Các công việc chính:
- Deploy Production environment.
- Verify tất cả resources.
- Populate Access Group Mapping.
- Verify Jira webhook integration.
- Verify SES production mode.
- Thiết lập CloudWatch alarms.
- Thiết lập dashboard giám sát.

**Milestone 5 - Production Go-Live**

### Bảng kế hoạch triển khai tổng quan

| Giai đoạn | Thời gian | Deliverables | Milestone |
|-----------|-----------|--------------|-----------|
| Phân tích & Thiết kế | Tuần 2-4 | Kiến trúc, workflow, data model | Architecture Approved |
| Xây dựng hạ tầng | Tuần 5-6 | Terraform infrastructure | Infrastructure Ready |
| Phát triển nghiệp vụ | Tuần 7-8 | Lambda functions, API integration | Core Features Completed |
| Kiểm thử & Nghiệm thu | Tuần 9-10 | Testing reports, UAT | UAT Passed |
| Production Deployment | Tuần 11 | Production system live | Production Go-Live |

### Kế hoạch mở rộng sau triển khai

Sau khi hệ thống đi vào vận hành ổn định, các cải tiến trong tương lai có thể bao gồm:
- Hỗ trợ Slack Approval Workflow.
- Dashboard theo dõi active sessions theo thời gian thực.
- Tích hợp SIEM cho Security Team.
- Risk-based Access Approval.
- Machine Learning anomaly detection cho access pattern.
- Multi-region deployment để tăng khả năng sẵn sàng.

## Ước tính ngân sách

| Dịch vụ | Đơn giá | Giả định | Chi phí ước tính mỗi tháng |
|---------|---------|----------|----------------------------|
| **AWS Lambda** | $0.2/triệu requests và $0.0000166667/mỗi GB-giây duration | Khoảng 3000 invocations, 256 MB, thời gian chạy trung bình khoảng 5 giây | ~ $0.06 |
| **API Gateway** | $3.5/triệu requests | Khoảng 2000 requests | ~ $0.01 |
| **DynamoDB** | On-demand pricing | Khoảng 4000 write và 2000 read | ~ $0.01 |
| **Secrets Manager** | $0.4/secret/tháng và $0.05/10000 lượt calls | 4 secrets, khoảng 10000 calls | ~ $1.6 |
| **Amazon SES** | $0.1/1000 emails  | Khoảng 2000 emails (grant + expiry) | ~ $0.02 |
| **CloudWatch Logs** | $0.5/GB ingested | Khoảng 0.5 GB log ingestion | ~ $0.25 |
| **S3 (Terraform State)** | $0,023/GB | Dưới 1 MB | ~ $0.00 |

Tổng chi phí ước tính cho toàn bộ hệ thống là khoảng **~$2/tháng**.

## Đánh giá rủi ro

Mặc dù Production Access Request Portal được xây dựng trên kiến trúc serverless với nhiều lớp bảo mật và cơ chế tự động hóa, hệ thống vẫn tồn tại một số rủi ro tiềm ẩn liên quan đến phụ thuộc dịch vụ bên ngoài, lỗi vận hành, lỗi cấu hình hoặc các sự cố bảo mật. Việc đánh giá rủi ro giúp chủ động xây dựng phương án giảm thiểu và đảm bảo tính sẵn sàng của hệ thống trong môi trường Production.

### Ma trận đánh giá rủi ro

Các rủi ro được đánh giá dựa trên ba tiêu chí chính: Khả năng xảy ra, mức độ ảnh hưởng, mức độ ưu tiên xử lý.

| Rủi ro | Mô tả | Khả năng xảy ra | Mức độ ảnh hưởng | Mức độ ưu tiên xử lý |
|--------|-------|:---------------:|:----------------:|:--------------------:|
| Jira Service Management bị gián đoạn | Không thể tạo request mới hoặc cập nhật trạng thái approval | Trung bình | Cao | Cao |
| AWS IAM Identity Center lỗi hoặc suy giảm dịch vụ | Không thể cấp hoặc thu hồi quyền truy cập | Thấp | Rất cao | Cao |
| DynamoDB TTL xử lý chậm | Session hết hạn nhưng chưa bị revoke đúng thời điểm | Trung bình | Cao | Cao |
| Lambda function lỗi runtime | Workflow provisioning hoặc expiry bị gián đoạn | Trung bình | Cao | Cao |
| SES gửi email thất bại | Approver hoặc requester không nhận được thông báo | Trung bình | Trung bình | Trung bình |
| Secrets Manager không truy cập được | Lambda không lấy được secret để xử lý request | Thấp | Cao | Trung bình |
| Webhook giả mạo hoặc payload bị sửa đổi | Có thể dẫn đến provisioning trái phép nếu không xác thực đúng | Thấp | Rất cao | Cao |
| Cấu hình sai access group mapping | User được cấp sai quyền hoặc sai account | Trung bình | Cao | Cao |
| CloudWatch logging lỗi hoặc thiếu log | Mất khả năng điều tra hoặc audit trail không đầy đủ | Thấp | Trung bình | Trung bình |
| Emergency revoke thất bại | Không thể thu hồi quyền ngay khi phát hiện rủi ro | Thấp | Rất cao | Cao |

### Kế hoạch ứng phó sự cố

Để giảm thiểu rủi ro khi sự cố xảy ra, hệ thống cần có runbook rõ ràng.

| Tình huống | Phản ứng ngay lập tức |
|-----------|------------------------|
| Lambda provisioning fail | Retry hoặc manual trigger |
| Identity Center API fail | Retry và escalate AWS Support |
| SES fail | Retry gửi email hoặc dùng Jira UI approval |
| TTL revoke fail | Manual revoke qua operator |
| Security incident | Emergency revoke toàn bộ sessions |
| Webhook authentication fail | Reject request và alert Security Team |

## Kết quả kỳ vọng

- **Chuẩn hóa và tự động hóa quy trình cấp quyền:** Hệ thống giúp thay thế hoàn toàn quy trình cấp quyền thủ công bằng một luồng xử lý tự động, thống nhất và có thể kiểm soát đầu cuối. Từ bước gửi yêu cầu, phê duyệt, cấp quyền cho đến thu hồi quyền đều được điều phối bởi Jira Service Management, AWS Lambda và IAM Identity Center, qua đó giảm đáng kể thời gian xử lý và hạn chế sai sót do con người.
- **Tăng cường bảo mật cho môi trường Production:** Nhờ áp dụng mô hình Just-in-Time Access, Zero Standing Privileges và Group-Based Access hệ thống chỉ cấp quyền trong đúng khoảng thời gian cần thiết và tự động vô hiệu hóa khi hết hạn. Điều này giúp giảm thiểu rủi ro tồn tại credentials dài hạn, đồng thời nâng cao khả năng bảo vệ môi trường Production trước các tình huống lộ thông tin xác thực hoặc lạm dụng quyền truy cập.
- **Rút ngắn thời gian cấp và thu hồi quyền:** So với kiến trúc cũ dựa trên Direct Assignment, phiên bản mới kỳ vọng rút ngắn đáng kể thời gian provisioning xuống còn dưới 5 giây và thu hồi quyền trong vòng khoảng 60 giây sau khi TTL hết hạn. Nhờ đó, đội ngũ kỹ thuật có thể truy cập nhanh hơn khi cần xử lý sự cố, trong khi đội ngũ bảo mật vẫn đảm bảo khả năng thu hồi quyền gần như tức thời khi phát hiện rủi ro.
- **Nâng cao khả năng kiểm toán và truy vết:** Toàn bộ hoạt động của hệ thống được ghi nhận qua Jira, CloudWatch, CloudTrail và DynamoDB, giúp tạo ra audit trail đầy đủ cho các sự kiện như request submitted, approved, provisioned, expired và revoked. Đây là nền tảng quan trọng để đáp ứng yêu cầu tuân thủ nội bộ, hỗ trợ kiểm toán và điều tra khi có sự cố bảo mật.
- **Tối ưu chi phí và công sức vận hành:** Với kiến trúc serverless trên AWS, hệ thống không yêu cầu quản lý máy chủ, tự động mở rộng theo nhu cầu và chỉ phát sinh chi phí theo mức sử dụng thực tế. Điều này giúp giảm gánh nặng vận hành cho đội ngũ DevOps, đồng thời đảm bảo chi phí triển khai ở mức thấp và phù hợp với quy mô sử dụng của tổ chức.
- **Cải thiện trải nghiệm người dùng và quy trình phê duyệt:** Người dùng có thể gửi yêu cầu truy cập qua Jira Portal và nhận thông báo rõ ràng qua email trong suốt vòng đời request. Người phê duyệt cũng có thể xử lý nhanh hơn thông qua Jira UI hoặc email approval link, giúp quy trình trở nên linh hoạt, minh bạch và thuận tiện hơn cho cả requester lẫn approver.
- **Mở rộng dễ dàng trong tương lai:** Thiết kế theo hướng module hóa và Infrastructure as Code giúp hệ thống dễ dàng mở rộng sang nhiều AWS account, thêm access type mới hoặc tích hợp thêm các kênh phê duyệt khác như Slack mà không cần thay đổi lớn về kiến trúc tổng thể.

Tổng kết lại, dự án kỳ vọng tạo ra một nền tảng quản lý truy cập Production hiện đại, an toàn và có khả năng mở rộng cao, đồng thời cân bằng tốt giữa bảo mật, tốc độ xử lý, khả năng kiểm toán và chi phí vận hành.