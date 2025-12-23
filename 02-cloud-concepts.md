# Domain 1: Cloud Concepts (24%)

[← Previous: Introduction](01-introduction.md) | [Back to Main](README.md) | [Next: Security and Compliance →](03-security-compliance.md)

## What is Cloud Computing?

### Definition

Cloud computing is the **on-demand delivery** of IT resources over the Internet with **pay-as-you-go pricing**. Instead of buying, owning, and maintaining physical data centers and servers, you access technology services on an as-needed basis.

### Six Advantages of Cloud Computing

#### 1. Trade Capital Expense for Variable Expense
- Pay only for what you consume
- No upfront infrastructure costs
- Lower Total Cost of Ownership (TCO)
- Convert fixed costs to variable costs

#### 2. Benefit from Massive Economies of Scale
- AWS achieves higher economies of scale
- Lower pay-as-you-go prices
- Prices decrease over time
- Shared infrastructure reduces costs

#### 3. Stop Guessing Capacity
- Scale up or down based on demand
- No over or under provisioning
- Elastic resources
- Automatic scaling capabilities

#### 4. Increase Speed and Agility
- Resources available in minutes
- Faster experimentation and innovation
- Reduced time to market
- Quick deployment of new features

#### 5. Stop Spending Money Running and Maintaining Data Centers
- Focus on business differentiators
- AWS manages infrastructure
- Reduce operational burden
- Eliminate "undifferentiated heavy lifting"

#### 6. Go Global in Minutes
- Deploy applications globally
- Low latency for users worldwide
- Multiple AWS Regions available
- Easy geographic expansion

> **Key Point:** These six advantages are frequently tested on the exam. Understand each one and be able to identify them in scenarios.

## Cloud Computing Models

### Infrastructure as a Service (IaaS)

**Definition:** Basic building blocks for cloud IT

**Characteristics:**
- Highest level of flexibility and control
- You manage: OS, applications, data
- AWS manages: Hardware, networking, facilities

**Examples:**
- Amazon EC2 (virtual servers)
- Amazon S3 (object storage)
- Amazon VPC (virtual networks)

**Use Cases:**
- Migration of existing applications
- Custom application development
- When you need full control over resources

### Platform as a Service (PaaS)

**Definition:** Removes need to manage underlying infrastructure

**Characteristics:**
- Focus on deployment and management of applications
- You manage: Applications, data
- AWS manages: Runtime, middleware, OS, servers

**Examples:**
- AWS Elastic Beanstalk
- AWS Lambda
- Amazon RDS

**Use Cases:**
- Web application deployment
- Rapid development and deployment
- When you want to focus on code, not infrastructure

### Software as a Service (SaaS)

**Definition:** Completed product run and managed by service provider

**Characteristics:**
- End-user applications
- You manage: User access, data input
- AWS manages: Everything else

**Examples:**
- Amazon WorkMail
- Amazon Chime
- Amazon QuickSight

**Use Cases:**
- Email and collaboration
- Business analytics
- CRM and productivity tools

> **Exam Tip:** Remember the progression: IaaS = most control, SaaS = least control; IaaS = most responsibility, SaaS = least responsibility.

## Cloud Deployment Models

### Cloud (Public Cloud)

**Characteristics:**
- Fully deployed in the cloud
- All parts of application run in cloud
- Applications built on cloud or migrated
- Can be built on low-level infrastructure or higher-level services

**Advantages:**
- No upfront investment
- Pay-as-you-go pricing
- Scalable and reliable
- No maintenance

**Examples:**
- Startup building entirely on AWS
- SaaS application
- Mobile app backend

### Hybrid

**Characteristics:**
- Connects cloud resources to on-premises infrastructure
- Integrates cloud with existing infrastructure
- Useful for legacy applications
- Common deployment model for many enterprises

**Connection Methods:**
- AWS Direct Connect
- AWS VPN
- AWS Storage Gateway

**Use Cases:**
- Gradual cloud migration
- Compliance requirements
- Extending on-premises capacity
- Disaster recovery

**Examples:**
- Company keeping databases on-premises, compute in cloud
- Backup and disaster recovery
- Cloud bursting for peak loads

### On-Premises (Private Cloud)

**Characteristics:**
- Resources deployed using virtualization and resource management tools
- Sometimes called "private cloud"
- Uses AWS Outposts for on-premises AWS infrastructure
- Increased resource utilization

**Use Cases:**
- Strict regulatory requirements
- Complete data control needed
- Legacy system constraints

**AWS Solution:**
- AWS Outposts brings AWS infrastructure on-premises

## AWS Well-Architected Framework

The AWS Well-Architected Framework describes key concepts, design principles, and architectural best practices for designing and running workloads in the cloud.

