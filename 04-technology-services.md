# Domain 3: Cloud Technology and Services (34%)

[← Previous: Security and Compliance](03-security-compliance.md) | [Next: Billing, Pricing, and Support →](05-billing-support.md)

---

This is the largest domain of the AWS Certified Cloud Practitioner exam, covering core AWS services across compute, storage, networking, databases, and more.

## Table of Contents

- [AWS Global Infrastructure](#aws-global-infrastructure)
- [Compute Services](#compute-services)
- [Storage Services](#storage-services)
- [Database Services](#database-services)
- [Networking and Content Delivery](#networking-and-content-delivery)
- [Management and Governance](#management-and-governance)
- [Additional Services](#additional-services)
- [Review Questions](#review-questions)

---

## AWS Global Infrastructure

### Regions

**Geographic areas with multiple Availability Zones**

- **Current Count**: 33 Regions worldwide (and growing)
- **Isolation**: Each Region is completely isolated from other Regions
- **Selection Criteria**:
  - **Compliance requirements**: Data sovereignty and regulatory requirements
  - **Proximity to users**: Lower latency for end users
  - **Available services**: Not all services are available in all Regions
  - **Pricing**: Costs vary by Region

> **Exam Tip**: Choose the right Region based on compliance, latency, service availability, and cost considerations.

### Availability Zones (AZs)

**Discrete data centers within a Region**

- **Composition**: One or more discrete data centers per AZ
- **Redundancy**: Each AZ has redundant power, networking, and connectivity
- **Physical Separation**: AZs are physically separated within a Region
- **Connectivity**: Connected with high-bandwidth, low-latency networking
- **Minimum Count**: At least 3 AZs per Region (most have more)
- **High Availability**: Deploy resources across multiple AZs for fault tolerance

**Key Benefits**:
- Fault isolation
- High availability through redundancy
- Disaster recovery within a Region

**Real-World Example**: Netflix deploys its content delivery infrastructure across multiple AZs within each Region. If one AZ experiences issues, their application automatically routes traffic to healthy AZs, ensuring uninterrupted streaming for millions of users.

**Common Configuration Mistakes**:
1. Deploying all resources in a single AZ (no fault tolerance)
2. Not considering cross-AZ data transfer costs
3. Assuming AZ names (us-east-1a) are the same across AWS accounts (they're randomized)
4. Not testing failover between AZs before production deployment

**Service Limits by Infrastructure**:
- Maximum VPCs per Region: 5 (soft limit, can be increased)
- Maximum subnets per VPC: 200
- Elastic IPs per Region: 5 (soft limit)
- VPC Peering connections per VPC: 125

### Edge Locations

**Content delivery endpoints worldwide**

- **Count**: 400+ Edge Locations globally
- **Primary Use**: CloudFront content caching
- **Performance**: Lower latency for end users
- **Services**: Also used by Route 53, AWS Shield, and AWS WAF
- **Coverage**: More Edge Locations than Regions

### AWS Local Zones

**Region extensions for ultra-low latency**

- Extension of a Region placed closer to specific geographic areas
- Single-digit millisecond latency to end users
- Ideal for latency-sensitive applications (gaming, live video, AR/VR)
- Not available in all locations

### AWS Wavelength

**5G edge computing**

- Embeds AWS compute and storage services within 5G networks
- Ultra-low latency applications
- Mobile edge computing use cases
- Reduces data routing to application servers

### AWS Outposts

**On-premises AWS infrastructure**

- Fully managed service extending AWS infrastructure to on-premises facilities
- Same AWS APIs, tools, and hardware available on-premises
- Enables true hybrid cloud deployments
- AWS manages and maintains the infrastructure
- Run AWS services locally with consistent experience

> **Key Point**: Remember the hierarchy: **Regions** contain **Availability Zones**. **Edge Locations** are separate and used primarily for content delivery.

### Global Infrastructure Comparison Table

| Component | Count | Primary Use | Redundancy Level | Latency |
|-----------|-------|-------------|------------------|---------|
| **Regions** | 33+ | Full AWS service deployment | Isolated from each other | Variable (based on distance) |
| **Availability Zones** | 100+ (3+ per Region) | Fault-tolerant deployments | Within Region | Low (single-digit ms) |
| **Edge Locations** | 400+ | Content caching | N/A | Minimal to end users |
| **Local Zones** | 16+ | Ultra-low latency compute | Extension of parent Region | <10ms |
| **Wavelength Zones** | 20+ | 5G edge computing | Within telecom networks | <1ms |

### Infrastructure Selection Flowchart

```
START: Where should I deploy my resources?
│
├─> Need global distribution?
│   ├─> YES: Deploy in multiple Regions
│   │   └─> Consider: CloudFront for content, Route 53 for DNS routing
│   │
│   └─> NO: Single Region deployment
│       │
│       ├─> Need high availability?
│       │   ├─> YES: Deploy across multiple AZs
│       │   │   └─> Use: Multi-AZ RDS, ALB across AZs, Auto Scaling
│       │   │
│       │   └─> NO: Single AZ (only for dev/test)
│       │
│       └─> Need ultra-low latency?
│           ├─> For specific metro areas: Use Local Zones
│           ├─> For mobile 5G apps: Use Wavelength
│           └─> For on-premises: Use Outposts
```

### Migration Scenario: Data Center to AWS

**Scenario**: Company with on-premises data center in Chicago needs to migrate to AWS.

**Current Setup**:
- 100 servers across 2 data centers
- Customers primarily in US and Europe
- Compliance requires data residency in US

**AWS Migration Strategy**:

1. **Region Selection**: Choose us-east-1 (N. Virginia) for primary and us-west-2 (Oregon) for DR
   - Reason: Established Regions with all services available
   - Cost: Lower pricing than newer Regions
   - Latency: Good for US customers

2. **Network Connectivity**:
   - Phase 1: Site-to-Site VPN (immediate, low cost)
   - Phase 2: Direct Connect (after 3 months, for production traffic)
   - Cost: VPN ~$0.05/hour, Direct Connect ~$0.30/hour (1 Gbps)

3. **High Availability Architecture**:
   - Deploy across 3 AZs in us-east-1
   - Application Load Balancer across all AZs
   - RDS Multi-AZ for databases
   - S3 for object storage (automatically multi-AZ)

4. **European Customers**:
   - CloudFront distribution with origin in us-east-1
   - Edge locations in Europe cache content
   - Route 53 latency-based routing

**Cost Comparison**:
- On-premises: $50,000/month (hardware, power, cooling, staff)
- AWS initial: $35,000/month (no hardware investment)
- AWS optimized (after 6 months with RIs): $22,000/month
- Savings: 56% reduction in monthly costs

---

## Compute Services

### Amazon EC2 (Elastic Compute Cloud)

**Virtual servers in the cloud**

Amazon EC2 provides resizable compute capacity in the cloud, allowing you to launch virtual servers (instances) on demand.

#### Instance Types

| Category | Use Case | Examples |
|----------|----------|----------|
| **General Purpose** | Balanced compute, memory, networking | T3, M5 |
| **Compute Optimized** | High-performance processors | C5, C6 |
| **Memory Optimized** | Large datasets in memory | R5, X1 |
| **Storage Optimized** | High sequential read/write access | I3, D2 |
| **Accelerated Computing** | GPU, FPGA workloads | P3, G4 |

**Detailed Instance Type Comparison**

| Instance Family | vCPU Range | Memory Range | Network Performance | Best Use Cases | Example Workloads |
|----------------|------------|--------------|---------------------|----------------|-------------------|
| **T3/T4g** | 2-8 | 0.5-32 GB | Up to 5 Gbps | Burstable, web servers | Small websites, dev environments |
| **M6i/M7g** | 2-128 | 8-512 GB | Up to 50 Gbps | Balanced applications | App servers, backend systems |
| **C6i/C7g** | 2-128 | 4-256 GB | Up to 50 Gbps | Compute-intensive | Video encoding, gaming servers, HPC |
| **R6i/R7g** | 2-128 | 16-1024 GB | Up to 50 Gbps | Memory-intensive | In-memory databases, big data analytics |
| **X2iedn** | 16-128 | 256-4096 GB | Up to 100 Gbps | Extreme memory | SAP HANA, real-time analytics |
| **I4i** | 2-128 | 16-1024 GB | Up to 75 Gbps | Storage-intensive | NoSQL databases, data warehousing |
| **P4d** | 96 | 1152 GB | 400 Gbps | ML training | Deep learning, GPU clusters |
| **G5** | 4-96 | 16-768 GB | Up to 100 Gbps | Graphics-intensive | ML inference, graphics rendering |

**Real-World Use Case: E-Commerce Website**

**Scenario**: Online retailer with variable traffic patterns, peak during holidays.

**Solution Architecture**:
- **Frontend Web Servers**: T3.medium instances (burstable for normal traffic)
  - Cost: $0.0416/hour = ~$30/month each
  - Auto Scaling: 2-20 instances based on demand
  - Normal: 4 instances = $120/month
  - Peak: 15 instances = $450/month

- **Application Servers**: M5.large instances (consistent performance)
  - Cost: $0.096/hour = ~$70/month each
  - Reserved Instances (3-year): 75% discount = $17.50/month each
  - Deploy: 8 instances = $140/month with RIs

- **Database**: R5.xlarge (memory-optimized for database)
  - Cost: $0.252/hour = ~$184/month
  - Reserved Instance: $46/month

**Total Monthly Cost**:
- Normal traffic: $306/month
- Peak traffic: $636/month
- Average: ~$400/month vs $2,000+ on-premises

**Performance Metrics**:
- Page load time: <2 seconds
- Database query response: <100ms
- Handles 10,000 concurrent users during peak
- 99.99% uptime with Multi-AZ deployment

#### EC2 Pricing Models

##### 1. On-Demand Instances

- **Billing**: Pay per hour or per second (minimum 60 seconds)
- **Commitment**: No upfront commitment or long-term contract
- **Cost**: Highest per-hour cost
- **Best For**:
  - Unpredictable workloads
  - Short-term, spiky workloads
  - Testing and development
  - Applications with flexible start/stop times

##### 2. Reserved Instances (RI)

- **Commitment**: 1 or 3 year terms
- **Discount**: Up to 75% compared to On-Demand pricing
- **Types**:
  - **Standard RI**: Maximum discount, cannot change instance type
  - **Convertible RI**: Lower discount, can change instance type/family
- **Payment Options**: All upfront, partial upfront, or no upfront
- **Best For**: Steady-state workloads with predictable usage

##### 3. Savings Plans

- **Commitment**: Consistent compute usage measured in $/hour
- **Term**: 1 or 3 year commitment
- **Discount**: Up to 72% compared to On-Demand
- **Flexibility**: More flexible than Reserved Instances
- **Coverage**: Applies to EC2, Lambda, and Fargate
- **Best For**: Flexible compute usage with commitment to consistent spend

##### 4. Spot Instances

- **Mechanism**: Bid on unused EC2 capacity
- **Discount**: Up to 90% compared to On-Demand pricing
- **Interruption**: Can be terminated by AWS with 2-minute warning
- **Best For**:
  - Fault-tolerant applications
  - Flexible workloads
  - Batch processing
  - Big data analysis
- **Not Suitable For**: Critical workloads or databases

##### 5. Dedicated Hosts

- **Definition**: Physical EC2 server dedicated exclusively to your use
- **Cost**: Most expensive option
- **Use Cases**:
  - Regulatory compliance requirements
  - Server-bound software licenses
  - Socket/core visibility needed for licensing
- **Control**: Full control over instance placement

##### 6. Dedicated Instances

- **Definition**: Instances run on hardware dedicated to a single customer
- **Sharing**: May share hardware with other instances in your account
- **Cost**: Less expensive than Dedicated Hosts
- **Isolation**: Hardware-level isolation from other AWS accounts

**EC2 Pricing Model Comparison Table**

| Pricing Model | Discount vs On-Demand | Commitment | Interruption Risk | Best For | Payment Options |
|---------------|----------------------|------------|-------------------|----------|-----------------|
| **On-Demand** | 0% (baseline) | None | None | Short-term, unpredictable | Per hour/second |
| **Reserved (Standard)** | Up to 75% | 1 or 3 years | None | Steady-state workloads | All/Partial/No upfront |
| **Reserved (Convertible)** | Up to 66% | 1 or 3 years | None | Flexible steady workloads | All/Partial/No upfront |
| **Savings Plans** | Up to 72% | 1 or 3 years | None | Flexible compute usage | All/Partial/No upfront |
| **Spot Instances** | Up to 90% | None | Can be interrupted | Fault-tolerant, flexible | Per hour |
| **Dedicated Hosts** | Varies | 1 or 3 years | None | Licensing, compliance | On-Demand or Reserved |

**Cost Comparison Example: m5.xlarge in us-east-1**

| Scenario | Pricing Model | Hourly Cost | Monthly Cost (730 hrs) | Annual Cost | 3-Year Total |
|----------|---------------|-------------|------------------------|-------------|--------------|
| **Dev/Test (8hrs/day, 22 days/month)** | On-Demand | $0.192 | $337 | $4,147 | $12,441 |
| **Production (24/7)** | On-Demand | $0.192 | $140 | $1,681 | $5,043 |
| **Production (24/7)** | Standard RI (3yr, all upfront) | $0.112 | $82 | $981 | $2,943 |
| **Production (24/7)** | Savings Plan (3yr) | $0.118 | $86 | $1,032 | $3,096 |
| **Batch Processing (avg 50% uptime)** | Spot | $0.038 | $14 | $168 | $504 |

**Cost Optimization Strategy**:
1. **Baseline workload**: Use Standard RIs or Savings Plans (75% savings)
2. **Variable workload**: Use Savings Plans for flexibility
3. **Peak capacity**: Auto Scale with On-Demand
4. **Batch/fault-tolerant**: Use Spot Instances (90% savings)

**Common Configuration Mistakes**:
1. Running On-Demand for steady-state workloads (missing 75% savings)
2. Using Spot Instances for databases or critical workloads
3. Over-provisioning instance size (wasting resources)
4. Not enabling detailed monitoring for performance optimization
5. Forgetting to delete stopped instances (still incurs EBS charges)
6. Not using Auto Scaling (manually managing capacity)
7. Storing data on instance store for persistent data (data loss on stop)
8. Not tagging instances (difficult cost allocation)

**Service Limits and Quotas**:
- On-Demand instance limit: Varies by instance family (typically 20-1280 vCPUs)
- Spot instance limit: 20 Spot instances per Region (soft limit)
- Reserved Instances: 20 per month (soft limit)
- Elastic IPs: 5 per Region (soft limit)
- EBS volumes: 5,000 per Region (soft limit)
- EBS snapshots: 10,000 per Region (soft limit)

**Monitoring and Troubleshooting**:

1. **CloudWatch Metrics** (5-minute intervals, free):
   - CPUUtilization
   - NetworkIn/NetworkOut
   - DiskReadOps/DiskWriteOps
   - StatusCheckFailed

2. **Detailed Monitoring** (1-minute intervals, paid):
   - Enable for production workloads
   - Cost: $0.14 per instance per month
   - Better for Auto Scaling responsiveness

3. **Common Issues**:
   - High CPU: Resize instance or optimize application
   - High memory: Use memory-optimized instance type
   - Network bottleneck: Use enhanced networking or larger instance
   - Disk I/O bottleneck: Use Provisioned IOPS EBS volumes

4. **Troubleshooting Commands**:
   ```
   # Check system status
   aws ec2 describe-instance-status --instance-ids i-1234567890abcdef0

   # View CloudWatch metrics
   aws cloudwatch get-metric-statistics --namespace AWS/EC2 \
     --metric-name CPUUtilization --dimensions Name=InstanceId,Value=i-xxx

   # Check security group rules
   aws ec2 describe-security-groups --group-ids sg-xxx
   ```

#### Auto Scaling

**Automatically adjust capacity based on demand**

- **Scaling Actions**:
  - **Scale Out**: Add instances when demand increases
  - **Scale In**: Remove instances when demand decreases

- **Scaling Policies**:
  - **Target Tracking**: Maintain a specific metric (e.g., 50% CPU utilization)
  - **Step Scaling**: Scale based on CloudWatch alarm thresholds
  - **Scheduled Scaling**: Scale based on predictable time-based patterns

- **Benefits**:
  - Improved availability
  - Cost optimization
  - Fault tolerance
  - Works seamlessly with Elastic Load Balancing

### AWS Lambda

**Serverless compute service**

Run code without provisioning or managing servers.

**Key Features**:
- **Zero Server Management**: No infrastructure to provision or manage
- **Automatic Scaling**: Scales automatically from a few requests to thousands
- **Subsecond Metering**: Pay only for compute time consumed (billed per 100ms)
- **Language Support**: Python, Node.js, Java, Go, C#, Ruby, PowerShell
- **Execution Limit**: Maximum 15 minutes per execution
- **Event-Driven**: Triggered by events from AWS services or custom applications

**Benefits**:
- No server management overhead
- Continuous automatic scaling
- Cost-effective for variable workloads
- Built-in high availability and fault tolerance

**Common Use Cases**:
- Real-time file processing
- Data transformation and ETL
- Serverless backends for web/mobile apps
- IoT backends
- Scheduled tasks (cron jobs)

**Lambda vs EC2 Comparison**

| Aspect | AWS Lambda | Amazon EC2 |
|--------|------------|------------|
| **Management** | Fully managed, zero administration | You manage OS, patches, scaling |
| **Scaling** | Automatic, instant (0-10,000+ concurrent) | Manual or Auto Scaling (minutes) |
| **Pricing** | Per request + duration (100ms increments) | Per hour/second of instance runtime |
| **Max Duration** | 15 minutes per invocation | Unlimited (runs continuously) |
| **Cold Start** | Yes (50-200ms initial delay) | No (always running) |
| **State** | Stateless (ephemeral storage) | Stateful (persistent storage) |
| **Best For** | Event-driven, sporadic workloads | Long-running, steady workloads |

**Real-World Use Case: Image Processing Service**

**Scenario**: Photography platform processes user-uploaded images (resize, thumbnail, watermark).

**Lambda Solution**:
```
Architecture:
User uploads image to S3
  └─> S3 triggers Lambda function
      └─> Lambda processes image
          └─> Saves processed images back to S3
          └─> Updates DynamoDB with metadata
```

**Cost Analysis** (1 million images per month):
- Average processing time: 3 seconds per image
- Memory allocation: 1024 MB
- Compute: 1M requests × 3 seconds × $0.0000166667/GB-second = $50
- Requests: 1M × $0.20 per 1M = $0.20
- **Total: $50.20/month**

**EC2 Comparison**:
- t3.medium running 24/7: ~$30/month (On-Demand)
- BUT: Needs management, updates, monitoring
- AND: Wastes capacity during low-traffic periods
- **Total with overhead: $100-150/month**

**Lambda Advantages for this use case**:
- 66% cost savings
- Zero server management
- Automatic scaling (handles traffic spikes)
- Only pay for actual processing time

**Cost Comparison Example: Lambda Pricing**

| Monthly Requests | Avg Duration | Memory | Compute Cost | Request Cost | Total Cost | Equivalent EC2 |
|-----------------|--------------|---------|--------------|--------------|------------|----------------|
| **100,000** | 200ms | 512 MB | $0.17 | $0.02 | $0.19 | t3.micro ($7.59) |
| **1,000,000** | 1 second | 1024 MB | $16.67 | $0.20 | $16.87 | t3.small ($15.18) |
| **10,000,000** | 500ms | 512 MB | $41.67 | $2.00 | $43.67 | t3.medium ($30.37) |
| **100,000,000** | 200ms | 256 MB | $33.33 | $20.00 | $53.33 | m5.large ($70.08) |

**Integration Example: Serverless Web Application**

```
Architecture:
CloudFront (CDN)
  └─> S3 (Static website hosting)
      └─> API Gateway (REST API)
          └─> Lambda (Business logic)
              ├─> DynamoDB (User data)
              ├─> RDS Aurora Serverless (Transactional data)
              └─> S3 (File storage)
```

**Benefits**:
- No servers to manage
- Automatic scaling from 0 to millions of requests
- Pay only for actual usage
- High availability built-in

**Common Configuration Mistakes**:
1. Not setting appropriate timeout (default 3s, max 15min)
2. Allocating too much or too little memory (affects CPU allocation)
3. Not handling cold starts for latency-sensitive applications
4. Storing state in /tmp (lost between invocations, max 10 GB)
5. Not implementing proper error handling and retries
6. Exceeding concurrent execution limit (default 1,000)
7. Not using Lambda Layers for shared dependencies
8. Embedding sensitive data in code (use environment variables/Secrets Manager)

**Service Limits and Quotas**:
- Concurrent executions: 1,000 (soft limit, can be increased)
- Function timeout: 15 minutes (hard limit)
- Deployment package size: 50 MB (zipped), 250 MB (unzipped)
- /tmp directory storage: 10 GB
- Environment variables: 4 KB total
- Memory allocation: 128 MB to 10,240 MB (64 MB increments)
- Ephemeral storage: 512 MB to 10,240 MB

**Monitoring and Troubleshooting**:

1. **CloudWatch Metrics** (automatic):
   - Invocations
   - Duration
   - Errors
   - Throttles
   - Concurrent Executions

2. **CloudWatch Logs**:
   - All console.log/print statements
   - Execution start/end
   - Error traces
   - Custom metrics

3. **AWS X-Ray** (distributed tracing):
   - Trace requests through entire application
   - Identify performance bottlenecks
   - Visualize service map

4. **Common Issues**:
   - Cold starts: Use provisioned concurrency (extra cost)
   - Timeout errors: Increase timeout or optimize code
   - Out of memory: Increase memory allocation
   - Throttling: Request concurrent execution limit increase

**Performance Optimization Tips**:
1. Minimize deployment package size
2. Use Lambda Layers for shared dependencies
3. Keep functions warm with scheduled invocations (if needed)
4. Optimize memory allocation (more memory = more CPU)
5. Use environment variables for configuration
6. Connection pooling for database connections
7. Lazy load dependencies outside handler function

### Compute Service Selection Flowchart

```
START: Which compute service should I use?
│
├─> Need full control over OS and applications?
│   └─> YES: Amazon EC2
│       ├─> Simple setup needed? → Lightsail
│       ├─> Application deployment focus? → Elastic Beanstalk
│       └─> Full control? → EC2
│
├─> Using containers?
│   └─> YES:
│       ├─> Already use Kubernetes? → EKS
│       ├─> Want AWS-native? → ECS
│       ├─> Want serverless containers? → Fargate
│       └─> Batch processing? → AWS Batch
│
└─> Event-driven, short-running tasks?
    └─> YES: AWS Lambda
        ├─> Workflows needed? → Step Functions + Lambda
        ├─> APIs? → API Gateway + Lambda
        └─> Event processing? → EventBridge + Lambda
```

**Migration Scenario: Monolith to Serverless**

**Current State**: Monolithic PHP application on 3 EC2 instances
- Monthly cost: $150 (EC2) + $50 (RDS) = $200
- Maintenance: 10 hours/month
- Scaling: Manual, 30-minute deployment

**Target State**: Serverless architecture
- Static content: S3 + CloudFront
- APIs: API Gateway + Lambda
- Database: Aurora Serverless
- Authentication: Cognito

**Migration Steps**:
1. Extract static assets to S3 (Week 1)
2. Create CloudFront distribution (Week 1)
3. Migrate APIs to Lambda (Weeks 2-4)
4. Migrate database to Aurora Serverless (Week 5)
5. Implement Cognito authentication (Week 6)
6. Decommission EC2 instances (Week 7)

**After Migration**:
- Monthly cost: $60 (80% traffic reduction)
- Maintenance: 2 hours/month (75% reduction)
- Scaling: Automatic, instant
- Deployment: Minutes (CI/CD pipeline)
- Performance: 40% faster (CloudFront CDN)

### Amazon Lightsail

**Simplified cloud platform for simple workloads**

- Easy-to-use virtual private servers (VPS)
- Predictable monthly pricing
- Bundled resources: compute, storage, networking, DNS
- Pre-configured application stacks (WordPress, Magento, LAMP, etc.)
- Ideal for:
  - Simple web applications
  - Blogs and websites
  - Small business applications
  - Development and test environments
- Perfect for users new to AWS or with simple requirements

### AWS Elastic Beanstalk

**Platform as a Service (PaaS)**

Deploy and manage applications without infrastructure complexity.

**Key Features**:
- **Language Support**: Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker
- **Automatic Management**: Capacity provisioning, load balancing, auto-scaling, health monitoring
- **Developer Control**: Retain full control over underlying AWS resources
- **No Additional Charge**: Pay only for the AWS resources used
- **Quick Deployment**: Deploy applications in minutes

**Best For**:
- Web applications
- Developers who want to focus on code, not infrastructure
- Standard application architectures

### Amazon ECS (Elastic Container Service)

**Fully managed container orchestration**

Run and scale Docker containers on AWS.

**Launch Types**:
- **EC2 Launch Type**:
  - You manage the underlying EC2 instances
  - More control over infrastructure
  - Good for cost optimization with Reserved Instances

- **Fargate Launch Type**:
  - Serverless container deployment
  - AWS manages the infrastructure
  - Pay only for container resources

**Features**:
- Deep AWS integration
- Service discovery
- Load balancing
- Auto Scaling

**Use Cases**:
- Microservices architectures
- Batch processing
- Machine learning applications

### Amazon EKS (Elastic Kubernetes Service)

**Managed Kubernetes service**

- Fully managed Kubernetes control plane
- Compatible with standard Kubernetes tooling and plugins
- Automatic Kubernetes version upgrades and patching
- Integrates with AWS services (IAM, VPC, CloudWatch)
- Multi-AZ control plane for high availability
- Best for teams already invested in Kubernetes ecosystem

### AWS Fargate

**Serverless compute engine for containers**

- Works with both ECS and EKS
- No need to provision, configure, or scale EC2 instances
- Pay only for the vCPU and memory resources your containers use
- Automatic scaling and load balancing
- Focus on building applications, not managing infrastructure

> **Key Point**: Compute spectrum from most to least control:
> **EC2** (full control) → **Containers (ECS/EKS)** (moderate control) → **Lambda/Fargate** (least control, most abstraction)

---

## Storage Services

### Amazon S3 (Simple Storage Service)

**Object storage service for any amount of data**

S3 is a highly durable, scalable object storage service designed to store and retrieve any amount of data from anywhere.

#### Key Concepts

| Concept | Description |
|---------|-------------|
| **Buckets** | Containers for objects with globally unique names |
| **Objects** | Files stored in buckets (up to 5 TB each) |
| **Keys** | Unique identifier for each object in a bucket |
| **Durability** | 99.999999999% (11 nines) |
| **Availability** | Varies by storage class |

#### S3 Storage Classes

##### 1. S3 Standard

- **Use Case**: Frequently accessed data
- **Performance**: Low latency and high throughput
- **Availability**: 99.99%
- **Cost**: Most expensive storage cost
- **Best For**: Active databases, frequently accessed content, dynamic websites

##### 2. S3 Intelligent-Tiering

- **Automation**: Automatically moves objects between access tiers
- **Optimization**: Cost optimization for unknown or changing access patterns
- **Tiers**: Frequent Access, Infrequent Access, Archive Instant Access, Archive Access, Deep Archive Access
- **Monitoring Fee**: Small monthly fee per object
- **No Retrieval Fees**: Between Frequent and Infrequent Access tiers

##### 3. S3 Standard-IA (Infrequent Access)

- **Use Case**: Infrequently accessed data that needs rapid access when required
- **Cost**: Lower storage cost, but retrieval fee applies
- **Availability**: 99.9%
- **Minimum Duration**: 30-day minimum storage charge
- **Best For**: Backups, disaster recovery, long-term storage

##### 4. S3 One Zone-IA

- **Storage**: Single Availability Zone (not multiple AZs)
- **Cost**: 20% less than Standard-IA
- **Availability**: 99.5%
- **Risk**: Data lost if AZ is destroyed
- **Best For**: Recreatable data, secondary backup copies

##### 5. S3 Glacier Instant Retrieval

- **Use Case**: Archive data requiring instant access
- **Retrieval**: Millisecond retrieval times
- **Cost**: Lower storage cost than Standard-IA
- **Minimum Duration**: 90-day minimum storage charge
- **Best For**: Medical images, news media assets accessed once per quarter

##### 6. S3 Glacier Flexible Retrieval

- **Use Case**: Archive data with retrieval times from minutes to hours
- **Retrieval Options**:
  - **Expedited**: 1-5 minutes
  - **Standard**: 3-5 hours
  - **Bulk**: 5-12 hours (lowest cost)
- **Minimum Duration**: 90-day minimum storage charge
- **Best For**: Backup and archive data accessed 1-2 times per year

##### 7. S3 Glacier Deep Archive

- **Use Case**: Long-term archive and digital preservation
- **Cost**: Lowest cost storage class
- **Retrieval Time**: 12-48 hours
- **Minimum Duration**: 180-day minimum storage charge
- **Best For**: Compliance archives, digital preservation, data retained for 7-10+ years

#### S3 Features

**Versioning**
- Keep multiple versions of an object
- Protect against accidental deletion
- Can be enabled/suspended per bucket

**Lifecycle Policies**
- Automatically transition objects between storage classes
- Automatically delete objects after a specified time
- Cost optimization through automation

**Encryption**
- **Server-Side Encryption (SSE)**: S3 encrypts objects
  - SSE-S3: S3-managed keys
  - SSE-KMS: AWS KMS-managed keys
  - SSE-C: Customer-provided keys
- **Client-Side Encryption**: Encrypt before uploading

**Access Control**
- Bucket policies (resource-based)
- IAM policies (identity-based)
- Access Control Lists (ACLs) - legacy
- S3 Block Public Access settings

**Additional Features**
- Static website hosting
- Cross-Region Replication (CRR)
- Same-Region Replication (SRR)
- Transfer Acceleration (via CloudFront edge locations)
- Event notifications
- S3 Select (query data using SQL)

**S3 Storage Class Cost Comparison**

| Storage Class | Storage Cost (per GB/month) | Retrieval Cost | Minimum Duration | Retrieval Time | Best Use Case |
|---------------|---------------------------|----------------|------------------|----------------|---------------|
| **S3 Standard** | $0.023 | None | None | Milliseconds | Frequently accessed data |
| **S3 Intelligent-Tiering** | $0.023-$0.0125 + $0.0025 monitoring | None (auto-tier) | None | Milliseconds | Unknown access patterns |
| **S3 Standard-IA** | $0.0125 | $0.01 per GB | 30 days | Milliseconds | Infrequent access |
| **S3 One Zone-IA** | $0.01 | $0.01 per GB | 30 days | Milliseconds | Recreatable data |
| **S3 Glacier Instant Retrieval** | $0.004 | $0.03 per GB | 90 days | Milliseconds | Archive with instant access |
| **S3 Glacier Flexible Retrieval** | $0.0036 | $0.01-$0.03 per GB | 90 days | Minutes-hours | Archive, 1-2 times/year |
| **S3 Glacier Deep Archive** | $0.00099 | $0.02 per GB | 180 days | 12-48 hours | Long-term compliance |

**Cost Example: 100 TB of Data Storage**

| Scenario | Storage Class | Monthly Storage | Retrieval (10% monthly) | Total Monthly Cost | Annual Cost |
|----------|---------------|----------------|------------------------|-------------------|-------------|
| **Active Website** | S3 Standard | $2,355 | $0 | $2,355 | $28,260 |
| **Backup (weekly access)** | S3 Standard-IA | $1,280 | $102 | $1,382 | $16,584 |
| **Compliance Archive** | Glacier Deep Archive | $102 | $205 | $307 | $3,684 |
| **Unknown Pattern** | Intelligent-Tiering | $850-$2,355 | $0 | ~$1,500 | $18,000 |

**Real-World Use Case: Media Company**

**Scenario**: Video streaming platform with 500 TB of content.

**Storage Strategy**:
1. **Recent content** (20 TB, last 30 days): S3 Standard
   - High access rate, low latency needed
   - Cost: 20,000 GB × $0.023 = $460/month

2. **Popular library** (100 TB, accessed weekly): S3 Intelligent-Tiering
   - Access patterns vary by content popularity
   - Cost: ~$1,700/month (auto-optimizes)

3. **Archive content** (300 TB, accessed rarely): Glacier Flexible Retrieval
   - Old shows, accessed 1-2 times per year
   - Cost: 300,000 GB × $0.0036 = $1,080/month

4. **Legal/compliance** (80 TB, 7-year retention): Glacier Deep Archive
   - Almost never accessed
   - Cost: 80,000 GB × $0.00099 = $79/month

**Total**: $3,319/month vs $11,500/month if everything in S3 Standard (71% savings)

**S3 Performance Benchmarks**

| Metric | Performance | Notes |
|--------|-------------|-------|
| **Request Rate** | 3,500 PUT/COPY/POST/DELETE per second per prefix | Can scale by using multiple prefixes |
| **Request Rate** | 5,500 GET/HEAD per second per prefix | Automatic scaling |
| **Throughput** | No limit | Scales automatically |
| **Latency** | 100-200ms (first byte) | Standard, IA, One Zone-IA |
| **Durability** | 99.999999999% (11 nines) | Designed to sustain loss of 2 facilities |
| **Availability SLA** | 99.9-99.99% | Varies by storage class |

**Common Configuration Mistakes**:
1. Using S3 Standard for infrequently accessed data (overpaying)
2. Not enabling versioning for critical data (risk of data loss)
3. Not using lifecycle policies (manual tier management)
4. Allowing public access unintentionally (security risk)
5. Not enabling server-side encryption (compliance issues)
6. Not using S3 Transfer Acceleration for global uploads
7. Storing small objects inefficiently (minimum billable size)
8. Not implementing proper backup/replication strategy

**Service Limits and Quotas**:
- Buckets per account: 100 (soft limit, can be increased to 1,000)
- Object size: 5 TB maximum
- Single PUT: 5 GB maximum (use multipart for larger)
- Bucket name: 3-63 characters, globally unique
- Bucket policy: 20 KB maximum
- Tags per object: 10

**Integration Example: Automated Data Pipeline**

```
Architecture:
On-premises data
  └─> AWS DataSync / S3 Transfer
      └─> S3 (Standard)
          ├─> Lambda (process new files)
          │   └─> DynamoDB (metadata)
          │
          ├─> Lifecycle Policy (30 days)
          │   └─> S3 Intelligent-Tiering
          │
          ├─> Lifecycle Policy (90 days)
          │   └─> Glacier Flexible Retrieval
          │
          └─> Lifecycle Policy (365 days)
              └─> Glacier Deep Archive
```

**Monitoring and Troubleshooting**:

1. **CloudWatch Metrics**:
   - NumberOfObjects
   - BucketSizeBytes
   - AllRequests
   - 4xxErrors / 5xxErrors
   - FirstByteLatency

2. **S3 Storage Lens** (analytics):
   - Usage and activity metrics
   - Cost optimization recommendations
   - Data protection insights
   - Access patterns

3. **S3 Access Logging**:
   - Track all requests to bucket
   - Security and access audits
   - Usage analysis

4. **Common Issues**:
   - Slow upload: Use S3 Transfer Acceleration or multipart upload
   - 403 Forbidden: Check bucket policy and IAM permissions
   - 503 SlowDown: Reduce request rate or use prefixes
   - High costs: Implement lifecycle policies and use appropriate storage class

**Best Practices**:
1. **Security**:
   - Enable S3 Block Public Access
   - Use bucket policies and IAM roles
   - Enable versioning for critical data
   - Enable server-side encryption
   - Use S3 Object Lock for compliance

2. **Cost Optimization**:
   - Use S3 Intelligent-Tiering for unknown patterns
   - Implement lifecycle policies
   - Delete incomplete multipart uploads
   - Use S3 Storage Lens for optimization insights
   - Consider Requester Pays for public datasets

3. **Performance**:
   - Use prefixes to distribute load
   - Enable S3 Transfer Acceleration for global users
   - Use CloudFront for frequently accessed content
   - Implement multipart upload for large files
   - Use byte-range fetches for large downloads

4. **Data Protection**:
   - Enable versioning
   - Use Cross-Region Replication for DR
   - Implement MFA Delete for critical buckets
   - Regular backup testing
   - Use S3 Object Lock for WORM requirements

### Storage Service Comparison Table

| Service | Type | Use Case | Shared Access | Pricing Model | Typical Latency |
|---------|------|----------|---------------|---------------|-----------------|
| **S3** | Object | Backups, web content, data lakes | Via API/URL | Per GB stored + requests | 100-200ms |
| **EBS** | Block | EC2 instance storage | Single instance | Per GB provisioned | <10ms |
| **EFS** | File | Shared file storage | Multiple instances | Per GB used | Low ms |
| **FSx for Windows** | File | Windows file shares | Multiple instances | Per GB provisioned | Sub-ms |
| **FSx for Lustre** | File | HPC, ML training | Multiple instances | Per GB provisioned | Sub-ms |
| **Storage Gateway** | Hybrid | On-premises to cloud | On-premises + cloud | Per GB stored | Varies |
| **Snow Family** | Physical | Offline data transfer | Physical device | Per device + data transfer | N/A |

### Amazon EBS (Elastic Block Store)

**Block-level storage volumes for EC2 instances**

Persistent block storage that can be attached to EC2 instances, similar to a hard drive.

**Key Characteristics**:
- Persistent storage that exists independently of instance lifetime
- Attached to a single EC2 instance at a time
- Automatically replicated within its Availability Zone
- Snapshots stored in Amazon S3
- Can detach and reattach to different instances
- Supports encryption at rest and in transit

#### EBS Volume Types

| Type | Category | Use Case | Performance |
|------|----------|----------|-------------|
| **gp3** | General Purpose SSD | Balanced price/performance | 3,000-16,000 IOPS |
| **gp2** | General Purpose SSD | Balanced price/performance | Baseline 3 IOPS/GB |
| **io2/io1** | Provisioned IOPS SSD | Mission-critical, high-performance | Up to 64,000 IOPS |
| **st1** | Throughput Optimized HDD | Frequently accessed, throughput-intensive | Good for big data, data warehouses |
| **sc1** | Cold HDD | Infrequently accessed | Lowest cost, file servers |

**Best Practices**:
- Take regular snapshots for backup
- Use Provisioned IOPS for database workloads
- General Purpose SSD for most use cases

### Amazon EFS (Elastic File System)

**Managed NFS file system for EC2**

- **File System Type**: Network File System (NFS) v4.1
- **Shared Access**: Multiple EC2 instances can access simultaneously
- **Automatic Scaling**: Grows and shrinks automatically as you add/remove files
- **Pricing**: Pay only for storage used (no pre-provisioning)
- **Availability**: Regional service spanning multiple Availability Zones
- **Performance Modes**:
  - **General Purpose**: Low latency (web serving, CMS)
  - **Max I/O**: Higher latency, massively parallel (big data, media processing)

**Use Cases**:
- Content management systems
- Web serving
- Data sharing and collaboration
- Development environments
- Container storage

**Comparison**:
- **EBS**: Single instance, must be in same AZ
- **EFS**: Multiple instances, cross-AZ access
- **S3**: Object storage, unlimited instances via API

### AWS Storage Gateway

**Hybrid cloud storage service**

Connects on-premises environments to AWS cloud storage.

#### Gateway Types

**1. File Gateway**
- Presents NFS/SMB file shares
- Files stored as objects in S3
- Local cache for frequently accessed data
- Use Case: Cloud-backed file shares

**2. Volume Gateway**
- Presents iSCSI block storage volumes
- Two modes:
  - **Cached Volumes**: Primary data in S3, cache on-premises
  - **Stored Volumes**: Primary data on-premises, async backup to S3
- Use Case: Block storage backup and disaster recovery

**3. Tape Gateway**
- Virtual tape library (VTL)
- Integrates with existing backup software
- Store virtual tapes in S3 and Glacier
- Use Case: Replace physical tape backup infrastructure

**Benefits**:
- Seamless cloud integration
- Local caching for low latency
- Cost-effective storage
- Disaster recovery

### AWS Snow Family

**Physical devices for data migration and edge computing**

Move large amounts of data into and out of AWS using secure physical devices.

#### Devices

| Device | Storage Capacity | Use Case |
|--------|-----------------|----------|
| **Snowcone** | 8 TB usable | Smallest, portable, edge computing |
| **Snowball Edge Storage Optimized** | 80 TB | Large data migrations |
| **Snowball Edge Compute Optimized** | 42 TB + compute | Edge computing + storage |
| **Snowmobile** | 100 PB | Massive data center migration |

**Snowball Edge Features**:
- Can run EC2 instances and Lambda functions
- Cluster multiple devices together
- Storage and compute in disconnected environments

**Process**:
1. Order device from AWS
2. AWS ships device to your location
3. Connect device and copy data
4. Ship device back to AWS
5. AWS loads data into your S3 bucket

**When to Use**:
- **Snowcone/Snowball**: Terabytes to petabytes of data
- **Snowmobile**: 10 PB or more
- Limited bandwidth or expensive network transfer
- Physical data migration requirements

> **Exam Tip**: Choose storage based on use case:
> - **S3**: Object storage, web content, backups
> - **EBS**: EC2 instance block storage, databases
> - **EFS**: Shared file system across multiple instances
> - **Storage Gateway**: Hybrid cloud storage
> - **Snow Family**: Large-scale data migration

---

## Database Services

### Amazon RDS (Relational Database Service)

**Managed relational database service**

RDS makes it easy to set up, operate, and scale relational databases in the cloud.

**Supported Database Engines**:
- MySQL
- PostgreSQL
- MariaDB
- Oracle Database
- Microsoft SQL Server
- Amazon Aurora

**Key Features**:
- **Automated Management**: Backups, patching, scaling
- **No OS Access**: Fully managed (no SSH access)
- **High Availability**: Multi-AZ deployments
- **Scalability**: Read replicas for read-heavy workloads
- **Security**: Encryption at rest and in transit
- **Monitoring**: CloudWatch integration

**Pricing**: Pay for instance type, storage, backups, and data transfer

#### Multi-AZ Deployments

**High availability and disaster recovery**

- **Replication**: Synchronous replication to standby instance in different AZ
- **Automatic Failover**: Automatic failover to standby if primary fails
- **Purpose**: High availability and disaster recovery (not performance)
- **Downtime**: Minimal during failover (typically under 1 minute)
- **Backup**: Backups taken from standby to avoid I/O impact

**When to Use**: Production workloads requiring high availability

#### Read Replicas

**Scale read-heavy workloads**

- **Replication**: Asynchronous replication from primary
- **Purpose**: Improve read performance, not high availability
- **Location**: Can be in same AZ, different AZ, or different Region
- **Promotion**: Can be promoted to standalone database
- **Limit**: Up to 5 read replicas per primary database
- **Use Cases**: Reporting, analytics, read-heavy applications

**Comparison**:
- **Multi-AZ**: Synchronous, same Region, automatic failover, HA/DR
- **Read Replicas**: Asynchronous, can be cross-Region, read scaling

### Amazon Aurora

**AWS cloud-native relational database**

- **Compatibility**: MySQL and PostgreSQL compatible
- **Performance**:
  - 5x faster than standard MySQL
  - 3x faster than standard PostgreSQL
- **Read Replicas**: Up to 15 Aurora read replicas
- **Storage**: Automatically scales from 10 GB to 128 TB
- **Durability**: 6 copies of data across 3 Availability Zones
- **Availability**: Self-healing storage, automated failover
- **Cost**: More expensive than RDS, but better performance and features

**Aurora Serverless**:
- On-demand, auto-scaling configuration
- Automatically starts up, scales, and shuts down
- Pay per second for resources consumed
- Ideal for infrequent, intermittent, or unpredictable workloads

**When to Choose Aurora**:
- Need better performance than standard RDS
- High availability requirements
- Rapid scaling needs
- MySQL or PostgreSQL compatibility required

### Amazon DynamoDB

**Fully managed NoSQL database service**

Fast and flexible NoSQL database for any scale.

**Key Characteristics**:
- **Type**: Key-value and document database
- **Performance**: Single-digit millisecond latency at any scale
- **Serverless**: Fully managed, no servers to provision
- **Scaling**: Automatic scaling of throughput and storage
- **Availability**: Multi-AZ replication, highly available

**Features**:
- **Global Tables**: Multi-region, multi-active replication
- **DynamoDB Streams**: Capture item-level changes
- **DAX (DynamoDB Accelerator)**: In-memory cache for microsecond latency
- **Transactions**: ACID transactions support
- **Backup**: Point-in-time recovery, on-demand backups

**Pricing Models**:
- **On-Demand**: Pay per request (unpredictable workloads)
- **Provisioned Capacity**: Reserve read/write capacity units (predictable workloads)

**Use Cases**:
- Mobile and web applications
- Gaming applications
- IoT data storage
- Session management
- Real-time bidding

### Amazon ElastiCache

**In-memory caching service**

Managed Redis or Memcached for improved application performance.

**Supported Engines**:
- **Redis**: Advanced data structures, persistence, replication, pub/sub
- **Memcached**: Simple caching, multi-threading

**Benefits**:
- **Performance**: Microsecond latency
- **Reduced Database Load**: Cache frequently accessed data
- **Scalability**: Easily scale in/out
- **Availability**: Multi-AZ with automatic failover (Redis)

**Common Use Cases**:
- Database query result caching
- Session stores for web applications
- Real-time analytics
- Leaderboards and counting
- Message queues (Redis)

**When to Use**:
- Improve read performance
- Reduce database costs
- Store session data
- Real-time applications

### Amazon Redshift

**Fully managed data warehouse**

Fast, scalable data warehouse for analytics.

**Key Features**:
- **Scale**: Petabyte-scale data warehouse
- **Storage**: Columnar storage format
- **Processing**: Massively parallel processing (MPP)
- **Query Language**: Standard SQL
- **Integration**: Integrates with BI tools (Tableau, Power BI, QuickSight)
- **Performance**: 10x better performance than traditional data warehouses

**Use Cases**:
- Business intelligence and analytics
- Historical data analysis
- Complex queries on large datasets
- Data consolidation from multiple sources

**Redshift Spectrum**:
- Query data directly in S3 without loading
- Extend queries beyond Redshift data warehouse

### Amazon Neptune

**Fully managed graph database**

- **Type**: Graph database service
- **Models**: Supports Property Graph and RDF graph models
- **Query Languages**: Gremlin and SPARQL
- **Performance**: Fast query execution on billions of relationships
- **Availability**: Multi-AZ, read replicas

**Use Cases**:
- Social networking applications
- Recommendation engines
- Fraud detection
- Knowledge graphs
- Network and IT operations

### Amazon DocumentDB

**MongoDB-compatible document database**

- **Compatibility**: MongoDB-compatible (supports MongoDB APIs)
- **Management**: Fully managed by AWS
- **Scaling**: Scale storage and compute independently
- **Availability**: Multi-AZ, automated backups
- **Performance**: 2x throughput improvement over MongoDB

**Use Cases**:
- Content management systems
- Product catalogs
- User profiles
- Mobile and web app backends

> **Key Point**: Database selection guide:
> - **RDS/Aurora**: Relational databases, ACID transactions, SQL
> - **DynamoDB**: NoSQL key-value, high scale, low latency
> - **Redshift**: Data warehouse, analytics, BI
> - **ElastiCache**: In-memory caching, session storage
> - **Neptune**: Graph databases, relationships
> - **DocumentDB**: Document database, MongoDB workloads

### Database Service Detailed Comparison

| Database | Type | Engine Options | Scaling | High Availability | Best For | Pricing Model |
|----------|------|----------------|---------|-------------------|----------|---------------|
| **RDS** | Relational | MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora | Vertical (instance resize) | Multi-AZ | Traditional RDBMS | Instance + storage |
| **Aurora** | Relational | MySQL, PostgreSQL | Auto storage + read replicas | Built-in Multi-AZ | High-performance RDBMS | Instance + I/O |
| **DynamoDB** | NoSQL Key-Value | N/A | Automatic horizontal | Built-in Multi-AZ | Massive scale, low latency | On-Demand or provisioned |
| **ElastiCache** | In-Memory | Redis, Memcached | Horizontal (add nodes) | Multi-AZ (Redis) | Caching, session store | Node hours |
| **Redshift** | Data Warehouse | PostgreSQL-compatible | Add nodes to cluster | Single-AZ (snapshots for backup) | Analytics, BI | Node hours |
| **Neptune** | Graph | Gremlin, SPARQL | Vertical + read replicas | Multi-AZ | Relationship queries | Instance + I/O |
| **DocumentDB** | Document | MongoDB-compatible | Vertical + read replicas | Multi-AZ | Document store | Instance + storage |
| **Timestream** | Time Series | N/A | Automatic | Multi-AZ | IoT, metrics | Storage + queries |

### Database Selection Flowchart

```
START: Which database service should I use?
│
├─> Need SQL and ACID transactions?
│   └─> YES: Relational Database
│       ├─> Maximum performance needed? → Aurora
│       ├─> Oracle/SQL Server licensing? → RDS (Oracle/SQL Server)
│       ├─> MySQL/PostgreSQL? → RDS or Aurora
│       └─> Analytics/BI workload? → Redshift
│
├─> Need to cache or session storage?
│   └─> YES: ElastiCache
│       ├─> Advanced data structures? → Redis
│       └─> Simple caching? → Memcached
│
├─> Need document database?
│   └─> YES:
│       ├─> MongoDB workloads? → DocumentDB
│       └─> Flexible NoSQL? → DynamoDB
│
├─> Need graph database?
│   └─> YES: Neptune
│
├─> Need NoSQL at massive scale?
│   └─> YES: DynamoDB
│       ├─> Predictable traffic? → Provisioned capacity
│       └─> Variable traffic? → On-Demand capacity
│
└─> Time-series data (IoT, metrics)?
    └─> YES: Timestream
```

### Real-World Database Use Cases

**Use Case 1: E-Commerce Platform**

**Requirements**:
- Product catalog: 1M products
- User accounts: 10M users
- Orders: 1M transactions/month
- Shopping carts: Temporary, high read/write

**Architecture**:
```
Frontend (S3 + CloudFront)
  └─> API Gateway + Lambda
      ├─> DynamoDB (Shopping carts, sessions)
      │   - On-Demand capacity
      │   - Single-digit millisecond latency
      │   - Cost: ~$200/month
      │
      ├─> ElastiCache Redis (Product catalog cache)
      │   - cache.t3.medium: $0.068/hour
      │   - Cost: ~$50/month
      │   - 99% cache hit rate
      │
      ├─> Aurora MySQL (Orders, inventory)
      │   - db.r5.large: $0.29/hour
      │   - Multi-AZ for HA
      │   - Cost: ~$430/month
      │
      └─> Redshift (Analytics, business intelligence)
          - dc2.large: $0.25/hour (2 nodes)
          - Cost: ~$360/month
```

**Total Database Cost**: ~$1,040/month

**Performance**:
- Cart operations: <10ms (DynamoDB)
- Product lookups: <5ms (ElastiCache)
- Order processing: <50ms (Aurora)
- Analytics queries: seconds to minutes (Redshift)

**Use Case 2: Social Media Application**

**Requirements**:
- User profiles and relationships
- Activity feeds
- Real-time messaging
- Content recommendations

**Architecture**:
```
Application Layer
  ├─> Neptune (User relationships, friend graphs)
  │   - db.r5.large: $0.348/hour
  │   - Cost: ~$254/month
  │   - Query: "Friends of friends" in milliseconds
  │
  ├─> DynamoDB (User profiles, posts, activity feeds)
  │   - Global Tables for multi-region
  │   - Cost: ~$500/month (variable)
  │
  ├─> ElastiCache Redis (Real-time messaging, sessions)
  │   - cache.r5.large cluster
  │   - Cost: ~$200/month
  │
  └─> DocumentDB (Content metadata, analytics)
      - db.r5.large: $0.29/hour
      - Cost: ~$212/month
```

**Total**: ~$1,166/month for millions of users

### Database Cost Comparison Examples

**Scenario: MySQL Database for Production Application**

| Workload | AWS Service | Configuration | Monthly Cost | Management Effort | Scalability |
|----------|-------------|---------------|--------------|-------------------|-------------|
| **Small** (< 100 GB) | RDS MySQL | db.t3.medium, 100 GB | $75 | Low | Vertical |
| **Small Serverless** | Aurora Serverless v2 | 0.5-1 ACU | $45-90 | Minimal | Automatic |
| **Medium** (500 GB) | RDS MySQL Multi-AZ | db.m5.large, 500 GB | $385 | Low | Vertical |
| **Medium** | Aurora MySQL | db.r5.large, 500 GB | $430 | Low | Vertical + Horizontal |
| **Large** (2 TB) | Aurora MySQL | db.r5.2xlarge + 5 read replicas | $1,850 | Low | High |
| **On-premises equivalent** | Self-managed | 3 servers, licenses, staff | $3,000+ | High | Manual |

**Scenario: NoSQL Database - DynamoDB vs Self-Managed**

| Metric | DynamoDB | Self-Managed Cassandra on EC2 |
|--------|----------|-------------------------------|
| **Setup Time** | Minutes | Days to weeks |
| **Management** | Fully managed | You manage everything |
| **Scaling** | Automatic, instant | Manual cluster management |
| **Performance** | Single-digit milliseconds | Tuning required |
| **High Availability** | Built-in, multi-AZ | Must configure |
| **Cost (10GB, 1M reads/writes per day)** | ~$25/month | ~$150/month (instances + management) |
| **Cost (1TB, 100M reads/writes per day)** | ~$1,250/month | ~$1,500/month |
| **Staff Required** | None | 1-2 DBAs |

### Database Migration Scenarios

**Scenario 1: Oracle to Aurora PostgreSQL**

**Current State**: On-premises Oracle RAC
- Database size: 2 TB
- Monthly license cost: $5,000
- Hardware and maintenance: $3,000/month
- **Total**: $8,000/month

**Migration Strategy**:
1. **Assessment** (Week 1):
   - Use AWS Schema Conversion Tool (SCT)
   - Identify incompatible schema objects
   - Estimate conversion effort

2. **Schema Conversion** (Weeks 2-3):
   - Convert DDL using SCT
   - Modify incompatible SQL
   - Test converted schema

3. **Data Migration** (Week 4):
   - Use AWS DMS for initial load
   - Setup ongoing replication
   - Validate data integrity

4. **Cutover** (Week 5):
   - Stop writes to source
   - Final sync with DMS
   - Switch application to Aurora
   - Monitor performance

**After Migration**:
- Aurora PostgreSQL: db.r5.2xlarge Multi-AZ
- Monthly cost: $950
- **Savings**: $7,050/month (88% reduction)
- **ROI**: Migration cost recovered in 2 months

**Scenario 2: Self-Managed MongoDB to DocumentDB**

**Current State**: MongoDB on EC2
- 3 r5.xlarge instances: $438/month
- Management overhead: 20 hours/month
- Backup storage: $100/month
- **Total**: $538/month + management time

**Migration Process**:
1. Create DocumentDB cluster
2. Use mongodump/mongorestore
3. Test application compatibility
4. Switch connection strings
5. Decommission EC2 instances

**After Migration**:
- DocumentDB: db.r5.large Multi-AZ
- Monthly cost: $424/month
- **Savings**: $114/month + management time
- **Benefits**: Automated backups, patching, monitoring

### Common Database Configuration Mistakes

**RDS/Aurora**:
1. Not enabling automated backups
2. Single-AZ for production databases
3. Not using read replicas for read-heavy workloads
4. Over-provisioning instance size
5. Not enabling encryption at rest
6. Public accessibility enabled
7. Not monitoring slow query logs
8. Inadequate maintenance window planning

**DynamoDB**:
1. Not using on-demand for variable workloads
2. Over-provisioning read/write capacity
3. Poor partition key design (hot partitions)
4. Not using DAX for read-heavy workloads
5. Not implementing TTL for temporary data
6. Missing Global Secondary Indexes
7. Not using DynamoDB Streams for change capture
8. Storing large items (> 400 KB inefficient)

**ElastiCache**:
1. Not implementing proper key expiration
2. Single-node for production (no HA)
3. Wrong cache eviction policy
4. Not monitoring cache hit ratio
5. Storing too much data per key
6. Not using cluster mode for Redis
7. Connection pooling not implemented
8. Cache stampede not handled

### Database Service Limits

**RDS**:
- DB instances per Region: 40 (can be increased)
- Max storage: 64 TB (SQL Server), 128 TB (others)
- Read replicas: 5 per primary (15 for Aurora)
- Automated backup retention: 35 days max
- Manual snapshots: 100 per Region (can be increased)

**DynamoDB**:
- Table size: Unlimited
- Item size: 400 KB max
- Partition throughput: 3,000 RCU / 1,000 WCU per partition
- Global Secondary Indexes: 20 per table
- Local Secondary Indexes: 5 per table
- Tables per Region: 2,500 (soft limit)

**ElastiCache**:
- Nodes per Region: 300 (soft limit)
- Nodes per cluster: 90 (Redis), 40 (Memcached)
- Parameter groups: 150 per Region
- Subnet groups: 150 per Region

### Database Monitoring and Troubleshooting

**RDS/Aurora Monitoring**:
1. **CloudWatch Metrics**:
   - DatabaseConnections
   - CPUUtilization
   - FreeableMemory
   - ReadLatency / WriteLatency
   - ReadIOPS / WriteIOPS

2. **Performance Insights**:
   - Top SQL queries by load
   - Wait events analysis
   - Database load visualization
   - Cost: Free for 7 days retention, paid for longer

3. **Enhanced Monitoring**:
   - OS-level metrics (50+ metrics)
   - Real-time monitoring (1-second granularity)
   - Process and thread information

**DynamoDB Monitoring**:
1. **CloudWatch Metrics**:
   - ConsumedReadCapacityUnits
   - ConsumedWriteCapacityUnits
   - UserErrors / SystemErrors
   - ThrottledRequests
   - ConditionalCheckFailedRequests

2. **DynamoDB Contributor Insights**:
   - Most accessed items
   - Throttled requests
   - Hot partitions identification

3. **Common Issues**:
   - Hot partitions: Redesign partition key
   - Throttling: Increase capacity or use on-demand
   - Large items: Break into smaller items
   - High costs: Review capacity settings and use on-demand

### Database Best Practices

**General Practices**:
1. **Security**:
   - Enable encryption at rest and in transit
   - Use IAM database authentication
   - Store credentials in Secrets Manager
   - Apply least privilege access
   - Regular security patching

2. **High Availability**:
   - Multi-AZ deployments for production
   - Regular backup testing
   - Automated failover testing
   - Cross-Region replication for DR

3. **Performance**:
   - Right-size instances based on metrics
   - Use read replicas for read scaling
   - Implement connection pooling
   - Monitor slow queries
   - Regular index maintenance

4. **Cost Optimization**:
   - Use Reserved Instances for steady workloads
   - Right-size instances (avoid over-provisioning)
   - Delete old snapshots
   - Use Aurora Serverless for variable workloads
   - Implement data lifecycle policies

---

## Networking and Content Delivery

### Amazon VPC (Virtual Private Cloud)

**Isolated virtual network in AWS**

VPC allows you to provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network you define.

**Core Concepts**:
- **Your Private Network**: Define your own IP address range
- **CIDR Blocks**: Define IP address range (e.g., 10.0.0.0/16)
- **Subnets**: Divide VPC into subnets across Availability Zones
- **Route Tables**: Control traffic routing
- **Isolation**: Complete control over networking environment

#### VPC Components

**Subnets**

Segments of your VPC's IP address range where you launch AWS resources.

- **Public Subnet**:
  - Has a route to an Internet Gateway
  - Instances can have public IP addresses
  - Accessible from the internet

- **Private Subnet**:
  - No direct route to the internet
  - Instances typically use private IP addresses only
  - Access internet via NAT Gateway

**Internet Gateway (IGW)**
- Horizontally scaled, redundant, highly available
- Allows communication between VPC and the internet
- Supports IPv4 and IPv6
- One IGW per VPC

**NAT Gateway**
- Enables instances in private subnet to access the internet
- Prevents internet from initiating connections to private instances
- Managed by AWS (highly available within AZ)
- Alternative: NAT Instance (EC2 instance, customer-managed)

**Route Tables**
- Control where network traffic is directed
- Each subnet must be associated with a route table
- Determines routing for traffic leaving the subnet

#### Security

**Security Groups**

Virtual firewall for EC2 instances (instance-level).

- **Level**: Instance level (first layer of defense)
- **Rules**: Allow rules only (no deny rules)
- **Statefulness**: Stateful - return traffic automatically allowed
- **Default**: All inbound traffic denied, all outbound allowed
- **Example**: Allow HTTP (port 80) from anywhere, SSH (port 22) from specific IP

**Network ACLs (Access Control Lists)**

Firewall for subnets (subnet-level).

- **Level**: Subnet level (second layer of defense)
- **Rules**: Both allow and deny rules
- **Statefulness**: Stateless - must explicitly allow return traffic
- **Evaluation**: Rules processed in order (numbered)
- **Default**: Default NACL allows all inbound/outbound traffic

**Comparison**:

| Feature | Security Groups | Network ACLs |
|---------|----------------|--------------|
| **Level** | Instance | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow and Deny |
| **Rule Processing** | All rules evaluated | Rules in number order |
| **Applies To** | Instances | All instances in subnet |

#### Connectivity

**VPC Peering**
- Connect two VPCs privately
- Route traffic using private IP addresses
- Can peer VPCs across accounts and Regions
- Non-transitive (no chaining)

**VPN Gateway**
- Connect on-premises network to VPC via VPN
- Encrypted connection over the internet
- Site-to-Site VPN

**Direct Connect**
- Dedicated private connection from on-premises to AWS
- Bypass the public internet
- Covered in detail below

### Amazon CloudFront

**Global Content Delivery Network (CDN)**

Deliver content to users with low latency and high transfer speeds.

**Key Features**:
- **Edge Locations**: 400+ globally distributed edge locations
- **Caching**: Cache content closer to end users
- **Origin Support**: S3, EC2, ELB, on-premises servers
- **Performance**: Reduced latency for global users
- **Security**: Integration with AWS Shield (DDoS protection) and AWS WAF

**Use Cases**:
- Static content delivery (images, CSS, JavaScript)
- Dynamic content delivery
- Video streaming (live and on-demand)
- API acceleration
- Software distribution

**How It Works**:
1. User requests content
2. Request routed to nearest edge location
3. CloudFront checks cache
4. If cached, content delivered immediately
5. If not cached, CloudFront retrieves from origin, caches, and delivers

**Benefits**:
- Improved performance
- Cost reduction (reduced origin load)
- Global reach
- Enhanced security

### Amazon Route 53

**Highly available and scalable DNS web service**

**Core Functions**:
1. **Domain Registration**: Register domain names
2. **DNS Routing**: Route internet traffic to resources
3. **Health Checking**: Monitor resource health and route traffic accordingly

**Routing Policies**:

| Policy | Description | Use Case |
|--------|-------------|----------|
| **Simple** | Single resource | One web server |
| **Weighted** | Distribute traffic by percentage | A/B testing, gradual migration |
| **Latency** | Route to lowest latency endpoint | Global applications |
| **Failover** | Active-passive failover | Disaster recovery |
| **Geolocation** | Route based on user's location | Content localization, compliance |
| **Geoproximity** | Route based on resource and user location | Bias traffic to specific locations |
| **Multi-value** | Return multiple IP addresses | Simple load balancing with health checks |

**Features**:
- 100% availability SLA
- Global anycast network
- DNSSEC for domain security
- Integration with AWS services
- Traffic flow for complex routing

### Elastic Load Balancing (ELB)

**Automatically distribute incoming traffic across multiple targets**

Load balancers improve application availability and fault tolerance.

#### Load Balancer Types

**Application Load Balancer (ALB)**

Layer 7 load balancer for HTTP/HTTPS traffic.

- **OSI Layer**: Layer 7 (Application)
- **Protocols**: HTTP, HTTPS, gRPC
- **Routing**: Path-based, host-based, HTTP header-based
- **Targets**: EC2 instances, containers, IP addresses, Lambda functions
- **Features**:
  - SSL/TLS termination
  - WebSocket support
  - HTTP/2 support
  - Sticky sessions
  - Authentication (OIDC, SAML)
- **Best For**: Web applications, microservices, container-based applications

**Network Load Balancer (NLB)**

Layer 4 load balancer for TCP/UDP traffic.

- **OSI Layer**: Layer 4 (Transport)
- **Protocols**: TCP, UDP, TLS
- **Performance**: Millions of requests per second, ultra-low latency
- **Static IP**: Static IP addresses per AZ
- **Targets**: EC2 instances, containers, IP addresses
- **Features**:
  - Extreme performance
  - Static/Elastic IP addresses
  - TLS termination
  - Preserve source IP
- **Best For**: High performance, low latency requirements, TCP/UDP applications

**Gateway Load Balancer**

Layer 3 load balancer for virtual appliances.

- **OSI Layer**: Layer 3 (Network)
- **Protocol**: IP
- **Use Case**: Deploy, scale, and manage third-party virtual appliances
- **Examples**: Firewalls, intrusion detection systems, deep packet inspection
- **Features**:
  - GENEVE protocol support
  - High availability for appliances
  - Centralized security management

**Classic Load Balancer (CLB)**

Previous generation load balancer.

- **Status**: Being phased out
- **OSI Layer**: Layer 4 and Layer 7
- **Recommendation**: Use ALB or NLB for new applications

**Choosing a Load Balancer**:
- **ALB**: HTTP/HTTPS applications, microservices
- **NLB**: TCP/UDP applications, extreme performance
- **GWLB**: Third-party virtual appliances

### AWS Direct Connect

**Dedicated private network connection**

Establish a dedicated private connection from on-premises to AWS.

**Key Features**:
- **Connection Type**: Private, dedicated network connection
- **Bandwidth**: 1 Gbps, 10 Gbps, or 100 Gbps
- **Bypass Internet**: Traffic doesn't traverse the public internet
- **Performance**: Consistent network performance
- **Access**: Connect to both public AWS services and VPCs
- **Cost**: Reduced bandwidth costs for large data transfer

**Benefits**:
- Predictable network performance
- Reduced bandwidth costs
- Consistent latency
- Enhanced security (private connection)
- Access to public and private AWS resources

**Setup Time**: Weeks to months to provision

**Use Cases**:
- Large data transfers
- Real-time data feeds
- Hybrid cloud architectures
- Accessing VPCs across multiple Regions

### AWS VPN

**Encrypted connection over the internet**

#### Site-to-Site VPN

Connect on-premises network to AWS VPC.

- **Connection**: Encrypted IPsec VPN tunnel over the internet
- **Components**: Customer Gateway (on-premises) + Virtual Private Gateway (AWS)
- **Setup Time**: Minutes to hours
- **Cost**: Lower cost than Direct Connect
- **Bandwidth**: Limited by internet connection

#### Client VPN

Connect individual users to AWS or on-premises networks.

- **Use Case**: Remote user access
- **Connection**: Encrypted TLS VPN connection
- **Access**: AWS VPC resources and on-premises resources

**VPN vs Direct Connect**:

| Feature | Site-to-Site VPN | Direct Connect |
|---------|-----------------|----------------|
| **Connection** | Over internet | Private dedicated line |
| **Setup Time** | Minutes | Weeks/months |
| **Cost** | Lower | Higher |
| **Bandwidth** | Internet-dependent | 1-100 Gbps |
| **Security** | Encrypted | Private (optionally encrypted) |
| **Performance** | Variable | Consistent |

---

## Management and Governance

### AWS CloudFormation

**Infrastructure as Code (IaC)**

Define and provision AWS infrastructure using template files.

**Key Concepts**:
- **Templates**: JSON or YAML files defining resources
- **Stacks**: Collection of AWS resources managed as a single unit
- **Change Sets**: Preview changes before applying

**Features**:
- **Automation**: Automate infrastructure provisioning
- **Version Control**: Track infrastructure changes over time
- **Consistency**: Ensure consistent deployments across environments
- **Reusability**: Reuse templates across accounts and Regions
- **Rollback**: Automatic rollback on errors
- **Cost**: No additional charge (pay for resources created)

**Use Cases**:
- Disaster recovery
- Environment replication (dev, test, prod)
- Compliance and governance
- Infrastructure version control

**Benefits**:
- Faster provisioning
- Reduced errors
- Consistent environments
- Easier management of complex architectures

### AWS CloudTrail

**Governance, compliance, and auditing**

Log and monitor all API activity in your AWS account.

**Key Features**:
- **API Logging**: Records all API calls in your account
- **Who, What, When, Where**: Comprehensive audit trail
- **Enabled by Default**: 90-day event history automatically
- **Long-term Storage**: Store logs in S3 for retention beyond 90 days
- **Integration**: CloudWatch Logs for monitoring and alerting

**Event Types**:
- **Management Events**: Control plane operations (create instance, modify security group)
- **Data Events**: Data plane operations (S3 object access, Lambda function invocations)
- **Insights Events**: Unusual API activity detection

**Use Cases**:
- Security analysis and compliance auditing
- Troubleshooting operational issues
- Change tracking
- Incident investigation
- Compliance requirements

**Benefits**:
- Complete visibility into account activity
- Simplified compliance auditing
- Security analysis and incident response
- Operational troubleshooting

### Amazon CloudWatch

**Monitoring and observability service**

Collect and track metrics, collect and monitor logs, set alarms, and automatically react to changes.

#### CloudWatch Components

**CloudWatch Metrics**
- Numerical time-series data points
- Built-in metrics for most AWS services (CPU, network, disk)
- Custom metrics for application-specific data
- Retention: Up to 15 months

**CloudWatch Logs**
- Collect and store log files from resources
- Monitor and troubleshoot systems and applications
- Log Groups and Log Streams organization
- Retention policies (never expire to 1 day)

**CloudWatch Alarms**
- Trigger actions based on metric thresholds
- States: OK, ALARM, INSUFFICIENT_DATA
- Actions: SNS notifications, Auto Scaling, EC2 actions
- Composite alarms for complex conditions

**CloudWatch Events / EventBridge**
- Event-driven automation
- Respond to state changes in AWS resources
- Schedule automated actions (cron jobs)
- Integration with Lambda, SQS, SNS, and more

**CloudWatch Dashboards**
- Customizable home pages for monitoring
- Visualize metrics and logs
- Share across teams

**Use Cases**:
- Application monitoring
- Performance optimization
- Troubleshooting
- Capacity planning
- Automated responses to operational changes

### AWS Systems Manager

**Operational hub for AWS resources**

Centralized operations management for AWS and on-premises resources.

**Key Capabilities**:

**Operations Management**
- **Session Manager**: Secure shell access without SSH keys or bastion hosts
- **Run Command**: Execute commands on multiple instances
- **Patch Manager**: Automate OS and application patching
- **Maintenance Windows**: Schedule operations tasks

**Configuration Management**
- **Parameter Store**: Centralized storage for configuration data and secrets
- **State Manager**: Maintain consistent configuration
- **Inventory**: Collect metadata from managed instances

**Insights**
- **OpsCenter**: Centralized operational issues management
- **CloudWatch Dashboard Integration**: Unified view

**Benefits**:
- Reduced operational overhead
- Improved security (no SSH keys needed)
- Centralized management
- Automated operations

**Common Use Cases**:
- Patch management across fleet
- Securely access instances
- Store application secrets
- Automate operational tasks

### AWS Trusted Advisor

**Best practice recommendations**

Real-time guidance to help provision resources following AWS best practices.

#### Five Pillars of Recommendations

**1. Cost Optimization**
- Identify unused or underutilized resources
- Recommendations to reduce costs
- Examples: Idle RDS instances, unattached EBS volumes, unused Elastic IPs

**2. Performance**
- Improve service performance
- Examples: High utilization of EC2 instances, EBS throughput optimization

**3. Security**
- Close security gaps
- Examples: S3 bucket permissions, security group rules, MFA on root account

**4. Fault Tolerance**
- Increase availability and redundancy
- Examples: Multi-AZ deployments, EBS snapshot age, Route 53 health checks

**5. Service Limits (Service Quotas)**
- Check usage against service limits
- Avoid service disruptions from hitting limits

#### Access Levels

**Free (All Customers)**
- 7 core checks:
  - S3 bucket permissions
  - Security groups - specific ports unrestricted
  - IAM use
  - MFA on root account
  - EBS public snapshots
  - RDS public snapshots
  - Service limits for common services

**Business or Enterprise Support**
- All checks (50+ checks)
- Automated notifications
- AWS Support API access
- Programmatic access

**Dashboard**:
- Color-coded status: Red (action recommended), Yellow (investigation recommended), Green (no problem)
- Downloadable reports

### AWS Control Tower

**Set up and govern multi-account AWS environment**

Automated setup and governance for multi-account AWS environments.

**Key Features**:
- **Landing Zone**: Automated multi-account setup based on best practices
- **Guardrails**: Governance rules for compliance
  - **Preventive**: Prevent policy violations (using SCPs)
  - **Detective**: Detect policy violations (using AWS Config)
- **Account Factory**: Automated account provisioning and configuration
- **Dashboard**: Centralized visibility and compliance monitoring

**Built On**:
- AWS Organizations
- AWS Service Catalog
- AWS CloudFormation
- AWS IAM Identity Center (SSO)
- AWS Config

**Use Cases**:
- Setting up new multi-account environments
- Governance for enterprise organizations
- Compliance requirements
- Account standardization

### AWS Service Catalog

**Create and manage catalogs of approved IT services**

Enable centralized management of commonly deployed IT services.

**Key Features**:
- **Product Portfolio**: Catalog of approved CloudFormation templates
- **Access Control**: Control who can deploy which services
- **Versioning**: Manage product versions
- **Constraints**: Define rules for product usage
- **Self-Service**: End users deploy approved resources

**Benefits**:
- Standardized deployments
- Centralized management
- Governance and compliance
- Cost control
- Faster time to market

**Use Cases**:
- Standardize infrastructure deployments
- Control cloud resource sprawl
- Enable self-service for developers
- Maintain compliance

---

## Additional Services

### Amazon SNS (Simple Notification Service)

**Pub/sub messaging service**

Fully managed messaging service for application-to-application (A2A) and application-to-person (A2P) communication.

**Key Concepts**:
- **Topics**: Communication channels
- **Publishers**: Send messages to topics
- **Subscribers**: Receive messages from topics
- **Message Filtering**: Subscribers receive only relevant messages

**Supported Protocols**:
- HTTP/HTTPS
- Email/Email-JSON
- SMS
- Amazon SQS
- AWS Lambda
- Mobile push notifications

**Use Cases**:
- Application alerts and notifications
- Push notifications to mobile devices
- Email and SMS messaging
- Fanout pattern (one message to many subscribers)
- Decoupled microservices

**Benefits**:
- Simple setup and operation
- High throughput
- Message durability and delivery
- Message filtering

### Amazon SQS (Simple Queue Service)

**Fully managed message queue service**

Decouple and scale microservices, distributed systems, and serverless applications.

**Queue Types**:

**Standard Queues**
- **Throughput**: Unlimited
- **Delivery**: At-least-once delivery (messages may be delivered multiple times)
- **Ordering**: Best-effort ordering
- **Use Case**: High throughput applications where occasional duplicates are acceptable

**FIFO Queues**
- **Throughput**: Up to 3,000 messages per second (higher with batching)
- **Delivery**: Exactly-once processing
- **Ordering**: Strict ordering guaranteed
- **Use Case**: When message order is critical

**Features**:
- **Retention**: Messages retained up to 14 days
- **Visibility Timeout**: Hide message while being processed
- **Dead-Letter Queues**: Handle failed messages
- **Delay Queues**: Postpone delivery of messages
- **Long Polling**: Reduce empty responses and costs

**Use Cases**:
- Decouple application components
- Buffer between producers and consumers
- Batch processing
- Asynchronous processing

**SNS vs SQS**:
- **SNS**: Push notifications, pub/sub, fanout
- **SQS**: Pull-based queue, decouple components, buffer
- **Together**: SNS sends to multiple SQS queues for fanout architecture

### AWS Step Functions

**Serverless workflow orchestration**

Coordinate multiple AWS services into serverless workflows.

**Key Features**:
- **Visual Workflows**: Graphical workflow designer
- **State Machines**: Define workflows as state machines
- **Service Integration**: Direct integration with AWS services
- **Error Handling**: Built-in retry and error handling
- **Standard and Express Workflows**:
  - **Standard**: Long-running (up to 1 year), exactly-once execution
  - **Express**: High-volume, short duration (up to 5 minutes), at-least-once execution

**Use Cases**:
- Orchestrate microservices
- Data processing pipelines
- Automate IT and security processes
- Build complex workflows from Lambda functions

**Benefits**:
- Visual workflow management
- Built-in error handling
- Serverless and scalable
- Audit trail of executions

### Amazon EventBridge

**Serverless event bus service**

Connect applications using events from AWS services, SaaS applications, and custom applications.

**Key Concepts**:
- **Events**: State changes or notifications
- **Event Buses**: Receive and route events
- **Rules**: Match events and route to targets
- **Targets**: Destinations for events (Lambda, SQS, SNS, etc.)

**Event Sources**:
- AWS services (EC2, S3, etc.)
- SaaS applications (Salesforce, Datadog, etc.)
- Custom applications

**Features**:
- **Schema Registry**: Discover and manage event schemas
- **Archive and Replay**: Store and replay events
- **Cross-Account Events**: Send events across AWS accounts
- **Filtering**: Advanced event pattern matching

**Previously**: CloudWatch Events (EventBridge is the enhanced version)

**Use Cases**:
- React to state changes in AWS services
- Schedule automated actions
- Integrate SaaS applications
- Event-driven architectures

### AWS Batch

**Fully managed batch processing**

Run batch computing workloads at any scale.

**Key Features**:
- **Automatic Provisioning**: Dynamically provisions compute resources
- **Job Queues**: Queue and prioritize jobs
- **Job Definitions**: Define how jobs are run
- **Scaling**: Automatically scales based on job volume
- **Integration**: Uses EC2 and Spot Instances

**Use Cases**:
- Data processing and analysis
- Image and video rendering
- Financial risk modeling
- Scientific simulations
- ETL (Extract, Transform, Load) jobs

**Benefits**:
- No infrastructure management
- Cost optimization with Spot Instances
- Automatic scaling
- Job dependency management

### Amazon Athena

**Interactive query service for S3**

Analyze data in Amazon S3 using standard SQL.

**Key Features**:
- **Serverless**: No infrastructure to manage
- **SQL Queries**: Standard SQL support
- **Pay Per Query**: Charged based on data scanned
- **Integration**: Works with AWS Glue Data Catalog
- **Formats**: Supports CSV, JSON, ORC, Avro, Parquet

**Use Cases**:
- Ad-hoc data analysis
- Log analysis
- Business intelligence
- Query CloudTrail logs
- Analyze application logs

**Benefits**:
- No ETL required
- Fast query performance
- Cost-effective (pay only for queries)
- Easy to use (just SQL)

**Best Practices**:
- Use columnar formats (Parquet, ORC) for better performance
- Partition data to reduce scanned data
- Compress data to reduce costs

### AWS Glue

**Fully managed ETL (Extract, Transform, Load) service**

Prepare data for analytics and machine learning.

**Key Components**:

**AWS Glue Data Catalog**
- Centralized metadata repository
- Stores table definitions, schemas, and metadata
- Integration point for Athena, EMR, Redshift Spectrum

**AWS Glue Crawlers**
- Automatically discover and catalog data
- Infer schemas
- Update the Data Catalog

**AWS Glue ETL Jobs**
- Serverless ETL operations
- Generate code in Python or Scala
- Visual ETL editor
- Built-in transformations

**Use Cases**:
- Prepare data for analytics
- Data lake management
- Database migration
- Data pipeline automation

**Benefits**:
- Serverless (no infrastructure management)
- Automatic schema discovery
- Integrated with AWS analytics services
- Cost-effective

### Amazon QuickSight

**Business intelligence (BI) service**

Create visualizations, perform ad-hoc analysis, and get business insights.

**Key Features**:
- **Serverless**: No servers to manage
- **Visualizations**: Interactive dashboards and visualizations
- **ML Insights**: Automatic insights powered by machine learning
- **SPICE Engine**: In-memory calculation engine for fast performance
- **Sharing**: Share dashboards with users and groups
- **Embedded Analytics**: Embed in applications

**Data Sources**:
- AWS services (RDS, Aurora, Redshift, S3, Athena)
- On-premises databases
- SaaS applications (Salesforce, Jira)
- File uploads (Excel, CSV, JSON)

**Pricing**:
- Pay per session (users pay only when accessing dashboards)
- Author and Reader pricing tiers

**Use Cases**:
- Business intelligence and reporting
- Data visualization
- Embedded analytics
- Ad-hoc analysis

### Amazon Kinesis

**Real-time data streaming**

Collect, process, and analyze real-time streaming data.

#### Kinesis Services

**Kinesis Data Streams**
- Capture and store data streams in real-time
- Shards for parallel processing
- Retain data for 1-365 days
- Use Case: Custom real-time applications

**Kinesis Data Firehose**
- Load streaming data into data stores
- Destinations: S3, Redshift, Elasticsearch, Splunk, HTTP endpoints
- Near real-time (60 seconds minimum)
- Automatic scaling
- Use Case: Simple data ingestion pipeline

**Kinesis Data Analytics**
- Analyze streaming data with SQL or Apache Flink
- Real-time analytics and insights
- Use Case: Real-time dashboards, metrics, anomaly detection

**Kinesis Video Streams**
- Capture and store video streams
- Process and analyze video
- Use Case: Video analytics, machine learning on video

**Common Use Cases**:
- Real-time log and event data collection
- IoT device telemetry
- Clickstream analysis
- Real-time analytics
- Video processing and analysis

### Amazon SageMaker

**Build, train, and deploy machine learning models**

Fully managed service for the complete machine learning workflow.

**Key Capabilities**:

**Build**
- Integrated Jupyter notebooks
- Pre-built algorithms and frameworks
- Data labeling service (Ground Truth)
- Feature Store

**Train**
- One-click training
- Automatic model tuning (hyperparameter optimization)
- Distributed training
- Managed Spot training for cost savings

**Deploy**
- One-click deployment
- Real-time and batch inference
- Multi-model endpoints
- Auto-scaling

**Additional Features**:
- SageMaker Studio (ML IDE)
- Model monitoring and debugging
- MLOps capabilities
- Pre-built ML solutions

**Use Cases**:
- Predictive analytics
- Recommendation systems
- Fraud detection
- Image and text classification
- Time series forecasting

### Amazon Rekognition

**Image and video analysis**

Add image and video analysis to applications using deep learning.

**Capabilities**:
- **Object and Scene Detection**: Identify thousands of objects and scenes
- **Facial Analysis**: Detect faces and analyze attributes (age, gender, emotions)
- **Face Comparison**: Compare faces for verification
- **Face Recognition**: Identify known faces in images and videos
- **Celebrity Recognition**: Recognize thousands of celebrities
- **Text Detection**: Detect and extract text from images
- **Content Moderation**: Detect inappropriate content
- **Video Analysis**: Track people, objects, activities in videos

**Use Cases**:
- User verification
- Content moderation
- Searchable image library
- Sentiment analysis
- Security and surveillance
- Media analysis

### Amazon Comprehend

**Natural Language Processing (NLP) service**

Extract insights and relationships from text using machine learning.

**Capabilities**:
- **Sentiment Analysis**: Determine positive, negative, neutral, or mixed sentiment
- **Entity Recognition**: Identify people, places, brands, events, etc.
- **Key Phrase Extraction**: Extract important phrases from text
- **Language Detection**: Identify the dominant language
- **Topic Modeling**: Organize documents by topic
- **Custom Classification**: Train custom models for specific domains

**Use Cases**:
- Customer feedback analysis
- Document classification
- Social media monitoring
- Knowledge management
- Business intelligence from documents

### Amazon Lex

**Build conversational interfaces (chatbots)**

Build chatbots and voice assistants using the same technology as Amazon Alexa.

**Key Features**:
- **Automatic Speech Recognition (ASR)**: Convert speech to text
- **Natural Language Understanding (NLU)**: Understand intent
- **Multi-turn Conversations**: Context management across conversation
- **8 kHz Telephony Audio**: Support for phone calls
- **Integration**: Connect to AWS Lambda, mobile apps, messaging platforms

**Use Cases**:
- Customer service chatbots
- Virtual assistants
- Voice-enabled applications
- Interactive Voice Response (IVR) systems
- Information bots

**Components**:
- **Intents**: Actions users want to perform
- **Utterances**: Phrases users might say
- **Slots**: Parameters for intents
- **Fulfillment**: Lambda function to fulfill the intent

### AWS Migration Hub

**Track application migrations**

Centralized location to track progress of application migrations across multiple AWS and partner solutions.

**Key Features**:
- **Single Dashboard**: View migration status across tools
- **Migration Tracking**: Track server and database migrations
- **Integration**: Works with AWS migration tools and partner tools
- **Application Grouping**: Group servers by application

**Integrated Tools**:
- AWS Application Migration Service
- AWS Database Migration Service
- CloudEndure Migration
- ATADATA ATAmotion
- RiverMeadow Server Migration

**Benefits**:
- Centralized visibility
- Track progress across multiple tools
- Identify and troubleshoot issues
- Plan and execute migrations

### AWS Database Migration Service (DMS)

**Migrate databases to AWS**

Migrate databases to AWS quickly and securely while the source database remains operational.

**Key Features**:
- **Minimal Downtime**: Source database remains operational during migration
- **Continuous Data Replication**: Ongoing replication after initial migration
- **Homogeneous Migrations**: Same database engine (Oracle to Oracle)
- **Heterogeneous Migrations**: Different database engines (Oracle to Aurora)
- **Schema Conversion**: AWS Schema Conversion Tool (SCT) for heterogeneous migrations

**Supported Sources**:
- Oracle, SQL Server, MySQL, PostgreSQL, MongoDB, SAP, DB2
- On-premises and cloud databases

**Supported Targets**:
- Amazon RDS, Aurora, Redshift, DynamoDB, S3, DocumentDB

**Use Cases**:
- Migrate to cloud
- Replicate for development/test
- Database consolidation
- Continuous data replication

**Benefits**:
- Cost-effective
- Minimal downtime
- Supports most databases
- Reliable and self-healing

### AWS Application Discovery Service

**Discover on-premises applications for migration planning**

Collect information about on-premises data centers to plan migrations.

**Discovery Methods**:

**Agentless Discovery (Application Discovery Service Agentless Collector)**
- VMware environment only
- Collects VM inventory, configuration, performance
- No agent installation required

**Agent-based Discovery (Application Discovery Agent)**
- Install agent on servers
- Collects system configuration, performance, running processes, network connections
- Works on physical and virtual servers

**Data Collected**:
- Server specifications (CPU, memory, disk)
- Resource utilization
- Network dependencies
- Running processes and applications

**Integration**:
- Data exported to Amazon S3
- Integrate with AWS Migration Hub
- Visualize dependencies with AWS Application Discovery Service

**Use Cases**:
- Migration planning
- Identify dependencies between applications
- Right-size target infrastructure
- Total cost of ownership (TCO) analysis

---

## Review Questions

Test your knowledge of AWS Cloud Technology and Services:

### Question 1
**Which AWS service provides a serverless compute platform that automatically scales and charges based on the number of requests and compute time?**

A) Amazon EC2
B) AWS Lambda
C) Amazon Lightsail
D) AWS Elastic Beanstalk

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS Lambda**

Lambda is serverless, automatically scales, and charges based on requests and compute time. EC2 requires instance management, Lightsail has predictable pricing, and Elastic Beanstalk is a PaaS that still provisions underlying resources.
</details>

---

### Question 2
**Your company needs to store frequently accessed files that must be available immediately. Which S3 storage class would be the MOST cost-effective?**

A) S3 Glacier Deep Archive
B) S3 Standard-IA
C) S3 Standard
D) S3 One Zone-IA

<details>
<summary>Click to reveal answer</summary>

**Answer: C) S3 Standard**

