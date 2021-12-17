---
layout: post
title: All you need to know for AWS Solutions Architect - Associate Exam
---

Hi folks, today I'm going to share my personal notes when I was studying for AWS Solutions Architect - Associate Exam. Without further ado, let's start!

---

## Exam Anatomy

First things first, you have to know and understand how to set your expectation about the "enemy"

Exam structure:
- Multiple choice (1 correct out of 4)
- Multiple response (2 or more correct out of 5)
- 15 unscored questions
- Total **65 questions**
- Most of the questions are story-based question (e.g. "You are a solution architect which is trying to migrate a company on-premise NFS to the AWS cloud. What is the right step to do?")

For this exam, the minimum passing score is **720** with each question weighing 15-16 points, depending on [the exam difficulty](https://aws.amazon.com/blogs/training-and-certification/demystifying-your-aws-certification-exam-score/)

There is **no penalty** whatsoever for wrong answers, so make sure you answer all questions

### Domains*
1. Design Resilient Architectures (30%)
2. Design High-Performing Architectures (28%)
3. Design Secure Applications and Architectures (24%)
4. Design Cost-Optimized Architectures (18%)

Total: 100%

  *How they define, weight, and split questions between domain can change overtime, this breakdowns still relevant as per 2021

- - - -
## Study Material

All study material are compiled from AWS courses provided by AWS Partner Network, whitepapers, various sources on the internet, and Quizlets

### AWS Global Infrastructure
Choosing regions:
  - latency
  - pricing
  - service availability
  - compliance

### Security and the AWS Shared Responsibility Model

- AWS: Security of the cloud
- Customer: Security in the cloud

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/1.png" alt="shared-responsibility" width="800px">

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/2.png" alt="data-state" width="800px">

### Best Practices on Role-Based Access Control
1. Lock down the AWS root user
2. Follow the principle of least privilege
3. Use IAM appropriately
4. Use IAM roles when possible
5. Consider using an identity provider
6. Consider AWS SSO

### Resource Tagging
- Maximum key length = 128 unicode characters
- Maximum value length = 255 unicode characters
- `aws:` tag prefix is reserver for AWS, we can’t add, edit, or delete tags with this prefix.

- - - -
## Compute as a Service
Types of compute options
- Virtual Machine (VMs)
  - Elastic Compute Cloud (EC2)
  	- By default, when you create an EC2 account with Amazon, your account is limited to a maximum of **20 instances per EC2 region** with two default High I/O Instances (hi1. 4xlarge) (not availability zone).
- Container Service
  - ECS —> EC2, Fargate
  - EKS —> EC2, Fargate
- Serverless (Lambda)

### Amazon EC2 instance Types
When choosing instance consider vCPU, Memory, Instance Storage, Network Bandwidth (Net In/Out), EBS Bandwidth (IOPS)

**c5.large** —> c5 = instance family, large = amount of instance capacity

### Architect for high availability

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/3.png" alt="ha-setting" width="800px">

### EC2 instance lifecycle

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/4.png" alt="ec2-instace-lifecycle" width="800px">
- Billing begins when the instance is `running` 
- When an instance is `rebooted`, it will lose any data on instance store because it will spawn on a new underlying physical server
- When an instance is `stopped`, the instance gets a new public IP address but maintains the same private IP address
- If an instance is terminated, we will lose instance store, public IP address, and private IP address

### Difference between stop and stop-hibernate
- When an instance is `stopped`, AWS does not charge sage or data transfer. But it charge the EBS volume provisioned
- When instance is `stopped`, the data stored in memory (RAM) is lost
- When instance is `stop-hibernated` , AWS will signal the OS to perform hibernation (suspend-to-disk) which saves contents from RAM to EBS root volume

### Reserve Instance
- Discount amount for RIs
	- Normal On-demand < No Upfront < Partial Upfront < All Upfront
	- Commitment option 1 year and 3 year term
	- Cannot transfer region but can transfer AZ

### Spot Instance
- Architectural considerations for Spot Instance
	- Spot Instance might be interrupted, so application has to be able to withstand sudden interruption.
	- Spot Instance pricing:
		- If your Spot instance is terminated or stopped by Amazon EC2 in the first instance hour, you will not be charged for that usage.
		- However, if you stop or terminate the Spot instance yourself, you will be charged to the nearest second. 
		- If the Spot instance is terminated or stopped by Amazon EC2 in any subsequent hour, you will be charged for your usage to the nearest second.
		- If you are running on Windows or Red Hat Enterprise Linux (RHEL) and you stop or terminate the Spot instance yourself, you will be charged for an entire hour.

### Choosing compute service
- EC2 if does not need refactoring, but still need scaling
- Lambda if automation is important and function is short-running
- EKS or ECS supports micro-services or service-oriented design. Containers boot up quicker and helps with code portability

- - - -
## Networking
### Amazon VPC
- Region
- IP range (in CIDR notation). Each VPC can have up to 5 /16 IPv4 ranges and 1 IPv6 block
- You can have 5 VPCs per region

### Subnet
- VPC where subnet lives
- Availability Zone
- CIDR block for subnet within the VPC range
- You can have 200 subnet per VPC

For high availability utilise Multi-AZ architecture on each subnet

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/5.png" alt="subnetting-example" width="800px">

### Amazon VPC Routing

### Main route table
- Created when VPC is created
- Governs traffic of the VPC, specifically subnets within it

### Custom route table
- Governs traffic per-subnet basis
- By default a subnet will inherit main route table
- If a custom route table associate with a subnet, it will use the custom route table instead 

### Amazon VPC Security

### Network ACL (NACL)
- Firewall at subnet level
- By default allow incoming and outgoing
- Stateless (always check packets)
- **Can create block / deny rules**

### Security Group
- Firewall at instance level (EC2)
- By default deny incoming and allow outgoing
- Stateful (remember allowed packets)
- Default VPC security group limit: 500
- Can not create block / deny rules

- - - -
## AWS Storage Types
- EBS
  - Optimised for low-latency operations
  - Uptime SLA 99.99% like EC2
  - Use cases:
  	- high-performance enterprise workloads
  	- Email servers
  	- RAID arrays
  	- Databases (with transactional read/write)
  	- Enterprise resource planning (ERP)
  	- throughput-intensive applications
- S3 (object storage)
  - An object storage, store data in a flat structure
  - Use cases:
  	- Backup and storage
  	- Media hosting
  	- Software delivery
  	- Data lakes
  	- Static websites
  	- Statice content
- EFS (elastic file system)
  - Must be used with AWS compute services (EC2, containers, serverless)
  - Use cases:
  	- Large content repositories
  	- Development environments
  	- User home directories
- EC2 instance store
  - locally attached block storage —> ideal for stateless data processing with frequent access
  	- Hadoop clusters
  	- buffers
  	- caches
  	- scratch data
  	- other temporary content

### Amazon EBS
- can only be connected with 1 computer at a time. EBS volumes have 1-1 relationship with EC2 instances at one time. (Recently, AWS introduces multi-attach EBS, but only for limited storage types and they have to be in the same AZ)
- can be detached from an EC2 instance and attach it to another EC2 instance in the same AZ
- EBS is living separately than EC2, if anything happens to EC2, the data still persist
- An EBS volume has max size of 16TB and once it’s increased it cannot be decreased
- EC2 has a 1-to-many relationship with EBS volume. Multiple EBS can be attached to an EC2 instance, but not the other way around.

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/6.png" alt="ebs-table" width="800px">
- ***Update:** SSD vol size is 4GB - 64 TB, HDD vol size is 125 GB - 16 TB
- HDD has good throughput and have lower IOPS. Good for sequential access
- SSD has better IOPS. Good for random access

### Amazon EBS Snapshots
- EBS backups that incrementally save the blocks and only save the recent changes after the last snapshot. E.g we have 10 GB data on a volume, and there’s 2GB modified data after last snapshot, the new snapshot will only store that 2 GB
- Backups are stored redundantly in multiple AZ using S3
- Snapshot can be used to create new volumes, no matter in what AZ they are

### Amazon S3
- Has a strong consistency model for no objects, and eventual consistency for updates
- Stores data as an object with unique identifier. However, it is displayed as a file system to give a sense of organisation
- Access can be controlled by **IAM policy (when you have many buckets with different permission requirements or maintains all policy information in one location**) or **Bucket Policy (when bucket is accessed by multiple AWS accounts or IAM policy limit is reached)**
- Highly redundant and reliable
- Enforces encryption for data in transit. Server-side encryption and client-side encryption for data at rest.
- Version-enabled bucket does not delete object, it can be restored
- Amazon S3 Storage Classes
  - **Amazon S3 Standard Storage:** general purpose
  	- Higher availability requirements: Use multi-AZ replication
  - **Amazon S3 Intelligent-Tiering:** data has unknown or changing patterns, will store your data to frequent and infrequent access tier according to usage
  - **Amazon S3 Standard Infrequent Access (S3 Standard-IA):** for less frequently access data but need rapid access when needed (e.g. backups, disaster recovery files, etc)
  	- Lower cost per GB stored
  	- Higher cost per PUT, COPY, POST, or GET request
  	- 30-day storage minimum
  - **Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA):** Same as Standard-IA, except it only stores data on 1 AZ instead of multi-AZ.
  - **Amazon S3 Glacier:** storage for data archiving,  retrieval takes from few minutes to hours
  	- Retrieval types:
  		- Expedited - more expensive, ~5 minutes
  		- Standard
  		- Bulk - cheapest, can take up 12 hours
  	- Encrypts data by default
  - **Amazon S3 Glacier Deep Archive:** lowest cost. Supports long term retention and digital preservation of data. Designed for customers in **highly regulated industries** such as financial services, healthcare, and public sectors that retain data sets for 7-10 years or longer to meet regulatory compliance. Retrieval can take hours to a couple of days
