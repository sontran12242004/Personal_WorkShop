---
title: "Blog 3"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Hiện đại hóa việc điều phối thanh toán theo thời gian thực trên AWS

by Neeraj Kaushik, Subhash Sharma, and Venkat Gomatham | on 01 OCT 2025 in | Amazon API Gateway | Amazon CloudFront | Amazon Elastic Container Service | Amazon Elastic Kubernetes Service | Amazon Managed Streaming for Apache Kafka (Amazon MSK) | Architecture | AWS Lambda | AWS Partner Network | AWS Solutions Implementations | Financial Services | Partner solutions | Public Sector | Public Sector Partners | Regions Permalink

Thị trường thanh toán theo thời gian thực toàn cầu đang trải qua mức tăng trưởng đáng kể. Theo Fortune Business Insights, thị trường này được định giá 24,91 tỷ USD vào năm 2024 và dự kiến tăng lên 284,49 tỷ USD vào năm 2032, với CAGR 35,4%. Tương tự, Grand View Research báo cáo rằng thị trường thanh toán di động toàn cầu, trị giá 88,50 tỷ USD vào năm 2024, dự kiến tăng với CAGR 38,0% từ 2025 đến 2030.

(Lưu ý: Các nghiên cứu và thống kê của bên thứ ba được cung cấp chỉ nhằm mục đích tham khảo. AWS và IBM không đảm bảo tính chính xác của thông tin này.)

Sự mở rộng nhanh chóng này nhấn mạnh tính cấp bách đối với các tổ chức tài chính trong việc hiện đại hóa cơ sở hạ tầng xử lý thanh toán. Các tổ chức tài chính thường cần xử lý khối lượng giao dịch lớn với độ trễ gần bằng không (near-zero latency) để đáp ứng các Service Level Agreements (SLAs) nghiêm ngặt, đồng thời hỗ trợ khối lượng mobile payments ngày càng tăng.

Tuy nhiên, các traditional payment orchestration systems, thường được xây dựng trên monolithic architectures, gặp khó khăn trong việc đáp ứng các yêu cầu này do latency, availability, và scalability challenges. Hơn nữa, việc phụ thuộc vào on-premises infrastructure dẫn đến chi phí cao và hạn chế innovation, làm nổi bật nhu cầu về modernization.

Khi sustainability trở thành ưu tiên, các tổ chức đang chuyển sang các cloud-based solutions để optimize infrastructure, reduce carbon footprints, và enhance energy efficiency. Sự chuyển dịch này không chỉ cung cấp scalability và performance, mà còn phù hợp với global sustainability goals, đảm bảo tương lai cho real-time payments.

Trong bài viết này, chúng tôi thảo luận về real-time payment orchestration framework. Framework này sử dụng event-driven architecture và các AWS serverless services để nâng cao resiliency, efficiency, và scalability của real-time payments. Bằng cách decomposing payment processing into distinct business capabilities, các tổ chức tài chính có thể cải thiện modularity và flexibility. Việc triển khai tenant-based segregation giúp data isolation và security. Ngoài ra, việc áp dụng asynchronous communication thông qua Amazon Managed Streaming for Apache Kafka (Amazon MSK) còn nâng cao scalability và resilience.

## Điều phối thanh toán theo thời gian thực truyền thống

Payment orchestration đóng vai trò như một middleware solution, giúp streamlining transaction processing qua nhiều payment methods, gateways, và financial institutions. Nó orchestrates key business functions như: Payment authorization, Payment processing, Settlement and clearing, Compliance and risk management, Account management, cho cả inbound và outbound payment flows.

Sơ đồ dưới đây minh họa high-level business capabilities được payment orchestrators hỗ trợ trên các payment flows khác nhau, bao gồm real-time payments, digital disbursements, tax payments, wires, và nhiều loại khác.

![Sơ đồ Hệ thống Xử lý Thanh toán](/images/3-BlogsTranslated/3.3-Blog3/payment-processing-diagram.png)

