---
title : "Introduction"
date :  2025-09-09
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Introduction
+ HumanResource là Hệ thống Quản lý Nguồn nhân lực được xây dựng trên nền tảng .NET Core, áp dụng Kiến trúc 3 tầng hiện đại.

+ Mục tiêu của Hội thảo này là chuyển đổi nền tảng ứng dụng từ môi trường On-Premise sang cơ sở hạ tầng AWS Cloud (Di chuyển lên nền tảng đám mây gốc) đồng thời đáp ứng các nguyên tắc chính của Khung Kiến trúc Tốt AWS:
  Bảo mật, Độ tin cậy, Hiệu suất Hiệu quả và Tối ưu hóa Chi phí.
#### Workshop overview
Kiến trúc Giải pháp:

+ Tính toán: Sử dụng AWS Elastic Beanstalk (nền tảng Docker) để triển khai ứng dụng HRM, đơn giản hóa việc quản lý cơ sở hạ tầng và hỗ trợ Tự động Mở rộng khi dữ liệu nhân viên hoặc tải hệ thống tăng lên.

+ Cơ sở dữ liệu: Sử dụng Amazon RDS cho SQL Server trong Mạng con Riêng để lưu trữ an toàn thông tin nhân sự như hồ sơ nhân viên, hợp đồng, hồ sơ chấm công và dữ liệu bảng lương.

+ Bộ nhớ đệm: Amazon ElastiCache (Redis) được sử dụng để lưu trữ phiên người dùng và bộ nhớ đệm dữ liệu nhân sự thường xuyên truy cập (danh sách nhân viên, cơ cấu phòng ban), cải thiện thời gian phản hồi của hệ thống.

Mạng & Bảo mật:

+ VPC: Được thiết kế với mô hình Mạng con Công khai/Riêng tư kết hợp với Cổng NAT, đảm bảo môi trường HRM an toàn và biệt lập.

+ Bảo mật Lớp Ứng dụng: Triển khai AWS WAF kết hợp với Amazon CloudFront để bảo vệ chống lại các cuộc tấn công web và tăng tốc độ phân phối nội dung cho nhân viên trên các khu vực khác nhau.

Lưu trữ:

+ Amazon S3 được sử dụng để lưu trữ các tài liệu nhân sự như hợp đồng lao động, hồ sơ nhân viên và tài liệu đào tạo với độ bền cao.

DevOps:

+ Một quy trình CI/CD hoàn toàn tự động sử dụng AWS CodePipeline và CodeBuild, cho phép triển khai nhanh chóng các tính năng quản lý nhân sự (HRM) như xử lý bảng lương, quản lý chấm công và quy trình phê duyệt nghỉ phép.

Giám sát:

+ Amazon CloudWatch theo dõi tình trạng hệ thống (CPU, Mạng) và gửi cảnh báo để đảm bảo hệ thống HRM luôn ổn định và có tính khả dụng cao.
![overview](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)
