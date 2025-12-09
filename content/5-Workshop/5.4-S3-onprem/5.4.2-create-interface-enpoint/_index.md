---
title : "Initialize Amazon RDS"
date : 2025-09-09
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

1. Access RDS Console > Subnet groups > Create DB subnet group
Name: db-private-group
Subnets: Select 2 AZs and select exactly 2 Private Subnets

![name](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint1.png)
![service](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint2.png)


![vpc](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint3.png)

2. Go to Databases > Create database

![subnets](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint4.png)

3. Engine options: Microsoft SQL Server (Express Edition)

![sg](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint5.png)

4. Templates: Free tier
5. Settings: Set Master Password (remember for later use)

![success](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint-success.png)

6. Connectivity:
* VPC: VPC you created for Web
* Subnet group: db-private-group
* Public access: No
* VPC security group: Select Security group you created for database

![sg](/images/5-Workshop/5.4-S3-onprem/s3-interface-endpoint6.png)

7. Click Create database