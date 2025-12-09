---
title : "Thiết lập cơ sở hạ tầng mạng"
date : 2025-09-09
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Overview

Trong phần này, chúng ta sẽ xây dựng nền tảng mạng cho ứng dụng MiniMarket. Kiến trúc mạng an toàn là điều kiện tiên quyết để bảo vệ ứng dụng và dữ liệu.

Chúng ta sẽ thiết kế một VPC bao gồm:

* Mạng con công cộng: Dành riêng cho các thành phần giao tiếp trực tiếp với Internet (Load Balancer, NAT Gateway).

* Mạng con riêng: Dành riêng cho các thành phần yêu cầu bảo mật (App Server, Database, Redis).

Ngoài ra, chúng ta sẽ cấu hình NAT Gateway để cho phép các máy chủ bên trong Mạng con riêng tải xuống các bản cập nhật và Docker Image từ Internet mà không để lộ địa chỉ IP ra bên ngoài.
![overview](/images/5-Workshop/5.3-S3-vpc/diagram2.png)


