---
title : "Worklog tuần 10"
date :  "`r Sys.Date()`" 
weight : 10
pre: <b> 1.10 </b>
chapter : false
---

### Mục tiêu tuần 10:

- Hiểu và thực hành quy trình phân tích chi phí và hiệu suất hệ thống trên AWS bằng Amazon Glue và Amazon Athena.
- Làm quen với cách xây dựng và vận hành data pipeline serverless trên AWS theo hướng Data Lake hiện đại.
- Củng cố kiến thức và kỹ năng làm việc với cơ sở dữ liệu NoSQL DynamoDB thông qua AWS SDK for Python (boto3).
- Hiểu cách tổ chức dữ liệu, tự động hóa ETL và trực quan hóa dữ liệu bằng QuickSight trong kiến trúc serverless.
- Hoàn thiện chức năng Email Token trong hệ thống quản lý quyền truy cập, bao gồm xử lý approve/reject trực tiếp từ email.
- Tích hợp cập nhật trạng thái tự động lên Jira ticket nhằm đồng bộ quy trình phê duyệt truy cập.
- Tăng độ ổn định và khả năng chịu lỗi của hệ thống bằng cách triển khai retry logic cho các API AWS/Jira.
- Xử lý các edge cases trong hệ thống như token hết hạn, user không tồn tại hoặc group bị xóa.
- Tiếp tục nâng cao tư duy thiết kế hệ thống serverless kết hợp automation, monitoring và fault tolerance trên AWS.
- Rèn luyện kỹ năng triển khai thực tế, quản lý tài nguyên và tối ưu chi phí khi làm việc với các dịch vụ dữ liệu AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Thực hành thiết lập hệ thống phân tích chi phí và hiệu suất với Amazon Glue và Amazon Athena:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Giới thiệu về Amazon Glue và Amazon Athena<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: Tạo S3 Bucket, thiết lập Cost and Usage Report, phân quyền IAM<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phân tích chi phí và hiệu suất: Sử dụng AWS Glue Crawler, truy vấn với Amazon Athena<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 27/04/2026 | 27/04/2026 | [Cost and performance analysis with AWS Glue and Amazon Athena](https://000040.awsstudygroup.com/) |
| 3 | - Thực hành làm quen với cơ sở dữ liệu NoSQL của AWS thông qua Python ([Module 07 - Lab 60](https://drive.google.com/drive/folders/19P-MVrC5ksfOOYbEyYGOz9-zro-sqTK2?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Giới thiệu về Amazon DynamoDB<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: Cấu hình AWS Credentials, cài đặt thư viện boto3<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Bắt đầu với AWS SDK: Tạo bảng, thêm dữ liệu, truy vấn dữ liệu, cập nhật và xóa<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 28/04/2026 | 28/04/2026 | [Work with Amazon DynamoDB](https://000060.awsstudygroup.com/) |
| 4 | - Thực hành xây dựng một data pipeline serverless:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: Khởi tạo các quyền IAM cần thiết, tạo các S3 Bucket, thiết lập Availability Zone thực hiện<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị dữ liệu: Tải dữ liệu mẫu lên AWS S3<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Nạp dữ liệu với AWS Glue<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xây dựng Data Pipeline<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Truy vấn với Amazon Athena<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Trực quan hóa với Amazon QuickSight<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 29/04/2026 | 29/04/2026 | [Building a Datalake with Your Data](https://000070.awsstudygroup.com/) |
| 5 | - Hoàn thiện Logic Email Token:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phát triển hàm xử lý link phê duyệt/từ chối từ email<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Viết logic cập nhật trạng thái trực tiếp lên Jira ticket khi Manager nhấn nút Approve/Reject. | 30/04/2026 | 30/04/2026 |  |
| 6 | - Xử lý lỗi và Edge Cases:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Viết logic retry cho các lần gọi API AWS/Jira thất bại<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Xử lý các tình huống đặc biệt: User đã bị xóa, Group không tồn tại, hoặc lỗi token hết hạn. | 01/05/2026 | 01/05/2026 |  |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 02/05/2026 | 02/05/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 03/05/2026 | 03/05/2026 | |

### Kết quả đạt được tuần 10:

- Về kiến thức lý thuyết:
  - Hiểu được quy trình xây dựng hệ thống phân tích chi phí và hiệu suất trên AWS bằng cách kết hợp Amazon Glue, Athena và Cost & Usage Report.
  - Nắm được vai trò của AWS Glue trong việc crawl dữ liệu, tạo metadata catalog và hỗ trợ truy vấn dữ liệu bằng Athena.
  - Hiểu cách hoạt động của mô hình Data Pipeline serverless trên AWS, bao gồm lưu trữ, ETL, truy vấn và trực quan hóa dữ liệu.
  - Củng cố kiến thức về DynamoDB và cách thao tác với NoSQL database thông qua AWS SDK for Python (boto3).
  - Hiểu rõ hơn về cơ chế xử lý lỗi, retry strategy và các tình huống edge cases trong hệ thống phân tán.
- Về thực hành (Hands-on Labs):
  - **Lab 40:** Thực hành xây dựng hệ thống phân tích chi phí và hiệu suất bằng AWS Glue và Amazon Athena, bao gồm cấu hình Cost & Usage Report, Glue Crawler và truy vấn dữ liệu.
  - **Lab 60:** Thực hành làm việc với Amazon DynamoDB bằng Python và boto3, bao gồm tạo bảng, thêm dữ liệu, truy vấn, cập nhật và xóa dữ liệu.
  - **Lab 70:** Xây dựng hoàn chỉnh một data pipeline serverless trên AWS với S3, Glue, Athena và QuickSight để xử lý và trực quan hóa dữ liệu.
- Về phát triển hệ thống:
  - Hoàn thiện logic Email Token phục vụ quy trình approve/reject quyền truy cập trực tiếp từ email.
  - Xây dựng chức năng cập nhật trạng thái Jira ticket tự động khi Manager thực hiện thao tác phê duyệt hoặc từ chối.
  - Triển khai retry logic cho các lời gọi API AWS và Jira nhằm tăng độ ổn định hệ thống khi gặp lỗi tạm thời.
  - Xử lý các edge cases quan trọng như token hết hạn, user không tồn tại hoặc group đã bị xóa khỏi hệ thống.
  - Cải thiện khả năng fault tolerance và tính tự động hóa trong workflow quản lý quyền truy cập.
- Về kỹ năng:
  - Nâng cao kỹ năng xây dựng và vận hành hệ thống dữ liệu serverless trên AWS.
  - Cải thiện khả năng làm việc với DynamoDB và AWS SDK thông qua Python.
  - Phát triển tư duy thiết kế workflow có khả năng chịu lỗi và xử lý exception hiệu quả.
  - Củng cố kỹ năng tích hợp nhiều dịch vụ AWS trong cùng một hệ thống thực tế.
  - Nâng cao khả năng đọc tài liệu kỹ thuật, triển khai lab và áp dụng kiến thức vào dự án.
- Về quản lý tài nguyên:
  - Thực hiện dọn dẹp tài nguyên sau các bài lab để tránh phát sinh chi phí ngoài ý muốn.
  - Theo dõi và tối ưu tài nguyên sử dụng trong quá trình triển khai Glue, Athena, S3 và QuickSight.
  - Có ý thức kiểm soát chi phí và hiệu suất khi xây dựng các hệ thống phân tích dữ liệu trên AWS.
