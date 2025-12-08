---
title: "Blog 1"
date: 2025-09-09
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Cách Novo Nordisk, Đại học Columbia và AWS hợp tác cùng OpenFold AI Consortium để phát triển OpenFold3

by Daniele Granata, Anamaria Todor, Arthur Grabman, Rômulo Jales, Jacob Mevorach, and Stig Bøgelund Nielsen | on 03 JUN 2025 in | Customer Solution | High Performance Computing | Life Sciences Permalink

Bài viết này được đóng góp bởi Daniele Granata, Principal Modelling Scientist; Søren Moos, Architect Advisor Director; Rômulo Jales, Senior Software Engineer; Stig Bøgelund Nielsen, Senior HPC Cloud Engineer tại Novo Nordisk; và Jake Mevorach, Senior Specialist, Application Modernization; Arthur Grabman, Principal Technical Account Manager; và Anamaria Todor, Principal Solutions Architect tại AWS.

Trong bài viết blog này, chúng tôi rất vui được chia sẻ cách Novo Nordisk, Đại học Columbia và AWS hợp tác để phát triển OpenFold3, một mô hình protein structure prediction tiên tiến, bằng cách sử dụng các giải pháp bioinformatics có khả năng mở rộng và tiết kiệm chi phí trên AWS.

OpenFold Consortium, một nhóm nghiên cứu và phát triển AI phi lợi nhuận, đã đi đầu trong việc tạo ra các công cụ open-source cho sinh học và phát triển thuốc. Dự án mới nhất của họ, OpenFold3, đại diện cho một bước tiến đáng kể trong protein structure prediction. Bài viết này sẽ khám phá cách chúng tôi tận dụng các dịch vụ AWS và các phương pháp sáng tạo để vượt qua những thách thức trong việc huấn luyện mô hình phức tạp này, đồng thời duy trì hiệu quả về chi phí và khả năng mở rộng.

Trước tiên, chúng tôi sẽ giới thiệu về protein structure prediction và thảo luận về tầm quan trọng của nó trong việc thúc đẩy nghiên cứu y học và phát triển dược phẩm.

Tiếp theo, chúng tôi sẽ đi sâu vào ba thách thức chính mà chúng tôi gặp phải và cách chúng tôi giải quyết:

1. **Tạo môi trường nghiên cứu gọn nhẹ:**

Chúng tôi sẽ thảo luận về cách phát triển Research Collaboration Platform (RCP) sử dụng các dịch vụ AWS, nhằm cung cấp một không gian làm việc secure, flexible, và efficient cho các nỗ lực hợp tác.

2. **Tạo Multiple Sequence Alignments (MSAs):**

Chúng tôi sẽ khám phá cách tối ưu hóa quá trình tạo MSA bằng AWS Graviton processors, đạt được cải thiện đáng kể về cả speed lẫn cost-efficiency.

3. **AI/ML Training:**

Chúng tôi sẽ trình bày cách tiếp cận của mình đối với nhiệm vụ tính toán nặng nề trong việc huấn luyện mô hình OpenFold3, bao gồm việc sử dụng Amazon EC2 Capacity Blocks for ML và Spot Instances để quản lý tài nguyên một cách hiệu quả.

Bài viết này cung cấp những insights quý giá cho các nhà nghiên cứu, data scientists, và các tổ chức muốn tận dụng cloud computing cho các nhiệm vụ bioinformatics phức tạp. Cho dù bạn đang làm việc về protein structure prediction hay các dự án tính toán nặng khác trong khoa học đời sống, các chiến lược và giải pháp được thảo luận có thể giúp bạn tối ưu hóa workflows về cả performance và cost-effectiveness.

## Protein Structure Prediction – Tóm tắt

