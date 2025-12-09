---
title : "Create VPC & Subnets"
date :  2025-09-09
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Open Amazon VPC console (Note: Choose region suitable for needs, here the group uses Region ap-southeast-1)
2. Select Create VPC



![endpoint](/images/5-Workshop/5.3-S3-vpc/endpoints.png)

### Configuration:
+ Name: HRM-VPC
+ IPv4 CIDR: 10.0.0.0/16

![endpoint](/images/5-Workshop/5.3-S3-vpc/create-s3-gwe1.png)

4. Create Subnets (Split 2 AZs to ensure High Availability):
+ Public Subnets (2): 10.0.1.0/24 & 10.0.2.0/24 (Used for Load Balancer & NAT)
+ Private Subnets (2): 10.0.3.0/24 & 10.0.4.0/24 (Used for App, DB, Redis)

![endpoint](/images/5-Workshop/5.3-S3-vpc/services.png)

+ Click Create VPC and wait for state to change to Available is successful
![endpoint](/images/5-Workshop/5.3-S3-vpc/vpc.png)

