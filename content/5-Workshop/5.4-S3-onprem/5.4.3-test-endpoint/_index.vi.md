---
title : "Initialize ElastiCache Redis"
date : 2025-09-09
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

1. Truy cập ElastiCache > Nhóm mạng con > Tạo nhóm mạng con
   Tên: redis-private-group
   Mạng con: Chọn 2 Mạng con riêng tư
   ![dns name](/images/5-Workshop/5.4-S3-onprem/dns.png)
   ![Start session](/images/5-Workshop/5.4-S3-onprem/start-session.png)

2Go to Redis OSS caches > Create cache
![Start session](/images/5-Workshop/5.4-S3-onprem/start-session1.png)

3. Tại màn hình Cài đặt cụm:
* Engine: Chọn Redis OSS
* Tùy chọn triển khai: Chọn Cụm dựa trên Node
* Phương pháp tạo: Chọn Bộ đệm cụm (Cấu hình và tạo cụm mới)
* Chế độ cụm: Chọn Tắt (Chế độ đơn giản, 1 phân đoạn)
  ![Start session](/images/5-Workshop/5.4-S3-onprem/start-session2.png)

4. Tại màn hình Vị trí:

* Vị trí: AWS Cloud
* Multi-AZ: Bỏ chọn (Bật) Lưu ý: Tắt tính năng này để tiết kiệm chi phí cho môi trường Lab
* Tự động chuyển đổi dự phòng: Bỏ chọn (Bật)
* Tại màn hình Cài đặt bộ đệm:

5. Phiên bản engine: Giữ nguyên mặc định (Ví dụ: 7.1)
* Cổng: 6379
* Loại nút: Chọn họ t3 > Chọn cache.t3.micro
* Số lượng bản sao: Nhập 0 (Chúng ta chỉ cần 1 nút chính, không cần nút bản sao)

![user](/images/5-Workshop/5.4-S3-onprem/cli1.png)
![copy file](/images/5-Workshop/5.4-S3-onprem/cli2.png)


6. Tại màn hình Kết nối:
* Kiểu mạng: IPv4
* Nhóm mạng con: Chọn Chọn nhóm mạng con hiện có > Chọn redis-private-group vừa tạo
  ![copy file](/images/5-Workshop/5.4-S3-onprem/cli2.3.png)


7. Tại màn hình Cài đặt nâng cao (Quan trọng):
* Mã hóa khi lưu trữ: Bật (Mặc định)
* Mã hóa khi truyền: Bỏ chọn (Tắt)
* Lý do: Tắt mã hóa khi truyền giúp đơn giản hóa kết nối từ mã .NET trong môi trường VPC nội bộ mà không cần cấu hình chứng chỉ SSL phức tạp
* Nhóm bảo mật đã chọn: Chọn Quản lý > chọn sg-redis-cache (Bỏ chọn mặc định)

![check bucket](/images/5-Workshop/5.4-S3-onprem/check-bucket.png)

8. Scroll to the bottom and click Create
### 3. Get connection information
Quá trình khởi tạo sẽ mất khoảng 5-10 phút

1. Khi trạng thái chuyển sang Khả dụng (Màu xanh lá cây)
2. Nhấp vào Tên cụm (webapp hoặc tên bạn đã đặt)
3. Tại tab Tổng quan, tìm Điểm cuối chính
4. Sao chép chuỗi kết nối này (Ví dụ: webapp.xxxx.cache.amazonaws.com)
   ![check bucket](/images/5-Workshop/5.4-S3-onprem/check-bucket2.png)




