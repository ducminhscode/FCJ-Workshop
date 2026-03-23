---
title : "Event 1"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 4.1 </b> "
---

# Bài Thu Hoạch "AWS Perimeter Protection Workshop – Tăng cường bảo mật với CloudFront & WAF"

## Mục Đích Của Sự Kiện

Workshop **AWS Perimeter Protection** được tổ chức nhằm giúp các kỹ sư và nhà phát triển hiểu rõ hơn về cách xây dựng kiến trúc bảo mật cho ứng dụng web trên nền tảng AWS.
Sự kiện tập trung vào việc giới thiệu các giải pháp bảo vệ hệ thống trước các mối đe dọa ngày càng gia tăng như DDoS, bot traffic và các lỗ hổng ứng dụng web.

Thông qua các phần trình bày và hands-on workshop, người tham dự có cơ hội tiếp cận với các dịch vụ bảo mật quan trọng của AWS như [Amazon CloudFront](https://aws.amazon.com/vi/cloudfront/), [AWS WAF](https://aws.amazon.com/vi/waf/) và [AWS Shield](https://aws.amazon.com/vi/shield/), từ đó nắm được cách triển khai kiến trúc bảo mật nhiều lớp (defense-in-depth) để bảo vệ ứng dụng trên internet.

## Danh Sách Diễn Giả

- **Nguyễn Gia Hưng:** Trưởng bộ phận kiến trúc sư giải pháp - Việt Nam và Campuchia
- **Julian Ju:** Kiến trúc sư giải pháp chuyên gia cấp cao về dịch vụ Edge
- **Kevin Lim:** Chuyên gia cấp cao về dịch vụ Edge - GTM

## Nội Dung Nổi Bật

Sự kiện bao gồm nhiều phiên chia sẻ và thực hành chuyên sâu:

- **Kiến trúc CDN với CloudFront:** Tập trung vào cách xây dựng hệ thống phân phối nội dung toàn cầu với CloudFront, tối ưu hiệu suất và giảm tải cho hệ thống origin.
- **Bảo vệ ứng dụng với AWS WAF:** Giới thiệu cách sử dụng WAF để ngăn chặn các cuộc tấn công phổ biến như OWASP Top 10, bot attack và các request độc hại.
- **Hands-on Workshop:** Người tham dự được trực tiếp thực hành cấu hình CloudFront, tối ưu hiệu năng web application và triển khai các cơ chế bảo mật với WAF.
- **Phân tích các mối đe dọa hiện nay:** Workshop cũng nhấn mạnh sự gia tăng của các cuộc tấn công DDoS và bot traffic, đặc biệt là các AI bot đang gia tăng nhanh chóng.

## Những Gì Học Được

- Hiểu rõ cách **CloudFront hoạt động từ Edge đến Origin** và cách CDN giúp giảm tải hệ thống backend.
- Nắm được cách sử dụng **WAF rules** để phát hiện và chặn các request độc hại.
- Biết cách thiết kế **thứ tự rule trong WAF** để tối ưu hiệu suất và độ chính xác.
- Tìm hiểu các kỹ thuật bảo vệ ứng dụng trước **DDoS** và **bot attacks**.
- Hiểu thêm về các tính năng mới như **VPC Origin** và **Mutual TLS** trong CloudFront.

## Ứng Dụng Vào Công Việc

- Triển khai **CloudFront** để tăng tốc độ truy cập website và giảm tải cho server.
- Sử dụng **AWS WAF** để bảo vệ API và web application trước các cuộc tấn công phổ biến.
- Thiết lập **rate limiting** và **bot detection** để kiểm soát traffic bất thường.
- Áp dụng mô hình **defense-in-depth** nhằm tăng cường bảo mật cho hệ thống.
- Kết hợp monitoring tools như CloudWatch để theo dõi hiệu suất và phát hiện sự cố.

## Trải Nghiệm Trong Event

Workshop được tổ chức trong không khí rất chuyên nghiệp và cởi mở. Các diễn giả chia sẻ nhiều kinh nghiệm thực tế từ các hệ thống production, giúp người tham dự hiểu rõ hơn về cách triển khai và vận hành dịch vụ trên AWS.

Phần **hands-on lab** là điểm thú vị nhất khi mọi người có thể trực tiếp thực hành các cấu hình CloudFront và WAF, đồng thời trao đổi với các chuyên gia để giải đáp các vấn đề thực tế.

Ngoài ra, sự kiện cũng là cơ hội tốt để **networking** với các kỹ sư cloud và những người cùng quan tâm đến AWS, giúp mở rộng kiến thức cũng như cộng đồng công nghệ.

## Một số hình ảnh buổi workshop

<img src="/images/event-1.jpg" alt="Event 1" style="width:900px !important; max-width:900px !important;">
