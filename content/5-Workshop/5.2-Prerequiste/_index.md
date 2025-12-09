---
title : "Prerequiste"
date :  2025-09-09
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions
+ Attach AdministratorAccess in IAM permission policy to the AWS account for easier workflow
+ Note: Using Administrator privileges is recommended only for the Workshop environment to ensure the deployment process is uninterrupted. In a real Production environment, adhere to the Least Privilege principle for each service
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}

```

#### Source Code

* GitHub repository containing Java code and a valid Dockerfile
![Source Code](/images/5-Workshop/5.2-Prerequisite/Source_Code.png)


