---
title: "Blog 1"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# How Novo Nordisk, Columbia University and AWS collaborated with the OpenFold AI Consortium to develop OpenFold3

by Daniele Granata, Anamaria Todor, Arthur Grabman, Rômulo Jales, Jacob Mevorach, and Stig Bøgelund Nielsen | on 03 JUN 2025 in | Customer Solution | High Performance Computing | Life Sciences Permalink

This post was contributed by Daniele Granata, Principal Modelling Scientist; Søren Moos, Architect Advisor Director; Rômulo Jales, Senior Software Engineer; Stig Bøgelund Nielsen, Senior HPC Cloud Engineer at Novo Nordisk; and Jake Mevorach, Senior Specialist, Application Modernization; Arthur Grabman, Principal Technical Account Manager; and Anamaria Todor, Principal Solutions Architect at AWS.

In this blog post, we are excited to share how Novo Nordisk, Columbia University, and AWS collaborated to develop OpenFold3, an advanced protein structure prediction model, using scalable and cost-effective bioinformatics solutions on AWS.

The OpenFold Consortium, a non-profit AI research and development group, has been at the forefront of creating open-source tools for biology and drug development. Their latest project, OpenFold3, represents a significant advancement in protein structure prediction. This post will explore how we leveraged AWS services and innovative approaches to overcome challenges in training this complex model while maintaining cost efficiency and scalability.

First, we will introduce protein structure prediction and discuss its importance in advancing medical research and pharmaceutical development.

Next, we will dive into three main challenges we faced and how we addressed them:

1. **Creating a lightweight research environment:**

We will discuss how we developed the Research Collaboration Platform (RCP) using AWS services, providing a secure, flexible, and efficient workspace for collaborative efforts.

2. **Creating Multiple Sequence Alignments (MSAs):**

We will explore how we optimized the MSA generation process using AWS Graviton processors, achieving significant improvements in both speed and cost-efficiency.

3. **AI/ML Training:**

We will present our approach to the computationally intensive task of training the OpenFold3 model, including the use of Amazon EC2 Capacity Blocks for ML and Spot Instances to efficiently manage resources.

This post provides valuable insights for researchers, data scientists, and organizations looking to leverage cloud computing for complex bioinformatics tasks. Whether you're working on protein structure prediction or other computationally intensive projects in life sciences, the strategies and solutions discussed can help you optimize workflows for both performance and cost-effectiveness.

## Protein Structure Prediction – Overview

If you're interested in curing diseases, advancing medicine, and even saving lives, a great place to start is understanding protein structures. There are many different types of proteins in the human body. Some proteins are beneficial, such as the lactase enzyme, which helps digest dairy products. Other proteins can be harmful and lead to disease or even death. Even small changes in the structure of these microscopic compounds can determine survival.

Protein-based therapeutics is a new and promising approach to treating diseases. However, because proteins have a strong impact on the human body, we need to ensure that we create proteins with the appropriate structure to treat diseases.

The traditional process of developing protein-based therapeutics typically includes:

1. Finding receptors or other structures in the body (in the industry called "targets") that you believe a protein can bind to achieve the desired therapeutic effect.
2. Developing a process to produce a protein capable of acting on this target.
3. Verifying through methods such as X-ray crystallography or Nuclear Magnetic Resonance (NMR) Spectroscopy that the production process creates a protein with the desired structure.
4. Conducting extensive research and testing, ultimately clinical trials, to ensure the newly developed protein achieves the desired effect.

Historically, the biggest problem was that while we could genetically engineer proteins with specific sequences, we lacked good tools to predict protein structure from genetic data. This made the therapeutic development process time-consuming, because even when you know how to create a protein with a specific genetic sequence, you still need many highly trained personnel operating expensive equipment—even by global pharmaceutical company standards—to verify the structure. If the structure is incorrect, you must start over and repeat the process until the correct structure is achieved. The process must be restarted each time a new molecule needs to be characterized.

All of this makes developing protein-based therapeutics extremely expensive and time-consuming. A breakthrough emerged when researchers realized that AI could be applied to significantly accelerate this process. Researchers had large amounts of historical data, combining protein structures with corresponding genetic data. The question was posed:

"If we have data combining protein structures with genetic data, can we use it to train a model that, when provided with a new genetic sequence, will predict the corresponding protein structure?"

The research community answered "yes," and from that point, many AI models emerged that take genetic sequences as input and output predicted protein structures.

Running these models requires only a small amount of electrical power but provides crucial feedback about protein behavior before starting expensive and time-consuming experiments.

## Introducing OpenFold3