For frequently accessed data requiring immediate availability, S3 Standard is the appropriate choice. Glacier Deep Archive has long retrieval times, and IA (Infrequent Access) classes charge retrieval fees that would add up with frequent access.
</details>

---

### Question 3
**Which EC2 pricing model would be MOST appropriate for a production database that must run continuously for the next three years?**

A) On-Demand Instances
B) Spot Instances
C) Reserved Instances
D) Dedicated Hosts

<details>
<summary>Click to reveal answer</summary>

**Answer: C) Reserved Instances**

For steady-state workloads running continuously, Reserved Instances provide the best cost savings (up to 75%). On-Demand is most expensive, Spot Instances can be interrupted (unsuitable for databases), and Dedicated Hosts are for specific compliance/licensing needs.
</details>

---

### Question 4
**What is the PRIMARY difference between Security Groups and Network ACLs?**

A) Security Groups are stateful, Network ACLs are stateless
B) Security Groups apply to subnets, Network ACLs apply to instances
C) Security Groups allow deny rules, Network ACLs only allow rules
D) Security Groups are free, Network ACLs have additional costs

<details>
<summary>Click to reveal answer</summary>

**Answer: A) Security Groups are stateful, Network ACLs are stateless**

Security Groups are stateful (return traffic automatically allowed), while Network ACLs are stateless (must explicitly allow return traffic). Security Groups apply to instances, NACLs apply to subnets. Security Groups only have allow rules, NACLs have both allow and deny rules. Both are free.
</details>

