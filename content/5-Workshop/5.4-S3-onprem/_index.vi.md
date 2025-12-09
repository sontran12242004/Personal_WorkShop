---
title : "Data Layer Deployment"
date : 2025-09-09
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

+ Dữ liệu là tài sản quan trọng nhất của mọi hệ thống. Vì vậy, chúng tôi sẽ thiết lập lớp dữ liệu (Data Layer) cho MiniMarket với tiêu chí: Bảo mật tối đa và Hiệu suất cao.

+ Chúng tôi sẽ triển khai hai dịch vụ cốt lõi:

* Amazon RDS (Relational Database Service): Sử dụng SQL Server để lưu trữ dữ liệu kinh doanh (Sản phẩm, Đơn hàng, Người dùng). Cơ sở dữ liệu sẽ được đặt trong Mạng con Riêng tư (Private Subnet) để ngăn chặn truy cập trực tiếp từ Internet.
* Amazon ElastiCache (Redis): Sử dụng Redis làm bộ nhớ đệm (In-memory Cache) để lưu trữ Phiên đăng nhập và giảm tải các truy vấn cho Cơ sở dữ liệu chính.
![Interface endpoint architecture](/images/5-Workshop/5.4-S3-onprem/diagram3.png)



