# Lesson 8: Service Comparisons and How to Choose the Right AWS Service

---

Welcome back. This lesson is different from everything else in the course. We are not covering theory or hands-on steps. We are doing something the exam demands constantly and that most students find surprisingly difficult: choosing between services.

Every domain of the AWS Cloud Practitioner exam includes scenario questions. The exam describes a situation and asks which AWS service or approach is the right fit. The problem is that AWS has hundreds of services and many of them appear to overlap. Students who only read about services in isolation get confused when two services seem like they could both work. The secret to answering these questions correctly is understanding not just what each service does, but what it is designed for and what it is not good for.

That is what this lesson is about. We are going to go category by category and build your ability to make the right choice quickly. I will give you spoken decision guides, memory anchors, and the key distinctions that appear most often on the exam. There are no tables here, no charts, no code. Just the comparisons you need, explained in plain language.

---

## Storage Services: The Four You Must Know

Storage is one of the most tested categories on this exam because AWS offers multiple storage services that students routinely confuse. Let me give you the four main ones and the single question that separates them.

The question is: what kind of data, accessed by how many things?

Amazon S3 is object storage. That means files, images, videos, documents, backups — things you store as complete units and access via the internet or internally using a URL. S3 is serverless, meaning you never provision capacity. You just upload data and it scales automatically. S3 is the right answer whenever a question talks about storing files that need to be accessed from anywhere, hosting static website content, building a data lake, storing backups and archives, or distributing content globally through CloudFront. The durability of S3 is eleven nines, which means it is effectively impossible to lose an object. That specific number appears on the exam.

Amazon EBS is block storage. Think of it like a hard drive. It attaches to an EC2 instance and behaves exactly like a disk attached to a physical server. EBS is the right answer when a question talks about a boot volume for an EC2 instance, a database running on EC2 that needs consistent performance, or an application that needs to read and write blocks of data directly. The critical constraint with EBS is that it can only be attached to one EC2 instance at a time. If the question says multiple servers need access, EBS is off the table.

Amazon EFS is a network file system. The key characteristic that separates it from EBS is that multiple EC2 instances can mount EFS simultaneously. It behaves like a shared drive on a network. EFS is the right answer when a question describes a web farm where all servers need to share the same files, a content management system that needs a shared file system, or a machine learning setup where multiple training nodes need access to the same dataset. If the question says multiple instances need shared access to a file system, EFS is almost certainly the answer.

Instance Store is ephemeral block storage. It is physically attached to the host server that runs your EC2 instance. The critical word here is ephemeral, meaning temporary. If your EC2 instance stops, the data in the Instance Store is gone. It is not persisted. It is not backed up. Instance Store is the right answer when a question talks about temporary scratch space, caches, buffers, or data that is already replicated elsewhere and does not need to survive a restart. It offers the highest possible input/output performance, but at the cost of permanence.

Here is the memory anchor. S3 is for things you want to keep forever and access from anywhere. EBS is a private hard drive for one instance. EFS is a shared drive for many instances. Instance Store is the fastest scratch pad that disappears when the instance stops.

---

## S3 Storage Classes

Within S3 itself, there are storage classes that trade retrieval speed for cost. The exam tests whether you can match a scenario to the right class.

S3 Standard is for data that is accessed frequently. It is the default and carries the highest per-gigabyte price, but retrieval is instant and free. Use it for active application data, website content, and anything accessed daily.

S3 Standard Infrequent Access, often abbreviated as Standard-IA, is for data that is accessed occasionally rather than regularly. The per-gigabyte storage cost is lower than Standard, but there is a retrieval fee. Use it for backups that you might need, disaster recovery files, or data accessed a few times per month.

S3 One Zone Infrequent Access stores data in only one Availability Zone instead of three. This reduces cost further, but if that Availability Zone fails, the data could be lost. Use it for non-critical data that can be recreated, like processed thumbnails from images you still have in Standard.

S3 Glacier Instant Retrieval is for archive data that you occasionally need urgently. Storage cost is very low, and retrieval is still measured in milliseconds.

S3 Glacier Flexible Retrieval is for archive data you rarely need. Retrieval takes one to five minutes for expedited, three to five hours for standard. Use it for regulatory archives and long-term backups where you can plan ahead.

