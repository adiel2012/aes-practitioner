# Comprehensive Service Comparisons and Decision Trees

[← Previous: Hands-On Labs](07-hands-on-labs.md) | [Back to Main](README.md) | [Next: Exam Scenarios →](09-exam-scenarios.md)

This chapter provides detailed comparisons of AWS services to help you choose the right service for specific use cases. These comparisons are critical for the exam.

## Storage Services Detailed Comparison

| Feature | Amazon S3 | Amazon EBS | Amazon EFS | Instance Store |
|---------|-----------|------------|------------|----------------|
| **Type** | Object Storage | Block Storage | File Storage | Ephemeral Block |
| **Use Case** | Backups, archives, web content, data lakes | Boot volumes, databases, transactional data | Shared file systems, content management | Temporary data, caches, buffers |
| **Access** | HTTP/S API, SDK, CLI | Attached to EC2 instance | NFSv4 protocol, multiple EC2 instances | Direct attached to EC2 |
| **Durability** | 11 nines (99.999999999%) | Replicated within AZ | Replicated across AZs in Region | Lost when instance stops |
| **Scalability** | Unlimited | Up to 64 TB per volume | Petabyte scale | Fixed to instance type |
| **Performance** | Varies by class | Up to 256,000 IOPS | Scales with size | Highest IOPS for instance |
| **Cost** | Low per GB, varies by class | $0.08-0.125/GB-month | $0.30/GB-month | Included with instance |
| **Availability** | 99.9-99.99% SLA | 99.8-99.9% | 99.99% | Dependent on instance |
| **Backup** | Versioning, lifecycle | Snapshots to S3 | AWS Backup | Not persistent |
| **Multi-AZ** | Yes | No (single AZ) | Yes | No |
| **Concurrent Access** | Unlimited | Single EC2 instance | Thousands of instances | Single instance |

### When to Use Each Storage Service

**Use S3 when:**
- Storing static website content
- Backup and archiving
- Data lakes for analytics
- Distributing software/media
- Disaster recovery

**Use EBS when:**
- EC2 boot volumes required
- Databases requiring consistent IOPS
- Applications needing block storage
- Single instance access sufficient

**Use EFS when:**
- Multiple instances need shared access
- Content management systems
- Web serving from shared storage
- Big data analytics requiring shared access
- Lift-and-shift of on-premises applications

**Use Instance Store when:**
- Temporary storage needs
- Cache and buffers
- Scratch data
- Data replicated across instances
- Highest possible IOPS needed

## Database Services Detailed Comparison

| Service | Type | Use Case | Scaling | Pricing Model | Managed |
|---------|------|----------|---------|---------------|---------|
| **RDS** | Relational (SQL) | Traditional apps, OLTP | Vertical, Read Replicas | Instance + storage | Fully managed |
| **Aurora** | Relational (MySQL/PostgreSQL compatible) | High-performance OLTP | Auto-scaling storage | Serverless or provisioned | Fully managed |
| **DynamoDB** | NoSQL Key-Value | Web/mobile apps, gaming, IoT | Auto horizontal | On-demand or provisioned | Fully managed |
| **Redshift** | Data Warehouse (OLAP) | Analytics, BI, big data | Add nodes | Nodes + storage | Fully managed |
| **ElastiCache** | In-memory cache | Session stores, leaderboards | Add nodes | Nodes (hourly) | Fully managed |
| **Neptune** | Graph database | Social networks, recommendations | Vertical | Instances | Fully managed |
| **DocumentDB** | Document (MongoDB compatible) | Content management, catalogs | Vertical, replicas | Instances | Fully managed |
| **Timestream** | Time series | IoT, DevOps metrics | Auto | Storage + queries | Fully managed |

### Database Selection Decision Tree

```
Need SQL and ACID transactions?
├─ Yes
│  ├─ Traditional app, existing database → RDS
│  ├─ Need extreme performance → Aurora
│  └─ Specific needs: Oracle/SQL Server → RDS with that engine
│
└─ Need NoSQL?
   ├─ Key-value, scale to millions of requests/sec → DynamoDB
   ├─ Document database, MongoDB compatible → DocumentDB
   └─ Graph relationships → Neptune

Need caching?
├─ In-memory cache → ElastiCache
├─ Redis features needed → ElastiCache for Redis
└─ Simple caching → ElastiCache for Memcached

Need data warehouse/analytics?
├─ OLAP, BI, big data → Redshift
├─ Query data in S3 → Athena
└─ Real-time analytics → Kinesis Data Analytics

Time-series data?
└─ IoT, metrics, logs → Timestream
```

## Compute Services Decision Tree

### Choose Your Compute Service

**Need full control over OS and configuration?**
- Yes → Use **EC2**
  - Want to save costs for predictable workloads? → Use **Reserved Instances**
  - Fault-tolerant batch workloads? → Use **Spot Instances**

**Want to run code without managing servers?**
- Event-driven, < 15 min execution → Use **Lambda**
- Long-running, stateless → Use **Fargate**

**Need to deploy web applications quickly?**
- Simple deployment, don't want infrastructure management → Use **Elastic Beanstalk**
- Need predictable pricing for small projects → Use **Lightsail**

**Using containers?**
- Want AWS-native orchestration → Use **ECS**
- Need Kubernetes compatibility → Use **EKS**
- Don't want to manage servers → Use **Fargate** (with ECS or EKS)

**Running batch processing jobs?**
- Use **AWS Batch**

### Compute Service Selection Table

| Requirement | Best Choice | Why |
|-------------|-------------|-----|
| Full OS control | EC2 | Complete flexibility |
| Serverless, event-driven | Lambda | No servers, pay per use |
| Deploy app quickly | Elastic Beanstalk | PaaS, managed |
| Containers, Kubernetes | EKS | Kubernetes compatible |
| Containers, AWS native | ECS | Simpler than EKS |
| Containers, no servers | Fargate | Serverless containers |
| Batch processing | AWS Batch | Optimized for batch |
| Simple website, blog | Lightsail | Predictable pricing |

## Networking Components Deep Dive

| Component | Purpose | Key Points |
|-----------|---------|------------|
| **Internet Gateway (IGW)** | Connect VPC to internet | One per VPC; enables internet access for public subnets |
| **NAT Gateway** | Outbound internet from private subnets | Highly available; placed in public subnet; charged per hour + data |
| **NAT Instance** | Alternative to NAT Gateway | EC2 instance; you manage; lower cost but less reliable |
| **VPC Peering** | Connect two VPCs | Non-transitive; can be cross-account/region; no overlapping CIDRs |
| **Transit Gateway** | Hub for connecting VPCs | Simplifies complex network topologies; central management |
| **VPN Gateway** | VPN connection to on-premises | IPsec VPN; encrypted over internet; quick setup |
| **Direct Connect** | Dedicated connection to on-premises | Private, consistent bandwidth; expensive; takes weeks to provision |
| **VPC Endpoints** | Private connection to AWS services | No internet required; Interface or Gateway endpoints; reduce costs |
| **PrivateLink** | Private connectivity to services | Access services in other VPCs; doesn't require VPC peering |

### VPC Components Decision Guide

**Public vs. Private Subnet:**
- **Public Subnet**: Has route to Internet Gateway (0.0.0.0/0 → IGW)
- **Private Subnet**: No direct internet access; uses NAT for outbound

**NAT Gateway vs. NAT Instance:**
- **NAT Gateway**: AWS managed, highly available, easier, more expensive
- **NAT Instance**: EC2 instance you manage, cheaper, less reliable

**VPC Peering vs. Transit Gateway:**
- **VPC Peering**: Connect two VPCs directly, non-transitive
- **Transit Gateway**: Hub-and-spoke model, simplifies multiple VPC connections

