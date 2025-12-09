---
title: "Blog 3"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---


# Modernizing real-time payment orchestration on AWS

by Neeraj Kaushik, Subhash Sharma, and Venkat Gomatham | on 01 OCT 2025 in | Amazon API Gateway | Amazon CloudFront | Amazon Elastic Container Service | Amazon Elastic Kubernetes Service | Amazon Managed Streaming for Apache Kafka (Amazon MSK) | Architecture | AWS Lambda | AWS Partner Network | AWS Solutions Implementations | Financial Services | Partner solutions | Public Sector | Public Sector Partners | Regions Permalink

The global real-time payment market is experiencing significant growth. According to Fortune Business Insights, the market was valued at $24.91 billion in 2024 and is expected to grow to $284.49 billion by 2032, with a CAGR of 35.4%. Similarly, Grand View Research reports that the global mobile payment market, valued at $88.50 billion in 2024, is expected to grow with a CAGR of 38.0% from 2025 to 2030.

(Note: Third-party research and statistics are provided for reference purposes only. AWS and IBM do not guarantee the accuracy of this information.)

This rapid expansion highlights the urgency for financial institutions to modernize their payment processing infrastructure. Financial institutions often need to handle large transaction volumes with near-zero latency to meet strict Service Level Agreements (SLAs), while supporting increasing mobile payment volumes.

However, traditional payment orchestration systems, often built on monolithic architectures, struggle to meet these requirements due to latency, availability, and scalability challenges. Furthermore, reliance on on-premises infrastructure leads to high costs and limits innovation, highlighting the need for modernization.

As sustainability becomes a priority, organizations are moving to cloud-based solutions to optimize infrastructure, reduce carbon footprints, and enhance energy efficiency. This shift not only provides scalability and performance but also aligns with global sustainability goals, ensuring a future for real-time payments.

In this post, we discuss a real-time payment orchestration framework. This framework uses event-driven architecture and AWS serverless services to enhance the resiliency, efficiency, and scalability of real-time payments. By decomposing payment processing into distinct business capabilities, financial institutions can improve modularity and flexibility. Implementing tenant-based segregation helps with data isolation and security. Additionally, applying asynchronous communication through Amazon Managed Streaming for Apache Kafka (Amazon MSK) enhances scalability and resilience.

## Traditional real-time payment orchestration

Payment orchestration serves as a middleware solution, helping streamline transaction processing across multiple payment methods, gateways, and financial institutions. It orchestrates key business functions such as: Payment authorization, Payment processing, Settlement and clearing, Compliance and risk management, Account management, for both inbound and outbound payment flows.

The diagram below illustrates the high-level business capabilities supported by payment orchestrators across different payment flows, including real-time payments, digital disbursements, tax payments, wires, and many others.

![Payment Processing System Diagram](/images/3-BlogsTranslated/3.3-Blog3/payment-processing-diagram.png)

*A detailed diagram describing the payment processing system with multiple components. The diagram displays primary payment types at the top (including Realtime Payments, Digital Disbursement, Credit Transfer, and Peer to Peer Payments) flowing down to core processing stages including Payment Acceptance, Execution, Clearing, Reporting, Tracking, Reversals, and Billing.*

Many financial institutions adopt a tenant-based approach organized by geography due to differences in clearing processes, localized regulations, and transaction requirements between AWS Regions. However, if services are not properly separated, teams often continue adding region-specific logic to existing services, gradually increasing monolithic complexity and using the same infrastructure for all payment flows.

Traditional payment systems process transactions linearly, with each step waiting for the previous one to complete. However, analyzing payment workflows reveals many opportunities for parallel execution:

- **Sanctions screening and fraud detection** – Compliance and fraud checks can run concurrently with initial routing decisions, rather than blocking all subsequent processing steps
- **Payment routing and authorization requests** – When basic validations complete, routing and authorization can proceed in parallel, not sequentially
- **Payment execution and ledger updates** – Actual payment execution doesn't need to wait for ledger records to be updated—they can occur simultaneously
- **Settlement, reconciliation, and tracking** – Post-transaction processes can start independently as soon as the primary transaction completes

This parallel approach can significantly improve throughput and reduce latency compared to traditional queue-based systems, where operations form a sequential chain, extending processing time and causing bottlenecks.

