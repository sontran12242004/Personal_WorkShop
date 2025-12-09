---
title : "Package with Docker"
date :  2025-09-09
weight : 5
chapter : false
pre : " <b> 5.5.1 </b> "
---

1. Before moving to Cloud, we need to package the .NET Core application into a Docker Image

    * Create Dockerfile: At the root directory of the Solution, create a file named Dockerfile (no extension)

![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy2.png)
![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy12.png)
2. Create buildspec.yml: Create file buildspec.yml to instruct AWS CodeBuild how to package and push to ECR

![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy3.png)

![endpoint diagram](/images/5-Workshop/5.5-Policy/s3-bucket-policy4.png)