**VPN vs. Direct Connect:**
- **VPN**: Quick setup (hours), encrypted, over internet, variable performance
- **Direct Connect**: Dedicated link, consistent performance, expensive, takes weeks

**VPC Endpoint Types:**
- **Interface Endpoint**: ENI with private IP, uses PrivateLink
- **Gateway Endpoint**: Route table entry, for S3 and DynamoDB only

## Load Balancer Detailed Comparison

| Feature | ALB | NLB | Gateway LB |
|---------|-----|-----|------------|
| **OSI Layer** | Layer 7 (Application) | Layer 4 (Transport) | Layer 3 (Network) |
| **Protocol** | HTTP, HTTPS, WebSocket | TCP, UDP, TLS | IP |
| **Routing** | Path-based, host-based, query string | IP address, port | N/A |
| **Use Case** | Web applications, microservices | High performance, low latency, static IP | Third-party appliances |
| **Target Types** | IP, instance, Lambda | IP, instance, ALB | IP, instance |
| **Performance** | Good | Extreme (millions req/sec) | High |
| **Static IP** | No (DNS only) | Yes (Elastic IP) | N/A |
| **SSL Termination** | Yes | Yes | No |
| **WebSocket** | Yes | Yes | No |
| **Health Checks** | Advanced | Basic | Advanced |
| **Pricing** | Per hour + LCU | Per hour + LCU | Per hour + LCU |

### When to Use Each Load Balancer

**Application Load Balancer (ALB):**
- Web applications
- Microservices architectures
- Need advanced routing (path, host, header-based)
- Target Lambda functions
- Need WebSocket support
- Content-based routing required

**Network Load Balancer (NLB):**
- Extreme performance required (millions of requests/second)
- Ultra-low latency needed
- Static IP addresses required
- TCP/UDP traffic
- Non-HTTP protocols
- Preserve source IP address

**Gateway Load Balancer:**
- Deploy third-party virtual appliances
- Firewalls, IDS/IPS
- Deep packet inspection
- Transparent network gateway

**Classic Load Balancer (CLB):**
- Legacy applications
- EC2-Classic network
- Being phased out - migrate to ALB or NLB

## Security Services Complete Matrix

| Service | What It Does | When to Use |
|---------|-------------|-------------|
| **IAM** | Identity and access management | Control who can access what in AWS |
| **AWS Organizations** | Multi-account management | Centralize billing, apply policies across accounts |
| **AWS SSO** | Single sign-on | Centrally manage access to multiple accounts and applications |
| **Cognito** | User authentication for apps | Add sign-up/sign-in to mobile and web apps |
| **Directory Service** | Managed Active Directory | Integrate AWS with existing Microsoft AD |
| **Secrets Manager** | Store and rotate secrets | Automatically rotate database credentials |
| **KMS** | Encryption key management | Create and control encryption keys |
| **CloudHSM** | Hardware security modules | Dedicated hardware for regulatory compliance |
| **Certificate Manager** | SSL/TLS certificates | Free certificates for ELB, CloudFront, API Gateway |
| **WAF** | Web application firewall | Protect against SQL injection, XSS attacks |
| **Shield Standard** | DDoS protection | Automatic protection (free) |
| **Shield Advanced** | Enhanced DDoS protection | 24/7 DDoS Response Team, cost protection ($3,000/month) |
| **GuardDuty** | Threat detection | Continuous monitoring for malicious activity |
| **Inspector** | Vulnerability assessment | Scan EC2 and container images for vulnerabilities |
| **Macie** | Data privacy and protection | Discover and protect sensitive data in S3 |
| **Detective** | Security investigation | Analyze and investigate security issues |
| **Security Hub** | Security posture management | Centralized view of security alerts and compliance |
| **Firewall Manager** | Centralized firewall management | Manage WAF, Shield across accounts |

### Security Service Selection Guide

**Identity and Access:**
- Internal AWS users/services → IAM
- External app users → Cognito
- Multiple AWS accounts → Organizations + SSO
- On-premises AD integration → Directory Service

**Data Protection:**
- Encryption key management → KMS
- Regulatory compliance encryption → CloudHSM
- Store secrets → Secrets Manager
- Discover sensitive data → Macie

**Threat Protection:**
- DDoS protection → Shield (Standard for all, Advanced for critical)
- Web application attacks → WAF
- Threat detection → GuardDuty
- Vulnerability scanning → Inspector
- Security investigation → Detective

**Compliance and Governance:**
- Multi-account security → Security Hub
- Multi-account firewall → Firewall Manager
- Configuration compliance → Config
- Audit logging → CloudTrail

## Service Limits Quick Reference

| Service | Default Limit | Notes |
|---------|--------------|-------|
| **EC2 Instances (On-Demand)** | 20 per region | Can request increase |
| **VPCs per Region** | 5 | Can request increase |
| **Internet Gateways per Region** | 5 | One per VPC typically |
| **S3 Buckets per Account** | 100 | Soft limit, can increase |
| **S3 Object Size** | 5 TB max | Use multipart upload for > 100 MB |
| **RDS DB Instances** | 40 per region | Can request increase |
| **Lambda Concurrent Executions** | 1,000 | Can request increase |
| **Lambda Function Timeout** | 15 minutes max | Cannot be increased |
| **CloudFormation Stacks** | 200 per region | Can request increase |
| **IAM Users per Account** | 5,000 | Use roles/federated identities instead |
| **IAM Groups per Account** | 300 | Plan group structure carefully |

> **Exam Tip:** Most service limits can be increased by requesting through AWS Support. Some hard limits (like Lambda 15-minute timeout) cannot be changed.

## Storage Selection Guide

| Use Case | Service | Reason |
|----------|---------|--------|
| Backups, archives | S3 Glacier | Lowest cost |
| Website content | S3 + CloudFront | Scalable, global |
| EC2 boot volume | EBS | Block storage |
| Shared file system | EFS | Multi-attach |
| Windows file shares | FSx for Windows | SMB, AD integration |
| HPC workloads | FSx for Lustre | High performance |
| Temporary data | Instance Store | Highest IOPS |
| Offline transfer | Snow Family | Large data volumes |

## Key Comparison Summary

### Storage Decision Matrix
- **Objects/Files** → S3
- **Block storage for EC2** → EBS
- **Shared file system** → EFS
- **Archive** → S3 Glacier
- **Large migration** → Snow Family

### Database Decision Matrix
- **Relational/SQL** → RDS or Aurora
- **NoSQL key-value** → DynamoDB
- **Caching** → ElastiCache
- **Data warehouse** → Redshift
- **Graph** → Neptune

### Compute Decision Matrix
- **Full control** → EC2
- **Serverless functions** → Lambda
- **Containers with orchestration** → ECS/EKS
- **Serverless containers** → Fargate
- **Simple web apps** → Lightsail
- **Quick app deployment** → Elastic Beanstalk

### Networking Decision Matrix
- **Private network** → VPC
- **Content delivery** → CloudFront
- **DNS** → Route 53
- **Load balancing** → ALB/NLB
- **Hybrid connectivity** → Direct Connect or VPN

## Serverless Services Comparison

| Service | Type | Use Case | Execution Model | Pricing | Integration |
|---------|------|----------|----------------|---------|-------------|
| **Lambda** | Function as a Service | Event-driven code execution, APIs, automation | Up to 15 min per execution | Per request + duration | All AWS services, API Gateway |
| **Fargate** | Serverless containers | Long-running containers, microservices | Continuous | Per vCPU + memory per hour | ECS, EKS |
| **AppSync** | GraphQL API | Real-time and offline mobile/web apps | Request-based | Per query + data transfer | DynamoDB, Lambda, RDS |
| **Step Functions** | Workflow orchestration | Coordinate multiple Lambda functions, services | State machine execution | Per state transition | Lambda, ECS, SNS, SQS, etc. |

### Serverless Service Selection