*Sơ đồ chi tiết mô tả hệ thống xử lý thanh toán với nhiều thành phần. Biểu đồ hiển thị primary payment types ở phía trên (bao gồm Realtime Payments, Digital Disbursement, Credit Transfer, và Peer to Peer Payments) đi xuống các core processing stages bao gồm Payment Acceptance, Execution, Clearing, Reporting, Tracking, Reversals, và Billing.*

Nhiều financial institutions áp dụng tenant-based approach được tổ chức theo địa lý do sự khác biệt trong clearing processes, localized regulations, và transaction requirements giữa các AWS Regions. Tuy nhiên, nếu không tách biệt các dịch vụ đúng cách, các nhóm thường tiếp tục thêm region-specific logic vào các dịch vụ hiện có, từ từ làm tăng monolithic complexity và sử dụng cùng một infrastructure cho tất cả các payment flows.

Traditional payment systems xử lý giao dịch theo linear, với mỗi bước phải chờ bước trước hoàn tất. Tuy nhiên, phân tích payment workflows cho thấy nhiều cơ hội để parallel execution:

- **Sanctions screening and fraud detection** – Compliance và fraud checks có thể chạy đồng thời với initial routing decisions, thay vì chặn tất cả các bước xử lý tiếp theo
- **Payment routing and authorization requests** – Khi các basic validations hoàn tất, routing và authorization có thể thực hiện parallel, không cần tuần tự
- **Payment execution and ledger updates** – Payment execution thực tế không cần chờ ledger records được cập nhật—có thể diễn ra đồng thời
- **Settlement, reconciliation, and tracking** – Các post-transaction processes có thể bắt đầu độc lập ngay khi primary transaction hoàn tất

Cách tiếp cận parallel này có thể cải thiện đáng kể throughput và giảm latency so với traditional queue-based systems, nơi các hoạt động tạo thành sequential chain, kéo dài thời gian xử lý và gây bottlenecks.

Hầu hết legacy payment orchestration systems phụ thuộc nhiều vào on-premises virtual machines (VMs), dẫn đến một số thách thức:

- **Multi-Region support** cho disaster recovery và multi-tenancy dẫn đến capital expenditure và operational overhead lớn
- **High latency và vấn đề SLA** do sequential message processing và độ trễ giữa các globally separated data centers
- **Limited reusability of payment flows** vì monolithic architectures yêu cầu region-specific changes cho local clearing mechanisms and regulations, làm tăng complexity và costs
- **Scalability challenges và tiêu thụ memory cao** do inefficient resource utilization và thực thi logic không cần thiết ở nhiều vùng
- **Complex cross-border payment routing** do sự khác nhau trong clearing rules, transaction limits, và local regulations, làm tăng latency và lỗi routing errors
- **Integration challenges** với các data formats khác nhau vì hệ thống legacy dựa vào proprietary standards (ví dụ: ISO 20022, SWIFT MT), làm phức tạp data conversion và compliance
- **High deployment complexity** cho các payment flows mới do monolithic architectures cần nhiều region-specific modifications, làm chậm time to market
- **Environmental impact và carbon footprint cao** từ on-premises infrastructure tiêu thụ nhiều năng lượng, trong khi cloud-based approaches cải thiện efficiency

## Tổng quan giải pháp

Để vượt qua các thách thức này, kiến trúc đề xuất áp dụng các nguyên tắc thiết kế (design principles) sau để xây dựng giải pháp real-time payment orchestration sẵn sàng cho tương lai:

- **Performance at scale** – Xử lý trên 1.000 transactions per second (TPS) với low latency ổn định dưới các điều kiện tải khác nhau.
- **High availability** – Đạt uptime 99.999% để đáp ứng các yêu cầu nghiêm ngặt của giao dịch tài chính.
- **Geographic resilience** – Hỗ trợ vận hành toàn cầu với region-specific compliance trong khi vẫn duy trì hiệu năng ổn định.
- **Cost optimization** – Giảm total cost of ownership (TCO) thông qua sử dụng hiệu quả tài nguyên và công nghệ serverless.
- **Security and compliance** – Hỗ trợ data protection và tuân thủ regulatory requirements tại các khu vực khác nhau.
- **Operational simplicity** – Đơn giản hóa deployment, monitoring, và maintenance trong toàn bộ hệ sinh thái thanh toán.

**Microservices** – Tách payment processing thành các business capabilities riêng biệt, giúp các tổ chức tài chính tăng tính modularity và flexibility. Kiến trúc dựa trên microservices này cho phép independent scaling và phát triển các thành phần quan trọng.

Sơ đồ dưới đây minh họa kiến trúc giải pháp (high-level solution architecture) cho real-time payments. Các kênh hiện có sử dụng synchronous hoặc asynchronous APIs có thể được điều chỉnh để sử dụng edge-optimized endpoints, giúp giảm latency.

![Kiến trúc Giải pháp Cấp cao cho Thanh toán Thời gian Thực](/images/3-BlogsTranslated/3.3-Blog3/solution-architecture.png)

*Sơ đồ kiến trúc chi tiết một AWS-based payment orchestration platform sử dụng các nguyên tắc event-driven. Nền tảng có các reusable components triển khai trên hai vùng (two regions), với các modules chuyên dụng cho payment initiation, execution, reconciliation, billing, và risk management. Kiến trúc này sử dụng pub/sub messaging patterns để giao tiếp giữa các thành phần (inter-component communication) và kết nối với các enterprise systems bao gồm accounting, compliance, và analytics.*

Một event-driven architecture được sử dụng cho payment orchestration, quản lý giao tiếp thông qua pub/sub pattern. Kiến trúc này duy trì persistent connections, cải thiện performance của toàn bộ quy trình real-time payment processing.

Event-driven architecture cho real-time payment processing cho phép nhiều thao tác thanh toán xảy ra đồng thời thông qua các adaptors khác nhau, trái ngược với các hệ thống truyền thống nơi các quy trình thanh toán được thực hiện tuần tự qua một single pipeline. Payment events được phân phối tới các specialized payment processor microservices dựa trên chức năng của chúng (initiation, execution, tracking, settlements), cho phép từng thành phần xử lý independently mà không cần chờ các thành phần khác hoàn tất.

Vì chúng ta đang chuyển từ sequential processing sang distributed processing, việc duy trì transaction traceability là cực kỳ quan trọng. Các payment tracking adapters trong sơ đồ kết nối với các enterprise analytics systems, tạo ra một specialized layer để giám sát các giao dịch. Pub/sub model cho phép gắn correlation IDs vào các events, giúp các hệ thống theo dõi các related events xuyên suốt các topics và các processing stages khác nhau.

Một standardized event schema là nền tảng cho kiến trúc này, đảm bảo consistency giữa các triển khai theo vùng (regional deployments) đồng thời cho phép customization ở cấp adapter. Schema này định nghĩa các uniform event structures chứa tenant-specific metadata và hỗ trợ versioning để đáp ứng các yêu cầu phát triển trong tương lai. Bằng cách cô lập các region-specific variations vào adapter layer, giải pháp duy trì core functionality trong khi kết nối với các enterprise systems đa dạng thông qua configuration-driven customization thay vì thay đổi code.

Đối với hầu hết các quy trình thanh toán, đặc biệt là các bước xử lý độc lập có thể chạy song song, kiến trúc này mang lại net performance gains mặc dù có topic switching overhead, đặc biệt đối với các giao dịch phức tạp yêu cầu nhiều independent validations hoặc các bước xử lý đồng thời.

## Triển khai trên AWS Cloud

