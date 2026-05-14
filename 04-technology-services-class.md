# Lesson 4: Cloud Technology and Services

---

Welcome to Domain Three, which covers Cloud Technology and Services. This is the largest domain on the exam, worth thirty-four percent of your score. That means more than one in three questions will come from this content. We are going to cover a lot of ground today, so let me tell you how to approach it. Your job is not to memorize every feature of every service. Your job is to know what each service does, when you would choose it over alternatives, and what the key distinctions are between similar services. Those distinctions are exactly what the exam tests.

Let us start at the foundation.

---

## How AWS Is Organized Around the World

AWS operates in geographic Regions. A Region is a physical location in the world where AWS has built data centers. Examples include US East Northern Virginia, EU West Ireland, Asia Pacific Tokyo, and so on. As of now there are over thirty Regions globally, and AWS continues to add more. When you launch a resource, you choose which Region it lives in.

Every Region is made up of multiple Availability Zones. An Availability Zone, which we abbreviate as AZ, is one or more discrete data centers within a Region. Each AZ has independent power, cooling, and physical security. The AZs within a Region are connected to each other by high-speed, low-latency fiber links, but they are physically separated so that a disaster at one data center does not affect the others. Most Regions have three or more AZs.

This two-layer structure — Regions containing multiple AZs — is the foundation of AWS's high availability design. When you deploy across multiple AZs, you protect your application from the failure of any single data center. When you deploy across multiple Regions, you protect against regional disasters and can serve users closer to their physical location.

The third tier of infrastructure is Edge Locations. These are smaller facilities distributed around the world, in many more cities than Regions. Edge Locations are used by CloudFront for content delivery and by Route 53 for DNS. There are hundreds of Edge Locations globally. They exist specifically to bring content and DNS responses physically closer to end users, reducing the round-trip time for requests.

Beyond Regions, AZs, and Edge Locations, AWS offers three specialty infrastructure options you should know about. Local Zones are extensions of a Region placed in major metropolitan areas. They bring AWS compute, storage, and database services closer to large population centers that are far from any full Region, useful for latency-sensitive workloads like live video production or real-time gaming. AWS Wavelength embeds compute infrastructure within telecommunications providers' 5G networks. Applications deployed to Wavelength zones can be accessed directly from 5G devices with single-digit millisecond latency, ideal for mobile applications that need to process data at the edge of the carrier network. AWS Outposts brings AWS infrastructure physically into your own data center. AWS delivers and manages actual AWS hardware on your premises, giving you the same APIs, tools, and services you use in the cloud, but running locally. This is the answer for workloads that must remain on-premises due to regulatory requirements or extreme latency needs, while still connecting to AWS cloud services.

When the exam asks which service brings AWS compute to your data center, the answer is Outposts. When it asks about low-latency access for 5G mobile devices, it is Wavelength. When it asks about extending AWS to a city that does not have a full Region, it is Local Zones.

---

## Compute Services: EC2 and How You Pay for It

Amazon EC2, which stands for Elastic Compute Cloud, is the fundamental compute service. An EC2 instance is a virtual server. You choose the operating system, the instance type which determines CPU and memory, the storage, and the networking configuration. The word elastic means you can scale the number of instances up and down as demand changes.

EC2 instance types are grouped into families based on their purpose. General purpose instances balance compute, memory, and networking, and they work well for web servers and small databases. Compute optimized instances have a higher ratio of CPU to memory and are used for batch processing, scientific modeling, and high-performance computing. Memory optimized instances have very large amounts of RAM relative to CPU and are designed for in-memory databases and real-time data processing of very large datasets. Storage optimized instances provide high sequential read and write performance to local storage, suited for data warehousing and distributed file systems. Accelerated computing instances include GPUs and are used for machine learning training and graphics rendering.

The exam does not require you to memorize specific instance type names in depth, but you should be able to match a workload description to the right family.

Now let us talk about how you pay for EC2, because this is heavily tested. There are six pricing models, and you need to know which one fits each scenario.

On-Demand instances are the default. You pay for compute by the hour or by the second with no upfront commitment and no minimum term. On-Demand is appropriate for workloads that are unpredictable, for applications you are testing for the first time, and for short-term needs where you cannot commit to a longer contract. It is the most flexible option but also the most expensive on a per-hour basis.

Reserved Instances allow you to commit to a specific instance type in a specific Region for a one-year or three-year term. In exchange for that commitment, you receive a significant discount compared to On-Demand pricing, up to seventy-two percent off depending on the payment option. Reserved Instances come in three payment options: all upfront, partial upfront, and no upfront. All upfront gives the deepest discount. Reserved Instances are appropriate for steady-state workloads — applications that run consistently at a predictable level, like a production web server that is always running.

Savings Plans offer the same level of discount as Reserved Instances but with more flexibility. Instead of committing to a specific instance type, you commit to a dollar amount of compute spend per hour, and that discount applies across any instance type, any Region, and even Lambda and Fargate usage. Compute Savings Plans are the most flexible type. EC2 Instance Savings Plans lock in a specific instance family in a specific Region but allow flexibility within that family. Savings Plans are better than Reserved Instances when your workload might change instance types over time.

Spot Instances let you bid for unused EC2 capacity at discounts of up to ninety percent compared to On-Demand pricing. The trade-off is that AWS can reclaim Spot Instances with only two minutes of warning when it needs the capacity back for On-Demand customers. This makes Spot Instances appropriate only for workloads that can tolerate interruption: batch processing jobs, data analysis pipelines, image rendering, and other fault-tolerant tasks. Never use Spot Instances for anything that requires continuous availability.

Dedicated Hosts are physical servers reserved entirely for your use. Unlike standard EC2 instances where you share physical hardware with other customers, a Dedicated Host ensures that no other AWS customer's instances run on the same physical machine. This is required for certain software licenses that are bound to physical hardware, such as some Microsoft and Oracle licenses that count sockets or cores on the physical server. Dedicated Hosts are also used to meet compliance requirements that prohibit shared tenancy.

Dedicated Instances are similar to Dedicated Hosts in that your instances run on hardware dedicated to a single customer. The difference is that Dedicated Instances do not give you visibility into or control over the physical host, whereas Dedicated Hosts do. You use Dedicated Hosts when you need to bring your own per-socket or per-core licenses. You use Dedicated Instances when you simply need physical isolation without the licensing control.

Here is the mental model for the exam. On-Demand for unpredictable short-term workloads. Reserved Instances or Savings Plans for predictable steady-state workloads, with Savings Plans being more flexible. Spot for fault-tolerant batch workloads where you prioritize cost. Dedicated Hosts when you have per-socket software licenses. Dedicated Instances when you need physical isolation for compliance.

---

## Auto Scaling

Auto Scaling automatically adjusts the number of EC2 instances in response to demand. You define a minimum number of instances, a maximum number, and a desired number. When demand increases, Auto Scaling launches additional instances up to the maximum. When demand decreases, it terminates instances down to the minimum. This provides high availability and cost efficiency simultaneously — you are not over-provisioned during quiet periods and not under-provisioned during peaks.

Auto Scaling policies determine when to scale. A target tracking policy maintains a metric at a target value, such as keeping average CPU utilization at sixty percent. A step scaling policy scales in steps based on how far a metric has crossed a threshold. A scheduled scaling policy scales at predetermined times, useful for workloads with predictable patterns like a business application that always peaks during business hours.

