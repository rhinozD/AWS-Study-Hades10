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
1. __Amazon Machine Image (AMI)__
> AMI hiểu đơn giản là một software configuration được apply vào EC2 instance. Nó bao gồm: Operating system, Application server, Applications
+ Bước đầu tiên để build fault-tolerant applications trên AWS là tạo ra một library AMIs(có thể là custom AMIs)
+ Should: Tạo ít nhất một custom AMI để sử dụng cho application
- Khi đã có pre-baked AMI, việc launching application trở nên đơn giản hơn.
- Ngoài ra còn cho phép việc recover khi gặp lỗi nhanh chóng hơn.
    - Nếu như một instance fails, hoặc chạy một cách bất thường, có thể launch một instnace thay thế khác dựa trên template có sẵn.

> Để giảm thiểu downtime, user có thể luôn giữ một instance running dự phòng - sẵn sàng để chuyển qua khi có sự cố xảy ra.
+ Sử dung EIP: Có thể attach EIP nhanh.
+ User dễ dàng failover sang instance thay thế hoặc instance dự phòng bằng cách remapping EIP đến instance mới đó(Floating IP)
- Có thể nhanh chóng khởi chạy các instance thay thế dựa trên AMI đã được định nghĩa là bước quan trọng đầu tiên hướng tới khả năng chịu lỗi(fault tolerance).

2. __Elastic Block Store (EBS)__ - Storing Persistent Data
Amazon EBS volumes có độ tin cậy cao(highly reliable), nhưng để giảm thiểu khả năng xảy ra lỗi, backups của các volume có thể được tạo bằng snapshots. 

Snapshots có thể được sử dụng để tạo các Amazon EBS volume mới, là bản sao chính xác của tập gốc tại thời điểm snapshot. 

Vì các backups represent the on-disk state of the application, nên phải cẩn thận để chuyển dữ liệu trong bộ nhớ vào đĩa trước khi bắt đầu snapshot (Dừng IO process)

> __EBS’s fault tolerance__:
- EBS volumes lưu trữ dữ liệu dự phòng(redundantly), làm cho chúng bền(durable) hơn ổ cứng thông thường.
- Một robust(mạnh mẽ) backup strategy sẽ bao gồm một khoảng thời gian (thời gian giữa các lần backup, thường là hàng ngày nhưng có thể thường xuyên hơn đối với một số ứng dụng),
    - A retention period (Khoảng thời gian lưu trữ) (dependent on the application and the business requirements for rollback), and a recovery plan.
- Snapshots được lữu trữ trong S3 đảm bảo high-durability.
- Có thể copy EBS snapshots sang regions khác để tạo EBS volumes. (Note: EBS volumes phạm vi AZ, Snapshot phạm vi region)

3. __Auto Scaling & ELB__

> __Auto Scaling__
- Auto Scaling enables you to automatically scale your Amazon EC2 capacity up or down.
- Using AS in conjunction(Kết hợp) with Cloud Watch to control termination and launching new EC2 instances.
- Since Auto Scaling will automatically detect failures and launch replacement instances,
    - If an instance is not behaving as expected (e.g., it is running with poor performance), users can simply terminate that instance and a new one will be launched.
- By using Auto Scaling, users can (and should) regularly turn their instances over to ensure that any leaks or degradation do not impact your application –
    - user can literally set expiry dates on the server instances to ensure they remain ‘fresh’

> __Elastic Load Balancing__
- Elastic Load Balancing is an AWS product that distributes incoming traffic to your application across several Amazon EC2 instances.
- When you use Elastic Load Balancing, you are given a DNS host name – any requests sent to this host name are delegated to a pool of Amazon EC2 instances.
- Elastic Load balancing is bound to a region
- Elastic Load Balancing detects unhealthy instances within its pool of Amazon EC2 instances and automatically reroutes traffic to healthy instances, until the unhealthy instances have been restored.

Bài toán:

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/businesscontinuity/baitoan.png" alt="bai toan">
    <br/>
</p>

4. __Regions and Availability Zones__