- Automate tier transition with object lifecycle policy by defining **transition actions** (define when object will be moved to another storage class) and **expiration actions** (define when object expire and should be permanently deleted)
  - Use cases:
  	- Periodic logs (log rotate)
  	- Data that changes in access frequency
- S3 pricing model
  - Pay only for what you use:
  	- storage GBs per month
  	- Transfer out of region
  	- PUT, COPY, POST, LIST, and GET request
  - Free of charge
  	- Transfer in to Amazon S3
  	- Transfer out of from Amazon S3 to Amazon CloudFront or the same region

### Amazon Elastic File System (Amazon EFS) and Amazon FSx
- Store data as a file storage with hierarchy
- Can be mounted onto multiple EC2 instances
- Don’t need provisioning
- Petabyte-scale file system

- - - -
## Databases on AWS

### Relational Databases (RDS)
- Benefits
  - Join operations
  - Reduced redundancy, can store data in multiple table using reference, instead of storing same data on different tables
  - Familiarity, most popular database type
  - Accuracy, ensure data persisted with high integrity and adhere to **ACID principle (atomicity, consistency, isolation, durability)**
- Use cases:
  - ERP applications
  - Customer relationship management (CRM) applications
  - Commerce and financial applications
- Federated Storage Engine for MySQL is **not supported**
- Supports SOAP only through HTTPS

