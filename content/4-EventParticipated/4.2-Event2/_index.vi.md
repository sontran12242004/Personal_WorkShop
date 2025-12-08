---
title: "Event 2"
date: 2025-10-03
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Bài thu hoạch "AWS GenAI Builders Club"

### Mục Đích Của Sự Kiện

Sự kiện nhằm khám phá vòng đời phát triển phần mềm dựa trên AI (AI DLC) và giới thiệu các công cụ phát triển hiện đại được hỗ trợ bởi AI, tập trung vào cách AI đang thay đổi thực hành phát triển phần mềm và cách các nhà phát triển có thể tích hợp AI hiệu quả vào quy trình làm việc của họ.

### Diễn Giả

1. **Toàn Huỳnh** - Senior Specialist SA (Solutions Architect)
   - Chủ đề: AI-driven Development Lifecycle (Vòng đời phát triển phần mềm dựa trên AI)

2. **My Nguyễn** - Senior Prototyping Architect
   - Chủ đề: Kiro - The AI IDE for Prototype to Production (Kiro - IDE AI từ prototype đến production)

### Điểm Nổi Bật

#### AI-Driven Development Lifecycle (bởi Toàn Huỳnh)

**Sự tiến hóa của AI trong phát triển phần mềm:**
- 2023: AI giúp developers viết code nhanh hơn
- 2024: AI tạo ra các đoạn code lớn hơn và trả lời nhanh hơn
- 2025: AI hoàn thành các tác vụ phát triển từ đầu đến cuối với con người trong vòng lặp

**Nguyên tắc chính:**
- AI DLC không phải chỉ là một công cụ - đó là một phương pháp luận
- AI nên được sử dụng cho pair-programming, không phải là giải pháp độc lập
- Developers phải luôn là người sở hữu và xác thực tất cả code do AI tạo ra
- Kiểm soát chất lượng là trách nhiệm của developer

**Thách thức khi scale với AI:**
- Vấn đề kiểm soát chất lượng khi tạo ra lượng code lớn
- Mất kiểm soát và không hiểu AI đang làm gì
- Rủi ro phải khởi động lại dự án nếu không duy trì chất lượng
- Sử dụng quá nhiều token trong các dự án lớn

**Quy trình AI DLC:**
- AI không nên đưa ra quyết định độc lập - developers đưa ra tất cả quyết định
- Sử dụng AI để phân tích vấn đề, xem xét yêu cầu và tạo kế hoạch
- Lưu trữ kết quả trong file Markdown để duy trì tính liên tục
- Chia nhỏ dự án lớn thành các đơn vị nhỏ hơn
- Xem xét và xác thực thường xuyên là điều cần thiết
- Sử dụng AI cho các tác vụ chuẩn hóa, xử lý các tác vụ suy nghĩ thủ công

**Thực hành tốt nhất:**
- Bắt đầu với yêu cầu đơn giản và chia thành các đơn vị
- Sử dụng AI để nhóm các đơn vị quan trọng cho end users
- Triển khai từng đơn vị như một dự án nhỏ
- Duy trì một track chung cho sự hợp tác backend và frontend
- Đối với dự án lớn, cân nhắc sử dụng Amazon Q thay vì các công cụ AI đơn giản

#### Kiro - The AI IDE (bởi My Nguyễn)

**Tổng quan:**
- Kiro là một IDE được hỗ trợ bởi AI được thiết kế cho phát triển từ prototype đến production
- Đã thảo luận về sự khác biệt chính giữa Kiro và VSCode
- Tập trung vào việc hợp lý hóa quy trình phát triển từ ý tưởng đến triển khai

### Bài Học Rút Ra

1. **AI như một đối tác, không phải thay thế:**
   - Xem AI như một đối tác hợp tác trong giải quyết vấn đề
   - Duy trì giám sát và quyền ra quyết định của con người
   - Đảm bảo chất lượng vẫn là trách nhiệm của con người

2. **Quy trình AI có cấu trúc:**
   - Định nghĩa vai trò và trách nhiệm rõ ràng trong prompts
   - Sử dụng file Markdown để theo dõi tiến độ và duy trì ngữ cảnh
   - Chia nhỏ các dự án phức tạp thành các đơn vị có thể quản lý
   - Triển khai các chu kỳ xem xét thường xuyên