Most legacy payment orchestration systems heavily depend on on-premises virtual machines (VMs), leading to several challenges:

- **Multi-Region support** for disaster recovery and multi-tenancy leads to large capital expenditure and operational overhead
- **High latency and SLA issues** due to sequential message processing and delays between globally separated data centers
- **Limited reusability of payment flows** because monolithic architectures require region-specific changes for local clearing mechanisms and regulations, increasing complexity and costs
- **Scalability challenges and high memory consumption** due to inefficient resource utilization and executing unnecessary logic in multiple regions
- **Complex cross-border payment routing** due to differences in clearing rules, transaction limits, and local regulations, increasing latency and routing errors
- **Integration challenges** with different data formats because legacy systems rely on proprietary standards (e.g., ISO 20022, SWIFT MT), complicating data conversion and compliance
- **High deployment complexity** for new payment flows because monolithic architectures require many region-specific modifications, slowing time to market
- **Environmental impact and high carbon footprint** from on-premises infrastructure consuming significant energy, while cloud-based approaches improve efficiency

## Solution overview

To overcome these challenges, the proposed architecture applies the following design principles to build a future-ready real-time payment orchestration solution:

- **Performance at scale** – Process over 1,000 transactions per second (TPS) with stable low latency under various load conditions.
- **High availability** – Achieve 99.999% uptime to meet strict financial transaction requirements.
- **Geographic resilience** – Support global operations with region-specific compliance while maintaining stable performance.
- **Cost optimization** – Reduce total cost of ownership (TCO) through efficient resource utilization and serverless technology.
- **Security and compliance** – Support data protection and regulatory compliance requirements across different regions.
- **Operational simplicity** – Simplify deployment, monitoring, and maintenance across the entire payment ecosystem.

**Microservices** – Separate payment processing into distinct business capabilities, helping financial institutions increase modularity and flexibility. This microservices-based architecture allows independent scaling and development of critical components.

The diagram below illustrates the solution architecture (high-level solution architecture) for real-time payments. Existing channels using synchronous or asynchronous APIs can be adapted to use edge-optimized endpoints, helping reduce latency.

![High-Level Solution Architecture for Real-Time Payments](/images/3-BlogsTranslated/3.3-Blog3/solution-architecture.png)

*A detailed architecture diagram of an AWS-based payment orchestration platform using event-driven principles. The platform has reusable components deployed across two regions, with specialized modules for payment initiation, execution, reconciliation, billing, and risk management. This architecture uses pub/sub messaging patterns for inter-component communication and connects with enterprise systems including accounting, compliance, and analytics.*

An event-driven architecture is used for payment orchestration, managing communication through a pub/sub pattern. This architecture maintains persistent connections, improving the performance of the entire real-time payment processing workflow.

Event-driven architecture for real-time payment processing allows multiple payment operations to occur simultaneously through different adapters, as opposed to traditional systems where payment processes are executed sequentially through a single pipeline. Payment events are distributed to specialized payment processor microservices based on their functions (initiation, execution, tracking, settlements), allowing each component to process independently without waiting for other components to complete.

Because we're moving from sequential processing to distributed processing, maintaining transaction traceability is extremely important. Payment tracking adapters in the diagram connect to enterprise analytics systems, creating a specialized layer to monitor transactions. The pub/sub model allows attaching correlation IDs to events, helping systems track related events across topics and different processing stages.

A standardized event schema is the foundation of this architecture, ensuring consistency across regional deployments while allowing customization at the adapter level. This schema defines uniform event structures containing tenant-specific metadata and supports versioning to meet future development requirements. By isolating region-specific variations into the adapter layer, the solution maintains core functionality while connecting with diverse enterprise systems through configuration-driven customization rather than code changes.

For most payment processes, especially independent processing steps that can run in parallel, this architecture delivers net performance gains despite topic switching overhead, particularly for complex transactions requiring multiple independent validations or concurrent processing steps.

## Implementation on AWS Cloud

The solution uses edge-optimized Amazon API Gateway for channels. An edge-optimized API endpoint routes requests to the nearest Amazon CloudFront Point of Presence (POP), which is useful when your customers are geographically distributed, helping efficient routing within each geographical region, improving global responsiveness by minimizing network round trips and ensuring requests follow the shortest possible path before transitioning from the public internet into the client network.