### Managed vs Unmanaged

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/8.png" alt="unmanaged-database" width="800px">
<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/9.png" alt="managed-database" width="800px">

### DB Instances

- Supported engines:
  - Commercial: Oracle, MS SQL server
  - Open source: MySQL, PostgreSQL
  - Cloud Native: Amazon Aurora
- Instance families:
  - Standard: general purposes instances (T,M)
  - Memory optimised: for memory-intensive apps (R)
  - Burstable performance: provide base performance level with ability to burst to full CPU usage (C)
- Storage types:
  - General purpose (SSD)
  - Provisioned IOPS (SSD)
  - Magnetic storage
- Backup:
  - Automated backup (0-35 days)
  - Manual snapshots (>35 days)
  - I/O Operations to the DB are suspended for a few minutes while the backup is in progress
- When to use provisioned IOPS
  - If we use production online transaction processing (OLTP) workloads
	
### AWS DB Service
- Amazon DynamoDB
  - non-relational DB (key-value pair)
  - charges based on the usage of the table
  - fit for milliseconds latency and massive scale operation
  - use SSD and replicated across multiple AZ in a region
  - encrypt data at rest
  - Core components:
  	- Tables — a table (e.g. People, Cars)
  	- Items — each table contains zero or more items (rows). Unlimited number can be stored
  	- Attributes — each item has one or more attributes (column) (e.g. FirstName, LastName)
- Amazon DocumentDB (MongoDB compatible)
  - Use case:
  	- content management
  	- catalogues
  	- user profile