Nếu bạn quan tâm đến việc chữa bệnh, thúc đẩy y học và thậm chí cứu sống mạng người, một nơi tuyệt vời để bắt đầu là hiểu cấu trúc của protein. Có rất nhiều loại protein khác nhau trong cơ thể con người. Một số protein có lợi, chẳng hạn như enzyme lactase, giúp tiêu hóa các sản phẩm từ sữa. Một số protein khác có thể gây hại và dẫn đến bệnh tật hoặc thậm chí tử vong. Ngay cả những thay đổi nhỏ trong cấu trúc của các hợp chất vi mô này cũng có thể quyết định sự sống còn.

Protein-based therapeutics là một phương pháp điều trị bệnh mới và đầy hứa hẹn. Tuy nhiên, do protein có tác động mạnh mẽ lên cơ thể con người, chúng ta cần đảm bảo rằng chúng ta tạo ra protein với cấu trúc phù hợp để điều trị bệnh.

Quá trình phát triển protein-based therapeutic truyền thống thường bao gồm:

1. Tìm các receptor hoặc cấu trúc khác trong cơ thể (trong ngành công nghiệp được gọi là "target") mà bạn tin rằng protein có thể gắn vào để đạt được hiệu quả điều trị mong muốn.
2. Phát triển quy trình sản xuất protein có khả năng tác động lên target này.
3. Xác minh bằng các phương pháp như X-ray crystallography hoặc Nuclear Magnetic Resonance (NMR) Spectroscopy rằng quy trình sản xuất tạo ra protein có cấu trúc mong muốn.
4. Tiến hành nghiên cứu và thử nghiệm mở rộng, cuối cùng là clinical trials, để đảm bảo protein mới phát triển đạt được hiệu quả mong muốn.

Trong lịch sử, vấn đề lớn nhất là dù chúng ta có thể genetically engineer proteins với các chuỗi cụ thể, nhưng chúng ta thiếu các công cụ tốt để dự đoán trước cấu trúc protein dựa trên dữ liệu di truyền. Điều này khiến quá trình phát triển therapeutics mất nhiều thời gian, vì ngay cả khi bạn biết cách tạo protein với chuỗi di truyền cụ thể, bạn vẫn phải có nhiều nhân lực được đào tạo chuyên sâu vận hành thiết bị đắt tiền—ngay cả theo tiêu chuẩn công ty dược phẩm toàn cầu—để xác minh cấu trúc. Nếu cấu trúc không đúng, bạn phải bắt đầu lại và lặp lại quá trình này cho đến khi đạt được cấu trúc đúng. Quá trình phải được khởi động lại mỗi khi có một molecule mới cần đặc trưng hóa.

Tất cả điều này làm cho việc phát triển protein-based therapeutics trở nên vô cùng tốn kém và mất thời gian. Một bước đột phá xuất hiện khi các nhà nghiên cứu nhận ra rằng AI có thể được áp dụng để tăng tốc đáng kể quá trình này. Các nhà nghiên cứu đã có lượng lớn dữ liệu lịch sử, kết hợp protein structures với dữ liệu di truyền tương ứng. Câu hỏi được đặt ra là:

"Nếu chúng ta có dữ liệu kết hợp protein structures với dữ liệu di truyền, liệu chúng ta có thể dùng nó để huấn luyện một mô hình, mà khi được cung cấp một chuỗi di truyền mới, sẽ dự đoán được cấu trúc của protein tương ứng không?"

Cộng đồng nghiên cứu đã trả lời là "có", và từ đó, nhiều mô hình AI ra đời, nhận vào genetic sequence và xuất ra predicted protein structure.

Chạy các mô hình này chỉ tốn một lượng điện năng rất nhỏ, nhưng cung cấp feedback quan trọng về hành vi protein trước khi bắt đầu các thử nghiệm tốn kém và mất thời gian.

## Giới thiệu OpenFold3

OpenFold Consortium là một consortium phi lợi nhuận về AI research and development, chuyên phát triển các công cụ open-source miễn phí hỗ trợ trong biology và drug discovery. Một trong những công cụ họ phát triển là OpenFold, một AI model nhận genetic sequence làm đầu vào và xuất ra predicted structure của protein.