The OpenFold Consortium is a non-profit AI research and development consortium, specializing in developing free open-source tools to support biology and drug discovery. One of the tools they develop is OpenFold, an AI model that takes genetic sequences as input and outputs predicted protein structures.

OpenFold3 is the latest version of this software, and to accelerate its development, Novo Nordisk and AWS collaborated with the OpenFold Consortium and Columbia University.

## First Challenge: An Optimal Research Environment

We started this project with limited time and budget, and the first challenge the project faced was that researchers needed a lightweight research environment as soon as possible.

As the priority scientific partner, Novo Nordisk needed to create an environment that could fundamentally change how research collaboration works. This environment had to enable seamless collaboration between Novo Nordisk, Columbia University, and the OpenFold Consortium, while strictly adhering to security and compliance standards.

It was important that researchers could use their preferred tools and devices, maintaining established workflows without disruption. With a tight timeline, we needed rapid setup capability and short infrastructure build times, combined with a streamlined onboarding process for external collaborators.

The system also needed to be flexible enough to meet different consortium contract terms, particularly regarding geographic data location and access controls. Most importantly, this environment had to accelerate early target discovery by providing robust computational resources while maintaining cost efficiency. Finally, an environment was needed that could dynamically scale up during intensive training phases but could scale down when not in use to maintain cost efficiency.

All of these requirements had to be balanced with empowering researchers with creative freedom—a factor particularly important when considering the high computational requirements for training large language models for protein structure prediction.

![Figure 1 – Research Collaboration Platform Architecture](/images/3-BlogsTranslated/3.1-Blog1/rcp-architecture.png)

*Figure 1 – This diagram illustrates Novo Nordisk's Research Collaboration Platform (RCP), showing how the platform creates a secure and high-performance research environment on AWS Cloud. Key elements to note are: (1) Secure access layer using Okta authentication, allowing researchers to connect securely via web portals or SSH.*

*(2) Flexible computing resources managed by AWS ParallelCluster, automatically scaling to match researcher needs. (3) AWS EFS and AWS FSx file systems handle research data. The key point is that this architecture allows scientists to conduct complex research securely and efficiently, especially for high-demand tasks like AI model training, while ensuring regulatory compliance.*

*This platform stands out by combining enterprise-grade security with the flexibility researchers need, and can be deployed in less than 2 hours through automation.*

To meet these requirements, Novo Nordisk developed the Research Collaboration Platform (RCP), a comprehensive solution built on AWS, providing a secure and flexible environment for computational research. The architecture (as illustrated in Figure 1) uses AWS ParallelCluster as the foundation, operating within a Virtual Private Cloud (VPC) to ensure secure isolation for research workloads.

At the center of the RCP security model is integration with Okta, providing robust user directory services and multi-factor authentication. Researchers can access the platform through web portals or SSH using Okta Advanced Server Access, ensuring secure and controlled access to resources. The platform entry point is an EC2 head node located in a public subnet, containing necessary research tools including RStudio, Docker containers, and scientific frameworks like Singularity.

The compute infrastructure is intelligently designed with SLURM job scheduler, managing workloads across different instance types, optimized for diverse computational needs—from general-purpose compute to memory-intensive tasks and GPU acceleration. Amazon EC2 Auto Scaling ensures resources are allocated dynamically based on demand, helping maintain cost efficiency while meeting computational requirements.

The platform uses Amazon Aurora Serverless for database management and Amazon DynamoDB for cluster state information, providing reliable and scalable data storage solutions.

Data management is handled through a combination of Amazon EFS and FSx for Lustre, providing both general-purpose and high-performance file storage. The entire infrastructure is monitored by Amazon CloudWatch, with AWS CloudFormation enabling infrastructure as code, ensuring consistent and repeatable deployments. Deploying an RCP instance takes less than 2 hours.

This architecture provides researchers with flexibility to use their preferred tools while ensuring security and compliance requirements. The combination of containerization, automated scaling, and high-performance computing capabilities enables rapid experimentation and accelerates research workflows, especially for computationally intensive tasks like training the OpenFold3 model.

## Second Challenge: The MSA Generation Process

With the research environment deployed and operational, we had to find a way to generate millions of Multiple Sequence Alignments (MSAs). A multiple sequence alignment (MSA) is essentially a unit of information describing the degree of similarity between three or more genetic sequences. This is important because these MSAs (along with other data) will be used to train OpenFold3.

The software we were tasked with scaling to generate these MSAs is HHBlits (specifically version 3, available for free on GitHub). When calculating cost and runtime for this step, we found it would take twice the time and more than double the cost compared to the expected budget and timeline if using the initial compute instances, r5.16xlarge.

