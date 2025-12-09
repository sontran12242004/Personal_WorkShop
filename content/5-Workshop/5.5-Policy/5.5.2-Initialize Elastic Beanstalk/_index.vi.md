---
title : "Initialize Elastic Beanstalk"
date :  2025-09-09
weight : 5
chapter : false
pre : " <b> 5.5.2 </b> "
---

1. Chúng ta sẽ tạo một môi trường để chạy ứng dụng

* Truy cập Elastic Beanstalk > Tạo ứng dụng

![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy5.png))

* Tên ứng dụng: MiniMarket-App
* Nền tảng: Docker (Amazon Linux 2023)
* Mã ứng dụng: Chọn ứng dụng mẫu (Để kiểm tra cơ sở hạ tầng trước)
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy6.png))
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy7.png))

2. Cấu hình mạng (Mạng) - Cực kỳ quan trọng:
* VPC: Chọn VPC bạn đã tạo cho MiniMarket
* Cài đặt phiên bản:
* Địa chỉ IP công khai: Bỏ chọn
* Mạng con: Chọn 2 Mạng con riêng tư
* Nhóm bảo mật EC2: Chọn sg-web-app
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy8.png))
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy9.png))

* Dung lượng:
* Loại môi trường: Chọn Cân bằng tải
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy10.png))
* Cài đặt mạng cân bằng tải:
* Hiển thị: Công khai
* Mạng con: Chọn 2 Mạng con Công khai
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy11.png))
3. Nhấp vào Tạo môi trường và đợi môi trường sẵn sàng