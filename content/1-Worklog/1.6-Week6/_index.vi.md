---
title: "Worklog Tuần 6"
date: "2025-11-11"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Báo cáo này được tổng hợp cho mục đích học tập và tham khảo. Không được sao chép nguyên văn hoặc sử dụng vào mục đích nộp chính thức.
{{% /notice %}}

### Mục tiêu tuần 6:

- Hiểu khái niệm cơ bản của **cơ sở dữ liệu (Database Concept)** và **RDBMS / NoSQL**.
- Phân biệt **OLTP**, **OLAP** và các hệ thống dữ liệu tương ứng.
- Nắm vững cấu trúc và vai trò của **Primary Key**, **Foreign Key**, **Index**, **Partition**, **Buffer**, **Log**.
- Tìm hiểu các dịch vụ cơ sở dữ liệu trên AWS như **Amazon RDS**, **Aurora**, **Redshift**, **ElastiCache**.
- Biết được cách **tối ưu, phục hồi, mở rộng và bảo mật** cơ sở dữ liệu trên nền tảng AWS.

---

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ----------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | Ôn tập các khái niệm cơ sở dữ liệu: Database, Session, Primary/Foreign Key, Index, Partition, Buffer, Execution Plan, DB Log. | 15/09/2025   | 15/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | Tìm hiểu **RDBMS** (mô hình quan hệ, SQL) và **NoSQL** (Document, Key-Value, Graph, Wide-column).                             | 16/09/2025   | 16/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | So sánh **OLTP** và **OLAP**, xác định loại ứng dụng phù hợp và vai trò của kho dữ liệu (Data Warehouse).                     | 17/09/2025   | 17/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | Học về **Amazon RDS**: kiến trúc, tính năng (backup, read replica, failover, scaling, encryption).                            | 18/09/2025   | 18/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | Tìm hiểu **Amazon Aurora**: hiệu năng đọc/ghi, backtrack, clone, global DB, multi-master.                                     | 19/09/2025   | 19/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | Học về **Amazon Redshift** và **ElastiCache**, ứng dụng cho OLAP và caching.                                                  | 20/09/2025   | 20/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Kết quả đạt được tuần 6:

#### 🧩 **Database Concept**

- **Database (CSDL)**: hệ thống thông tin có cấu trúc/bán cấu trúc, lưu trữ trên thiết bị nhằm phục vụ truy xuất đồng thời từ nhiều ứng dụng.
- **Session**: khoảng thời gian từ khi kết nối đến khi ngắt kết nối với hệ CSDL.
- **Primary Key**: xác định duy nhất mỗi bản ghi trong bảng.
- **Foreign Key**: liên kết giữa các bảng thông qua tham chiếu đến khóa chính.
- **Index**: cấu trúc dữ liệu giúp **tăng tốc truy xuất**, nhưng tốn **bộ nhớ và chi phí ghi**.
- **Partition**: chia nhỏ bảng lớn thành nhiều phần để **truy vấn nhanh hơn**.
- **Execution Plan**: kế hoạch thực thi truy vấn do **query optimizer** tạo ra để chọn phương án hiệu quả nhất.
- **DB Log**: lưu vết thay đổi giúp **khôi phục và đồng bộ giữa primary–replica**.
- **Buffer**: vùng nhớ tạm để **tăng tốc đọc/ghi dữ liệu** trước khi đồng bộ với đĩa.

---

#### **RDBMS & NoSQL**

- **RDBMS (Relational Database Management System)**:

    - Dữ liệu được tổ chức theo bảng, hàng, cột; quan hệ giữa các bảng được biểu diễn bằng **khóa**.
    - Sử dụng **SQL** để truy vấn và quản lý dữ liệu.
    - Đảm bảo tính toàn vẹn dữ liệu, hỗ trợ ACID (Atomicity, Consistency, Isolation, Durability).