S3 Glacier Deep Archive is the cheapest storage in AWS. Retrieval takes twelve hours. Use it for data that must be kept for compliance but will almost never be accessed, like seven-year regulatory archives. If the exam describes a scenario with the lowest possible storage cost and long retrieval times are acceptable, Glacier Deep Archive is the answer.

S3 Intelligent-Tiering is the class for unpredictable access patterns. It monitors your access patterns and automatically moves objects between frequent and infrequent access tiers without a retrieval penalty. Use it when you do not know how often data will be accessed.

---

## Database Services: Matching the Right Tool to the Workload

Database questions are among the most common on this exam. The core decision is relational versus NoSQL, but within each category there are important distinctions.

Amazon RDS is the managed relational database service. It supports MySQL, PostgreSQL, Oracle, SQL Server, and MariaDB. RDS is the right answer when a question describes an existing application that uses SQL, a traditional business application with structured data, or any scenario that mentions ACID transactions, joins, or a specific database engine. RDS handles patching, backups, and failover. You choose the instance size and AWS does the rest.

Amazon Aurora is AWS's own cloud-native relational database. It is compatible with MySQL and PostgreSQL, meaning you can use the same drivers and applications, but Aurora is up to five times faster than standard MySQL and three times faster than standard PostgreSQL. Aurora automatically grows its storage from ten gigabytes up to one hundred twenty-eight terabytes without you having to provision space. Aurora supports up to fifteen read replicas, compared to five for other RDS engines. When the exam asks for the highest performance relational database or the one that scales storage automatically, Aurora is the answer.

Amazon DynamoDB is a fully managed NoSQL database. The key characteristics are that it stores key-value and document data, it scales horizontally to handle millions of requests per second, and it delivers single-digit millisecond latency at any scale. DynamoDB is the right answer when a question describes gaming leaderboards, session management, shopping carts, IoT telemetry, mobile application backends, or any workload that needs massive scale and very fast access but does not require complex joins or SQL. The moment a question mentions joins, complex transactions across multiple tables, or SQL queries, DynamoDB is not the answer.

Amazon Redshift is a data warehouse. It is optimized for OLAP, which stands for Online Analytical Processing. While RDS and DynamoDB are designed for transactional workloads where you are reading and writing individual records constantly, Redshift is designed for analytical workloads where you are running complex queries across enormous datasets to answer business questions. When a question mentions business intelligence, data warehousing, running reports across years of historical data, or petabyte-scale analytics, Redshift is the answer.

Amazon ElastiCache is an in-memory caching service. It supports two engines: Redis and Memcached. ElastiCache is not a database itself — it is a layer that sits in front of your database to speed up repeated reads. When a question describes a situation where an application is slow because the same database queries run repeatedly and the data does not change often, ElastiCache is the caching solution. Redis is the more feature-rich option with support for data persistence, pub/sub messaging, sorted sets, and replication. Memcached is simpler and is best when you only need basic caching. The exam usually just says ElastiCache without specifying the engine.

Amazon Neptune is a managed graph database. Use it when a question describes social networks, fraud detection, recommendation engines, or any scenario that involves relationships between entities rather than tabular data. Neptune is not commonly tested in depth on the Cloud Practitioner exam, but you should be able to recognize the use case.

---

## Compute Services: From Full Control to No Servers

The compute category has the widest range of options, from full control over a virtual server to completely serverless code execution. The exam tests your ability to match the right service to the right scenario.

Amazon EC2 is the virtual server service. EC2 gives you full control over the operating system, the software installed, the networking configuration, and the storage attached. EC2 is the right answer when the question requires a specific operating system, when an application needs to be lifted and shifted from on-premises without modification, when GPU compute is required, when the workload is consistent and predictable, or when the application has licensing restrictions that require dedicated hardware. EC2 is not the right answer when the question emphasizes not managing servers or when the workload is event-driven and intermittent.

AWS Lambda is serverless function execution. You write code, deploy it to Lambda, and it runs in response to events. You never see a server, never provision anything, and pay only when your code is actually running. Lambda is the right answer when a question describes processing an image when it is uploaded to S3, responding to an API request, running a scheduled task, or processing events from a database stream. The most important Lambda constraint to memorize is the fifteen-minute maximum execution timeout. If a question describes a task that takes longer than fifteen minutes, Lambda is disqualified.