### Six Pillars

#### 1. Operational Excellence

**Focus:** Run and monitor systems to deliver business value

**Key Principles:**
- Perform operations as code
- Annotate documentation
- Make frequent, small, reversible changes
- Refine operations procedures frequently
- Anticipate failure
- Learn from all operational failures

**Key Services:**
- AWS CloudFormation (Infrastructure as Code)
- AWS Config (Track configuration changes)
- AWS CloudTrail (Audit API calls)
- Amazon CloudWatch (Monitoring and logging)

**Example Questions:**
- "How do we ensure our operations are efficient?"
- "How do we support development and run workloads effectively?"

#### 2. Security

**Focus:** Protect information, systems, and assets

**Key Principles:**
- Implement a strong identity foundation
- Enable traceability
- Apply security at all layers
- Automate security best practices
- Protect data in transit and at rest
- Keep people away from data
- Prepare for security events

**Key Services:**
- AWS IAM (Identity and Access Management)
- AWS Organizations (Multi-account management)
- AWS KMS (Key Management Service)
- AWS Shield (DDoS protection)
- Amazon GuardDuty (Threat detection)

**Example Questions:**
- "How do we control access to resources?"
- "How do we protect our data?"

#### 3. Reliability

**Focus:** Ensure workload performs its intended function correctly and consistently

**Key Principles:**
- Automatically recover from failure
- Test recovery procedures
- Scale horizontally to increase aggregate workload availability
- Stop guessing capacity
- Manage change in automation

**Key Services:**
- Amazon RDS Multi-AZ (High availability databases)
- AWS Auto Scaling (Automatic scaling)
- Amazon CloudWatch (Monitoring)
- AWS Backup (Centralized backup)

**Example Questions:**
- "How do we ensure our application can recover from failures?"
- "How do we meet availability requirements?"

#### 4. Performance Efficiency

**Focus:** Use computing resources efficiently to meet requirements

**Key Principles:**
- Democratize advanced technologies
- Go global in minutes
- Use serverless architectures
- Experiment more often
- Consider mechanical sympathy

**Key Services:**
- AWS Lambda (Serverless compute)
- Amazon EBS (Block storage)
- Amazon RDS (Managed databases)
- AWS Auto Scaling (Right-sizing)

**Example Questions:**
- "How do we select the right resource types?"
- "How do we ensure we use resources efficiently?"

#### 5. Cost Optimization

**Focus:** Run systems to deliver business value at lowest price point

**Key Principles:**
- Implement cloud financial management
- Adopt a consumption model
- Measure overall efficiency
- Stop spending money on undifferentiated heavy lifting
- Analyze and attribute expenditure

**Key Services:**
- AWS Cost Explorer (Analyze spending)
- AWS Budgets (Set cost alerts)
- Reserved Instances (Reduce costs)
- Savings Plans (Flexible pricing)
- AWS Trusted Advisor (Cost optimization recommendations)

**Example Questions:**
- "How do we reduce costs without impacting performance?"
- "How do we match capacity with demand?"

#### 6. Sustainability

**Focus:** Minimize environmental impacts of running cloud workloads

**Key Principles:**
- Understand your impact
- Establish sustainability goals
- Maximize utilization
- Anticipate and adopt new, more efficient hardware and software
- Use managed services
- Reduce downstream impact

**Key Services:**
- Amazon EC2 Auto Scaling (Optimize resource usage)
- AWS Lambda (Serverless reduces waste)
- Amazon S3 Intelligent-Tiering (Automatic storage optimization)

**Example Questions:**
- "How do we minimize environmental impact?"
- "How do we optimize resource usage?"

> **Key Point:** The Well-Architected Framework is frequently tested on the exam. Understand each pillar's purpose and key services.

### Well-Architected Tool

- Free service in AWS Console
- Review workload architecture
- Compare against best practices
- Get improvement recommendations
- Periodic reviews recommended

## Cloud Economics

### Total Cost of Ownership (TCO)

**Definition:** Financial estimate to identify direct and indirect costs

**Components:**
- **Server costs:** Hardware purchase and depreciation
- **Storage costs:** Storage hardware and maintenance
- **Network costs:** Networking equipment and bandwidth
- **IT labor costs:** Staff to manage infrastructure
- **Facility costs:** Power, cooling, space rental

**On-Premises Hidden Costs:**
- Hardware refresh cycles
- Over-provisioning for peak capacity
- Disaster recovery infrastructure
- Physical security
- Compliance and auditing

**AWS Advantages:**
- No upfront hardware costs
- Pay only for what you use
- No over-provisioning needed
- Built-in disaster recovery options
- AWS handles facility costs