**Use Lambda when:**
- Event-driven processing (S3 uploads, DynamoDB streams)
- API backends via API Gateway
- Scheduled tasks (CloudWatch Events)
- Real-time file/stream processing
- Execution time < 15 minutes
- Stateless operations

**Use Fargate when:**
- Long-running applications
- Microservices that run continuously
- Migrating Docker containers to serverless
- Need more than 15 minutes execution
- Application requires persistent connections
- Want container benefits without managing servers

**Use AppSync when:**
- Building GraphQL APIs
- Real-time data synchronization needed
- Offline mobile app support required
- Multiple data sources (DynamoDB, Lambda, RDS)
- Need built-in caching and subscriptions

**Use Step Functions when:**
- Coordinating multiple Lambda functions
- Complex workflows with branching logic
- Need retry and error handling
- Long-running workflows (up to 1 year)
- Visual workflow management required
- Orchestrating microservices

## Analytics Services Comparison

| Service | Type | Best For | Data Source | Query Method | Pricing |
|---------|------|----------|-------------|--------------|---------|
| **Athena** | Interactive query | Ad-hoc SQL queries on S3 | S3 (CSV, JSON, Parquet, ORC) | SQL via Presto | Per TB scanned |
| **EMR** | Big Data platform | Hadoop, Spark workloads | S3, HDFS | MapReduce, Spark, Hive, Pig | Per instance hour |
| **Redshift** | Data warehouse | OLAP, BI, complex queries | S3, databases via COPY | SQL (PostgreSQL compatible) | Per node hour |
| **QuickSight** | BI visualization | Dashboards and reports | Athena, Redshift, RDS, S3 | Visual interface | Per user/session |
| **Glue** | ETL service | Data preparation, cataloging | S3, RDS, Redshift | Python/Scala (Spark) | Per DPU hour |
| **Kinesis** | Real-time streaming | Real-time data processing | Streams, applications, IoT | Kinesis API, SQL | Per shard hour |
| **Lake Formation** | Data lake | Build and secure data lakes | S3 | Integrated with Athena, EMR | Storage + query costs |
| **MSK** | Managed Kafka | Streaming with Kafka | Applications | Kafka API | Per broker hour |

### Analytics Service Selection Guide

**Use Athena when:**
- Querying data already in S3
- Serverless, no infrastructure to manage
- Ad-hoc analysis and exploration
- Cost-effective for infrequent queries
- No need to load data into database

**Use EMR when:**
- Need full Hadoop/Spark ecosystem
- Complex data processing pipelines
- Custom big data frameworks
- ML model training at scale
- Have existing Hadoop workloads

**Use Redshift when:**
- Data warehouse for OLAP workloads
- Complex joins across large datasets
- BI tools need fast query performance
- Petabyte-scale analytics
- Consistent query performance needed

**Use QuickSight when:**
- Creating business dashboards
- Visualizing data from multiple sources
- Sharing insights with stakeholders
- Mobile-friendly BI needed
- Cost-effective per-user pricing

**Use Glue when:**
- ETL jobs to prepare data
- Automatic schema discovery needed
- Data catalog for analytics services
- Moving data between data stores
- Serverless ETL without managing servers

**Use Kinesis when:**
- Real-time data streaming
- Log and event data collection
- Real-time analytics
- Click stream analysis
- IoT telemetry processing

## Migration Tools Comparison

| Tool | Purpose | Use Case | Data Size | Transfer Method | Timeframe |
|------|---------|----------|-----------|----------------|-----------|
| **MGN (Application Migration Service)** | Lift-and-shift server migration | Migrate physical/virtual servers to EC2 | Any | Network (agent-based replication) | Days to weeks |
| **DMS (Database Migration Service)** | Database migration | Migrate databases with minimal downtime | Databases | Network (continuous replication) | Hours to days |
| **DataSync** | Data transfer | Migrate file data to/from AWS | GBs to PBs | Network (accelerated) | Hours to days |
| **Snow Family** | Physical data transfer | Offline bulk data transfer | TBs to exabytes | Physical device shipping | 1-2 weeks |
| **Transfer Family** | SFTP/FTPS/FTP | Migrate file transfer workflows | Any | Network (legacy protocols) | Immediate |
| **Migration Hub** | Migration tracking | Centralized migration planning and tracking | N/A | Dashboard | Ongoing |

### Migration Tool Details

**AWS Application Migration Service (MGN):**
- Formerly CloudEndure Migration
- Automated lift-and-shift for servers
- Continuous block-level replication
- Minimal downtime (minutes)
- Supports physical, virtual, cloud servers
- Free for 90 days (pay only for resources)

**Database Migration Service (DMS):**
- Homogeneous migrations (Oracle to Oracle)
- Heterogeneous migrations (Oracle to Aurora)
- Continuous replication for minimal downtime
- Schema Conversion Tool (SCT) for different engines
- Supports most commercial and open-source databases

**DataSync:**
- 10x faster than open-source tools
- Automates data transfer
- Validates data integrity
- Use for: NFS/SMB to EFS, FSx
- Bandwidth optimization and encryption
- Schedule transfers

**Snow Family:**
- **Snowcone**: 8 TB usable, edge computing
- **Snowball Edge**: 80 TB usable, compute options
- **Snowmobile**: 100 PB, truck-sized container
- Use when: Limited bandwidth, weeks to transfer over network
- Includes compute for edge processing
- Ruggedized for harsh environments

**Transfer Family:**
- Managed SFTP, FTPS, FTP service
- Stores files in S3 or EFS
- Migrate legacy file transfer workflows
- Integrate with existing authentication
- Fully managed, no servers to maintain

### Migration Decision Tree

```
How much data to migrate?
├─ < 10 TB with good bandwidth → DataSync over network
├─ 10-80 TB or limited bandwidth → Snowcone/Snowball
└─ > 80 TB or very limited bandwidth → Snowball Edge/Snowmobile

What type of data?
├─ Servers/VMs → MGN (Application Migration Service)
├─ Databases → DMS (Database Migration Service)
├─ Files/NAS → DataSync or Snow Family
└─ SFTP workflows → Transfer Family

Need ongoing synchronization?
├─ Yes → DataSync (scheduled) or DMS (continuous)
└─ No → One-time Snow Family transfer

Network bandwidth available?
├─ > 100 Mbps → DataSync
├─ 10-100 Mbps → Consider Snowcone
└─ < 10 Mbps → Snow Family required
```

## Container Orchestration Detailed Comparison

| Feature | ECS | EKS | Fargate (with ECS) | Fargate (with EKS) |
|---------|-----|-----|-------------------|-------------------|
| **Orchestrator** | AWS proprietary | Kubernetes | ECS (serverless) | Kubernetes (serverless) |
| **Learning Curve** | Easier | Steeper | Easiest | Moderate |
| **Control Plane** | Free | $0.10/hour per cluster | Free | $0.10/hour per cluster |
| **Infrastructure Management** | You manage EC2 | You manage nodes | AWS manages | AWS manages |
| **Kubernetes Compatibility** | No | Yes | No | Yes |
| **Use Case** | AWS-native containers | Kubernetes workloads | Serverless containers | Serverless Kubernetes |
| **Pricing** | EC2 costs only | Cluster + EC2 costs | Per vCPU + memory | Per vCPU + memory |
| **Add-ons/Ecosystem** | AWS services | Kubernetes ecosystem | AWS services | Both |
| **Multi-cloud** | AWS only | Portable | AWS only | More portable |

### Container Service Selection

**Use ECS when:**
- Deep AWS integration preferred
- Don't need Kubernetes
- Want simpler container orchestration
- Team familiar with AWS services
- No existing Kubernetes investment
- Cost-sensitive (no control plane cost)