The following diagram illustrates the high-level solution architecture for real-time payments.

![Comprehensive AWS Payment Orchestration Solution](/images/3-BlogsTranslated/3.3-Blog3/aws-payment-orchestration.png)

*This comprehensive AWS Payment Orchestration solution implements modern cloud-native architecture principles. Core processing logic is executed as Lambda functions, including workflows such as initiation, execution, reconciliation, billing, tracking, risk management, and settlement. The solution leverages Amazon MSK for reliable event streaming between components, with separate Kafka topics for each processing stage. Data persistence is handled by Amazon DynamoDB, supporting cross-region operations. This architecture demonstrates AWS best practices for financial services, including regional redundancy, serverless computing, managed services, and event-driven design patterns. The system integrates with external banking infrastructure and enterprise systems while maintaining separation of concerns through microservices architecture. The solution has built-in support for compliance monitoring, risk management, and payment tracking through specialized Lambda functions.*

The solution uses Amazon MSK to implement an event-driven architecture, efficiently handling both inbound and outbound channel traffic through API requests and asynchronous message-based events. Amazon MSK communicates using a high-performance binary protocol between producers, consumers, and brokers, ensuring low latency and high throughput. Real-time payments are logically partitioned for multiple tenants across geographical regions—North America, EMEA, LATAM, and Asia-Pacific.

Each real-time payment tenant follows an active/active disaster recovery strategy by deploying MSK clusters across multiple AWS Regions, designed to achieve high availability and resilience. Amazon MSK provides both serverless and provisioned cluster options. Technical teams can choose either based on non-functional requirements and team expertise. Amazon MSK automatically manages partition leadership with leaders in primary Regions and followers in secondary Regions. During failover, leaders are re-elected in healthy Regions, helping maintain processing capability during regional incidents. Sticky partitioning uses consistent hashing for deterministic routing, and cooperative rebalancing helps efficient failover. Multi-AZ deployment provides zone redundancy and isolated clusters per Region to ensure data sovereignty compliance through programmatic AWS Identity and Access Management (IAM) and Virtual Private Cloud (VPC) boundaries.

To support seamless cross-Region replication and maintain message continuity, Amazon MSK Replicator—a fully managed feature of Amazon MSK—is used to replicate topics and synchronize consumer group offsets between clusters. MSK Replicator simplifies building multi-Region Kafka applications without custom code, open-source tool configuration, or infrastructure management. This service automatically provisions and scales necessary resources, allowing teams to focus on business logic while only paying for data being replicated. In the event of a regional outage or failover, traffic can be automatically redirected to healthy Regions without data loss or service interruption, delivering near-zero Recovery Time Objectives (RTOs) and uninterrupted operation for downstream services such as payment processors and audit trail consumers.

Beyond regional redundancy, this architecture uses an event-driven architecture to enable parallel and decoupled processing of payment transactions. Events such as transaction initiation, validation, and settlement are published asynchronously and consumed by independent microservices, helping drastically reduce end-to-end latency.

To handle events at scale, the architecture can use AWS Lambda, Amazon Elastic Container Service (Amazon ECS), or Amazon Elastic Kubernetes Service (Amazon EKS) depending on non-functional requirements. Automatic scaling responds to Amazon CloudWatch metrics, and exponential backoff retry logic combined with dead-letter queues (DLQs) handles throttling scenarios. Circuit breakers are used to prevent cascade failures when high error rates occur.

One of the main benefits of the solution is the reusability of payment flows across different regions. Although each region has its own compliance requirements and settlement rules, the core functionalities of real-time payments (such as payment authorization, payment processing, settlement, and clearing) are largely similar. This allows rapid deployment of payment solutions in new regions without needing to rearchitect the entire system.

For example, real-time payment systems in the US and UK can share similar business logic for real-time gross settlement, but differ in clearing and compliance requirements. This solution views regions as bounded contexts in microservices architecture, providing flexibility while ensuring each region can handle its own rules and regulations.

## Sustainability