3. **Kiểm soát chất lượng:**
   - Không bao giờ ủy thác hoàn toàn nhiệm vụ cho AI mà không xác thực
   - Xem xét code và kế hoạch do AI tạo ra thường xuyên
   - Xây dựng tài liệu (file Markdown) để tránh đi chệch hướng
   - Sử dụng AI cho các tác vụ chuẩn hóa, không phải tư duy phản biện

4. **Quản lý dự án:**
   - Bắt đầu với yêu cầu đơn giản và thu hẹp phạm vi
   - Sử dụng AI để nhóm các đơn vị quan trọng cho end users
   - Coi mỗi đơn vị như một dự án nhỏ riêng biệt
   - Duy trì các kênh giao tiếp chung cho sự hợp tác nhóm

5. **Lựa chọn công cụ:**
   - Đối với dự án lớn, cân nhắc các giải pháp doanh nghiệp như Amazon Q
   - Các công cụ AI đơn giản phù hợp cho các tác vụ nhỏ, được xác định rõ
   - Chọn công cụ dựa trên độ phức tạp và yêu cầu của dự án

### Áp Dụng Vào Công Việc

1. **Triển khai phương pháp luận AI DLC:**
   - Áp dụng cách tiếp cận có cấu trúc cho phát triển được hỗ trợ bởi AI
   - Tạo tài liệu Markdown cho lập kế hoạch và theo dõi dự án
   - Thiết lập quy trình làm việc rõ ràng cho sự hợp tác AI

2. **Đảm bảo chất lượng:**
   - Luôn xem xét và xác thực code do AI tạo ra
   - Triển khai các điểm kiểm tra thường xuyên trong quy trình phát triển
   - Duy trì quyền sở hữu tất cả các sản phẩm giao hàng

3. **Chia nhỏ dự án:**
   - Chia các dự án lớn thành các đơn vị nhỏ, có thể quản lý
   - Sử dụng AI để giúp xác định và nhóm các thành phần quan trọng
   - Theo dõi tiến độ bằng cách sử dụng checkbox trong tài liệu lập kế hoạch

4. **Hợp tác nhóm:**
   - Thiết lập các track chung cho các nhóm backend và frontend
   - Sử dụng AI để tạo điều kiện giao tiếp và lập kế hoạch
   - Duy trì tài liệu rõ ràng để đồng bộ nhóm

5. **Đánh giá công cụ:**
   - Đánh giá các công cụ AI dựa trên nhu cầu dự án
   - Cân nhắc các giải pháp doanh nghiệp cho các dự án quy mô lớn
   - Sử dụng công cụ phù hợp cho các giai đoạn phát triển khác nhau

### Trải Nghiệm Sự Kiện

Sự kiện cung cấp những hiểu biết có giá trị về việc áp dụng thực tế của AI trong phát triển phần mềm. Các diễn giả nhấn mạnh tầm quan trọng của việc duy trì kiểm soát và giám sát của con người trong khi tận dụng khả năng của AI. Cuộc thảo luận về phương pháp luận AI DLC đặc biệt có ý nghĩa, cho thấy cách cấu trúc sự hợp tác AI một cách hiệu quả.

Bài trình bày về Kiro đã cung cấp cái nhìn sâu sắc về các công cụ phát triển thế hệ tiếp theo tích hợp AI trực tiếp vào IDE, có khả năng hợp lý hóa quy trình phát triển từ prototype đến production.

Bài học quan trọng là AI nên nâng cao khả năng của developer thay vì thay thế các quy trình tư duy phản biện và ra quyết định. Tích hợp AI thành công đòi hỏi lập kế hoạch cẩn thận, xem xét thường xuyên và duy trì quyền sở hữu rõ ràng của quy trình phát triển.

#### Một số hình ảnh sự kiện

![Hình ảnh sự kiện 2 - 1](/images/4-EventParticipated/4.2-Event2/event2-photo1.jpeg)

![Hình ảnh sự kiện 2 - 2](/images/4-EventParticipated/4.2-Event2/event2-photo2.jpeg)

![Hình ảnh sự kiện 2 - 3](/images/4-EventParticipated/4.2-Event2/event2-photo3.jpeg)

![Hình ảnh sự kiện 2 - 4](/images/4-EventParticipated/4.2-Event2/event2-photo4.jpeg)

![Hình ảnh sự kiện 2 - 5](/images/4-EventParticipated/4.2-Event2/event2-photo5.jpeg)

> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn giúp tôi định hình lại suy nghĩ về vòng đời phát triển phần mềm dựa trên AI và cách tích hợp hiệu quả các công cụ AI vào quy trình phát triển trong khi vẫn duy trì chất lượng và kiểm soát.