AWS Fargate is serverless container execution. The difference between Lambda and Fargate is that Lambda runs individual functions while Fargate runs containers. If your application is packaged as a Docker container and needs to run continuously, Fargate eliminates the need to manage EC2 instances. Fargate works with both ECS and EKS. Use Fargate when a question describes containerized applications that should not require managing server infrastructure.

Elastic Beanstalk is a platform-as-a-service for deploying web applications. You give it your application code and tell it what language or runtime you are using, and it handles the underlying infrastructure including EC2 instances, load balancers, and auto scaling. Elastic Beanstalk is for developers who want to deploy applications quickly without thinking about infrastructure. You still have access to the underlying resources if you need them, unlike Lambda where the infrastructure is completely hidden.

Amazon ECS is the Elastic Container Service. It is AWS's native container orchestration platform for running Docker containers at scale. ECS manages clusters of containers, handles deployment, scaling, and networking. Use ECS when the question mentions containers and AWS-native tooling without requiring Kubernetes.

Amazon EKS is the Elastic Kubernetes Service. It runs a managed Kubernetes control plane so you can use standard Kubernetes tooling. Use EKS when the question specifically mentions Kubernetes, when teams have existing Kubernetes expertise, or when portability across cloud providers matters.

AWS Lightsail is the simplest compute option. It provides virtual servers, databases, storage, and networking in a bundled package with predictable monthly pricing. Lightsail is designed for simple websites, blogs, and small applications where the team does not need the full complexity of EC2. When a question describes a small business launching a simple website on a fixed budget, Lightsail is often the answer.

---

## Load Balancers: Three Types with Different Strengths

AWS offers three current load balancer types, and the exam tests which one to use in a given scenario.

The Application Load Balancer, commonly called the ALB, operates at Layer Seven of the network stack, which is the application layer. That means it understands HTTP and HTTPS traffic and can make routing decisions based on the content of the request. For example, it can send requests to the path slash API to one group of servers and requests to the path slash images to a different group. It also supports WebSockets, integration with Lambda functions as targets, and advanced health checks. When a question describes a web application, a microservices architecture, routing based on URL path or host header, or HTTPS termination, ALB is the answer.

The Network Load Balancer, commonly called the NLB, operates at Layer Four, which is the transport layer. It works with TCP, UDP, and TLS traffic and is built for extreme performance: millions of requests per second with ultra-low latency. NLB supports static IP addresses and Elastic IPs, which is important when clients need a fixed IP address to allow-list. When a question describes non-HTTP traffic, extreme throughput requirements, the need for a static IP on the load balancer, or preserving the original client IP address, NLB is the answer.

The Gateway Load Balancer is used to deploy third-party virtual network appliances like firewalls, intrusion detection systems, and deep packet inspection tools. When a question describes routing traffic through a security appliance before it reaches your application, Gateway Load Balancer is the answer. This is less commonly tested at the Cloud Practitioner level, but you should be able to recognize the scenario.

The Classic Load Balancer is an older generation that AWS discourages. If the exam mentions it, know that it is being phased out and that ALB or NLB should replace it.

---

## Networking: VPC Components and Hybrid Connectivity

VPC networking questions test whether you understand what each component does within a network architecture.

The Internet Gateway is what connects a VPC to the public internet. Without an internet gateway attached and a route pointing traffic to it, nothing in your VPC can communicate with the outside world and nothing from the internet can reach your resources. When a question says an EC2 instance in a public subnet cannot connect to the internet, the answer usually involves a missing internet gateway or a missing route to it.

The NAT Gateway allows resources in private subnets to initiate outbound connections to the internet without being reachable from the internet. For example, an EC2 instance in a private subnet needs to download software updates. It sends that request through the NAT Gateway, which forwards it to the internet and returns the response, without ever exposing the private instance directly. The NAT Gateway is deployed in a public subnet and requires an internet gateway on the VPC to work. It carries an hourly charge plus a data processing fee, which makes it a common source of unexpected costs.

The NAT Instance is an older alternative to NAT Gateway. It is an EC2 instance you manage yourself, configured to perform network address translation. NAT Instances are cheaper but less reliable and require more management. When an exam question contrasts them, AWS recommends NAT Gateway unless the question specifically calls out cost constraints or legacy architecture.

VPC Peering connects two VPCs directly so resources in each can communicate using private IP addresses. The most important fact about VPC peering is that it is non-transitive. If VPC A is peered with VPC B, and VPC B is peered with VPC C, VPC A cannot communicate with VPC C through VPC B. You must create a direct peering connection between A and C. Also, peered VPCs cannot have overlapping IP address ranges.

