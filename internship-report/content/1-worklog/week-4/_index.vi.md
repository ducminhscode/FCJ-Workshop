---
title : "Worklog tuần 4"
date :  "`r Sys.Date()`" 
weight : 4
pre: <b> 1.4 </b>
chapter : false
---

### Mục tiêu tuần 4:

- Hiểu rõ các dịch vụ lưu trữ trên AWS, đặc biệt là Amazon S3, bao gồm kiến trúc object storage, các lớp lưu trữ (storage classes) và cách tối ưu chi phí lưu trữ.
- Nắm vững các tính năng nâng cao của S3 như Static Website Hosting, CORS, Access Control (IAM, Bucket Policy, ACL), Versioning và VPC Endpoint.
- Tìm hiểu các giải pháp lưu trữ dữ liệu dài hạn với chi phí thấp như Amazon S3 Glacier và các cơ chế truy xuất dữ liệu.
- Nghiên cứu các giải pháp di chuyển dữ liệu và lưu trữ hybrid trên AWS như AWS Snow Family và AWS Storage Gateway.
- Hiểu rõ các chiến lược Disaster Recovery (DR) và cơ chế sao lưu dữ liệu tập trung với AWS Backup.
- Thực hành triển khai và quản lý hệ thống backup, bao gồm backup và restore dữ liệu trên nhiều dịch vụ AWS (EBS, RDS, DynamoDB).
- Nâng cao hiểu biết về kiến trúc serverless và multi-tenant SaaS, đặc biệt là cơ chế tenant isolation trong AWS Lambda thông qua việc dịch blog chuyên sâu.
- Rèn luyện kỹ năng đọc hiểu tài liệu kỹ thuật và dịch thuật các nội dung chuyên ngành Cloud/AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Xem giới thiệu về các dịch vụ lưu trữ trên nền tảng AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dịch vụ Object Storage: Đi sâu vào Amazon S3 (Simple Storage Service) - một trong những dịch vụ phổ biến và quan trọng nhất của AWS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các giải pháp kết nối và di chuyển dữ liệu: Amazon Storage Gateway, AWS Snow Family<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cold Storage: Tìm hiểu về các giải pháp lưu trữ dữ liệu ít truy cập với chi phí thấp (như S3 Glacier)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Disaster Recovery: Cách thiết kế và xây dựng môi trường dự phòng trên AWS để đảm bảo tính liên tục của doanh nghiệp<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sao lưu dữ liệu: Sử dụng dịch vụ AWS Backup để quản lý và tự động hóa việc sao lưu trên nhiều dịch vụ AWS khác nhau<br>- Đi sâu vào các khái niệm cốt lõi, đặc tính kỹ thuật và các lớp lưu trữ của S3:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm Object Storage: Đơn vị lưu trữ, Mô hình WORM<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Đặc tính kỹ thuật nổi bật: Dung lượng, độ bền và độ sẵn sàng, nhân bản dữ liệu, S3 Event Trigger<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cách thức truy cập dữ liệu: Giao thức HTTP/HTTPS (REST API), cấu trúc Key-Value<br>&nbsp;&nbsp;&nbsp;&nbsp;+ S3 Access Point: Quản lý quyền truy cập dễ dàng hơn khi có nhiều ứng dụng cùng truy cập vào một Bucket<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các lớp lưu trữ và tối ưu chi phí<br>- Tìm hiểu các tính năng nâng cao của Amazon S3 và dịch vụ lưu trữ Glacier:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ S3 Static Website Hosting và CORS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm soát quyền truy cập: ACL (Access Control List), IAM & Bucket Policy<br>&nbsp;&nbsp;&nbsp;&nbsp;+ S3 VPC Endpoint<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Versioning và bảo mật dữ liệu<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Hiệu năng và Object Key<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon S3 Glacier: Mục đích, cơ chế truy xuất, Glacier Vault Lock<br>- Tiếp cận các giải pháp di chuyển dữ liệu, lưu trữ Hybrid và chiến lược phục hồi sau thảm họa trên AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Snow Family: Snowball, Snowball Edge, Snowmobile<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Storage Gateway: Giải pháp lưu trữ Hybrid kết hợp giữa hạ tầng tại chỗ (On-premises) và Cloud<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Disaster Recovery (DR) trên AWS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Backup | 16/03/2026 | 16/03/2026 | [Dịch Vụ Lưu Trữ Trên AWS](https://youtu.be/hsCfP0IxoaM?si=0HbBG1CLbHeK-08Y)<br>[Amazon Simple Storage Service ( S3 ) - Access Point - Storage Class](https://youtu.be/_yunukwcAwc?si=T-PUjhOY_lwk_WbL)<br>[S3 Static Website & CORS - Control Access - Object Key & Performance - Glacier](https://youtu.be/mPBjB6Ltl_Q?si=G1x-LC4YsbRRat8G)<br>[Snow Family - Storage Gateway - Backup](https://youtu.be/YXn8Q_Hpsu4?si=DIOoYiT883xx-3Ya) |
| 3 | - Thực hành triển khai AWS Backup cho hệ thống ([Module 04 - Lab 13](https://drive.google.com/drive/folders/1xGjj8feG0K4l9q4sOejxJuxYpPim-pLR?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị hạ tầng: Tạo S3 Bucket, khởi tạo máy chủ EC2, cơ sở dữ liệu RDS, bảng DynamoDB<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Backup Plan: Thiết lập Backup Plan, Gán tài nguyên<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình thông báo: Amazon SNS, liên kết thông báo<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra và phục hồi: Backup thủ công, thực hành Restore<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 17/03/2026 | 17/03/2026 | [Deploy AWS Backup to the System](https://000013.awsstudygroup.com/) |
| 4 | - Nghiên cứu và dịch bài blog **Building multi-tenant SaaS applications with AWS Lambda’s new tenant isolation mode**<br>- Phân tích kiến trúc multi-tenant SaaS và các phương pháp isolation (shared function vs function-per-tenant)<br>- Tìm hiểu cơ chế hoạt động của **AWS Lambda tenant isolation mode**: tenant-id routing, execution environment isolation, reuse theo tenant<br>- Dịch nội dung về các lợi ích (bảo mật, đơn giản hóa kiến trúc) và trade-offs (cold start, chi phí)<br>- Rà soát thuật ngữ kỹ thuật và chỉnh sửa bản dịch đảm bảo tính chính xác và tự nhiên | 18/03/2026 | 18/03/2026 | [Blog 02](https://aws.amazon.com/vi/blogs/compute/building-multi-tenant-saas-applications-with-aws-lambdas-new-tenant-isolation-mode/) |
| 5 | - Thực hành sao lưu và phục hồi - Amazon EBS ([Module 04 - Lab 14](https://drive.google.com/drive/folders/1lZFzO_z09Y4nyMCv14j3N0yw-LlWvfXi?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: EC2 Instance, gắn ổ đĩa EBS, tạo dữ liệu mẫu<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sao lưu dữ liệu với EBS Snapshots<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tự động hóa với Amazon Data Lifecycle Manager (DLM): Thiết lập Lifecycle Policy, cấu hình Retention, gán Tag<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phục hồi dữ liệu: Tạo ổ đĩa từ Snapshot, gắn và kiểm tra, mở rộng dung lượng<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 19/03/2026 | 19/03/2026 | [VM Import/Export](https://000014.awsstudygroup.com/) |
| 6 | - Thực hành triển khai AWS Storage Gateway (Tài khoản Free Tier không thể thực hiện được) ([Module 04 - Lab 24](https://drive.google.com/drive/folders/1XCUuNDfmr1AnilJTwEEONTN-XD7nTZhx?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị hạ tầng: S3 Bucket, triển khai Gateway Appliance (EC2)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kích hoạt AWS Storage Gateway: Kết nối Gateway, cấu hình Local Cache<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo File Share<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kết nối và kiểm tra từ phía Client: Mount File Share, thử nghiệm đồng bộ<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 20/03/2026 | 20/03/2026 | [Using File Storage Gateway](https://000024.awsstudygroup.com/) |
| 7 | - Tham gia buổi gặp gỡ cộng đồng **AWS First Cloud AI Journey Community Day 2026** (kick-off FCAJ Bootcamp 2026) để kết nối với các chuyên gia Cloud, khám phá những ứng dụng mới của Cloud & Generative AI, cùng trải nghiệm các demo thực tế và mở rộng networking trong hệ sinh thái công nghệ<br>- Họp trực tiếp cùng các thành viên trong nhóm để thảo luận và thống nhất các phương án triển khai dự án | 21/03/2026 | 21/03/2026 | ![team](/images/team.jpg) [AWS First Cloud AI Journey Community Day 2026](/4-events/event-2/) |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 22/03/2026 | 22/03/2026 | |

### Kết quả đạt được tuần 4:

- Về kiến thức lý thuyết:
  - Hiểu rõ kiến trúc Object Storage của Amazon S3, bao gồm cách tổ chức dữ liệu theo mô hình key-value và các đặc tính như độ bền (durability), độ sẵn sàng (availability) và khả năng mở rộng.
  - Nắm được các storage classes của S3 và cách lựa chọn phù hợp để tối ưu chi phí theo từng nhu cầu sử dụng dữ liệu.
  - Hiểu rõ các cơ chế bảo mật và kiểm soát truy cập trong S3 như IAM Policy, Bucket Policy, ACL và S3 Access Point.
  - Phân biệt các giải pháp lưu trữ dài hạn như Amazon S3 Glacier, bao gồm các mức độ truy xuất và chi phí tương ứng.
  - Hiểu được các giải pháp Hybrid Storage với AWS Storage Gateway và cơ chế di chuyển dữ liệu quy mô lớn với AWS Snow Family.
  - Nắm được quy trình thiết kế hệ thống Disaster Recovery (DR) và cách sử dụng AWS Backup để quản lý sao lưu tập trung.
  - Hiểu rõ kiến trúc multi-tenant SaaS trên serverless, đặc biệt là cơ chế tenant isolation mode của AWS Lambda, giúp tăng cường bảo mật giữa các tenant và đơn giản hóa thiết kế hệ thống.
  - Phân tích được trade-offs giữa bảo mật và chi phí/hiệu năng (cold start, resource isolation) trong kiến trúc serverless.
- Về thực hành (Hands-on Labs):
  - **Lab 13:** Triển khai thành công hệ thống backup với AWS Backup, tạo Backup Plan, cấu hình thông báo và thực hiện restore dữ liệu.
  - **Lab 14:** Thực hành sao lưu và phục hồi dữ liệu với Amazon EBS Snapshot, sử dụng Data Lifecycle Manager (DLM) để tự động hóa backup.
  - **Lab 24:** Tìm hiểu và triển khai mô hình lưu trữ hybrid với AWS Storage Gateway, thiết lập File Share kết nối giữa hệ thống local và S3.
  - Thực hành kiểm tra, xác minh dữ liệu và dọn dẹp tài nguyên sau mỗi lab để tối ưu chi phí.
- Về kỹ năng:
  - Nâng cao khả năng triển khai và quản lý hệ thống lưu trữ trên AWS, từ object storage đến hybrid storage.
  - Có khả năng thiết lập và vận hành hệ thống backup & recovery cho nhiều dịch vụ AWS.
  - Rèn luyện kỹ năng phân tích kiến trúc hệ thống Cloud, đặc biệt là serverless và multi-tenant.
  - Cải thiện kỹ năng đọc hiểu tài liệu kỹ thuật tiếng Anh và dịch thuật chuyên ngành AWS.
  - Phát triển tư duy tối ưu chi phí và hiệu năng khi thiết kế hệ thống trên cloud.
- Về hoạt động cộng đồng và làm việc nhóm:
  - Tham gia sự kiện AWS First Cloud AI Journey Community Day 2026, mở rộng networking với cộng đồng Cloud và cập nhật các xu hướng mới về Generative AI và Cloud Computing.
  - Trao đổi và làm việc nhóm để thống nhất các phương án triển khai dự án, cải thiện kỹ năng giao tiếp và teamwork.
- Về quản lý tài nguyên:
  - Thực hiện tốt việc dọn dẹp tài nguyên AWS sau mỗi bài lab, đảm bảo không phát sinh chi phí ngoài ý muốn.
  - Có ý thức tối ưu tài nguyên và sử dụng dịch vụ trong giới hạn Free Tier khi thực hành.
