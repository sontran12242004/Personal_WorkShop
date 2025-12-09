---
title : "Set up Security Groups"
date : 2025-09-09
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

1. Access EC2 > Security Groups > Create security group

![route 53 diagram](/images/5-Workshop/5.4-S3-onprem/route53.png)

![Create stack](/images/5-Workshop/5.4-S3-onprem/create-stack.png)
### Group 1: Web Server (sg-web-app)
* Description: Allow HTTP from Internet
* Inbound Rules:
* Type: HTTP (80) | Source: 0.0.0.0/0 (Or only from Load Balancer if wanting stricter security)


![Button](/images/5-Workshop/5.4-S3-onprem/create-stack-button.png)

### Group 2: Database (sg-db-sql)
* Description: Allow access only from Web Server
* Inbound Rules:
* Type: MSSQL (1433) | Source: Custom > Select ID of sg-web-app

![ec2 id](/images/5-Workshop/5.4-S3-onprem/ec2-onprem-id.png)

### Group 3: Redis Cache (sg-redis-cache)
* Description: Allow access only from Web Server
* Inbound Rules:
* Type: Custom TCP (6379) | Source: Custom > Select ID of sg-web-app

![add route](/images/5-Workshop/5.4-S3-onprem/add-route.png)





