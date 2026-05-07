---
title : "Worklog tuần 11"
date :  "`r Sys.Date()`" 
weight : 11
pre: <b> 1.11 </b>
chapter : false
---

### Mục tiêu tuần 11:


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
