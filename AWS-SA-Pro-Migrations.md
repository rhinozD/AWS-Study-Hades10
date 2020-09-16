# AWS-SA-Pro-Notes
Ghi chú lại kiến thức đã học trong quá trình luyện thi chứng chỉ AWS Solution Architect Professional Certificate.

## Tuần 4 - Migrations
- *__Operational Costs__ – Operational costs are the costs of running your infrastructure. They include the unit price of infrastructure, matching supply and demand, investment risk for new applications, markets, and ventures, employing an elastic cost base, and building transparency into the IT operating model.*
- *__Workforce Productivity__ – Workforce productivity is how efficiently you are able to get your services to market. You can quickly provision AWS services, which increases your productivity by letting you focus on the things that make your business different; rather than spending time on the things that don’t, like managing data centers. With over 90 services at your disposal, you eliminate the need to build and maintain these independently. We see workforce productivity improvements of 30%-50% following a large migration.*
- *__Cost Avoidance__ – Cost avoidance is setting up an environment that does not create unnecessary costs. Eliminating the need for hardware refresh and maintenance programs is a key contributor to cost avoidance. Customers tell us they are not interested in the cost and effort required to execute a big refresh cycle or data center renewal and are accelerating their move to the cloud as a result.*
- *__Operational Resilience__ – Operational resilience is reducing your organization’s risk profile and the cost of risk mitigation. With 16 Regions comprising 42 Availability Zones (AZs) as of June 2017, With AWS, you can deploy your applications in multiple regions around the world, which improves your uptime and reduces your risk-related costs. After migrating to AWS, our customers have seen improvements in application performance, better security, and reduction in high-severity incidents.For example, GE Oil & Gas saw a 98% reduction in P1/P0 incidents with improved application performance.*
- *__Business Agility__ – Business agility is the ability to react quickly to changing market conditions. Migrating to the AWS Cloud helps increase your overall operational agility. You can expand into new markets, take products to market quickly, and acquire assets that offer a competitive advantage. You also have the flexibility to speed up divestiture or acquisition of lines of business. Operational speed, standardization, and flexibility develop when you use DevOps models automation, monitoring, and auto-recovery or high-availability capabilities.*
### Business Drivers

### Migration - Strategies

- **Re-Host**: "Lift and Shift"; chuyển assets từ on-prem lên cloud mà không thay đổi gì hết. Effort: * * , Khả năng(cơ hội) tối ưu hóa: * *  \ Ex: Chuyển on-prem MySQL DB lên EC2 Instance.
- **Re-Platform**: "Lift and Reshape"; Chuyển assets và thay đổi underlying platform. Effort: * * * * , Khả năng(cơ hội) tối ưu hóa: * * *  \ Ex: Migrate on-prem MySQL DB đên RDS MySQL.
- **Re-Purchase**: "Drop and Shop"; Bỏ hệ thống đang chạy trên on-prem, tạo mới hoàn toàn trên cloud(*Move from perpetual licenses to a software-as-a-service model*). Effort: * * * , Khả năng(cơ hội) tối ưu hóa: *  \ Ex: Migrate legacy on-prem CRM system to salesforece.com
- **Rearchitect**: Design lại apps với cloud-native manner. Effort: * * * * * , Khả năng(cơ hội) tối ưu hóa: * * * * *  \ Ex: Tạo một serverless version của application.
- **Retire**: Đánh giá những app không còn cần nữa(*Remove applications that are no longer needed*). Effort: - , Khả năng(cơ hội) tối ưu hóa: - \ Ex: ...
- **Retain**: "Do nothing option"; Quyết định sẽ đánh giá lại trong tương lai. Effort: * , Khả năng(cơ hội) tối ưu hóa: - \ Ex: ...
- Tham khảo thêm: [6 Strategies for Migrating Applications to the Cloud](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/)

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/migration/CompareMigrationStrategies.png" alt="Compare Migration Strategies">
</p>

### Cloud Adoption Framework
#### 1. The Open Group Architecture Framework(TOGAF)
- Tự tìm hiểu sau
#### 2. AWS Cloud Adoption Framework
- *Cloud adoption requires that fundamental changes are discussed and considered across the entire organization, and that stakeholders across all organizational units—both outside and within IT—support these changes. The AWS Cloud Adoption Framework (AWS CAF) provides guidance that supports each unit in your organization so that each area understands how to update skills, adapt existing processes, and introduce new processes to take maximum advantage of the services provided by cloud computing.*

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/migration/AwsCAF.png" alt="AWS CAF">
</p>

- *In general, the Business, People, and Governance Perspectives focus on business capabilities, and the Platform, Security, and Operations Perspectives focus on technical capabilities.*

### Hybrid Architecture
- Hybrid Architecture kết hợp cloud resources và on-premises resources.
- Bước đầu tiên để migrate on-premise lên cloud.
- Infrastructure có thể gia tăng hoặc mở rộng on-prem platforms. ví dụ như VMWare.
- Về lý thuyết, integrations có kết nối không chặt chẽ - nghĩa là on-prem hoặc cloud có thể tồn tại mà không phụ thuộc quá nhiều vào phía còn lại.
- Thông tin tìm hiểu thêm: [Hybrid Cloud use cases](https://aws.amazon.com/hybrid/use-cases/)

### Migration Tools
#### 1. Storage Migration
##### AWS Storage Gateway
- Cầu nối giữa on-prem data và cloud data trong S3.
- Use cases: disaster recovery, backup & restore, tiered storage
- 3 loại Storage Gateway:
###### File Gateway
- Là một máy ảo cầu nối giữa NFS và S3
- Metadata and directory structure are preserved ????????
- Config để có thể access được S3 thông qua NFS và SMB protocol
- Mỗi File Gateway nên có một IAM role để access S3.
- Hầu hết "recently used data" được cache ở File Gateway
- Có thể mount trên nhiều server.
- Tham khảo thêm: [File Gateway for Hybrid Architecture](https://d0.awsstatic.com/whitepapers/aws-storage-gateway-file-gateway-for-hybrid-architectures.pdf)

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/migration/FileGateway.png" alt="File Gateway">
    <br/>
    <a>File Gateway</a>
</p>

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/migration/FileGatewayExtensions.png" alt="File Gateway: Extentions">
    <br/>
    <a>File Gateway: Extensions</a>
</p>

###### Volume Gateway
###### Tape Gateway
##### AWS Snowball
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