**Use EKS when:**
- Need Kubernetes compatibility
- Existing Kubernetes workloads
- Want portability across clouds
- Team has Kubernetes expertise
- Need Kubernetes ecosystem tools
- Hybrid/multi-cloud strategy

**Use Fargate (with ECS or EKS) when:**
- Don't want to manage servers
- Variable workloads
- Want pay-per-task pricing
- Security isolation per task
- Quick deployment without infrastructure
- Development and testing environments

**Container Launch Types:**
- **EC2 Launch Type**: You manage EC2 instances, more control, potentially lower cost
- **Fargate Launch Type**: Serverless, no EC2 management, pay per task

### Container Architecture Patterns

**ECS Architecture:**
- Task Definition (container config)
- Service (maintains desired count)
- Cluster (logical grouping)
- EC2 or Fargate launch type

**EKS Architecture:**
- Managed Kubernetes control plane
- Worker nodes (EC2 or Fargate)
- Standard Kubernetes objects (Pods, Deployments)
- Compatible with kubectl and Kubernetes tools

## Monitoring and Logging Services Comparison

| Service | Purpose | What It Monitors | Data Retention | Use Case | Pricing |
|---------|---------|-----------------|----------------|----------|---------|
| **CloudWatch** | Monitoring and observability | Metrics, logs, events, alarms | 15 months (metrics), custom (logs) | Performance monitoring, alarms | Per metric, per GB ingested |
| **CloudTrail** | API activity logging | All API calls in AWS account | 90 days (event history), indefinite (S3) | Security auditing, compliance | First trail free, $2/100k events |
| **Config** | Resource configuration tracking | AWS resource configurations | Configurable | Compliance, change tracking | Per config item recorded |
| **X-Ray** | Distributed tracing | Application performance, requests | 30 days | Debugging, performance analysis | Per trace recorded + retrieved |
| **VPC Flow Logs** | Network traffic logging | Network interfaces in VPC | S3 or CloudWatch | Network troubleshooting, security | Storage costs only |
| **EventBridge** | Event bus | Events from AWS, SaaS, custom | N/A (routing) | Event-driven architectures | Per million events |

### Monitoring Service Details

**CloudWatch:**
- **Metrics**: CPU, disk, network for EC2, RDS, etc.
- **Logs**: Application logs, system logs
- **Alarms**: Trigger actions based on metrics
- **Dashboards**: Visualize metrics
- **Events/EventBridge**: React to state changes
- **Insights**: Query and analyze log data
- **Synthetics**: Monitor endpoints and APIs
- Default vs. detailed monitoring

**CloudTrail:**
- Logs every API call in your account
- Who, what, when, where for all actions
- Essential for security and compliance
- Integrates with CloudWatch Logs for alerts
- Multi-region and organization trails
- Integrity validation with log file hashing

**Config:**
- Records configuration changes over time
- Configuration history for resources
- Compliance checking with Config Rules
- Integrates with Systems Manager for remediation
- Relationship tracking between resources
- Multi-account, multi-region support

**X-Ray:**
- End-to-end request tracing
- Service map visualization
- Identify performance bottlenecks
- Track requests across microservices
- Integrates with Lambda, ECS, Elastic Beanstalk
- Annotations and metadata for filtering

### Monitoring Decision Guide

**Use CloudWatch when:**
- Need performance metrics and alarms
- Monitoring resource utilization
- Setting up automated responses to metrics
- Collecting and analyzing logs
- Creating operational dashboards

**Use CloudTrail when:**
- Security auditing required
- Compliance and governance needs
- Tracking who did what in AWS
- Investigating security incidents
- Forensic analysis of API activity

**Use Config when:**
- Need resource inventory
- Tracking configuration changes over time
- Compliance auditing against rules
- Understanding resource relationships
- Point-in-time configuration history

**Use X-Ray when:**
- Debugging distributed applications
- Analyzing microservices performance
- Identifying latency issues
- Tracing requests across services
- Understanding application behavior

## AI/ML Services Comparison

| Service | Type | Use Case | Expertise Required | Training Required | Pricing |
|---------|------|----------|-------------------|-------------------|---------|
| **SageMaker** | Full ML platform | Custom ML models, training, deployment | High (data scientists) | Yes, custom models | Per instance hour + storage |
| **Rekognition** | Computer vision | Image/video analysis, face detection | Low (API calls) | No, pre-trained | Per image/video analyzed |
| **Comprehend** | NLP | Text analysis, sentiment, entities | Low (API calls) | No, pre-trained | Per unit of text |
| **Lex** | Conversational AI | Chatbots, voice assistants | Medium | Yes, train intents | Per request |
| **Polly** | Text-to-speech | Convert text to lifelike speech | Low (API calls) | No, pre-trained | Per character |
| **Transcribe** | Speech-to-text | Convert audio to text | Low (API calls) | No, pre-trained | Per second of audio |
| **Translate** | Translation | Real-time text translation | Low (API calls) | No, pre-trained | Per character |
| **Forecast** | Time-series | Time-series forecasting | Medium | Yes, train on your data | Per forecast + training hours |
| **Personalize** | Recommendations | Recommendation engines | Medium | Yes, train on your data | Per training hour + usage |
| **Textract** | Document analysis | Extract text from documents | Low (API calls) | No, pre-trained | Per page analyzed |
| **Kendra** | Intelligent search | Enterprise search with NLP | Low | Yes, index documents | Per hour + queries |

### AI/ML Service Details

**SageMaker (Custom ML):**
- Build, train, and deploy custom ML models
- Jupyter notebooks for development
- Built-in algorithms and frameworks
- AutoML with Autopilot
- Model monitoring and debugging
- For data scientists and ML engineers

**Rekognition (Computer Vision):**
- Object and scene detection
- Facial analysis and recognition
- Text in images (OCR)
- Content moderation
- Celebrity recognition
- Video analysis

**Comprehend (Natural Language Processing):**
- Sentiment analysis (positive, negative, neutral)
- Entity recognition (people, places, dates)
- Key phrase extraction
- Language detection
- Topic modeling
- PII detection

**Lex (Conversational Interfaces):**
- Build chatbots and voice assistants
- Powers Amazon Alexa
- Natural language understanding
- Multi-turn conversations
- Integration with Lambda for fulfillment
- Deploy to mobile, web, messaging platforms

**Polly (Text-to-Speech):**
- 60+ voices in 30+ languages
- Neural TTS for most natural voice
- SSML support for customization
- Speech marks for lip-syncing
- Lexicons for pronunciation
- Ideal for accessibility, IVR systems

**Transcribe (Speech-to-Text):**
- Real-time and batch transcription
- Speaker identification
- Custom vocabulary
- Automatic language identification
- Channel identification
- Medical and call analytics versions

### AI/ML Selection Guide

**Use SageMaker when:**
- Building custom ML models
- Have data science team
- Need full control over training
- Complex ML requirements
- Want to use specific algorithms

**Use Pre-trained AI Services when:**
- No ML expertise required
- Quick integration via API
- Standard use cases covered
- Cost-effective for most scenarios
- Don't want to manage training

**By Use Case:**
- Analyze images/videos → Rekognition
- Analyze text sentiment → Comprehend
- Build chatbot → Lex
- Text to speech → Polly
- Speech to text → Transcribe
- Translate languages → Translate
- Extract from documents → Textract
- Product recommendations → Personalize
- Sales forecasting → Forecast
- Enterprise search → Kendra
- Custom ML → SageMaker

## Enhanced Decision Trees

### Networking Architecture Decision Tree