The Transit Gateway solves the problem of connecting many VPCs. Instead of creating peering connections between every pair of VPCs, each VPC connects to a central Transit Gateway, which routes traffic between them. Think of it as a cloud router that simplifies complex multi-VPC architectures. The Transit Gateway also supports connectivity to on-premises networks.

VPC Endpoints allow resources inside your VPC to communicate with AWS services like S3 and DynamoDB without the traffic going over the public internet. There are two types. The Gateway Endpoint is a route table entry and is only available for S3 and DynamoDB. The Interface Endpoint uses a private network interface with a private IP address and supports most other AWS services. Using VPC endpoints can reduce data transfer costs because traffic stays on the AWS network rather than going through a NAT Gateway.

For connecting your on-premises data center to AWS, you have two main options. A VPN Gateway creates an encrypted connection over the public internet. Setup is measured in hours. It is cost-effective and appropriate for moderate traffic volumes. AWS Direct Connect is a dedicated private network connection between your data center and AWS. It bypasses the public internet entirely, providing consistent bandwidth and lower latency. Direct Connect takes weeks to provision and is more expensive, but it is the right answer when a question emphasizes consistent performance, high bandwidth, or compliance requirements for private connectivity.

PrivateLink provides private connectivity to services in other VPCs or to AWS services without requiring VPC peering or an internet gateway. When a question describes accessing a service in another account's VPC privately, PrivateLink is usually the answer.

---

## Security Services: Matching the Right Tool

Security is thirty percent of the exam, and the security services are some of the most frequently confused.

IAM is for controlling access to AWS resources by internal users and services. When the question is about who can do what inside your AWS account, the answer involves IAM. Users, groups, roles, and policies are all IAM concepts.

Amazon Cognito is for authenticating the users of your application. When you build a mobile app or web application and need sign-up, sign-in, and user management for your customers, Cognito handles that. IAM is for your developers and services. Cognito is for your end users. These two are commonly confused on the exam.

AWS Secrets Manager stores and automatically rotates sensitive credentials like database passwords and API keys. When a question describes an application that needs to retrieve a database password securely without hardcoding it, or a situation where credentials need to be rotated automatically, Secrets Manager is the answer.

AWS KMS, the Key Management Service, creates and manages encryption keys. When a question asks how data is encrypted at rest, KMS is typically involved. It integrates with most AWS services so that you can encrypt data with a key you control.

AWS CloudHSM provides dedicated hardware security modules for generating and storing encryption keys. It is used when regulatory requirements mandate that you have exclusive control over the hardware managing your cryptographic keys. It is more expensive than KMS and requires more management. If the exam mentions regulatory compliance requiring dedicated hardware for encryption, CloudHSM is the answer.

AWS WAF is the Web Application Firewall. It protects your web applications from common attacks like SQL injection and cross-site scripting by inspecting HTTP traffic before it reaches your application. WAF integrates with CloudFront, ALB, and API Gateway.

AWS Shield Standard provides automatic DDoS protection for all AWS customers at no extra cost. It protects against the most common network and transport layer attacks. Shield Advanced is the paid tier, costing three thousand dollars per month, and adds protection against more sophisticated application-layer attacks, a twenty-four-seven DDoS response team, and cost protection so that you are not billed for AWS resources consumed during a DDoS attack. When a question asks about DDoS protection and mentions advanced features or the response team, Shield Advanced is the answer.

Amazon GuardDuty continuously monitors your AWS account for malicious activity and unauthorized behavior by analyzing VPC flow logs, DNS logs, and CloudTrail events. GuardDuty is the threat detection service. It does not prevent attacks — it detects them and alerts you. When a question describes detecting unusual API calls, compromised instances, or suspicious network traffic, GuardDuty is the answer.

Amazon Inspector automatically scans your EC2 instances and container images for known software vulnerabilities and unintended network exposure. Where GuardDuty detects active threats, Inspector identifies vulnerabilities before they are exploited.

Amazon Macie uses machine learning to discover and protect sensitive data in S3. It identifies personally identifiable information, financial data, and other sensitive content and alerts you when that data is not adequately protected. When a question describes discovering PII or sensitive data in S3, Macie is the answer.

---

## Monitoring Services: The Critical Trio