> __Regions and Availability Zones__
- Another key element to achieving greater fault tolerance is to distribute your application geographically.
- Amazon Web Services are available in geographic Regions.
- Regions consist of one or more Availability Zones, are geographically dispersed(phân tán), and are in separate geographic areas or countries.
- Availability Zones are distinct locations that are engineered to be insulated(tách) from failures in other Availability Zones
- They provide inexpensive, low latency network connectivity to other Availability Zones in the same Region.
- By launching instances in separate Availability Zones, you can protect your applications from a failure (ít khi xảy ra) that affects an entire zone.

> __Multi AZ Architectures__
- Within an AWS region, the desired goal is to have an independent copy of each application stack in two or more Availability Zones.
- Use redundant(dự phòng) instances for each tier (e.g. web,application, and database) of an application could be placed in distinct Availability Zones thereby creating a multi-site solution.
- To achieve even more fault tolerance with less manual intervention(sự can thiệp), you can use Elastic Load Balancing.
- Auto Scaling can work across multiple Availability Zones in an AWS Region,
    - This makes it easier to automate increasing and decreasing of capacity.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/businesscontinuity/multiaz.png" alt="multi AZ">
    <br/>
</p>

- Elastic Load Balancing can detect the health of Amazon EC2 instances.
    - When it detects unhealthy Amazon EC2 instances, it no longer routes traffic to those unhealthy instances.
- Instead, it spreads the load across the remaining healthy instances.
- If all of your Amazon EC2 instances in a particular Availability Zone are unhealthy, but you have set up instances in multiple Availability Zones, Elastic Load Balancing will route traffic to your healthy Amazon EC2 instances in those other zones.
- This multi-site solution is highly available, and by design will cope with individual component or even Availability Zone failures.
- You can also use Route53 to build fault tolerance applications across AWS Regions for higher availability and Disaster Recovery

5. __Simple Storage Service(S3)__
- Amazon Simple Storage Service (Amazon S3) is a deceptively simple(đơn giản) web service that provides highly durable, fault-tolerant data/object storage.
- Amazon Web Services is responsible for maintaining availability and fault-tolerance; you simply pay for the storage that you use.
- Behind the scenes, Amazon S3 stores objects redundantly on multiple devices across multiple facilities in an Amazon S3 Region –
    - This caters(cung cấp, phục vụ) for the case of a failure in an Amazon Web Service data center, where data will still be accessible.
- Amazon S3 is ideal for any kind of object data storage requirements that your application might have.
- Amazon S3's Versioning feature allows you to retain prior versions of objects stored in S3
    - Versioning also protects against accidental deletions initiated by a misbehaving application. Versioning can be enabled for any of your S3 buckets.
- By using Amazon S3, you can delegate the responsibility of one critical aspect of fault tolerance – data storage – to Amazon Web Services.(AWS lo, chỉ cần user setting)

6. __Relational DB Service__
> __RDS__
- In the context of building fault-tolerant and highly available applications, Amazon RDS offers several features to enhance(nâng cao) the reliability of critical databases.
    - Automated backups:
        - Of your database enable point-in-time recovery for your database instance.
        - Amazon RDS will back up your database and transaction logs and store both for a user-specified retention period. This feature is enabled by default.
    - Manual Snapshots:
        - You can initiate snapshots of your DB Instance.
        - These full database backups will be stored by Amazon RDS until you explicitly delete them.
            - You can create a new DB Instance from a DB Snapshot whenever you desire.
- Amazon RDS also supports a Multi-AZ deployment feature.
    - If this is enabled, a synchronous standby replica of your database is provisioned in a different Availability Zone.
- Updates to your DB Instance are synchronously replicated across Availability Zones to the standby in order to keep both databases in sync.
- In case of a failover scenario, the standby is promoted to be the primary and will handle your database operations.
- Running your DB Instance as a Multi-AZ deployment safeguards your data in the unlikely event of a DB Instance component failure or service health disruption in one Availability Zone.(Multi-AZ giúp bảo vệ dữ liệu kho có lỗi hoặc AZ chết).

### Using AWS for Disaster Recovery (DR)
- Any event that has a negative impact on a company’s business continuity or finances could be termed a disaster. This includes:
    - Hardware or software failure, A network outage, A power outage,
    - Physical damage to a building like fire, earthquakes, hurricanes, or flooding,
    - Human error