- Amazon Neptune (graph database)
  - Graph database
  - Use cases:
  	- Social networking
  	- Recommendation engine
  	- Fraud detection
- Amazon QLDB (Quantum Ledger Database)
  - immutable ledger
  - Use cases:
  	- supply chain
  	- banking
  	- financial institution


<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/10.png" alt="aws-databases-service-use-cases" width="800px">

- - - -
## Monitoring, Optimization, and Serverless
### Amazon CloudWatch
- Benefits
  - Detect anomalous behaviour
  - Set alarms
  - Visualize logs and metrics
  - Take automated actions
  - Troubleshoot issues
  - Discover insights to keep your app healthy

### CloudWatch metrics
- a metric has one or more dimensions (key/value pair that is part of the metric’s identity)
- **metrics** sent for free per one data point per metric per 5 minute interval
- we can activate **detailed metrics** that incur cost but can send metric per minutes instead
- can use custom metrics, can send metrics up to per second, by sending the programmatically using PutMetricData API
- custom metrics example:
	- web page load times
	- request error rates
	- number of processes or threads on your instance
	- amount of work performed by your application
	- memory usage (an OS-level metrics not Host level metrics)

### CloudWatch Dashboard
- Visualise one or more metrics through widgets, such as graph or text
- Can pull data from different regions
- We can use another visualisation tool using metrics from cloud watch by using GetMetricData API
- Who can view for manage the dashboards can be controlled using IAM policies

### CloudWatch Logs
- Monitor, store, and access log files from apps running on EC2, lambda, and other sources
- Can be used to query and filter log data
- To export log, we must install a CloudWatch Logs agent on the instance (except lambda). An agent includes following components:
- Plug-in to the AWS CLI
- Script that initiates the process to push data to CloudWatch logs
- Cron job that ensures daemon is always running
- Terminologies:
  - **Log event:** record of activity by the app or resource being monitored, has a timestamp and an event message
  - **Log stream:** log events are grouped into log streams according to its original resource (e.g. an EC2 instance logs are group in a log stream)
  - **Log groups:** log stream are then organised into log groups. A group composed of log streams that all share the same retention and permission settings (e.g. grouping log stream from each EC2 instance)

### CloudWatch Alarms
- Automatically initiate actions based on state changes of our metrics
- An alarm has 3 possible states:
  - OK: the metric is within defined threshold
  - ALARM: the metric is outside the defined threshold
  - INSUFFICIENT_DATA: alarm just started, metric is not available or not enough data available for the metric to determine the alarm state
- - - -
## Solutions Optimization

### Replication, redirection, and high availability

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/11.png" alt="sla-cheatsheet" width="800px">

EC2
- deploy in multi-AZ
- DNS configuration that switch to available instance (not recommended, because propagation may take time)
- Load balancer

EC2 Placement Group
- Hardware configuration for EC2 placement
- Improve connectivity and latency between instances
- Does not support cross AZ or region

Types of high availability
- Active-Passive: only one of two instance available at a time. Useful for stateful applications, where they keep user’s session on the server
- Active-Active: more scalable. Works better for stateless apps

- - - -
## Traffic Routing with AWS ELB (Elastic Load Balancing)
ELB features:
- Round-robin load balancing
- Can work to balance traffic to on-premise servers
- Highly available, can be deployed across multiple AZ
- Health check
- Connection draining
- Components:
  - Rule: contain condition to decide which target group to send traffic to
  - Listener: connects a target group to a ELB, must define port (e.g. HTTP, and HTTPS). Multiple listener can be configured for a single ELB. Also known as client-side
  - Target Group: backed servers, or server-side is defined in one or more target groups. This is where we want to direct traffic to. Can be EC2 instance, lambda, or IP addresses. Health check must be configured for each target group