---

### Question 5
**Which database service would be BEST for a social networking application that needs to efficiently query relationships between users?**

A) Amazon RDS
B) Amazon DynamoDB
C) Amazon Neptune
D) Amazon Redshift

<details>
<summary>Click to reveal answer</summary>

**Answer: C) Amazon Neptune**

Neptune is a graph database designed for highly connected data and relationship queries (social networks, recommendation engines). RDS is for relational data, DynamoDB for key-value data, and Redshift for analytics/data warehousing.
</details>

---

### Question 6
**Your application needs to send notifications to multiple subscribers via email, SMS, and HTTP endpoints. Which service should you use?**

A) Amazon SQS
B) Amazon SNS
C) AWS Step Functions
D) Amazon EventBridge

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Amazon SNS**

SNS is a pub/sub messaging service that can send notifications to multiple subscribers across multiple protocols (email, SMS, HTTP). SQS is a message queue, Step Functions orchestrates workflows, and EventBridge is for event-driven architectures.
</details>

---

### Question 7
**Which AWS service allows you to define infrastructure as code using JSON or YAML templates?**

A) AWS CloudTrail
B) AWS CloudFormation
C) AWS Config
D) AWS Systems Manager

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS CloudFormation**

CloudFormation uses templates (JSON/YAML) to define infrastructure as code. CloudTrail logs API calls, Config tracks resource configurations, and Systems Manager manages operations.
</details>

