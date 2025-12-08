---
title: "Event 5"
date: 2025-11-29
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Bài thu hoạch "AWS Cloud Mastery Series #3"

### Mục Đích Của Sự Kiện

Sự kiện nhằm khám phá các thực hành tốt nhất về AWS Identity and Access Management (IAM), tập trung vào các nguyên tắc bảo mật, kiểm soát truy cập dựa trên vai trò, Single Sign-On (SSO), AWS Organizations, quản lý credentials, và các tính năng IAM nâng cao cho bảo mật doanh nghiệp.

### Diễn Giả

1. **Huỳnh Hoàng Long**
2. **Đinh Lê Hoàng Anh**

### Điểm Nổi Bật

#### Hiểu Về IAM (Identity and Access Management)

**Định nghĩa:**
- IAM là các role có quyền nhất định và gán cho các user nhất định
- Tăng tính xác thực cho hệ thống
- Cung cấp kiểm soát truy cập chi tiết đến tài nguyên AWS

**Khái niệm cốt lõi:**
- Roles định nghĩa các hành động có thể thực hiện
- Users được gán roles dựa trên trách nhiệm của họ
- Tăng cường bảo mật hệ thống thông qua quản lý truy cập phù hợp

#### Thực Hành Tốt Nhất Về IAM

**1. Nguyên Tắc Least Privilege:**
- Thực hành tốt nhất: Không cho một account quá nhiều quyền
- Chỉ cấp các quyền cụ thể cần thiết cho công việc
- Giảm rủi ro bảo mật và thiệt hại tiềm năng từ các account bị xâm phạm

**2. Quản Lý Root Access Key:**
- **Xóa root access key** vì nó có quyền hạn cao nhất
- Nếu mất access key thì không ảnh hưởng quá nhiều
- Giảm tối đa tổn thất
- Root account chỉ nên được sử dụng cho thiết lập ban đầu và các tình huống khẩn cấp

**3. Tránh Wildcard Permissions:**
- Tránh dùng `*` có nghĩa là toàn quyền cho tất cả dịch vụ
- Sử dụng quyền cụ thể cho service và action thay thế
- Cung cấp bảo mật và khả năng kiểm tra tốt hơn

**4. Sử Dụng AWS Login (Temporary Credentials):**
- Nên sử dụng AWS login vì nó reset ít nhất mỗi 15 phút một lần và nhiều nhất là 36 tiếng một lần
- Tăng bảo mật đáng kể
- Temporary credentials giảm rủi ro phơi nhiễm lâu dài
- Tự động hết hạn, giảm bề mặt tấn công

**5. Multi-Factor Authentication (MFA):**
- MFA là một bước xác thực giúp account không bị đăng nhập ở nơi khác
- Ngăn chặn truy cập trái phép
- Thêm một lớp bảo mật ngoài mật khẩu
- Nên được bật cho tất cả các account có đặc quyền

**6. Đổi Mật Khẩu Thường Xuyên:**
- Thường xuyên nên đổi mật khẩu của account
- Tuân theo thực hành tốt nhất về bảo mật cho quản lý credentials
- Giảm rủi ro credentials bị xâm phạm

#### Single Sign-On (SSO)

**Định nghĩa:**
- SSO cho phép một login có thể truy cập nhiều hệ thống
- Users xác thực một lần và có quyền truy cập nhiều ứng dụng
- Khi chuyển sang app khác thì không cần login lại

**Lợi Ích Doanh Nghiệp:**
- Quản lý tốt hơn trong môi trường doanh nghiệp
- Có nhiều role và mỗi role có một số account nhất định
- Kiểm soát truy cập tập trung
- Trải nghiệm người dùng đơn giản hóa

**Tích Hợp AWS:**
- Khi kích hoạt SSO thì đồng thời kích hoạt AWS Organizations
- Cho phép quản lý tập trung nhiều AWS accounts
- Cung cấp kiểm soát truy cập thống nhất trên toàn tổ chức

#### AWS Organizations và Service Control Policies (SCP)

**AWS Organizations:**
- Cho phép quản lý nhiều AWS accounts từ một vị trí trung tâm
- Cung cấp consolidated billing và quản lý account
- Hoạt động cùng với SSO để kiểm soát truy cập doanh nghiệp

