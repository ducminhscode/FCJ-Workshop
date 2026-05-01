---
title : "Worklog tuần 8"
date :  "`r Sys.Date()`" 
weight : 8
pre: <b> 1.8 </b>
chapter : false
---

### Mục tiêu tuần 8:


### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
|:---:|-----------|:------------:|:---------------:|----------------|
| 2 | - Tổng quan về các khái niệm cơ bản của cơ sở dữ liệu trước khi đi sâu vào các dịch vụ cụ thể của AWS:<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Khái niệm cơ bản về cơ sở dữ liệu<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các thành phần chính trong Database quan hệ: Primary Key, Foreign Key, chuẩn hóa, Index, Partition<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Cơ chế vận hành và tối ưu: Execution Plan, Database Log, Buffer<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Phân loại cơ sở dữ liệu: RDBMS, NoSQL, OLTP vs OLAP<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Các dịch vụ AWS sẽ học trong tuần: Amazon RDS, Amazon Aurora, Amazon ElastiCache, Amazon Redshift<br>- Giới thiệu về hai dịch vụ cơ sở dữ liệu quan hệ quan trọng trên AWS là Amazon RDS và Amazon Aurora<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon RDS: Khái niệm, lợi ích, các tính năng nổi bật<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon Aurora: Khái niệm, kiến trúc đặc biệt<br>&nbsp;&nbsp;&nbsp;&nbsp;+ So sánh và ứng dụng<br>- Giới thiệu về hai dịch vụ dữ liệu quan trọng của AWS là Amazon Redshift và Amazon ElastiCache<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon Redshift: Kiến trúc MPP, lưu trữ dạng cột, tối ưu chi phí<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Amazon ElastiCache: Hai Engine hỗ trợ (Redis, Memcached), lợi ích<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Lộ trình thực hành và tài liệu bổ sung | 13/04/2026 | 13/04/2026 | [Database Concepts review](https://youtu.be/OOD2RwWuLRw?si=4sj2X9mO-rr-6Tog)<br>[Amazon RDS & Amazon Aurora](https://youtu.be/qbrobQZrokY?si=jrfgL-muOLWaAoDX)<br>[Redshift - Elasticache](https://youtu.be/UvdiRW34aNI?si=fsusJsJ6ziH5ufaS) |
| 3 | - Thực hành di chuyển cơ sở dữ liệu từ các hệ quản trị nguồn sang các đích đến trên AWS ([Module 06 - Lab 43](https://drive.google.com/drive/folders/1zMHCjPz3Z_mf0vSZyLpobyq0r3kk5hAS?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Thiết lập nguồn (Source) và đích (Target) cho DMS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai sao chép không máy chủ (Serverless replication)<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Theo dõi và khắc phục sự cố trong quá trình di cư<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 14/04/2026 | 14/04/2026 | [Database Schema Conversion & Migration](https://000043.awsstudygroup.com/) |
| 4 | - Thực hành nắm vững cách triển khai một hệ thống cơ sở dữ liệu hoàn chỉnh ([Module 06 - Lab 05](https://drive.google.com/drive/folders/1-4v8yKN1CbKdEZ_K0_nngmkcv6G6i__G?usp=sharing)):<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Chuẩn bị môi trường: Khởi tạo VPC, thiết lập các Subnets và Route Tables, tạo Security Group<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo EC2 Instance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Tạo RDS Database Instance<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Triển khai ứng dụng: Kết nối ứng dụng trên EC2 tới RDS<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Quản lý sao lưu và khôi phục<br>&nbsp;&nbsp;&nbsp;&nbsp;+ Dọn dẹp tài nguyên | 15/04/2026 | 15/04/2026 | [Amazon Relational Database Service (Amazon RDS)](https://000005.awsstudygroup.com/) |
| 5 |  | 16/04/2026 | 16/04/2026 |  |
| 6 |  | 17/04/2026 | 17/04/2026 |  |
| 7 | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 18/04/2026 | 18/04/2026 | |
| CN | - Nghỉ ngơi và chuẩn bị cho tuần tiếp theo | 19/04/2026 | 19/04/2026 | |

### Kết quả đạt được tuần 8:
