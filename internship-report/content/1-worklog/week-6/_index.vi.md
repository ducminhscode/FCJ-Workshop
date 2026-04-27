---
title : "Worklog tuần 6"
date :  "`r Sys.Date()`" 
weight : 6
pre: <b> 1.6 </b>
chapter : false
---

### Mục tiêu tuần 6:

- Hiểu rõ mô hình bảo mật trên AWS, đặc biệt là Shared Responsibility Model, nhằm phân định trách nhiệm giữa nhà cung cấp dịch vụ và người sử dụng trong việc bảo vệ hệ thống.
- Nắm vững các khái niệm và cơ chế hoạt động của AWS Identity and Access Management (IAM), bao gồm IAM User, Group, Role và Policy (JSON), cùng với nguyên tắc phân quyền least privilege.
- Hiểu được vai trò và lợi ích của IAM Role, đặc biệt trong việc cấp quyền tạm thời và tăng cường bảo mật cho các dịch vụ AWS.
- Tìm hiểu cơ chế xác thực và phân quyền người dùng thông qua Amazon Cognito, bao gồm User Pool, Identity Pool và cách tích hợp với ứng dụng web/mobile.
- Hiểu rõ cách quản lý môi trường đa tài khoản với AWS Organizations, bao gồm cấu trúc tổ chức, Organizational Units (OU) và cơ chế kiểm soát quyền bằng Service Control Policy (SCP).
- Tìm hiểu AWS Identity Center (SSO) để quản lý truy cập tập trung và triển khai cơ chế đăng nhập một lần cho nhiều tài khoản và ứng dụng.
- Hiểu cơ chế mã hóa và quản lý khóa trong AWS Key Management Service (KMS), bao gồm các loại khóa (CMK, Data Key) và cách sử dụng để bảo vệ dữ liệu.
- Tìm hiểu dịch vụ AWS Security Hub, bao gồm cách tổng hợp, phân tích và đánh giá các vấn đề bảo mật theo các tiêu chuẩn như CIS Benchmark, AWS Foundational Security Best Practices.
- Thực hành các bài lab liên quan đến:
  - Phân tích và đánh giá bảo mật với AWS Security Hub.
  - Tối ưu chi phí EC2 bằng cách sử dụng AWS Lambda.
  - Quản lý tài nguyên bằng Tags và Resource Groups.
  - Kiểm soát truy cập EC2 thông qua IAM và Resource Tags.