Auto Scaling works closely with load balancers. As new instances are launched, they are registered with the load balancer and begin receiving traffic. As instances are terminated, they are deregistered first. This integration is what makes Auto Scaling useful for production applications rather than just for cost control.

---

## Serverless Compute: Lambda

AWS Lambda lets you run code without provisioning or managing servers. You write a function, define what event should trigger it, and Lambda runs it. You pay only for the compute time consumed while your function is executing, measured in milliseconds. There is no charge when your code is not running, and you never pay for idle servers.

Lambda functions are triggered by events. An API Gateway call, an S3 object upload, a DynamoDB table update, an SNS message, a scheduled CloudWatch Events rule — all of these can trigger a Lambda function. Lambda scales automatically. If a thousand events arrive simultaneously, Lambda runs a thousand concurrent function instances.

Lambda has limits you should know. The maximum execution time for a single function invocation is fifteen minutes. Functions have a memory limit of ten gigabytes. If your workload requires longer running processes or more memory, Lambda is not the right compute service — you would use EC2 or containers instead.

Lambda is the right answer when the exam describes event-driven processing, when the application has variable or unpredictable traffic where paying for idle capacity would be wasteful, or when the team wants to focus entirely on code and not on infrastructure management.

---

## Other Compute Options

AWS Lightsail is a simplified compute service designed for users who are new to AWS or who want a straightforward virtual server experience without dealing with the complexity of EC2. Lightsail bundles a virtual machine, storage, data transfer, and a static IP address into a predictable monthly price. It is designed for simple web applications, WordPress sites, small databases, and development environments. Lightsail is appropriate when you need something simple and predictable, not when you need the full flexibility and scalability of EC2.

Elastic Beanstalk is a platform as a service for deploying web applications. You provide your application code, and Elastic Beanstalk automatically handles the deployment, capacity provisioning, load balancing, auto scaling, and health monitoring. It supports common platforms including Java, Python, Node.js, PHP, Ruby, Go, and Docker. The underlying EC2 instances, load balancers, and auto scaling groups are still there — Beanstalk just manages them for you. You retain full access to the underlying resources and pay only for those resources, not for Beanstalk itself. When the exam describes a developer who wants to deploy a web application without managing infrastructure, Elastic Beanstalk is often the answer.

Amazon ECS is the Elastic Container Service. Containers package application code together with its dependencies into a portable unit that runs consistently across any environment. ECS is AWS's service for running Docker containers at scale. You define a task, which specifies the container image and resource requirements, and ECS manages placing and running that container across a cluster of EC2 instances.

Amazon EKS is the Elastic Kubernetes Service. Kubernetes is an open-source container orchestration platform that has become the industry standard. EKS is a managed Kubernetes service — AWS runs the Kubernetes control plane for you. If your organization has existing Kubernetes expertise or workloads, EKS lets you run standard Kubernetes on AWS without managing the control plane yourself.

AWS Fargate is the serverless compute engine for containers. When you use ECS or EKS with Fargate, you do not manage any EC2 instances. You define your container requirements, and Fargate provisions the appropriate infrastructure, runs your containers, and terminates the infrastructure when you are done. You pay only for the CPU and memory your containers actually use. Fargate removes the need to think about EC2 instances entirely for containerized workloads.

The exam distinction: ECS for AWS-native container orchestration on EC2 instances you manage, EKS for Kubernetes-compatible workloads, Fargate as the serverless option for either ECS or EKS that eliminates the need to manage underlying instances.

---

## Storage Services: S3 Deep Dive

Amazon S3, Simple Storage Service, is object storage. You store files as objects within containers called buckets. Objects can be up to five terabytes in size. S3 is not a file system — you do not mount it like a drive. You access objects via URL or API. S3 is infinitely scalable from the user's perspective. You never need to pre-provision storage capacity.

S3 is highly durable. AWS designs S3 to provide eleven nines of durability — that is 99.999999999 percent. This means that if you stored ten million objects, you would expect to lose one object every ten thousand years. This durability comes from the fact that AWS automatically stores copies of every object across multiple devices within a Region.

S3 has seven storage classes, and you need to know when to use each one. They are organized by how frequently you access data and how quickly you need to retrieve it.

S3 Standard is the default class for frequently accessed data. It provides low latency, high throughput, and stores data across at least three AZs. There is no retrieval fee. Use this for active workloads, websites, and content distribution.

S3 Standard Infrequent Access, or S3 Standard-IA, is for data you access less frequently but still need to retrieve quickly when you do. It has a lower storage cost than S3 Standard but charges a per-gigabyte retrieval fee. Data is still stored across multiple AZs. Use it for backups and disaster recovery copies that you might need to restore but do not access daily.

S3 One Zone Infrequent Access, or S3 One Zone-IA, is similar to Standard-IA but stores data in only a single AZ rather than across multiple. This makes it cheaper, but if that AZ is lost, so is your data. Use it for data that you can recreate if lost, or for secondary backup copies.

S3 Glacier Instant Retrieval is for archive data that you rarely access but need to retrieve in milliseconds when you do. It has much lower storage costs than Standard-IA but higher retrieval fees. Think of it as cold storage with on-demand retrieval.

S3 Glacier Flexible Retrieval, formerly just called S3 Glacier, is for archive data where you can wait minutes to hours for retrieval. Retrieval options range from expedited retrieval in one to five minutes, standard retrieval in three to five hours, and bulk retrieval in five to twelve hours. The bulk option is the cheapest. This is for long-term archive where cost is the priority and immediate access is not needed.

S3 Glacier Deep Archive is for data that you will almost never access, with a minimum storage duration of one hundred eighty days. Retrieval takes twelve to forty-eight hours. It is the lowest-cost S3 storage option and is appropriate for regulatory archives, legal records, or any data that must be retained for years but will almost certainly never be retrieved.

S3 Intelligent-Tiering is a special class that automatically moves objects between access tiers based on changing access patterns. You pay a small monthly monitoring fee per object, and S3 automatically moves objects to the appropriate tier without performance impact or retrieval fees when objects move between the frequent access and infrequent access tiers. Use Intelligent-Tiering when your access patterns are unpredictable and you want to optimize cost without managing lifecycle rules manually.

S3 Lifecycle Policies let you define rules that automatically transition objects between storage classes or delete them after a specified time. For example, a rule might move objects to Standard-IA after thirty days, to Glacier Flexible Retrieval after ninety days, and delete them after seven years. Lifecycle policies are the mechanism for implementing a data retention strategy without manual intervention.

Additional S3 features worth knowing: S3 Versioning keeps multiple versions of an object in the same bucket. If you overwrite or delete an object, previous versions are retained and can be restored. S3 Object Lock prevents objects from being deleted or overwritten for a specified retention period, which is used for compliance requirements. S3 Transfer Acceleration speeds up uploads to S3 by routing traffic through CloudFront edge locations, which connect to S3 over AWS's optimized backbone network rather than the public internet.

---

## Other Storage Services

Amazon EBS, Elastic Block Store, provides persistent block storage for EC2 instances. Think of EBS as a hard drive that you attach to a virtual server. Unlike the instance store, which is local storage tied to the physical host and lost when the instance stops, EBS volumes persist independently. If you stop an EC2 instance and restart it, the EBS volume and all its data remain.