**Service Control Policies (SCP):**
- Chính sách kiểm soát dịch vụ, có khả năng cho phép các quyền hạn tối đa trong một account
- Cung cấp quản trị bảo mật tập trung
- Có thể hạn chế các hành động có thể thực hiện trên các accounts
- Giúp thực thi các chính sách bảo mật tổ chức

#### Permission Boundaries

**Định nghĩa:**
- Là một dịch vụ IAM nâng cao xử lý vấn đề bảo mật trung tâm
- Đặt giới hạn tối đa các quyền mà một identity-based policy có thể cấp
- Cung cấp một lớp kiểm soát bảo mật bổ sung
- Ngăn chặn privilege escalation ngay cả khi policies bị cấu hình sai

**Use Cases:**
- Ủy quyền permissions cho developers hoặc teams
- Đảm bảo users không thể vượt quá permissions dự định
- Quản lý bảo mật tập trung trong các tổ chức lớn

#### Credential Spectrum và Rotation

**Credential Spectrum:**
- Hiểu các loại credentials khác nhau và ý nghĩa bảo mật của chúng
- Từ long-term access keys đến temporary credentials
- Cân bằng giữa bảo mật và khả năng sử dụng

**Credential Rotation:**
- Nói về dịch vụ AWS Secrets Manager
- Quy trình: Tạo ra các tài khoản giả với mật khẩu mới, xem xem có thể sử dụng không, áp dụng vào account của mình, và cuối cùng xóa account cũ
- Tự động hóa việc xoay credentials của database, API keys, và các secrets khác
- Giảm rủi ro credentials bị xâm phạm
- Đảm bảo credentials được cập nhật thường xuyên mà không cần can thiệp thủ công

**Lợi Ích AWS Secrets Manager:**
- Tự động xoay secrets
- Lưu trữ credentials an toàn
- Tích hợp với các dịch vụ AWS
- Audit trail của secret access

#### IAM Access Analyzer

**Tổng quan:**
- Công cụ để phân tích IAM policies và access patterns
- Xác định các tài nguyên được chia sẻ với các thực thể bên ngoài
- Giúp phát hiện truy cập không mong muốn
- Cung cấp khuyến nghị để cải thiện security posture

**Tính Năng Chính:**
- Phân tích resource-based policies
- Xác định các tài nguyên có thể truy cập công khai
- Đề xuất cải thiện policies
- Giám sát liên tục các access patterns

### Bài Học Rút Ra

1. **Nguyên Tắc IAM Cơ Bản:**
   - IAM roles và users cung cấp kiểm soát truy cập chi tiết
   - Cấu hình IAM phù hợp là điều cần thiết cho bảo mật AWS
   - Authentication và authorization là các khái niệm riêng biệt nhưng liên quan

2. **Thực Hành Bảo Mật Tốt Nhất:**
   - Tuân theo nguyên tắc least privilege
   - Xóa root access keys
   - Tránh wildcard permissions
   - Sử dụng temporary credentials (AWS login)
   - Bật MFA cho tất cả accounts
   - Xoay passwords thường xuyên

3. **Quản Lý Truy Cập Doanh Nghiệp:**
   - SSO đơn giản hóa truy cập trên nhiều hệ thống
   - AWS Organizations cho phép quản lý account tập trung
   - SCPs cung cấp quản trị bảo mật toàn tổ chức
   - Permission boundaries ngăn chặn privilege escalation

4. **Quản Lý Credentials:**
   - Sử dụng AWS Secrets Manager cho credential rotation
   - Ưu tiên temporary credentials hơn long-term access keys
   - Tự động hóa credential rotation khi có thể
   - Giám sát việc sử dụng và access patterns của credentials

5. **Bảo Mật Liên Tục:**
   - Sử dụng IAM Access Analyzer cho đánh giá bảo mật liên tục
   - Xem xét và kiểm tra IAM policies thường xuyên
   - Giám sát truy cập không mong muốn
   - Cập nhật với các thực hành bảo mật AWS tốt nhất

### Áp Dụng Vào Công Việc

1. **Triển Khai Thực Hành IAM Tốt Nhất:**
   - Xem xét và xóa root access keys
   - Bật MFA cho tất cả các account có đặc quyền
   - Thay thế wildcard permissions bằng specific permissions
   - Sử dụng temporary credentials thay vì long-term access keys

2. **Thiết Lập SSO:**
   - Đánh giá triển khai SSO cho tổ chức của bạn
   - Cấu hình AWS Organizations nếu quản lý nhiều accounts
   - Triển khai role-based access control
   - Tập trung quản lý users