```
What do you need to connect?

AWS Resources to Internet
├─ From public subnet → Internet Gateway
├─ From private subnet → NAT Gateway (or NAT Instance)
└─ Private access to AWS services → VPC Endpoints

Multiple VPCs
├─ 2 VPCs → VPC Peering
├─ 3+ VPCs → Transit Gateway
└─ Shared services to many VPCs → PrivateLink

On-Premises to AWS
├─ Quick setup, encrypted → VPN Gateway
├─ Consistent bandwidth, private → Direct Connect
├─ Redundancy needed → Multiple VPN or Direct Connect + VPN
└─ Low latency required → Direct Connect

Content Delivery
├─ Static content globally → CloudFront + S3
├─ Dynamic content → CloudFront + ALB
└─ DNS routing → Route 53

Load Balancing
├─ HTTP/HTTPS → Application Load Balancer (ALB)
├─ TCP/UDP, extreme performance → Network Load Balancer (NLB)
├─ Third-party appliances → Gateway Load Balancer
└─ Legacy (avoid) → Classic Load Balancer

Security
├─ Web application protection → WAF + ALB/CloudFront
├─ DDoS protection → Shield (Standard/Advanced)
├─ Network ACLs → Subnet-level firewall (stateless)
└─ Security Groups → Instance-level firewall (stateful)
```

### Disaster Recovery Strategy Selector

```
What's your RTO/RPO requirement?

RTO: Minutes, RPO: Seconds
└─ Multi-Site Active/Active
   - Hot standby in multiple regions
   - Highest cost, immediate failover
   - Use: Route 53 health checks, Aurora Global Database
   - Cost: 200% of single site

RTO: < 1 hour, RPO: Minutes
└─ Warm Standby
   - Scaled-down version running in DR region
   - Can scale up quickly
   - Use: Scaled-down EC2, read replicas promoted
   - Cost: 50-100% of single site

RTO: Hours, RPO: Minutes to Hours
└─ Pilot Light
   - Core components always running (e.g., databases)
   - Full environment provisioned on failover
   - Use: RDS standby, data replicated to S3
   - Cost: 20-50% of single site

RTO: Days, RPO: Hours
└─ Backup and Restore
   - Regular backups to S3/Glacier
   - Restore from backup on disaster
   - Use: AWS Backup, S3 cross-region replication
   - Cost: < 10% of single site (storage only)

Implementation Patterns:
- Database replication → RDS Multi-AZ, Aurora replicas
- Data replication → S3 Cross-Region Replication
- Infrastructure as Code → CloudFormation for quick rebuild
- Traffic management → Route 53 health checks and failover
- Continuous sync → DataSync, Storage Gateway
```

### Data Analytics Platform Selector

```
What type of analytics?

Real-Time Analytics
├─ Streaming data → Kinesis Data Streams + Analytics
├─ Real-time dashboards → QuickSight with SPICE
└─ ML on streams → Kinesis + Lambda + SageMaker

Batch Analytics
├─ SQL on S3 data → Athena
├─ Complex ETL → Glue
├─ Large-scale processing → EMR (Spark/Hadoop)
└─ Data warehouse → Redshift

Interactive Exploration
├─ Ad-hoc queries → Athena
├─ Notebook-based → SageMaker notebooks or EMR notebooks
└─ Quick insights → QuickSight

Business Intelligence
├─ Dashboards and reports → QuickSight
├─ Complex queries → Redshift + QuickSight
└─ Self-service BI → QuickSight with datasets

Data Preparation
├─ Visual ETL → Glue DataBrew
├─ Code-based ETL → Glue (PySpark)
├─ Schema discovery → Glue Crawlers
└─ Data catalog → Glue Data Catalog

Machine Learning
├─ Feature engineering → Glue, SageMaker Data Wrangler
├─ Model training → SageMaker
├─ ML on big data → EMR with Spark MLlib
└─ AutoML → SageMaker Autopilot

Data Volume Considerations:
- < 1 TB → Athena, RDS with analytics
- 1-100 TB → Redshift
- 100+ TB → Redshift, EMR, or S3 data lake with Athena
- Petabyte scale → EMR, S3 data lake
```

### Container Deployment Decision Flowchart

```
Do you need Kubernetes?
├─ Yes
│  ├─ Want to manage nodes → EKS with EC2
│  └─ Serverless → EKS with Fargate
│
└─ No
   ├─ Want to manage hosts → ECS with EC2
   └─ Serverless → ECS with Fargate or just Lambda

How much control do you need?
├─ Full OS control → EC2 with Docker (not orchestrated)
├─ Container orchestration → ECS or EKS
└─ Just run code → Lambda

How long does it run?
├─ < 15 minutes, event-driven → Lambda
├─ Continuous → ECS/EKS with Fargate or EC2
└─ Batch jobs → AWS Batch

Team expertise?
├─ Know Kubernetes well → EKS
├─ AWS experience → ECS
├─ Want simplest → Fargate with ECS
└─ Minimal DevOps → Lambda or Elastic Beanstalk

Cost optimization?
├─ Spot Instances → ECS/EKS with EC2 Spot
├─ Reserved Instances → ECS/EKS with EC2 RIs
├─ Pay per use → Fargate or Lambda
└─ Savings Plans → Fargate with Compute Savings Plans

Deployment pattern:
ECS with EC2:
- Register container instances to cluster
- Define task definitions
- Create services with desired count
- Use Application Load Balancer

ECS with Fargate:
- No instances to manage
- Define task definitions (CPU/memory)
- Create services
- AWS handles infrastructure

EKS with EC2:
- Managed Kubernetes control plane
- Self-managed worker nodes
- Standard kubectl commands
- Use Kubernetes services/ingress

EKS with Fargate:
- No nodes to manage
- Define Fargate profiles
- Deploy Kubernetes pods
- AWS provisions compute
```

### Monitoring Solution Selector

```
What do you need to monitor?

Application Performance
├─ Metrics and alarms → CloudWatch
├─ Distributed tracing → X-Ray
├─ Real user monitoring → CloudWatch RUM
└─ Synthetic monitoring → CloudWatch Synthetics

Security and Compliance
├─ API activity → CloudTrail
├─ Configuration changes → Config
├─ Threat detection → GuardDuty
├─ Vulnerability scanning → Inspector
└─ Centralized security → Security Hub

Resource Changes
├─ Configuration history → Config
├─ Change notifications → EventBridge
└─ Drift detection → CloudFormation

Network Traffic
├─ VPC traffic → VPC Flow Logs
├─ DNS queries → Route 53 Resolver Query Logs
└─ CloudFront access → CloudFront Logs

Cost Monitoring
├─ Cost tracking → Cost Explorer
├─ Budget alerts → AWS Budgets
├─ Anomaly detection → Cost Anomaly Detection
└─ Rightsizing → Compute Optimizer

Logging Strategy:
Application Logs → CloudWatch Logs
Access Logs → S3, CloudWatch Logs
Audit Logs → CloudTrail to S3
Flow Logs → CloudWatch Logs or S3

Monitoring Architecture:
1. Collect: CloudWatch Agent, X-Ray SDK, CloudTrail
2. Store: CloudWatch Logs, S3, CloudWatch Metrics
3. Analyze: CloudWatch Insights, Athena on S3
4. Alert: CloudWatch Alarms, SNS, EventBridge
5. Visualize: CloudWatch Dashboards, QuickSight
```

## Cost Comparison Matrices

### Storage Cost Comparison (per GB-month)