- Types of Load Balancer:
  - **Application Load Balancer (ALB)**
  	- Works on top of **Layer 7** OSI layer (HTTP / HTTPS)
  	- Route based on request data (e.g. URL path)
  	- Sends response directly to the client. Sends redirect from HTTP to HTTPS, can setup custom HTML page
  	- Use TLS offloading. Keep secure traffic between client and ALB, and remove TLS before sending to server (TLS offloading / TLS termination)
  	- Authenticates users, can use OpenID Connect protocol (e.g. SAML, LDAP, Microsoft AD, etc)
  	- Secured traffic using security group
  	- Uses round-robin routing algorithm
  	- Uses the least outstanding request routing algorithm, works well if target has varied processing capabilities. Measured by size or CPU utilisation. 
  		- **Outstanding request** is when a request is sent to backend server and a response hasn’t been received yet
  	- Uses sticky sessions. Useful for stateful apps, uses HTTP cookie to remember across connection which server to send traffic to
  - **Network Load Balancer**
  	- Supports TCP, UDP, and TLS protocol (HTTPS uses TCP and TLS as protocols)
  	- Uses flow hash routing algorithm (if packets has the same parameters, the packets are sent to the exact same target, if any of them different in the next packer, the request might be sent to different target). Based on:
  		- Protocol
  		- Source IP address and source port
  		- Destination IP address and destination port
  		- TCP sequence number
  	- Supports sticky sessions using IP address instead of cookie
  	- Supports TLS offloading
  	- Can handle millions of request per second. Whilst ALB can also do this, it will takes time to scale, meanwhile NLB does it instantly
  	- Supports static and elastic IP address. Useful when you can’t use DNS or require firewall for connecting to clients
  	- Preserve source IP address. With ALB, the request will display ALB IP instead of client’s, but with NLB it will display client’s.
  - **Gateway Load Balancer**


![](&&&SFLOCALFILEPATH&&&3C2C0481-A0FB-4602-8ADB-C40A09DE15FB.png)

### ELB with EC2 Auto Scaling
- ELB supports 2 types of health check:
  - establishing connection to a backend EC2 instance using TCP (marking instance as available if successful)
  - making an HTTP or HTTPS request to a webpage that you specify and validating that HTTP response code is returned
- EC2 Auto Scaling components:
- launch template or configuration: what resource should be automatically scaled
	- Stored parameters used to create EC2 instances - AMI ID, instance type, security group, EBS volumes, etc
	- Supports versioning. Helpful when we need to rollback
	- 3 ways to create launch template:
		- Use existing EC2 instance
		- Create from already existing template or previous version
		- Create from scratch
	- Alternatively we can use **launch configuration**, similar to launch template, but it does not allow versioning nor using existing EC2 instance. Launch template is more recommended
    - EC2 auto scaling group: where should the resource be deployed
    	- Define where EC2 auto scaling deploy resources
    	- Define VPC and subnets where EC2 launches
    	- Can use on-demand only, spot only, or combination of the two
    	- 3 capacity settings:
    		- Minimum: minimum number of instance run in an Auto Scaling group even if the threshold of lowering the amount of instance reached
    		- Maximum: Maximum number of instance run in an ASG, even if the threshold for adding new instance is reached
    		- Desired capacity: amount of instance that should be in your ASG, this number can only be within or equal to the minimum or maximum. EC2 AS will automatically adds or remove instance to match desired capacity number
    - Scaling policies: When should the resource be added or removed
    	- Use CloudWatch metrics and alarms
    	- 3 types of scaling policies:
    		- Simple scaling
    			- Define one action when alarm is triggered
    		- Step scaling
    			- Respond to additional alarms even when a scaling is in progress
    		- Target tracking scaling
    			- Use CloudWatch metrics instead of alarm (e.g. average CPU utilisation, average network utilisation, request count, etc)
- - - -
# Domain 1: Design Resilient Architectures
- Choose reliable / resilient storage
- Determine how to design **decoupling mechanism** using AWS services
	- Decouple from health of components
	- Decouple from identity of components
- Determine how to design a multi-tier architecture solution
	- Separate tier between apps (e.g. web servers - app servers - database)
	- Use load balancer to connect each tiers
- Determine how to design high availability and/or fault tolerant solutions
	- high availability means system is up and available but it might perform in a degraded state
	- fault tolerance means the users does not experience any impact from a fault so that the SLA is met  —> higher bar

### Test Axioms
- Expect “Single AZ” answer will never be a right answer
- Using AWS managed services should always be preferred
- Fault tolerant and high availability **are not the same thing**
- Expect that everything will fail at some point and design accordingly

### CloudFormation
- Declarative programming language for deploying AWS resource at scale
  - Define as JSON templates and converts them as infrastructure which called stacks
  - AMI IDs are different for each region