- Rèn luyện kỹ năng thiết kế hệ thống bảo mật trên cloud theo hướng security-first, kết hợp nhiều dịch vụ AWS để xây dựng hệ thống an toàn và hiệu quả.
- Nâng cao kỹ năng đọc hiểu tài liệu kỹ thuật, triển khai thực hành trên AWS Console và CLI, đồng thời hình thành tư duy tối ưu chi phí và quản lý tài nguyên hiệu quả.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Tìm hiểu về bảo mật trên AWS và Shared Responsibility Model:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Shared Responsibility Model<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các dịch vụ bảo mật chính sẽ học trong tuần 5: IAM, Amazon Cognito, AWS Organizations & Identity Center, AWS KMS<br>- Quản lý định danh và quyền truy cập trên AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm về tài khoản Root<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm IAM User và IAM Group<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm IAM Policy - được viết dưới dạng JSON: Nguyên tắc ưu tiên, phân loại<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm quan trọng IAM Role<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tại sao nên dùng IAM Role?<br>- Quản lý xác thực và cấp phép người dùng qua Amazon Cognito cho các ứng dụng web và di động:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm Amazon Cognito: Chức năng chính, lợi ích<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thành phần cốt lõi của Amazon Cognito: User Pool và Identity Pool<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cách thức hoạt động và kiến trúc: sử dụng User Pool để xác thực<br>- AWS Organizations, một công cụ quan trọng trong việc quản lý tập trung môi trường đa tài khoản AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Vai trò của AWS Organizations: Quản lý tập trung, giảm thiểu rủi ro, tự động hóa<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu trúc của một Organization: Management Account (Master Account), Organizational Units, Member Accounts<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm soát quyền hạn với Service Control Policy (SCP): Cơ chế Boundary, tính ưu tiên cao, các ví dụ<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Consolidated Billing<br>- AWS Identity Center, một dịch vụ giúp quản lý quyền truy cập tập trung cho nhiều tài khoản AWS và các ứng dụng kinh doanh khác<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm AWS Identity Center<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Mô hình hoạt động trong AWS Organizations: Master Account, Development Account & Production Account, cơ chế phân quyền<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Lợi ích chính: Đăng nhập một lần, quản lý tập trung, tăng cường bảo mật<br>- AWS Key Management Service, một dịch vụ tạo và quản lý các khóa mã hóa để bảo vệ dữ liệu trên AWS- Khái niệm AWS KMS: Chức năng, tiêu chuẩn bảo mật<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các loại khóa quan trọng: CMK, Data Key<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cơ chế hoạt động<br>- AWS Security Hub, một dịch vụ giúp quản lý và kiểm tra bảo mật trên hạ tầng AWS một cách tập trung:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm và cơ chế hoạt động AWS Security Hub<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phạm vi kiểm tra<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tiêu chuẩn bảo mật: PCI DSS, AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cách đọc kết quả<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Ví dụ cụ thể về các hạng mục kiểm tra<br>- Các bài thực hành và tài liệu nghiên cứu chuyên sâu về bảo mật của AWS | 30/03/2026 | 30/03/2026 | [Share Responsibility Model](https://youtu.be/tsobAlSg19g?si=VRtW8Y1D1zuJUnFL)<br>[Amazon Identity and access management](https://youtu.be/N_vlJGAqZxo?si=h_taNgRzWo2OlED5)<br>[Amazon Cognito](https://youtu.be/pZ2fgEFK3Vs?si=0bjyUxZmq4EAPFQg)<br>[AWS Organization](https://youtu.be/5oQY8Rogz9Y?si=Xx2M1i3wiO7h5HNz)<br>[AWS Identity Center](https://youtu.be/NW1xrMkNMjU?si=Y0WEJoN_k6JAjbrR)<br>[Amazon Key Management Service](https://youtu.be/GMihNQojhZc?si=hkSfBPC9axfHKS0G)<br>[AWS Security Hub](https://youtu.be/clj2E0rNBEs?si=O9WI1Q_939tNxLKG)<br>[Hands-on and Additional research](https://youtu.be/0SdpD2GPYz4?si=yN_FWFNR3honqcu1) |
| 3 | - Thực hành làm quen với AWS Security Hub (Tài khoản Free Tier không thể thực hiện được) ([Module 05 - Lab 18](https://drive.google.com/drive/folders/1V35rUoHCcNq5eqQzhATQSY5dBU8wcOiu?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tiêu chuẩn bảo mật: Tìm hiểu về các bộ tiêu chuẩn như AWS Foundational Security Best Practices hoặc CIS AWS Foundations Benchmark<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kích hoạt Security Hub<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phân tích các phát hiện bảo mật<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Đánh giá mức độ tuân thủ<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 31/03/2026 | 31/03/2026 | [Getting Started with AWS Security Hub](https://000018.awsstudygroup.com/) |
| 4 | - Thực hành tối ưu hóa chi phí EC2 bằng cách sử dụng AWS Lambda ([Module 05 - Lab 22](https://drive.google.com/drive/folders/1b4NZnxlqQlM4Ku0PZ6i4MzabPinZzPHP?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị hạ tầng: VPC, Security Group, EC2, tích hợp Web-hooks với Slack để nhận thông báo<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Gán nhãn: Sử dụng thẻ (Tags) để đánh dấu các EC2 cụ thể mà Lambda sẽ tác động vào<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phân quyền: Tạo IAM Role cho phép Lambda thực hiện StartInstances và StopInstances trên dịch vụ EC2<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xây dựng hàm Lambda: Function stop instance, Function start instance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra kết quả<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 01/04/2026 | 01/04/2026 | [Optimizing EC2 Costs with Lambda](https://000022.awsstudygroup.com/) |
| 5 | - Thực hành quản lý tài nguyên bằng cách sử dụng Thẻ (Tags) và Nhóm tài nguyên (Resource Groups) trên AWS ([Module 05 - Lab 27](https://drive.google.com/drive/folders/1ME_mYIb3V8FS36IrNMnCtosvYF_DLxw9?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sử dụng Tags trên Console<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sử dụng Tags thông qua CLI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Nhóm tài nguyên (Resource Groups): cho phép gom nhóm các tài nguyên (như EC2, S3, CloudFormation stacks) cùng vùng (Region) lại với nhau dựa trên kết quả truy vấn<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 02/04/2026 | 02/04/2026 | [Manage Resources Using Tags and Resource Groups](https://000027.awsstudygroup.com/) |
| 6 | - Thực hành quản lý quyền truy cập dịch vụ EC2 thông qua thẻ tài nguyên (Resource Tags) và dịch vụ IAM ([Module 05 - Lab 28](https://drive.google.com/drive/folders/1uNkrzbHSkqqGdMQkmgfyCVnLOAxnAJo7?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị: Tạo người dùng IAM và cấu hình các yêu cầu cần thiết<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo IAM Policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo IAM Role<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra chính sách<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 03/04/2026 | 03/04/2026 | [Manage access to EC2 Services with Resource Tags through IAM Services](https://000028.awsstudygroup.com/) |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 04/04/2026 | 04/04/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 05/04/2026 | 05/04/2026 | |

### Kết quả đạt được tuần 6:

- Về kiến thức lý thuyết:
  - Hiểu rõ mô hình Shared Responsibility Model, phân biệt trách nhiệm bảo mật giữa AWS và người dùng ở các lớp (infrastructure, platform, application, data).
  - Nắm vững các thành phần của IAM (Identity and Access Management):
    - IAM User, Group, Role và Policy (JSON)
    - Nguyên tắc least privilege và cơ chế đánh giá policy (explicit deny > allow)
  - Hiểu rõ vai trò và cách sử dụng IAM Role, đặc biệt trong việc cấp quyền tạm thời cho dịch vụ (EC2, Lambda).
  - Hiểu cơ chế xác thực và phân quyền của Amazon Cognito:
    - Phân biệt User Pool (authentication) và Identity Pool (authorization)
    - Nắm được quy trình đăng nhập và cấp quyền cho ứng dụng web/mobile
  - Hiểu cách tổ chức và quản lý đa tài khoản với AWS Organizations:
    - Cấu trúc: Management Account, Organizational Units (OU), Member Accounts
    - Cơ chế kiểm soát quyền với Service Control Policy (SCP)
    - Consolidated Billing
  - Nắm được cách hoạt động của AWS Identity Center (SSO):
    - Quản lý truy cập tập trung
    - Phân quyền theo account trong môi trường multi-account
  - Hiểu cơ chế mã hóa và quản lý khóa với AWS Key Management Service (KMS):
    - Phân biệt Customer Managed Key (CMK) và Data Key
    - Cơ chế mã hóa envelope encryption
  - Hiểu vai trò của AWS Security Hub:
    - Tổng hợp và phân tích các cảnh báo bảo mật
    - Các tiêu chuẩn: CIS AWS Foundations Benchmark, AWS Foundational Security Best Practices, PCI DSS
    - Cách đọc và đánh giá kết quả kiểm tra bảo mật
- Về thực hành (Hands-on Labs):
  - **Lab 18:**
    - Kích hoạt và làm quen với AWS Security Hub (giới hạn bởi Free Tier)
    - Phân tích các phát hiện bảo mật (findings)
    - Đánh giá mức độ tuân thủ theo tiêu chuẩn bảo mật
  - **Lab 22:**
    - Xây dựng Lambda function để tự động start/stop EC2
    - Sử dụng Tags để xác định tài nguyên mục tiêu
    - Tích hợp thông báo qua webhook (Slack)
  - **Lab 27:**
    - Gắn thẻ tài nguyên trên Console và CLI
    - Tạo Resource Groups để quản lý tập trung
  - **Lab 28:**
    - Xây dựng IAM Policy dựa trên điều kiện tag
    - Tạo IAM Role và kiểm tra phân quyền truy cập EC2
  - Thực hiện kiểm tra hoạt động và dọn dẹp tài nguyên sau mỗi bài lab
- Về kỹ năng:
  - Nâng cao kỹ năng thiết kế hệ thống bảo mật trên AWS theo chuẩn best practices
  - Có khả năng xây dựng và kiểm soát quyền truy cập linh hoạt bằng IAM, Role và Policy
  - Thành thạo việc sử dụng Tags để quản lý và kiểm soát tài nguyên
  - Rèn luyện kỹ năng phân tích cảnh báo bảo mật và đánh giá compliance
  - Nâng cao khả năng đọc tài liệu kỹ thuật và triển khai thực tế trên AWS Console
- Về tư duy hệ thống:
  - Hình thành tư duy security-first khi thiết kế hệ thống cloud
  - Hiểu cách kết hợp nhiều dịch vụ (IAM, Cognito, Organizations, KMS) để xây dựng hệ thống bảo mật toàn diện
  - Nhận thức rõ trade-offs giữa bảo mật, chi phí và hiệu năng
- Về quản lý tài nguyên:
  - Thực hiện dọn dẹp tài nguyên sau mỗi bài lab nhằm tránh phát sinh chi phí
  - Biết cách sử dụng Tags để kiểm soát và tối ưu tài nguyên hiệu quả
  - Có ý thức sử dụng dịch vụ trong phạm vi Free Tier khi thực hành