EBS volumes are tied to a single AZ and can only be attached to EC2 instances in the same AZ. You can snapshot an EBS volume to S3, and snapshots are stored across multiple AZs, which allows you to restore or copy a volume to a different AZ or Region.

EBS volume types differ in performance characteristics. SSD-backed volumes are optimized for input/output-intensive workloads and are appropriate for databases and boot volumes. The GP3 and GP2 types are general purpose SSDs. IO2 and IO1 are provisioned IOPS SSDs for workloads requiring consistent very high performance. HDD-backed volumes are lower cost and optimized for throughput rather than random access, suitable for big data workloads and log processing.

Amazon EFS, Elastic File System, is a fully managed shared file system that can be mounted concurrently by thousands of EC2 instances across multiple AZs. Unlike EBS, which is attached to a single instance, EFS provides a shared file system that multiple servers can read and write simultaneously. It uses the NFS protocol and automatically scales as you add and remove files. It is appropriate for shared content repositories, web server content that multiple instances need to serve, and applications designed for Linux that expect a standard file system.

AWS Storage Gateway connects on-premises environments to AWS cloud storage. It runs as a virtual machine on-premises and presents AWS storage as a local storage target. There are three gateway types. File Gateway presents an NFS or SMB interface that stores files as objects in S3. Volume Gateway stores data as Amazon EBS snapshots, either caching frequently accessed data locally or storing all data locally with snapshots to S3. Tape Gateway presents a virtual tape library to backup software, which then stores tapes in S3 Glacier. Storage Gateway is the answer when an on-premises application needs to continue using its existing storage interface while data is backed up or archived to the cloud.

The Snow Family services move large amounts of data physically when network transfer would be too slow or too expensive. AWS Snowcone is the smallest device, ruggedized for edge computing and data collection in remote locations, holding up to fourteen terabytes of storage. AWS Snowball Edge is a briefcase-sized device that holds petabytes of data when used in a cluster and includes compute capability so you can run Lambda functions locally to process data before shipping it back. Snowball Edge comes in a storage-optimized and a compute-optimized version. AWS Snowmobile is an actual shipping container on a semi-truck that can move up to one hundred petabytes in a single transfer. The exam asks about Snow devices when scenarios involve migrating very large datasets where network transfer would take too long or be too costly.

---

## Database Services

AWS offers managed database services for different data models and workload types. In each case, managed means AWS handles hardware provisioning, patching, backups, and replication so that you focus on your data and applications.

Amazon RDS, Relational Database Service, supports traditional relational databases: MySQL, PostgreSQL, Oracle, Microsoft SQL Server, and MariaDB, plus Amazon's own Aurora. RDS automates backups, software patching, failure detection, and recovery. You cannot access the underlying EC2 instances or operating system — that is AWS's responsibility. If you need OS-level access to your database server, you would run the database yourself on EC2 rather than using RDS.

The two key high availability and performance features of RDS are Multi-AZ and Read Replicas, and the exam tests these constantly.

Multi-AZ deployment maintains a synchronous standby replica of your database in a different AZ. All writes to the primary database are synchronously replicated to the standby before the write is acknowledged. If the primary fails, RDS automatically fails over to the standby, typically in one to two minutes. The standby does not serve any read traffic under normal conditions — its only purpose is to be ready for failover. Multi-AZ is a high availability feature, not a performance feature.

Read Replicas, by contrast, are asynchronous copies of the database that serve read traffic. You can have up to five Read Replicas for MySQL and PostgreSQL. Applications that read data far more than they write, such as reporting applications or content-heavy websites, can direct read queries to the replicas and reduce load on the primary. Because replication is asynchronous, there is a small lag between writes to the primary and when those writes appear on replicas. Read Replicas are a performance feature, not strictly a high availability feature — though you can promote a Read Replica to become an independent primary database if needed.

Here is how to answer the exam question: if the scenario mentions surviving a database failure or automatic recovery, the answer is Multi-AZ. If the scenario mentions read-heavy workloads, reporting queries, or reducing database load, the answer is Read Replicas.

Amazon Aurora is AWS's cloud-native relational database. It is compatible with MySQL and PostgreSQL, so applications built for those databases can use Aurora without code changes. Aurora stores data across three AZs by default and maintains six copies of your data — two in each AZ. It replicates faster than standard RDS engines and automatically grows storage in ten-gigabyte increments as needed. Aurora is up to five times faster than standard MySQL and three times faster than standard PostgreSQL. It also supports Aurora Serverless, a version that automatically scales compute capacity up and down based on demand and pauses when there is no activity, charging only for actual database usage.

Amazon DynamoDB is a fully managed NoSQL database service. Unlike relational databases that organize data into tables with fixed schemas and relationships, DynamoDB uses a flexible key-value and document model. You define a partition key, and optionally a sort key, and DynamoDB distributes data automatically across enough servers to support millions of requests per second. DynamoDB is serverless — you do not provision database instances. You can configure it in two capacity modes: provisioned capacity, where you specify the read and write units you expect, or on-demand capacity, where DynamoDB automatically scales and you pay per request. DynamoDB is the answer when the exam describes applications that need consistent single-digit millisecond performance at any scale, high-traffic web applications, gaming leaderboards, shopping carts, and session management.

DynamoDB Accelerator, or DAX, is an in-memory cache that sits in front of DynamoDB and provides microsecond response times for reads. DAX is fully managed and API-compatible with DynamoDB, meaning your application uses the same calls and DAX transparently serves cache hits.

Amazon ElastiCache provides in-memory data stores for applications that need extremely fast access to frequently used data. It supports two engines: Redis and Memcached. Redis supports complex data structures including lists, sets, and sorted sets, supports persistence, and supports replication and clustering for high availability. Memcached is simpler and optimized purely for caching with no persistence or replication. Use Redis when you need persistence, replication, or complex data operations such as pub/sub messaging or sorted sets for leaderboards. Use Memcached when you want a simple, multi-threaded cache for straightforward read caching with no need for persistence.

The exam typically tests ElastiCache in scenarios where an application's database is overwhelmed by repeated reads of the same data, such as a popular product catalog or session data. Adding ElastiCache reduces database load by serving cache hits from memory.

Amazon Redshift is a data warehouse service. A data warehouse is a database optimized for analytical queries over very large datasets — millions or billions of rows. Unlike transactional databases that are optimized for many small individual operations, Redshift is optimized for complex aggregation queries across entire tables. Redshift stores data in columnar format, compresses it efficiently, and can scale to multiple petabytes. Redshift is the answer when the exam describes business intelligence queries, historical data analysis, or analytical workloads on large datasets.

Amazon Neptune is a fully managed graph database. Graph databases store entities and the relationships between them as a first-class data model, rather than forcing relationships into relational joins. Neptune is appropriate for social networks where you need to traverse relationships like "friends of friends," fraud detection that requires analyzing transaction patterns and connections between accounts, recommendation engines, and knowledge graphs. When the exam describes a highly connected dataset where the relationships between items are as important as the items themselves, Neptune is the answer.