3. **Triển Khai Permission Boundaries:**
   - Sử dụng permission boundaries cho delegated access
   - Đặt giới hạn permission tối đa cho các teams
   - Ngăn chặn privilege escalation
   - Tập trung các chính sách bảo mật

4. **Credential Rotation:**
   - Sử dụng AWS Secrets Manager cho database credentials
   - Tự động hóa rotation của API keys và secrets
   - Thiết lập lịch rotation
   - Kiểm tra quy trình rotation trước production

5. **Giám Sát Bảo Mật:**
   - Bật IAM Access Analyzer
   - Xem xét access patterns thường xuyên
   - Xác định và khắc phục truy cập không mong muốn
   - Sử dụng CloudTrail cho audit logging

6. **Kiểm Tra Bảo Mật Định Kỳ:**
   - Tiến hành xem xét IAM policies định kỳ
   - Xóa credentials và roles không sử dụng
   - Cập nhật policies dựa trên yêu cầu thay đổi
   - Tài liệu hóa yêu cầu truy cập và lý do

### Trải Nghiệm Sự Kiện

Sự kiện cung cấp những hiểu biết toàn diện về các thực hành bảo mật AWS IAM tốt nhất, bao gồm mọi thứ từ các nguyên tắc cơ bản đến các tính năng doanh nghiệp nâng cao. Các diễn giả chia sẻ kinh nghiệm thực tế và các tình huống thực tế nhấn mạnh tầm quan trọng của cấu hình IAM phù hợp.

#### Hiểu Nguyên Tắc IAM Cơ Bản

- Học được rằng IAM là về các roles với permissions cụ thể được gán cho users
- Hiểu cách cấu hình IAM phù hợp tăng cường authentication hệ thống
- Nhận ra tầm quan trọng của kiểm soát truy cập chi tiết

#### Thực Hành Bảo Mật Tốt Nhất

- **Least Privilege**: Nhận ra tầm quan trọng của việc chỉ cấp permissions cần thiết
- **Root Access**: Hiểu tại sao root access keys nên được xóa
- **Wildcard Avoidance**: Học cách tránh permissions `*` để bảo mật tốt hơn
- **Temporary Credentials**: Đánh giá cao lợi ích bảo mật của AWS login với rotation tự động
- **MFA**: Nhận ra MFA là điều cần thiết để ngăn chặn truy cập trái phép
- **Password Management**: Hiểu nhu cầu đổi mật khẩu thường xuyên

#### Tính Năng Doanh Nghiệp

- **SSO**: Học cách Single Sign-On đơn giản hóa truy cập trên nhiều hệ thống
- **AWS Organizations**: Hiểu cách nó hoạt động với SSO để quản lý tập trung
- **SCP**: Khám phá cách Service Control Policies thực thi bảo mật toàn tổ chức
- **Permission Boundaries**: Nhận ra vai trò của chúng trong việc ngăn chặn privilege escalation

#### Quản Lý Credentials

- **Credential Rotation**: Học về AWS Secrets Manager và rotation tự động
- **Credential Spectrum**: Hiểu các loại credentials khác nhau và ý nghĩa bảo mật của chúng
- **Best Practices**: Khám phá quy trình credential rotation và testing

#### Công Cụ Bảo Mật

- **IAM Access Analyzer**: Học cách sử dụng nó cho đánh giá bảo mật liên tục
- **Monitoring**: Hiểu tầm quan trọng của phân tích access patterns liên tục
- **Remediation**: Khám phá cách xác định và sửa các vấn đề bảo mật

#### Một số hình ảnh sự kiện

![Hình ảnh sự kiện 5 - 1](/images/4-EventParticipated/4.5-Event5/event5-photo1.jpeg)

![Hình ảnh sự kiện 5 - 2](/images/4-EventParticipated/4.5-Event5/event5-photo2.jpeg)

![Hình ảnh sự kiện 5 - 3](/images/4-EventParticipated/4.5-Event5/event5-photo3.jpeg)

> Tổng thể, sự kiện đã thành công trong việc chứng minh rằng IAM là nền tảng của bảo mật AWS. Sự nhấn mạnh vào thực hành tốt nhất, tính năng doanh nghiệp, và giám sát bảo mật liên tục cung cấp một cách tiếp cận toàn diện để quản lý truy cập và bảo vệ tài nguyên AWS một cách hiệu quả.

