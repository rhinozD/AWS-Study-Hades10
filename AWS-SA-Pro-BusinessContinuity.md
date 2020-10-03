# AWS-SA-Pro-Notes
Ghi chú lại kiến thức đã học trong quá trình luyện thi chứng chỉ AWS Solution Architect Professional Certificate.

## Tuần 4 - Business Continuity

### Architecture Pillars and Principles
- Business Continuity and Disaster Recovery
    - Resiliency(Khả năng phục hồi) and fault Tolerance(Khả năng chịu lỗi)
    - Redundancy(Khả năng dự phòng, đáp ứng trường hợp lỗi xảy ra), and High Availability(Tính khả dụng cao).
- Cost
- Performance
- Security
- Monitoring
- Scalability and Elasticity
- Ease of Deployment
- Migration and Hybrid architectures

### Building Fault Tolerant Apps on AWS
1. Amazon Machine Image (AMI)
AMI hiểu đơn giản là một software configuration được apply vào EC2 instance. Nó bao gồm: Operating system, Application server, Applications
- Bước đầu tiên để build fault-tolerant applications trên AWS là tạo ra một library AMIs(có thể là custom AMIs)
- Should: Tạo ít nhất một custom AMI để sử dụng cho application
    - Khi đã có pre-baked AMI, việc launching application trở nên đơn giản hơn.
    - Ngoài ra còn cho phép việc recover khi gặp lỗi nhanh chóng hơn.
        - Nếu như một instance fails, hoặc chạy một cách bất thường, có thể launch một instnace thay thế khác dựa trên template có sẵn.

To minimize downtime, you might even always keep a spare instance running – ready to take over in the event of a failure
    – This can be done efficiently using elastic IP addresses.
    – You can easily fail over to a replacement instance or spare running instance by remapping your elastic IP address to the new instance (Floating IP)
- Being able to quickly launch replacement instances based on an AMI that you define is a critical first step towards fault tolerance.


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


<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/migration/FileGatewayReadOnlyReplicas.png" alt="File Gateway: Read Only Replicas">
    <br/>
    <a>File Gateway: Read Only Replicas</a>
</p>


<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/migration/FileGatewayBackupAndLifeCycle.png" alt="File Gateway: Backup And LifeCycle">
    <br/>
    <a>File Gateway: Backup And LifeCycle</a>
</p>

- File Architectures: Những khả năng khác
    - Amazon S3 Object Versioning
        - Khả năng lưu trữ nhiều object versions khi chúng được sửa đổi.
        - Có thể restore một file về version trước đó.
        - Có thể restore toàn bộ file system về phiên bản trước đó.
        - Phải sử dụng RefreshCache API trên Gateway để có thể notify việc restore.
    - Amazon S3 Object Lock
        - Cho phép File Gateway có chức năng: Write Once Read Many (WORM)
        - Nếu có file được sửa hoặc đổi tên trên file share clients, file gateway tạo một version mới của object mà không ảnh hưởng đến versions trước, và version đã được lock đầu tiền sẽ luôn luôn không đổi.
###### Volume Gateway
- Block storage using iSCSI protocol backed by S3
- Cached volumes: low latency access to most recent data, full data on S3
- Stored volumes: entire dataset is on premise, scheduled backups to S3
- Can create EBS snapshots from the volumes and restore as EBS!
- Up to 32 volumes per gateway
    - Each volume up to 32TB in cached mode (1PB per Gateway)
    - Each volume up to 16 TB in stored mode (512TB per Gateway)

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/migration/VolumeGateway.png" alt="Volume Gateway">
    <br/>
    <a>Volume Gateway</a>
</p>

###### Tape Gateway
- Some companies have backup processes using physical tapes (!)
- With Tape Gateway, companies use the same processes but in the cloud
- Virtual Tape Library (VTL) backed by Amazon S3 and Glacier
- Back up data using existing tape-based processes (and iSCSI interface)
- Works with leading backup software vendors
- You can’t access single file within tapes. You need to restore the tape entirely

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/migration/TapeGateway.png" alt="Tape Gateway">
    <br/>
    <a>Tape Gateway</a>
</p>

##### AWS Snowball
- Physical data transport solution that helps moving TBs or PBs of data in or out of AWS
- Alternative to moving data over the network (and paying network fees)
- Secure, tamper resistant, uses KMS 256 bit encryption
- Tracking using SNS and text messages. E-ink shipping label
- Snowball size: 50TB and 80TB
- Use cases: large data cloud migrations, DC decommission(ngừng hoạt động), disaster recovery
- If it takes more than a week to transfer over the network, use Snowball devices!
- Snowball Process
    - Request snowball devices from the AWS console for delivery
    - Install the snowball client on your servers
    - Connect the snowball to your servers and copy files using the client
    - Ship back the device when you’re done (goes to the right AWS facility)
    - Data will be loaded into an S3 bucket
    - Snowball is completely wiped
    - Tracking is done using SNS, text messages and the AWS console

##### AWS Snowball Edge
- Snowball Edges add computational capability to the device
- 100 TB capacity with either: 
    - Storage optimized – 24 vCPU 
    - Compute optimized – 52 vCPU & optional GPU
- Supports a custom EC2 AMI so you can perform processing on the go
- Supports custom Lambda functions
- Very useful to pre-process the data while moving
- Use case: data migration, image collation, IoT capture, machine learning
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

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/migration/DMSSourceAndTarget.png" alt="DMS">
    <br/>
    <a>DMS Source and Target</a>
</p>

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