Amazon DocumentDB is a fully managed document database with MongoDB compatibility. It stores data as JSON-like documents, making it natural for applications that work with semi-structured data. DocumentDB is the answer when the exam describes an application built on MongoDB that needs to move to a managed AWS service without rewriting application code.

---

## Networking: VPC and Beyond

Amazon VPC, Virtual Private Cloud, is your private network within AWS. When you create a VPC, you define an IP address range using CIDR notation. Within the VPC, you create subnets, which are subdivisions of the IP range. Subnets that you configure to route traffic to an Internet Gateway are public subnets. Subnets that do not have that route are private subnets.

Resources in a public subnet can communicate directly with the internet. Resources in a private subnet cannot initiate connections to the internet, and the internet cannot initiate connections to them, which is why databases and application servers that do not need to be publicly accessible should live in private subnets.

For private subnet resources that need to initiate outbound connections to the internet — for software updates, for example — you use a NAT Gateway. The NAT Gateway lives in a public subnet and forwards outbound requests from private subnet resources to the internet, returning the responses to the originating private resource. Inbound connections from the internet to private subnet resources are still blocked.

A Security Group is a stateful virtual firewall that controls inbound and outbound traffic at the instance level. Stateful means that if you allow an inbound connection, the return traffic is automatically allowed regardless of outbound rules. Security Groups use only allow rules — you cannot write deny rules in a Security Group. By default, a new Security Group denies all inbound traffic and allows all outbound traffic.

A Network ACL, or NACL, is a stateless virtual firewall that operates at the subnet level. Stateless means that inbound and outbound rules are evaluated independently, so if you allow inbound traffic from a particular port, you must also explicitly allow the outbound return traffic. NACLs support both allow and deny rules. Because they operate at the subnet level, they affect all resources within that subnet.

When the exam asks about blocking a specific IP address from accessing your application, the answer is NACLs, because Security Groups have no deny capability.

CloudFront is AWS's global content delivery network. It caches copies of your content at Edge Locations around the world. When a user requests content, CloudFront serves it from the nearest Edge Location rather than from the origin server. This reduces latency for end users and reduces load on the origin. CloudFront supports static assets like images, videos, and web pages as well as dynamic content. It also integrates with AWS WAF and Shield for security, and with ACM for HTTPS certificates at no additional cost.

Amazon Route 53 is AWS's DNS service. DNS translates human-readable domain names into IP addresses. Route 53 is highly available and scalable. It supports multiple routing policies that the exam tests by scenario.

Simple routing returns a single resource for a given name, appropriate when you have one resource serving a domain. Failover routing returns the primary resource under normal conditions and automatically switches to a backup resource if health checks detect that the primary is unhealthy. Weighted routing distributes traffic across multiple resources in proportions you define — for example, sending ten percent of traffic to a new version of your application and ninety percent to the existing version for a controlled rollout. Latency-based routing directs users to the resource that provides the lowest latency based on their geographic location. Geolocation routing directs users based on their location as determined by DNS — you can send European users to European endpoints and North American users to North American endpoints regardless of latency. Geoproximity routing is similar but allows you to shift the geographic boundary by adjusting a bias value, and requires Route 53 Traffic Flow. Multivalue answer routing returns up to eight healthy records for a name, providing basic load balancing at the DNS level.

Elastic Load Balancing distributes incoming traffic across multiple targets. There are three types. The Application Load Balancer, or ALB, operates at HTTP and HTTPS level, which is Layer 7 of the network stack. It can route requests based on URL path, hostname, HTTP headers, and other HTTP attributes, making it ideal for web applications and microservices. The Network Load Balancer, or NLB, operates at Layer 4 using TCP and UDP. It handles millions of requests per second at ultra-low latency and supports static IP addresses, making it appropriate for high-performance applications, gaming, and IoT. The Gateway Load Balancer, or GWLB, is used to deploy, scale, and manage third-party network appliances like firewalls and intrusion detection systems. It presents those appliances transparently in the traffic path.

AWS Direct Connect provides a dedicated private network connection from your on-premises environment to AWS. Instead of routing traffic over the public internet, Direct Connect uses a physical fiber connection to an AWS Direct Connect location, which connects to the AWS backbone. This provides more consistent bandwidth and lower latency than a VPN over the internet and is appropriate for high-bandwidth workloads, latency-sensitive applications, and compliance requirements that prohibit data from traversing the public internet.

AWS Site-to-Site VPN creates an encrypted connection between your on-premises network and your AWS VPC over the public internet. VPN is faster to establish than Direct Connect — Direct Connect requires working with a telecommunications provider to provision a physical line, which can take weeks. VPN is appropriate for encrypted connectivity when Direct Connect's setup time and cost are not justified, for backup connectivity alongside Direct Connect, and for smaller or temporary connections.

---

## Management and Governance

AWS CloudFormation is infrastructure as code for AWS. You write templates in JSON or YAML that describe the AWS resources you want — EC2 instances, S3 buckets, RDS databases, IAM roles, VPCs — and CloudFormation provisions and configures those resources in the correct order, resolving dependencies automatically. A CloudFormation stack is a collection of resources managed as a unit. You create, update, and delete stacks, and CloudFormation handles the corresponding resource lifecycle operations. This allows you to version your infrastructure in source control, deploy consistent environments across development, staging, and production, and tear down entire environments when you are done. If a deployment fails partway through, CloudFormation rolls back to the previous state automatically.

AWS CloudTrail records API calls made to your AWS account. Every time someone or something calls an AWS API — creating an EC2 instance, modifying a Security Group, deleting an S3 object — CloudTrail logs who made the call, when, from where, and what parameters were used. CloudTrail logs are stored in S3. CloudTrail is the primary tool for security auditing, compliance investigation, and answering questions like "who made this change and when?" By default, CloudTrail retains the last ninety days of management events in the console, but you should create a Trail to store logs in S3 for long-term retention.

Amazon CloudWatch is the monitoring and observability service. CloudWatch collects metrics from AWS services — CPU utilization, network traffic, request count, error rate — and from your own applications if you send custom metrics. CloudWatch Logs collects log files from EC2 instances, Lambda functions, RDS, and other services. CloudWatch Alarms trigger actions when metrics cross thresholds — for example, sending an SNS notification when CPU utilization exceeds eighty percent for five minutes, or triggering an Auto Scaling action. CloudWatch Dashboards provide custom visualizations of metrics. CloudWatch Events, now called EventBridge, triggers actions based on changes in AWS resource state.

AWS Systems Manager provides a suite of operational tools for managing EC2 instances and on-premises servers at scale. Session Manager allows you to connect to instances via a browser-based shell without needing SSH or a bastion host, which improves security by eliminating inbound port 22 access. Parameter Store provides a secure, centralized location for configuration data and secrets, supporting plain text and encrypted values. Patch Manager automates the process of applying operating system and application patches. Automation lets you create and run runbooks — sequences of operational tasks — across many instances simultaneously.

AWS Trusted Advisor analyzes your AWS environment and provides recommendations across five categories: cost optimization, performance, security, fault tolerance, and service limits. The security recommendations are particularly relevant — Trusted Advisor alerts you if S3 buckets are publicly accessible, if MFA is not enabled on the root account, if Security Groups have overly permissive rules, and similar issues. Basic Trusted Advisor checks are available to all AWS customers. Business and Enterprise support plans unlock all Trusted Advisor checks and provide programmatic access via API.

