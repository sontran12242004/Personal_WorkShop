---
title : "Tạo VPC và Mạng con"
date :  2025-09-09
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. ---
title : "Create VPC & Subnets"
date :  2025-09-09
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Mở bảng điều khiển Amazon VPC (Lưu ý: Chọn vùng phù hợp với nhu cầu, ở đây nhóm sử dụng Vùng ap-southeast-1)
2. Chọn Tạo VPC



![endpoint](/images/5-Workshop/5.3-S3-vpc/endpoints.png)

### Configuration:
+ Tên: HRM-VPC
+ IPv4 CIDR: 10.0.0.0/16

![endpoint](/images/5-Workshop/5.3-S3-vpc/create-s3-gwe1.png)

4. Tạo mạng con (Chia 2 AZ để đảm bảo tính sẵn sàng cao):
+ Mạng con công khai (2): 10.0.1.0/24 & 10.0.2.0/24 (Dùng cho Load Balancer & NAT)
+ Mạng con riêng tư (2): 10.0.3.0/24 & 10.0.4.0/24 (Dùng cho App, DB, Redis)

![endpoint](/images/5-Workshop/5.3-S3-vpc/services.png)

+ Click Tạo VPC và đợi trạng thái chuyển sang Khả dụng là thành công

  ![endpoint](/images/5-Workshop/5.3-S3-vpc/vpc.png)