Amazon EC2 R8g instances, running on the latest generation AWS Graviton4 processors, had the potential to provide better price performance for this memory-intensive workload. Benchmark testing on r8g.16xlarge showed 50% lower runtime and 55% lower cost for HHBlits compared to r5.16xlarge.

Combined with some other optimizations, the entire process ran much faster at lower cost, allowing us to generate over one million MSAs per day.

One challenge when transitioning from R5 to R8g was the difference in CPU architecture. R8g uses ARM-based CPUs, while R5 is x86-64. In terms of source code, this was not an issue because all code is C, C++, and Python, supporting both AArch64 and x86-64. However, we had to create an entirely new cluster because ParallelCluster requires head nodes and compute nodes to have the same CPU type.

We solved this by defining a new AWS ParallelCluster configuration, synchronizing CPU architecture between head nodes and compute nodes. To improve performance, we also customized the EC2 image with EBS as local storage. All generated MSAs were stored directly into an Amazon S3 bucket immediately. This configuration proved to be very efficient.

From the end-user perspective, there was very little change. Scientists only needed to use a different host name to log in and access the Graviton Slurm queues. We also reused the same EFS and FSx for Lustre filesystems configured initially without performance penalties.

The best aspect of this architecture is its high scalability. If we want to adjust the number of MSAs generated simultaneously, we simply add or remove nodes. The figure of one million MSAs mentioned above is not the maximum limit; in fact, we don't yet know the upper limit for throughput when using this method.

## Third Challenge: AI/ML Training

As is known, the key factor for running AI/ML workloads is GPUs. But for protein prediction, GPUs alone are not enough. The system must also meet a range of prerequisites including petabyte-scale storage, high-speed networking, and hundreds of GPUs.

The OpenFold3 project required up to 256 GPUs for training. This equates to 32 p5en.48xlarge EC2 instances running in parallel, processing and generating data over several weeks, 24 hours a day.

This configuration requires infrastructure that is scalable, resilient, observable, and most importantly, flexible.

Initially, scientists needed to build training logic almost from scratch. The level of uncertainty was high; therefore, 32 GPU machines were not necessary from the start. What they needed was an environment to develop, conduct small experiments, and gradually scale up the number of nodes used for training. This is exactly the infrastructure capability that AWS ParallelCluster can provide.

When training OpenFold3 on the generated data, we faced three main challenges:

1. Budget constraints required a cost-effective solution, avoiding costly errors.
2. Increasing model complexity required distributed AI/ML training infrastructure, as the model no longer fits on a single GPU or instance.
3. Securing large GPU resources for both training and preliminary testing required quick action and coordination among all stakeholders.

To address these challenges, we implemented the following solutions:

### GPU Resource Allocation:

We used Amazon EC2 Capacity Blocks for ML, a consumption model that allows reservation of high-performance GPUs for short periods for machine learning workloads. This service allows users to reserve hundreds of NVIDIA GPUs synchronously in Amazon EC2 UltraClusters, specifying cluster size, future start date, and duration. Capacity Blocks provide reliable, predictable GPU access without long-term commitments, ideal for training and fine-tuning ML models, running experiments, and preparing for future scaling needs. Pricing is dynamic, based on supply and demand, typically around P5 On-Demand rates.

### Cost Optimization:

To optimize costs, we used Amazon EC2 Spot instances. Spot instances allow use of spare EC2 capacity with discounts up to 90% compared to On-Demand pricing for fault-tolerant and flexible workloads. As AWS GPU infrastructure expands, many new GPU instance types are available through the Spot pricing model, enabling large-scale workloads to run at significantly lower costs thanks to AWS's massive operational scale. Spot instances can be reclaimed by AWS when EC2 needs resources, typically with a two-minute interruption notice, helping applications handle transitions gracefully.

By implementing these methods, Novo Nordisk's RCP team achieved an 85% cost reduction compared to using On-Demand instances for our use case.

### Distributed Training Infrastructure:

We drew inspiration from AWSome Distributed Training examples, particularly those about DeepSpeed, to build our scalable training infrastructure. This configuration efficiently uses GPUs from multiple instances simultaneously, creating a highly effective and scalable system. The final infrastructure allows Slurm job submissions to an autoscaling, efficient, distributed GPU cluster, capable of handling the complex task of training the new OpenFold3 model.

By implementing these solutions, we successfully overcame the initial challenges, creating a cost-effective, scalable, and efficient training environment for OpenFold3.

## Conclusion

The successful training of OpenFold3 represents an important collaboration between Novo Nordisk, Columbia University, and AWS, overcoming three major challenges through innovative solutions:

1. Novo Nordisk developed the Research Collaboration Platform (RCP), a secure and flexible research environment built on AWS, enabling seamless collaboration while maintaining strict security standards. The platform can be deployed in less than two hours and provides dynamic scaling for computational resources.

2. Together, we optimized the Multiple Sequence Alignment (MSA) generation process using AWS Graviton processors, achieving a 50% reduction in runtime and a 55% cost reduction. This enabled researchers to generate over one million MSAs per day with a highly scalable architecture.

3. Together, we addressed AI/ML training challenges by combining Amazon EC2 Capacity Blocks for ML and Spot instances. Our distributed training infrastructure efficiently utilized 256 GPUs across 32 p5en.48xlarge EC2 machines.

These solutions not only made the project cost-effective and efficient but also created a blueprint for future large-scale bioinformatics collaborations between Novo Nordisk and AWS.

The successful development of OpenFold3 advances the field of protein structure prediction, contributing to faster and more efficient therapeutic development processes.

**TAGS:** Protein Folding

---

## About the Authors

![Daniele Granata](/images/3-BlogsTranslated/3.1-Blog1/Daniele-Granata.png)

### Daniele Granata

Daniele Granata is a Principal Modelling Scientist in the In Silico Protein Discovery field at Novo Nordisk. He is passionate about building connections between people and technology and working at the intersection of protein science, AI, and HPC. He holds a bachelor's and master's degree in Physics, a PhD in "Physics and Chemistry of Biological Systems," and 5 years of postdoctoral experience in the US and Denmark.

Over the past 6 years at Novo Nordisk, Daniele has been prominent in advancing data science and generative AI to design new drugs for patients, while maintaining a passion for open sourcing for the scientific community.

![Anamaria Todor](/images/3-BlogsTranslated/3.1-Blog1/Anamaria-Todor.png)

### Anamaria Todor

Anamaria Todor is a Principal Solutions Architect based in Copenhagen, Denmark. She first encountered computers at the age of 4 and has never left computer science, video games, and engineering since. She has worked in various technical roles, from freelancer, full-stack developer, data engineer, technical lead, to CTO, at various companies in Denmark, focusing on the gaming and advertising industries. She has worked at AWS for over 5 years as a Principal Solutions Architect, primarily focusing on life sciences and AI/ML. Anamaria holds a bachelor's degree in Applied Engineering and Computer Science, a master's in Computer Science, and over 10 years of experience with AWS. When not working or playing video games, she coaches girls and women professionals to help them understand and find their path in the technology field.

![Arthur Grabman](/images/3-BlogsTranslated/3.1-Blog1/Arthur-Grabman.png)

### Arthur Grabman

Arthur Grabman is a Principal Technical Account Manager with over 15 years of experience, specializing in HPC for enterprise clients. He is passionate about translating complex technical concepts into concrete business value. Outside of work, Arthur enjoys traveling with his family.

![Rômulo Jales](/images/3-BlogsTranslated/3.1-Blog1/Romulo-Jales.png)

### Rômulo Jales

Rômulo Jales is a Senior Software Engineer at Novo Nordisk. With a solid foundation of over 15 years in software development, Rômulo enjoys completing work by creating simple and elegant solutions, even in critical mission applications.

Rômulo holds a bachelor's degree in Computer Engineering and is still driven by the initial curiosity about how and why things work the way they do. For him, there is always something new to learn and discover.

![Jacob Mevorach](/images/3-BlogsTranslated/3.1-Blog1/Jacob-Mevorach.png)

### Jacob Mevorach

Jacob Mevorach is a Senior Specialist in containers for healthcare and life sciences at AWS. Jacob has a background in bioinformatics and machine learning. Before joining AWS, Jacob focused on supporting and implementing large-scale analyses for genomics and other scientific fields.

![Stig Bøgelund Nielsen](/images/3-BlogsTranslated/3.1-Blog1/Stig-Bogelund-Nielsen.png)

### Stig Bøgelund Nielsen

Stig Bøgelund Nielsen is a Senior Cloud & HPC Engineer at Novo Nordisk. A lifelong engagement with IT, starting from supporting local businesses, has shaped his career. Over 8 years at IBM provided a solid foundation, with roles such as IT Service and Infrastructure Architect, Team Lead, and Transition & Transformation Architect/Project Manager, all focused on creating business value through effective IT strategies and implementations.

He joined Novo Nordisk nearly two years ago as a Cloud & HPC Engineer and is currently the Tech Lead of the Research Collaboration Platform (RCP), directly supporting pioneering research, including the OpenFold project. He is passionate about leveraging technology to drive research and innovation.