| Storage Type | Hot/Frequent | Warm/Infrequent | Cold/Archive | Use Case |
|-------------|--------------|-----------------|--------------|----------|
| **S3 Standard** | $0.023 | - | - | Active data, frequent access |
| **S3 Intelligent-Tiering** | $0.023 + $0.0025 monitoring | Auto-moves | Auto-moves | Unknown access patterns |
| **S3 Standard-IA** | - | $0.0125 + $0.01/GB retrieval | - | Infrequent but fast access |
| **S3 One Zone-IA** | - | $0.01 + $0.01/GB retrieval | - | Non-critical infrequent |
| **S3 Glacier Instant** | - | $0.004 + $0.03/GB retrieval | - | Archive, instant retrieval |
| **S3 Glacier Flexible** | - | - | $0.0036 + retrieval costs | Archive, 1-5 min retrieval |
| **S3 Glacier Deep Archive** | - | - | $0.00099 + retrieval costs | Long-term archive, 12 hrs |
| **EBS gp3** | $0.08 | - | - | General purpose SSD |
| **EBS io2** | $0.125 + IOPS cost | - | - | High performance SSD |
| **EBS st1** | $0.045 | - | - | Throughput optimized HDD |
| **EBS sc1** | $0.015 | - | - | Cold HDD |
| **EFS Standard** | $0.30 | - | - | Shared file system |
| **EFS IA** | - | $0.025 | - | Infrequent access files |
| **FSx for Windows** | $0.13 | - | $0.013 (backup) | Windows file shares |
| **FSx for Lustre** | $0.145-0.240 | - | - | HPC workloads |

**Storage Cost Optimization Tips:**
- Use S3 Lifecycle policies to move to cheaper tiers
- Enable S3 Intelligent-Tiering for unpredictable access
- Use EBS snapshots to S3 for backups (cheaper than keeping unused volumes)
- Delete unused EBS volumes
- Use EFS IA for files not accessed in 30+ days

### Database Cost Comparison (per hour)

| Database | Small Instance | Medium Instance | Large Instance | Storage | Notes |
|----------|---------------|-----------------|----------------|---------|-------|
| **RDS MySQL db.t3.micro** | $0.017 | - | - | $0.115/GB-month | General purpose |
| **RDS MySQL db.t3.medium** | - | $0.068 | - | $0.115/GB-month | |
| **RDS MySQL db.m5.large** | - | - | $0.188 | $0.115/GB-month | |
| **Aurora MySQL** | - | $0.082 (db.t3.medium) | $0.29 (db.r5.large) | $0.10/GB-month | Auto-scaling storage |
| **Aurora Serverless v2** | - | - | - | $0.12/ACU-hour + $0.10/GB | Auto-scales, min 0.5 ACU |
| **DynamoDB On-Demand** | - | - | - | $1.25/million writes, $0.25/million reads | Pay per request |
| **DynamoDB Provisioned** | - | - | - | $0.00065/WCU-hour, $0.00013/RCU-hour | Reserved capacity available |
| **Redshift dc2.large** | - | $0.25 | - | 160 GB SSD included | Min 1 node |
| **Redshift ra3.xlplus** | - | - | $1.086 | Managed storage $0.024/GB | Scalable storage |
| **ElastiCache t3.micro** | $0.017 | - | - | Included | Redis or Memcached |
| **ElastiCache r5.large** | - | - | $0.243 | Included | Memory-optimized |
| **DocumentDB r5.large** | - | - | $0.277 | $0.10/GB-month | MongoDB compatible |
| **Neptune db.t3.medium** | - | $0.073 | - | $0.10/GB-month | Graph database |

**Database Cost Optimization:**
- Use Aurora Serverless v2 for variable workloads
- Reserved Instances for RDS (up to 69% savings)
- DynamoDB On-Demand for unpredictable traffic
- DynamoDB Provisioned with auto-scaling for predictable
- Stop dev/test RDS instances when not in use
- Use read replicas instead of larger instances

### Compute Pricing Comparison (per hour)

| Instance Type | On-Demand | Spot (avg 70% off) | 1-Year Reserved | 3-Year Reserved | Savings Plan 1-Year |
|---------------|-----------|-------------------|-----------------|-----------------|---------------------|
| **t3.micro** | $0.0104 | ~$0.003 | $0.0062 (40% off) | $0.0042 (60% off) | Similar to RI |
| **t3.medium** | $0.0416 | ~$0.012 | $0.0250 (40% off) | $0.0166 (60% off) | Similar to RI |
| **m5.large** | $0.096 | ~$0.029 | $0.058 (40% off) | $0.038 (60% off) | Similar to RI |
| **m5.xlarge** | $0.192 | ~$0.058 | $0.115 (40% off) | $0.077 (60% off) | Similar to RI |
| **c5.xlarge** | $0.17 | ~$0.051 | $0.102 (40% off) | $0.068 (60% off) | Similar to RI |
| **r5.xlarge** | $0.252 | ~$0.076 | $0.151 (40% off) | $0.101 (60% off) | Similar to RI |
| **Lambda** | $0.20/1M requests + $0.0000166667/GB-sec | - | - | - | Compute Savings Plan |
| **Fargate vCPU** | $0.04048/vCPU-hour | - | - | - | Compute Savings Plan (up to 50% off) |
| **Fargate Memory** | $0.004445/GB-hour | - | - | - | Compute Savings Plan |

**Compute Cost Optimization:**
- Spot Instances for fault-tolerant workloads (up to 90% savings)
- Reserved Instances for steady-state workloads (up to 72% savings)
- Savings Plans for flexible commitment (up to 72% savings)
- Right-size instances using Compute Optimizer
- Use Auto Scaling to match capacity to demand
- Lambda for event-driven (no idle costs)
- Schedule start/stop for dev/test environments

### Data Transfer Cost Scenarios (per GB)

| Scenario | Cost | Notes |
|----------|------|-------|
| **Data IN to AWS (from internet)** | FREE | All inbound data transfer |
| **Data OUT from AWS to internet** | $0.09 (first 10 TB) | Decreases with volume |
| **Data OUT from AWS to internet** | $0.085 (next 40 TB) | 10-50 TB tier |
| **Data OUT from AWS to internet** | $0.07 (next 100 TB) | 50-150 TB tier |
| **Between S3 and EC2 (same region)** | FREE | No data transfer charges |
| **Between S3 and CloudFront** | FREE | CloudFront to S3 in same region |
| **CloudFront to internet** | $0.085 | Lower than direct from region |
| **Between regions** | $0.02 | Inter-region data transfer |
| **Cross-AZ within region** | $0.01 (each direction) | $0.02 round trip |
| **Between VPC peers (same region)** | $0.01 (each direction) | Same as cross-AZ |
| **Direct Connect (DX) data OUT** | $0.02-0.03 | Lower than internet |
| **Data OUT via NAT Gateway** | $0.045 + internet OUT costs | NAT Gateway processing fee |

**Data Transfer Cost Optimization:**
- Use CloudFront for content delivery (lower egress costs)
- Keep resources in same AZ when possible
- Use VPC endpoints for S3/DynamoDB (no NAT Gateway or internet charges)
- Compress data before transfer
- Use Direct Connect for large data transfers
- Leverage S3 Transfer Acceleration for uploads
- Use AWS DataSync for migrations (no additional transfer fee)

### Regional Cost Variations

**Note:** Prices vary by region. Examples above are for US East (N. Virginia).

| Region | Cost Multiplier | Notes |
|--------|----------------|-------|
| **US East (N. Virginia)** | 1.0x | Baseline, usually cheapest |
| **US West (Oregon)** | 1.0x | Similar to US East |
| **EU (Ireland)** | ~1.1x | Slightly higher |
| **Asia Pacific (Singapore)** | ~1.15x | Higher than US |
| **Asia Pacific (Sydney)** | ~1.2x | Higher data transfer costs |
| **South America (São Paulo)** | ~1.5x | Highest costs generally |

**Cost Optimization by Region:**
- Deploy in US East (N. Virginia) for lowest costs
- Choose region closest to users for performance
- Consider data sovereignty requirements
- Use S3 Intelligent-Tiering across regions

## When to Use What - Detailed Scenarios

### Compute Service Scenarios

**Lambda - Perfect For:**
- Image thumbnail generation when uploaded to S3
- Processing DynamoDB streams
- Scheduled tasks (every 5 minutes, hourly, daily)
- API backends with API Gateway
- Real-time file processing
- Chatbots and Alexa skills
- IoT backend processing
- ETL jobs under 15 minutes

