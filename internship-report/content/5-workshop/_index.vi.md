---
title : "Workshop"
date :  "`r Sys.Date()`" 
weight : 5
pre: <b> 5. </b>
chapter : false
---

# Production Access Request Portal

### Tổng quan

Dự án được xây dựng nhằm thiết lập một hệ thống quản lý tập trung, cho phép người dùng yêu cầu và được phê duyệt quyền truy cập vào các tài nguyên quan trọng trên nền tảng AWS (Amazon Web Services) một cách an toàn và có kiểm soát. Thay vì cấp quyền cố định, hệ thống này hướng tới việc quản lý truy cập theo nhu cầu (Just-in-Time access), giúp tăng cường bảo mật cho môi trường Production của tổ chức.

Tài liệu này tập trung vào việc hướng dẫn chi tiết quy trình thiết lập thủ công (manual setup) toàn bộ hạ tầng cần thiết trên AWS Console. Các nội dung chính bao gồm:
- Thiết lập IAM Identity Center, tạo và cấu hình permission sets, access groups và group assignments.
- Triển khai các thành phần lưu trữ như DynamoDB, Secrets Manager, SES.
- Xây dựng và phân quyền cho các Lambda functions.
- Cấu hình API Gateway.
- Liên kết các dịch vụ này thành một luồng vận hành hoàn chỉnh cho hệ thống cấp quyền truy cập tạm thời.

Tài liệu cũng trình bày thứ tự triển khai, quy ước đặt tên tài nguyên, các giá trị cần chuẩn bị trước, cùng những lưu ý quan trọng để bảo đảm quá trình thiết lập diễn ra đúng, đủ và nhất quán trên môi trường AWS Console.

Dưới đây là kiến trúc triển khai:

<img src="/images/figure-proposal.png" alt="figure-proposal" style="width:600px !important; max-width:900px !important;">

### Đối tượng sử dụng

Hệ thống được xây dựng để phục vụ nhiều nhóm người dùng khác nhau trong quá trình quản lý và kiểm soát quyền truy cập vào môi trường production trên AWS:

- **Người dùng cuối (Developer/Engineer):** Là những người cần truy cập tạm thời vào môi trường production để thực hiện các công việc như triển khai hệ thống, xử lý sự cố, kiểm tra log hoặc bảo trì dịch vụ.
- **Người phê duyệt (Team Lead/Manager):** Có trách nhiệm xem xét, phê duyệt hoặc từ chối các yêu cầu truy cập nhằm đảm bảo việc cấp quyền đúng mục đích và đúng đối tượng.
- **Đội ngũ Platform/DevOps Engineer:** Phụ trách triển khai, cấu hình, vận hành và giám sát toàn bộ hạ tầng của hệ thống trên AWS, đồng thời đảm bảo các dịch vụ hoạt động ổn định và an toàn.
- **Bộ phận Security/Compliance:** Theo dõi nhật ký truy cập, kiểm tra audit trail và đảm bảo hệ thống tuân thủ các chính sách bảo mật, quản trị và kiểm soát truy cập của tổ chức.

### Nội dung

1. [Giới thiệu](1-introduction/)
2. [Chuẩn bị tài nguyên](2-preparation/)
3. [Demo sản phẩm](3-product-demonstration/)
4. [Dọn dẹp tài nguyên](4-clean-up-resources/)