OpenFold3 là phiên bản mới nhất của phần mềm này, và để accelerate its development, Novo Nordisk và AWS đã hợp tác với OpenFold Consortium và Đại học Columbia.

## Thách thức đầu tiên: một môi trường nghiên cứu tối ưu

Chúng tôi bắt đầu dự án này với thời gian và ngân sách hạn chế, và thách thức đầu tiên mà dự án gặp phải là các nhà nghiên cứu cần một môi trường nghiên cứu gọn nhẹ càng sớm càng tốt.

Là scientific partner ưu tiên, Novo Nordisk cần tạo ra một môi trường có thể cơ bản thay đổi cách thức hợp tác nghiên cứu. Môi trường này phải cho phép hợp tác liền mạch giữa Novo Nordisk, Đại học Columbia và OpenFold Consortium, đồng thời tuân thủ nghiêm ngặt các security và compliance standards.

Điều quan trọng là các nhà nghiên cứu có thể sử dụng các tools và devices ưa thích của họ, duy trì workflow đã thiết lập mà không bị gián đoạn. Với timeline gấp rút, chúng tôi cần khả năng thiết lập nhanh và thời gian xây dựng cơ sở hạ tầng ngắn, kết hợp với quy trình onboarding tinh gọn cho các cộng tác viên bên ngoài.

Hệ thống cũng cần đủ flexible để đáp ứng các consortia contract terms khác nhau, đặc biệt là về geographic data location và access controls. Quan trọng nhất, môi trường này phải thúc đẩy quá trình khám phá target sớm bằng cách cung cấp robust computational resources, đồng thời duy trì cost efficiency. Cuối cùng, cần một môi trường có khả năng dynamically scale up trong giai đoạn huấn luyện cường độ cao, nhưng có thể scale down khi không sử dụng để giữ chi phí hiệu quả.

Tất cả các yêu cầu này phải được cân bằng với việc trao quyền cho các nhà nghiên cứu tự do sáng tạo – một yếu tố đặc biệt quan trọng khi xét đến yêu cầu tính toán cao trong việc huấn luyện các large language models phục vụ protein structure prediction.

![Hình 1 – Kiến trúc Research Collaboration Platform](/images/3-BlogsTranslated/3.1-Blog1/rcp-architecture.png)

*Hình 1 – Sơ đồ này minh họa Research Collaboration Platform (RCP) của Novo Nordisk, cho thấy cách nền tảng này tạo ra một môi trường nghiên cứu bảo mật và hiệu suất cao trên AWS Cloud. Những yếu tố chính cần lưu ý là: (1) Secure access layer sử dụng Okta authentication, cho phép các nhà nghiên cứu kết nối an toàn qua web portals hoặc SSH.*

*(2) Flexible computing resources được quản lý bởi AWS ParallelCluster, tự động scale để phù hợp với nhu cầu của các nhà nghiên cứu. (3) AWS EFS và AWS FSx file systems xử lý dữ liệu nghiên cứu. Điểm chính là kiến trúc này cho phép các nhà khoa học thực hiện nghiên cứu phức tạp một cách secure và efficient, đặc biệt với các nhiệm vụ yêu cầu cao như AI model training, đồng thời đảm bảo regulatory compliance.*

*Nền tảng này nổi bật nhờ kết hợp enterprise-grade security với flexibility mà các nhà nghiên cứu cần, và có thể deploy trong vòng chưa đầy 2 giờ thông qua automation.*

Để đáp ứng các yêu cầu này, Novo Nordisk đã phát triển Research Collaboration Platform (RCP), một giải pháp toàn diện xây dựng trên AWS, cung cấp một môi trường secure và flexible cho computational research. Kiến trúc (như minh họa trong Hình 1) sử dụng AWS ParallelCluster làm nền tảng, hoạt động trong Virtual Private Cloud (VPC) để đảm bảo secure isolation cho các workload nghiên cứu.