**Lambda - NOT Good For:**
- Long-running processes (> 15 min)
- Applications with consistent traffic (better to use EC2/containers)
- Processes requiring local storage > 10 GB
- Applications needing GPUs
- Legacy applications with complex dependencies

**EC2 - Perfect For:**
- Custom applications requiring specific OS
- Applications needing GPUs
- Lift-and-shift migrations
- Consistent workloads (use Reserved Instances)
- Applications with licensing restrictions
- High-performance computing clusters
- Windows-based applications

**EC2 - NOT Good For:**
- Simple event-driven functions (use Lambda)
- Unpredictable traffic spikes (unless using Auto Scaling)
- Microservices that could be containerized
- When you want zero administration

**Fargate - Perfect For:**
- Microservices architectures
- Long-running containers
- Batch jobs and scheduled tasks
- CI/CD agents
- Applications with variable load
- When you don't want to manage servers

**Fargate - NOT Good For:**
- Consistent high workloads (EC2 with containers cheaper)
- GPU workloads (use ECS with EC2)
- Windows containers requiring host customization
- Extremely latency-sensitive apps (cold starts)

### Storage Service Scenarios

**S3 Standard - Perfect For:**
- Active website content and assets
- Frequently accessed data
- Content distribution with CloudFront
- Data lake hot tier
- Mobile and gaming applications
- Big data analytics (frequently queried)

**S3 Standard-IA - Perfect For:**
- Backups accessed occasionally
- Disaster recovery files
- Data retained for compliance
- Old data accessed few times per month
- Long-lived but infrequently accessed

**S3 Glacier - Perfect For:**
- Digital preservation
- Archive data rarely accessed
- Tape replacement
- Regulatory archives
- Long-term backups

**S3 Glacier - NOT Good For:**
- Frequently accessed data (expensive retrievals)
- Data needed immediately (retrieval time)
- Small files (minimum storage duration charges)

**EFS - Perfect For:**
- Web serving and content management
- Container persistent storage
- Shared data between instances
- Big data analytics needing shared access
- ML training data accessed by multiple instances
- Lift-and-shift applications expecting NFS

**EFS - NOT Good For:**
- Single-instance workloads (EBS cheaper)
- Windows applications (use FSx for Windows)
- Highest IOPS needs (use EBS io2)
- Object storage needs (use S3)

### Database Service Scenarios

**RDS - Perfect For:**
- Traditional relational databases
- Existing applications using MySQL/PostgreSQL/Oracle/SQL Server
- OLTP workloads
- When you need Multi-AZ for HA
- Read replicas for read-heavy workloads
- Lift-and-shift of existing databases

**RDS - NOT Good For:**
- Massive scale (consider DynamoDB or Aurora)
- Need root access (use EC2 with self-managed DB)
- Simple key-value access (use DynamoDB)
- Frequent schema changes (consider NoSQL)

**Aurora - Perfect For:**
- High-performance OLTP
- Applications needing MySQL/PostgreSQL compatibility
- Global applications (Global Database)
- Variable workloads (Serverless v2)
- Read-heavy workloads (15 read replicas)
- When you need best RDS performance

**DynamoDB - Perfect For:**
- Serverless applications
- Mobile backends
- Gaming leaderboards
- Session stores
- Shopping carts
- IoT data
- Millisecond latency required
- Massive scale (millions of requests/sec)

**DynamoDB - NOT Good For:**
- Complex queries with joins
- Ad-hoc queries and analytics (use RDS or Athena)
- ACID transactions across items (use RDS)
- When you need SQL
- Applications with unpredictable query patterns

**Redshift - Perfect For:**
- Business intelligence
- OLAP workloads
- Complex queries across large datasets
- Data warehouse consolidation
- Historical data analysis
- Petabyte-scale analytics

**Redshift - NOT Good For:**
- OLTP workloads (use RDS or DynamoDB)
- Real-time data ingestion (use Kinesis)
- Small datasets < 100 GB (use RDS or Athena)
- High-concurrency workloads (use RDS)

### Networking Service Scenarios

**CloudFront - Perfect For:**
- Global content delivery
- Website acceleration
- Video streaming
- API acceleration
- Software distribution
- DDoS protection with Shield

**CloudFront - NOT Good For:**
- Same-region traffic (no benefit)
- Dynamic content that can't be cached
- Backend-to-backend communication

**Direct Connect - Perfect For:**
- Large data migrations to AWS
- Consistent bandwidth requirements
- Hybrid architectures with high throughput
- Compliance requiring private connectivity
- Cost reduction for high data transfer volumes

**Direct Connect - NOT Good For:**
- Quick setup needs (use VPN)
- Low data transfer volumes
- Temporary connections
- Budget-constrained projects

**VPN - Perfect For:**
- Quick hybrid connectivity
- Backup for Direct Connect
- Low to moderate traffic
- Encrypted connectivity
- Branch office connections

**VPN - NOT Good For:**
- High bandwidth requirements
- Latency-sensitive applications
- Very high data transfer volumes

### Security Service Scenarios

**IAM - Perfect For:**
- Internal AWS access control
- Service-to-service permissions
- Cross-account access
- Temporary credentials with roles

**IAM - NOT Good For:**
- External user authentication (use Cognito)
- Thousands of users (use federation)

**Cognito - Perfect For:**
- Mobile app authentication
- Web app user sign-up/sign-in
- Social identity federation
- Guest user access
- User profile management

**Cognito - NOT Good For:**
- AWS resource access control (use IAM)
- Internal employee access (use SSO)

**Secrets Manager - Perfect For:**
- Database credentials
- API keys
- Automatic rotation of secrets
- Cross-region secret replication
- Applications using AWS SDKs

**Secrets Manager - NOT Good For:**
- Configuration data (use Parameter Store)
- Public data (use Parameter Store)
- When you don't need rotation (Parameter Store cheaper)

## Service Anti-Patterns (When NOT to Use)

### Common Mistakes and Anti-Patterns

**Lambda Anti-Patterns:**
- Using Lambda for long-running processes (use ECS/Fargate)
- Storing state in /tmp (use S3 or DynamoDB)
- Synchronous invocation chains (use Step Functions)
- Not using connection pooling for databases
- Ignoring cold start optimization

**S3 Anti-Patterns:**
- Using S3 as a database (use DynamoDB or RDS)
- Frequent small file updates (use EFS or database)
- Sequential file naming causing hot partitions
- Not using lifecycle policies for old data
- Public buckets without careful review

**EC2 Anti-Patterns:**
- Not using Auto Scaling for variable workloads
- On-Demand instances for predictable workloads (use RIs)
- Over-provisioning instance sizes
- Not using EBS snapshots for backups
- Forgetting to terminate unused instances

**RDS Anti-Patterns:**
- Not using Multi-AZ for production
- Single large instance instead of read replicas
- Not monitoring storage and IOPS
- Storing files/blobs in database (use S3)
- Not using parameter groups for tuning

**DynamoDB Anti-Patterns:**
- Using it as a relational database with complex joins
- Hot partition keys
- Storing large items > 400 KB
- Not using DynamoDB Streams for replication
- Provisioned capacity with unpredictable traffic

**VPC Anti-Patterns:**
- Overlapping CIDR blocks preventing peering
- Too small CIDR ranges
- Not using multiple AZs for HA
- Public subnets for databases
- Not using VPC endpoints for S3/DynamoDB

**ECS/EKS Anti-Patterns:**
- Using Fargate for consistent high-volume (EC2 cheaper)
- Not using Application Load Balancer for HTTP services
- Ignoring container health checks
- Large monolithic containers
- Not using ECR for private images

## Common Misconceptions

### Clearing Up Confusion

