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

---

[← Previous: Hands-On Labs](07-hands-on-labs.md) | [Back to Main](README.md) | [Next: Exam Scenarios →](09-exam-scenarios.md)
