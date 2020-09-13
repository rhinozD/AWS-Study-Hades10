# AWS-SA-Pro-Notes
Ghi chú lại kiến thức đã học trong quá trình luyện thi chứng chỉ AWS Solution Architect Professional Certificate.

# Cloud Guru & Whitepapers

## Tuần 3 - Security

### Security - Concepts

#### 1. Nguyên tắc Least Privilege(Quyền tối thiểu)
Cung cấp cho users hoặc services quyền hạn tối thiểu nhất cần để thực thi(chỉ khi users hoặc services cần các quyền này)

#### 2. Các khía cạnh của Security
- __Identity__ \
    (Who you are?) - ex: IAM user, Root account, etc
- __Authentication__ \
    (Prove that you're who you say) - ex: Client-side SSL Cert, MFA
- __Authorization__ \
    (Are you allowed to do this) - ex: IAM Policies
- __Trust__ \
    (Do other entities that I trust say they trust you) - ex: Cross-account access, SAML-based Federation, Web Identity Federation

#### 3. SAML vs OAuth vs OpenID
- __SAML 2.0__
    - Có thể handle cả authen và author
    - XML-based protocol
    - Có thể chưa user, group membership và những thông tin hữu ích khác
    - Sử dụng assertions trong XML để authen, author hoặc cung cấp attributes
    - Phù hợp cho use-case: SSO cho enterprise users(người dùng doanh nghiệp)

- __OAuth 2.0__
    - Cho phép chia sẻ protected assets mà không cần gửi login credentials
    - Chỉ handle author, không handle authen
    - Cung cấp token cho client
    - Application validates token bằng Author Server
    - Uỷ quyền truy cập, cho phép client applications truy cập thông tin thay mặt user
    - Phù hợp cho use-case: API Author giữa các apps

- __OpenID Connect__
    - Identity layer trên OAuth 2.0, thêm authen
    - Sử dụng REST/JSON message flows
    - Supports web clients, mobile, clients, JS clients
    - Có khả năng mở rộng
    - Phù hợp cho use-case: SSO cho khách hàng

#### 4. Multi-Account Management
- Nhiều tổ chức lớn có multiple accounts
- Phân chia nhiệm vụ, quản lý cost, tăng tính linh động
- Cần các phương thức để quản lý và maintain các account.
- Nên sử dụng multiple accounts khi:
    - Muốn quản lý tách biệt các workloads
    - Muốn giới hạn, khoanh vùng các workloads (chỉ account A mới thấy và làm việc được workload A)
    - Muốn tách biệt để khi xảy ra sự cố thì giảm thiểu thiệt hại nhất. (Nếu bị hack thì thì không mất hoàn toàn)
    - Muốn tách biệt để recovery và auditing data
- AWS Tools for Account Management
    - AWS Organizations
    - SCPs
    - Tagging
    - Resource Groups
    - Consolidated Billing
- Identity Account Structure
    - Quản lý tất cả user accounts tập trung
    - Users sủ dụng trust relationship từ IAM roles trong sub-account đến Identity Account để cung cấp quyền access tạm thời
    - Variations bao gồm Business Unit, Deployment Env, Geography
- Logging Account Structure
    - Tập trung logging repository
    - Vì logging tập trung nên có thể bảo mật khi chỉ cần quản lý account chưa logging, cũng như không sợ mất log hay log thay đổi
    - Có thể sử dụng SCPs để ngăn không cho sub-account thay đổi logging settings
- Publishing Account Structure
    - Tạo Common repository cho AMI's, Containers, Code
    - Đảm bảo sub-accounts có thể sử dụng những services(or assets) đã được chuẩn hoá
- Information Security Account Structure
    - Giống Identity Account Structure cho hybrid
- Central IT Account Structure
    - IT có thể quản lý IAM users và groups trong khi assign sub-account roles
    - IT có thể cung cấp shared services và các asset đã được chuẩn hoá (AMI's, DB, EBS,etc) phù hợp với policy của tổ chức
- AWS Organizations
    - Chia theo OU (Organization Unit)
    - có thể setting SCPs

#### 5. Network Controls and Security Groups
- Security Groups
    - Virtual Firewalls cho các asset riêng biệt
    - Control inbound và outbound traffic
    - Có thể setting port hoặc port ranges
    - Inbound rules: Source IP, Subnet, SG khác
    - Outbound rules: Des IP, Subnet, SG khác
- NACL
    - Lớp bảo mật cho VPC(như firewall) ở tầng Subnet
    - Default NACL cho phép all inbound và outbound traffic
    - NACLs là stateless
    - Có thể trùng hoặc kêt hợp với SGs để kiểm soát access
    - Phải setting port cho Outbound
 
#### 6. AWS Directory Services
- Các loại Directory Service
    - AWS Cloud Directory
    - Amazon Cognito
    - AWS Directory Service for Microsoft AD
    - AD Connector
        - Phải có AD đang tồn tại để kết nối
        - Users của AD đang tồn tại có thể access vào AWS assets thông qua IAM roles
        - Supports MFA thông qua kiến trúc RADIUS-based MFA
    - Simple AD
        - Stand-alone AD based on Samba
        - Supports user accounts, groups, group policies, và domain
        - Kerberos-based SSO ( [Kerberos](https://directory.apache.org/apacheds/kerberos-user-guide.html) )
        - Không support MFA
        - Không có Trust Relationships

#### 7. Credential and Access Management
- STS Service
- Secure Token Service Flow

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/security/secureTokenServiceFlow-1.png" alt="Secure Token Service Flow">
</p>

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/security/webIdentityFederation.jpg" alt="Web Identity Federation">
</p>

- Token Vending Machine Concept
    - AWS recommends dùng Cognito cho mobile development
- AWS Secrets Manager
    - Lưu trữ passwords, encryption keys, API keys, SSH keys, PGP keys, etc
    - Thay thế cách lưu trữ passwords hoặc keys trong một vault (software hoặc vật lý)
    - Có thể access secrets thông qua API với sự kiểm soát truy cập được cung cấp bới IAM
    - Auto rotate RDS DB credentials cho MySQL, PostgreSQL và Aurora
    - Tốt hơn là hard coding credentials trong application hoặc scripts

#### 8. Encryption
- Encryption at Rest
    - Dữ liệu được encrypt khi được lưu trữ trong EBS, S3, RDS DB, hoặc trong một SQS queue đang chờ để được xử lý.
- Encryption in Transit
    - Dữ liệu được encrypt khi đang chạy qua các luồng thông qua network hoặc process. ex: SSL/TLS cho HTTPS, IPsec cho VPN Connections
- KMS
    - Lưu trữ Key, quản lý và auditing
    - Kết hợp với nhiều services như Lambda, S3, EBS, SQS, etc
    - Có thể import keys hoặc generate keys
    - Control ai có thể quản lý, access những key trong KMS thông qua IAM users và roles
    - Audit dùng keys thông qua CloudTrail
    - Khác Secret Manager
    - Validated bởi nhiều compliance schemes
    - HA và durable
    - AWS quản lý root of trust
- CloudHSM
    - Hardware device chuyên dụng, thuê riêng
    - CloudHSM phải nằm trong một VPC và có thể access qua VPC Peering
    - Offload SSL từ web servers, CloudHSM tương tự như cung cấp CA, bật TDE cho Oracle DBs
    - Khách hàng tự quản lý durability và available
    - Khách hàng quản lý root of trust
- AWS Certificate Manager
    - Cung cấp, quản lý và deploy public hoặc private SSL/TLS Certs
    - Kết hợp với nhiều service như CloudFront, ELB, API Gateway
    - Public Certs miễn phí khi dùng với AWS Services; không cần đăng kí thông qua một 3rd party cert authority nhưng có thể import 3rd party certs để dùng trên AWS
    - Support wildcard domains (*.domain.com) để cover được hết subdomains
    - Quản lý renewal certs
    - Có thể tạo Private Cert Authority cho internal hoặc những app, service hoặc device độc quyền

#### 9. DDoS 
- Phishing
- Amplification/Reflection Attacks
- Application Attacks
- Giảm thiểu nguy cơ bị tấn công DDoS
    - Giảm thiểu attack surface. AWS Services: NACLs, SGs, VPC Design
    - Scale để hấp thụ attack. AWS Services: ASGs, AWS CloudFront, Static Web Content via S3
    - Bảo vệ các tài nguyên đã expose. AWS Services: Route53, AWS WAF, AWS Shield
    - Học những hành vi bình thường. AWS Services: AWS GuardDuty, CloudWatch
    - Chủ động xây dựng hệ thống chống DDoS

#### 10. IDS and IPS
- IDS (Intruder Detection System) - Phát hiện kẻ xâm nhập \
Theo dõi network và hệ thống nhằm phát hiện các hoạt động đáng nghi có thể gây tổn hại đến hệ thống
- IPS (Intruder Prevention System) - Phòng chống kẻ xâm nhập \
Ngăn cản các hành vi khai thác lỗ hỏng bằng cách dùng firewalls và scan, phân thích các content đáng nghi \
Thông thường bao gồm một hệ thống Collection / Monitoring và các monitoring agent trên mỗi hệ thống \
Logs được thu thập hoặc phân tích trong CloudWatch, S3 hoặc 3rd party tools(Splunk, SumoLogic, etc) được gọi là hệ thống SIEM (Security Information and Event Management)
- CloudWatch vs CloudTrail

#### 11. AWS Service Catalog




