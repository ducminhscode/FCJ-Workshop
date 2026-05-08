---
title : "Worklog tuần 11"
date :  "`r Sys.Date()`" 
weight : 11
pre: <b> 1.11 </b>
chapter : false
---

### Mục tiêu tuần 11:

- Hoàn thiện và kiểm thử toàn bộ luồng tích hợp của hệ thống quản lý quyền truy cập từ Jira đến AWS Identity Center.
- Đảm bảo cơ chế cấp và thu hồi quyền hoạt động ổn định, tự động và đúng theo lifecycle đã thiết kế.
- Nâng cao khả năng giám sát hệ thống bằng cách triển khai logging và monitoring chuyên sâu với Amazon CloudWatch.
- Tối ưu hiệu năng và chi phí vận hành của các hàm AWS Lambda thông qua việc điều chỉnh runtime và memory phù hợp.
- Hoàn thiện tài liệu kỹ thuật và sơ đồ kiến trúc nhằm hỗ trợ quá trình bàn giao và bảo trì hệ thống.
- Rèn luyện kỹ năng trình bày và demo hệ thống thông qua buổi vận hành và review cuối cùng với nhóm.
- Tiếp tục mở rộng kiến thức về hệ sinh thái phân tích dữ liệu AWS, đặc biệt là mô hình Serverless Data Lake.
- Hiểu quy trình xử lý dữ liệu từ ingest, catalog, transform đến analysis và visualization trên AWS.
- Làm quen với việc xây dựng dashboard trực quan và tương tác bằng Amazon QuickSight.
- Nâng cao kỹ năng thiết kế hệ thống serverless kết hợp data analytics, event-driven architecture và visualization.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Kiểm thử tích hợp (Integration Test):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phối hợp với thành viên nhóm để test luồng: Jira Webhook → API Gateway → Lambda Executor → Identity Center<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra luồng thu hồi quyền tự động dựa trên các khoảng thời gian | 04/05/2026 | 04/05/2026 |  |
| 3 | - Tối ưu hóa & Logging:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình CloudWatch Logs chuyên sâu để theo dõi số lượng request thành công hay thất bại<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tối ưu hóa thời gian chạy (Runtime) và bộ nhớ của Lambda để giảm chi phí | 05/05/2026 | 05/05/2026 | [Amazon CloudWatch](https://aws.amazon.com/vi/cloudwatch/) |
| 4 | - Đóng gói và bàn giao:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Hoàn thiện tài liệu hướng dẫn về Codebase và sơ đồ logic của các hàm Lambda<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thực hiện buổi demo cuối cùng và vận hành dự án cùng các thành viên trong nhóm | 06/05/2026 | 06/05/2026 |  |
| 5 | - Thực hành làm quen với các dịch vụ phân tích dữ liệu trong hệ sinh thái AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Luồng xử lý dữ liệu (Data Pipeline): Xây dựng một kiến trúc Serverless Data Lake hoàn chỉnh, bao gồm các giai đoạn như **Ingest & Store** (Sử dụng Amazon S3 làm nền tảng lưu trữ chính cho Data Lake và Amazon Kinesis để xử lý dữ liệu truyền tải (streaming) theo thời gian thực) và **Catalog & Transform** (Sử dụng AWS Glue Crawler để tự động quét và tạo sơ đồ dữ liệu (Catalog))<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phân tích và hiển thị (Analysis & Visualization): Sau khi dữ liệu đã được xử lý và lưu trữ, ta sẽ học cách khai thác giá trị từ chúng bằng cách **Truy vấn** (Amazon Athena), tạo **Kho dữ liệu** (Amazon Redshift), **Trực quan hóa** (Amazon QuickSight), **Xử lý sự kiện** (AWS Lambda) | 07/05/2026 | 07/05/2026 | [Analytics on AWS workshop](https://000072.awsstudygroup.com/) |
| 6 | - Thực hành xây dựng một dashboard để visualize data:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị tài nguyên: Đăng nhập vào Amazon QuickSight, lựa chọn Region để thực hiện, chuẩn bị và kết nối các Data source (như S3, Athena) và tạo Dataset<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xây dựng Dashboard: Tạo một Analysis, thêm các Visual để biểu diễn dữ liệu<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cải tiến Dashboard: Tùy chỉnh màu sắc, định dạng số và nhãn, sử dụng các tính năng nâng cao để làm nổi bật các chỉ số quan trọng<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Dashboard tương tác: Thiết lập các bộ lọc và bộ điều khiển, cài đặt các tham số để thay đổi chế độ xem linh hoạt, tạo các action để khi click vào một biểu đồ thì các biểu đồ khác sẽ tự động cập nhật theo<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xuất bản và chia sẻ<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 08/05/2026 | 08/05/2026 | [Get started with Quick Sight](https://000073.awsstudygroup.com/) |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 09/05/2026 | 09/05/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 10/05/2026 | 10/05/2026 | |

### Kết quả đạt được tuần 11:

- Về kiến thức lý thuyết:
  - Hiểu rõ quy trình kiểm thử tích hợp (Integration Testing) trong hệ thống serverless đa dịch vụ trên AWS.
  - Nắm được cách sử dụng Amazon CloudWatch để theo dõi log, metric và giám sát trạng thái hoạt động của hệ thống.
  - Hiểu cách tối ưu hiệu năng Lambda thông qua điều chỉnh memory allocation và runtime execution.
  - Củng cố kiến thức về kiến trúc Serverless Data Lake và luồng xử lý dữ liệu trong hệ sinh thái Analytics on AWS.
  - Hiểu vai trò của các dịch vụ như Amazon Kinesis, AWS Glue, Athena, Redshift, QuickSight và Lambda trong pipeline phân tích dữ liệu.
  - Nắm được cách thiết kế dashboard trực quan, tương tác và hỗ trợ phân tích dữ liệu hiệu quả bằng Amazon QuickSight.
- Về thực hành (Hands-on Labs):
  - **Lab 72:** Thực hành xây dựng kiến trúc Serverless Data Lake hoàn chỉnh với S3, Kinesis, Glue, Athena, Redshift, QuickSight và Lambda.
  - **Lab 73:** Xây dựng dashboard trực quan với Amazon QuickSight, bao gồm tạo dataset, visualization, dashboard tương tác và chia sẻ báo cáo.
- Về phát triển hệ thống:
  - Thực hiện kiểm thử hoàn chỉnh luồng Jira Webhook → API Gateway → Lambda Executor → AWS Identity Center để đảm bảo hệ thống hoạt động ổn định.
  - Kiểm tra và xác nhận cơ chế revoke quyền tự động hoạt động đúng với các mốc thời gian TTL đã thiết kế.
  - Cấu hình CloudWatch Logs và theo dõi số lượng request thành công/thất bại nhằm hỗ trợ monitoring và troubleshooting.
  - Tối ưu hiệu năng của các Lambda functions nhằm cải thiện độ ổn định và giảm chi phí hệ thống.
  - Hoàn thiện tài liệu codebase, sơ đồ logic và hướng dẫn triển khai để phục vụ bàn giao dự án.
  - Thực hiện demo và vận hành thử nghiệm hệ thống cùng các thành viên trong nhóm.
- Về kỹ năng:
  - Nâng cao kỹ năng kiểm thử hệ thống tích hợp nhiều dịch vụ AWS.
  - Cải thiện khả năng monitoring, logging và troubleshooting trong môi trường serverless.
  - Phát triển kỹ năng tối ưu chi phí và hiệu năng hệ thống cloud.
  - Nâng cao khả năng thiết kế dashboard và trực quan hóa dữ liệu phục vụ phân tích.
  - Rèn luyện kỹ năng làm việc nhóm, trình bày hệ thống và bàn giao dự án thực tế.
  - Củng cố tư duy xây dựng hệ thống tự động hóa và event-driven trên AWS.
- Về quản lý tài nguyên:
  - Thực hiện dọn dẹp tài nguyên sau các bài thực hành để tránh phát sinh chi phí ngoài ý muốn.
  - Theo dõi mức sử dụng tài nguyên của Lambda, Athena, Kinesis và QuickSight để tối ưu chi phí.
  - Có ý thức quản lý tài nguyên và giám sát hiệu năng trong quá trình triển khai các hệ thống phân tích dữ liệu trên AWS.