CloudWatch, CloudTrail, and AWS Config are three monitoring services that the exam tests together because students consistently confuse them. Let me give you the clearest possible distinction.

Amazon CloudWatch monitors the performance of your running AWS resources. It collects metrics like CPU utilization, network throughput, and database connections. It stores logs from applications and services. It evaluates those metrics against thresholds you define and sends alerts when a threshold is crossed. If you want to know that an EC2 instance's CPU usage has been above eighty percent for five minutes, CloudWatch is the service that tells you. The question CloudWatch answers is: how is everything performing right now?

AWS CloudTrail records API calls. Every action taken in your AWS account, through the Console, through the command line, or through a program, generates an API call, and CloudTrail logs that call. It tells you who did what, when they did it, and from where. CloudTrail is your audit trail. If someone deleted an S3 bucket at two in the morning and you want to know who did it, CloudTrail has the answer. The question CloudTrail answers is: who did what in my account?

AWS Config tracks the configuration of your AWS resources over time. It records what your resources look like at any point in time and can compare them against rules you define. If you want to know whether your EC2 instances all have encryption enabled, or whether any security group allows unrestricted inbound access, Config is what enforces and audits those rules. The question Config answers is: are my resources configured the way they should be, and when did they change?

Memorize these three distinctions. CloudWatch is about performance metrics. CloudTrail is about API activity and who did what. Config is about resource configuration and compliance. The exam will give you scenarios that could superficially apply to more than one of these, but the distinctions above will guide you to the right answer every time.

---

## Serverless: Lambda, Fargate, and Step Functions

Within the serverless category, three services work together and are often confused.

Lambda runs code in response to events. It is stateless, meaning each invocation is independent. Lambda is triggered by events from S3, DynamoDB, API Gateway, SQS, SNS, and dozens of other services. It handles the scaling automatically, running as many parallel instances as needed. Maximum execution time is fifteen minutes.

Fargate runs containers without managing servers. Unlike Lambda, containers can run continuously and maintain state through attached storage. Fargate is the answer when the workload is containerized and runs longer than fifteen minutes, or when the team is already using containers and wants to eliminate server management.

Step Functions orchestrates workflows across multiple Lambda functions or services. When you have a business process that involves a sequence of steps, conditional branching, error handling, and retries, Step Functions manages the coordination. Think of it as a state machine that knows what step a workflow is on, what to do next, and how to handle failures. When a question describes coordinating multiple Lambda functions into a pipeline or managing a long-running multi-step process, Step Functions is the answer.

---

## Migration Services

The migration category tests your ability to choose the right tool based on data volume, data type, and network bandwidth.

The Snow Family consists of physical devices AWS ships to you so you can copy data onto them and ship them back. Snowcone holds up to eight terabytes and is the size of a small box. Snowball Edge holds up to eighty terabytes and includes compute capability for edge processing. Snowmobile is a semi-truck-sized container holding up to one hundred petabytes. The Snow Family is the right answer when a question describes very large amounts of data that would take too long to transfer over the internet, or when the network connection to AWS is slow or unreliable. The general rule is: if transferring the data over your network would take more than a week, consider Snow Family.

AWS Database Migration Service, called DMS, migrates databases to AWS with minimal downtime. It supports homogeneous migrations, like MySQL to MySQL, and heterogeneous migrations, like Oracle to Aurora. DMS can replicate data continuously so that your source database stays active until you are ready to cut over. When the question involves migrating a database, DMS is the answer.

AWS DataSync transfers file data between on-premises storage and AWS storage services like S3, EFS, or FSx. It validates data integrity and can schedule recurring transfers. DataSync is for file migration and synchronization, not database migration.

AWS Application Migration Service, sometimes called MGN, automates the lift-and-shift migration of physical servers and virtual machines to EC2. It continuously replicates your servers at the block level so that when you are ready to cut over, the downtime is measured in minutes.

---

## Analytics Services

The analytics category spans from simple SQL queries to large-scale machine learning pipelines. The exam tests which tool fits which scenario.

Amazon Athena lets you run SQL queries directly against data stored in S3 without loading it into a database first. It is serverless and charges per terabyte of data scanned. Athena is the right answer when a question describes ad-hoc analysis of data already in S3, or when a team needs to query log files or CSV data without setting up a database.

