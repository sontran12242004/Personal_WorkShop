---
title: "Triển khai ứng dụng"
date: 2025-09-09
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---


### Overview
* Sau khi đã hạ tầng mạng và cơ sở dữ liệu, bước tiếp theo là đưa ra mã nguồn ứng dụng Java Spring Boot (Hệ thống quản lý nhân sự) lên môi trường Cloud.
* Thay vì phải quản lý công việc trên máy chủ EC2, chúng tôi sử dụng nền tảng PaaS – AWS Elastic Beanstalk để tự động hóa quá trình phát triển khai báo và vận hành.
* Mục tiêu của mô-đun này:
1. Hóa container
* Đóng gói ứng dụng HR Management System (Java Spring Boot) vào Docker Container để đảm bảo môi trường chạy thống nhất giữa Dev/Test/Prod (Dev = Prod), hạn chế cấu hình lỗi và giúp phát triển khai báo một cách dễ dàng.
2. Triển khai
* Triển khai Docker Container lên AWS Elastic Beanstalk. Elastic Beanstalk sẽ tự động:
* Cung cấp các phiên bản EC2 cấp và cấu hình
* Cài đặt Cân bằng tải ứng dụng
* Tạo Auto Scaling Group để tự động mở rộng khi tải
* Quản lý vòng đời ứng dụng mà không cần thao tác thủ công
![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy.png)

