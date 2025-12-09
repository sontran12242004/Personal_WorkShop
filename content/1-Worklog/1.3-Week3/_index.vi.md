---
title: "Worklog Tuần 3"
date: "2025-11-11"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---



### Mục tiêu tuần 3:

- Tìm hiểu chuyên sâu về **Amazon EC2** và các dịch vụ lưu trữ liên quan (EBS, EFS, FSx).
- Nắm được cơ chế hoạt động, cấu hình và các tùy chọn giá của EC2.
- Thực hành tạo và quản lý EC2 Instance, EBS Volume, Snapshot.
- Hiểu và ứng dụng Auto Scaling, Pricing Option và Lightsail.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tổng quan **Amazon EC2**: khái niệm, khả năng co giãn, so sánh với máy chủ vật lý.<br>- Tìm hiểu các **Instance Type**, thông số kỹ thuật (CPU, Memory, Network, Storage).                         | 25/08/2025   | 25/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Nghiên cứu **AMI (Amazon Machine Image)**, quy trình provision EC2 Instance.<br>- Tìm hiểu **Hypervisor (KVM, HVM, PV)** và cơ chế lựa chọn.                                                       | 26/08/2025   | 26/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu **Key Pair** và cơ chế mã hóa thông tin đăng nhập (Linux/Windows).<br>- Tìm hiểu **EBS (Elastic Block Store)**: loại HDD, SSD, snapshot, replicate dữ liệu.                               | 27/08/2025   | 27/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Nghiên cứu **Instance Store**: đặc điểm, ưu nhược điểm, ứng dụng thực tế.<br>- Thực hành backup EBS bằng snapshot và phục hồi.                                                                     | 28/08/2025   | 28/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Tìm hiểu **User Data** và **Metadata** trong EC2, cách tự động hóa khi khởi tạo Instance.<br>- Học về **EC2 Auto Scaling**, Scaling Policy, Load Balancer integration.                             | 29/08/2025   | 29/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - Tìm hiểu các **Pricing Options**: On-demand, Reserved Instance, Saving Plan, Spot Instance.<br>- Nghiên cứu **Amazon Lightsail**, **EFS**, **FSx** và **AWS Application Migration Service (MGN)**. | 30/08/2025   | 30/08/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 3:

- Hiểu rõ **kiến trúc và hoạt động của Amazon EC2**:

    - Giống máy ảo truyền thống nhưng có khả năng mở rộng linh hoạt.
    - Có thể khởi tạo nhanh, hỗ trợ nhiều loại workload như web, ứng dụng, database...

- Nắm được cơ chế **EC2 Instance Type**, **AMI**, **Hypervisor**, **Key Pair**.
- Biết cách **tạo, kết nối, và backup EC2 Instance** thông qua snapshot EBS.
- Hiểu rõ sự khác biệt giữa **EBS**, **Instance Store**, **EFS**, và **FSx**:

    - EBS: block storage gắn trực tiếp vào EC2, hoạt động độc lập qua mạng riêng.
    - Instance Store: tốc độ cực cao, không lưu dữ liệu khi stop instance.
    - EFS: lưu trữ dùng chung nhiều EC2 (Linux).
    - FSx: tương tự EFS nhưng hỗ trợ NTFS và SMB (Windows/Linux).

- Nắm được **EC2 Auto Scaling**:

    - Tự động tăng giảm số lượng Instance.
    - Hỗ trợ nhiều AZ và tích hợp Load Balancer.
    - Có thể kết hợp nhiều Pricing Option.

- Hiểu rõ 4 **Pricing Option chính**:

    - On-demand: linh hoạt, giá cao.
    - Reserved Instance & Saving Plan: tiết kiệm khi cam kết dài hạn.
    - Spot Instance: giá rẻ, nhưng có thể bị dừng đột ngột.

- Biết sử dụng **Amazon Lightsail** cho các workload nhẹ, test/dev.
- Nắm được cơ chế **AWS Application Migration Service (MGN)** để sao chép máy chủ thật/ảo sang EC2.
- Hiểu rõ quá trình replicate dữ liệu, snapshot incremental, và tối ưu hóa chi phí bằng deduplication.
- Hoàn thành việc **tổng hợp tài liệu và ghi chú chi tiết** về toàn bộ chuỗi dịch vụ liên quan đến EC2.

---
