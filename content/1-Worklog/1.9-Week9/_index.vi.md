---
title: "Worklog Tuần 8"
date: 2025-09-16
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Đây là bản tham khảo. Vui lòng **không sao chép nguyên văn** cho bài báo cáo thực tập của bạn, kể cả phần cảnh báo này.
{{% /notice %}}

### 🎯 Mục tiêu tuần 8:

* Hiểu sâu hơn về kiến trúc mạng trong AWS (VPC, Subnet, Routing).
* Làm quen với cách thiết kế hạ tầng chuẩn cho dự án backend Spring Boot.
* Thực hành xây dựng nền tảng hạ tầng phục vụ cho hệ thống quản lý nhân sự (HRM Project).

---

### 📌 Các công việc triển khai trong tuần:

| Thứ | Công việc | Bắt đầu | Hoàn thành | Nguồn tài liệu |
| --- | --------- | ------- | ----------- | --------------- |
| 2 | - Họp với mentor / trainer để review kiến thức đã học tuần 7. <br> - Nhận roadmap tuần 8 (VPC – Network – Security). | 25/08/2025 | 25/08/2025 | |
| 3 | - Tìm hiểu AWS VPC: <br> &emsp;+ CIDR, IP Addressing <br> &emsp;+ Public / Private Subnet <br> &emsp;+ Route Table <br> &emsp;+ Internet Gateway | 26/08/2025 | 26/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Nghiên cứu cơ chế bảo mật mạng AWS: <br> &emsp;+ Security Group <br> &emsp;+ Network ACL <br> &emsp;+ Differences SG vs NACL | 27/08/2025 | 27/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - **Thực hành:** <br> &emsp;+ Tạo VPC mới cho dự án HRM <br> &emsp;+ Tạo Public Subnet + Private Subnet <br> &emsp;+ Gán Internet Gateway vào Public Subnet | 28/08/2025 | 29/08/2025 | AWS Console / AWS Docs |
| 6 | - **Thực hành nâng cao:** <br> &emsp;+ Tạo NAT Gateway cho Private Subnet <br> &emsp;+ Tạo EC2 test trong từng subnet <br> &emsp;+ Kiểm tra SSH, outbound Internet và routing | 29/08/2025 | 29/08/2025 | AWS Console / CLI |

---

### ✅ Kết quả đạt được tuần 8:

* Hiểu rõ cấu trúc mạng của AWS VPC và vai trò từng thành phần:
    * CIDR → quản lý dải IP.
    * Public subnet → dùng cho tài nguyên cần internet (jump server).
    * Private subnet → triển khai backend, database.
    * Route Table → điều hướng traffic.
    * Internet Gateway & NAT Gateway → quản lý traffic ra/vào.

* Phân biệt và sử dụng được:
    * **Security Group** (stateless inbound/outbound rules).
    * **Network ACL** (stateless firewall layer deeper than SG).

* Tự xây dựng thành công một môi trường VPC chuẩn cho dự án:
    * 01 VPC riêng.
    * 02 subnet (Public / Private).
    * 01 Internet Gateway + 01 NAT Gateway.
    * 02 EC2 instance test → hoạt động đúng chức năng từng subnet.

* Kiểm thử thành công:
    * EC2 Public Subnet → SSH trực tiếp.
    * EC2 Private Subnet → không SSH từ internet (đúng thiết kế).
    * Private Subnet có thể truy cập Internet thông qua NAT.

* Bắt đầu hiểu cách AWS networking phục vụ triển khai ứng dụng Spring Boot HRM sau này:
    * Backend chạy Private Subnet → bảo mật cao hơn.
    * Chỉ Load Balancer public được người dùng truy cập.
    * Database để private hoàn toàn.

---

### 📌 Nhận xét cá nhân:
Tuần 8 giúp tôi hiểu sâu hơn về lớp mạng – phần quan trọng nhất khi xây dựng ứng dụng doanh nghiệp trên nền tảng AWS. Việc tự tay cấu hình VPC và test kết nối giúp tôi hiểu tư duy “secure-by-design” và cách các đội kỹ thuật AWS triển khai hệ thống thực tế. Đây là nền tảng để bước sang tuần tiếp theo