- Use templates and stacks to provision resources
- CRUD a set of resources as a single unit (stack)

- - - -
# Domain 2: Design Performant Architecture
- Choose performant storage and databases
- Apply caching to improve performance
- Design solutions for elasticity and scalability

### When to use RDS and when not to use RDS

Use RDS when:
- complex transactions or complex queries
- a medium-to-high query/write rate
- no more than a single worker node / shard
- high durability

Do not use RDS when:
- Massive read/write rates (e.g. 150 k write/second)
- Sharding
- Simple GET/PUT requests and queries
- RDBMS customisation

### DynamoDB: Provisioned Throughput
- Read capacity unit (for an item up to 4 KB in size):
  - 1 strongly consistent read per second
  - 2 eventually consistent reads per second
- Write capacity unit (for an item up to 1 KB in size)
  - 1 write per second

### Amazon CloudFront
- Use case and benefits
  - 0 TTL
- Auto scaling
	- Vertical scaling (scale up and down)
		- Change in the specifications of instances (more CPU, memory)
	- Horizontal scaling (scale in and out)
		- Change in the number of instances (add and remove instances as needed)
- Content - static and dynamic
- Origins - S3, EC2, ELB, HTTP servers
- Protect private content
- Improve security
  - AWS Shield Standard and Advanced
  - AWS WAF

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/12.png" alt="caching-cloufront" width="800px">
<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/13.png" alt="caching-elasticache" width="800px">

### Amazon ElastiCache

Memcached (simpler)
- Multithreading
- Low maintenance
- Easy horizontal scalability with auto discovery

Redis
- Support for data structures
- Persistence
- Atomic operations
- pub/sub messaging
- read replicas / failover
- cluster mode / sharded cluster

### Auto Scaling
- Launches or terminates instances
- Automatically registers new instances with load balancers
- Can launch across AZs

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/14.png" alt="elasticity-implementation" width="800px">

### Auto Scaling Components
- Auto Scaling launch configuration
  - Specifies EC2 instance size and AMI name
- Auto Scaling group
  - References the launch configuration
  - Specifies min, max, and desired size of the ASG
  - May reference an ELB
  - Health check type
- Auto scaling policy
  - Specifies how much to scale in or scale out
  - One or more may be attached to ASG

### CloudWatch Metrics
- Know what CloudWatch can monitor
  - CPU
  - Network
  - Queue Size
- Understand CloudWatch Logs
- Understand the difference between default and custom metrics

### Test Axioms
- If data is unstructured, Amazon S3 is generally the storage solution
- Use caching strategically to improve performance
- Know when and why to use Auto Scaling
- Choose the instance and database type that makes the most sense for your workload and performance need
- - - -
# Domain 3:  Specify Secure Applications and Architecture
- Determine how to secure application tiers
- Determine how to secure data
- Define the networking infrastructure for single VPC application

### Infrastructure
- Shared responsibility model
  - 2 permission types used by AWS
    - User-based
    - Resource-based

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/15.png" alt="shared-responsibility-model" width="800px">

- Principle of least privileges
  - Persons (or processes) can perform all activities they need to perform, no more
- Identities
  - AWS IAM
  	- centrally manage users and user permissions in AWS
  	- Using AWS IAM you can:
  		- Create users, group, roles, and policies
  		- Define permissions to control which AWS resources users can access
  	- IAM integrates with Microsoft Active Directory and AWS Directory Service using SAML identity federation
  	- Max IAM group membership: 10 groups
- Types of Identities on AWS
	- IAM Users: users created within the account
	- Roles: temporary identities used by EC2 instances, lambdas, and external users
	- Federation:  users with Active Directory identities or other corporate credentials have role assigned in IAM
	- Web Identity Federation: users with web identities from Amazon.com or other Open ID provider have role assigned using Security Token Service (STS)

### Computer / Network Architecture
- Virtual Private Cloud (VPC)
- Organization: subnets
	- Recommendations: use subnets to define internet accessibility
	- Public subnets: 
		- to support inbound/outbound access to the public internet, include a routing table entry to an internet gateway
	- Private subnets:
		- Do not have a routing table entry to an internet gateway
		- Not directly accessible from public internet
		- To support restricted, **outbound** only public internet access, typically use a “jump box” (NAT/proxy/bastion host)
	- Security: security group/access control lists
		