Giải pháp sử dụng edge-optimized Amazon API Gateway cho các kênh (channels). Một edge-optimized API endpoint sẽ định tuyến các yêu cầu (requests) đến Amazon CloudFront Point of Presence (POP) gần nhất, điều này hữu ích khi khách hàng của bạn phân bố địa lý rộng, giúp routing hiệu quả trong từng khu vực (geographical region), cải thiện global responsiveness bằng cách minimize network round trips và đảm bảo các yêu cầu đi theo ngắn nhất có thể trước khi chuyển từ public internet vào client network.

Sơ đồ sau đây minh họa kiến trúc giải pháp high-level cho real-time payments.

![Giải pháp AWS Payment Orchestration Toàn diện](/images/3-BlogsTranslated/3.3-Blog3/aws-payment-orchestration.png)

*Giải pháp AWS Payment Orchestration toàn diện này triển khai các nguyên tắc modern cloud-native architecture. Core processing logic được thực thi dưới dạng Lambda functions, bao gồm các workflow như initiation, execution, reconciliation, billing, tracking, risk management, và settlement. Giải pháp tận dụng Amazon MSK để truyền tải sự kiện (event streaming) một cách đáng tin cậy giữa các thành phần, với các Kafka topics riêng biệt cho từng giai đoạn xử lý. Data persistence được xử lý bằng Amazon DynamoDB, hỗ trợ cross-region operations. Kiến trúc này thể hiện AWS best practices dành cho financial services, bao gồm regional redundancy, serverless computing, managed services, và event-driven design patterns. Hệ thống được tích hợp với external banking infrastructure và enterprise systems trong khi vẫn duy trì separation of concerns thông qua microservices architecture. Giải pháp có built-in support cho compliance monitoring, risk management, và payment tracking thông qua các specialized Lambda functions.*

Giải pháp sử dụng Amazon MSK để triển khai event-driven architecture, giúp xử lý hiệu quả cả inbound và outbound channels traffic thông qua API requests và asynchronous message-based events. Amazon MSK giao tiếp bằng high-performance binary protocol giữa producers, consumers, và brokers, đảm bảo low latency và high throughput. Real-time payments được logically partitioned cho nhiều tenants trong các geographical regions — North America, EMEA, LATAM, và Asia-Pacific.

Mỗi real-time payment tenant tuân theo active/active disaster recovery strategy bằng cách triển khai MSK clusters trên nhiều AWS Regions, được thiết kế để đạt được high availability và resilience. Amazon MSK cung cấp cả hai tùy chọn serverless và provisioned cluster. Nhóm kỹ thuật có thể chọn một trong hai dựa trên non-functional requirements và team expertise. Amazon MSK tự động quản lý partition leadership với leaders trong primary Regions và followers trong secondary Regions. Trong quá trình failover, leaders được re-elected tại các healthy Regions, giúp duy trì khả năng xử lý trong các sự cố khu vực. Sticky partitioning sử dụng consistent hashing để deterministic routing, và cooperative rebalancing giúp efficient failover. Multi-AZ deployment cung cấp zone redundancy và isolated clusters per Region nhằm đảm bảo data sovereignty compliance thông qua programmatic AWS Identity and Access Management (IAM) và Virtual Private Cloud (VPC) boundaries.

Để hỗ trợ seamless cross-Region replication và duy trì message continuity, Amazon MSK Replicator — một fully managed feature của Amazon MSK — được sử dụng để replicate topics và synchronize consumer group offsets giữa các clusters. MSK Replicator đơn giản hóa việc xây dựng multi-Region Kafka applications mà không cần custom code, open-source tool configuration, hoặc infrastructure management. Dịch vụ này tự động provision và scale các tài nguyên cần thiết, cho phép nhóm tập trung vào business logic trong khi chỉ trả phí cho data being replicated. Trong trường hợp xảy ra regional outage hoặc failover, traffic có thể được tự động redirect đến healthy Region mà không mất dữ liệu hoặc gián đoạn dịch vụ, mang lại near-zero Recovery Time Objectives (RTOs) và hoạt động uninterrupted cho các downstream services như payment processors và audit trail consumers.