- Disaster recovery (DR) is all about preparing for and recovering from a disaster.
- Question to be answered:
    - What are the best practices to improve the DR processes, from minimal investments(đầu tư) to full-scale availability and fault tolerance, and
    - How to use AWS services to reduce cost and ensure business continuity during a DR event.

1. __RTO and RPO__
- Recovery time objective (RTO) —
    - The time it takes after a disruption to restore a business process to its service level, as defined by the operational level agreement (OLA).
    - For example, if a disaster occurs at 12:00 PM (noon) and the RTO is eight hours, the DR process should restore the business process to the acceptable service level by 8:00 PM.
- Recovery point objective (RPO) —
    - The acceptable amount of data loss measured in time.
    - For example, if a disaster occurs at 12:00 PM (noon) and the RPO is one hour, the system should recover all data that was in the system before 11:00 AM.
        - Data loss will span only one hour, between 11:00 AM and 12:00 PM (noon).
- A company typically decides on an acceptable RTO and RPO based on the financial impact to the business when systems are unavailable.

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/businesscontinuity/rporto.jpg" alt="RTO RPO">
    <br/>
</p>

2. __AWS Services for Backup and DR__
- __Elastic Compute Cloud (Amazon EC2)__
    - Within minutes, you can create Amazon EC2 instances, which are virtual machines over which you have complete control.
- In the context of DR, the ability to rapidly create virtual machines that you can control is critical.
- Amazon Machine Images (AMIs) are preconfigured with operating systems, and some preconfigured AMIs might also include application stacks.
- In the context of DR, AWS strongly recommends that you configure and identify your own AMIs so that they can launch as part of your recovery procedure.
    - Such AMIs should be preconfigured with your operating system of choice plus appropriate pieces of the application stack.
    - You can copy your AMIs to other regions for DR purposes
- __Storage__
    - __Simple Storage Service (S3)__:
        - Provides a highly durable storage infrastructure designed for mission-critical and primary data storage.
        - Objects are redundantly stored on multiple devices across multiple facilities within a region, designed to provide a durability of 99.999999999%.
        - AWS provides further protection for data retention and archiving through versioning in Amazon S3, AWS multi-factor authentication (AWS MFA), bucket policies, and AWS IAM
    - __Glacier__:
        - Provides extremely low-cost storage for data archiving and backup. Objects (or archives, as they are known in Amazon Glacier) are optimized for infrequent access, for which retrieval times of several hours are adequate.
        - Amazon Glacier is designed for the same durability as Amazon S3.
- __AWS Elastic Block Store (EBS)__:
    - Provides the ability to create point-in-time snapshots of data volumes.
    - You can use the snapshots as the starting point for new Amazon EBS volumes.
    - You can protect your data for long-term durability because snapshots are stored within Amazon S3.
    - Amazon EBS volumes provide off-instance storage that persists independently from the life of an instance
        - It is replicated across multiple servers in an Availability Zone to prevent the loss of data from the failure of any single component. (được replica trên nhiều servers trong AZ để đảm bảo không mất data)

3. __AWS Storage Gateway__
- AWS Storage Gateway supports three storage interfaces (or Storage Configurations): file gateway, volume gateway , and tape gateway.
    - Each gateway you have can provide one type of interface.
- The volume gateway provides block storage to your applications using the iSCSI protocol.
    - Data on the volumes is stored in Amazon S3.
        - To access your iSCSI volumes in AWS, you can take EBS snapshots which can be used to create EBS volumes.
- The tape gateway provides your backup application with an iSCSI virtual tape library (VTL) interface, consisting of a virtual media changer, virtual tape drives, and virtual tapes.
    - Virtual tape data is stored in Amazon S3 or can be archived to Amazon Glacier.

4. __Services for Backup and DR__
- __Snowball__ : Used to transfer Terabytes to Petabytes of data in and out of AWS
    - As a rule of thumb, if it takes more than one week to upload your data to AWS using the spare capacity of your existing Internet connection, then you should consider using Snowball.
    - Comes in three flavors, Snowball, Snowball Edge, and Snowmobile

