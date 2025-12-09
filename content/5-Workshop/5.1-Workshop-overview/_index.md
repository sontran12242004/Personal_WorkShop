---
title : "Introduction"
date :  2025-09-09 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Introduction
+ HumanResource is a Human Resource Management System built on the .NET Core platform, applying a modern 3-Tier Architecture.

+ The goal of this Workshop is to re-platform the application from an On-premise environment to AWS Cloud infrastructure (Cloud Native Migration) while meeting the key principles of the AWS Well-Architected Framework:
Security, Reliability, Performance Efficiency, and Cost Optimization.
#### Workshop overview
Solution Architecture:

+ Compute: Use AWS Elastic Beanstalk (Docker platform) to deploy the HRM application, simplify infrastructure management, and support Auto Scaling as employee data or system load increases.

+ Database: Utilize Amazon RDS for SQL Server in a Private Subnet to securely store HR information such as employee profiles, contracts, attendance records, and payroll data.

+ Caching: Amazon ElastiCache (Redis) is used to store user sessions and cache frequently accessed HR data (employee lists, department structures), improving system response time.

Network & Security:

+ VPC: Designed with a Public/Private Subnet model combined with a NAT Gateway, ensuring a secure and isolated HRM environment.

+ Application Layer Security: Implement AWS WAF combined with Amazon CloudFront to protect against web attacks and accelerate content delivery for employees across different regions.

Storage:

+ Amazon S3 is used to store HR documents such as employment contracts, employee records, and training materials with high durability.

DevOps:

+ A fully automated CI/CD pipeline using AWS CodePipeline and CodeBuild, enabling rapid deployment of HRM features such as payroll processing, attendance management, and leave approval workflows.

Monitoring:

+ Amazon CloudWatch monitors system health (CPU, Network) and sends alerts to ensure the HRM system remains stable and highly available.

![overview](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)
