---
title : "Configure Internet & NAT Gateway"
date :  2025-09-09 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

### Create Internet Gateway
1. In the VPC dashboard click on Internet gateways

![Create bucket](/images/5-Workshop/5.3-S3-vpc/create-bucket.png)

2. Then click on Create internet gateway
![Bucket name](/images/5-Workshop/5.3-S3-vpc/bucket-name.png)

3. In the Internet gateway creation section, name it as desired then click on Create internet gateway and wait for it to be created

![Create](/images/5-Workshop/5.3-S3-vpc/create-button.png) 

4. After the Internet gateway is finished creating go to Actions and click on Attach to VPC to attach it to the VPC created in the previous section

![Success](/images/5-Workshop/5.3-S3-vpc/bucket-success.png)


![system manager](/images/5-Workshop/5.3-S3-vpc/sm.png)

### Create NAT Gateway
+ Create NAT Gateway placed in Public Subnet 1
+ Assign Elastic IP to have a static address to the Internet

![system manager](/images/5-Workshop/5.3-S3-vpc/sm1.png)


![Start session](/images/5-Workshop/5.3-S3-vpc/start-session.png)