<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/16.png" alt="ebs-table" width="800px">

  - Network isolation: Internet gateways / private virtual gateways / NAT gateways
  - Traffic direction: routes
  - Design your network architecture in the cloud
    - Security
    - Routing
    - Network isolation
    - Management
    - Bastion hosts

### VPC Connections
  - Internet gateway: connect to the internet
  - Virtual private gateway: connect to VPN
  - AWS Direct connect: dedicated pipe
  - VPC peering: connect to other VPCs
  - NAT gateways: allow internet traffic from private subnets

### Data Tier
  - Data in transit
      - In and out of AWS
      - Within AWS
      - Transferring data in and out of your AWS infrastructure
      	- SSL over web
      	- VPN for IPSec
      	- IPSec over AWS Direct Connect
      	- Import / Export / Snowball
      - Data sent to AWS
      	- AWS API calls use HTTPS/SSL by default
  - Data at rest
      - Amazon S3
      	- Data stored in S3 is private by default, require AWS credentials for access
      		- Access over HTTP or HTTPS
      		- Audit of access to all objects
      		- Supports ACL and policies
      			- Buckets
      			- Prefixes (directory/folder)
      			- Objects
      - Amazon EBS
      - Server-side encryption options
      	- Amazon S3 - Managed Keys (SSE-S3)
      	- KMS-Managed Keys (SSE-KMS)
      	- Customer-Provided Keys (SSE-C)
      - Client-side encryption options
      	- KMS managed master encryption keys (CSE-KMS)
      	- Customer managed master encryption keys (CSE-C)
      - Managing your keys
      	- Key Management Service
      		- Customer software-based key management
      		- Integrated with many AWS services (EBS, S3, RDS, Redshift, Elastic Transcoder, Workmail, EMR)
      		- Use directly from application
      	- AWS CloudHSM
      		- Hardware-based key management
      		- Use directly from application
      		- FIPS 140-2 compliance

### Test Axioms
- Lock down the root user
- Security groups only allow. Network ACLs allow explicit deny
- Prefer IAM roles to access keys

- - - -
# Domain 4: Design Cost-Optimized Architectures
- Determine how to design cost-optimised storage
- Determine how to design cost-optimised compute 

AWS Pricing overview
- Pay as you go
- Pay less when you reserve
- Pay even less per unit by using more (economy of scale)
- Three fundamental characteristics you pay for with AWS:
  - compute
  - storage
  - data transfer

### Amazon EC2 Pricing
- Considerations for estimating the cost of using Amazon EC2:
  1. Clock hours of server time
  2. Machine configuration
  3. Machine purchase type
  4. Number of instances
  5. Load balancing
  6. Detailed monitoring
  7. Auto scaling
  8. Elastic IP address
  9. Operating systems and software packages
- Amazon EC2 Pricing factors
  - Instance family
  - Tenancy
  - Pricing options
  	- Reserved instances
  		- EC2 Reserved instances (RI) provide a significant discount (up to 75%) compared to on-demand pricing
  		- RI Types:
  			- Standard RIs
  			- Convertible RIs
  			- Scheduled Ris
  	- Spot instances
  		- Spot instances are spare compute capacity in the AWS Cloud available to you at steep discounts compared to on-demand prices (30-45%)

### Amazon S3 Pricing
- Considerations for estimating the cost of using Amazon EC2:
1. Storage class
	- Standard storage
	- Standard - Infrequent Access
	- Amazon Glacier
2. Storage
3. Requests
4. Data transfer

### Amazon EBS Pricing
- Considerations for estimating the cost of using Amazon EBS
  1. Volumes
  2. Input/output operations per second (IOPS)
  3. Snapshots
  4. Data transfer
- 2 types of EBS volume
  - SSD: more expensive, higher IOPS, for random access
  - HDD: cheaper, has lower IOPS, for sequential data

### Amazon CloudFront Pricing
- Considerations for estimating the cost of using Amazon CloudFront
  1. Traffic distribution
  2. Requests
  3. Data transfer out

### Test Axioms
- If you know it’s going to be on, reserve it.
- Any unused CPU time is a waste of money
- Use the most cost-effective data storage service and class
- Determine the most cost-effective EC2 pricing model and instance type for each workload

