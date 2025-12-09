---
title : "Monitoring & Operations"
date : 2025-09-09
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---
### Tổng quan
* Một hệ thống sản xuất không thể được coi là hoàn chỉnh nếu thiếu khả năng Giám sát và Cảnh báo. Bạn không thể ngồi nhìn màn hình 24/7 để kiểm tra xem Máy chủ còn hoạt động hay không.

* Trong mô-đun này, chúng ta sẽ thiết lập giám sát và cảnh báo cho MiniMarket bằng các dịch vụ quản lý vận hành của AWS:

* Amazon CloudWatch: Thu thập số liệu (Metrics) từ EC2, RDS, ELB
* Amazon SNS (Simple Notification Service): Dịch vụ thông báo. Chúng ta sẽ sử dụng dịch vụ này để gửi Email cho quản trị viên khi hệ thống gặp sự cố.
* Chúng ta sẽ thiết lập Báo động CloudWatch để giám sát CPU của Máy chủ Web. Nếu CPU vượt quá 70% (dấu hiệu quá tải hoặc bị tấn công), hệ thống sẽ tự động kích hoạt SNS để gửi Email cảnh báo khẩn cấp.
  ![hosted zone](/images/5-Workshop/5.6-Cleanup/delete-zone.png)