Ngoài regional redundancy, kiến trúc này sử dụng event-driven architecture để cho phép parallel và decoupled processing của các payment transactions. Các events như transaction initiation, validation, và settlement được phát hành asynchronously và được tiêu thụ bởi các microservices độc lập, giúp drastically reduce end-to-end latency.

Để xử lý các events ở quy mô lớn, kiến trúc có thể sử dụng AWS Lambda, Amazon Elastic Container Service (Amazon ECS), hoặc Amazon Elastic Kubernetes Service (Amazon EKS) tùy thuộc vào non-functional requirements. Automatic scaling phản hồi theo Amazon CloudWatch metrics, và exponential backoff retry logic kết hợp dead-letter queues (DLQs) xử lý các tình huống throttling. Circuit breakers được sử dụng để ngăn cascade failures khi xảy ra high error rates.

Một trong những lợi ích chính của giải pháp là reusability của payment flows trên nhiều regions khác nhau. Mặc dù mỗi region có compliance requirements và settlement rules riêng, nhưng core functionalities của real-time payments (như payment authorization, payment processing, settlement, và clearing) phần lớn tương tự nhau. Điều này cho phép rapid deployment của các giải pháp thanh toán tại new regions mà không cần rearchitecting toàn bộ hệ thống.

Ví dụ, real-time payment system tại US và UK có thể chia sẻ business logic tương tự cho real-time gross settlement, nhưng khác biệt trong clearing và compliance requirements. Giải pháp này xem các khu vực như bounded contexts trong microservices architecture, cung cấp flexibility trong khi vẫn đảm bảo mỗi region có thể xử lý rules và regulations riêng của mình.

## Bền vững

AWS không ngừng đổi mới trong thiết kế, xây dựng và vận hành hạ tầng nhằm tiến tới net-zero carbon vào năm 2040 và trở thành water positive vào năm 2030. Amazon MSK với các AWS Graviton-based instances sử dụng ít hơn tới 60% năng lượng so với các M5 instances tương đương, giúp bạn đạt được các mục tiêu sustainability.

Lambda vốn đã sustainable by design. Mô hình serverless của nó đảm bảo compute resources chỉ được sử dụng khi cần, giảm đáng kể idle infrastructure và năng lượng lãng phí. Thay vì giữ các always-on servers cho những tác vụ không thường xuyên, Lambda provisions compute power just-in-time, đạt gần như zero idle capacity.

## Bảo mật và tuân thủ trong dịch vụ tài chính

Do bản chất nhạy cảm của các giao dịch thanh toán và dữ liệu tài chính, bạn nên áp dụng các security controls cần thiết để đáp ứng các financial regulations như AWS PCI DSS và AWS Federal Information Processing Standard (FIPS) 140-3 theo nhu cầu của tổ chức bạn.

Giải pháp nên bao gồm multi-layered security controls, continuous monitoring, và automated compliance auditing để đáp ứng các yêu cầu nghiêm ngặt từ banking regulators và internal risk teams. Để biết thêm thông tin, hãy tham khảo Security Guidance.

## Kết luận

Hiện đại hóa hệ thống điều phối thanh toán (payment orchestration) sử dụng kiến trúc event-driven và các công nghệ AWS serverless đánh dấu một bước tiến quan trọng trong việc đáp ứng nhu cầu của ngành financial services đang phát triển nhanh chóng ngày nay. Giải pháp này giải quyết các thách thức chính mà các hệ thống thanh toán truyền thống gặp phải, đồng thời mang lại lợi ích đáng kể về performance, scalability, cost optimization, global resilience, sustainability, và compliance.