AWS continuously innovates in designing, building, and operating infrastructure to move toward net-zero carbon by 2040 and become water positive by 2030. Amazon MSK with AWS Graviton-based instances uses up to 60% less energy than equivalent M5 instances, helping you achieve sustainability goals.

Lambda is inherently sustainable by design. Its serverless model ensures compute resources are only used when needed, significantly reducing idle infrastructure and wasted energy. Instead of maintaining always-on servers for infrequent tasks, Lambda provisions compute power just-in-time, achieving near-zero idle capacity.

## Security and compliance in financial services

Due to the sensitive nature of payment transactions and financial data, you should apply necessary security controls to meet financial regulations such as AWS PCI DSS and AWS Federal Information Processing Standard (FIPS) 140-3 as needed by your organization.

The solution should include multi-layered security controls, continuous monitoring, and automated compliance auditing to meet strict requirements from banking regulators and internal risk teams. For more information, refer to Security Guidance.

## Conclusion

Modernizing payment orchestration systems using event-driven architecture and AWS serverless technologies marks an important step in meeting the needs of today's rapidly evolving financial services industry. This solution addresses key challenges faced by traditional payment systems while delivering significant benefits in performance, scalability, cost optimization, global resilience, sustainability, and compliance.

By leveraging advanced cloud technologies and strong security controls, financial institutions can now build a future-ready platform that adapts to changing business needs while maintaining the highest standards of performance, security, and reliability. As the real-time payments market continues to grow strongly, this modern architecture provides a solution that meets current needs while being ready to support future payment innovations. Organizations looking to modernize their payment infrastructure can use this blueprint to accelerate their digital transformation journey, supporting sustainable, secure, and efficient payment processing at scale in an increasingly competitive global market.

The architecture presented here is for reference only. IBM will work closely with you to implement the solution according to industry standards and compliance requirements.

## Additional resources

- IBM Consulting on AWS
- AWS for Financial Services
- Transforming transactions: Streamlining PCI compliance using AWS serverless architecture
- Best practices for right-sizing your Apache Kafka clusters to optimize performance and cost
- How to choose the right Amazon MSK cluster type for you
- AWS Shared Responsibility Model
- AWS Federal Information Processing Standard (FIPS) 140-3
- Sustainability with AWS Graviton
- AWS PCI DSS

IBM Consulting is an AWS Premier Tier Services Partner, helping customers using AWS harness the power of innovation and drive business transformation. They are recognized as a Global Systems Integrator (GSI) with over 22 competencies, including Financial Services Consulting. For more information, please contact an IBM Representative.

**TAGS:** AWS partner, Customer Solutions, IBM

---

## About the Authors

![Neeraj Kaushik](/images/3-BlogsTranslated/3.3-Blog3/Neeraj-Kaushik.png)

### Neeraj Kaushik

Neeraj Kaushik is an AWS Ambassador and Client-Growth Leader at IBM Financial Services. He is an Open Group Certified Distinguished Architect with two decades of experience in direct customer-facing implementation roles, spanning multiple industries, including travel and transportation, banking, retail, education, healthcare, and anti-human trafficking. As a trusted advisor, he works directly with client executives and architects to identify business strategy and build technology roadmaps. As a practicing AWS Professional Certified Architect and Natural Language Processing Expert, he has led many complex cloud modernization programs and AI initiatives.

![Subhash Sharma](/images/3-BlogsTranslated/3.3-Blog3/Subhash-Sharma.png)

### Subhash Sharma

Subhash Sharma is a Sr. Partner Solutions Architect at AWS. He has over 25 years of experience implementing distributed, scalable, highly available, and secured software products using microservices, AI/ML, Internet of Things (IoT), and blockchain following a DevSecOps approach. In his spare time, Subhash enjoys spending time with family and friends, hiking, walking on the beach, and watching TV.

![Venkat Gomatham](/images/3-BlogsTranslated/3.3-Blog3/Venkat-Gomatham.png)

### Venkat Gomatham

Venkat Gomatham is a Senior Partner Solutions Architect at AWS, providing strategic guidance to partners in their cloud transformation journey. With over 20 years of experience as an IT architect, he drives innovation and digital transformation initiatives, helping organizations modernize IT landscapes through advanced technologies such as generative AI and IoT.
