---
title: "Week 5 Worklog"
date: 2025-09-09
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

{{% notice warning %}}
⚠️ **Note:** This report is compiled for learning and reference purposes. Do not copy verbatim or use for official submission.
{{% /notice %}}

### Week 5 Objectives:

- Understand the **Shared Responsibility Model** and security responsibilities between AWS and customers.
- Master **Identity and Access Management (IAM)** mechanisms — users, groups, roles, policies.
- Learn about **AWS Organization**, **AWS Identity Center**, and **AWS KMS** in security management.
- Know how to **authorize, encrypt, and audit security** in AWS environment.
- Familiarize with **Amazon Cognito** and **AWS Security Hub**.

---

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                              | Start Date | Completion Date | Reference Material                        |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | Learn about **Shared Responsibility Model**: division of responsibilities between AWS and customers; service classification by management level (Infrastructure, Hybrid, Fully-managed). | 08/09/2025 | 08/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | Study **Root Account** and **best practices**: create IAM admin user, lock root credentials, protect domain/email information.                                       | 09/09/2025 | 09/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | Learn about **IAM Users, Groups, Roles, Policies** — authorization mechanisms, trust policy, explicit deny.                                                                   | 10/09/2025 | 10/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | Practice creating **IAM Role**, **assume role**, and using **AWS STS (Security Token Service)** to grant temporary permissions.                                                         | 11/09/2025 | 11/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | Learn about **Amazon Cognito** (User Pool & Identity Pool) — login, authentication, and AWS resource access mechanisms.                                                       | 12/09/2025 | 12/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | Study **AWS Organization**, **Identity Center**, **KMS**, and **Security Hub** — centralized management, data encryption, automated security assessment.                      | 13/09/2025 | 13/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 5 Achievements:

- **Shared Responsibility Model**
    - AWS is responsible for **security _of_ the cloud** — hardware, infrastructure, hypervisor.
    - Customers are responsible for **security _in_ the cloud** — service configuration, data, encryption, user management.
    - Level of responsibility varies by service type:
        - **Infrastructure (EC2, VPC, EBS)** → customers bear most.
        - **Managed Services (RDS, EKS)** → shared.
        - **Fully Managed (S3, Lambda)** → AWS bears more.

---

- **AWS Root Account**
    - Has full access to all AWS services and resources.
    - Should **lock credentials**, only use in emergencies.
    - **Best practices:**
        - Create **IAM Administrator user** to replace root.
        - **Split root access** between 2 different administrators.
        - Ensure **domain and email root account renewal** regularly.

---

- **AWS Identity and Access Management (IAM)**
    - Service that controls access to AWS resources.
    - **Principals** include:
        - AWS Root user, IAM user, Federated user (via SAML/OAuth), IAM role, AWS service, Anonymous user.
    - **IAM User**:
        - No default permissions.
        - Can log in via **Management Console** or use **Access Key/Secret Key**.
        - Not used to manage applications/operating systems.
    - **IAM Group**:
        - Groups users for easier management (nested groups not supported).
    - **IAM Policy (JSON)**:
        - **Identity-based policy** (attached to Principal).
        - **Resource-based policy** (attached to Resource).
        - Always prioritize **explicit Deny > Allow**.
    - **IAM Role**:
        - No fixed credentials, only used when **assumed**.
        - **Trust policy** determines who can assume the role.
        - Used for cross-account access or granting permissions to **AWS services (like EC2)**.
        - Linked with **AWS STS** to provide temporary credentials.

---

- **Amazon Cognito**
    - Service for **authentication and user management** for web/mobile apps.
    - Consists of 2 components:
        - **User Pool** – stores, registers, and authenticates users.
        - **Identity Pool** – grants access to AWS services.
    - Supports login via **username/password** or **third-party (Facebook, Google, Amazon)**.
    - User Pool combined with Identity Pool to directly access AWS resources after authentication.

---

- **AWS Organization**
    - Allows management of **multiple AWS accounts** within the same organization.
    - Supports **OU (Organizational Units)** and **Consolidated Billing**.
    - Can assign **Service Control Policy (SCP)** to set maximum permission limits on OU/account.
    - SCP is **deny-based**, does not grant permissions but only sets boundaries.

---

- **AWS Identity Center (formerly SSO)**
    - Manages access for multiple AWS accounts and external applications.
    - **Identity source** can be:
        - AWS Managed AD
        - On-premises AD (via Trust/Connector)
    - **Permission Set**: set of permissions assigned to user/group → grants role to accounts in Organization.

---

- **AWS Key Management Service (KMS)**
    - Service that creates and manages **encryption keys** to encrypt data.
    - Supports **Encryption at Rest** meeting **FIPS 140-2** standards.
    - **CMK (Customer Managed Key)** commonly used to create **Data Key**, serving actual data encryption.
    - AWS never exports CMK out of the system.

---

- **AWS Security Hub**
    - Service that automatically audits security based on **AWS Best Practices** and **industry standards**.
    - Runs continuously, evaluates service configurations and displays **security score**.
    - Supports aggregating security alerts from multiple other services (GuardDuty, Config, Inspector...).

---
