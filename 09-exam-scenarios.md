# Chapter 9: Common Exam Scenarios and Real-World Solutions

[← Previous: Exam Preparation](08-exam-preparation.md) | [Next: Additional Resources →](10-additional-resources.md)

---

## Table of Contents
- [Scenario-Based Learning](#scenario-based-learning)
  - [Scenario 1: Cost Optimization for Predictable Workloads](#scenario-1-cost-optimization-for-predictable-workloads)
  - [Scenario 2: Designing for High Availability](#scenario-2-designing-for-high-availability)
  - [Scenario 3: Large Data Migration](#scenario-3-large-data-migration)
  - [Scenario 4: Serverless Application Architecture](#scenario-4-serverless-application-architecture)
  - [Scenario 5: Compliance and Governance](#scenario-5-compliance-and-governance)
  - [Scenario 6: Disaster Recovery Strategy](#scenario-6-disaster-recovery-strategy)
  - [Scenario 7: Hybrid Cloud Connectivity](#scenario-7-hybrid-cloud-connectivity)
- [Common Troubleshooting Scenarios](#common-troubleshooting-scenarios)
  - [Cannot Connect to EC2 Instance](#cannot-connect-to-ec2-instance)
  - [S3 Access Denied Errors](#s3-access-denied-errors)
  - [Lambda Function Issues](#lambda-function-issues)

---

## Scenario-Based Learning

### Scenario 1: Cost Optimization for Predictable Workloads

**Situation**: A company runs a web application on EC2 instances that experiences predictable traffic Monday-Friday 9 AM-5 PM EST. Traffic is minimal on weekends and nights.

**Current Setup**:
- 10 m5.large instances running 24/7
- On-Demand pricing
- Monthly cost: $1,200

**Question**: What's the MOST cost-effective solution?

**Analysis**:
- Predictable schedule = opportunity for optimization
- Not running 24/7 = On-Demand might be wasteful
- Regular business hours = scheduled scaling
- Baseline capacity needed = Reserved Instances candidate

**Recommended Solution**:

1. **Purchase 3-year Standard Reserved Instances for 2-3 instances (baseline capacity)**
   - Savings: Up to 75% on these instances

2. **Configure EC2 Auto Scaling with scheduled actions**:
   - Scale up Monday-Friday 8:30 AM EST (before traffic starts)
   - Scale down at 5:30 PM EST (after traffic ends)
   - Minimum capacity on weekends: 2-3 instances

3. **Use On-Demand for peak periods during business hours**

4. **Store session data in ElastiCache or DynamoDB (not on instances)**

**Expected Savings**: 40-60% reduction in monthly costs

---

### Scenario 2: Designing for High Availability

**Situation**: An e-commerce company's application must remain available even if an entire Availability Zone fails. The application currently runs on a single EC2 instance with a MySQL database.

**Question**: How should you architect this for high availability?

**Current Problems**:
- Single point of failure (one EC2 instance)
- Database not redundant
- No automatic failover
- Session data tied to instance

**Recommended Solution**:

#### 1. Multi-AZ Application Tier
- Deploy **Application Load Balancer** spanning multiple AZs
- Create **Auto Scaling group** with minimum 2 instances across different AZs
- Set desired capacity based on traffic patterns
- Configure health checks on ALB and Auto Scaling

#### 2. Multi-AZ Database
- Migrate MySQL to **Amazon RDS Multi-AZ**
- Automatic failover to standby in different AZ
- Synchronous replication
- Minimal downtime during failover

#### 3. Stateless Application Design
- Store session data in **ElastiCache** (Redis with Multi-AZ)
- Or use **DynamoDB** for session storage
- Enable sticky sessions on ALB if needed (but prefer stateless)

#### 4. Static Assets
- Store in **S3** (automatically multi-AZ)
- Use **CloudFront** for global distribution

#### 5. Monitoring
- Set up **CloudWatch alarms** for health checks
- Configure **SNS notifications** for failures

**Architecture Benefits**:
- Survives AZ failure
- Automatic scaling for traffic spikes
- Automatic failover for database
- No single point of failure

---

### Scenario 3: Large Data Migration

**Situation**: A healthcare company needs to migrate 80 TB of medical imaging data from on-premises storage to S3. Compliance requires data to be encrypted and migration completed within 2 weeks.

**Constraints**:
- Internet connection: 100 Mbps
- Upload via internet would take: ~74 days
- Deadline: 2 weeks
- Data must be encrypted
- HIPAA compliance required

**Question**: What's the best migration approach?

**Analysis**:
- Data volume too large for internet upload
- Time constraint eliminates internet-based solutions
- Security and compliance requirements
- Need physical device for transfer

**Recommended Solution**:

#### 1. Use AWS Snowball Edge Storage Optimized
- 80 TB usable capacity per device
- Order 1-2 devices (for redundancy)
- 256-bit encryption built-in
- HIPAA compliant

#### 2. Migration Process

1. Order Snowball device via AWS Console
2. AWS ships device (2-3 days)
3. Connect to network, unlock with credentials
4. Copy data using Snowball client (2-4 days for 80 TB)
5. Ship device back to AWS (2-3 days)
6. AWS uploads to S3 (1-2 days)

#### 3. S3 Configuration
- Enable **S3 server-side encryption (SSE-S3 or SSE-KMS)**
- Enable **versioning** for data protection
- Configure **lifecycle policies** to transition older data to Glacier
- Enable **S3 Object Lock** for compliance (WORM)

#### 4. Compliance
- Use **AWS Artifact** to access HIPAA BAA
- Sign Business Associate Addendum (BAA)
- Enable **CloudTrail** for audit logging
- Use **AWS Config** for compliance monitoring

**Timeline**: 7-12 days (meets 2-week deadline)

> **Tip**: For data volumes >100 PB, use **AWS Snowmobile**

---

### Scenario 4: Serverless Application Architecture

**Situation**: A startup wants to build a mobile app backend with REST API. They have limited DevOps resources and want to minimize operational overhead while paying only for actual usage.

**Requirements**:
- REST API for mobile app
- User authentication
- Data storage
- Image storage
- Scalable to millions of users
- Minimal operational management
- Pay-per-use pricing

**Question**: What AWS services should they use?

**Recommended Serverless Architecture**:

#### 1. API Layer
- **Amazon API Gateway**: Create and manage REST API
- Features: Request throttling, API keys, caching, CORS
- Pay per million API calls

#### 2. Compute Layer
- **AWS Lambda**: Run business logic without servers
- Languages: Node.js, Python, Java, Go, etc.
- Auto-scaling built-in
- Pay only for execution time

#### 3. Authentication
- **Amazon Cognito**: User sign-up, sign-in, access control
- User pools for authentication
- Identity pools for AWS resource access
- Social identity providers (Facebook, Google)
- Free tier: 50,000 MAUs

#### 4. Data Storage
- **Amazon DynamoDB**: NoSQL database
- Single-digit millisecond latency
- Automatic scaling
- On-demand or provisioned capacity
- Always-free tier: 25 GB storage

#### 5. Image Storage
- **Amazon S3**: Store user-uploaded images
- Lifecycle policies to move old images to Glacier
- CloudFront for fast image delivery

#### 6. Optional Enhancements
- **Amazon CloudFront**: CDN for API and static assets
- **AWS AppSync**: GraphQL API (alternative to API Gateway + Lambda)
- **Amazon SES**: Send transactional emails
- **Amazon SNS**: Push notifications to mobile devices

**Benefits**:
- Zero server management
- Automatic scaling from 0 to millions of users
- Pay only for actual usage
- High availability built-in
- Focus on application code, not infrastructure
- Fast deployment and iteration

**Cost Example**:
- 1 million API requests: ~$3.50
- Lambda executions: ~$0.20
- DynamoDB: ~$1.25
- S3 storage (100 GB): ~$2.30
- **Total: ~$7.25/month for 1M requests**

---

### Scenario 5: Compliance and Governance

**Situation**: A financial services company with 50 AWS accounts needs to ensure no S3 buckets are publicly accessible across the organization. They also need to track all changes and demonstrate compliance.

**Requirements**:
- Enforce no public S3 buckets
- Apply to all accounts
- Monitor compliance continuously
- Audit all changes
- Automated remediation preferred

**Question**: How can they enforce and monitor this policy?

**Recommended Solution**:

#### 1. AWS Organizations Setup
- Group accounts using **Organizational Units (OUs)**
- Example structure: Production OU, Development OU, Test OU

#### 2. Service Control Policies (SCPs)
- Create SCP denying `s3:PutBucketPublicAccessBlock` with value False
- Deny `s3:PutBucketPolicy` if it allows public access
- Apply to root or specific OUs
- SCPs define maximum permissions (even admins can't override)

#### 3. S3 Block Public Access
- Enable **S3 Block Public Access** at organization level
- Applies to all accounts in organization
- Prevents accidental public exposure

#### 4. Continuous Monitoring
- Enable **AWS Config** across all accounts
- Deploy **s3-bucket-public-read-prohibited** rule
- Deploy **s3-bucket-public-write-prohibited** rule
- Automatic compliance reporting

#### 5. Automated Remediation
- Configure **AWS Config auto-remediation**
- Use AWS Systems Manager Automation documents
- Automatically disable public access when detected

#### 6. Audit and Logging
- Enable **CloudTrail** in all accounts
- Centralize logs in dedicated security account
- Track all S3 API calls
- Set up **CloudWatch alarms** for policy violations

#### 7. Centralized Security
- Use **AWS Security Hub** for centralized security view
- Aggregates findings from Config, GuardDuty, Inspector
- Compliance dashboards for standards (PCI DSS, CIS)

**Additional Recommendations**:
- Regular compliance reports using **AWS Artifact**
- Periodic access reviews
- Employee training on security best practices
- Implement least privilege IAM policies

---

### Scenario 6: Disaster Recovery Strategy

**Situation**: An e-commerce company needs disaster recovery for their application. Their business requires:
- RPO (Recovery Point Objective): 1 hour
- RTO (Recovery Time Objective): 4 hours
- Currently running in us-east-1

**Question**: What DR strategy should they implement?

**DR Strategy Options**:

| Strategy | RPO | RTO | Cost | Best For |
|----------|-----|-----|------|----------|
| **Backup and Restore** | Hours to days | Hours to days | Lowest | Non-critical workloads |
| **Pilot Light** ⭐ | Minutes to hours | Hours | Low-Medium | This scenario |
| **Warm Standby** | Seconds to minutes | Minutes | Medium-High | Critical applications |
| **Multi-Site Active/Active** | Near zero | Near zero | Highest | Mission-critical systems |

> **Recommendation**: **Pilot Light** is the optimal strategy for this scenario, meeting the 1-hour RPO and 4-hour RTO requirements at a reasonable cost.

**Recommended Pilot Light Implementation**:

#### 1. Data Replication
- Use **RDS cross-region read replicas**
- Replicate from us-east-1 to us-west-2
- Meets 1-hour RPO requirement

#### 2. Application AMIs
- Regularly copy AMIs to DR region
- Keep AMIs up-to-date
- Automate with Lambda

#### 3. Infrastructure as Code
- Use **CloudFormation templates**
- Pre-create VPC, subnets, security groups in DR region
- Keep Auto Scaling groups in DR region with 0 capacity

#### 4. DNS Failover
- Use **Route 53 health checks**
- Configure failover routing policy
- Automatic DNS failover to DR region

#### 5. Testing
- Quarterly DR drills
- Document runbooks
- Measure actual RTO/RPO

**Failover Process**:

1. Detect primary region failure (Route 53 health check)
2. Promote RDS read replica to master
3. Update CloudFormation stack to scale up Auto Scaling
4. Route 53 automatically redirects traffic
5. **Total time: ~2-3 hours (meets 4-hour RTO)**

---

### Scenario 7: Hybrid Cloud Connectivity

**Situation**: A manufacturing company wants to extend their on-premises data center to AWS while maintaining consistent network performance for their ERP system.

**Requirements**:
- Consistent network latency
- Private connection (no internet)
- Bandwidth: 1 Gbps
- Access to multiple VPCs

**Connection Options Analysis**:

| Solution | Pros | Cons | Best For |
|----------|------|------|----------|
| **Site-to-Site VPN** | Quick setup (hours), low cost, encrypted | Variable latency, internet-based, limited bandwidth | Dev/test, temporary connections |
| **AWS Direct Connect** ⭐ | Consistent performance, high bandwidth, private | Expensive, takes weeks, not encrypted by default | Production, high bandwidth needs |
| **Direct Connect + VPN** | Best of both worlds | Most expensive, complex | Regulated industries requiring encryption |

**Recommended Solution: AWS Direct Connect**

#### 1. Direct Connect Setup
- Order 1 Gbps Direct Connect port
- Work with AWS Direct Connect Partner
- Provision takes 2-4 weeks
- Set up cross-connect at colocation facility

#### 2. Multiple VPC Access
- Use **Direct Connect Gateway**
- Connect to multiple VPCs across regions
- Single Direct Connect connection
- Simplifies connectivity

#### 3. High Availability
- Order second Direct Connect connection (different location)
- Configure BGP for automatic failover
- Or use VPN as backup connection

#### 4. Security
- Layer VPN over Direct Connect for encryption
- Or use **MACsec** encryption
- Private VIF for VPC access
- Public VIF for public AWS services

---

## Common Troubleshooting Scenarios

### Cannot Connect to EC2 Instance

**Symptoms**: SSH or RDP connection times out or refused

**Troubleshooting Steps**:

#### 1. Verify Instance Status
- Check instance state is "running"
- Check status checks are passing
- View system log for boot errors

#### 2. Check Security Group
- Ensure inbound rule allows SSH (22) or RDP (3389)
- Verify source IP is allowed (0.0.0.0/0 or your IP)
- Check if security group changed recently

#### 3. Check Network ACL
- Ensure NACL allows inbound traffic on port
- Ensure NACL allows ephemeral outbound ports (1024-65535)
- **Important**: NACLs are stateless!

#### 4. Verify Network Configuration
- Instance has public IP (if connecting from internet)
- Instance in public subnet (has IGW route)
- Or using bastion host for private subnet

#### 5. Check Key Pair
- Using correct .pem/.ppk file
- File permissions correct (`chmod 400` for .pem)
- Key pair matches instance

#### 6. Check Route Table
- Subnet has route to IGW (0.0.0.0/0 → igw-xxx)
- Or route to NAT Gateway for private subnet

> **Tip**: Use EC2 Instance Connect or Systems Manager Session Manager as alternatives to SSH/RDP when troubleshooting connectivity issues.

---

### S3 Access Denied Errors

**Common Causes and Solutions**:

#### 1. IAM Permissions
- Verify IAM policy grants `s3:GetObject`, `s3:PutObject`
- Check for explicit Deny statements
- Verify resource ARN in policy matches bucket

#### 2. Bucket Policy
- Check bucket policy doesn't deny access
- Verify Principal in policy
- Check for IP-based restrictions

#### 3. Block Public Access
- If public access needed, disable Block Public Access
- Check both bucket-level and account-level settings

#### 4. Encryption
- If using SSE-KMS, verify KMS key policy
- Ensure user has `kms:Decrypt` permission

#### 5. Cross-Account Access
- Bucket policy must allow cross-account access
- Assume role with correct permissions

> **Debugging Tip**: Use AWS CloudTrail to review the API call and see the exact reason for access denial. Look for the `errorCode` and `errorMessage` fields in the CloudTrail logs.

---

### Lambda Function Issues

#### Issue 1: Function Timing Out

**Solutions**:
- Increase timeout (default 3 sec, max 15 min)
- Optimize code performance
- Check VPC configuration (can add latency)
- Increase memory (also increases CPU)
- Investigate cold start delays

> **Best Practice**: Set the timeout to slightly higher than your expected execution time, but not unnecessarily high to avoid long-running failed executions.

#### Issue 2: Insufficient Permissions

**Solutions**:
- Check Lambda execution role has required permissions
- Review CloudWatch Logs for permission errors
- Add necessary IAM policies to execution role
- For VPC: Ensure role has VPC execution permissions

**Common Required Permissions**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:*:*:*"
    }
  ]
}
```

#### Issue 3: Throttling

**Solutions**:
- Request concurrency limit increase
- Implement exponential backoff in calling application
- Use SQS to buffer requests
- Consider reserved concurrency for critical functions

**Understanding Throttling**:
- **Account-level limit**: 1,000 concurrent executions per region (default)
- **Function-level limit**: Can be set with reserved concurrency
- **Unreserved pool**: Shared across all functions without reserved concurrency

> **Tip**: Monitor the `ConcurrentExecutions` and `Throttles` metrics in CloudWatch to identify throttling issues early.

---

## Key Takeaways

1. **Cost Optimization**: Combine Reserved Instances, Spot Instances, and On-Demand based on workload patterns
2. **High Availability**: Always design across multiple Availability Zones
3. **Data Migration**: Use AWS physical devices (Snowball/Snowmobile) for large datasets
4. **Serverless**: Ideal for unpredictable workloads and minimal operational overhead
5. **Compliance**: Use AWS Organizations, SCPs, and AWS Config for governance at scale
6. **Disaster Recovery**: Choose DR strategy based on RPO/RTO requirements and budget
7. **Hybrid Connectivity**: Direct Connect for production, VPN for dev/test
8. **Troubleshooting**: Follow systematic approach from instance status to network configuration

---

[← Previous: Exam Preparation](08-exam-preparation.md) | [Next: Additional Resources →](10-additional-resources.md)