AWS Control Tower is a service for setting up and governing multi-account AWS environments. Organizations that need to deploy AWS at scale with many accounts use Control Tower to establish a landing zone — a well-architected multi-account baseline with centralized logging, security guardrails, and account provisioning automation. Control Tower uses AWS Organizations under the hood and adds a layer of automation and a management dashboard for governing the environment. When the exam describes a large organization setting up a new AWS environment with multiple accounts and centralized governance, Control Tower is often the answer.

---

## Application Integration Services

Amazon SNS, Simple Notification Service, is a fully managed pub/sub messaging service. A publisher sends a message to an SNS topic, and SNS fans that message out to all subscribers of the topic. Subscribers can be Lambda functions, SQS queues, HTTP endpoints, email addresses, SMS numbers, or mobile push notification endpoints. SNS is appropriate when you need to send the same message to multiple destinations simultaneously or when you need to decouple the sender from multiple receivers.

Amazon SQS, Simple Queue Service, is a fully managed message queue. A producer sends a message to the queue, and a consumer receives and processes it. Unlike SNS, which pushes messages to all subscribers immediately, SQS holds messages until a consumer explicitly requests them. Messages stay in the queue until the consumer deletes them after processing, which ensures no message is lost even if the consumer fails. SQS is appropriate for decoupling components so that a downstream service being slow or unavailable does not cause the upstream service to fail. It provides a buffer that absorbs traffic spikes.

SQS supports two queue types. Standard queues provide unlimited throughput and at-least-once delivery, meaning a message may occasionally be delivered more than once, and message order is not guaranteed. FIFO queues guarantee exactly-once processing and preserve message order, with throughput limited to three hundred messages per second or higher with batching.

AWS Step Functions is a serverless workflow orchestration service. It allows you to define workflows as state machines, sequencing AWS services like Lambda, ECS, SQS, and others into multi-step workflows with branching, parallel execution, and error handling. When the exam describes automating a multi-step business process — such as processing an order through inventory check, payment processing, and shipping confirmation — Step Functions is the answer.

Amazon EventBridge is a serverless event bus that routes events from AWS services, your own applications, and third-party SaaS providers to target services based on rules. When an EC2 instance changes state, when an S3 object is created, when a custom application emits an event, EventBridge can route that event to a Lambda function, an SQS queue, a Step Functions workflow, and more. EventBridge was formerly known as CloudWatch Events.

---

## Analytics Services

Amazon Athena is an interactive query service that lets you analyze data in S3 using standard SQL without setting up a server or database. You define a schema over your S3 data using a data catalog, and Athena executes your SQL queries directly against the files. You pay per terabyte of data scanned. Athena is appropriate for ad-hoc analysis of log files, click streams, and other data already in S3 without loading it into a database first.

AWS Glue is a serverless data integration service for ETL — extract, transform, and load — operations. Glue crawls your data sources, infers schemas, and stores them in the Glue Data Catalog. Glue jobs transform data from source format to target format and load it into a destination like Redshift or S3. Glue works with Athena — Athena uses the Glue Data Catalog for schema information. When the exam describes preparing and transforming data for analysis, Glue is the answer.

Amazon QuickSight is a cloud-native business intelligence and data visualization service. It connects to data sources including Redshift, RDS, S3, Athena, and others, and allows business analysts to create dashboards and reports without writing code. When the exam describes a non-technical business analyst who needs to visualize data or a company that wants to share interactive dashboards, QuickSight is the answer.

Amazon Kinesis is a platform for collecting, processing, and analyzing real-time streaming data. It has several components. Kinesis Data Streams ingests real-time data from sources like application logs, IoT sensors, social media feeds, and financial transactions. Consumers read from the stream and process records in real time. Kinesis Data Firehose is the simplest way to load streaming data into AWS destinations — it automatically captures, transforms, and loads data into S3, Redshift, OpenSearch, and other destinations without writing consumer applications. Kinesis Data Analytics lets you run SQL queries against streaming data in real time. Kinesis Video Streams captures video from connected cameras and devices for real-time processing.

When the exam describes real-time data ingestion from many sources, the answer is Kinesis Data Streams or Kinesis Data Firehose depending on whether you need custom processing or automatic delivery to a destination.

---

## AI and Machine Learning Services

Amazon SageMaker is the comprehensive machine learning platform for building, training, and deploying custom machine learning models. It provides tools for each phase of the ML lifecycle: data labeling, data preprocessing, model training, model tuning, model evaluation, and model deployment. SageMaker is for teams that need to build their own models from training data. It requires data science expertise. When the exam describes training a custom ML model at scale, SageMaker is the answer.

Amazon Rekognition analyzes images and video using pre-trained computer vision models. It can detect objects, people, text, and scenes in images, identify faces and compare them to other faces, detect inappropriate content, and analyze video for actions and movement. No machine learning expertise is needed — you call the API with an image or video and get back structured results. When the exam describes identifying faces in photos, detecting objects in images, or moderating user-uploaded content, Rekognition is the answer.

Amazon Comprehend is a natural language processing service that analyzes text. It identifies the sentiment of text — positive, negative, neutral, or mixed — detects entities like people, places, and organizations, extracts key phrases, identifies the language, and classifies documents into categories you define. Use cases include analyzing customer feedback, categorizing support tickets, and extracting information from documents.

Amazon Lex provides conversational interface capabilities — chatbots and voice assistants. Lex understands natural language input and maintains conversational state across multiple turns. It is the same technology that powers Amazon Alexa. When the exam describes building a chatbot for customer service or a voice-activated interface, Lex is the answer.

---

## Migration Services

AWS Database Migration Service, DMS, migrates databases to AWS with minimal downtime. It supports migrations between the same database engine — for example, Oracle on-premises to Oracle RDS — and between different engines, called heterogeneous migrations, such as Oracle on-premises to Aurora PostgreSQL. During the migration, DMS keeps the source database running and continuously replicates changes to the target, so you can cut over with minimal interruption. For heterogeneous migrations, you also use the Schema Conversion Tool to translate the database schema and stored procedures.

AWS Application Discovery Service collects information about your on-premises servers to plan migrations. It runs in agentless mode by integrating with VMware vCenter or in agent mode with a lightweight agent installed on each server. It collects server specifications, utilization data, and application dependency maps, which helps you understand what you are migrating and what depends on what.

AWS Migration Hub provides a central dashboard to track the progress of migrations across multiple tools. Instead of checking each migration service individually, Migration Hub aggregates status in one place and provides a complete view of what has been migrated and what remains.

---

## Review Questions

All right, let us go through the review questions together. I will ask each one, pause for you to think, and then explain the answer.

Question One. A company needs to choose an AWS Region for a new application. Their primary considerations are data residency regulations that require customer data to remain in Europe and the need for low latency for European users. What should guide their Region selection?

Think about it. The answer is that they should choose an AWS Region located in Europe, such as EU West Ireland or EU Central Frankfurt. Data residency regulations determine which geographic Region they can use. Once constrained to European Regions, they should select the specific Region closest to the majority of their users to minimize latency. No other AWS infrastructure layer — not AZs, not Edge Locations — determines data residency. That is a Region-level decision.

Question Two. An application runs in one Availability Zone and experiences an outage when that AZ has a power failure. Which architectural change would provide high availability?

