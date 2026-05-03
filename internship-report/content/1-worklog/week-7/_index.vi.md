---
title : "Worklog tuần 7"
date :  "`r Sys.Date()`" 
weight : 7
pre: <b> 1.7 </b>
chapter : false
---

### Mục tiêu tuần 7:

- Nắm vững cách giới hạn quyền người dùng trong AWS IAM bằng Permission Boundary, hiểu rõ khi nào nên áp dụng để kiểm soát quyền hiệu quả hơn so với policy thông thường.
- Hiểu và thực hành bảo vệ dữ liệu nhạy cảm thông qua mã hóa khi lưu trữ, sử dụng AWS KMS để quản lý khóa và kiểm soát truy cập.
- Làm chủ cách phân quyền linh hoạt trong IAM bằng cách sử dụng Condition (IP, thời gian, tag), thay vì chỉ dùng quyền tĩnh.
- Biết cách cấp quyền an toàn cho ứng dụng truy cập tài nguyên AWS thông qua IAM Role, tránh sử dụng Access Key trực tiếp.
- Tiếp cận tư duy thiết kế hệ thống hiện đại qua việc nghiên cứu kiến trúc hỗ trợ agentic AI development trên AWS, bao gồm tổ chức code, testing và môi trường phát triển.
- Củng cố thói quen thực hành, kiểm tra và dọn dẹp tài nguyên sau mỗi lab để tối ưu chi phí và đảm bảo môi trường sạch.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Thực hành giới hạn quyền hạn người dùng bằng IAM Permission Boundary ([Module 05 - Lab 30](https://drive.google.com/drive/folders/1v_kjFM7quYAqDmXRfyDXxrWMCtlG03lh?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Giới thiệu về Permission Boundary<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị tài khoản AWS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo chính sách giới hạn (Permission Boundary)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo IAM Limited User, gán quyền và áp dụng Permission Boundary đã tạo ở trên<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra giới hạn quyền<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 06/04/2026 | 06/04/2026 | [Limitation of user rights with IAM Permission Boundary](https://000030.awsstudygroup.com/) |
| 3 | - Thực hành bảo vệ dữ liệu nhạy cảm bằng cách mã hóa dữ liệu khi lưu trữ trên AWS ([Module 05 - Lab 33](https://drive.google.com/drive/folders/1JpPL7tYB1Hw8QPkuWPGp0sPLZZ6BhAJC?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo Customer Managed Key<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Mã hóa tài nguyên lưu trữ: Amazon EBS và Amazon S3<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra tính minh bạch và quyền truy cập<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Giám sát qua AWS CloudTrail<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 07/04/2026 | 07/04/2026 | [Encrypt at rest with AWS KMS](https://000033.awsstudygroup.com/) |
| 4 | - Thực hành phân quyền linh hoạt và bảo mật hơn thay vì chỉ sử dụng các quyền hạn cố định ([Module 05 - Lab 44](https://drive.google.com/drive/folders/1qQYDh2hPUwYENRNucp0RAx7_hyl3F0Rc?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Giới thiệu về IAM<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo IAM Group <br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo IAM User<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sử dụng Condition để giới hạn truy cập theo IP<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Giới hạn theo thời gian và Tag<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra và xác minh<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 08/04/2026 | 08/04/2026 | [IAM Role & Condition](https://000044.awsstudygroup.com/) |
| 5 | - Thực hành cấp quyền cho ứng dụng truy cập các dịch vụ AWS bằng IAM Role ([Module 05 - Lab 48](https://drive.google.com/drive/folders/1YOydEc2uf3lXutnHmRAUXd7A_KoSkZcp?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: Khởi tạo dịch vụ EC2 Instance và S3 Bucket<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sử dụng Access Key: Tạo IAM Role và Access Key <br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sử dụng IAM Role trên EC2: Tạo IAM Role và gán Role cho EC2<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 09/04/2026 | 09/04/2026 | [Granting authorization for an application to access AWS services with an IAM role](https://000048.awsstudygroup.com/) |
| 6 | - Nghiên cứu và dịch bài blog **Architecting for agentic AI development on AWS**: Tập trung vào các nội dung chính như thiết kế system architecture hỗ trợ feedback nhanh, local emulation, hybrid testing, preview environments, cũng như các nguyên tắc tổ chức code base (domain-driven design, layered testing, monorepo). Qua đó hiểu rõ cách xây dựng kiến trúc phù hợp để AI agents có thể tự động phát triển, kiểm thử và cải tiến code một cách hiệu quả | 10/04/2026 | 10/04/2026 | [Blog 03](https://aws.amazon.com/vi/blogs/architecture/architecting-for-agentic-ai-development-on-aws/) |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 11/04/2026 | 11/04/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 12/04/2026 | 12/04/2026 | |

### Kết quả đạt được tuần 7:

- Về kiến thức lý thuyết:
  - Hiểu rõ cơ chế IAM Permission Boundary:
    - Cách hoạt động như một lớp giới hạn quyền tối đa cho IAM User/Role
    - Phân biệt giữa Permission Boundary và IAM Policy thông thường
    - Khi nào nên sử dụng để kiểm soát quyền trong môi trường lớn
  - Nắm vững nguyên tắc mã hóa dữ liệu at-rest trên AWS:
    - Vai trò của AWS KMS trong quản lý khóa
    - Phân biệt Customer Managed Key và AWS Managed Key
    - Hiểu cách các dịch vụ như EBS, S3 tích hợp với KMS
  - Hiểu cơ chế phân quyền nâng cao với IAM Condition:
    - Giới hạn truy cập theo IP
    - Giới hạn theo thời gian (time-based access)
    - Kiểm soát truy cập dựa trên Tag
  - Hiểu rõ cách cấp quyền cho ứng dụng bằng IAM Role:
    - So sánh IAM Role và Access Key
    - Cơ chế cấp quyền tạm thời (temporary credentials)
    - Lợi ích bảo mật khi sử dụng Role cho EC2
  - Tiếp cận kiến trúc agentic AI development trên AWS:
    - Các nguyên tắc thiết kế hỗ trợ feedback nhanh và tự động hóa
    - Khái niệm local emulation, hybrid testing, preview environments
    - Tổ chức code theo hướng domain-driven design và monorepo
- Về thực hành (Hands-on Labs):
  - **Lab 30:**
    - Tạo và áp dụng Permission Boundary cho IAM User
    - Kiểm tra và xác minh giới hạn quyền truy cập
  - **Lab 33:**
    - Tạo Customer Managed Key sử dụng AWS KMS
    - Mã hóa dữ liệu trên Amazon EBS và Amazon S3
    - Theo dõi hoạt động thông qua AWS CloudTrail
  - **Lab 44:**
    - Tạo IAM User và Group
    - Áp dụng Condition để giới hạn truy cập theo IP, thời gian và Tag
    - Kiểm tra tính hiệu lực của policy
  - **Lab 48:**
    - Tạo IAM Role và gán cho EC2 Instance
    - Kiểm tra truy cập S3 thông qua Role
    - So sánh với phương pháp sử dụng Access Key
    - Thực hiện kiểm tra và dọn dẹp tài nguyên sau mỗi bài lab
- Về kỹ năng:
  - Nâng cao kỹ năng kiểm soát quyền truy cập nâng cao trong AWS IAM
  - Thành thạo việc triển khai mã hóa dữ liệu và quản lý khóa
  - Có khả năng thiết kế quyền linh hoạt dựa trên nhiều điều kiện thực tế
  - Hiểu và áp dụng tốt IAM Role trong thực tế triển khai ứng dụng
  - Cải thiện kỹ năng đọc hiểu tài liệu kiến trúc và phân tích hệ thống
- Về tư duy hệ thống:
  - Hình thành tư duy kiểm soát quyền theo nhiều lớp (defense in depth)
  - Hiểu cách kết hợp IAM, KMS và các cơ chế điều kiện để tăng cường bảo mật
  - Tiếp cận tư duy thiết kế hệ thống hỗ trợ tự động hóa và AI-driven development
  - Nhận thức rõ hơn về cân bằng giữa bảo mật, tính linh hoạt và trải nghiệm phát triển
- Về quản lý tài nguyên:
  - Duy trì thói quen dọn dẹp tài nguyên sau khi thực hành
  - Chủ động kiểm soát tài nguyên sử dụng trong phạm vi hợp lý
  - Nâng cao ý thức tối ưu chi phí khi sử dụng các dịch vụ AWS