---

### Question 8
**What is the MINIMUM number of Availability Zones in an AWS Region?**

A) 1
B) 2
C) 3
D) 4

<details>
<summary>Click to reveal answer</summary>

**Answer: C) 3**

All AWS Regions have a minimum of 3 Availability Zones to provide high availability and fault tolerance.
</details>

---

### Question 9
**Which AWS service provides a private, dedicated network connection from your on-premises data center to AWS?**

A) AWS VPN
B) AWS Direct Connect
C) Amazon CloudFront
D) AWS Transit Gateway

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS Direct Connect**

Direct Connect provides a dedicated private network connection. VPN uses encrypted connection over the internet, CloudFront is a CDN, and Transit Gateway connects VPCs and on-premises networks.
</details>

---

### Question 10
**Your company needs to migrate 100 TB of data to AWS. Network bandwidth is limited. Which service should you use?**

A) AWS DataSync
B) AWS Snowball
C) Amazon S3 Transfer Acceleration
D) AWS Direct Connect

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS Snowball**

For large data migrations with limited bandwidth, Snowball Edge (80 TB capacity) is ideal. You would use 2 devices for 100 TB. DataSync is for ongoing transfers, Transfer Acceleration speeds up S3 uploads over internet, and Direct Connect takes weeks to provision.
</details>

