---
title: "Week 6 Worklog"
date: 2025-09-09
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

{{% notice warning %}}
⚠️ **Note:** This report is compiled for learning and reference purposes. Do not copy verbatim or use for official submission.
{{% /notice %}}

### Week 6 Objectives:

- Understand basic concepts of **Database** and **RDBMS / NoSQL**.
- Distinguish **OLTP**, **OLAP** and corresponding data systems.
- Master the structure and role of **Primary Key**, **Foreign Key**, **Index**, **Partition**, **Buffer**, **Log**.
- Learn about AWS database services such as **Amazon RDS**, **Aurora**, **Redshift**, **ElastiCache**.
- Know how to **optimize, recover, scale, and secure** databases on AWS platform.

---

### Tasks to be carried out this week:

| Day | Task                                                                                                                     | Start Date | Completion Date | Reference Material                        |
| --- | ----------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | Review database concepts: Database, Session, Primary/Foreign Key, Index, Partition, Buffer, Execution Plan, DB Log. | 15/09/2025 | 15/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | Learn about **RDBMS** (relational model, SQL) and **NoSQL** (Document, Key-Value, Graph, Wide-column).                             | 16/09/2025 | 16/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | Compare **OLTP** and **OLAP**, identify suitable application types and role of data warehouse.                     | 17/09/2025 | 17/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | Learn about **Amazon RDS**: architecture, features (backup, read replica, failover, scaling, encryption).                            | 18/09/2025 | 18/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | Learn about **Amazon Aurora**: read/write performance, backtrack, clone, global DB, multi-master.                                     | 19/09/2025 | 19/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | Learn about **Amazon Redshift** and **ElastiCache**, applications for OLAP and caching.                                                  | 20/09/2025 | 20/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 6 Achievements:

#### 🧩 **Database Concept**

- **Database**: structured/semi-structured information system, stored on devices to serve simultaneous retrieval from multiple applications.
- **Session**: time period from connection to disconnection with the database system.
- **Primary Key**: uniquely identifies each record in a table.
- **Foreign Key**: link between tables through reference to primary key.
- **Index**: data structure that helps **speed up retrieval**, but costs **memory and write overhead**.
- **Partition**: divides large tables into multiple parts to **query faster**.
- **Execution Plan**: query execution plan created by **query optimizer** to choose the most efficient approach.
- **DB Log**: records changes to help **recovery and synchronization between primary–replica**.
- **Buffer**: temporary memory area to **speed up read/write data** before syncing with disk.

---

#### **RDBMS & NoSQL**

- **RDBMS (Relational Database Management System)**:

    - Data organized by tables, rows, columns; relationships between tables represented by **keys**.
    - Uses **SQL** to query and manage data.
    - Ensures data integrity, supports ACID (Atomicity, Consistency, Isolation, Durability).

- **NoSQL (Not only SQL)**:
    - Does not store data in table format, has flexible structure.
    - Common types:
        - **Document-based**: MongoDB.
        - **Key-Value**: Redis, DynamoDB.
        - **Wide-column**: Cassandra.
        - **Graph**: Neo4j.
    - Suitable for **big data**, **unstructured**, **flexible scaling** applications.

---

#### **OLTP vs OLAP**

| Feature      | OLTP                           | OLAP                                     |
| ------------- | ------------------------------ | ---------------------------------------- |
| Purpose      | Real-time transaction processing | Historical data analysis                |
| Data       | Frequently updated          | Aggregated, read-only                        |
| Application      | Banking, retail, booking      | Reporting, BI, analytics                   |
| Focus     | Transaction processing speed         | Query response speed                 |
| AWS Technology | **RDS**, **Aurora**            | **Redshift**, **Athena**, **QuickSight** |

---

#### **Amazon RDS (Relational Database Service)**

- Fully managed database service.
- Supports: **Aurora, MySQL, PostgreSQL, MariaDB, Oracle, MSSQL**.
- **Key features:**
    1. **Automatic backup** (DB + Log, keep up to 35 days).
    2. **Read Replica** supports read workload (reporting).
    3. **Automatic failover** between Primary/Standby (Multi-AZ).
    4. **Encryption at rest & in transit**.
    5. **Auto scaling storage & instance size**.
    6. **Security with Security Group and NACL**.
    7. Commonly used for **OLTP applications**.

---

#### **Amazon Aurora**

- High-performance optimized RDBMS, compatible with **MySQL** and **PostgreSQL**.
- Inherits all RDS features, adds characteristics:
    - **Backtrack** – restore DB to previous point in time.
    - **Clone** – create fast copy.
    - **Global Database** – multiple regions with 1 master, multiple read replicas.
    - **Multi-master** – supports parallel writes from multiple nodes.
- Data stored distributed and automatically synchronized across multiple AZs.

---

#### **Amazon Redshift**

- **Data Warehouse** service managed by AWS, core is **PostgreSQL** optimized for OLAP.
- Uses **Massively Parallel Processing (MPP)** architecture:
    - **Leader Node** coordinates queries.
    - **Compute Node** stores and processes data.
- Stores data **columnar (Columnar Storage)** → optimized for analytics.
- Supports:
    - **SQL, JDBC, ODBC**.
    - **Redshift Spectrum** – query data directly in S3.
    - **Transient cluster** – cost savings when paused.

---

#### **Amazon ElastiCache**

- Fully managed **caching engine** service.
- Supports two engines: **Redis** and **Memcached**.
- Helps **reduce database load**, improve data access performance.
- AWS automatically detects and replaces failed nodes.
- Redis generally recommended more due to:
    - Rich features (replication, persistence, pub/sub).
    - High performance and good scalability.
- Need to **manage caching logic in application** to ensure data consistency.

---