Tại trung tâm của RCP security model là tích hợp với Okta, cung cấp robust user directory services và multi-factor authentication. Các nhà nghiên cứu có thể truy cập nền tảng thông qua web portals hoặc SSH sử dụng Okta Advanced Server Access, đảm bảo secure và controlled access tới các tài nguyên. Điểm vào của nền tảng là EC2 head node nằm trong public subnet, nơi chứa các công cụ nghiên cứu cần thiết bao gồm RStudio, Docker containers, và các scientific frameworks như Singularity.

Compute infrastructure được thiết kế hợp lý với SLURM job scheduler, quản lý các workload trên các instance types khác nhau, tối ưu cho các nhu cầu tính toán đa dạng – từ general-purpose compute đến các nhiệm vụ memory-intensive và GPU acceleration. Amazon EC2 Auto Scaling đảm bảo tài nguyên được phân bổ dynamically theo nhu cầu, giúp duy trì cost efficiency trong khi đáp ứng các yêu cầu tính toán.

Nền tảng sử dụng Amazon Aurora Serverless cho database management và Amazon DynamoDB cho thông tin trạng thái của cluster, cung cấp giải pháp lưu trữ dữ liệu reliable và scalable.

Data management được xử lý thông qua sự kết hợp của Amazon EFS và FSx for Lustre, cung cấp cả general-purpose và high-performance file storage. Toàn bộ cơ sở hạ tầng được monitor bằng Amazon CloudWatch, với AWS CloudFormation cho phép infrastructure as code, đảm bảo consistent và repeatable deployments. Việc deploy một instance của RCP mất chưa đầy 2 giờ.

Kiến trúc này cung cấp cho các nhà nghiên cứu flexibility để sử dụng các công cụ ưa thích của họ trong khi vẫn đảm bảo security và compliance requirements. Sự kết hợp giữa containerization, automated scaling và high-performance computing capabilities cho phép rapid experimentation và accelerate research workflow, đặc biệt với các nhiệm vụ tính toán nặng như training OpenFold3 model.

## Thách thức thứ hai: Quá trình tạo MSA

Với research environment đã được triển khai và vận hành, chúng tôi phải tìm cách generate hàng triệu Multiple Sequence Alignments (MSAs). Một multiple sequence alignment (MSA) cơ bản là một đơn vị thông tin mô tả mức độ tương đồng giữa ba hoặc nhiều genetic sequences. Điều này quan trọng vì các MSA này (cùng với các dữ liệu khác) sẽ được sử dụng để train OpenFold3.

Phần mềm mà chúng tôi được giao nhiệm vụ scale để tạo các MSA này là HHBlits (cụ thể phiên bản 3, có thể tải miễn phí trên GitHub). Khi tính toán cost và runtime cho bước này, chúng tôi nhận thấy nó sẽ mất gấp đôi thời gian và hơn gấp đôi chi phí so với ngân sách và timeline dự kiến nếu dùng compute instances ban đầu là r5.16xlarge.

Các Amazon EC2 R8g instances, chạy trên AWS Graviton4 processors thế hệ mới nhất, có tiềm năng cung cấp price performance tốt hơn cho workload memory-intensive này. Thử nghiệm benchmark trên r8g.16xlarge cho thấy runtime thấp hơn 50% và cost thấp hơn 55% cho HHBlits so với r5.16xlarge.

Kết hợp với một số tối ưu hóa khác, toàn bộ quá trình chạy nhanh hơn nhiều với chi phí thấp hơn, cho phép chúng tôi tạo hơn một triệu MSA mỗi ngày.

Một thách thức khi chuyển từ R5 sang R8g là sự khác biệt về CPU architecture. R8g sử dụng ARM-based CPUs, trong khi R5 là x86-64. Về mặt mã nguồn, điều này không phải vấn đề vì toàn bộ code đều là C, C++ và Python, hỗ trợ cả AArch64 và x86-64. Tuy nhiên, chúng tôi phải tạo một cluster hoàn toàn mới vì ParallelCluster yêu cầu head node và compute nodes phải cùng loại CPU.

