---
title : "Worklog tuần 5"
date :  "`r Sys.Date()`" 
weight : 5
pre: <b> 1.5 </b>
chapter : false
---

### Mục tiêu tuần 5:

- Nắm vững cách triển khai và quản trị các dịch vụ lưu trữ trên AWS, bao gồm Amazon FSx và Amazon S3, thông qua các bài lab thực tế.
- Hiểu và áp dụng được mô hình xây dựng hệ thống file chia sẻ hiệu năng cao trên nền tảng Windows Server trong môi trường cloud.
- Làm quen với việc triển khai website tĩnh sử dụng S3 kết hợp CloudFront, đảm bảo yếu tố bảo mật và tối ưu hiệu năng.
- Xây dựng nền tảng cho hệ thống backend serverless với AWS Lambda, bao gồm tổ chức project, tái sử dụng code (shared logic) và quản lý dependency bằng Lambda Layer.
- Phát triển các module tích hợp bên ngoài (Jira API) và dịch vụ AWS (DynamoDB), phục vụ cho bài toán quản lý workflow.
- Thiết kế và triển khai logic phê duyệt (approval flow) với cơ chế bảo mật token (HMAC-SHA256), đảm bảo tính toàn vẹn và an toàn dữ liệu.
- Từng bước hoàn thiện kỹ năng thiết kế, phát triển và vận hành một hệ thống serverless thực tế trên AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Thực hành thiết lập một hệ thống tệp tin chia sẻ hiệu năng cao, được xây dựng trên nền tảng Windows Server:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị hạ tầng: Khởi tạo VPC, Active Directory (AWS Managed Microsoft AD), tạo máy chủ Windows (EC2)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khởi tạo hệ thống tệp FSx: SSD Multi-AZ file system, HDD Multi-AZ file system<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kết nối và tạo File Share<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tối ưu hóa lưu trữ: Chống trùng lặp dữ liệu, Shadow Copies<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Quản trị và giám sát: Quản lý Session, hạn ngạch bộ nhớ (User Quotas), truy cập tiếp tục (Continuously Available Shares)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Mở rộng hệ thống: Mở rộng thông lượng và dung lượng<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Giám sát và đánh giá hiệu năng hệ thống<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 23/03/2026 | 23/03/2026 | [Amazon FSx for Windows File Server](https://000025.awsstudygroup.com/) |
| 3 | - Thực hành làm quen với Amazon S3 thông qua việc thiết lập một trang web tĩnh (Static Website) ([Module 04 - Lab 57](https://drive.google.com/drive/folders/1wwixlCyGcefwCB5F8Gq094LTd1t_eScY?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị dữ liệu: Tạo S3 Bucket, tải dữ liệu<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình Static Website Hosting: Kích hoạt tính năng Static website hosting trên S3, chỉ định tệp tin trang chủ và tệp báo lỗi<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập quyền truy cập (Security và Permissions): Cấu hình Block Public Access, cấu hình Bucket Policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tăng tốc và bảo mật với CloudFront: Amazon CloudFront, cấu hình OAC/OAI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Quản lý vòng đời và dữ liệu: Bucket Versioning, di chuyển và sao chép<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 24/03/2026 | 24/03/2026 | [Starting with Amazon S3](https://000057.awsstudygroup.com/) |
| 4 | - Khởi tạo môi trường và Shared Logic:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập cấu trúc thư mục project cho Lambda<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xây dựng thư viện dùng chung shared (logging, exception handling)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Viết module `dynamodb_session.py` để tương tác với các bảng session và token | 25/03/2026 | 25/03/2026 | [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)<br>[Python Virtualenv](https://docs.python.org/3/library/venv.html) |
| 5 | - Xây dựng Lambda Layer và Jira Client:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Đóng gói thư viện external (requests, boto3) vào Lambda Layer<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phát triển `jira_client.py`: Tích hợp API Jira để cập nhật trạng thái ticket và lấy thông tin User/Group từ Jira Service Management | 26/03/2026 | 26/03/2026 | [Boto3](https://docs.aws.amazon.com/boto3/latest/)<br>[AWS SSO Admin API](https://docs.aws.amazon.com/boto3/latest/reference/services/sso-admin.html)<br>[Jira Service Management](https://developer.atlassian.com/cloud/jira/service-desk/webhooks/)<br>[Atlassian](https://developer.atlassian.com/cloud/jira/platform/webhooks/) |
| 6 | - Phát triển Approval Logic:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Viết hàm generate_token cho approval: Tạo mã token bảo mật dựa trên HMAC-SHA256<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phát triển Lambda xử lý phản hồi từ email: Xác thực token và cập nhật trạng thái phê duyệt vào DynamoDB | 27/03/2026 | 27/03/2026 | [AWS DynamoDB](https://aws.amazon.com/vi/dynamodb/) |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 28/03/2026 | 28/03/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 29/03/2026 | 29/03/2026 | |

### Kết quả đạt được tuần 5:

- Về kiến thức lý thuyết:
  - Hiểu rõ kiến trúc và cách triển khai hệ thống file chia sẻ hiệu năng cao trên nền tảng Windows Server trong môi trường AWS.
  - Nắm được cách hoạt động của Amazon FSx for Windows File Server, bao gồm Multi-AZ, cơ chế lưu trữ SSD/HDD và các tính năng tối ưu như deduplication, Shadow Copies.
  - Hiểu cách triển khai static website trên Amazon S3 và vai trò của CloudFront trong việc tăng tốc và bảo mật nội dung.
  - Nắm được các cơ chế kiểm soát truy cập trên S3 như Bucket Policy, Block Public Access và Versioning.
  - Củng cố kiến thức về kiến trúc serverless với AWS Lambda và cách tổ chức code theo hướng tái sử dụng.
- Về thực hành (Hands-on Labs):
  - **Lab 25:** Triển khai hệ thống file sharing với Amazon FSx, bao gồm tạo file system, kết nối, cấu hình quyền truy cập và giám sát hiệu năng.
  - **Lab 57:** Xây dựng static website trên Amazon S3, cấu hình hosting, phân quyền truy cập và tích hợp CloudFront.
- Về phát triển hệ thống:
  - Thiết lập cấu trúc project cho AWS Lambda theo hướng module hóa và dễ mở rộng.
  - Xây dựng các thư viện dùng chung (shared logic) như logging và exception handling.
  - Phát triển module `dynamodb_session.py` để quản lý session và token với DynamoDB.
  - Tạo Lambda Layer để đóng gói và tái sử dụng các thư viện bên ngoài (requests, boto3).
  - Xây dựng `jira_client.py` để tích hợp với Jira API phục vụ quản lý ticket.
  - Triển khai logic approval flow, bao gồm tạo token bảo mật bằng HMAC-SHA256 và xử lý phản hồi từ email.
  - Kết nối với DynamoDB để lưu trữ và cập nhật trạng thái phê duyệt.
- Về kỹ năng:
  - Nâng cao khả năng triển khai và quản lý hệ thống lưu trữ trên AWS.
  - Cải thiện kỹ năng xây dựng hệ thống serverless với Lambda và các dịch vụ liên quan.
  - Rèn luyện tư duy thiết kế hệ thống theo hướng module hóa và tái sử dụng code.
  - Phát triển kỹ năng tích hợp API bên ngoài (Jira) vào hệ thống backend.
  - Cải thiện khả năng xử lý bảo mật trong các luồng nghiệp vụ (token-based approval).
- Về quản lý tài nguyên:
  - Thực hiện dọn dẹp tài nguyên sau mỗi bài lab để tránh phát sinh chi phí.
  - Có ý thức theo dõi và tối ưu tài nguyên khi triển khai các dịch vụ lưu trữ và serverless trên AWS.