**AWS TCO Calculator:**
- Helps estimate cost savings
- Compares on-premises to AWS
- Provides detailed cost breakdown

### Capital Expenditure (CapEx) vs. Operational Expenditure (OpEx)

#### CapEx (On-Premises)
- **Definition:** Upfront purchase of physical infrastructure
- **Characteristics:**
  - Fixed, sunk cost
  - Depreciates over time
  - Requires capacity planning
  - Long procurement cycles
  - Difficult to adjust

- **Examples:**
  - Purchasing servers
  - Building data centers
  - Networking equipment
  - Storage arrays

#### OpEx (Cloud)
- **Definition:** Pay for what you use
- **Characteristics:**
  - Variable cost based on consumption
  - No upfront commitment
  - Scales with business needs
  - Easier budgeting
  - Tax advantages

- **Examples:**
  - AWS monthly bills
  - Pay-per-use services
  - Subscription models

> **Exam Tip:** Cloud computing shifts IT spending from CapEx to OpEx, providing more flexibility and better alignment with business needs.

### Migration Strategies (6 R's)

Understanding these strategies helps you choose the right approach for cloud migration.

#### 1. Rehosting (Lift and Shift)

**Description:** Move applications without changes

**Advantages:**
- Fastest migration approach
- Minimal application changes
- Quick time to cloud
- Lower risk

**When to Use:**
- Need to migrate quickly
- Large-scale legacy migrations
- Cost optimization is immediate goal

**Example:**
- Move on-premises web servers directly to EC2

#### 2. Replatforming (Lift, Tinker, and Shift)

**Description:** Make a few cloud optimizations

**Advantages:**
- Tangible benefits without changing core architecture
- Some performance improvements
- Better cost efficiency than rehosting

**When to Use:**
- Want some cloud benefits
- Don't want to change core application
- Willing to make minor changes

**Example:**
- Migrate self-managed database to Amazon RDS
- Move application to Elastic Beanstalk

#### 3. Repurchasing

**Description:** Move to a different product

**Advantages:**
- Modern features and capabilities
- Reduced maintenance burden
- Often better user experience

**When to Use:**
- Legacy license costs are high
- Better SaaS alternative exists
- Want to modernize quickly

**Example:**
- Move CRM to Salesforce
- Migrate email to Amazon WorkMail
- Switch from on-premises Office to Office 365

#### 4. Refactoring / Re-architecting

**Description:** Reimagine how application is architected

**Advantages:**
- Use cloud-native features
- Best performance and scalability
- Highest long-term benefits
- Greatest cost optimization

**Disadvantages:**
- Most expensive upfront
- Most time-consuming
- Requires developers

**When to Use:**
- Need scalability and performance
- Want to add features difficult on-premises
- Long-term strategic applications

**Example:**
- Convert monolith to microservices
- Move to serverless architecture with Lambda
- Containerize applications with ECS/EKS

#### 5. Retire

**Description:** Identify IT assets that are no longer useful

**Advantages:**
- Reduce costs
- Reduce security risks
- Simplify portfolio

**When to Use:**
- Applications no longer used
- Redundant systems
- End of business function

**Example:**
- Shut down old reporting system
- Decommission legacy applications

#### 6. Retain

**Description:** Keep applications on-premises

**Reasons:**
- Not ready to migrate
- Recent upgrades made
- No business value in migration
- Regulatory requirements

**When to Use:**
- Applications require extensive refactoring
- On-premises dependencies
- Compliance constraints
- Plan to retire soon anyway

**Example:**
- Keep mainframe systems
- Retain applications being sunset
- Applications with data residency requirements

> **Exam Tip:** The 6 R's help organizations develop a migration strategy. Most migrations use a combination of these approaches.

## Review Questions

Test your understanding of cloud concepts:

1. Which of the following are advantages of cloud computing? (Choose TWO)
   - A. Trade variable expense for capital expense
   - B. Benefit from massive economies of scale
   - C. Stop guessing capacity
   - D. Increase spending on data center maintenance

2. What type of cloud deployment connects on-premises infrastructure with cloud resources?
   - A. Public cloud
   - B. Hybrid cloud
   - C. Private cloud
   - D. Multi-cloud

3. Which pillar of the AWS Well-Architected Framework focuses on protecting information and systems?
   - A. Operational Excellence
   - B. Security
   - C. Reliability
   - D. Performance Efficiency

4. Which migration strategy involves moving an application to the cloud without making changes?
   - A. Replatforming
   - B. Refactoring
   - C. Rehosting
   - D. Repurchasing

**Answers:**
1. B, C
2. B
3. B
4. C

---

[← Previous: Introduction](01-introduction.md) | [Back to Main](README.md) | [Next: Security and Compliance →](03-security-compliance.md)
