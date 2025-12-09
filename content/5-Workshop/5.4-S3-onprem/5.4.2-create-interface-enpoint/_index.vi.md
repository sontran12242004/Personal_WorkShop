---
title : "Initialize Amazon RDS"
date : 2025-09-09
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

1. Truy cập Bảng điều khiển RDS > Nhóm mạng con > Tạo nhóm mạng con DB
   * Tên: db-private-group
   * Mạng con: Chọn 2 Vùng chứa dữ liệu và chọn chính xác 2 Mạng con riêng tư

![name](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint1.png)
![service](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint2.png)


![vpc](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint3.png)

2. Go to Databases > Create database

![subnets](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint4.png)

3. Tùy chọn công cụ: Microsoft SQL Server (Phiên bản Express)

![sg](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint5.png)

4. Mẫu: Miễn phí
5. Cài đặt: Đặt Mật khẩu chính (ghi nhớ để sử dụng sau)

![success](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint-success.png)

6. Kết nối:
* VPC: VPC bạn đã tạo cho Web
* Nhóm mạng con: db-private-group
* Quyền truy cập công khai: Không
* Nhóm bảo mật VPC: Chọn nhóm bảo mật bạn đã tạo cho cơ sở dữ liệu

![sg](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint6.png)

7. Click Create database