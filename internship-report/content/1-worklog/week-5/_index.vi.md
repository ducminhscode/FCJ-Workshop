---
title : "Worklog tuần 5"
date :  "`r Sys.Date()`" 
weight : 5
pre: <b> 1.5 </b>
chapter : false
---

### Mục tiêu tuần 5:

- Nắm vững cách thiết lập và quản trị hệ thống tệp tin chia sẻ hiệu năng cao trên Amazon FSx for Windows File Server, bao gồm chuẩn bị hạ tầng Active Directory, tạo Multi-AZ file system (SSD & HDD), cấu hình File Share, tối ưu hóa lưu trữ với Data Deduplication & Shadow Copies, quản lý User Quotas, Continuously Available Shares, giám sát hiệu năng và mở rộng dung lượng.
- Hiểu rõ và thực hành thành thạo triển khai Static Website Hosting trên Amazon S3 kết hợp Amazon CloudFront, bao gồm cấu hình Bucket Policy, Block Public Access, OAC/OAI, Bucket Versioning, quản lý vòng đời dữ liệu và tăng tốc độ truy cập.
- Nghiên cứu tài liệu kỹ thuật và hiểu rõ luồng dữ liệu giữa Jira Service Management (JSM) và AWS.
- Thiết lập môi trường phát triển (Development Environment) và khởi tạo cấu trúc mã nguồn.
- Xác định cấu trúc dữ liệu (Payload) chuẩn để trao đổi giữa các thành phần.
- Hoàn thiện module `secrets.py` với cơ chế caching.
- Phát triển logic Just-In-Time core `jit_access.py`: Mapping thời gian và quản lý Assignment/Group.
- Xây dựng cơ chế thu hồi quyền khẩn cấp (Emergency Revoke).
- Xây dựng module `jira_client.py` để kết nối hệ thống tự động hóa với Jira Service Management.
- Triển khai cơ chế Retry và Exponential Backoff để đảm bảo tính ổn định khi gọi API.
- Hiện thực hóa logic chuyển trạng thái ticket (Transitions) và tích hợp phê duyệt/từ chối qua Service Desk API (với fallback mechanism).

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Thực hành thiết lập một hệ thống tệp tin chia sẻ hiệu năng cao, được xây dựng trên nền tảng Windows Server ([Module 04 - Lab 25](https://drive.google.com/drive/folders/1gp6idSIk6RIaVF3olLtOnx2_w-ontByp?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị hạ tầng: Khởi tạo VPC, Active Directory (AWS Managed Microsoft AD), tạo máy chủ Windows (EC2)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khởi tạo hệ thống tệp FSx: SSD Multi-AZ file system, HDD Multi-AZ file system<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kết nối và tạo File Share<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tối ưu hóa lưu trữ: Chống trùng lặp dữ liệu, Shadow Copies<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Quản trị và giám sát: Quản lý Session, hạn ngạch bộ nhớ (User Quotas), truy cập tiếp tục (Continuously Available Shares)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Mở rộng hệ thống: Mở rộng thông lượng và dung lượng<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Giám sát và đánh giá hiệu năng hệ thống<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 23/03/2026 | 23/03/2026 | [Amazon FSx for Windows File Server](https://000025.awsstudygroup.com/) |
| 3 | - Thực hành làm quen với Amazon S3 thông qua việc thiết lập một trang web tĩnh (Static Website) ([Module 04 - Lab 57](https://drive.google.com/drive/folders/1wwixlCyGcefwCB5F8Gq094LTd1t_eScY?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị dữ liệu: Tạo S3 Bucket, tải dữ liệu<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình Static Website Hosting: Kích hoạt tính năng Static website hosting trên S3, chỉ định tệp tin trang chủ và tệp báo lỗi<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập quyền truy cập (Security và Permissions): Cấu hình Block Public Access, cấu hình Bucket Policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tăng tốc và bảo mật với CloudFront: Amazon CloudFront, cấu hình OAC/OAI<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Quản lý vòng đời và dữ liệu: Bucket Versioning, di chuyển và sao chép<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 24/03/2026 | 24/03/2026 | [Starting with Amazon S3](https://000057.awsstudygroup.com/) |
| 4 | **Phân tích Logic & Khởi tạo mã nguồn**<br>- Nghiên cứu tài liệu để bắt đầu thực hiện dự án nhóm:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tìm hiểu tài liệu API của AWS IAM Identity Center (`boto3`, `sso-admin`) để chuẩn bị cho việc cấp/thu hồi quyền<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Nghiên cứu cấu trúc Webhook của Jira Service Management để biết cách trích xuất dữ liệu từ Ticket<br>- Thiết lập môi trường:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khởi tạo Git Repository cho phần Backend<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập môi trường ảo Python (Virtualenv) và cài đặt các dependencies cơ bản: `boto3`, `requests`, `pytest`<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình AWS CLI với quyền IAM phù hợp để test local các hàm Lambda<br>- Xây dựng cấu trúc thư mục theo chuẩn AWS Lambda:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ `/src/lambda_functions/`: Chứa mã nguồn các hàm (Executor, Expiry, Email)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ `/src/shared/`: Chứa các module dùng chung (Jira client, AWS secrets)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ `/scripts/`: Chứa các script bổ trợ (Populate data)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ `/tests/`: Chứa các bản test unit cho logic xử lý<br>- Thiết kế luồng logic:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Vẽ sơ đồ tuần tự (Sequence Diagram) chi tiết cho hàm Executor | 25/03/2026 | 25/03/2026 | [AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)<br>[Boto3](https://docs.aws.amazon.com/boto3/latest/)<br>[AWS SSO Admin API](https://docs.aws.amazon.com/boto3/latest/reference/services/sso-admin.html)<br>[Jira Service Management](https://developer.atlassian.com/cloud/jira/service-desk/webhooks/)<br>[Atlassian](https://developer.atlassian.com/cloud/jira/platform/webhooks/)<br>[AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)<br>[Python Virtualenv](https://docs.python.org/3/library/venv.html)<br>[AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) |
| 5 | **Xây dựng Lambda Layer (Common Utilities)**<br>- Phát triển module `secrets.py`:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Viết hàm `get_secret(secret_name, region_name, use_cache)` hỗ trợ cache toàn cục<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xây dựng các wrapper chuyên dụng: `get_jira_credentials` và `get_webhook_auth_secrets` để tách biệt logic xác thực<br>- Phát triển module `jit_access.py`:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Hiện thực hóa `map_duration_to_tier(duration_hours)`: Hỗ trợ các mốc 1h, 2h, 4h, 8h, 12h với cơ chế làm tròn lên (round-up)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phát triển lớp `JITAccessManager`: Hỗ trợ cả hai phương thức cấp quyền: trực tiếp qua Account Assignment và gián tiếp qua Access Group<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xử lý thu hồi quyền: Viết logic `immediate_revoke_with_user_tag` để vô hiệu hóa credentials ngay lập tức bằng cách gắn tag người dùng hoặc vô hiệu hóa tài khoản<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Emergency Revoke: Lập trình hàm `emergency_revoke_all_access` để thu hồi toàn bộ quyền truy cập hiện có của người dùng trong trường hợp xảy ra sự cố bảo mật | 26/03/2026 | 26/03/2026 |  |
| 6 | **Xây dựng Lambda Layer (Jira Integration)**<br>- Xây dựng hạ tầng kết nối:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai hàm `_create_session_with_retry` sử dụng `urllib3.util.retry`<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập chiến lược thử lại: 3 lần thử, hệ số giãn cách (backoff factor) là 1, tập trung vào các lỗi 500, 502, 503, 504<br>- Phát triển logic cập nhật trạng thái:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Viết hàm `transition_jira_status`: Tự động tìm kiếm ID của transition dựa trên tên để tăng tính linh hoạt<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai hàm `update_jira_status` làm wrapper để map các trạng thái nghiệp vụ (Executed, Failed, Expired) vào các bước tương ứng trong Jira workflow<br>- Tích hợp Jira Service Desk API:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phát triển hàm `approve_service_desk_request`: Sử dụng endpoint chuyên biệt của Service Desk thay vì API Jira Core thông thường<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xây dựng cơ chế Fallback: Nếu không tìm thấy approval cụ thể, hệ thống sẽ tự động chuyển sang dùng Transition API để phê duyệt yêu cầu (`_approve_via_transition`)<br>- Xác thực và bảo mật: Kết nối thành công với module `secrets.py` để lấy thông tin xác thực (`base_url`, `api_token`, `user_email`) một cách an toàn | 27/03/2026 | 27/03/2026 |  |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 28/03/2026 | 28/03/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 29/03/2026 | 29/03/2026 | |

### Kết quả đạt được tuần 5:

- Về thực hành (Hands-on Labs):
  - **Lab 25:** Thiết lập hệ thống tệp tin chia sẻ hiệu năng cao trên Amazon FSx for Windows File Server (Multi-AZ, SSD & HDD, tối ưu hóa với Data Deduplication, Shadow Copies, User Quotas, Continuously Available Shares).
  - **Lab 57:** Thiết lập Static Website Hosting trên Amazon S3 kết hợp Amazon CloudFront (với OAC, Bucket Policy, Versioning).
- Về dự án nhóm:
  - Môi trường phát triển đã được thiết lập hoàn chỉnh trên máy cá nhân.
  - Cấu trúc mã nguồn được tổ chức rõ ràng theo best practice cho AWS Lambda.
  - Module `secrets.py` hoạt động ổn định, có khả năng truy xuất Jira API Token, Webhook Secret và AWS Config với cơ chế Global Caching giúp tối ưu hóa execution context của Lambda.
  - Module `jit_access.py`: Xử lý chính xác logic tính toán thời gian thực (JIT). Đã tích hợp thành công các phương thức:
    - Mapping thời gian yêu cầu sang các Tier (1h, 2h, 4h, 8h, 12h).
    - Quản lý Account Assignment và Access Group trong AWS Identity Center.
    - Cơ chế thu hồi quyền khẩn cấp (Emergency Revoke) thông qua User Tagging.
  - Hoàn thành module `jira_client.py` với khả năng xử lý lỗi mạng cực tốt nhờ cơ chế Retry.
  - Hệ thống đã có khả năng điều khiển luồng công việc trên Jira (phê duyệt, kết thúc ticket, báo lỗi) một cách tự động.
  - Đảm bảo tính tương thích với cả Jira Software thông thường và Jira Service Management.
  - Khó khăn & Giải pháp:
    - AWS Lambda mặc định dùng UTC, trong khi người dùng và vận hành tại Việt Nam dùng GMT+7 → dễ gây sai lệch thời gian thu hồi quyền. **Giải pháp:** Toàn bộ logic xử lý và lưu trữ timestamp trong DynamoDB sử dụng UTC. Chỉ chuyển đổi sang Asia/Ho_Chi_Minh khi hiển thị trên Jira Ticket hoặc gửi Email thông báo.
    - Vấn đề phức tạp của AWS IAM Identity Center: Nhiều ARN, Instance ID, Permission Set ID,...**Giải pháp:** Tách biệt việc thu thập metadata bằng script populate một lần, lưu trữ tĩnh vào Secrets Manager.
    - Khác biệt giữa Jira Software và Jira Service Management. **Giải pháp:** Triển khai logic kiểm tra `canAnswerApproval` → ưu tiên dùng Service Desk Approval API khi có approval pending, fallback về Transition API nếu không.
- Về kỹ năng:
  - Nâng cao kỹ năng thực hành hạ tầng AWS: Quản trị Amazon FSx for Windows File Server và tối ưu hóa Static Website trên S3 và CloudFront.
  - Thành thạo hơn việc sử dụng boto3 với AWS IAM Identity Center (SSO Admin API) và cách xử lý các định danh phức tạp (ARN, Permission Set, Account Assignment).
  - Kỹ năng phát triển Python module sạch, tái sử dụng (Lambda Layer) với cơ chế caching, retry và error handling chuyên nghiệp.
  - Thành thạo tích hợp API bên thứ ba (Jira Service Management & Service Desk API), bao gồm xử lý webhook, transition, approval workflow và fallback mechanism.
  - Cải thiện khả năng thiết kế logic Just-In-Time Access Control, quản lý thời gian thu hồi quyền và xử lý sự cố bảo mật khẩn cấp.
  - Kỹ năng debug và giải quyết vấn đề thực tế liên quan đến múi giờ, API rate limit, và sự khác biệt giữa Jira Core với Jira Service Management.
  - Tăng cường tư duy tổ chức mã nguồn dự án lớn, sử dụng Virtualenv, Git và chuẩn bị cho môi trường serverless (AWS Lambda).
