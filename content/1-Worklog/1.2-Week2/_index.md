---
title: "Week 2 Worklog"
date: 2025-09-09
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}

### Week 2 Objectives:

- Understand **Amazon VPC** architecture and how network components work in AWS.
- Master the concepts of **Subnet**, **Route Table**, **Security Group**, **NACL**, **VPC Endpoint**, **Peering**, and **Load Balancer**.
- Know how to choose appropriate connectivity solutions between **on-premises** and **AWS Cloud** environments.

### Tasks to be carried out this week:

| Day | Task                                                                                                                            | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn concepts & structure of **Amazon VPC**, account limits, CIDR IPv4/IPv6. <br> - Configure basic VPC via Console.          | 18/08/2025 | 18/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn about **Subnet**, **Availability Zone**, IP addresses. <br> - Create public/private subnets and assign corresponding route tables.               | 19/08/2025 | 19/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn about **Route Table**, **Internet Gateway**, **NAT Gateway**, **VPC Endpoint**. <br> - Distinguish interface & gateway endpoints.  | 20/08/2025 | 20/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Event:** Attend **Vietnam Cloud Day 2025 – Track 1: GenAI and Data**. <br> - Overview of AI applications in cloud infrastructure. | 21/08/2025 | 21/08/2025      |                                           |
| 6   | - Study **Security Group**, **Network ACL**, **VPC Flow Logs**. <br> - Practice logging & access control between subnets.      | 22/08/2025 | 22/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 7   | - Learn about **VPC Peering**, **Transit Gateway**, **VPN**, **Direct Connect**, **Elastic Load Balancer** (ALB, NLB, CLB, GLB).        | 23/08/2025 | 23/08/2025      | <https://cloudjourney.awsstudygroup.com/> |

---

### Week 2 Achievements:

- Understand **VPC (Virtual Private Cloud)** is a virtual network environment in AWS that helps separate and manage resources by environment (Production/Dev/Test/Staging).

    - Default limit: 5 VPCs per AWS account.
    - Each VPC requires an IPv4 CIDR block (required) and can add IPv6 (optional).

- **Subnet**:

    - Each subnet is located in a specific **Availability Zone**.
    - When creating a subnet, must specify CIDR within the VPC's CIDR range.
    - AWS reserves the first 5 IP addresses in each subnet for internal use.

- **Route Table**:

    - Determines routing paths in the network.
    - By default, there is one _default route table_ (cannot be deleted) that allows subnets within the VPC to communicate internally.

- **Elastic Network Interface (ENI)** & **Elastic IP**:

    - ENI is a virtual network card that can be attached to EC2 and moved between instances.
    - Elastic IP is a static public IPv4 address that can be attached to ENI.

- **VPC Endpoint**:

    - Connects resources within VPC to AWS services without Internet.
    - Two types:
        - Interface Endpoint – uses ENI + private IP.
        - Gateway Endpoint – uses route table (supports S3 and DynamoDB).

- **Internet Gateway** & **NAT Gateway**:

    - Internet Gateway: allows EC2 in public subnets to access Internet, managed by AWS, no autoscale configuration needed.
    - NAT Gateway: allows private subnets to access Internet (outbound only).

- **Security Group (SG)** and **Network ACL (NACL)**:

    - SG is a _stateful_ firewall applied to ENI, only has "allow" rules.
    - NACL is a _stateless_ firewall applied to subnet, has "allow/deny" rules and reads from top to bottom.
    - Default: SG blocks inbound, allows outbound; NACL allows all.

- **VPC Flow Logs**:

    - Records IP traffic to/from VPC, subnet, or ENI.
    - Stored in **CloudWatch Logs** or **S3**, does not record packet content.

- **VPC Peering** & **Transit Gateway**:

    - VPC Peering: direct connection between 2 VPCs (1:1), no transitive routing support, CIDR cannot overlap.
    - Transit Gateway: hub center connecting multiple VPCs or on-premises networks.

- **VPN Site-to-Site** & **Client-to-Site**:

    - Site-to-Site: connects data center to AWS via **Virtual Private Gateway** and **Customer Gateway**.
    - Client-to-Site: allows 1 host to access resources within VPC (VPN client).

- **AWS Direct Connect**:

    - Creates physical connection from data center to AWS (via Direct Connect Partner in VN).
    - Low latency (~20–30ms), bandwidth can be adjusted.

- **Elastic Load Balancing (ELB)**:

    - Distributes traffic to multiple EC2/Container.
    - Four types:
        - **Application Load Balancer (ALB)** – Layer 7, supports path-based routing.
        - **Network Load Balancer (NLB)** – Layer 4, high performance, supports static IP.
        - **Classic Load Balancer (CLB)** – Layer 4 & 7, legacy, rarely used.
        - **Gateway Load Balancer (GLB)** – Layer 3, uses GENEVE protocol (port 6081).
    - Supports **Sticky Session** and **Access Logs** (stored in S3).

- **Attended Vietnam Cloud Day 2025 - Track 1: GenAI and Data** event, which helped better understand how **AWS combines artificial intelligence and big data** to optimize infrastructure, accelerate data analysis, and enhance customer experience in cloud-native systems.

- Understand how to separate environments, secure resources, and route in AWS VPC.
- Master hybrid connectivity models (VPN, Direct Connect) and AWS network scalability and security capabilities.
