---
title: "Week 3 Worklog"
date: 2025-09-09
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---



### Week 3 Objectives:

- Deep dive into **Amazon EC2** and related storage services (EBS, EFS, FSx).
- Understand EC2 operation mechanisms, configuration, and pricing options.
- Practice creating and managing EC2 Instance, EBS Volume, Snapshot.
- Understand and apply Auto Scaling, Pricing Options, and Lightsail.

---

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                            | Start Date | Completion Date | Reference Material                        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Overview of **Amazon EC2**: concepts, scalability, comparison with physical servers.<br>- Learn about **Instance Types**, specifications (CPU, Memory, Network, Storage).                         | 25/08/2025 | 25/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Study **AMI (Amazon Machine Image)**, EC2 Instance provisioning process.<br>- Learn about **Hypervisor (KVM, HVM, PV)** and selection mechanisms.                                                       | 26/08/2025 | 26/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn about **Key Pair** and login encryption mechanisms (Linux/Windows).<br>- Learn about **EBS (Elastic Block Store)**: HDD types, SSD, snapshot, data replication.                               | 27/08/2025 | 27/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Study **Instance Store**: characteristics, advantages/disadvantages, practical applications.<br>- Practice backing up EBS using snapshot and recovery.                                                                     | 28/08/2025 | 28/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Learn about **User Data** and **Metadata** in EC2, how to automate when initializing Instance.<br>- Learn about **EC2 Auto Scaling**, Scaling Policy, Load Balancer integration.                             | 29/08/2025 | 29/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - Learn about **Pricing Options**: On-demand, Reserved Instance, Saving Plan, Spot Instance.<br>- Study **Amazon Lightsail**, **EFS**, **FSx**, and **AWS Application Migration Service (MGN)**. | 30/08/2025 | 30/08/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 3 Achievements:

- Understand **Amazon EC2 architecture and operation**:

    - Similar to traditional virtual machines but with flexible scalability.
    - Can be quickly initialized, supports various workloads such as web, applications, database...

- Master **EC2 Instance Type**, **AMI**, **Hypervisor**, **Key Pair** mechanisms.
- Know how to **create, connect, and backup EC2 Instance** through EBS snapshot.
- Understand the differences between **EBS**, **Instance Store**, **EFS**, and **FSx**:

    - EBS: block storage directly attached to EC2, operates independently via private network.
    - Instance Store: extremely high speed, does not persist data when instance is stopped.
    - EFS: shared storage for multiple EC2 (Linux).
    - FSx: similar to EFS but supports NTFS and SMB (Windows/Linux).

- Master **EC2 Auto Scaling**:

    - Automatically increases/decreases number of Instances.
    - Supports multiple AZs and integrates with Load Balancer.
    - Can combine multiple Pricing Options.

- Understand 4 main **Pricing Options**:

    - On-demand: flexible, higher price.
    - Reserved Instance & Saving Plan: savings with long-term commitment.
    - Spot Instance: low price, but may be terminated suddenly.

- Know how to use **Amazon Lightsail** for light workloads, test/dev.
- Master **AWS Application Migration Service (MGN)** mechanism to replicate physical/virtual servers to EC2.
- Understand data replication process, incremental snapshot, and cost optimization through deduplication.
- Completed **documentation and detailed notes** on the entire chain of services related to EC2.

---