5. __AWS VM Import/Export__
- VM Import/Export enables you to easily import virtual machine images from your existing environment to Amazon EC2 instances.
- You can also export the imported instances back to your on-premises virtualization infrastructure, allowing you to deploy workloads across your IT infrastructure.
- VM Import/Export is available at no additional charge beyond standard usage charges for Amazon EC2 and Amazon S3.

6. __Networking__
- When you are dealing with a disaster, it’s very likely that you will have to modify network settings as your system is failing over to another site.
- The following AWS services and features enable you to manage and modify network settings.
    - Amazon Route 53
        - It gives developers and businesses a reliable, cost-effective way to route users to Internet applications.
        - Amazon Route 53 includes a number of global load-balancing capabilities (which can be effective when you are dealing with DR scenarios such as DNS endpoint health checks) and,
        - The ability to failover between multiple endpoints and even static websites hosted in Amazon S3.
    - Elastic IP addresses
        - Elastic IP addresses enable you to mask instance or Availability Zone failures by programmatically remapping your public IP addresses to instances in your account in a particular region.
        - For DR, you can also pre-allocate some IP addresses for the most critical systems so that their IP addresses are already known before disaster strikes.
    - Elastic Load Balancing
        - Just as you can pre-allocate Elastic IP addresses, you can pre-allocate your load balancer so that its DNS name is already known, which can simplify the execution of your DR plan.
    - Amazon Virtual Private Cloud (Amazon VPC)
        - In the context of DR, you can use Amazon VPC to extend your existing network topology to the cloud;(Kêt hợp giữa on-pre và cloud)
        - This can be especially appropriate when recovering enterprise applications that are typically on the internal network.
    - Amazon Direct Connect makes it easy to set up a dedicated network connection from your premises to AWS.
        - This can reduce your network costs, increase bandwidth throughput, and provide a more consistent network experience than Internet-based connections.

7. __Database__
- Amazon Relational Database Service (__Amazon RDS__)
    - You can use Amazon RDS either in the preparation phase for DR to hold your critical data in a database that is already running, or in the recovery phase to run your production database.
    - When you want to look at multiple regions, Amazon RDS gives you the ability to snapshot data from one region to another, and also to have a read replica running in another region.
- __Amazon DynamoDB__
    - You can also use it in the preparation phase to copy data to DynamoDB in another region or to Amazon S3.
    - During the recovery phase of DR, you can scale up seamlessly in a matter of minutes with a single click or API call.
    - You can also benefit from Global Tables (Cross Region Replication)
- __Amazon Redshift__
    - You can use Amazon Redshift in the preparation phase to snapshot your data warehouse to be durably stored in Amazon S3 within the same region or copied to another region.
    - During the recovery phase of DR, you can quickly restore your data warehouse into the same region or within another AWS region.

8. __DR Approaches/Strategies__
- Various approaches to DR:
    - Backup and Restore
    - Pilot Light
    - Warm standby
    - Multi Site

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/businesscontinuity/drstragies.jpg" alt="DR Approaches">
    <br/>
</p>

<p align="center"> 
    <img src="https://github.com/sadsun92/AWS-Study-Hades10/blob/master/resources/images/businesscontinuity/drstragiessolution.jpg" alt="DR Approaches Spectrum Solutions">
    <br/>
</p>

9. __Backup and Restore__
Key steps for backup and restore:
- Select an appropriate tool or method to back up your data into AWS.
- Ensure that you have an appropriate retention(lưu trữ) policy for this data.
- Ensure that appropriate security measures are in place for this data, including encryption and access policies.
- Regularly test the recovery of this data and the restoration of your system.

10. __Replication Methods and Self Healing__

__Data Replication__

- Many database systems support asynchronous data replication.
- The database replica can be located remotely, and the replica does not have to be completely synchronized with the primary database server.
    - This is acceptable in many scenarios, for example, as a backup source or reporting/read-only use cases.
    - In addition to database systems, you can also extend it to network file systems and data volumes.

__Self Healing__

- SQS to decouple
- CW and Auto Scaling terminating unhealthy instance
- Auto Scaling creating replacement EC2 instances to replace those terminated
- Amazon S3 also performs regular, systematic data integrity checks(tính toàn vẹn dữ liệu) and is built to be automatically self-healing.
- Amazon Glacier performs regular, systematic data integrity checks and is built to be automatically self-healing.