---

### Question 11
**Which service would you use to analyze data in S3 using standard SQL without moving the data?**

A) Amazon Redshift
B) Amazon Athena
C) AWS Glue
D) Amazon EMR

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Amazon Athena**

Athena allows you to query data directly in S3 using SQL without loading data into a database. Redshift is a data warehouse (requires loading data), Glue is for ETL, and EMR is for big data processing frameworks.
</details>

---

### Question 12
**What AWS service provides real-time guidance to help you provision resources following AWS best practices?**

A) AWS CloudTrail
B) AWS Trusted Advisor
C) AWS Inspector
D) AWS Config

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS Trusted Advisor**

Trusted Advisor provides real-time best practice recommendations across 5 categories. CloudTrail logs API calls, Inspector assesses application security, and Config tracks resource configurations.
</details>

---

### Question 13
**Which load balancer type operates at Layer 7 and can route traffic based on URL path?**

A) Classic Load Balancer
B) Network Load Balancer
C) Application Load Balancer
D) Gateway Load Balancer

<details>
<summary>Click to reveal answer</summary>

**Answer: C) Application Load Balancer**

ALB operates at Layer 7 (application layer) and supports advanced routing including path-based and host-based routing. NLB operates at Layer 4, Gateway Load Balancer at Layer 3, and Classic Load Balancer is being phased out.
</details>

