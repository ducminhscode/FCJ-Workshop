---
title : "Worklog tuần 1"
date :  "`r Sys.Date()`" 
weight : 1 
pre: <b> 1.1 </b>
chapter : false
---

### Mục tiêu tuần 1:
- Làm quen với môi trường thực tập tại AWS First Cloud Journey (FCJ), nắm rõ quy trình làm việc, cách quản lý task và báo cáo tiến độ.
- Trang bị kiến thức nền tảng về Cloud Computing và các mô hình dịch vụ (IaaS, PaaS, SaaS).
- Hiểu rõ sự khác biệt giữa mô hình On-premise và Cloud, cũng như lợi ích của điện toán đám mây.
- Tạo và cấu hình thành công tài khoản AWS Free Tier, đảm bảo tuân thủ các nguyên tắc bảo mật (MFA, IAM).
- Làm quen với AWS Management Console và AWS CLI để quản lý tài nguyên.
- Bước đầu tiếp cận AWS Global Infrastructure, tối ưu chi phí và AWS Support.
- Làm quen với Hugo để xây dựng website báo cáo thực tập cá nhân.
- Thực hành các lab cơ bản trong Module 01 và áp dụng kiến thức vào thực tế.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Làm quen với các thành viên FCJ<br>- Xem video giới thiệu chính thức **First Cloud Journey Kick-off** về tổ chức và định hướng đào tạo<br>- Đọc kỹ, lưu ý các nội dung, quy định làm việc, cách báo cáo tiến độ và quản lý task tại AWS First Cloud Journey Internship | 23/02/2026 | 23/02/2026 | [First Cloud Journey Kick-off](https://youtu.be/AQlsd0nWdZk?si=LrHqeTag2kuwwjIK)<br>[Github Repository](https://github.com/AWS-First-Cloud-Journey/Internship) |
| 3 | - Cài đặt các công cụ cần thiết: Git, VS Code, AWS CLI, SSH client<br>- Nghiên cứu các mô hình dịch vụ:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ IaaS (Infrastructure as a Service)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ PaaS (Platform as a Service)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ SaaS (Software as a Service)<br>- So sánh mô hình On-premise và Cloud<br>- Tìm hiểu lợi ích của điện toán đám mây: scalability, high availability, cost optimization | 24/02/2026 | 24/02/2026 | [Tutorial Git](https://dev.to/womakerscode/tutorial-git-acesso-ao-github-com-ssh-4kg9?utm_source=chatgpt.com)<br>[VS Code](https://code.visualstudio.com/docs/remote/ssh-tutorial)<br>[AWS CLI](https://github.com/aws/aws-cli?utm_source=chatgpt.com)<br>[SSH Client](https://aws.amazon.com/vi/what-is/cli/?utm_source=chatgpt.com)<br>[Cloud Computing Models](https://www.ssh.com/academy/cloud/computing-models?utm_source=chatgpt.com)<br>[On-premise & Cloud](https://www.cleo.com/blog/knowledge-base-on-premise-vs-cloud?utm_source=chatgpt.com) |
| 4 | - Tạo và cấu hình tài khoản AWS Free Tier<br>- Thiết lập MFA cho tài khoản AWS<br>- Tạo Admin Group và Admin User<br>- Khám phá và cấu hình AWS Management Console & AWS CLI<br>- Tạo và quản lý các trường hợp hỗ trợ trong AWS<br>- [Thực hành Module 01 - Lab 01](https://drive.google.com/drive/folders/1z9YIhelvzdy-Navy4ec6YVc0IlIr74Y7?usp=drive_link) | 25/02/2026 | 25/02/2026 | [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)<br>[Creating Your First AWS Account](https://000001.awsstudygroup.com/)<br>[Getting Started with the AWS Management Console](https://docs.aws.amazon.com/hands-on/latest/getting-started-with-aws-management-console/getting-started-with-aws-management-console.html)<br>[Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html?utm_source=chatgpt.com) |
| 5 | - Nghiên cứu và cài đặt Hugo<br>- Tìm hiểu cấu trúc project và bố cục giao diện trong Hugo<br>- Thực hành chỉnh sửa menu và logo từ Theme<br>- Khởi tạo và chạy website tĩnh trên localhost | 26/02/2026 | 26/02/2026 | [Hugo Introduction](https://learn.netlify.app/en/)<br>[Hugo Guides](https://van-hoang-kha.github.io/)<br>[Sample Internship Report](https://workshop-sample.fcjuni.com/)<br>[AWS Workshop Guide](https://youtu.be/mXRqgMr_97U?si=Vl6aZ_dlWKkpJLJO) |
| 6 | - Tìm hiểu khái niệm Cloud Computing và sự khác biệt mà AWS tạo nên<br> - Hiểu rõ AWS Global Infrastructure (Region, AZ, Edge Location)<br>- Sử dụng công cụ quản lý AWS services<br>- Tối ưu hóa chi phí trên AWS và làm việc với AWS Support<br>- Xem tổng quan bài thực hành và nghiên cứu bổ sung<br>- Tập thiết kế một vài mẫu sơ đồ kiến trúc AWS trên [draw.io](https://app.diagrams.net/) | 27/02/2026 | 27/02/2026 | [Cloud Computing](https://youtu.be/HxYZAK1coOI?si=X-8BVLO9nDDZJ_CW)<br>[AWS Differences](https://youtu.be/IK59Zdd1poE?si=PGYDpXUKUzJOUjEk)<br>[Cloud Start](https://youtu.be/HSzrWGqo3ME?si=D5U7nENLOij67aHP)<br>[AWS Global Infrastructure](https://youtu.be/pjr5a-HYAjI?si=AXuTURzZ45fIsjFW)<br>[Management AWS Services](https://youtu.be/2PQYqH_HkXw?si=Z__aJdeFJ8TemItt)<br>[Cost Optimization & AWS Support](https://youtu.be/IY61YlmXQe8?si=ygtX2n35bl5B7V3j)<br>[Practice & Research](https://youtu.be/Hku7exDBURo?si=3ubdpTXhEKQtlnq-)<br>[AWS Architectural Design Guide](https://youtu.be/l8isyDe-GwY?si=oycMhtJbg209ZWLT) |
| 7 | - Thực hành tạo AWS Budgets ([Module 01 - Lab 07](https://drive.google.com/drive/folders/1fOBq_rkLVNqv-0Fb6-pEcXMjIYy0MvP5?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cost Budget<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Budget Template<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tìm hiểu Usage Budget, Reserved Instances Budget, Savings Plans Budget<br>- Thực hành tạo AWS Support & Support Case ([Module 01 - Lab 09](https://drive.google.com/drive/folders/170kC9oeZSh8VWXcbM2aNPEloQoJcNFRp?usp=drive_link))<br>- Nghiên cứu AWS Well-Architected Framework | 28/02/2026 | 28/02/2026 | [Cost Management with AWS Budget](https://000007.awsstudygroup.com/)<br>[Request Support with AWS Support](https://000009.awsstudygroup.com/)<br>[AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/) |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 01/03/2026 | 01/03/2026 | |

### Kết quả đạt được tuần 1:

- Làm quen môi trường thực tập FCJ:
  - Hiểu quy trình làm việc, cách quản lý task và báo cáo tiến độ.
  - Nắm rõ định hướng đào tạo và lộ trình học tập trong chương trình.

- Cài đặt và cấu hình môi trường làm việc:
  - Cài đặt Git và cấu hình SSH với GitHub.
  - Cài đặt VS Code và thiết lập môi trường phát triển.
  - Cài đặt và cấu hình AWS CLI.
  - Sử dụng SSH Client để kết nối máy chủ.

- Nắm vững kiến thức nền tảng Cloud Computing:
  - Phân biệt được IaaS, PaaS, SaaS.
  - Sư khác biệt giữa mô hình On-premise và Cloud.
  - Hiểu các lợi ích: Scalability, High Availability, Cost Optimization.

- Thiết lập và bảo mật tài khoản AWS:
  - Tạo và cấu hình thành công tài khoản AWS Free Tier.
  - Bật MFA cho root account.
  - Tạo Admin Group và Admin User theo nguyên tắc bảo mật IAM.

- Làm quen AWS Management Console và AWS CLI:
  - Thực hành quản lý IAM.
  - Khám phá EC2, S3.
  - Làm quen Billing Dashboard và Support Center.
  - Thực hành thành công các lệnh CLI cơ bản để kiểm tra và quản lý tài nguyên.

- Tìm hiểu AWS Global Infrastructure:
  - Hiểu khái niệm Region.
  - Hiểu Availability Zone (AZ).
  - Hiểu Edge Location.
  - Nhận biết cách AWS triển khai hạ tầng toàn cầu.

- Thực hành quản lý chi phí và hỗ trợ:
  - Tạo AWS Budgets (Cost Budget).
  - Tìm hiểu thêm về Usage Budget, Reserved Instances Budget, Savings Plans Budget.
  - Tạo và quản lý Support Case trên AWS.

- Nghiên cứu AWS Well-Architected Framework:
  - Hiểu 6 trụ cột: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability.
  - Bước đầu áp dụng tư duy kiến trúc chuẩn khi thiết kế hệ thống.

- Làm quen và xây dựng website báo cáo bằng Hugo:
  - Cài đặt Hugo.
  - Tìm hiểu cấu trúc project (content, layouts, static, themes).
  - Tùy chỉnh menu và logo từ theme.
  - Chạy website tĩnh thành công trên localhost.

- Thực hành thiết kế kiến trúc:
  - Sử dụng draw.io để vẽ sơ đồ kiến trúc AWS cơ bản.
  - Bước đầu hình dung mô hình triển khai hệ thống trên Cloud.
