---
title: "Week 8 Worklog"
date: 2025-09-16
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice warning %}}
⚠️ **Note:** This is a reference version. Please *do not copy verbatim* for your report.
{{% /notice %}}

### 🎯 Week 8 Objectives:
* Master knowledge about **AWS VPC**, network security, subnet structure.
* Know how to design basic network systems in AWS.
* Practice creating VPC, subnet, security group, route table and test connectivity.

---

### 📌 Tasks to be carried out this week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------- | ---------------- | --------------- |
| 2 | - Review EC2 knowledge from last week <br> - Receive week 8 learning objectives from mentor | 18/08/2025 | 18/08/2025 | |
| 3 | - Learn about VPC concepts: <br> &emsp;+ CIDR <br> &emsp;+ Subnet (Public/Private) <br> &emsp;+ Internet Gateway <br> &emsp;+ NAT Gateway | 19/08/2025 | 19/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Deep dive into network security: <br> &emsp;+ Security Group <br> &emsp;+ Network ACL (NACL) <br> &emsp;+ Route Table | 20/08/2025 | 20/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - **Practice:** <br> &emsp;+ Create new VPC <br> &emsp;+ Create Public/Private Subnet <br> &emsp;+ Create and bind Internet Gateway, NAT Gateway <br> &emsp;+ Update Route Table | 21/08/2025 | 22/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 6 | - **Testing:** <br> &emsp;+ Deploy EC2 to Public Subnet and SSH from Internet <br> &emsp;+ Deploy EC2 to Private Subnet and test Internet access via NAT | 22/08/2025 | 22/08/2025 | https://cloudjourney.awsstudygroup.com/ |

---

### ✅ Week 8 Achievements:

* Understand AWS VPC network architecture including:
    * IP range – CIDR
    * Public and Private Subnet
    * Internet Gateway and NAT Gateway
    * Route Table – routing mechanism between subnets

* Apply network security knowledge:
    * Set up appropriate Security Group (SSH, HTTP, HTTPS)
    * Understand inbound/outbound rule mechanism
    * Know how to distinguish SG and NACL

* Practice building **a complete VPC environment**:
    * 01 VPC
    * 01 Public Subnet + 01 Private Subnet
    * 01 Internet Gateway + 01 NAT Gateway
    * Manually configured Route Table
    * EC2 operating stably in each subnet

* Successfully tested independently:
    * SSH into EC2 in Public Subnet
    * Private Subnet cannot SSH directly from Internet
    * Private Subnet accesses Internet via NAT Gateway

* Begin to understand "infrastructure as network topology" thinking — how cloud separates resources by network layer.

---

### 📌 Personal Notes:
Week 8 helped me understand more about network systems in AWS — the most important part when deploying backend applications. Hands-on VPC creation and connection testing helped me visualize how enterprises design secure systems, limiting risks. This is an important step to prepare for advanced content next week like Load Balancer, Auto Scaling, IAM, S3, and serverless services.