**S3 Misconceptions:**
- "S3 is just for backups" - FALSE: It's for any object storage, including active applications
- "S3 is slow" - FALSE: Can handle thousands of requests per second per prefix
- "You need to manage S3 servers" - FALSE: Fully serverless and managed

**Lambda Misconceptions:**
- "Lambda is always cheaper" - FALSE: Consistent traffic makes EC2/Fargate cheaper
- "Lambda scales infinitely" - FALSE: Limited by concurrent execution limit (can be increased)
- "Lambda is only for simple functions" - FALSE: Can run complex applications

**VPC Misconceptions:**
- "All traffic between AZs is free" - FALSE: $0.01/GB each direction
- "Private subnet means secure" - FALSE: Still need Security Groups and NACLs
- "VPC peering is transitive" - FALSE: Must create direct peering between each VPC pair

**RDS Misconceptions:**
- "Multi-AZ means read scaling" - FALSE: It's only for HA, use read replicas for scaling
- "Automated backups are free" - FALSE: Charged for backup storage beyond DB size
- "All databases should use RDS" - FALSE: NoSQL workloads better on DynamoDB

**IAM Misconceptions:**
- "Root user should be used daily" - FALSE: Create IAM users and lock down root
- "IAM is free so roles don't matter" - FALSE: Proper roles crucial for security
- "Users need passwords" - FALSE: Service accounts should use roles, not users

## Service Limits Extended

### Soft Limits vs Hard Limits

**Soft Limits** (Can be increased by request):
- EC2 instance limits
- VPC limits
- RDS instance limits
- S3 bucket limit
- Lambda concurrent executions
- CloudFormation stacks
- Elastic IP addresses

**Hard Limits** (Cannot be increased):
- S3 object size (5 TB)
- Lambda execution timeout (15 minutes)
- Lambda /tmp storage (10 GB)
- Lambda deployment package size (250 MB)
- S3 bucket name length (63 characters)
- IAM policy size (6,144 characters for managed policies)

### Extended Service Limits Table

| Service | Limit Type | Default | Max After Increase | How to Request |
|---------|-----------|---------|-------------------|----------------|
| **EC2 On-Demand Instances** | Soft | 20 vCPUs | Unlimited | Service Quotas console |
| **EC2 Spot Instances** | Soft | 20 vCPUs | Unlimited | Service Quotas console |
| **VPCs per Region** | Soft | 5 | 100+ | Service Quotas console |
| **Subnets per VPC** | Soft | 200 | 200+ | Service Quotas console |
| **Security Groups per Region** | Soft | 2,500 | 10,000 | Service Quotas console |
| **Rules per Security Group** | Soft | 60 | 250 | Service Quotas console |
| **Route Tables per VPC** | Soft | 200 | 200+ | Service Quotas console |
| **S3 Buckets** | Soft | 100 | 1,000+ | Support case |
| **S3 Object Size** | Hard | 5 TB | Cannot increase | N/A |
| **S3 PUT/COPY/POST/DELETE** | Soft | 3,500 req/sec | 5,500+ req/sec | Automatic |
| **S3 GET/HEAD** | Soft | 5,500 req/sec | Unlimited | Automatic |
| **RDS DB Instances** | Soft | 40 | 100+ | Service Quotas console |
| **RDS Read Replicas per Master** | Hard | 15 (Aurora), 5 (others) | Cannot increase | N/A |
| **RDS DB Snapshots** | Soft | 100 | 500+ | Service Quotas console |
| **DynamoDB Table Count** | Soft | 2,500 | 10,000+ | Service Quotas console |
| **DynamoDB Throughput** | Soft | 40,000 RCU, 40,000 WCU | Unlimited | Automatic with on-demand |
| **Lambda Concurrent Executions** | Soft | 1,000 | 100,000+ | Service Quotas console |
| **Lambda Function Timeout** | Hard | 15 minutes | Cannot increase | N/A |
| **Lambda /tmp Space** | Hard | 10 GB | Cannot increase | N/A |
| **Lambda Deployment Package** | Hard | 250 MB (unzipped) | Cannot increase | N/A |
| **Lambda Environment Variables** | Hard | 4 KB | Cannot increase | N/A |
| **Lambda Layers** | Hard | 5 per function | Cannot increase | N/A |
| **EBS Volumes per Instance** | Soft | 128 | 128 | Cannot increase |
| **EBS Volume Size (gp3/io2)** | Hard | 64 TB | Cannot increase | N/A |
| **EBS Snapshots** | Soft | 10,000 | 100,000+ | Service Quotas console |
| **CloudFormation Stacks** | Soft | 2,000 | 10,000+ | Service Quotas console |
| **CloudFormation Stack Sets** | Soft | 100 | 500+ | Service Quotas console |
| **CloudWatch Alarms** | Soft | 5,000 | 15,000+ | Service Quotas console |
| **CloudWatch Log Groups** | Soft | Unlimited | Unlimited | N/A |
| **CloudTrail Trails** | Soft | 5 | 5 | Cannot increase |
| **IAM Users** | Soft | 5,000 | 5,000 | Cannot increase |
| **IAM Groups** | Soft | 300 | 500 | Support case |
| **IAM Roles** | Soft | 1,000 | 5,000+ | Service Quotas console |
| **IAM Policies per User/Group/Role** | Hard | 10 managed, 1 inline | Cannot increase | N/A |
| **ELB (ALB/NLB) per Region** | Soft | 50 | 100+ | Service Quotas console |
| **Target Groups per Region** | Soft | 3,000 | 5,000+ | Service Quotas console |
| **Targets per Target Group** | Soft | 1,000 | 1,000+ | Service Quotas console |
| **VPN Connections per Region** | Soft | 50 | 200+ | Service Quotas console |
| **Direct Connect Connections** | Soft | 10 | 50+ | Support case |
| **Route 53 Hosted Zones** | Soft | 500 | 5,000+ | Service Quotas console |
| **Route 53 Records per Hosted Zone** | Soft | 10,000 | 100,000+ | Service Quotas console |

### How to Request Service Limit Increases

**Method 1: Service Quotas Console** (Preferred)
1. Open Service Quotas console
2. Choose AWS service
3. Select quota to increase
4. Request quota increase
5. Provide use case justification

**Method 2: Support Case**
- For limits not in Service Quotas console
- Create support case under "Service Limit Increase"
- Provide detailed justification
- Include projected usage

**Method 3: Automatic Increases**
- Some services auto-increase based on usage (S3, DynamoDB On-Demand)
- No action required

**Best Practices:**
- Request increases proactively before hitting limits
- Monitor usage with CloudWatch and Service Quotas
- Set up CloudWatch alarms for approaching limits
- Design applications to handle throttling gracefully
- Use multiple accounts to multiply limits (via AWS Organizations)

### Limits That Affect Architecture

**Design Considerations:**

**EC2 Instance Limits:**
- Plan Auto Scaling max size within limits
- Use multiple instance types for diversity
- Consider Reserved Instances against limits

**Lambda Concurrency:**
- Reserve concurrency for critical functions
- Use SQS for buffering
- Design for throttling (exponential backoff)

**DynamoDB Throughput:**
- Use On-Demand for unpredictable traffic
- Provision with auto-scaling for predictable
- Partition data to avoid hot keys

**S3 Request Rates:**
- Use random prefixes for high request rates
- CloudFront for read-heavy workloads
- Transfer Acceleration for uploads

**VPC Limits:**
- Plan IP addressing carefully (CIDR sizing)
- Use Transit Gateway for many VPCs
- Monitor ENI usage (Lambda in VPC consumes ENIs)

**IAM Limits:**
- Use roles instead of users where possible
- Group permissions efficiently
- Consider AWS SSO for human users
- Federation for external identities

---

[← Previous: Hands-On Labs](07-hands-on-labs.md) | [Back to Main](README.md) | [Next: Exam Scenarios →](09-exam-scenarios.md)