---

### Question 14
**Which AWS service would you use to coordinate multiple AWS services into a serverless workflow?**

A) Amazon SQS
B) Amazon SNS
C) AWS Step Functions
D) AWS Lambda

<details>
<summary>Click to reveal answer</summary>

**Answer: C) AWS Step Functions**

Step Functions orchestrates multiple AWS services into serverless workflows with visual workflow design. SQS is a message queue, SNS is pub/sub messaging, and Lambda executes individual functions.
</details>

---

### Question 15
**Your application requires block storage for an EC2 instance. Which service should you use?**

A) Amazon S3
B) Amazon EBS
C) Amazon EFS
D) AWS Storage Gateway

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Amazon EBS**

EBS provides block storage for EC2 instances. S3 is object storage, EFS is file storage for multiple instances, and Storage Gateway is for hybrid cloud storage.
</details>

---

### Question 16
**Which DynamoDB capacity mode should you choose for unpredictable, variable workloads?**

A) Provisioned Capacity
B) Reserved Capacity
C) On-Demand
D) Spot Capacity

<details>
<summary>Click to reveal answer</summary>

**Answer: C) On-Demand**

On-Demand capacity mode is ideal for unpredictable workloads as you pay per request. Provisioned Capacity requires setting read/write capacity units in advance. Reserved and Spot Capacity don't exist for DynamoDB.
</details>

