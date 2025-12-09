---
title : "Initialize Elastic Beanstalk"
date :  2025-09-09
weight : 5
chapter : false
pre : " <b> 5.5.2 </b> "
---

1. We will create an environment to run the application

 * Access Elastic Beanstalk > Create application


![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy5.png))

* App Name: MiniMarket-App
* Platform: Docker (Amazon Linux 2023)
* Application code: Select Sample application (To test infrastructure first)
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy6.png))
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy7.png))

2. Network Configuration (Networking) - Extremely Important:
* VPC: Select the VPC you created for MiniMarket
* Instance settings:
* Public IP address: Uncheck
* Subnets: Select 2 Private Subnets
* EC2 security groups: Select sg-web-app
 ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy8.png))
 ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy9.png))

* Capacity:
 * Environment type: Select Load balanced
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy10.png))
* Load balancer network settings:
  * Visibility: Public
  * Subnets: Select 2 Public Subnets
  ![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy11.png))
3. Click Create environment and wait for environment to be ready