- **NoSQL (Not only SQL)**:
    - Không lưu dữ liệu dạng bảng, có cấu trúc linh hoạt.
    - Các loại phổ biến:
        - **Document-based**: MongoDB.
        - **Key-Value**: Redis, DynamoDB.
        - **Wide-column**: Cassandra.
        - **Graph**: Neo4j.
    - Phù hợp ứng dụng **dữ liệu lớn**, **phi cấu trúc**, **mở rộng linh hoạt**.

---

#### **OLTP vs OLAP**

| Đặc điểm      | OLTP                           | OLAP                                     |
| ------------- | ------------------------------ | ---------------------------------------- |
| Mục tiêu      | Xử lý giao dịch thời gian thực | Phân tích dữ liệu lịch sử                |
| Dữ liệu       | Cập nhật thường xuyên          | Tổng hợp, chỉ đọc                        |
| Ứng dụng      | Ngân hàng, bán lẻ, đặt vé      | Báo cáo, BI, phân tích                   |
| Trọng tâm     | Tốc độ xử lý giao dịch         | Tốc độ phản hồi truy vấn                 |
| Công nghệ AWS | **RDS**, **Aurora**            | **Redshift**, **Athena**, **QuickSight** |

---

#### **Amazon RDS (Relational Database Service)**

- Dịch vụ cơ sở dữ liệu được quản lý hoàn toàn.
- Hỗ trợ: **Aurora, MySQL, PostgreSQL, MariaDB, Oracle, MSSQL**.
- **Tính năng chính:**
    1. **Tự động sao lưu** (DB + Log, giữ tối đa 35 ngày).
    2. **Read Replica** hỗ trợ workload đọc (reporting).
    3. **Failover tự động** giữa Primary/Standby (Multi-AZ).
    4. **Encryption at rest & in transit**.
    5. **Auto scaling storage & instance size**.
    6. **Bảo mật bằng Security Group và NACL**.
    7. Thường dùng cho **ứng dụng OLTP**.

---

#### **Amazon Aurora**

- RDBMS tối ưu hiệu năng cao, tương thích **MySQL** và **PostgreSQL**.
- Kế thừa toàn bộ tính năng của RDS, thêm các đặc điểm:
    - **Backtrack** – khôi phục DB về thời điểm trước đó.
    - **Clone** – tạo bản sao nhanh.
    - **Global Database** – nhiều vùng (region) với 1 master, nhiều read replica.
    - **Multi-master** – hỗ trợ ghi song song từ nhiều node.
- Dữ liệu được lưu trữ phân tán và đồng bộ tự động trên nhiều AZ.

---

#### **Amazon Redshift**

- Dịch vụ **Data Warehouse** do AWS quản lý, lõi là **PostgreSQL** được tối ưu cho OLAP.
- Sử dụng kiến trúc **Massively Parallel Processing (MPP)**:
    - **Leader Node** điều phối truy vấn.
    - **Compute Node** lưu trữ và xử lý dữ liệu.
- Lưu trữ dữ liệu **theo cột (Columnar Storage)** → tối ưu cho phân tích.
- Hỗ trợ:
    - **SQL, JDBC, ODBC**.
    - **Redshift Spectrum** – truy vấn trực tiếp dữ liệu trong S3.
    - **Transient cluster** – tiết kiệm chi phí khi tạm ngưng.

---

#### **Amazon ElastiCache**

- Dịch vụ **caching engine** được quản lý hoàn toàn.
- Hỗ trợ hai engine: **Redis** và **Memcached**.
- Giúp **giảm tải database**, tăng hiệu năng truy cập dữ liệu.
- AWS tự động phát hiện và thay thế node lỗi.
- Redis thường được khuyến nghị hơn do:
    - Tính năng phong phú (replication, persistence, pub/sub).
    - Hiệu năng cao và mở rộng tốt.
- Cần **quản lý caching logic trong ứng dụng** để đảm bảo dữ liệu đồng nhất.

---
