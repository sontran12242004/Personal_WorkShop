---
title: "Application Deployment"
date: 2025-09-09
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

### Overview
* After the network and database infrastructure, the next step is to release the Java Spring Boot application source code (HR Management System) to the Cloud environment.
* Because I have to manage the work on the EC2 server, I use the PaaS platform - AWS Elastic Beanstalk to automate the process of developing declarations and operations.
* The goal of this module:
1. Containerization
* Packaging the HR Management System application (Java Spring Boot) into a Docker Container to ensure a unified running environment between Dev/Test/Prod (Dev = Prod), limiting configuration errors and making it easy to develop declarations.
2. Deployment
* Deploying Docker Container to AWS Elastic Beanstalk. Elastic Beanstalk will automatically:
* Provision EC2 Instance Levels and Configuration
* Set up Application Load Balancing
* Create Auto Scaling Groups to automatically scale under load
* Manage application lifecycles without manual intervention
![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy.png)