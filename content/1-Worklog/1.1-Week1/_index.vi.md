---
title: "Worklog Tuần 1"
date: "2025-11-11"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1:

- Kết nối, làm quen với các thành viên trong **First Cloud Journey (FCJ)**.
- Hiểu các dịch vụ AWS cơ bản, cách sử dụng **AWS Management Console** và **AWS CLI**.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Làm quen với các thành viên FCJ. <br> - Đọc và nắm các nội quy, quy định tại đơn vị thực tập.                                                                                                   | 11/08/2025   | 11/08/2025      |                                           |
| 3   | - Thiết lập & quản lý AWS qua **Console** và **CLI**.                                                                                                                                             | 12/08/2025   | 12/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tạo AWS Free Tier account. <br> - Tìm hiểu AWS Console & AWS CLI. <br> - **Thực hành:** <br>&emsp; + Tạo AWS account. <br>&emsp; + Cài AWS CLI & cấu hình. <br>&emsp; + Sử dụng AWS CLI cơ bản. | 13/08/2025   | 13/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu EC2 cơ bản: <br>&emsp; + Instance types, AMI, EBS,... <br> - Các cách remote SSH vào EC2. <br> - Tìm hiểu **Elastic IP**.                                                              | 14/08/2025   | 15/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Thực hành:** <br>&emsp; + Tạo EC2 instance. <br>&emsp; + Kết nối SSH. <br>&emsp; + Gắn EBS volume.                                                                                            | 15/08/2025   | 15/08/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 1:

- Học cách **vẽ kiến trúc AWS** trên draw.io và sử dụng các **AWS Architecture Icons** chuẩn.

- Đã **tạo và cấu hình AWS Free Tier account** thành công:

    - Hiểu khái niệm **Root Account** và **IAM User**.
    - Tạo **IAM Group** và **IAM User**, gán quyền quản trị.
    - Phân biệt **Access Key/Secret Key** (CLI) và **Console Password** (Web login).

- Làm quen với **AWS Management Console**:

    - Biết cách đăng nhập và thao tác trên giao diện web.
    - Quản lý **User Group**, **User**, và **Permissions** trong IAM.
    - Hiểu ý nghĩa việc đặt mật khẩu cho User.

- Cài đặt và cấu hình **AWS CLI v2** trên máy tính:

    - Thiết lập Access Key, Secret Key, Region mặc định.
    - Kết nối CLI với tài khoản AWS bằng lệnh:
      ```bash
      aws sts get-caller-identity
      ```

- Sử dụng **AWS CLI** để thực hiện các thao tác cơ bản:

    - Kiểm tra thông tin tài khoản (`aws sts get-caller-identity`).
    - Lấy danh sách region (`aws ec2 describe-regions`).
    - Xem thông tin EC2 (`aws ec2 describe-instances`).
    - Tạo và quản lý key pair (`aws ec2 create-key-pair`).
    - Upload file lên S3 (`aws s3 cp <file> s3://<bucket>`).

- Có khả năng kết hợp **Console và CLI** để quản lý tài nguyên AWS song song.

- Tìm hiểu thêm:
    - Cách tạo **domain** qua **Route 53**.
    - Giới thiệu về **CDN**, **AWS WAF**, và các dịch vụ bảo mật khác.
    - Quản lý **chi tiêu trên AWS Billing Dashboard**.
    - Tìm hiểu về **AWS Support Plans**.

---
