# AWS-SA-Pro-Notes
Ghi chú lại kiến thức đã học trong quá trình luyện thi chứng chỉ AWS Solution Architect Professional Certificate.

## Tuần 4 - Migrations

### Migration - Strategies

- **Re-Host**: "Lift and Shift"; chuyển assets từ on-prem lên cloud mà không thay đổi gì hết. Effort: * * , Khả năng(cơ hội) tối ưu hóa: * *  \ Ex: Chuyển on-prem MySQL DB lên EC2 Instance.
- **Re-Platform**: "Lift and Reshape"; Chuyển assets và thay đổi underlying platform. Effort: * * * * , Khả năng(cơ hội) tối ưu hóa: * * *  \ Ex: Migrate on-prem MySQL DB đên RDS MySQL.
- **Re-Purchase**: "Drop and Shop"; Bỏ hệ thống đang chạy trên on-prem, tạo mới hoàn toàn trên cloud. Effort: * * * , Khả năng(cơ hội) tối ưu hóa: *  \ Ex: Migrate legacy on-prem CRM system to salesforece.com
- **Rearchitect**: Design lại apps với cloud-native manner. Effort: * * * * * , Khả năng(cơ hội) tối ưu hóa: * * * * *  \ Ex: Tạo một serverless version của application.
- **Retire**: Đánh giá những app không còn cần nữa. Effort: - , Khả năng(cơ hội) tối ưu hóa: - \ Ex: ...
- **Retain**: "Do nothing option"; Quyết định sẽ đánh giá lại trong tương lai. Effort: * , Khả năng(cơ hội) tối ưu hóa: - \ Ex: ...

### Cloud Adoption Framework
#### 1. The Open Group Architecture Framework(TOGAF)
- Tự tìm hiểu sau

### Hybrid Architecture
- Hybrid Architecture kết hợp cloud resources và on-premises resources.
- Bước đầu tiên để migrate on-premise lên cloud.
- Infrastructure có thể gia tăng hoặc mở rộng on-prem platforms. ví dụ như VMWare.
- Về lý thuyết, integrations có kết nối không chặt chẽ - nghĩa là on-prem hoặc cloud có thể tồn tại mà không phụ thuộc quá nhiều vào phía còn lại.
- Thông tin tìm hiểu thêm: [Hybrid Cloud use cases](https://aws.amazon.com/hybrid/use-cases/)

### Migration Tools
#### 1. Storage Migration
- AWS Storage Gateway
- AWS Snowball
#### 2. Server Migration Service
- AWS Server Migration Service
    - Auto migration on-prem VMware vSphere hoặc Microsoft Hyper-V/SCVMM virtual machines lên AWS.
    - Replicates VMs sang AWS, sync các volumes và tạo AMIs định kỳ.
    - Để giảm thiểu cutover downtime, VMs được sync incrementally.
    - Shỉ support Windows và Linux VMs.
    - Server Migration Connector được download vào on-prem vSphere hoặc Hyper-V setup như một virtual appliance.
#### 3. Database Migration Service
- Data Migration Service(DMS) cùng với Schema Conversion Tool(SCT) giúp customers migrate DB đến AWS RDS hoặc EC2-based DBs.
- SCT có thể copy DB schemas cho cùng một loại DB (MySQL -> RDS MySQL) hoặc convert schemas cho khác loại DB (Oracle -> Aurora)
- DMS sử dụng conversions nhỏ hơn, đơn giản hơn và có support MongoDB và DynamoDB.
- SCT sử dụng cho datasets lớn hơn, phức tạp hơn như data warehouses.
- DMS có chức năng replication cho on-prem -> AWS hoặc -> Snowball hoặc -> S3.
#### 4. Application Discovery Service
- Thu thập thông tin của on-prem data centers phục vụ cloud migration planning.
- Thông thường thì customers không biết 100% hệ thống cũng như trạng thái của tất cả data center assets của họ, Application Discovery Service có thể giải quyết vấn đề này.
- Thu thập config, usage và behavior data từ servers để phục vụ việc estimate TCO (Total Cost of Ownership) khi mà chạy trên AWS.
- Có thể chạy như agent-less (VMware env) hoặc agent-based(non-VMware env).
- Chỉ support Linux và Windows Linux.
#### 5. AWS Migration Hub
- Kết hợp tất cả những services trên lại với nhau.

### Network Migration and Cutovers
#### 1. CIDR Reservations
- Đảm bảo IP addresses không bị overlap giữa VPC và on-prem.
- VPCs support IPv4 netmasks range từ /16 đến /28.
- Note: 5 IPs ở mỗi VPC sẽ được AWS sử dụng, user không thể sử dụng.
#### 2. Network Migrations
- Hầu hết các tổ chức sẽ dùng VPN connection kết nối đến AWS.
- Nếu như quy mô phát triển lớn, khách hàng có thể lựa chọn DX nhưng vẫn giữ VPN như một phương án backup.
- Transition từ VPN đến DX có thể liền mạch khi sử dụng BGP.
- Khi DX được setup, config cả VPN và DX với cùng một BGP prefix.
- Ở phía AWS, DX path luôn được chỉ định, nhưng cần phải đảm bảo DX path là route được chỉ định từ on-prem network đến AWS và không phải VPN (thông qua BGP weighting hoặc static routes)

### AWS Snow Family
- Phát triển từ xử lý AWS Import/Export
- Chuyển một lượng lớn data ra vào AWS
- Encrypted at rest
- Encrypted in transit
#### 1. AWS Import/Export
- Chuyển một external hard drive đến AWS. Hard Drive được cắm vào và copy data vào S3
#### 2. AWS Snowball
- Dữ liệu được chuyển bằng AWS box (có bảo mật). Có thể chứa tối đa 80TB. Data được copy over to S3.
#### 3. AWS Snowball Edge
- Giống như AWS Snowball, nhưng dùng cho onboard Lambda và clustering.
#### 4. AWS Snowmobile
- Chuyển dữ liệu chứa trong một container (tối đa 100PB) bằng phương tiện chuyên dụng.