Chúng tôi giải quyết vấn đề này bằng cách định nghĩa một AWS ParallelCluster configuration mới, đồng bộ kiến trúc CPU giữa head node và compute nodes. Để cải thiện hiệu suất, chúng tôi cũng customized EC2 image với EBS làm local storage. Tất cả MSA được tạo đều được lưu trực tiếp vào Amazon S3 bucket ngay lập tức. Cấu hình này chứng minh là rất efficient.

Về phía người dùng cuối, có rất ít thay đổi. Các nhà khoa học chỉ cần sử dụng host name khác để đăng nhập và truy cập vào graviton Slurm queues. Chúng tôi cũng tái sử dụng cùng EFS và FSx for Lustre filesystems được cấu hình ban đầu mà không gặp performance penalties.

Điểm tốt nhất của kiến trúc này là highly scalable. Nếu muốn điều chỉnh số lượng MSA tạo ra cùng lúc, chúng tôi chỉ cần thêm hoặc bớt nodes. Con số một triệu MSA nói trên không phải là giới hạn tối đa; thực tế, chúng tôi chưa biết upper limit về throughput khi sử dụng phương pháp này.

## Thách thức thứ ba: AI/ML Training

Được biết, yếu tố then chốt để chạy các workload AI/ML là GPU. Nhưng đối với protein prediction, chỉ có GPU thôi là chưa đủ. Hệ thống còn phải đáp ứng một loạt pre-requisites bao gồm petabyte-scale storage, high-speed networking, và hàng trăm GPU.

Dự án OpenFold3 yêu cầu lên tới 256 GPUs cho việc huấn luyện. Điều này tương đương với 32 p5en.48xlarge EC2 instances chạy song song, xử lý và tạo dữ liệu trong vài tuần, 24 giờ mỗi ngày.

Cấu hình này đòi hỏi infrastructure phải scalable, resilient, observable, và quan trọng nhất là flexible.

Ban đầu, các nhà khoa học cần xây dựng training logic gần như từ đầu. Mức độ uncertainty cao; do đó, 32 GPU machines không cần thiết ngay từ đầu. Điều họ cần là một môi trường để develop, thực hiện các thử nghiệm nhỏ, và dần dần scale up số lượng nodes dùng cho huấn luyện. Đây chính là đặc điểm hạ tầng mà AWS ParallelCluster có thể cung cấp.

Khi huấn luyện OpenFold3 trên dữ liệu đã tạo, chúng tôi gặp ba thách thức chính:

1. Budget constraints yêu cầu giải pháp cost-effective, không để xảy ra lỗi tốn kém.
2. Tăng độ phức tạp của mô hình mới đòi hỏi distributed AI/ML training infrastructure, vì mô hình không còn vừa trên một GPU hay một instance.
3. Securing GPU resources lớn cho cả huấn luyện và thử nghiệm sơ bộ cần hành động nhanh và phối hợp giữa tất cả các bên liên quan.

Để giải quyết các thách thức này, chúng tôi triển khai các giải pháp sau:

### GPU Resource Allocation:

Chúng tôi sử dụng Amazon EC2 Capacity Blocks for ML, một mô hình tiêu thụ cho phép reservation GPU high-performance trong thời gian ngắn cho các workload machine learning. Dịch vụ này cho phép người dùng reserve hundreds of NVIDIA GPUs đồng bộ trong Amazon EC2 UltraClusters, chỉ định cluster size, future start date, và duration. Capacity Blocks cung cấp quyền truy cập GPU reliable, predictable mà không cần cam kết dài hạn, lý tưởng cho training và fine-tuning ML models, chạy thử nghiệm, và chuẩn bị cho các nhu cầu tăng trong tương lai. Giá cả dynamic, dựa trên cung và cầu, thường quanh mức P5 On-Demand rates.

### Cost Optimization:

Để tối ưu chi phí, chúng tôi sử dụng Amazon EC2 Spot instances. Spot instances cho phép sử dụng spare EC2 capacity với discounts lên tới 90% so với giá On-Demand cho các workload fault tolerant và flexible. Khi GPU infrastructure của AWS mở rộng, nhiều loại GPU instance mới có sẵn qua Spot pricing model, cho phép chạy các workload quy mô lớn với chi phí thấp hơn đáng kể nhờ scale vận hành khổng lồ của AWS. Spot instances có thể bị AWS thu hồi khi EC2 cần tài nguyên, thường có two-minute interruption notice, giúp ứng dụng xử lý chuyển đổi một cách graceful.

Bằng cách triển khai các phương pháp này, RCP team của Novo Nordisk đã đạt được giảm 85% chi phí so với việc sử dụng On-Demand instances cho trường hợp sử dụng của chúng tôi.

### Distributed Training Infrastructure:

Chúng tôi lấy cảm hứng từ các ví dụ AWSome Distributed Training, đặc biệt là những ví dụ về DeepSpeed, để xây dựng scalable training infrastructure của mình. Cấu hình này sử dụng GPUs từ nhiều instances đồng thời một cách hiệu quả, tạo ra một hệ thống highly effective và scalable. Hạ tầng cuối cùng cho phép Slurm job submissions đến một autoscaling, efficient, distributed GPU cluster, có khả năng xử lý nhiệm vụ phức tạp là training mô hình OpenFold3 mới.

Bằng cách triển khai các giải pháp này, chúng tôi đã thành công vượt qua các thách thức ban đầu, tạo ra một training environment cost-effective, scalable, và efficient cho OpenFold3.

## Kết luận

Việc huấn luyện thành công OpenFold3 đại diện cho một sự hợp tác quan trọng giữa Novo Nordisk, Đại học Columbia, và AWS, vượt qua ba thách thức lớn thông qua các giải pháp sáng tạo:

1. Novo Nordisk phát triển Research Collaboration Platform (RCP), một môi trường nghiên cứu secure và flexible xây dựng trên AWS, cho phép hợp tác liền mạch đồng thời duy trì các security standards nghiêm ngặt. Nền tảng này có thể deploy trong vòng chưa đầy hai giờ và cung cấp dynamic scaling cho các computational resources.

2. Chúng tôi cùng nhau tối ưu hóa quá trình Multiple Sequence Alignment (MSA) generation bằng cách sử dụng AWS Graviton processors, đạt giảm 50% runtime và giảm 55% chi phí. Điều này cho phép các nhà nghiên cứu tạo ra hơn một triệu MSA mỗi ngày với một kiến trúc highly scalable.

3. Chúng tôi cùng nhau giải quyết các thách thức AI/ML training bằng cách kết hợp Amazon EC2 Capacity Blocks for ML và Spot instances. Hạ tầng distributed training của chúng tôi sử dụng hiệu quả 256 GPUs trên 32 p5en.48xlarge EC2 machines.

Các giải pháp này không chỉ làm cho dự án trở nên cost-effective và efficient mà còn tạo ra một blueprint cho các hợp tác bioinformatics quy mô lớn trong tương lai giữa Novo Nordisk và AWS.

Việc phát triển thành công OpenFold3 thúc đẩy lĩnh vực protein structure prediction, đóng góp vào các quá trình phát triển therapeutics nhanh hơn và hiệu quả hơn.

**TAGS:** Protein Folding

---

## Về các Tác giả

![Daniele Granata](/images/3-BlogsTranslated/3.1-Blog1/Daniele-Granata.png)

### Daniele Granata

Daniele Granata là Principal Modelling Scientist trong lĩnh vực In Silico Protein Discovery tại Novo Nordisk. Ông đam mê xây dựng kết nối giữa con người và công nghệ và làm việc ở ranh giới của protein science, AI và HPC. Ông có bằng cử nhân và thạc sĩ Vật lý, PhD về "Physics and Chemistry of Biological Systems" và 5 năm postdoctoral experience tại Mỹ và Đan Mạch.

Trong 6 năm gần đây tại Novo Nordisk, Daniele đã nổi bật với việc đẩy mạnh data science và generative AI để thiết kế new drugs cho bệnh nhân, đồng thời vẫn duy trì đam mê open sourcing cho cộng đồng khoa học.

