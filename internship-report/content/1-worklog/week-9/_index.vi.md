---
title : "Worklog tuần 9"
date :  "`r Sys.Date()`" 
weight : 9
pre: <b> 1.9 </b>
chapter : false
---

### Mục tiêu tuần 9:

- Hiểu rõ cách xử lý và bảo vệ dữ liệu nhạy cảm trong hệ thống logging, đặc biệt là cơ chế data masking và kiểm soát truy cập trong Amazon CloudWatch.
- Nắm được cách cân bằng giữa bảo mật và khả năng vận hành (giảm thời gian xử lý sự cố) khi làm việc với log thực tế.
- Xây dựng và hiểu kiến trúc tổng thể của một hệ thống Data Lake trên AWS, từ thu thập, lưu trữ đến phân tích và trực quan hóa dữ liệu.
- Làm quen với các dịch vụ phân tích dữ liệu như Athena và QuickSight, cũng như quy trình tạo Data Catalog và xử lý dữ liệu.
- Nâng cao kỹ năng thiết kế và tối ưu dữ liệu trong DynamoDB, bao gồm cách xây dựng data model phù hợp với từng use case.
- Hoàn thiện hệ thống quản lý quyền truy cập bằng cách phát triển chức năng thu hồi quyền (revocation flow).
- Tự động hóa việc thu hồi quyền truy cập thông qua DynamoDB Streams và cơ chế TTL, giảm thiểu thao tác thủ công.
- Tích hợp thông báo qua SES để đảm bảo người dùng được cập nhật kịp thời khi quyền truy cập thay đổi.
- Tiếp tục củng cố tư duy xây dựng hệ thống serverless hoàn chỉnh, kết hợp giữa xử lý dữ liệu, bảo mật và tự động hóa trên AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Nghiên cứu và dịch bài blog **Handling sensitive log data using Amazon CloudWatch**: Tập trung vào các nội dung chính như cơ chế data masking với data protection policies, cách phát hiện và che (mask) dữ liệu nhạy cảm (PII), kiểm soát truy cập bằng IAM với quyền logs:Unmask, cũng như quy trình cấp quyền tạm thời (privilege escalation) và audit bằng CloudTrail. Qua đó hiểu rõ cách cân bằng giữa bảo mật dữ liệu và hiệu quả vận hành (MTTR) khi xử lý log trong hệ thống thực tế. | 20/04/2026 | 20/04/2026 | [Blog 04](https://aws.amazon.com/vi/blogs/mt/handling-sensitive-log-data-using-amazon-cloudwatch/) |
| 3 | - Thực hành xây dựng một hệ thống Data Lake hoàn chỉnh:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: Tạo các quyền IAM và cấu hình ban đầu để các dịch vụ có thể giao tiếp với nhau<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thu thập và lưu trữ dữ liệu<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Data Catalog<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuyển đổi dữ liệu<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phân tích dữ liệu bằng Athena<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Mô hình hóa và tạo Dashboard với QuickSight<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 21/04/2026 | 21/04/2026 | [Data Lake on AWS](https://000035.awsstudygroup.com/) |
| 4 | - Thực hành làm quen với việc thiết kế, quản lý và tối ưu hóa dữ liệu trên DynamoDB thông qua các tình huống thực tế. Bài thực hành được chia thành các lộ trình khác nhau: Người bắt đầu (Làm quen với giao diện AWS Console và CLI), Thiết kế dữ liệu nâng cao (Học về NoSQL data modeling), Ứng dụng thực tế & AI. | 22/04/2026 | 22/04/2026 | [Amazon DynamoDB Immersion Day](https://000039.awsstudygroup.com/) |
| 5 | - Xây dựng Lambda Revocation:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phát triển hàm revoke-access: Viết logic xóa người dùng khỏi IAM Identity Center Group<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tích hợp module gửi thông báo qua SES để báo cho user khi quyền truy cập hết hạn | 23/04/2026 | 23/04/2026 |  |
| 6 | - Xây dựng Lambda Revocation:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình xử lý DynamoDB Streams: Viết logic để Lambda tự động kích hoạt khi một item trong bảng AccessSessions bị xóa do hết hạn (TTL) | 24/04/2026 | 24/04/2026 |  |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 25/04/2026 | 25/04/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 26/04/2026 | 26/04/2026 | |

### Kết quả đạt được tuần 9:

- Về kiến thức lý thuyết:
  - Hiểu rõ cơ chế xử lý và bảo vệ dữ liệu nhạy cảm trong hệ thống logging với CloudWatch, đặc biệt là cách áp dụng data protection policies để phát hiện và che (mask) dữ liệu PII.
  - Nắm được cách kiểm soát truy cập log thông qua IAM (quyền logs:Unmask), cũng như quy trình cấp quyền tạm thời (privilege escalation) và audit bằng CloudTrail.
  - Hiểu được sự đánh đổi giữa bảo mật dữ liệu và hiệu quả vận hành (MTTR) khi xử lý sự cố trong hệ thống thực tế.
  - Nắm được kiến trúc tổng thể của một hệ thống Data Lake trên AWS, bao gồm các thành phần: lưu trữ, catalog, xử lý và phân tích dữ liệu.
  - Hiểu cách hoạt động và vai trò của các dịch vụ như Athena (truy vấn dữ liệu) và QuickSight (trực quan hóa dữ liệu).
  - Củng cố kiến thức về thiết kế dữ liệu NoSQL trên DynamoDB, đặc biệt là tư duy data modeling theo access pattern.
- Về thực hành (Hands-on Labs):
  - **Lab 35:** Xây dựng hoàn chỉnh một hệ thống Data Lake trên AWS, bao gồm thu thập dữ liệu, tạo Data Catalog, xử lý và truy vấn dữ liệu bằng Athena, trực quan hóa bằng QuickSight.
  - **Lab 39:** Thực hành thiết kế và tối ưu dữ liệu trên DynamoDB, làm quen với nhiều cách tiếp cận từ cơ bản đến nâng cao (CLI, Console, data modeling).
- Về phát triển hệ thống:
  - Xây dựng hoàn chỉnh Lambda Revocation để thu hồi quyền truy cập người dùng khỏi IAM Identity Center Group.
  - Tích hợp DynamoDB Streams và TTL để tự động kích hoạt quá trình thu hồi quyền khi session hết hạn.
  - Kết nối hệ thống với Amazon SES để gửi thông báo cho người dùng khi quyền truy cập bị thu hồi.
  - Hoàn thiện luồng lifecycle của quyền truy cập (provision → sử dụng → hết hạn → revoke) trong hệ thống.
- Về kỹ năng:
  - Nâng cao khả năng thiết kế và triển khai hệ thống xử lý dữ liệu trên AWS theo mô hình Data Lake.
  - Cải thiện kỹ năng làm việc với DynamoDB, đặc biệt là thiết kế schema tối ưu cho hiệu năng và chi phí.
  - Rèn luyện tư duy bảo mật khi xử lý log và dữ liệu nhạy cảm trong hệ thống cloud.
  - Phát triển khả năng xây dựng hệ thống serverless hoàn chỉnh, có tính tự động hóa cao.
  - Cải thiện kỹ năng đọc hiểu tài liệu kỹ thuật và áp dụng vào thực tế.
- Về quản lý tài nguyên:
  - Thực hiện dọn dẹp tài nguyên sau mỗi bài lab, đảm bảo kiểm soát chi phí.
  - Có ý thức theo dõi và tối ưu tài nguyên khi triển khai các dịch vụ liên quan đến dữ liệu và phân tích.


