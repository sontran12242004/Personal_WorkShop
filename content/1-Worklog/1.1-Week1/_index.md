---
title: "Week 1 Worklog"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Week 1 Objectives:

- Connect and get acquainted with members of **First Cloud Journey (FCJ)**.
- Understand basic AWS services, how to use **AWS Management Console** and **AWS CLI**.

---

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                         | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Get acquainted with FCJ members. <br> - Read and understand the rules and regulations at the internship unit.                                                                                                   | 11/08/2025 | 11/08/2025      |                                           |
| 3   | - Set up & manage AWS via **Console** and **CLI**.                                                                                                                                             | 12/08/2025 | 12/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Create AWS Free Tier account. <br> - Learn about AWS Console & AWS CLI. <br> - **Practice:** <br>&emsp; + Create AWS account. <br>&emsp; + Install AWS CLI & configure. <br>&emsp; + Use AWS CLI basics. | 13/08/2025 | 13/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn basic EC2: <br>&emsp; + Instance types, AMI, EBS,... <br> - Methods to remote SSH into EC2. <br> - Learn about **Elastic IP**.                                                              | 14/08/2025 | 15/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Practice:** <br>&emsp; + Create EC2 instance. <br>&emsp; + Connect via SSH. <br>&emsp; + Attach EBS volume.                                                                                            | 15/08/2025 | 15/08/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 1 Achievements:

- Learned how to **draw AWS architecture** on draw.io and use standard **AWS Architecture Icons**.

- Successfully **created and configured AWS Free Tier account**:

    - Understand the concepts of **Root Account** and **IAM User**.
    - Create **IAM Group** and **IAM User**, assign administrative permissions.
    - Distinguish between **Access Key/Secret Key** (CLI) and **Console Password** (Web login).

- Familiarized with **AWS Management Console**:

    - Know how to log in and operate on the web interface.
    - Manage **User Group**, **User**, and **Permissions** in IAM.
    - Understand the importance of setting passwords for Users.

- Installed and configured **AWS CLI v2** on the computer:

    - Set up Access Key, Secret Key, default Region.
    - Connect CLI to AWS account using command:
      ```bash
      aws sts get-caller-identity
      ```

- Used **AWS CLI** to perform basic operations:

    - Check account information (`aws sts get-caller-identity`).
    - Get list of regions (`aws ec2 describe-regions`).
    - View EC2 information (`aws ec2 describe-instances`).
    - Create and manage key pairs (`aws ec2 create-key-pair`).
    - Upload files to S3 (`aws s3 cp <file> s3://<bucket>`).

- Acquired the ability to combine **Console and CLI** to manage AWS resources in parallel.

- Additional learning:
    - How to create **domain** via **Route 53**.
    - Introduction to **CDN**, **AWS WAF**, and other security services.
    - Manage **spending on AWS Billing Dashboard**.
    - Learn about **AWS Support Plans**.

---