![Anamaria Todor](/images/3-BlogsTranslated/3.1-Blog1/Anamaria-Todor.png)

### Anamaria Todor

Anamaria Todor là Principal Solutions Architect có trụ sở tại Copenhagen, Đan Mạch. Cô lần đầu tiếp xúc với máy tính khi mới 4 tuổi và kể từ đó không bao giờ rời xa computer science, video games, và engineering. Cô đã làm việc ở nhiều vai trò kỹ thuật khác nhau, từ freelancer, full-stack developer, data engineer, technical lead, đến CTO, tại nhiều công ty ở Đan Mạch, tập trung vào ngành gaming và advertising. Cô đã làm việc tại AWS hơn 5 năm với vai trò Principal Solutions Architect, chủ yếu tập trung vào life sciences và AI/ML. Anamaria có bằng cử nhân Applied Engineering và Computer Science, thạc sĩ Computer Science, và hơn 10 năm kinh nghiệm với AWS. Khi không làm việc hay chơi video games, cô tham gia coaching các bạn gái và nữ chuyên gia để giúp họ hiểu và tìm ra con đường trong lĩnh vực công nghệ.

![Arthur Grabman](/images/3-BlogsTranslated/3.1-Blog1/Arthur-Grabman.png)

### Arthur Grabman

Arthur Grabman là Principal Technical Account Manager với hơn 15 năm kinh nghiệm, chuyên về HPC cho enterprise clients. Anh đam mê việc dịch các khái niệm kỹ thuật phức tạp thành giá trị kinh doanh cụ thể. Ngoài công việc, Arthur thích du lịch cùng gia đình.

![Rômulo Jales](/images/3-BlogsTranslated/3.1-Blog1/Romulo-Jales.png)

### Rômulo Jales

Rômulo Jales là Senior Software Engineer tại Novo Nordisk. Với nền tảng vững chắc hơn 15 năm trong software development, Rômulo thích hoàn thành công việc bằng cách tạo ra các giải pháp đơn giản và tinh tế, ngay cả trong các ứng dụng quan trọng (critical mission applications).

Rômulo có bằng cử nhân Computer Engineering, và vẫn bị thúc đẩy bởi curiosity ban đầu về cách thức và lý do các sự vật vận hành như thế nào. Đối với anh, luôn có điều mới để học hỏi và khám phá.

![Jacob Mevorach](/images/3-BlogsTranslated/3.1-Blog1/Jacob-Mevorach.png)

### Jacob Mevorach

Jacob Mevorach là Senior Specialist về containers cho healthcare và life sciences tại AWS. Jacob có nền tảng trong bioinformatics và machine learning. Trước khi gia nhập AWS, Jacob tập trung vào việc hỗ trợ và thực hiện các phân tích quy mô lớn cho genomics và các lĩnh vực khoa học khác.

![Stig Bøgelund Nielsen](/images/3-BlogsTranslated/3.1-Blog1/Stig-Bogelund-Nielsen.png)

### Stig Bøgelund Nielsen

Stig Bøgelund Nielsen là Senior Cloud & HPC Engineer tại Novo Nordisk. Engagement suốt đời với IT, bắt đầu từ những công việc hỗ trợ các doanh nghiệp địa phương, đã định hình sự nghiệp của anh. Hơn 8 năm tại IBM cung cấp nền tảng vững chắc, với các vai trò như IT Service and Infrastructure Architect, Team Lead, và Transition & Transformation Architect/Project Manager, tất cả tập trung vào việc tạo giá trị kinh doanh thông qua các chiến lược và triển khai IT hiệu quả.

Anh gia nhập Novo Nordisk gần hai năm trước với vai trò Cloud & HPC Engineer và hiện là Tech Lead của Research Collaboration Platform (RCP), trực tiếp hỗ trợ các nghiên cứu tiên phong, bao gồm cả OpenFold project. Anh đam mê tận dụng công nghệ để thúc đẩy nghiên cứu và đổi mới.
