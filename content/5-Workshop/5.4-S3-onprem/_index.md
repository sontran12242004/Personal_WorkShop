---
title : "Data Layer Deployment"
date : 2025-09-09
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

+ Data is the most important asset of every system. Therefore we will set up the data layer (Data Layer) for MiniMarket with criteria: Maximum Security and High Performance

+ We will deploy two core services:

* Amazon RDS (Relational Database Service): Use SQL Server to store business data (Products, Orders, Users). Database will be placed in Private Subnet to prevent direct access from the Internet
* Amazon ElastiCache (Redis): Use Redis as cache memory (In-memory Cache) to store Login Sessions and offload queries for the main Database

![Interface endpoint architecture](/images/5-Workshop/5.4-S3-onprem/diagram3.png)