AWS Glue is a managed extract, transform, and load service. It prepares data for analytics by discovering its schema, cleaning it, and moving it between data stores. Glue Crawlers automatically infer the structure of your data and populate the Glue Data Catalog, which is a metadata repository that Athena and other services can use to find data. When a question describes preparing data for analysis, transforming data between formats, or building an ETL pipeline, Glue is the answer.

Amazon EMR runs large-scale data processing using frameworks like Apache Spark, Hadoop, and Hive. EMR is for complex, large-scale processing pipelines that need the full ecosystem of open-source big data tools. When a question describes petabyte-scale processing, machine learning at scale, or existing Spark or Hadoop workloads moving to AWS, EMR is the answer.

Amazon Kinesis is for real-time data streaming. When data is being generated continuously and you need to process it as it arrives — like clickstream data from a website, IoT sensor readings, or financial transaction streams — Kinesis captures and processes that stream in real time. Kinesis is the answer when the question emphasizes real-time or streaming rather than batch processing.

Amazon QuickSight is the business intelligence service. It creates dashboards and visualizations from data across services like Athena, Redshift, RDS, and S3. When a question describes executives needing charts and dashboards to understand business trends, QuickSight is the answer.

Amazon Redshift is the data warehouse for complex analytical queries across large datasets. It was covered in the database section, but in the analytics context, Redshift is the answer when data from multiple sources needs to be joined and analyzed in complex queries, especially for petabyte-scale datasets.

---

## AI and Machine Learning Services

AWS provides two categories of AI services: pre-trained services you can call via an API without any machine learning expertise, and SageMaker for building custom models.

Amazon SageMaker is the platform for data scientists who need to build, train, and deploy custom machine learning models. It requires expertise. When a question describes a company with a data science team building a custom model on their own data, SageMaker is the answer.

For most exam questions, you will match a pre-trained service to a use case. Here is the map.

Amazon Rekognition analyzes images and videos to identify objects, scenes, faces, and text. When a question describes detecting inappropriate content in uploaded photos, identifying people in a video, or reading text in images, Rekognition is the answer.

Amazon Comprehend performs natural language processing. It analyzes text to extract sentiment, identify entities like names and places, detect the language, and find key phrases. When a question describes analyzing customer reviews for sentiment or extracting information from text documents, Comprehend is the answer.

Amazon Transcribe converts speech to text. When a question describes transcribing call center recordings or generating subtitles for video, Transcribe is the answer.

Amazon Polly converts text to speech. When a question describes reading text aloud or building an automated voice system, Polly is the answer.

Amazon Translate converts text between languages. When a question describes real-time translation or localizing content, Translate is the answer.

Amazon Lex builds conversational chatbots and voice interfaces. Lex powers Amazon Alexa. When a question describes a customer service chatbot or a voice assistant application, Lex is the answer.

Amazon Textract extracts structured text and data from scanned documents. Unlike simple OCR, Textract understands the layout of forms and tables. When a question describes extracting data from invoices, medical forms, or government documents, Textract is the answer.

Amazon Kendra is an intelligent enterprise search service. When a question describes employees needing to search across company documents and knowledge bases using natural language questions, Kendra is the answer.

---

## Common Misconceptions: What the Exam Expects You to Know

Let me go through the most common wrong assumptions that trip students up on this exam.

The first misconception is that Multi-AZ on RDS improves read performance. It does not. Multi-AZ creates a standby replica in a different Availability Zone for failover purposes only. If the primary instance fails, AWS promotes the standby automatically. But the standby does not serve any traffic while the primary is healthy. To scale read performance, you use Read Replicas. The exam tests this distinction regularly.

The second misconception is that VPC peering is transitive. It is not. If you peer VPC A with VPC B, and VPC B with VPC C, VPC A still cannot communicate with VPC C. You must create direct peering between every pair that needs to communicate. Transit Gateway solves this by acting as a hub.

The third misconception is that cross-AZ traffic is free. Moving data between EC2 instances in different Availability Zones in the same region costs one cent per gigabyte in each direction. This is a real cost that architects must account for. The exam may use this to test whether you understand data transfer pricing.

The fourth misconception is that private subnets are inherently secure. A private subnet only means that the subnet does not have a route to the internet gateway. Resources inside a private subnet can still be reached by other resources within the VPC. Security groups and network ACLs are what actually control access. Do not assume private means protected.

