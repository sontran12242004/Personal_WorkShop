---
title: "Worklog Tuần 11"
date: 2025-10-07
weight: 1
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Đây là bản tham khảo. Vui lòng **không sao chép nguyên văn** cho bài báo cáo thực tập của bạn.
{{% /notice %}}

### 🎯 Mục tiêu tuần 11:

* Hoàn thiện các module còn lại của hệ thống quản lý nhân sự (HRM).
* Tích hợp **AWS SES** để gửi email tự động cho các nghiệp vụ nhân sự.
* Sử dụng **AWS CloudWatch** để theo dõi, ghi log và cảnh báo.
* Thực hiện **Integration Testing**, chuẩn bị bước sang giai đoạn test toàn hệ thống.

---

### 📌 Các công việc triển khai trong tuần:

| Thứ | Công việc | Bắt đầu | Hoàn thành | Tài liệu |
| --- | --------- | ------- | ----------- | -------- |
| 2  | - Tổng hợp feedback từ mentor tuần 10 <br> - Cập nhật lại IAM Role & S3 Policy cho chuẩn | 15/09/2025 | 15/09/2025 |
| 3  | - Tích hợp SES vào hệ thống HRM: <br> &emsp;+ Gửi email thông báo nghỉ phép <br> &emsp;+ Gửi mail onboarding nhân viên mới <br> &emsp;+ Gửi OTP / quên mật khẩu | 16/09/2025 | 16/09/2025 | AWS SES Docs |
| 4  | - Thiết lập CloudWatch Logs cho ứng dụng EC2 <br> - Tạo Log Group / Metric Filter <br> - Thử tạo cảnh báo đơn giản (Error > 10 events) | 17/09/2025 | 17/09/2025 | AWS CloudWatch Docs |
| 5  | - Tối ưu backend HRM: <br> &emsp;+ Query payroll <br> &emsp;+ Sửa logic attendance <br> &emsp;+ Cải thiện API response time | 18/09/2025 | 18/09/2025 | |
| 6  | - Viết Integration Test (JUnit + Mockito) cho: <br> &emsp;+ Payroll Service <br> &emsp;+ Attendance Service <br> &emsp;+ Leave Management | 19/09/2025 | 19/09/2025 | Testing Docs |

---

### ✅ Kết quả đạt được tuần 11:

#### ✉️ AWS SES – Email Automation
Đã tích hợp thành công các tính năng gửi email từ hệ thống HRM:
* Email thông báo nghỉ phép cho cấp quản lý.
* Email gửi tài khoản & mật khẩu cho nhân viên mới.
* Email OTP / quên mật khẩu.
* Hiểu và áp dụng:
    * Domain verification
    * Identity management
    * Sandbox mode (test environment)

Tạo được service gửi email có thể tái sử dụng, gọn nhẹ và dễ mở rộng.

---

#### 📊 AWS CloudWatch – Logging & Monitoring
* Push log từ EC2 & Spring Boot lên CloudWatch.
* Tạo Log Group riêng: `hrm-backend-prod-logs`.
* Thiết lập Metric Filter để phát hiện:
    * ERROR
    * WARN
    * Unauthorized access
* Tạo CloudWatch Alarm gửi email khi số lượng lỗi vượt quá ngưỡng.

---

#### ⚙️ Backend Optimization (HRM Project)
* Giảm thời gian xử lý payroll bằng cách tối ưu SQL + Stream API.
* Sửa logic attendance tracking bị double-check.
* Tăng hiệu năng API nhờ cache cục bộ (Local caching).
* Chuẩn hóa DTO + Response Model.

---

#### 🧪 Integration Testing (Spring Boot)
Đã viết test cho các module chính:

| Module | Tiến độ |
| ------ | ------- |
| Payroll Service | ✔ Hoàn thành |
| Attendance Service | ✔ Hoàn thành |
| Leave Management | ✔ Hoàn thành |
| Authentication + JWT | ⏳ Đang thực hiện (sang tuần 12) |

Kết quả:
* Đạt độ bao phủ (coverage) ~ 65%.
* Phát hiện và sửa 3 lỗi logic do thiếu kiểm tra null & missing field.

---

### 📌 Nhận xét cá nhân:
Tuần 11 giúp tôi hiểu quy trình hoàn thiện backend trong môi trường cloud thực tế: từ email service, logging, monitoring cho đến kiểm thử tích hợp. Mặc dù workload khá nhiều, nhưng nhờ chia tasks rõ ràng và được mentor hỗ trợ, tôi đã triển khai được tr