---

### Question 17
**What is the purpose of AWS CloudFront Edge Locations?**

A) Run EC2 instances closer to users
B) Cache content closer to users for faster delivery
C) Store backups in multiple locations
D) Host databases in multiple regions

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Cache content closer to users for faster delivery**

Edge Locations are part of CloudFront CDN and cache content to reduce latency. They don't run EC2 instances, store backups, or host databases.
</details>

---

### Question 18
**Which AWS service logs all API calls made in your AWS account for auditing and compliance?**

A) Amazon CloudWatch
B) AWS CloudTrail
C) AWS Config
D) AWS Inspector

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS CloudTrail**

CloudTrail logs all API calls for governance, compliance, and auditing. CloudWatch monitors metrics and logs, Config tracks resource configurations, and Inspector assesses security vulnerabilities.
</details>

---

### Question 19
**Which storage service is best for shared file storage accessible by multiple EC2 instances simultaneously?**

A) Amazon EBS
B) Amazon S3
C) Amazon EFS
D) Instance Store

<details>
<summary>Click to reveal answer</summary>

**Answer: C) Amazon EFS**

EFS provides shared NFS file system accessible by multiple EC2 instances simultaneously. EBS attaches to single instance, S3 is object storage (not file system), and Instance Store is ephemeral.
</details>

---

### Question 20
**Your company needs to run Docker containers without managing the underlying infrastructure. Which service combination is MOST appropriate?**

A) Amazon ECS with EC2 launch type
B) Amazon ECS with Fargate launch type
C) Amazon EKS with EC2 nodes
D) Amazon EC2 with Docker installed

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Amazon ECS with Fargate launch type**

ECS with Fargate is serverless - you don't manage any infrastructure. ECS with EC2 and EKS with EC2 require managing EC2 instances. Running Docker on EC2 requires full infrastructure management.
</details>

---

### Question 21
**A company needs to process large amounts of data for scientific simulations. The workload runs for 8 hours per day with predictable schedules. Which EC2 pricing model provides the BEST cost optimization?**

A) On-Demand Instances with Auto Scaling
B) Spot Instances
C) Reserved Instances with Scheduled Reserved Instances
D) Dedicated Hosts

<details>
<summary>Click to reveal answer</summary>

**Answer: C) Reserved Instances with Scheduled Reserved Instances**

Scheduled Reserved Instances allow you to reserve capacity for predictable recurring schedules (daily, weekly, monthly). Since the workload runs predictably for 8 hours/day, this provides significant savings compared to On-Demand while ensuring capacity availability. Spot Instances could be interrupted, and Dedicated Hosts are unnecessarily expensive for this use case.
</details>

