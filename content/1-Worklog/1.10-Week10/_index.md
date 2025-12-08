---
title: "Week 10 Worklog"
date: 2025-09-30
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}}
⚠️ **Note:** This is a reference version. Please **do not copy verbatim** for your internship report.
{{% /notice %}}

### 🎯 Week 10 Objectives:

* Complete HRM backend system at the level of connecting with AWS infrastructure.
* Learn and deploy **AWS IAM**, standard access permissions for the project.
* Integrate AWS services such as **S3, SES, Secrets Manager** into project.
* Add basic logging – monitoring.

---

### 📌 Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Documentation |
| --- | --------- | ------- | ----------- | -------- |
| 2 | - Review week 9 results (ALB + Auto Scaling). <br> - Receive week 10 requirements from mentor. | 08/09/2025 | 08/09/2025 | |
| 3 | - Learn about IAM: <br> &emsp;+ IAM User / Role / Group <br> &emsp;+ Inline Policy vs Managed Policy <br> &emsp;+ Best Practices "Least Privilege" | 09/09/2025 | 09/09/2025 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - **IAM Practice:** <br> &emsp;+ Create IAM Role for EC2 <br> &emsp;+ Grant S3, Secrets Manager access permissions <br> &emsp;+ Configure IAM User for dev/test | 10/09/2025 | 10/09/2025 | AWS Docs |
| 5 | - Learn about AWS S3 & integrate into HRM backend: <br> &emsp;+ Upload employee avatar <br> &emsp;+ Retrieve files with signed URL <br> &emsp;+ Set up Bucket Policy, CORS | 11/09/2025 | 11/09/2025 | AWS S3 Docs |
| 6 | - Learn about Secrets Manager: <br> &emsp;+ Store database credentials <br> &emsp;+ Get secret in Spring Boot (SDK) <br> - Learn about SES to prepare for sending HR notification emails | 12/09/2025 | 12/09/2025 | AWS Docs |

---

### ✅ Week 10 Achievements:

#### 🔐 IAM (Identity & Access Management)
* Understand authorization model in AWS – from basic to advanced.
* Successfully created:
    * IAM Role for EC2 (permissions to read S3, Secrets Manager).
    * IAM User for dev/test environment.
    * Custom Managed Policy for HRM project.
* Applied **Least Privilege** standard to all resources.

#### 📦 AWS S3 – Integration into HRM System
* Created dedicated S3 Bucket for project (avatar + employee records).
* Configured:
    * Bucket Policy
    * CORS
    * Encryption (SSE-S3)
* Spring Boot integration:
    * Upload/delete files
    * Generate Pre-signed URL
    * Validate file type & size
* Ensured personal data stored securely following Private Bucket model.

#### 🗝 AWS Secrets Manager
* Store database passwords, JWT secret key.
* Integrated Java AWS SDK to load secrets when application runs.
* Removed hard-coded credentials → increased security.

#### ✉️ AWS SES (Simple Email Service)
* Send email notifications for HRM events: leave requests, account creation, password reset.
* Configured domain verification & create identities (sandbox mode).

#### 📊 Basic Logging & Monitoring
* Integrated CloudWatch Logs in EC2.
* Push Spring Boot application logs to CloudWatch to monitor errors & performance.
* Created separate log group for each environment.

---

### 📌 Personal Notes:
This week is one of the most important weeks, as I began connecting the backend with real services on AWS. Content like IAM, S3, Secrets Manager and SES helped me better understand how enterprises design systems that are both secure and scalable. Integrating these services into the HRM project provided direct experience with cloud application development in practice.