Bằng việc sử dụng các công nghệ cloud tiên tiến và các security controls mạnh mẽ, các tổ chức tài chính giờ đây có thể xây dựng một nền tảng future-ready, thích ứng với các nhu cầu kinh doanh đang thay đổi, đồng thời duy trì các tiêu chuẩn cao nhất về performance, security, và reliability. Khi thị trường real-time payments tiếp tục tăng trưởng mạnh mẽ, kiến trúc hiện đại này cung cấp giải pháp đáp ứng nhu cầu hiện tại đồng thời sẵn sàng hỗ trợ các payment innovations trong tương lai. Các tổ chức muốn hiện đại hóa hạ tầng thanh toán có thể sử dụng blueprint này để tăng tốc hành trình digital transformation, hỗ trợ xử lý thanh toán sustainable, secure, and efficient ở quy mô lớn trong một thị trường toàn cầu ngày càng cạnh tranh.

Kiến trúc được trình bày ở đây chỉ mang tính tham khảo. IBM sẽ làm việc chặt chẽ với bạn để triển khai giải pháp theo industry standards và các yêu cầu compliance.

## Tham khảo thêm các tài nguyên

- IBM Consulting on AWS
- AWS for Financial Services
- Transforming transactions: Streamlining PCI compliance using AWS serverless architecture
- Best practices for right-sizing your Apache Kafka clusters to optimize performance and cost
- How to choose the right Amazon MSK cluster type for you
- AWS Shared Responsibility Model
- AWS Federal Information Processing Standard (FIPS) 140-3
- Sustainability with AWS Graviton
- AWS PCI DSS

IBM Consulting là AWS Premier Tier Services Partner, giúp khách hàng sử dụng AWS khai thác sức mạnh của innovation và thúc đẩy business transformation. Họ được công nhận là Global Systems Integrator (GSI) với hơn 22 competencies, bao gồm Financial Services Consulting. Để biết thêm thông tin, vui lòng liên hệ với IBM Representative.

**TAGS:** AWS partner, Customer Solutions, IBM

---

## Về các Tác giả

![Neeraj Kaushik](/images/3-BlogsTranslated/3.3-Blog3/Neeraj-Kaushik.png)

### Neeraj Kaushik

Neeraj Kaushik là AWS Ambassador và Client-Growth Leader tại IBM Financial Services. Ông là Open Group Certified Distinguished Architect với hai thập kỷ kinh nghiệm trong các vai trò triển khai trực tiếp với khách hàng, trải dài nhiều ngành, bao gồm travel and transportation, banking, retail, education, healthcare, và anti-human trafficking. Với vai trò là trusted advisor, ông làm việc trực tiếp với các client executive và architects để xác định business strategy và xây dựng technology roadmap. Là một AWS Professional Certified Architect và Natural Language Processing Expert thực hành, ông đã dẫn dắt nhiều chương trình cloud modernization phức tạp và các sáng kiến AI.

![Subhash Sharma](/images/3-BlogsTranslated/3.3-Blog3/Subhash-Sharma.png)

### Subhash Sharma

Subhash Sharma là Sr. Partner Solutions Architect tại AWS. Ông có hơn 25 năm kinh nghiệm trong việc triển khai các sản phẩm software phân tán, scalable, highly available, và secured bằng cách sử dụng microservices, AI/ML, Internet of Things (IoT) và blockchain theo DevSecOps approach. Trong thời gian rảnh, Subhash thích dành thời gian với gia đình và bạn bè, đi hiking, đi dạo trên beach, và xem TV.

![Venkat Gomatham](/images/3-BlogsTranslated/3.3-Blog3/Venkat-Gomatham.png)

### Venkat Gomatham

Venkat Gomatham là Senior Partner Solutions Architect tại AWS, cung cấp strategic guidance cho các đối tác trong hành trình cloud transformation. Với hơn 20 năm kinh nghiệm là một IT architect, ông thúc đẩy các sáng kiến innovation và digital transformation, giúp các tổ chức hiện đại hóa IT landscapes thông qua các công nghệ tiên tiến như generative AI và IoT.
