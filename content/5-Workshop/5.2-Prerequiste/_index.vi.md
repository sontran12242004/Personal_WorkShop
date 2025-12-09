---
title : "Prerequiste"
date :  2025-09-09
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions
+ Đính kèm AdministratorAccess trong chính sách cấp phép IAM vào tài khoản AWS để quy trình làm việc dễ dàng hơn.
+ Lưu ý: Chỉ nên sử dụng quyền Quản trị viên cho môi trường Workshop để đảm bảo quá trình triển khai không bị gián đoạn. Trong môi trường Production thực tế, hãy tuân thủ nguyên tắc Quyền Tối thiểu cho mỗi dịch vụ.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}

```

#### Source Code

* Kho lưu trữ GitHub chứa mã Java và Dockerfile hợp lệ
  ![Source Code](/images/5-Workshop/5.2-Prerequisite/Source_Code.png)


