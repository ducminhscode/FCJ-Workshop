---
title : "Worklog tuần 8"
date :  "`r Sys.Date()`" 
weight : 8
pre: <b> 1.8 </b>
chapter : false
---

### Mục tiêu tuần 8:

- Nắm vững các khái niệm cốt lõi về cơ sở dữ liệu, bao gồm mô hình quan hệ, NoSQL, cơ chế tối ưu và cách phân biệt OLTP và OLAP.
- Hiểu rõ kiến trúc, đặc điểm và trường hợp sử dụng của các dịch vụ cơ sở dữ liệu trên AWS như Amazon RDS, Aurora, Redshift và ElastiCache.
- So sánh được các dịch vụ database để lựa chọn phù hợp với từng bài toán thực tế.
- Thực hành di chuyển cơ sở dữ liệu lên AWS bằng DMS, hiểu quy trình migration và cách xử lý các vấn đề phát sinh.
- Triển khai hoàn chỉnh một hệ thống database trên AWS, từ hạ tầng mạng đến kết nối ứng dụng.
- Tiếp tục phát triển hệ thống serverless bằng cách xây dựng Lambda Executor, xử lý request từ API Gateway hoặc webhook.
- Tích hợp các dịch vụ AWS như Secrets Manager, IAM Identity Center và SES vào workflow để tự động hóa việc cấp quyền truy cập.
- Hoàn thiện logic xử lý, kiểm tra dữ liệu đầu vào và cơ chế xử lý lỗi trong hệ thống.
- Nâng cao khả năng thiết kế và triển khai hệ thống backend thực tế, kết hợp giữa database, serverless và các dịch vụ AWS liên quan.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Tổng quan về các khái niệm cơ bản của cơ sở dữ liệu trước khi đi sâu vào các dịch vụ cụ thể của AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm cơ bản về cơ sở dữ liệu<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các thành phần chính trong Database quan hệ: Primary Key, Foreign Key, chuẩn hóa, Index, Partition<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cơ chế vận hành và tối ưu: Execution Plan, Database Log, Buffer<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phân loại cơ sở dữ liệu: RDBMS, NoSQL, OLTP vs OLAP<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các dịch vụ AWS sẽ học trong tuần: Amazon RDS, Amazon Aurora, Amazon ElastiCache, Amazon Redshift<br>- Giới thiệu về hai dịch vụ cơ sở dữ liệu quan hệ quan trọng trên AWS là Amazon RDS và Amazon Aurora<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon RDS: Khái niệm, lợi ích, các tính năng nổi bật<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon Aurora: Khái niệm, kiến trúc đặc biệt<br>&nbsp;&nbsp;&nbsp;&nbsp;+ So sánh và ứng dụng<br>- Giới thiệu về hai dịch vụ dữ liệu quan trọng của AWS là Amazon Redshift và Amazon ElastiCache<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon Redshift: Kiến trúc MPP, lưu trữ dạng cột, tối ưu chi phí<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon ElastiCache: Hai Engine hỗ trợ (Redis, Memcached), lợi ích<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Lộ trình thực hành và tài liệu bổ sung | 13/04/2026 | 13/04/2026 | [Database Concepts review](https://youtu.be/OOD2RwWuLRw?si=4sj2X9mO-rr-6Tog)<br>[Amazon RDS & Amazon Aurora](https://youtu.be/qbrobQZrokY?si=jrfgL-muOLWaAoDX)<br>[Redshift - Elasticache](https://youtu.be/UvdiRW34aNI?si=fsusJsJ6ziH5ufaS) |
| 3 | - Thực hành di chuyển cơ sở dữ liệu từ các hệ quản trị nguồn sang các đích đến trên AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập nguồn (Source) và đích (Target) cho DMS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai sao chép không máy chủ (Serverless replication)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Theo dõi và khắc phục sự cố trong quá trình di cư<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 14/04/2026 | 14/04/2026 | [Database Schema Conversion & Migration](https://000043.awsstudygroup.com/) |
| 4 | - Thực hành nắm vững cách triển khai một hệ thống cơ sở dữ liệu hoàn chỉnh ([Module 06 - Lab 05](https://drive.google.com/drive/folders/1-4v8yKN1CbKdEZ_K0_nngmkcv6G6i__G?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: Khởi tạo VPC, thiết lập các Subnets và Route Tables, tạo Security Group<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo EC2 Instance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo RDS Database Instance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai ứng dụng: Kết nối ứng dụng trên EC2 tới RDS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Quản lý sao lưu và khôi phục<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 15/04/2026 | 15/04/2026 | [Amazon Relational Database Service (Amazon RDS)](https://000005.awsstudygroup.com/) |
| 5 | - Phát triển Lambda Executor:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Viết logic chính cho hàm provision-access: Nhận payload từ API Gateway/Jira Webhook<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra tính hợp lệ của Request và tra cứu Group Mapping trong Secrets Manager | 16/04/2026 | 16/04/2026 | [AWS Secrets Manager](https://aws.amazon.com/vi/secrets-manager/) |
| 6 | - Phát triển Lambda Executor:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thực hiện gọi AWS SDK để thêm User vào IAM Identity Center Group<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xử lý trường hợp lỗi (error handling) và gửi thông báo kết quả qua SES cho người yêu cầu | 17/04/2026 | 17/04/2026 | [AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html)<br>[AWS SES](https://aws.amazon.com/vi/ses/) |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 18/04/2026 | 18/04/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 19/04/2026 | 19/04/2026 | |

### Kết quả đạt được tuần 8:

- Về kiến thức lý thuyết:
  - Nắm vững các khái niệm nền tảng về cơ sở dữ liệu, bao gồm mô hình quan hệ, NoSQL, Primary Key, Foreign Key, chuẩn hóa, Index và Partition.
  - Hiểu cơ chế hoạt động của hệ quản trị cơ sở dữ liệu như Execution Plan, Database Log và Buffer.
  - Phân biệt rõ giữa OLTP và OLAP, cũng như cách áp dụng trong từng bài toán thực tế.
  - Hiểu kiến trúc, đặc điểm và use case của các dịch vụ database trên AWS như Amazon RDS, Aurora, Redshift và ElastiCache.
  - So sánh được các dịch vụ để lựa chọn phù hợp theo yêu cầu về hiệu năng, chi phí và mục đích sử dụng.
- Về thực hành (Hands-on Labs):
  - **Lab 43:** Thực hành di chuyển cơ sở dữ liệu bằng AWS DMS, bao gồm thiết lập source/target, cấu hình replication và theo dõi quá trình migration.
  - **Lab 05:** Triển khai hệ thống database hoàn chỉnh trên AWS, từ cấu hình VPC, Subnet, Security Group đến tạo RDS instance và kết nối ứng dụng trên EC2.
- Về phát triển hệ thống:
  - Xây dựng Lambda Executor để xử lý request từ API Gateway hoặc Jira Webhook.
  - Tích hợp AWS Secrets Manager để quản lý thông tin cấu hình (group mapping) một cách bảo mật và linh hoạt.
  - Thực hiện gọi AWS SDK để thêm user vào IAM Identity Center Group.
  - Xây dựng cơ chế xử lý lỗi (error handling) và phản hồi kết quả.
  - Tích hợp Amazon SES để gửi thông báo kết quả cấp quyền tới người dùng.
- Về kỹ năng:
  - Nâng cao khả năng thiết kế và triển khai hệ thống database trên AWS.
  - Cải thiện kỹ năng làm việc với các dịch vụ database khác nhau và hiểu rõ cách lựa chọn phù hợp.
  - Rèn luyện kỹ năng phân tích và xử lý bài toán migration dữ liệu trong thực tế.
  - Phát triển kỹ năng xây dựng backend serverless kết hợp nhiều dịch vụ AWS.
  - Cải thiện khả năng đọc hiểu tài liệu kỹ thuật và áp dụng vào triển khai thực tế.
- Về quản lý tài nguyên:
  - Thực hiện dọn dẹp tài nguyên sau mỗi bài lab để tránh phát sinh chi phí.
  - Có ý thức theo dõi và tối ưu tài nguyên khi làm việc với các dịch vụ database trên AWS.