---

### Question 22
**Which storage service provides the LOWEST cost for storing 200 TB of data that must be retained for 10 years for compliance but will never be accessed unless required by auditors?**

A) S3 Standard
B) S3 Glacier Flexible Retrieval
C) S3 Glacier Deep Archive
D) S3 One Zone-IA

<details>
<summary>Click to reveal answer</summary>

**Answer: C) S3 Glacier Deep Archive**

Glacier Deep Archive offers the lowest storage cost ($0.00099 per GB/month) and is specifically designed for long-term archival storage with retrieval times of 12-48 hours. The 200 TB would cost approximately $200/month compared to $4,600/month in S3 Standard. The long retrieval time is acceptable for compliance data rarely accessed.
</details>

---

### Question 23
**Your application needs to make millions of cache lookups per second with sub-millisecond latency. Which service should you use?**

A) Amazon RDS with read replicas
B) Amazon DynamoDB with DAX
C) Amazon ElastiCache
D) Amazon Aurora

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Amazon DynamoDB with DAX**

DynamoDB Accelerator (DAX) provides microsecond latency for millions of requests per second, making it ideal for extreme performance requirements. ElastiCache could also work, but DAX is specifically designed for DynamoDB and provides the best integration. RDS and Aurora have higher latency (milliseconds).
</details>

---

### Question 24
**A company wants to analyze customer behavior by querying relationships like "customers who bought this also bought that." Which database is MOST appropriate?**

A) Amazon RDS
B) Amazon DynamoDB
C) Amazon Neptune
D) Amazon Redshift

<details>
<summary>Click to reveal answer</summary>

**Answer: C) Amazon Neptune**

Neptune is a graph database designed for querying highly connected data and relationships. It excels at queries like friend-of-friend, recommendations, and relationship traversals. RDS and DynamoDB would require complex joins or denormalization, and Redshift is for analytics, not real-time relationship queries.
</details>

---

### Question 25
**Which combination provides the MOST cost-effective solution for a serverless web application with unpredictable traffic?**

A) EC2 Auto Scaling + RDS
B) Lambda + DynamoDB On-Demand + S3
C) ECS Fargate + Aurora Serverless
D) Lightsail + RDS

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Lambda + DynamoDB On-Demand + S3**

This combination provides true serverless scaling with pay-per-use pricing. Lambda charges only for actual executions, DynamoDB On-Demand charges per request, and S3 charges for storage and requests. This is ideal for unpredictable traffic with zero waste during low-traffic periods. Option C is also serverless but more expensive.
</details>

---

### Question 26
**Your company needs to ensure that EC2 instances in a private subnet can download software updates from the internet without being directly accessible from the internet. What should you configure?**

A) Internet Gateway
B) NAT Gateway
C) Virtual Private Gateway
D) VPC Peering

<details>
<summary>Click to reveal answer</summary>

**Answer: B) NAT Gateway**

NAT Gateway enables instances in private subnets to initiate outbound connections to the internet while preventing inbound connections from the internet. An Internet Gateway would require instances to be in public subnets with public IPs, making them directly accessible.
</details>

---

### Question 27
**Which AWS service automatically distributes incoming application traffic across multiple targets and can route based on URL path?**

A) Network Load Balancer
B) Application Load Balancer
C) Classic Load Balancer
D) CloudFront

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Application Load Balancer**

ALB operates at Layer 7 and supports content-based routing including path-based, host-based, and HTTP header-based routing. NLB operates at Layer 4 and doesn't inspect application-layer content. CloudFront is a CDN, not a load balancer.
</details>

---

### Question 28
**A company needs to migrate 500 TB of data to AWS but has limited network bandwidth (10 Mbps). What is the MOST efficient migration method?**

A) AWS DataSync over internet
B) AWS Direct Connect
C) AWS Snowball Edge devices
D) S3 Transfer Acceleration

<details>
<summary>Click to reveal answer</summary>

**Answer: C) AWS Snowball Edge devices**

At 10 Mbps, transferring 500 TB would take approximately 463 days. Snowball Edge devices (80 TB capacity each) can transfer this data in weeks. You would order 7 devices, copy data locally, and ship them to AWS. Direct Connect takes weeks to provision, and DataSync/Transfer Acceleration would still be limited by bandwidth.
</details>

---

### Question 29
**Which RDS feature provides automatic failover to a standby instance in a different Availability Zone?**

A) Read Replicas
B) Multi-AZ deployment
C) Automated Backups
D) Manual Snapshots

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Multi-AZ deployment**

Multi-AZ provides synchronous replication to a standby instance and automatic failover (typically under 1 minute). Read Replicas use asynchronous replication and require manual promotion. Backups and snapshots don't provide automatic failover.
</details>

---

### Question 30
**Your application needs to store session data that expires after 24 hours. Which service combination is MOST cost-effective?**

A) RDS with custom cleanup scripts
B) DynamoDB with Time-To-Live (TTL)
C) S3 with Lifecycle policies
D) ElastiCache with manual deletion

<details>
<summary>Click to reveal answer</summary>

**Answer: B) DynamoDB with Time-To-Live (TTL)**

DynamoDB TTL automatically deletes expired items at no additional cost, making it ideal for session data. ElastiCache could work but requires configuration and memory management. RDS and S3 are not optimized for high-frequency session data with automatic expiration.
</details>

---

### Question 31
**Which service would you use to create a visual workflow that coordinates multiple Lambda functions with error handling and retry logic?**

A) Amazon SQS
B) AWS Step Functions
C) Amazon EventBridge
D) AWS Batch

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS Step Functions**

Step Functions provides visual workflow orchestration with built-in error handling, retries, and state management. It's specifically designed to coordinate multiple AWS services including Lambda. SQS is just a message queue, EventBridge routes events, and Batch is for batch computing.
</details>

---

### Question 32
**A company wants to analyze application logs stored in S3 using SQL without loading data into a database. Which service should they use?**

A) Amazon Redshift
B) Amazon Athena
C) Amazon EMR
D) AWS Glue

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Amazon Athena**

Athena allows direct SQL queries against data in S3 without loading or transformation. You pay only for queries run ($5 per TB scanned). Redshift requires loading data, EMR requires cluster management, and Glue is for ETL (though it works well with Athena).
</details>

---

### Question 33
**Which AWS service provides recommendations for cost optimization, security, performance, and fault tolerance?**

A) AWS CloudTrail
B) AWS Config
C) AWS Trusted Advisor
D) AWS Inspector

<details>
<summary>Click to reveal answer</summary>

**Answer: C) AWS Trusted Advisor**

Trusted Advisor provides real-time best practice recommendations across five categories: cost optimization, performance, security, fault tolerance, and service limits. CloudTrail logs API calls, Config tracks configurations, and Inspector assesses security vulnerabilities.
</details>

---

### Question 34
**Your application experiences a 10x traffic increase during a 2-hour window every day. Which compute solution provides automatic scaling with minimal management?**

A) EC2 with manual scaling
B) EC2 with Auto Scaling
C) Lambda with CloudWatch Events
D) Lightsail

<details>
<summary>Click to reveal answer</summary>

**Answer: C) Lambda with CloudWatch Events**

Lambda automatically scales from zero to thousands of concurrent executions with no configuration needed. While EC2 Auto Scaling works, it takes minutes to provision new instances. Lambda scales instantly and you pay only for the 2-hour peak period. CloudWatch Events can trigger scheduled scaling if needed.
</details>

---

### Question 35
**Which database service provides automatic scaling of both storage and compute capacity?**

A) Amazon RDS
B) Amazon Aurora Serverless
C) Amazon DynamoDB
D) Amazon Redshift

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Amazon Aurora Serverless**

Aurora Serverless automatically adjusts database capacity based on application needs, scaling both compute (Aurora Capacity Units) and storage. DynamoDB can auto-scale throughput but not in the same way. RDS requires manual instance resizing for compute, though storage can auto-scale. Redshift requires manual cluster resizing.
</details>

---

### Question 36
**A company needs to connect their VPC to an on-premises data center with a consistent, private connection at 10 Gbps. Which service should they use?**

A) Site-to-Site VPN
B) AWS Direct Connect
C) VPC Peering
D) Transit Gateway

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS Direct Connect**

Direct Connect provides dedicated private network connectivity at speeds from 1 Gbps to 100 Gbps with consistent performance. Site-to-Site VPN goes over the internet with variable performance, VPC Peering connects VPCs (not on-premises), and Transit Gateway connects networks but doesn't provide the physical connection.
</details>

---

### Question 37
**Which S3 storage class automatically moves objects between access tiers based on changing access patterns?**

A) S3 Standard
B) S3 Intelligent-Tiering
C) S3 Standard-IA
D) S3 Glacier

<details>
<summary>Click to reveal answer</summary>

**Answer: B) S3 Intelligent-Tiering**

Intelligent-Tiering automatically moves objects between Frequent Access and Infrequent Access tiers based on access patterns, optimizing costs without performance impact or operational overhead. Other storage classes require manual management or lifecycle policies.
</details>

---

### Question 38
**Your application needs shared file storage accessible by multiple EC2 instances across different Availability Zones. Which service should you use?**

A) Amazon EBS
B) Amazon EFS
C) Instance Store
D) Amazon S3

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Amazon EFS**

EFS provides shared NFS file storage accessible by multiple EC2 instances simultaneously across AZs. EBS can only attach to one instance at a time, Instance Store is ephemeral and instance-specific, and S3 is object storage (not a file system).
</details>

---

### Question 39
**Which service would you use to track WHO made WHAT change to AWS resources and WHEN?**

A) Amazon CloudWatch
B) AWS CloudTrail
C) AWS Config
D) AWS X-Ray

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS CloudTrail**

CloudTrail logs all API calls (who, what, when, where) for governance, compliance, and auditing. CloudWatch monitors metrics and logs, Config tracks resource configurations over time, and X-Ray traces application requests.
</details>

---

### Question 40
**A company needs to run Docker containers without managing EC2 instances. Which service combination should they use?**

A) ECS with EC2 launch type
B) ECS with Fargate launch type
C) EKS with EC2 nodes
D) EC2 with Docker installed

<details>
<summary>Click to reveal answer</summary>

**Answer: B) ECS with Fargate launch type**

ECS with Fargate is serverless container orchestration where AWS manages all infrastructure. You define containers and Fargate handles provisioning, scaling, and management. ECS with EC2 and EKS with EC2 require managing instances.
</details>

---

### Question 41
**Which database is optimized for storing and querying time-series data from IoT devices?**

A) Amazon RDS
B) Amazon DynamoDB
C) Amazon Timestream
D) Amazon Redshift

<details>
<summary>Click to reveal answer</summary>

**Answer: C) Amazon Timestream**

Timestream is purpose-built for time-series data with automatic data lifecycle management and built-in time-series analytics functions. While DynamoDB can store time-series data, Timestream is optimized for this use case with better performance and cost efficiency.
</details>

---

### Question 42
**Your application needs to send notifications to thousands of subscribers via multiple protocols (email, SMS, mobile push). Which service should you use?**

A) Amazon SQS
B) Amazon SNS
C) Amazon EventBridge
D) Amazon SES

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Amazon SNS**

SNS (Simple Notification Service) is a pub/sub messaging service that can send notifications to multiple subscribers across different protocols (email, SMS, HTTP, mobile push, SQS, Lambda). SQS is for queuing, EventBridge for event routing, and SES is only for email.
</details>

---

### Question 43
**Which service helps you discover and catalog data sources to prepare for migration to AWS?**

A) AWS Migration Hub
B) AWS Application Discovery Service
C) AWS Database Migration Service
D) AWS DataSync

<details>
<summary>Click to reveal answer</summary>

**Answer: B) AWS Application Discovery Service**

Application Discovery Service collects information about on-premises servers including configuration, performance, and dependencies to plan migrations. Migration Hub tracks progress, DMS migrates databases, and DataSync transfers data.
</details>

---

### Question 44
**A company needs to ensure their CloudFormation templates follow organizational standards and prevent non-compliant resources from being created. Which service should they use?**

A) AWS Config
B) AWS CloudTrail
C) AWS Control Tower
D) AWS Organizations

<details>
<summary>Click to reveal answer</summary>

**Answer: C) AWS Control Tower**

Control Tower provides guardrails (preventive and detective) to enforce compliance across accounts. Preventive guardrails use SCPs to prevent non-compliant actions. Config detects but doesn't prevent, CloudTrail logs actions, and Organizations provides account management but not guardrails.
</details>

---

### Question 45
**Which ElastiCache engine supports complex data structures like sorted sets, lists, and pub/sub messaging?**

A) Memcached
B) Redis
C) Both support these features equally
D) Neither supports these features

<details>
<summary>Click to reveal answer</summary>

**Answer: B) Redis**

Redis supports advanced data structures (strings, hashes, lists, sets, sorted sets), persistence, replication, and pub/sub messaging. Memcached is simpler and only supports simple key-value caching with multi-threading. For advanced use cases, Redis is the better choice.
</details>

---

## Summary

Domain 3: Cloud Technology and Services represents the largest portion (34%) of the AWS Certified Cloud Practitioner exam. This domain covers:

**Key Areas**:
- **Global Infrastructure**: Regions, AZs, Edge Locations, and specialized infrastructure
- **Compute**: From full control (EC2) to fully serverless (Lambda)
- **Storage**: Object (S3), block (EBS), file (EFS), and hybrid (Storage Gateway)
- **Databases**: Relational, NoSQL, caching, analytics, and specialized databases
- **Networking**: VPC, load balancing, content delivery, and connectivity
- **Management**: IaC, monitoring, logging, governance, and operations
- **Additional Services**: Messaging, analytics, ML/AI, and migration tools

**Study Tips**:
1. Understand the use cases for each service
2. Know when to choose one service over another
3. Remember pricing models, especially for EC2 and S3
4. Understand high availability and fault tolerance patterns
5. Know the differences between similar services (e.g., SNS vs SQS, RDS vs DynamoDB)

---

[← Previous: Security and Compliance](03-security-compliance.md) | [Next: Billing, Pricing, and Support →](05-billing-support.md)
