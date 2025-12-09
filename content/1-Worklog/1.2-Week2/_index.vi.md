---
title: "Worklog Tuần 2"
date: "2025-11-11"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---



### Mục tiêu tuần 2:

- Hiểu rõ về kiến trúc **Amazon VPC** và cách hoạt động của các thành phần mạng trong AWS.
- Nắm vững mô hình **Subnet**, **Route Table**, **Security Group**, **NACL**, **VPC Endpoint**, **Peering**, và **Load Balancer**.
- Biết cách lựa chọn giải pháp kết nối phù hợp giữa môi trường **on-premises** và **AWS Cloud**.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu khái niệm & cấu trúc **Amazon VPC**, giới hạn tài khoản, CIDR IPv4/IPv6. <br> - Cấu hình VPC cơ bản qua Console.          | 18/08/2025   | 18/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Học về **Subnet**, **Availability Zone**, địa chỉ IP. <br> - Tạo subnet public/private và gán route table tương ứng.               | 19/08/2025   | 19/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu **Route Table**, **Internet Gateway**, **NAT Gateway**, **VPC Endpoint**. <br> - Phân biệt interface & gateway endpoint.  | 20/08/2025   | 20/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Sự kiện:** Tham dự **Vietnam Cloud Day 2025 – Track 1: GenAI and Data**. <br> - Tổng quan các ứng dụng AI trong hạ tầng đám mây. | 21/08/2025   | 21/08/2025      |                                           |
| 6   | - Nghiên cứu **Security Group**, **Network ACL**, **VPC Flow Logs**. <br> - Thực hành ghi log & kiểm soát truy cập giữa subnet.      | 22/08/2025   | 22/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - Tìm hiểu **VPC Peering**, **Transit Gateway**, **VPN**, **Direct Connect**, **Elastic Load Balancer** (ALB, NLB, CLB, GLB).        | 23/08/2025   | 23/08/2025      | <https://cloudjourney.awsstudygroup.com/> |

--- | 24/08/2025 | 24/08/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được tuần 2:

- Hiểu rõ **VPC (Virtual Private Cloud)** là môi trường mạng ảo trong AWS giúp phân tách và quản lý tài nguyên theo từng môi trường (Production/Dev/Test/Staging).

    - Giới hạn mặc định: 5 VPC trên mỗi tài khoản AWS.
    - Mỗi VPC yêu cầu một IPv4 CIDR block (bắt buộc) và có thể thêm IPv6 (tùy chọn).

- **Subnet**:

    - Mỗi subnet nằm trong một **Availability Zone** cụ thể.
    - Khi tạo subnet, phải chỉ định CIDR nằm trong phạm vi CIDR của VPC.
    - AWS giữ lại 5 địa chỉ IP đầu tiên trong mỗi subnet để sử dụng nội bộ.

- **Route Table**:

    - Xác định đường đi trong mạng.
    - Mặc định có một _default route table_ (không thể xóa) cho phép các subnet trong VPC liên lạc nội bộ.

- **Elastic Network Interface (ENI)** & **Elastic IP**:

    - ENI là card mạng ảo có thể gán cho EC2 và di chuyển giữa các instance.
    - Elastic IP là địa chỉ IPv4 tĩnh public, có thể gắn với ENI.

- **VPC Endpoint**:

    - Kết nối các tài nguyên trong VPC đến dịch vụ AWS mà không cần Internet.
    - Hai loại:
        - Interface Endpoint – dùng ENI + IP private.
        - Gateway Endpoint – dùng route table (hỗ trợ S3 và DynamoDB).

- **Internet Gateway** & **NAT Gateway**:

    - Internet Gateway: cho phép EC2 trong subnet public truy cập Internet, do AWS quản lý, không cần cấu hình autoscale.
    - NAT Gateway: cho phép subnet private truy cập Internet (chỉ outbound).

- **Security Group (SG)** và **Network ACL (NACL)**:

    - SG là tường lửa _stateful_ áp dụng cho ENI, chỉ có rule “allow”.
    - NACL là tường lửa _stateless_ áp dụng cho subnet, có rule “allow/deny” và thứ tự đọc từ trên xuống.
    - Mặc định: SG chặn inbound, cho phép outbound; NACL cho phép tất cả.

- **VPC Flow Logs**:

    - Ghi nhận lưu lượng IP đến/đi từ VPC, subnet hoặc ENI.
    - Lưu trữ trên **CloudWatch Logs** hoặc **S3**, không ghi nội dung gói tin.

- **VPC Peering** & **Transit Gateway**:

    - VPC Peering: kết nối trực tiếp 2 VPC (1:1), không hỗ trợ transitive routing, không được trùng CIDR.
    - Transit Gateway: trung tâm hub kết nối nhiều VPC hoặc mạng on-premises.

- **VPN Site-to-Site** & **Client-to-Site**:

    - Site-to-Site: kết nối trung tâm dữ liệu với AWS qua **Virtual Private Gateway** và **Customer Gateway**.
    - Client-to-Site: cho phép 1 host truy cập tài nguyên trong VPC (VPN client).

- **AWS Direct Connect**:

    - Tạo kết nối vật lý từ data center đến AWS (qua đối tác Direct Connect Partner ở VN).
    - Độ trễ thấp (~20–30ms), băng thông có thể thay đổi.

- **Elastic Load Balancing (ELB)**:

    - Phân phối lưu lượng đến nhiều EC2/Container.
    - Có 4 loại:
        - **Application Load Balancer (ALB)** – Layer 7, hỗ trợ path-based routing.
        - **Network Load Balancer (NLB)** – Layer 4, hiệu năng cao, hỗ trợ IP tĩnh.
        - **Classic Load Balancer (CLB)** – Layer 4 & 7, cũ, ít dùng.
        - **Gateway Load Balancer (GLB)** – Layer 3, dùng giao thức GENEVE (port 6081).
    - Hỗ trợ **Sticky Session** và **Access Logs** (lưu vào S3).

- **Tham dự sự kiện Vietnam Cloud Day 2025 - Track 1: GenAI and Data** giúp hiểu rõ hơn cách **AWS kết hợp trí tuệ nhân tạo và dữ liệu lớn** để tối ưu hạ tầng, tăng tốc phân tích dữ liệu, và nâng cao trải nghiệm khách hàng trong các hệ thống cloud-native.

- Hiểu được cách phân tách môi trường, bảo mật tài nguyên và định tuyến trong AWS VPC.
- Nắm được mô hình kết nối hybrid (VPN, Direct Connect) và khả năng mở rộng, bảo mật của mạng AWS.