The answer is deploying the application across multiple Availability Zones with a load balancer distributing traffic. If the application runs in two or three AZs, the failure of one AZ does not take down the entire application. The load balancer detects the unhealthy instances in the failed AZ and routes traffic to the healthy instances in the other AZs. This is the fundamental high availability pattern on AWS.

Question Three. A startup is launching a web application that will experience highly variable traffic — very low overnight, high during business hours, with occasional viral spikes that are impossible to predict. Which EC2 purchasing option makes the most sense for the web tier?

The answer is a combination approach. Use Reserved Instances or Savings Plans for the baseline capacity that runs consistently during business hours, and use Auto Scaling with On-Demand or Spot Instances to handle unexpected spikes. The baseline is predictable enough to commit to reserved capacity. The spike is unpredictable enough that you need the flexibility of On-Demand or the cost savings of Spot if the spike workload is fault-tolerant.

If the question is simpler and asks only about unpredictable variable workloads, the answer is On-Demand because there is no commitment required.

Question Four. A company wants to run batch data processing jobs that analyze log files. The jobs can be restarted if interrupted. They want the lowest possible compute cost. Which EC2 pricing model should they use?

The answer is Spot Instances. Batch processing jobs that can restart from failure are the ideal use case for Spot. The workload is fault-tolerant, the cost savings can be up to ninety percent versus On-Demand, and the risk of interruption is acceptable because the job resumes where it left off.

Question Five. A company has a legacy application with a Microsoft SQL Server license that specifies the number of processors on the physical server. They want to run this on AWS while remaining compliant with their license. Which EC2 option do they need?

The answer is Dedicated Hosts. Dedicated Hosts give you visibility into and control over the physical host's configuration — you can see how many sockets and cores the host has. This is required for per-socket and per-core software licenses from vendors like Microsoft and Oracle. Dedicated Instances provide physical isolation but do not give you the hardware visibility needed for license compliance.

Question Six. A developer uploads a new version of an application to AWS. The application begins returning errors, and the developer wants the deployment to automatically revert to the previous version. Which service provides this capability for EC2-based deployments?

The answer is AWS CodeDeploy. CodeDeploy supports automatic rollback when CloudWatch alarms detect failures after deployment. It can also perform blue/green deployments where you deploy to a new set of instances, shift traffic gradually, and roll back by shifting traffic back to the original instances if problems occur.

Question Seven. A company stores one terabyte of financial records that must be retained for seven years to satisfy audit requirements. The records will never be accessed during the retention period unless there is an audit, which has never happened. What is the most cost-effective storage option?

The answer is S3 Glacier Deep Archive. Records that will essentially never be accessed, must be retained for years, and where retrieval time is not a concern are the exact use case for S3 Glacier Deep Archive. It is the lowest-cost S3 storage class. S3 Glacier Flexible Retrieval would also be a reasonable answer, but Deep Archive is cheaper and more appropriate for a seven-year retention period with effectively zero expected retrievals.

Question Eight. A company's data access patterns are unpredictable. Some files are accessed frequently for weeks and then never accessed again. Others are rarely accessed initially and then become popular. The company wants to optimize storage costs without manually managing storage class transitions. What should they use?

The answer is S3 Intelligent-Tiering. When access patterns are unpredictable, Intelligent-Tiering automatically moves objects to the most cost-effective tier based on actual access. You pay a small monitoring fee but avoid the need to predict and manage lifecycle rules manually.

Question Nine. An application requires a file system that can be mounted simultaneously by fifty EC2 instances running in different Availability Zones for shared access to a common set of files. Which storage service meets this requirement?

The answer is Amazon EFS. EFS is a network file system that can be mounted concurrently by many instances across multiple AZs within a Region. EBS volumes can only be attached to one EC2 instance at a time and are AZ-specific. S3 is object storage, not a mountable file system. EFS is the right choice for shared file system access across instances.

Question Ten. A company is migrating an on-premises Windows application that uses Windows file shares with SMB protocol and Active Directory authentication. What AWS storage service should they use?

The answer is Amazon FSx for Windows File Server. FSx for Windows provides a native Windows file system with SMB protocol support and full Active Directory integration. EFS uses NFS protocol and does not support SMB natively. This is the service specifically designed for Windows applications that depend on Windows file system features and Active Directory.

Question Eleven. A company needs to migrate two hundred terabytes of data from their on-premises data center to S3. Their internet connection is one gigabit per second, and uploading that much data over the internet would take weeks. What should they do?

The answer is AWS Snowball Edge. Rather than transferring data over the internet, AWS ships you a physical Snowball Edge device. You copy the data to the device on-premises, ship it back to AWS, and AWS imports it to S3. For two hundred terabytes, this is far faster than waiting for a slow internet transfer to complete.

Question Twelve. A production database on RDS experiences periodic read-heavy queries from a reporting system that are causing slowness for other application users. What should the team do?

The answer is create Read Replicas for the RDS database and direct the reporting queries to the replicas. This offloads the read-heavy reporting workload from the primary database to the replicas, preserving primary database performance for the application. Multi-AZ would not help here — the standby instance does not serve read traffic.

Question Thirteen. A company wants their RDS database to automatically switch to a backup instance in a different AZ if the primary fails, with minimal downtime. What should they enable?

The answer is Multi-AZ deployment for RDS. Multi-AZ maintains a synchronous standby in a different AZ and automatically fails over to it when the primary fails. Failover typically takes one to two minutes. This is the RDS high availability feature. Read Replicas do not provide automatic failover — they must be manually promoted.

Question Fourteen. A startup is building a mobile game leaderboard that needs to store scores for millions of players, return results in single-digit milliseconds, and scale automatically as the game becomes more popular. Which database should they use?

The answer is Amazon DynamoDB. DynamoDB is a NoSQL key-value and document database that provides single-digit millisecond performance at any scale. It scales automatically through its on-demand capacity mode. A relational database like RDS would struggle with this scale and this access pattern. DynamoDB is the go-to choice for high-traffic, low-latency workloads where the data model fits its key-value structure.

Question Fifteen. An application generates session data that needs to be shared across multiple web server instances. The session data must be readable in under one millisecond. What should the team use?

The answer is Amazon ElastiCache. Storing session data in memory rather than in a database provides submillisecond access times and removes session state from individual web servers, allowing any server to handle any request. Between Redis and Memcached, Redis is typically preferred for session management because it supports persistence — if the cache restarts, session data can be restored rather than losing all active sessions.

Question Sixteen. A business intelligence team needs to run complex SQL queries joining hundreds of millions of rows across multiple tables for quarterly reporting. The queries take hours to run on the existing MySQL database. What should they migrate to?

The answer is Amazon Redshift. Redshift is a columnar data warehouse designed specifically for analytical queries over very large datasets. Its columnar storage format, massively parallel processing architecture, and data compression make it dramatically faster than row-oriented relational databases for aggregation queries over large result sets.

Question Seventeen. A company has an on-premises application that needs a stable, private, high-bandwidth connection to AWS for data transfer workloads. Internet-based connectivity has too much variability. What should they use?

The answer is AWS Direct Connect. Direct Connect provides a dedicated private network connection from on-premises to AWS, bypassing the public internet entirely. It provides consistent bandwidth and latency that is not subject to internet variability. A Site-to-Site VPN would still route over the public internet and would not solve the variability problem.

