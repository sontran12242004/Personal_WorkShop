---
title: "Worklog Tuần 10"
date: 2025-09-30
weight: 1
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Đây là bản tham khảo. Vui lòng **không sao chép nguyên văn** cho bài báo cáo thực tập của bạn.
{{% /notice %}}

### 🎯 Mục tiêu tuần 10:

* Hoàn thiện hệ thống backend HRM ở mức kết nối với hạ tầng AWS.
* Tìm hiểu và triển khai **AWS IAM**, phân quyền truy cập chuẩn cho dự án.
* Tích hợp các dịch vụ AWS như **S3, SES, Secrets Manager** vào project.
* Bổ sung logging – monitoring cơ bản.

---

### 📌 Các công việc triển khai trong tuần:

| Thứ | Công việc | Bắt đầu | Hoàn thành | Tài liệu |
| --- | --------- | ------- | ----------- | -------- |
| 2 | - Review kết quả tuần 9 (ALB + Auto Scaling). <br> - Nhận yêu cầu tuần 10 từ mentor. | 08/09/2025 | 08/09/2025 | |
| 3 | - Tìm hiểu IAM: <br> &emsp;+ IAM User / Role / Group <br> &emsp;+ Inline Policy vs Managed Policy <br> &emsp;+ Best Practices “Least Privilege” | 09/09/2025 | 09/09/2025 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - **Thực hành IAM:** <br> &emsp;+ Tạo IAM Role cho EC2 <br> &emsp;+ Gán quyền truy cập S3, Secrets Manager <br> &emsp;+ Cấu hình IAM User dev/test | 10/09/2025 | 10/09/2025 | AWS Docs |
| 5 | - Tìm hiểu AWS S3 & tích hợp vào backend HRM: <br> &emsp;+ Upload avatar nhân viên <br> &emsp;+ Truy xuất file có signed URL <br> &emsp;+ Thiết lập Bucket Policy, CORS | 11/09/2025 | 11/09/2025 | AWS S3 Docs |
| 6 | - Tìm hiểu Secrets Manager: <br> &emsp;+ Lưu database credentials <br> &emsp;+ Lấy secret trong Spring Boot (SDK) <br> - Tìm hiểu SES để chuẩn bị gửi mail thông báo nhân sự | 12/09/2025 | 12/09/2025 | AWS Docs |

---

### ✅ Kết quả đạt được tuần 10:

#### 🔐 IAM (Identity & Access Management)
* Hiểu mô hình phân quyền trong AWS – từ cơ bản đến nâng cao.
* Tạo được:
    * IAM Role cho EC2 (quyền đọc S3, Secrets Manager).
    * IAM User dành cho môi trường dev/test.
    * Managed Policy tùy chỉnh cho dự án HRM.
* Áp dụng chuẩn **Least Privilege** cho toàn bộ tài nguyên.

#### 📦 AWS S3 – Tích hợp vào hệ thống HRM
* Tạo S3 Bucket riêng cho dự án (avatar + hồ sơ nhân viên).
* Cấu hình:
    * Bucket Policy
    * CORS
    * Encryption (SSE-S3)
* Tích hợp Spring Boot:
    * Upload/delete file
    * Generate Pre-signed URL
    * Validate file type & size
* Đảm bảo dữ liệu cá nhân được lưu trữ an toàn theo mô hình Private Bucket.

#### 🗝 AWS Secrets Manager
* Lưu mật khẩu database, JWT secret key.
* Tích hợp Java AWS SDK để load secret khi ứng dụng chạy.
* Loại bỏ hard-code credentials → tăng bảo mật.

#### ✉️ AWS SES (Simple Email Service)
* Gửi email thông báo sự kiện HRM: nghỉ phép, cấp tài khoản, quên mật khẩu.
* Cấu hình domain verification & create identities (sandbox mode).

#### 📊 Logging & Monitoring cơ bản
* Tích hợp CloudWatch Logs trong EC2.
* Đẩy log ứng dụng Spring Boot lên CloudWatch để theo dõi error & performance.
* Tạo log group riêng cho từng môi trường.

---

### 📌 Nhận xét cá nhân:
Tuần này là một trong những tuần quan trọng nhất, vì tôi bắt đầu kết nối backend với các dịch vụ thực tế trên AWS. Các nội dung như IAM, S3, Secrets Manager và SES giúp tôi hiểu rõ hơn về cách doanh nghiệp thiết kế hệ thống vừa an toàn, vừa dễ mở rộng. Việc tích hợp các dịch vụ này vào dự án HRM mang lại trải nghiệm trực tiếp về cloud application development trong thực tế.

