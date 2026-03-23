---
title : "Worklog tuần 2"
date :  "`r Sys.Date()`" 
weight : 2 
pre: <b> 1.2 </b>
chapter : false
---

### Mục tiêu tuần 2:

- Nắm vững kiến thức nền tảng về Networking trên AWS, hiểu sâu về cấu trúc, cách thức hoạt động và các thành phần cốt lõi của Amazon VPC.
- Làm chủ các giải pháp kết nối Hybrid và Multi-VPC, thực hành được các phương thức kết nối an toàn giữa các mạng nội bộ (On-premises) và AWS như VPN, Direct Connect, cũng như kết nối giữa các VPC thông qua Peering và Transit Gateway.
- Tối ưu hóa khả năng phân giải DNS và cân bằng tải, sử dụng Route 53 Resolver cho mô hình Hybrid và nắm vững các loại Elastic Load Balancer (ALB, NLB, GWLB) để đảm bảo tính sẵn sàng cao cho hệ thống.
- Nghiên cứu kiến trúc bảo mật nhiều lớp (defense-in-depth) cho các dịch vụ hiện đại như serverless và microservices qua các tình huống thực tế từ Blog AWS.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Tìm hiểu về các dịch vụ mạng, thiết lập và quản lý môi trường mạng ảo riêng tư trên nền tảng AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ VPC (Virtual Private Cloud)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các thành phần cốt lõi trong VPC: Subnet (Mạng con), Route Table (Bảng định tuyến), ENI (Elastic Network Interface)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kết nối Internet: Internet Gateway (IGW), NAT Gateway<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các dịch vụ mạng nâng cao khác: VPC Peering & Transit Gateway, VPN & Direct Connect, VPC Endpoint<br>- Hiểu rõ về bảo mật trong VPC và các tính năng kết nối nhiều VPC:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Security Group (SG)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Network Access Control List (NACL)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ VPC Flow Logs<br>&nbsp;&nbsp;&nbsp;&nbsp;+ VPC Peering<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Transit Gateway (TGW)<br>- Tập trung vào các giải pháp kết nối Hybrid (On-premises với Cloud) và dịch vụ Cân bằng tải (Elastic Load Balancer):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ VPN Site-to-Site: Virtual Private Gateway, Customer Gateway<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Client VPN<br>&nbsp;&nbsp;&nbsp;&nbsp;+ AWS Direct Connect<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Application Load Balancer (ALB)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Network Load Balancer (NLB)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Classic Load Balancer (CLB)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Gateway Load Balancer (GWLB)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Health Check<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sticky Session | 02/03/2026 | 02/03/2026 | [AWS Virtual Private Cloud](https://youtu.be/O9Ac_vGHquM?si=_aG8YUGohIVlaDPZ)<br>[VPC Security and Multi-VPC features](https://youtu.be/BPuD1l2hEQ4?si=po48SGsVTvdnuACl)<br>[VPN - DirectConnect - LoadBalancer - ExtraResources](https://youtu.be/CXU8D3kyxIc?si=P7v3T0wo4oYdROvi) |
| 3 | - Thực hành tạo Amazon VPC và AWS Site-to-Site VPN đầu tiên của mình ([Module 02 - Lab 03](https://drive.google.com/drive/folders/1bAvUbXWHi7QfQw2QimYraeJYIwYDyM8s?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường VPC<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai tài nguyên EC2<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập AWS Site-to-Site VPN<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Sử dụng Transit Gateway để quản lý kết nối VPN tập trung<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 03/03/2026 | 03/03/2026 | [Amazon VPC and AWS Site-to-Site VPN Workshop](https://000003.awsstudygroup.com/) |
| 4 | - Nghiên cứu và dịch bài blog **Building an AI-powered defense-in-depth security architecture for serverless microservices**<br>- Phân tích kiến trúc bảo mật nhiều lớp cho serverless trên AWS<br>- Kiểm tra thuật ngữ AWS và chỉnh sửa bản dịch để đảm bảo tính chính xác và tự nhiên | 04/03/2026 | 04/03/2026 | [Blog 01](https://aws.amazon.com/vi/blogs/security/building-an-ai-powered-defense-in-depth-security-architecture-for-serverless-microservices/) |
| 5 | - Thực hành xây dựng hệ thống DNS Hybrid với dịch vụ Amazon Route 53 trên AWS ([Module 02 - Lab 10](https://drive.google.com/drive/folders/1s_nuYrVH6RYskrV3jYz513TpLr-Ko4ct?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: Tạo Keypair, khởi tạo hạ tầng cơ sở bằng CloudFormation, cấu hình Security Groups<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập hạ tầng On-premises giả lập: Kết nối vào RDGW, triển khai Microsoft Active Directory (AD)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình Route 53 Resolver: Tạo Outbound Endpoint, tạo Resolver Rules, tạo Inbound Endpoint<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra kết quả<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 05/03/2026 | 05/03/2026 | [Set up Hybrid DNS with Route 53 Resolver](https://000010.awsstudygroup.com/) |
| 6 | - Thực hành thiết lập VPC Peering cho phép các VPC giao tiếp với nhau bằng địa chỉ Private IP ([Module 02 - Lab 19](https://drive.google.com/drive/folders/1xvC3ypW4yMa0oaOL_IKSL3bs6pA3-XwR?usp=drive_link)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: Sử dụng CloudFormation để tạo nhanh VPC và subnet, tạo Security Group, tạo EC2 Instance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cập nhật Network ACL: Cấu hình Inbound rules và Outbound rules<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập VPC Peering Connection: Gửi yêu cầu và chấp nhận yêu cầu từ VPC này sang VPC kia<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình Route Tables<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cấu hình Cross-Peer DNS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Kiểm tra kết quả: Thực hiện lệnh ping hai VPC bằng Private IP để xác nhận kết nối thành công<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 06/03/2026 | 06/03/2026 | [Setting up VPC Peering](https://000019.awsstudygroup.com/) |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 07/03/2026 | 07/03/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 08/03/2026 | 08/03/2026 | |

### Kết quả đạt được tuần 2:

- Về kiến thức lý thuyết:
  - Phân biệt rõ ràng chức năng và trường hợp sử dụng của các loại Load Balancer (ALB/NLB/GWLB) và các thành phần bảo mật mạng (Security Group vs NACL).
  - Hiểu rõ cơ chế hoạt động của Route 53 Inbound/Outbound Endpoints trong môi trường lai.
  - Nắm được quy trình triển khai VPN Site-to-Site và vai trò của Transit Gateway trong việc quản lý mạng tập trung.
- Về thực hành (Hands-on Labs):
  - **Lab 03**: Triển khai thành công môi trường VPC, cấu hình VPN Site-to-Site và tích hợp Transit Gateway để quản lý kết nối.
  - **Lab 10**: Xây dựng hệ thống DNS Hybrid hoàn chỉnh, cho phép phân giải tên miền thông suốt giữa Microsoft Active Directory (giả lập On-premises) và AWS.
  - **Lab 19**: Thiết lập thành công VPC Peering, cấu hình bảng định tuyến và Cross-Peer DNS giúp các VPC giao tiếp bằng Private IP.
- Về nghiên cứu và phân tích:
  - Hoàn thành bản dịch và phân tích bài blog về kiến trúc bảo mật cho serverless microservices.
  - Hệ thống hóa được các thuật ngữ chuyên ngành AWS Networking và Security, đảm bảo tính chính xác trong tài liệu kỹ thuật.
- Về quản lý tài nguyên:
  - Thực hiện tốt việc dọn dẹp tài nguyên sau mỗi bài lab để tối ưu hóa chi phí sử dụng AWS.