Question Eighteen. A company needs to quickly establish an encrypted connection between their office network and an AWS VPC to test a cloud migration. Direct Connect would take too long to provision. What is the appropriate solution?

The answer is AWS Site-to-Site VPN. VPN can be set up in hours by configuring the VPN Gateway in AWS and a customer gateway on the on-premises side. It creates an encrypted tunnel over the existing internet connection. Direct Connect requires working with a telecom provider and can take weeks or months to provision.

Question Nineteen. Which Route 53 routing policy would you use to split traffic between two versions of an application so that ten percent of users see the new version and ninety percent see the old version?

The answer is weighted routing. Weighted routing distributes traffic across multiple resources in proportions you define. Assigning the new version a weight of ten and the old version a weight of ninety achieves exactly this split. This pattern is called a canary deployment or A/B testing.

Question Twenty. A global company wants users in Asia to be directed to servers in the Singapore Region and users in North America to be directed to servers in the Virginia Region. Which Route 53 routing policy implements this?

The answer is geolocation routing. Geolocation routing directs users based on their geographic location as detected by their DNS query's source. You define rules that map geographic areas — continents, countries, or US states — to specific endpoints.

Question Twenty-One. A company's database-backed web application crashes under heavy load. Analysis shows that the same queries are executed by many users and return the same data. What is the most effective way to reduce database load?

The answer is to add an ElastiCache caching layer in front of the database. Frequently requested data that does not change often can be cached in memory. Subsequent requests for the same data are served from the cache without hitting the database, dramatically reducing database load.

Question Twenty-Two. A DevOps team wants to define their entire AWS infrastructure — VPCs, subnets, EC2 instances, RDS databases — as code so they can version it, review changes, and deploy consistent environments. What service should they use?

The answer is AWS CloudFormation. CloudFormation is AWS's native infrastructure as code service. Teams write templates in JSON or YAML, check them into source control, and use CloudFormation to provision and update infrastructure. This provides repeatability, version history, and the ability to deploy identical environments for development, staging, and production.

Question Twenty-Three. A security team needs to determine who deleted an S3 bucket and when the deletion occurred. Where should they look?

The answer is AWS CloudTrail logs. CloudTrail records all API calls to AWS services, including the identity of the caller, the timestamp, and the API parameters. The deletion of an S3 bucket is an API call that CloudTrail captures. If a Trail is configured to send logs to S3, the team can search those logs to find the deletion event.

Question Twenty-Four. A company wants to receive an email alert when their EC2 instance's CPU utilization exceeds eighty percent for five minutes. How do they set this up?

The answer is create a CloudWatch Alarm on the CPUUtilization metric for the EC2 instance, set the threshold to eighty percent for a five-minute period, and configure the alarm action to publish to an SNS topic that sends email to the team. This is the standard pattern: CloudWatch monitors the metric, the alarm evaluates the threshold, and SNS delivers the notification.

Question Twenty-Five. An application uploads images to S3, and each upload should trigger a Lambda function to create thumbnail versions. What is the cleanest way to connect S3 events to Lambda?

The answer is to configure an S3 event notification that triggers the Lambda function when an object is created. Alternatively, you could use EventBridge with an S3 event source, but the direct S3 to Lambda trigger is the most straightforward approach. Lambda scales automatically to handle many concurrent uploads.

Question Twenty-Six. A company processes customer orders through multiple steps: verify inventory, charge payment, update the order record, and send a confirmation email. Each step is a separate Lambda function. How should they orchestrate this workflow?

The answer is AWS Step Functions. Step Functions lets you define a state machine that coordinates Lambda functions and other services in a defined sequence, handles errors and retries for each step, and maintains the state of the workflow. This is exactly the multi-step orchestration use case Step Functions is designed for.

Question Twenty-Seven. A mobile application generates a continuous stream of user activity events — page views, clicks, and purchases — from millions of users. The data must be captured in real time and analyzed for immediate personalization. What service should handle this data ingestion?

The answer is Amazon Kinesis Data Streams. Kinesis Data Streams is designed to collect and process large streams of real-time data from many sources simultaneously. Consumers can read from the stream and process events within milliseconds of ingestion, enabling real-time analysis.

Question Twenty-Eight. A data engineering team needs to transform raw CSV files in S3 into a Parquet format suitable for Athena queries, on a scheduled basis. What service should they use?

The answer is AWS Glue. Glue is a serverless ETL service that can read data from S3, transform it, and write it back in a different format. Glue crawlers can automatically detect the schema of the CSV files, and Glue jobs apply the transformation logic. The resulting Parquet files are then queryable by Athena more efficiently.

Question Twenty-Nine. A business analyst needs to create interactive dashboards from data stored in Amazon Redshift, without writing any code. What service should they use?

The answer is Amazon QuickSight. QuickSight is AWS's business intelligence service designed for non-technical users to connect to data sources like Redshift, create visualizations, and share dashboards. It requires no coding and provides a drag-and-drop interface for building reports.

Question Thirty. A retail company wants to train a custom machine learning model on their historical sales data to predict which products individual customers are most likely to buy next. What AWS service should they use?

The answer is Amazon SageMaker. SageMaker is the platform for building, training, and deploying custom machine learning models. Training a model on historical sales data to make purchase predictions is a custom ML task that requires SageMaker's training and inference infrastructure. Note that Amazon Personalize is an alternative if the use case fits its recommendation model — Personalize is a pre-built recommendation engine that requires less ML expertise.

Question Thirty-One. A company wants to automatically moderate user-uploaded profile photos and reject images that contain inappropriate content. What AWS service does this?

The answer is Amazon Rekognition. Rekognition's content moderation capability analyzes images and returns labels identifying potentially inappropriate or unsafe content. You can set confidence thresholds and reject images that exceed them. No machine learning expertise is required — you call the API with the image.

Question Thirty-Two. A company wants to analyze thousands of customer support emails to automatically classify them by topic and understand whether the sentiment is positive or negative. What service should they use?

The answer is Amazon Comprehend. Comprehend provides natural language processing capabilities including sentiment analysis and topic classification. You can train a custom classifier on labeled examples or use the built-in pre-trained models. This automates the manual process of reading and categorizing text.

Question Thirty-Three. A company is building a customer service chatbot for their website that can understand natural language questions about orders and returns. What service should they use to build the conversational interface?

The answer is Amazon Lex. Lex provides the natural language understanding and conversation management for building chatbots and voice interfaces. Developers define intents — things the user wants to do — and the utterances that express each intent, and Lex matches user input to the appropriate intent and manages multi-turn conversation.

Question Thirty-Four. A company needs to migrate their Oracle database to Amazon Aurora PostgreSQL. The schema and stored procedures need to be converted to PostgreSQL syntax as part of the migration. What combination of services do they use?

The answer is the AWS Schema Conversion Tool to translate the schema and stored procedures from Oracle syntax to PostgreSQL syntax, and AWS DMS, Database Migration Service, to migrate the actual data with minimal downtime. This is the standard heterogeneous database migration approach — different source and target engines require schema conversion before or alongside data migration.