- - - -
# (Extra) Domain 5: Define Operationally-excellent Architectures
- Choose design features in solutions that enable operational excellence

### Operational Excellence
- The ability to run and monitor systems to deliver business value and continually improve supporting processes and procedures
  - Prepare
  - Operate
  - Evolve

### Operational excellence: design principles
- Perform operations with code
- Annotate documentation
- Make frequent, small, reversible changes
- Refine operations procedures frequently
- Anticipate failure
- Learn from all operational failures

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/17.png" alt="aws-operational-excellence" width="800px">

### Services that supports operational excellence
- AWS Config: tracks resources and verify resource compliance to a configuration
- AWS CloudFormation: convert JSON and YAML templates into resources
- AWS Trusted Advisor: check accounts best practices security, reliability, performance, cost, and service limits
- AWS Inspector: checks EC2 instances for security vulnerability
- AWS Flow logs: logs network traffic (layer 3 and 4 IP-level logs)
- AWS Cloud Trail: logs API calls to AWS
- AWS CloudWatch Metrics: tracks metrics and create triggers alarms when a metric is exceeded
- AWS CloudWatch Logs: stores log files from EC2 instances from Cloud Trail and Lambda. It can also turns these log files into metrics by extracting patterns from log lines

<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/18.png" alt="cloudwatch" width="800px">

### Test Axioms
- IAM roles are easier and safer than keys and password
- Monitor metrics across the system
- Automate responses to metric where appropriate
- Provide alerts for anomalous conditions

- - - -
### 6 Migration Strategies
<img src="/assets/images/2021-12-17-All-You-Need-To-Know-AWS-Solutions-Architect-Associate/19.png" alt="6-migration-strategies" width="800px">

- Rehosting (lift-and-shift)
	- The most simple migration strategy. Take your on-premise applications or system and move them exactly as is into the cloud
	- Example:
		- Move self-hosted MySQL databases to AWS EC2 instances
- Replatforming (lift-tinker-and-shift / lift-and-reshape)
	- Take advantage of cloud services to run your applications or system
	- Example:
		- Move self-hosted MySQL databases to AWS RDS instance
		- Move apps to Elastic Beanstalk
- Refactoring / Re-architecting
	- Most advanced migration strategy. Redesign of apps in a more cloud-native manner, changing the app architecture to take advantage cloud services.
	- Example:
		- Migrate on-premises Oracle DB to Amazon Aurora PSQL
		- Breakdown monolith app into micro services and host them on Kubernetes
- Repurchasing
	- Abandon existing software and move to cloud-first apps (e.g. buying licensed software on cloud or using SaaS)
	- Example:
		- Changing owned email services to email SaaS (e.g. MailChimp)
- Retire
	- Not using it anymore
- Retain
	- Keep the application exactly as is 
	- Common cause:
		- Unsupported OS and application
		- Legacy apps that do not have business justification for migrating to the cloud

- - - -

## Closing Remarks

After you done with the study material, you can test your knowledge by doing some mock questions or quizlets. I will share the ones I used for my study below Also, look for mock questions with the most story-based questions, it is in fact closer to the real thing.

On the other hand, there's also a ton of free courses and materials with mock questions on AWS Partner Network. Unfortunately, it only available for account with business emails.

Trust in yourself and don't be nervous. Good luck on your exam !! 🍀

- - - -

## Recommended Study Materials
- AWS Partner Network Courses (Business email required)
  - [https://explore.skillbuilder.aws/learn/lp/78/architect-learning-plan](https://explore.skillbuilder.aws/learn/lp/78/architect-learning-plan)
  - [https://explore.skillbuilder.aws/learn/course/125/exam-readiness-aws-certified-solutions-architect-associate-digital;lp=78](https://explore.skillbuilder.aws/learn/course/125/exam-readiness-aws-certified-solutions-architect-associate-digital;lp=78)
- Quizlets
  - [https://quizlet.com/144321056/aws-certified-solutions-architect-associate-practice-questions-flash-cards/](https://quizlet.com/144321056/aws-certified-solutions-architect-associate-practice-questions-flash-cards/)
  - [https://quizlet.com/123620854/flashcards](https://quizlet.com/123620854/flashcards)
  - [https://quizlet.com/291364595/flashcards](https://quizlet.com/291364595/flashcards)