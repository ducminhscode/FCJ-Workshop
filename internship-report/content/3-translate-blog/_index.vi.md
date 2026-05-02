---
title : "Các blog đã dịch"
date :  "`r Sys.Date()`" 
weight : 3 
pre: <b> 3. </b>
chapter : false
---

**Blog 1:** [Blog 1 - Xây dựng kiến trúc bảo mật defense-in-depth được hỗ trợ bởi AI cho serverless microservices](./blog-1)

Bài viết này trình bày cách xây dựng một kiến trúc bảo mật nhiều lớp (defense-in-depth) cho ứng dụng serverless microservices trên AWS, kết hợp AI/ML để chống lại các mối đe dọa hiện đại. Mở rộng diện tấn công với nhiều API và hàm chức năng, kiến trúc này đặt 7 lớp bảo vệ từ ngoài vào trong - từ chặn traffic độc hại, xác thực người dùng, kiểm soát API, cô lập mạng, bảo vệ môi trường compute, quản lý credentials đến bảo vệ dữ liệu và giám sát liên tục bằng AI để phát hiện và phản ứng sớm với tấn công.

**Blog 2:** [Blog 2 - Xây dựng ứng dụng multi-tenant SaaS với chế độ tenant isolation mới của AWS Lambda](./blog-2)

Bài blog giới thiệu **chế độ tenant isolation** mới của AWS dành cho **AWS Lambda**, giúp xây dựng ứng dụng multi-tenant SaaS an toàn hơn. Tính năng này đảm bảo mỗi tenant được chạy trong execution environment riêng, không chia sẻ với tenant khác (tránh rò rỉ dữ liệu), nhưng vẫn tái sử dụng môi trường cho cùng tenant để giảm cold start.

**Blog 3:** [Blog 3 - Thiết kế kiến trúc cho phát triển agentic AI trên AWS](./blog-3)

Bài viết trình bày cách thiết kế kiến trúc hệ thống và code base nhằm hỗ trợ **agentic AI development** trên AWS. Nội dung tập trung vào việc giảm friction thông qua các cơ chế phản hồi nhanh như local emulation, hybrid testing và preview environments. Đồng thời, bài viết đề xuất các nguyên tắc tổ chức code như domain-driven design, layered testing, monorepo và tài liệu machine-readable để giúp AI agents hiểu, chỉnh sửa và kiểm thử hệ thống hiệu quả. Mục tiêu là xây dựng môi trường nơi AI agents có thể tự động lặp, validate và cải tiến code một cách an toàn và nhanh chóng.

**Blog 4:** [Blog 4 - Xử lý dữ liệu log nhạy cảm bằng Amazon CloudWatch](./blog-4)

Bài viết tập trung vào cách bảo vệ dữ liệu nhạy cảm (đặc biệt là PII) trong log ứng dụng khi sử dụng Amazon CloudWatch, đồng thời vẫn đảm bảo khả năng debug và vận hành hiệu quả. Nội dung chính xoay quanh việc sử dụng **data protection policies** để tự động phát hiện và che (mask) các thông tin nhạy cảm như email, số thẻ tín dụng, cũng như áp dụng kiểm soát truy cập chi tiết với IAM để giới hạn ai có thể xem dữ liệu gốc (unmask). Bên cạnh đó, bài viết cũng đề cập đến việc kết hợp Audit và Deidentify để giám sát và bảo vệ dữ liệu, xây dựng quy trình cấp quyền tạm thời (privilege escalation) khi cần truy cập log đầy đủ, và sử dụng CloudTrail để audit toàn bộ hoạt động truy cập. Giải pháp này giúp cân bằng giữa bảo mật, tuân thủ và hiệu quả xử lý sự cố (MTTR) trong các hệ thống hiện đại.