Question Thirty-Five. A company wants to discover all the servers running in their on-premises data center and understand the dependencies between applications before planning a migration to AWS. What service helps with this?

The answer is AWS Application Discovery Service. It collects server inventory and performance data from on-premises environments and maps application dependencies, giving migration teams a clear picture of what needs to move and what depends on what.

Question Thirty-Six. A company is running a large-scale migration of fifty applications from on-premises to AWS using multiple migration tools simultaneously. They want a single dashboard to see the progress of all migrations. What service provides this?

The answer is AWS Migration Hub. Migration Hub aggregates migration status from multiple tools and services into a single view, allowing teams to track which applications have been migrated, which are in progress, and which have not started.

Question Thirty-Seven. An application sends messages to a queue, and a processing service reads those messages. When the processing service is temporarily slow or unavailable, no messages should be lost. What service provides this behavior?

The answer is Amazon SQS. SQS holds messages in the queue until a consumer explicitly retrieves and deletes them. If the consumer is slow, messages accumulate in the queue. If the consumer fails, messages remain in the queue and will be processed when the consumer recovers. No messages are lost. This decoupling is the core value of SQS.

Question Thirty-Eight. A company wants to send the same notification to five different services simultaneously when an order is placed: an inventory service, a shipping service, an analytics service, an email notification, and a mobile push notification. What service should they use?

The answer is Amazon SNS. SNS uses a publish/subscribe model where a single message published to a topic is fanned out to all subscribers simultaneously. The five services each subscribe to the order topic, and when an order is placed, all five receive the message in parallel.

Question Thirty-Nine. A development team pushes code to a Git repository and wants that push to automatically trigger a build, run automated tests, and if tests pass, deploy to production. What service orchestrates this pipeline?

The answer is AWS CodePipeline. CodePipeline connects the repository — which could be CodeCommit, GitHub, or another source — to CodeBuild for compilation and testing, and to CodeDeploy for deployment, orchestrating the full continuous delivery pipeline automatically.

Question Forty. Which service would you use to automatically check whether your EC2 instances are sized correctly and identify instances where you are paying for more capacity than you are actually using?

The answer is AWS Compute Optimizer. Compute Optimizer uses machine learning to analyze actual utilization data from CloudWatch and recommends optimal instance types and sizes for EC2 instances, EBS volumes, Lambda functions, and Auto Scaling groups. It identifies both over-provisioned and under-provisioned resources.

Question Forty-One. A company wants to ensure that every EC2 instance launched across all accounts in their organization uses an approved Amazon Machine Image and that no instance can be launched in certain high-risk Regions. What is the appropriate governance approach?

The answer is AWS Organizations with Service Control Policies, or SCPs. SCPs are permission guardrails that apply across entire OUs or accounts. You can write an SCP that denies the RunInstances API call for non-approved AMIs or for specific Regions. No entity within the restricted account or OU can override an SCP restriction, even if they have administrator permissions in their account.

Question Forty-Two. A company is setting up a new multi-account AWS environment from scratch and wants to implement AWS security and architecture best practices as the foundation, including centralized logging, guardrails, and an account vending machine for creating new accounts. What service provides this?

The answer is AWS Control Tower. Control Tower automates the setup of a well-architected multi-account environment with centralized logging through CloudTrail and Config, security guardrails implemented as SCPs, and the ability to create new accounts with approved baseline configurations through Account Factory.

Question Forty-Three. A company uses Auto Scaling for their web application. The application typically uses ten instances during business hours and three instances overnight. They want to configure Auto Scaling to add instances at eight in the morning and remove them at eight in the evening, matching their known traffic pattern. What type of Auto Scaling policy should they use?

The answer is a scheduled scaling policy. Scheduled scaling changes capacity at specific times you define. Because the traffic pattern is entirely predictable — business hours every day — there is no need for reactive policies that respond to metrics. Scheduled scaling proactively prepares capacity before the load arrives.

Question Forty-Four. A company runs a web application behind an Application Load Balancer. They want the load balancer to route requests for the path /api to a set of API servers and requests for the path /web to a set of web servers, using the same load balancer and the same domain name. What type of load balancer supports this?

The answer is the Application Load Balancer. ALBs support content-based routing rules that evaluate HTTP attributes including the path, host header, query string, and headers. Path-based routing to different target groups is a core ALB feature. A Network Load Balancer does not inspect HTTP content and cannot perform path-based routing.

Question Forty-Five. CloudFront is serving cached copies of your website's images from Edge Locations. A user in Tokyo requests an image that is not yet in the Tokyo Edge Location's cache. What happens?

The answer is that CloudFront forwards the request to the origin server — your S3 bucket or application server — retrieves the image, caches it at the Tokyo Edge Location, and returns it to the user. Subsequent requests for the same image from Tokyo are served directly from the Tokyo Edge Location cache without contacting the origin again. This is how CDN caching works: cache miss fetches from origin and populates the cache, cache hit serves locally.

---

## Key Patterns to Remember

Let me close with the patterns that separate the correct answers from the plausible distractors.

For EC2 pricing, match the workload characteristic to the model. Unpredictable workloads go to On-Demand. Predictable steady-state workloads go to Reserved Instances or Savings Plans. Fault-tolerant batch workloads go to Spot. Per-socket software licenses go to Dedicated Hosts.

For databases, the key distinction is between workload type. Relational data with complex queries and transactions goes to RDS. NoSQL key-value at massive scale with single-digit millisecond latency goes to DynamoDB. Analytics queries over very large datasets go to Redshift. Highly connected data where relationships matter goes to Neptune. In-memory caching for reducing database load goes to ElastiCache.

For storage, the S3 storage class selection depends on access frequency and retrieval tolerance. Frequent access with no retrieval delay: S3 Standard. Infrequent access with immediate retrieval: S3 Standard-IA or Glacier Instant Retrieval. Archival with hours-level retrieval acceptable: Glacier Flexible Retrieval. Archival with next-day retrieval acceptable and essentially no expected access: Glacier Deep Archive. Unpredictable access patterns: Intelligent-Tiering.

For shared file systems, EFS for Linux workloads using NFS, FSx for Windows for Windows workloads using SMB with Active Directory.

For high availability versus performance in RDS: Multi-AZ is availability, Read Replicas are read performance.

For messaging, SNS is for fan-out to multiple subscribers simultaneously. SQS is for point-to-point decoupling with guaranteed delivery and no lost messages. Step Functions is for multi-step workflow orchestration.

For analytics, Athena for ad-hoc SQL queries on data already in S3. Glue for ETL transformations between formats and sources. QuickSight for business intelligence dashboards. Kinesis for real-time streaming data ingestion and processing.

For AI and ML, SageMaker when you build custom models. Rekognition for image and video analysis. Comprehend for text analysis and sentiment. Lex for conversational interfaces. Polly for text to speech. Transcribe for speech to text. Translate for language translation. Textract for extracting structured data from documents.

For networking and connectivity, CloudFront for caching static content at the edge. Global Accelerator for routing all traffic over the AWS network without caching. Direct Connect for private dedicated high-bandwidth connectivity. VPN for encrypted internet-based connectivity that can be set up quickly.

These patterns are the backbone of Domain Three. When a scenario describes a problem, identify the category — compute, storage, database, networking, messaging, analytics, AI — and then apply the matching pattern to select the right service.

---
