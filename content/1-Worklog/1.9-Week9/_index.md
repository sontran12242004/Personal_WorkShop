---
title: "Week 9 Worklog"
date: 2025-09-16
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---


### 🎯 Week 9 Objectives:

* Understand network architecture in AWS more deeply (VPC, Subnet, Routing).
* Familiarize with standard infrastructure design for Spring Boot backend projects.
* Practice building infrastructure platform for Human Resource Management system (HRM Project).

---

### 📌 Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------- | ----------- | --------------- |
| 2 | - Meeting with mentor / trainer to review knowledge learned in week 7. <br> - Receive week 8 roadmap (VPC – Network – Security). | 25/08/2025 | 25/08/2025 | |
| 3 | - Learn about AWS VPC: <br> &emsp;+ CIDR, IP Addressing <br> &emsp;+ Public / Private Subnet <br> &emsp;+ Route Table <br> &emsp;+ Internet Gateway | 26/08/2025 | 26/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 4 | - Study AWS network security mechanisms: <br> &emsp;+ Security Group <br> &emsp;+ Network ACL <br> &emsp;+ Differences SG vs NACL | 27/08/2025 | 27/08/2025 | https://cloudjourney.awsstudygroup.com/ |
| 5 | - **Practice:** <br> &emsp;+ Create new VPC for HRM project <br> &emsp;+ Create Public Subnet + Private Subnet <br> &emsp;+ Attach Internet Gateway to Public Subnet | 28/08/2025 | 29/08/2025 | AWS Console / AWS Docs |
| 6 | - **Advanced practice:** <br> &emsp;+ Create NAT Gateway for Private Subnet <br> &emsp;+ Create test EC2 in each subnet <br> &emsp;+ Test SSH, outbound Internet and routing | 29/08/2025 | 29/08/2025 | AWS Console / CLI |

---

### ✅ Week 9 Achievements:

* Understand AWS VPC network structure and role of each component:
    * CIDR → manage IP range.
    * Public subnet → for resources needing internet (jump server).
    * Private subnet → deploy backend, database.
    * Route Table → route traffic.
    * Internet Gateway & NAT Gateway → manage inbound/outbound traffic.

* Distinguish and use:
    * **Security Group** (stateful inbound/outbound rules).
    * **Network ACL** (stateless firewall layer deeper than SG).

* Successfully built a standard VPC environment for the project:
    * 01 dedicated VPC.
    * 02 subnets (Public / Private).
    * 01 Internet Gateway + 01 NAT Gateway.
    * 02 test EC2 instances → functioning correctly in each subnet.

* Successfully tested:
    * EC2 Public Subnet → direct SSH.
    * EC2 Private Subnet → cannot SSH from internet (correct design).
    * Private Subnet can access Internet via NAT.

* Begin to understand how AWS networking serves Spring Boot HRM application deployment later:
    * Backend runs in Private Subnet → higher security.
    * Only public Load Balancer is accessible to users.
    * Database completely private.

---

### 📌 Personal Notes:
Week 9 helped me understand network layer more deeply — the most important part when building enterprise applications on AWS platform. Hands-on VPC configuration and connection testing helped me understand "secure-by-design" thinking and how AWS technical teams deploy real systems. This is the foundation to move to next week
