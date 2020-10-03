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

2. Elastic Block Store (EBS) - Storing Persistent Data
Amazon EBS volumes có độ tin cậy cao(highly reliable), nhưng để giảm thiểu khả năng xảy ra lỗi, backups của các volume có thể được tạo bằng snapshots. 

Snapshots có thể được sử dụng để tạo các Amazon EBS volume mới, là bản sao chính xác của tập gốc tại thời điểm snapshot. 

Vì các backups represent the on-disk state of the application, nên phải cẩn thận để chuyển dữ liệu trong bộ nhớ vào đĩa trước khi bắt đầu snapshot (Dừng IO process)

> __EBS’s fault tolerance__:
- EBS volumes lưu trữ dữ liệu dự phòng(redundantly), làm cho chúng bền(durable) hơn ổ cứng thông thường.
- Một robust(mạnh mẽ) backup strategy sẽ bao gồm một khoảng thời gian (thời gian giữa các lần backup, thường là hàng ngày nhưng có thể thường xuyên hơn đối với một số ứng dụng),
    - A retention period (Khoảng thời gian lưu trữ) (dependent on the application and the business requirements for rollback), and a recovery plan.
- Snapshots được lữu trữ trong S3 đảm bảo high-durability.
- Có thể copy EBS snapshots sang regions khác để tạo EBS volumes. (Note: EBS volumes phạm vi AZ, Snapshot phạm vi region)

3. Auto Scaling & ELB

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

4. Regions and Availability Zones

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

5. Simple Storage Service(S3)
- Amazon Simple Storage Service (Amazon S3) is a deceptively simple(đơn giản) web service that provides highly durable, fault-tolerant data/object storage.
- Amazon Web Services is responsible for maintaining availability and fault-tolerance; you simply pay for the storage that you use.
- Behind the scenes, Amazon S3 stores objects redundantly on multiple devices across multiple facilities in an Amazon S3 Region –
    - This caters(cung cấp, phục vụ) for the case of a failure in an Amazon Web Service data center, where data will still be accessible.
- Amazon S3 is ideal for any kind of object data storage requirements that your application might have.
- Amazon S3's Versioning feature allows you to retain prior versions of objects stored in S3
    - Versioning also protects against accidental deletions initiated by a misbehaving application. Versioning can be enabled for any of your S3 buckets.
- By using Amazon S3, you can delegate the responsibility of one critical aspect of fault tolerance – data storage – to Amazon Web Services.(AWS lo, chỉ cần user setting)

6. Relational DB Service
> __RDS__