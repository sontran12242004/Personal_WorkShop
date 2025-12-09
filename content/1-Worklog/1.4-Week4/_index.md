---
title: "Week 4 Worklog"
date: 2025-09-09
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Week 4 Objectives:

- Deep dive into **Amazon Simple Storage Service (S3)** and related storage services.
- Understand **object-based storage** structure, distinguish from block storage.
- Master **storage classes**, **lifecycle policies**, **CORS**, and **versioning** mechanisms.
- Learn about **Amazon Glacier**, **Snow Family**, **AWS Storage Gateway**, and **AWS Backup**.
- Understand **RTO/RPO** concepts and disaster recovery strategies (Disaster Recovery).

---

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                                     | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Introduction to **Amazon S3**: concepts, characteristics of object storage.<br>- Learn about data replication mechanisms, 99.999999% durability and 99.99% availability.                                              | 01/09/2025 | 01/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Study **bucket, object, key, access point**.<br>- Learn about **storage classes**: Standard, IA, Intelligent Tiering, One Zone, Glacier, Deep Archive.                                                      | 02/09/2025 | 02/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Configure **S3 lifecycle policy** to automatically transition storage tiers.<br>- Learn about **multipart upload** and **event trigger** when uploading/deleting files.                                                            | 03/09/2025 | 03/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn about **S3 hosting static website**, **CORS policy**, **Access Control List (ACL)**, **Bucket Policy**, **IAM Policy**.<br>- Practice setting up access permissions.                                      | 04/09/2025 | 04/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Learn about **S3 Versioning**, **S3 Endpoint**, **partition & prefix performance optimization**.<br>- Study **Amazon Glacier**: data retrieval mechanisms (Expedited, Standard, Bulk).                      | 05/09/2025 | 05/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - Study **Snow Family (Snowball, Snowball Edge, Snowmobile)**.<br>- Learn about **AWS Storage Gateway** (File, Volume, Tape).<br>- Learn about **AWS Backup**, **RTO/RPO**, and **Disaster Recovery Strategies**. | 06/09/2025 | 06/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 4 Achievements:

- **Master knowledge about Amazon S3:**

    - It is an **object storage service**, cannot edit parts but must upload entire object.
    - Data is replicated across **3 Availability Zones** within the same region.
    - Supports **trigger events**, **multipart upload**, **versioning**, and **CORS configuration**.

- **Understand storage tiers (Storage Classes):**

    - **S3 Standard**: frequently accessed data.
    - **S3 Standard-IA** / **One Zone-IA**: infrequently accessed data, lower cost.
    - **S3 Intelligent-Tiering**: automatically moves data between tiers.
    - **S3 Glacier** / **Deep Archive**: long-term storage, slow retrieval, low cost.

- **Practice configuring Lifecycle Policy:**

    - Set up automatic transition of objects after X days between storage classes.
    - Automatically delete old objects after specified time.

- **Learn about CORS and permission mechanisms:**

    - **CORS (Cross-Origin Resource Sharing)** allows web apps to access resources from different domains.
    - **S3 ACL**: basic access permissions by bucket/object.
    - **Bucket Policy / IAM Policy**: defines detailed access permissions using JSON policy.

- **Understand Versioning:**

    - When versioning is enabled, deletion or overwriting does not lose old data.
    - Supports recovery of deleted or incorrectly overwritten files.

- **Optimize S3 performance:**

    - Use **random prefix** to improve search performance in partitions.
    - Understand S3's **partition & key map hash** mechanism.

- **Amazon Glacier:**

    - Serves long-term data storage with 3 retrieval levels:
        - Expedited (1–5 minutes)
        - Standard (3–5 hours)
        - Bulk (5–12 hours)

- **Snow Family:**

    - **Snowball / Snowball Edge / Snowmobile** used to **migrate large-scale data (TB → EB)** from on-premise to AWS.
    - Can process, encrypt, and compress data before importing to S3 or Glacier.

- **AWS Storage Gateway:**

    - Combines **on-premise + cloud** storage, includes 3 types:
        - **File Gateway (NFS/SMB)** → writes to S3.
        - **Volume Gateway (iSCSI)** → stores block data, snapshots to EBS.
        - **Tape Gateway (VTL)** → stores virtual tapes on S3/Glacier.

- **AWS Backup & Disaster Recovery:**
    - Understand concepts of **RTO (Recovery Time Objective)** and **RPO (Recovery Point Objective)**.
    - Distinguish 4 DR strategies:
        1. Backup & Restore
        2. Pilot Light
        3. Low Capacity Active-Active
        4. Full Capacity Active-Active
    - AWS Backup supports EBS, EC2, RDS, DynamoDB, EFS, Storage Gateway.

---