The fifth misconception is that Lambda scales infinitely. Lambda does scale automatically, but there is a default concurrent execution limit of one thousand across your account. If more than one thousand Lambda functions try to run simultaneously, additional invocations are throttled. This limit can be increased, but it is not unlimited by default.

The sixth misconception is that the root account is for everyday use. The root account has unlimited permissions and cannot be restricted. Best practice is to use the root account only for tasks that specifically require it, like closing the account or changing the billing method. All daily operations should use IAM users or roles.

The seventh misconception is that S3 is only for backups and archives. S3 is a general-purpose object storage service that powers active applications, static websites, data lakes, content distribution, and much more. Never dismiss S3 as a backup-only service.

---

## Service Anti-Patterns: Things You Should Not Do

The exam sometimes describes a correct architecture and asks which element is wrong. Knowing the common anti-patterns helps you spot the wrong answer.

Using S3 as a database is an anti-pattern. S3 is object storage and is not designed for transactional data access with updates, deletes, and complex queries. If a question describes updating individual records frequently, S3 is not the right tool. Use DynamoDB or RDS.

Using Lambda for long-running processes is an anti-pattern. If a process might take more than fifteen minutes, Lambda is the wrong tool. Use ECS with Fargate or an EC2-based approach instead.

Storing files or binary objects in a relational database is an anti-pattern. Store the metadata in RDS or DynamoDB and the actual files in S3. Databases are not designed for large binary objects and storing them there degrades performance and drives up storage costs.

Using On-Demand EC2 instances for stable, predictable workloads is a cost anti-pattern. If a workload runs continuously and its usage is predictable, Reserved Instances or Savings Plans should be used instead. On-Demand pricing is designed for variable or unpredictable workloads.

Placing databases in public subnets is a security anti-pattern. Databases should live in private subnets and be accessible only from the application layer, never directly from the internet. When an exam question describes a database in a public subnet, that is usually the thing to fix.

Not using VPC endpoints when connecting to S3 or DynamoDB from inside a VPC is a cost anti-pattern. Without VPC endpoints, traffic from inside a VPC to S3 or DynamoDB goes through a NAT Gateway, which charges a data processing fee. A gateway endpoint for S3 and DynamoDB is free and eliminates that cost.

---

## Service Limits: Hard Versus Soft

The exam tests a handful of specific service limits, and the critical distinction is whether a limit can be increased.

Soft limits are default limits that can be increased by submitting a request through the Service Quotas console or creating a support case. Examples include the number of EC2 instances per region, the number of VPCs per region, the number of S3 buckets per account, the number of Lambda concurrent executions, and the number of CloudFormation stacks per region. If you hit a soft limit and need more, you can get more.

Hard limits are absolute maximums that cannot be increased regardless of how you ask. You must design your architecture to work within them. The ones the exam is most likely to test are these: Lambda functions can execute for a maximum of fifteen minutes. An S3 object can be at most five terabytes. Lambda functions have at most ten gigabytes of temporary storage in the slash tmp directory. Lambda deployment packages cannot exceed two hundred fifty megabytes when unzipped. RDS can have at most five Read Replicas for most engines, while Aurora supports up to fifteen.

The fifteen-minute Lambda timeout and the five-terabyte S3 object size limit appear most frequently on the exam. If a question describes a workload or a file that exceeds those limits, you know Lambda or direct S3 single-part upload is not the right answer.

---

## A Final Word on Choosing the Right Service

The exam gives you scenarios and asks you to pick from four options. The fastest path to the right answer is almost always to read the scenario for the key constraint, then match that constraint to a service characteristic.

If the scenario emphasizes no servers to manage and short execution time, think Lambda. If it says multiple EC2 instances need shared storage, think EFS. If it says consistent bandwidth to on-premises and private connection, think Direct Connect. If it says detect who made unauthorized API calls, think CloudTrail. If it says the company needs to monitor application performance and set up alarms, think CloudWatch. If it says the company wants to enforce configuration compliance across all resources, think Config.

Every service comparison in this lesson was built around that pattern: find the key constraint in the scenario, and match it to the service that was designed to address exactly that constraint. That is how the exam is written, and that is how you should read every question.

Work through the service comparisons in this lesson until the distinctions are automatic. When a question says multiple servers need shared file access, EFS should come to mind before you finish the sentence. When a question says audit trail and API calls, CloudTrail should be there immediately. That automatic recognition is what separates students who pass comfortably from those who struggle on the hard questions.

---
