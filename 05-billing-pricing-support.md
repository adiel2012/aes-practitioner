# Domain 4: Billing, Pricing, and Support

[Previous: Technology and Services](./04-technology-services.md) | [Table of Contents](./README.md) | [Next: Practice Questions](./06-practice-questions.md)

---

## Table of Contents

- [AWS Pricing Fundamentals](#aws-pricing-fundamentals)
  - [Core Principles](#core-principles)
  - [AWS Free Tier](#aws-free-tier)
- [Pricing Models by Service Category](#pricing-models-by-service-category)
  - [Compute Pricing](#compute-pricing)
  - [Storage Pricing](#storage-pricing)
  - [Database Pricing](#database-pricing)
  - [Network Pricing](#network-pricing)
- [Cost Management Tools](#cost-management-tools)
  - [AWS Pricing Calculator](#aws-pricing-calculator)
  - [AWS Cost Explorer](#aws-cost-explorer)
  - [AWS Budgets](#aws-budgets)
  - [AWS Cost and Usage Report](#aws-cost-and-usage-report)
  - [AWS Cost Anomaly Detection](#aws-cost-anomaly-detection)
- [Consolidated Billing and AWS Organizations](#consolidated-billing-and-aws-organizations)
- [AWS Support Plans](#aws-support-plans)
  - [Support Plan Comparison](#support-plan-comparison)
  - [Additional Support Resources](#additional-support-resources)
- [Cost Optimization Strategies](#cost-optimization-strategies)
- [Review Questions](#review-questions)

---

## AWS Pricing Fundamentals

### Core Principles

AWS pricing is built on several foundational principles that differentiate cloud computing from traditional on-premises infrastructure:

1. **Pay-as-you-go**: Pay only for what you use
   - No upfront commitments required
   - Start and stop resources at any time
   - Only charged for actual consumption

2. **Pay less when you reserve**: Reserved capacity discounts
   - Commit to usage for 1 or 3 years
   - Receive significant discounts (up to 75%)
   - Available for EC2, RDS, ElastiCache, Redshift, and more

3. **Pay less with volume-based discounts**: Use more, pay less per unit
   - Tiered pricing automatically applies as usage increases
   - Data transfer and storage pricing decreases with volume
   - No negotiations required

4. **No upfront costs**: No capital expenditure
   - Trade capital expense (CAPEX) for variable expense (OPEX)
   - No infrastructure to purchase upfront
   - Start with zero investment

5. **No termination fees**: Stop anytime
   - No contracts or long-term commitments (unless you choose Reserved Instances)
   - Delete resources when no longer needed
   - Stop paying immediately

---

### AWS Free Tier

AWS offers three types of free tier offerings to help new customers get started and experiment with services:

#### 1. Always Free

Services that never expire and are available to all AWS customers:

- **DynamoDB**: 25 GB of storage
- **Lambda**: 1 million requests per month
- **SNS**: 1 million publishes
- **CloudWatch**: 10 custom metrics and alarms
- **AWS Free Tier dashboard**: Monitor usage

#### 2. 12 Months Free

Services available for 12 months starting from account creation date:

- **EC2**: 750 hours/month of t2.micro or t3.micro instances
- **S3**: 5 GB of standard storage
- **RDS**: 750 hours/month of db.t2.micro database instances
- **CloudFront**: 50 GB data transfer out
- **Elastic Load Balancing**: 750 hours per month

> **Note**: The 750 hours of EC2 is enough to run one t2.micro instance continuously for a full month.

#### 3. Trials

Short-term free trials for specific services:

- **SageMaker**: 2 months free
- **Inspector**: 90 days free
- **Lightsail**: 1 month free (first month)
- **Amazon Comprehend Medical**: Various trial periods

> **Important**: Always set up billing alerts when using Free Tier to avoid unexpected charges if you exceed limits.

---

## Pricing Models by Service Category

### Compute Pricing

#### Amazon EC2

- **Instance Hours**: Pay for running instances (charged per second with 60-second minimum)
- **Pricing varies by**:
  - Instance type (t2.micro, m5.large, etc.)
  - Region (us-east-1 vs. eu-west-1)
  - Operating system (Linux, Windows, RHEL)
  - Tenancy (Shared vs. Dedicated)
- **Additional charges**:
  - Data transfer out
  - EBS storage volumes
  - Elastic IP addresses (when not attached)

**EC2 Purchase Options**:

| Purchase Option | Description | Discount | Use Case |
|----------------|-------------|----------|----------|
| On-Demand | Pay by the second, no commitment | Baseline | Short-term, unpredictable workloads |
| Reserved Instances | 1 or 3 year commitment | Up to 75% | Steady-state, predictable workloads |
| Spot Instances | Bid on unused capacity | Up to 90% | Fault-tolerant, flexible workloads |
| Savings Plans | Commitment to consistent usage ($/hour) | Up to 72% | Flexible compute usage |
| Dedicated Hosts | Physical server dedicated to you | Varies | Compliance, licensing requirements |

#### AWS Lambda

- **Requests**: $0.20 per 1 million requests
- **Compute time**: Charged per GB-second
  - Duration calculated from code execution start to return/termination
  - Rounded up to nearest 1ms
- **Free tier**: 1 million requests/month (always free)
- **No charges** when code is not running

---

### Storage Pricing

#### Amazon S3

Pricing components:

1. **Storage**: Pay for GB/month stored
   - Varies by storage class (Standard, Infrequent Access, Glacier, etc.)
   - Standard: ~$0.023 per GB/month
   - Standard-IA: ~$0.0125 per GB/month
   - Glacier: ~$0.004 per GB/month

2. **Requests**:
   - PUT, COPY, POST, LIST requests: $0.005 per 1,000
   - GET, SELECT requests: $0.0004 per 1,000

3. **Data transfer**:
   - Transfer IN: Free
   - Transfer OUT to internet: Tiered pricing (first 10 TB/month at $0.09/GB)
   - Transfer to CloudFront: Free

4. **Management features**:
   - S3 Inventory, Analytics, Object Tagging

#### Amazon EBS

- **Provisioned storage**: Pay for capacity provisioned per GB/month
  - gp3: $0.08/GB-month
  - gp2: $0.10/GB-month
  - io2: $0.125/GB-month + IOPS charges
- **Snapshots**: Incremental backup storage per GB/month
- **Varies by volume type**: General Purpose (SSD), Provisioned IOPS (SSD), Throughput Optimized (HDD)

> **Key Difference**: EBS charges for provisioned capacity, not used capacity. A 100 GB volume costs the same whether you store 10 GB or 100 GB.

---

### Database Pricing

#### Amazon RDS

Pricing components:

1. **Instance hours**: Based on instance class (db.t2.micro, db.m5.large)
2. **Storage**: Per GB/month of provisioned storage
3. **Backup storage**: Automated backups beyond database size
4. **Data transfer**: Standard AWS data transfer rates
5. **Additional features**:
   - Multi-AZ deployment (doubles cost)
   - Read replicas (charged as separate instances)

#### Amazon DynamoDB

Two capacity modes:

1. **On-Demand**:
   - Pay per request
   - No capacity planning required
   - Good for unpredictable workloads
   - Write Request Units (WRU) and Read Request Units (RRU)

2. **Provisioned Capacity**:
   - Pay for provisioned read/write capacity units
   - Auto Scaling available
   - More cost-effective for predictable workloads
   - Reserve capacity for additional discounts

3. **Storage**: $0.25 per GB/month (first 25 GB free with Always Free tier)

---

### Network Pricing

Understanding data transfer costs is crucial for cost optimization:

- **Data transfer IN**: Generally **free** from the internet to AWS
- **Data transfer OUT to internet**: **Charged** with tiered pricing
  - First 10 TB/month: $0.09/GB
  - Next 40 TB/month: $0.085/GB
  - Over 150 TB/month: $0.05/GB
- **Data transfer between Regions**: **Charged** at inter-region rates
- **Data transfer within same Region**:
  - Between AZs: $0.01/GB in each direction
  - Within same AZ: Free (using private IPs)
- **CloudFront data transfer out**: Lower cost than direct from services
- **VPC Endpoints**: Reduce data transfer costs for S3 and DynamoDB

> **Cost Optimization Tip**: Use CloudFront CDN to cache content at edge locations, reducing data transfer costs from origin services.

---

## Cost Management Tools

### AWS Pricing Calculator

**Purpose**: Estimate monthly AWS costs before deploying infrastructure

**Features**:
- Configure service specifications and get price estimates
- Create cost estimates for complete solutions
- Share estimates with stakeholders via URL
- Compare different configurations and pricing models
- Export estimates to CSV or PDF
- **Free to use** - no AWS account required

**Use Cases**:
- Planning new workload deployments
- Comparing Reserved Instance vs. On-Demand pricing
- Estimating migration costs
- Budget planning and forecasting

**Access**: https://calculator.aws

---

### AWS Cost Explorer

**Purpose**: Visualize, understand, and manage AWS costs and usage over time

**Features**:
- View up to **12 months** of historical cost data
- Forecast future costs for up to **12 months**
- Filter and group costs by:
  - Service (EC2, S3, RDS, etc.)
  - Linked account
  - Region
  - Tag
  - Instance type
  - Usage type
- Identify cost trends and anomalies
- **Default reports** (Monthly costs, Daily costs, etc.)
- **Custom reports** (save and reuse)
- Recommendations for Reserved Instances and Savings Plans

**Pricing**:
- UI access: **Free**
- API access: $0.01 per request

**Best Practices**:
- Review costs weekly or monthly
- Set up custom reports for specific projects/teams
- Use cost allocation tags for granular tracking

---

### AWS Budgets

**Purpose**: Set custom cost and usage budgets with automated alerts

**Features**:
- Create budgets for:
  - **Cost budgets**: Track spending against a budget
  - **Usage budgets**: Track usage amounts (EC2 hours, S3 GB)
  - **Reservation budgets**: Monitor RI/Savings Plans utilization
  - **Savings Plans budgets**: Track Savings Plans coverage
- Alert when exceeding (or forecasted to exceed) thresholds
- Notifications via:
  - Email (SNS)
  - Amazon Chatbot (Slack/Chime)
- Set multiple alert thresholds (50%, 80%, 100%)
- Budget actions: Automated responses (stop instances, etc.)

**Pricing**:
- **First 2 budgets**: Free
- **Additional budgets**: $0.02/day per budget (~$0.60/month)

**Example Use Case**:
```
Budget Name: Development Team Monthly Budget
Budget Amount: $1,000/month
Alerts:
  - 80% threshold → Email team lead
  - 100% threshold → Email team lead + manager
  - 120% threshold → Trigger Lambda to stop non-production instances
```

---

### AWS Cost and Usage Report

**Purpose**: Most comprehensive and detailed cost and usage data available

**Features**:
- Line-item detail for all AWS costs
- Detailed breakdown of usage and costs by:
  - Service
  - Operation
  - Resource
  - Tag
  - Hour/Day/Month
- Delivered to **S3 bucket** (CSV or Parquet format)
- Integrate with analytics tools:
  - Amazon Athena (query with SQL)
  - Amazon Redshift (data warehousing)
  - Amazon QuickSight (visualization)
- Update frequency: Hourly, daily, or monthly
- Include resource IDs and tags

**Pricing**: **Free** (only pay for S3 storage)

**Use Cases**:
- Deep-dive cost analysis
- Chargeback/showback reporting
- Custom billing reports
- Financial analysis and auditing

---

### AWS Cost Anomaly Detection

**Purpose**: Detect unusual spending patterns using machine learning

**Features**:
- **Machine learning** automatically identifies anomalies
- Root cause analysis for detected anomalies
- Alert via email or SNS when anomalies detected
- Configurable detection sensitivity
- Monitor specific services, accounts, or cost allocation tags
- No manual threshold configuration needed

**Pricing**: **No additional cost**

**How it works**:
1. Analyzes historical spending patterns
2. Identifies unusual spikes or changes
3. Sends alerts with details and root cause
4. Provides recommendations

**Example**: Detects when EC2 costs increase 200% due to accidentally launching large instances

---

## Consolidated Billing and AWS Organizations

### Consolidated Billing

**What is it**: A feature of AWS Organizations that combines billing across multiple AWS accounts

**Benefits**:

1. **Single bill**: One payment method for all accounts in the organization
2. **Volume discounts**: Combined usage across all accounts for tiered pricing
   - If Account A uses 8 TB of S3 storage and Account B uses 4 TB, you get pricing for 12 TB total
3. **Easy tracking**: Track charges per account while paying centrally
4. **Free tier sharing**: Free tier applies once per organization (not per account)
5. **Reserved Instance sharing**: RIs can be shared across accounts
6. **No additional cost**: Free feature of AWS Organizations

**Account Structure**:
```
Management Account (Payer)
├── Production Account
├── Development Account
├── Testing Account
└── Security Account
```

**Use Cases**:
- Large organizations with multiple departments
- Separate environments (prod, dev, test)
- Cost allocation by team or project
- Centralized billing management

---

## AWS Support Plans

AWS offers four support plans, each providing different levels of technical support and response times.

### Support Plan Comparison

| Feature | Basic | Developer | Business | Enterprise |
|---------|-------|-----------|----------|------------|
| **Cost** | Free | $29/month or 3% of monthly AWS usage (whichever is greater) | $100/month or 10% (tiered 10%-3%) | $15,000/month or 10% (tiered 10%-3%) |
| **Use Case** | All customers | Testing and development | Production workloads | Mission-critical workloads |
| **Technical Support** | None | Business hours via email | 24/7 via email, chat, phone | 24/7 via email, chat, phone |
| **Response Time - General Guidance** | N/A | < 24 hours | < 24 hours | < 24 hours |
| **Response Time - System Impaired** | N/A | < 12 hours | < 12 hours | < 12 hours |
| **Response Time - Production System Down** | N/A | N/A | < 4 hours | < 4 hours |
| **Response Time - Business-Critical Down** | N/A | N/A | < 1 hour | < 1 hour |
| **Response Time - Mission-Critical Down** | N/A | N/A | N/A | **< 15 minutes** |
| **Who Can Open Cases** | N/A | 1 primary contact | **Unlimited contacts** | **Unlimited contacts** |
| **Trusted Advisor Checks** | 7 core checks | 7 core checks | **All checks** | **All checks** |
| **Third-Party Software Support** | No | No | **Yes** | **Yes** |
| **Architectural Guidance** | No | General | Contextual to use case | **Consultative** |
| **Technical Account Manager (TAM)** | No | No | No | **Yes** |
| **Proactive Programs** | No | No | No | **Yes** (IEM, Well-Architected Reviews) |
| **Concierge Support Team** | No | No | No | **Yes** (billing/account) |

### Response Time Summary

> **Critical for Exam**: Memorize these response times!

**Developer**:
- General guidance: < 24 hours
- System impaired: < 12 hours

**Business**:
- General guidance: < 24 hours
- System impaired: < 12 hours
- Production system down: **< 4 hours**
- Business-critical down: **< 1 hour**

**Enterprise**:
- All Business plan response times, PLUS:
- Mission-critical system down: **< 15 minutes**

### Key Differences

**Basic Support** (Free):
- Access to:
  - Customer Service (billing and account questions)
  - AWS documentation, whitepapers, support forums
  - AWS Trusted Advisor (7 core checks)
  - AWS Personal Health Dashboard
- **No technical support**

**Developer Support** ($29/month minimum):
- For experimentation and testing
- **One** primary contact can open support cases
- Business hours email access
- General architectural guidance

**Business Support** ($100/month minimum):
- For production workloads
- **Unlimited** contacts can open cases
- **24/7 phone, email, and chat support**
- Full Trusted Advisor checks
- Third-party software support (interactions with AWS services)
- Contextual architectural guidance

**Enterprise Support** ($15,000/month minimum):
- For mission-critical workloads
- All Business features, PLUS:
- **Technical Account Manager (TAM)**: Designated technical point of contact
- **Concierge Support Team**: Billing and account experts
- **Infrastructure Event Management**: Support for product launches, events
- **Well-Architected Reviews**: Architectural guidance
- **15-minute response time** for mission-critical issues

---

### Additional Support Resources

#### AWS Personal Health Dashboard

- **Personalized** view of AWS service health affecting your resources
- **Proactive notifications** about scheduled maintenance, security issues
- **Alerts** for events impacting your resources
- Detailed **remediation guidance**
- **Available to all customers** (all support plans)
- Integrated with CloudWatch Events for automation

**Difference from Service Health Dashboard**:
- **Service Health Dashboard**: General AWS service status (all customers see the same view)
- **Personal Health Dashboard**: Customized to YOUR resources and accounts

#### AWS Health API

- **Programmatic access** to AWS Health information
- Integrate health events with monitoring and incident management systems
- Requires **Business or Enterprise Support**
- Automate responses to health events (Lambda triggers, etc.)

#### AWS Managed Services (AMS)

- AWS **operates your infrastructure** on your behalf
- Features:
  - 24/7 infrastructure operations
  - Incident detection and management
  - Patching, backup, monitoring
  - Security and compliance
  - Change management
- **Separate service** with additional cost
- Ideal for organizations wanting AWS to handle operations

#### AWS Professional Services

- **Global team of AWS experts**
- Services:
  - Help design and architect solutions
  - Build, migrate, and modernize applications
  - Work alongside your team
  - Training and knowledge transfer
- **Consulting engagement** (separate fees)
- Specialized teams: Migration, DevOps, Analytics, Machine Learning, etc.

#### AWS Partner Network (APN)

- **Global community** of AWS partners
- **Consulting Partners**: Professional services, system integration
- **Technology Partners**: Software solutions integrated with AWS
- **AWS Marketplace**: Purchase third-party software and services
- Find partners at: https://partners.amazonaws.com

---

## Cost Optimization Strategies

### 1. Right Sizing

**What is it**: Matching instance types and sizes to workload requirements

**How to implement**:
- Use **CloudWatch metrics** to identify underutilized resources
- Use **AWS Compute Optimizer** for ML-powered recommendations
- Analyze CPU, memory, network, and disk utilization
- Downsize or change instance families based on actual usage
- Review regularly (monthly or quarterly)

**Example**:
- Running an m5.2xlarge instance with 10% CPU usage
- Right-size to m5.large → Save ~50% on compute costs

**Tools**:
- AWS Compute Optimizer
- AWS Cost Explorer Right Sizing Recommendations
- CloudWatch metrics and alarms

---

### 2. Reserved Capacity

**Services with Reserved options**:
- **Reserved Instances**: EC2, RDS, ElastiCache, Redshift, Elasticsearch
- **Savings Plans**: EC2, Fargate, Lambda (Compute Savings Plans)

**Savings**: Up to **75%** compared to On-Demand pricing

**Commitment Terms**:
- 1 year or 3 years
- Payment options:
  - All Upfront (highest discount)
  - Partial Upfront (medium discount)
  - No Upfront (lowest discount, monthly payments)

**Best Practices**:
- Analyze usage patterns for 1-3 months before purchasing
- Start with 1-year commitments
- Use Reserved Instances for steady-state workloads
- Consider Savings Plans for flexibility across instance families

**Example**:
- Baseline workload: 10 m5.large instances running 24/7
- Purchase 10 Reserved Instances (3-year, All Upfront)
- Save ~72% compared to On-Demand

---

### 3. Spot Instances

**Discount**: Up to **90%** compared to On-Demand

**How it works**:
- Bid on unused EC2 capacity
- AWS can reclaim instances with 2-minute warning
- Price varies based on supply and demand

**Ideal for**:
- Fault-tolerant applications
- Flexible start/end times
- Batch processing jobs
- Big data analytics
- Containerized workloads (with auto-restart)
- CI/CD pipeline workers
- Rendering and transcoding

**NOT suitable for**:
- Databases (without proper architecture)
- Stateful applications (without checkpointing)
- Applications requiring guaranteed availability

**Best Practices**:
- Use **Spot Fleet** to launch multiple instance types/AZs
- Implement **checkpointing** to save progress
- Use **Spot Instance interruption notices** (2-minute warning)

---

### 4. Auto Scaling

**Benefits**:
- Scale resources based on actual demand
- Avoid over-provisioning
- Reduce costs during low-demand periods
- Maintain performance during high-demand

**Services with Auto Scaling**:
- EC2 Auto Scaling
- DynamoDB Auto Scaling
- Aurora Auto Scaling
- ECS/EKS Auto Scaling
- Application Auto Scaling (Lambda, etc.)

**Example**:
- Web application with variable traffic
- Scale from 2 instances (off-peak) to 10 instances (peak)
- Average 4 instances instead of always running 10
- Save ~60% on compute costs

---

### 5. Storage Optimization

**S3 Storage Classes**:

| Storage Class | Use Case | Cost (relative) |
|--------------|----------|-----------------|
| S3 Standard | Frequently accessed | Baseline ($$$) |
| S3 Intelligent-Tiering | Unknown or changing access | Automatic optimization |
| S3 Standard-IA | Infrequent access | ~50% cheaper ($$) |
| S3 One Zone-IA | Infrequent, non-critical | ~60% cheaper ($) |
| S3 Glacier Instant Retrieval | Archive, millisecond retrieval | ~70% cheaper ($) |
| S3 Glacier Flexible Retrieval | Archive, minute-hour retrieval | ~80% cheaper ($) |
| S3 Glacier Deep Archive | Long-term archive, 12-hour retrieval | ~90% cheaper ($) |

**Optimization Strategies**:
- Implement **S3 Lifecycle Policies** to transition objects automatically
- Use **S3 Intelligent-Tiering** for unpredictable access patterns
- Delete **unused EBS volumes** and **snapshots**
- Use **EBS gp3** instead of gp2 (20% cheaper with better performance)
- Enable **EBS snapshot archival** for long-term backups

**Example Lifecycle Policy**:
```
Day 0-30: S3 Standard
Day 31-90: S3 Standard-IA
Day 91-365: S3 Glacier Flexible Retrieval
Day 365+: Delete or move to Glacier Deep Archive
```

---

### 6. Data Transfer Optimization

**Strategies**:

1. **Use CloudFront** for content delivery
   - Cache content at edge locations
   - Reduce data transfer from origin
   - Lower data transfer pricing than direct from S3/EC2

2. **Keep data in same Region** when possible
   - Avoid cross-region data transfer charges
   - Use multi-AZ for high availability (minimal cost)

3. **Use VPC Endpoints** for S3 and DynamoDB
   - Traffic stays within AWS network
   - No data transfer charges
   - No need for Internet Gateway

4. **Compress data** before transfer
   - Reduce amount of data transferred
   - Use gzip, Brotli, or other compression

5. **Use AWS Direct Connect** for large data transfers
   - Dedicated network connection to AWS
   - Lower data transfer costs than internet
   - More consistent performance

**Cost Comparison**:
```
Scenario: Transfer 1 TB/month from EC2 to internet
- Direct from EC2: 1,024 GB × $0.09/GB = $92.16
- Via CloudFront: $85.00 (CloudFront pricing)
- Savings: ~$7/TB
```

---

### 7. Use AWS Cost Optimization Tools

**Tools and Services**:

1. **AWS Compute Optimizer**
   - ML-powered recommendations for EC2, EBS, Lambda
   - Analyze utilization patterns
   - Suggest right-sized resources

2. **AWS Trusted Advisor** (Business/Enterprise Support)
   - Cost optimization checks:
     - Idle RDS instances
     - Underutilized EC2 instances
     - Unassociated Elastic IP addresses
     - Low utilization EBS volumes
     - Idle Load Balancers

3. **Cost Explorer Recommendations**
   - Reserved Instance purchase recommendations
   - Savings Plans recommendations
   - Based on your historical usage

4. **AWS Cost Anomaly Detection**
   - Detect unexpected cost spikes
   - Get alerted to unusual spending

**Best Practice**: Review recommendations monthly and implement applicable suggestions

---

## Summary of Key Concepts

### Pricing Models to Remember

| Model | Discount | Commitment | Best For |
|-------|----------|------------|----------|
| On-Demand | 0% | None | Unpredictable workloads |
| Reserved (1-year) | ~40% | 1 year | Steady workloads |
| Reserved (3-year) | ~60-75% | 3 years | Long-term steady workloads |
| Spot Instances | ~90% | None (can be interrupted) | Fault-tolerant, flexible |
| Savings Plans | ~72% | 1-3 years | Flexible compute usage |

### Support Plans to Remember

| Plan | Cost | TAM | Response (Critical) | Trusted Advisor |
|------|------|-----|---------------------|----------------|
| Basic | Free | No | N/A | 7 checks |
| Developer | $29/mo | No | N/A | 7 checks |
| Business | $100/mo | No | < 1 hour | All checks |
| Enterprise | $15,000/mo | **Yes** | **< 15 min** | All checks |

### Free Tier to Remember

- **EC2**: 750 hours/month for 12 months
- **S3**: 5 GB storage for 12 months
- **Lambda**: 1 million requests/month (always free)
- **DynamoDB**: 25 GB storage (always free)
- **CloudFront**: 50 GB transfer out for 12 months

---

## Review Questions

### Question 1
Which AWS pricing principle allows customers to pay only for the compute resources they consume?

A. Pay less when you reserve
B. Pay-as-you-go
C. Volume-based discounts
D. Reserved capacity

<details>
<summary>Show Answer</summary>

**Answer: B. Pay-as-you-go**

Explanation: The pay-as-you-go pricing model is the core principle that allows customers to pay only for what they use, with no upfront costs or long-term commitments.
</details>

---

### Question 2
A company wants to reduce EC2 costs for a steady-state production workload that runs 24/7. Which purchasing option provides the MOST cost savings?

A. On-Demand Instances
B. Spot Instances
C. 3-year Reserved Instances with All Upfront payment
D. Dedicated Hosts

<details>
<summary>Show Answer</summary>

**Answer: C. 3-year Reserved Instances with All Upfront payment**

Explanation: For steady-state workloads running continuously, Reserved Instances with 3-year commitment and All Upfront payment provide the highest discount (up to 75%). Spot Instances offer higher discounts but can be interrupted, making them unsuitable for steady production workloads.
</details>

---

### Question 3
Which AWS Support plan provides a Technical Account Manager (TAM)?

A. Basic
B. Developer
C. Business
D. Enterprise

<details>
<summary>Show Answer</summary>

**Answer: D. Enterprise**

Explanation: Only the Enterprise Support plan includes a Technical Account Manager (TAM) who serves as a designated technical point of contact.
</details>

---

### Question 4
What is the response time for a business-critical system down issue under the Business Support plan?

A. < 15 minutes
B. < 1 hour
C. < 4 hours
D. < 12 hours

<details>
<summary>Show Answer</summary>

**Answer: B. < 1 hour**

Explanation: Business Support provides < 1 hour response for business-critical system down issues. Enterprise Support provides < 15 minutes for mission-critical issues.
</details>

---

### Question 5
Which tool should a company use to estimate costs BEFORE deploying resources to AWS?

A. AWS Cost Explorer
B. AWS Pricing Calculator
C. AWS Budgets
D. AWS Cost and Usage Report

<details>
<summary>Show Answer</summary>

**Answer: B. AWS Pricing Calculator**

Explanation: AWS Pricing Calculator is designed to estimate costs before deployment. Cost Explorer analyzes historical costs, Budgets sets cost alerts, and Cost and Usage Report provides detailed billing data.
</details>

---

### Question 6
A company has multiple AWS accounts and wants to receive a single bill for all accounts. Which feature should they use?

A. AWS Organizations with consolidated billing
B. AWS Cost Explorer
C. AWS Budgets
D. AWS Cost Allocation Tags

<details>
<summary>Show Answer</summary>

**Answer: A. AWS Organizations with consolidated billing**

Explanation: Consolidated billing through AWS Organizations combines usage from all accounts into a single bill, potentially providing volume discounts.
</details>

---

### Question 7
Which AWS service uses machine learning to detect unusual spending patterns and send alerts?

A. AWS Budgets
B. AWS Cost Anomaly Detection
C. AWS Cost Explorer
D. AWS Trusted Advisor

<details>
<summary>Show Answer</summary>

**Answer: B. AWS Cost Anomaly Detection**

Explanation: AWS Cost Anomaly Detection uses machine learning to automatically identify unusual spending patterns and send alerts, with no manual threshold configuration required.
</details>

---

### Question 8
How many Trusted Advisor checks are available with the Basic and Developer support plans?

A. None
B. 7 core checks
C. 50 checks
D. All checks

<details>
<summary>Show Answer</summary>

**Answer: B. 7 core checks**

Explanation: Basic and Developer support plans have access to 7 core Trusted Advisor checks. Business and Enterprise plans have access to all checks (50+).
</details>

---

### Question 9
Which AWS Free Tier offering NEVER expires?

A. 750 hours/month of EC2 t2.micro
B. 5 GB of S3 Standard storage
C. 1 million Lambda requests per month
D. 750 hours/month of RDS db.t2.micro

<details>
<summary>Show Answer</summary>

**Answer: C. 1 million Lambda requests per month**

Explanation: Lambda's 1 million requests/month is part of the "Always Free" tier that never expires. EC2, S3, and RDS offerings mentioned are part of the 12-month free tier.
</details>

---

### Question 10
A development team needs to track their AWS spending and receive alerts when costs exceed $1,000 per month. Which service should they use?

A. AWS Cost Explorer
B. AWS Budgets
C. AWS Pricing Calculator
D. AWS Organizations

<details>
<summary>Show Answer</summary>

**Answer: B. AWS Budgets**

Explanation: AWS Budgets allows you to set custom cost budgets and receive alerts (via email or SNS) when spending exceeds thresholds. The first two budgets are free.
</details>

---

### Question 11
Which data transfer scenario is typically FREE in AWS?

A. Data transfer out to the internet
B. Data transfer from S3 to the internet
C. Data transfer IN to AWS from the internet
D. Data transfer between AWS Regions

<details>
<summary>Show Answer</summary>

**Answer: C. Data transfer IN to AWS from the internet**

Explanation: Data transfer INTO AWS from the internet is generally free. Data transfer OUT to the internet and between Regions is charged.
</details>

---

### Question 12
Which EC2 pricing option is BEST for fault-tolerant workloads that can handle interruptions?

A. On-Demand Instances
B. Reserved Instances
C. Spot Instances
D. Dedicated Hosts

<details>
<summary>Show Answer</summary>

**Answer: C. Spot Instances**

Explanation: Spot Instances offer up to 90% discount but can be interrupted by AWS with a 2-minute warning, making them ideal for fault-tolerant, flexible workloads like batch processing.
</details>

---

### Question 13
What is the MOST comprehensive source of detailed AWS cost and usage data?

A. AWS Cost Explorer
B. AWS Cost and Usage Report
C. AWS Budgets
D. Monthly billing statement

<details>
<summary>Show Answer</summary>

**Answer: B. AWS Cost and Usage Report**

Explanation: AWS Cost and Usage Report provides the most detailed line-item breakdown of costs and usage, delivered to S3 for analysis with tools like Athena or Redshift.
</details>

---

### Question 14
Which AWS Support plan is the MINIMUM required for 24/7 phone support?

A. Basic
B. Developer
C. Business
D. Enterprise

<details>
<summary>Show Answer</summary>

**Answer: C. Business**

Explanation: Business Support is the minimum plan that provides 24/7 phone, email, and chat support. Developer only provides business hours email support.
</details>

---

### Question 15
A company wants architectural guidance specific to their use cases and production environment. Which support plan should they choose at MINIMUM?

A. Basic
B. Developer (General guidance)
C. Business (Contextual guidance)
D. Enterprise (Consultative guidance)

<details>
<summary>Show Answer</summary>

**Answer: C. Business**

Explanation: Business Support provides contextual architectural guidance related to specific use cases. Developer provides only general guidance, while Enterprise provides consultative guidance with a TAM.
</details>

---

## Key Takeaways

✅ **Pricing Fundamentals**: Understand pay-as-you-go, reserved capacity, volume discounts, and no upfront costs

✅ **Free Tier**: Know the three types - Always Free, 12 Months Free, and Trials

✅ **Cost Management Tools**: Pricing Calculator (estimate), Cost Explorer (analyze), Budgets (alert), Cost and Usage Report (detailed data)

✅ **Support Plans**: Memorize response times, TAM availability (Enterprise only), and Trusted Advisor access

✅ **Cost Optimization**: Right-sizing, Reserved Instances, Spot Instances, Auto Scaling, storage optimization, and data transfer optimization

✅ **Consolidated Billing**: Combine accounts in AWS Organizations for volume discounts and single billing

✅ **Data Transfer**: Inbound is free, outbound and cross-region are charged

---

[Previous: Technology and Services](./04-technology-services.md) | [Table of Contents](./README.md) | [Next: Practice Questions](./06-practice-questions.md)
