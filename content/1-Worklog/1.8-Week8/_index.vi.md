---
title: "Worklog Tuần 8"
date: 2025-09-16
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---


### 🎯 Mục tiêu tuần 8:
* Nắm vững kiến thức về **AWS VPC**, bảo mật mạng, cấu trúc subnet.
* Biết cách thiết kế hệ thống mạng cơ bản trong AWS.
* Thực hành tạo VPC, subnet, security group, route table và kiểm thử kết nối.

---

### 📌 Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------- | ---------------- | --------------- |
| 2 | - Tổng hợp kiến thức EC2 tuần trước <br> - Nhận mục tiêu học tập tuần 8 từ mentor | 18/08/2025 | 18/08/2025 | |
| 3 | - Tìm hiểu khái niệm tổng quan về VPC: <br> &emsp;+ CIDR <br> &emsp;+ Subnet (Public/Private) <br> &emsp;+ Internet Gateway <br> &emsp;+ NAT Gateway | 19/08/2025 | 19/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Nghiên cứu sâu về bảo mật mạng: <br> &emsp;+ Security Group <br> &emsp;+ Network ACL (NACL) <br> &emsp;+ Route Table | 20/08/2025 | 20/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - **Thực hành:** <br> &emsp;+ Tạo VPC mới <br> &emsp;+ Tạo Public/Private Subnet <br> &emsp;+ Tạo và bind Internet Gateway, NAT Gateway <br> &emsp;+ Cập nhật Route Table | 21/08/2025 | 22/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - **Kiểm thử:** <br> &emsp;+ Deploy EC2 vào Public Subnet và SSH từ Internet <br> &emsp;+ Deploy EC2 vào Private Subnet và kiểm thử truy cập Internet thông qua NAT | 22/08/2025 | 22/08/2025 | https://cloudjourney.awsstudygroup.com/ |

---

### ✅ Kết quả đạt được tuần 8:

* Hiểu rõ kiến trúc mạng AWS VPC gồm:
    * Dải IP – CIDR
    * Public và Private Subnet
    * Internet Gateway và NAT Gateway
    * Route Table – cơ chế định tuyến giữa các subnet

* Áp dụng kiến thức bảo mật mạng:
    * Thiết lập Security Group phù hợp (SSH, HTTP, HTTPS)
    * Hiểu cơ chế inbound/outbound rule
    * Biết phân biệt SG và NACL

* Thực hành xây dựng **một môi trường VPC hoàn chỉnh**:
    * 01 VPC
    * 01 Public Subnet + 01 Private Subnet
    * 01 Internet Gateway + 01 NAT Gateway
    * Route Table cấu hình thủ công
    * EC2 hoạt động ổn định trong từng subnet

* Tự kiểm thử thành công:
    * SSH vào EC2 của Public Subnet
    * Private Subnet không SSH trực tiếp từ Internet
    * Private Subnet truy cập Internet thông qua NAT Gateway

* Bắt đầu hiểu tư duy “infrastructure as network topology” — cách cloud tách biệt tài nguyên bằng lớp mạng.

---

### 📌 Nhận xét cá nhân:
Tuần 8 giúp tôi hiểu rõ hơn về hệ thống mạng trong AWS — phần quan trọng nhất khi triển khai ứng dụng backend. Việc tự tay tạo VPC và test kết nối giúp tôi hình dung cách doanh nghiệp thiết kế hệ thống an toàn, hạn chế rủi ro. Đây là bước quan trọng để chuẩn bị cho các nội dung nâng cao ở tuần sau như Load Balancer, Auto Scaling, IAM, S3, và các dịch vụ serverless.

