---
title: "Worklog Tuần 4"
date: "2025-11-11"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Báo cáo này chỉ nhằm mục đích tham khảo học tập. Vui lòng **không sao chép nguyên văn** hoặc sử dụng vào mục đích nộp chính thức.
{{% /notice %}}

### Mục tiêu tuần 4:

- Tìm hiểu chuyên sâu về **Amazon Simple Storage Service (S3)** và các dịch vụ lưu trữ liên quan.
- Hiểu cấu trúc **object-based storage**, phân biệt với block storage.
- Nắm được các **storage class**, chính sách **lifecycle**, **CORS**, và cơ chế **versioning**.
- Tìm hiểu **Amazon Glacier**, **Snow Family**, **AWS Storage Gateway**, và **AWS Backup**.
- Hiểu khái niệm **RTO/RPO** và các chiến lược phục hồi thảm họa (Disaster Recovery).

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Giới thiệu **Amazon S3**: khái niệm, đặc điểm của lưu trữ theo đối tượng.<br>- Tìm hiểu cơ chế nhân bản dữ liệu, durability 99.999999% và availability 99.99%.                                              | 01/09/2025   | 01/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Nghiên cứu **bucket, object, key, access point**.<br>- Tìm hiểu **storage class**: Standard, IA, Intelligent Tiering, One Zone, Glacier, Deep Archive.                                                      | 02/09/2025   | 02/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Cấu hình **S3 lifecycle policy** để tự động chuyển đổi cấp lưu trữ.<br>- Tìm hiểu **multipart upload** và **event trigger** khi upload/xóa file.                                                            | 03/09/2025   | 03/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Học về **S3 hosting static website**, **CORS policy**, **Access Control List (ACL)**, **Bucket Policy**, **IAM Policy**.<br>- Thực hành thiết lập phân quyền truy cập.                                      | 04/09/2025   | 04/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Tìm hiểu **S3 Versioning**, **S3 Endpoint**, **partition & prefix performance optimization**.<br>- Nghiên cứu **Amazon Glacier**: cơ chế retrieve dữ liệu (Expedited, Standard, Bulk).                      | 05/09/2025   | 05/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - Nghiên cứu **Snow Family (Snowball, Snowball Edge, Snowmobile)**.<br>- Tìm hiểu **AWS Storage Gateway** (File, Volume, Tape).<br>- Học về **AWS Backup**, **RTO/RPO**, và **Disaster Recovery Strategies**. | 06/09/2025   | 06/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 4:

- **Nắm vững kiến thức về Amazon S3:**

    - Là **dịch vụ lưu trữ đối tượng** (object storage), không chỉnh sửa từng phần mà phải upload lại toàn bộ.
    - Dữ liệu nhân bản trên **3 Availability Zones** trong cùng một region.
    - Hỗ trợ **trigger event**, **multipart upload**, **versioning**, và **CORS configuration**.

- **Hiểu rõ các lớp lưu trữ (Storage Class):**

    - **S3 Standard**: dữ liệu truy cập thường xuyên.
    - **S3 Standard-IA** / **One Zone-IA**: dữ liệu ít truy cập, chi phí thấp hơn.
    - **S3 Intelligent-Tiering**: tự động di chuyển dữ liệu giữa các lớp.
    - **S3 Glacier** / **Deep Archive**: lưu trữ dài hạn, truy xuất chậm, giá rẻ.

- **Thực hành cấu hình Lifecycle Policy:**

    - Thiết lập tự động di chuyển đối tượng sau X ngày giữa các class lưu trữ.
    - Tự động xóa đối tượng cũ sau thời gian quy định.

- **Tìm hiểu CORS và cơ chế phân quyền:**

    - **CORS (Cross-Origin Resource Sharing)** cho phép web app truy cập tài nguyên từ domain khác.
    - **S3 ACL**: quyền truy cập cơ bản theo bucket/object.
    - **Bucket Policy / IAM Policy**: xác định chi tiết quyền truy cập bằng JSON policy.

- **Hiểu rõ về Versioning:**

    - Khi bật versioning, việc xóa hoặc ghi đè không làm mất dữ liệu cũ.
    - Hỗ trợ khôi phục file bị xoá hoặc ghi đè sai.

- **Tối ưu hiệu suất S3:**

    - Dùng **random prefix** để tăng hiệu năng tìm kiếm trong partition.
    - Hiểu cơ chế **partition & key map hash** của S3.

- **Amazon Glacier:**

    - Phục vụ lưu trữ dữ liệu dài hạn với 3 cấp độ truy xuất:
        - Expedited (1–5 phút)
        - Standard (3–5 giờ)
        - Bulk (5–12 giờ)

- **Snow Family:**

    - **Snowball / Snowball Edge / Snowmobile** dùng để **migrate dữ liệu quy mô lớn (TB → EB)** từ on-premise sang AWS.
    - Có thể xử lý, mã hóa, và nén dữ liệu trước khi import lên S3 hoặc Glacier.

- **AWS Storage Gateway:**

    - Kết hợp lưu trữ **on-premise + cloud**, gồm 3 loại:
        - **File Gateway (NFS/SMB)** → ghi vào S3.
        - **Volume Gateway (iSCSI)** → lưu block data, snapshot lên EBS.
        - **Tape Gateway (VTL)** → lưu băng ảo trên S3/Glacier.

- **AWS Backup & Disaster Recovery:**
    - Hiểu các khái niệm **RTO (Recovery Time Objective)** và **RPO (Recovery Point Objective)**.
    - Phân biệt 4 chiến lược DR:
        1. Backup & Restore
        2. Pilot Light
        3. Low Capacity Active-Active
        4. Full Capacity Active-Active
    - AWS Backup hỗ trợ EBS, EC2, RDS, DynamoDB, EFS, Storage Gateway.

---
