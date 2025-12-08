---
title: "Worklog Tuần 5"
date: "2025-11-11"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Báo cáo này được tổng hợp cho mục đích học tập và tham khảo. Không được sao chép nguyên văn hoặc sử dụng vào mục đích nộp chính thức.
{{% /notice %}}

### Mục tiêu tuần 5:

- Hiểu rõ mô hình **Shared Responsibility Model** và trách nhiệm bảo mật giữa AWS và khách hàng.
- Nắm vững cơ chế **Identity and Access Management (IAM)** — users, groups, roles, policies.
- Tìm hiểu **AWS Organization**, **AWS Identity Center**, và **AWS KMS** trong quản lý bảo mật.
- Biết cách **phân quyền, mã hóa, và kiểm tra bảo mật** trong môi trường AWS.
- Làm quen với **Amazon Cognito** và **AWS Security Hub**.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                              | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | Tìm hiểu **Shared Responsibility Model**: phân chia trách nhiệm giữa AWS và khách hàng; phân loại dịch vụ theo mức độ quản lý (Infrastructure, Hybrid, Fully-managed). | 08/09/2025   | 08/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | Nghiên cứu **Root Account** và các **best practices**: tạo IAM admin user, khóa credentials root, bảo vệ thông tin domain/email.                                       | 09/09/2025   | 09/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | Tìm hiểu về **IAM Users, Groups, Roles, Policies** — cơ chế phân quyền, trust policy, explicit deny.                                                                   | 10/09/2025   | 10/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | Thực hành tạo **IAM Role**, **assume role**, và sử dụng **AWS STS (Security Token Service)** để cấp quyền tạm.                                                         | 11/09/2025   | 11/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | Tìm hiểu **Amazon Cognito** (User Pool & Identity Pool) — cơ chế đăng nhập, xác thực và truy cập tài nguyên AWS.                                                       | 12/09/2025   | 12/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | Nghiên cứu **AWS Organization**, **Identity Center**, **KMS**, và **Security Hub** — quản lý tập trung, mã hóa dữ liệu, đánh giá bảo mật tự động.                      | 13/09/2025   | 13/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 5:

- **Shared Responsibility Model**
    - AWS chịu trách nhiệm **bảo mật của đám mây (security _of_ the cloud)** — phần cứng, cơ sở hạ tầng, hypervisor.
    - Khách hàng chịu trách nhiệm **bảo mật trong đám mây (security _in_ the cloud)** — cấu hình dịch vụ, dữ liệu, mã hóa, quản lý người dùng.
    - Mức độ trách nhiệm thay đổi tùy theo loại dịch vụ:
        - **Infrastructure (EC2, VPC, EBS)** → khách hàng chịu phần lớn.
        - **Managed Services (RDS, EKS)** → chia sẻ.
        - **Fully Managed (S3, Lambda)** → AWS chịu nhiều hơn.

---

- **AWS Root Account**
    - Có toàn quyền truy cập mọi dịch vụ và tài nguyên AWS.
    - Nên **khóa thông tin xác thực**, chỉ dùng trong trường hợp khẩn cấp.
    - **Best practices:**
        - Tạo **IAM Administrator user** để thay thế root.
        - **Chia nhỏ quyền root access** cho 2 người quản lý khác nhau.
        - Đảm bảo **gia hạn domain và email root account** thường xuyên.

---

- **AWS Identity and Access Management (IAM)**
    - Là dịch vụ kiểm soát quyền truy cập đến tài nguyên AWS.
    - Các **Principal** gồm:
        - AWS Root user, IAM user, Federated user (qua SAML/OAuth), IAM role, AWS service, Anonymous user.
    - **IAM User**:
        - Không có quyền mặc định.
        - Có thể đăng nhập qua **Management Console** hoặc dùng **Access Key/Secret Key**.
        - Không dùng để quản lý ứng dụng/hệ điều hành.
    - **IAM Group**:
        - Gom nhóm user để quản lý dễ dàng hơn (không lồng nhóm được).
    - **IAM Policy (JSON)**:
        - **Identity-based policy** (gắn với Principal).
        - **Resource-based policy** (gắn với Resource).
        - Luôn ưu tiên **explicit Deny > Allow**.
    - **IAM Role**:
        - Không có credential cố định, chỉ dùng khi được **assume**.
        - **Trust policy** xác định ai được assume role.
        - Dùng cho cross-account access hoặc cấp quyền cho **AWS services (như EC2)**.
        - Liên kết với **AWS STS** để cấp temporary credentials.

---

- **Amazon Cognito**
    - Dịch vụ **xác thực và quản lý người dùng** cho web/mobile app.
    - Gồm 2 thành phần:
        - **User Pool** – lưu trữ, đăng ký và xác thực người dùng.
        - **Identity Pool** – cấp quyền truy cập vào dịch vụ AWS.
    - Hỗ trợ đăng nhập qua **username/password** hoặc **third-party (Facebook, Google, Amazon)**.
    - User Pool kết hợp với Identity Pool để truy cập trực tiếp tài nguyên AWS sau khi xác thực.

---

- **AWS Organization**
    - Cho phép quản lý **nhiều AWS account** trong cùng tổ chức.
    - Hỗ trợ **OU (Organizational Units)** và **Consolidated Billing**.
    - Có thể gán **Service Control Policy (SCP)** để giới hạn quyền tối đa trên OU/account.
    - SCP là **deny-based**, không cấp quyền mà chỉ thiết lập rào giới hạn.

---

- **AWS Identity Center (trước đây là SSO)**
    - Quản lý truy cập cho nhiều tài khoản AWS và ứng dụng bên ngoài.
    - **Identity source** có thể là:
        - AWS Managed AD
        - On-premises AD (qua Trust/Connector)
    - **Permission Set**: bộ quyền được gán cho user/group → cấp role vào các account trong Organization.

---

- **AWS Key Management Service (KMS)**
    - Dịch vụ tạo và quản lý **encryption key** để mã hóa dữ liệu.
    - Hỗ trợ **Encryption at Rest** đạt chuẩn **FIPS 140-2**.
    - **CMK (Customer Managed Key)** thường dùng để tạo **Data Key**, phục vụ mã hóa dữ liệu thực tế.
    - AWS không bao giờ xuất CMK ra khỏi hệ thống.

---

- **AWS Security Hub**
    - Dịch vụ kiểm tra bảo mật tự động dựa trên **AWS Best Practices** và **tiêu chuẩn ngành**.
    - Chạy liên tục, đánh giá cấu hình dịch vụ và hiển thị **điểm số bảo mật**.
    - Hỗ trợ tổng hợp cảnh báo bảo mật từ nhiều dịch vụ khác (GuardDuty, Config, Inspector...).

---
