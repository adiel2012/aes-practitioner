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
  - [Scenario 8: Multi-Region Architecture for Global Application](#scenario-8-multi-region-architecture-for-global-application)
  - [Scenario 9: Security Incident Response and Prevention](#scenario-9-security-incident-response-and-prevention)
  - [Scenario 10: Modernizing Legacy Monolith Application](#scenario-10-modernizing-legacy-monolith-application)
  - [Scenario 11: Big Data Analytics Platform](#scenario-11-big-data-analytics-platform)
  - [Scenario 12: DevOps CI/CD Pipeline Implementation](#scenario-12-devops-cicd-pipeline-implementation)
- [Common Troubleshooting Scenarios](#common-troubleshooting-scenarios)
  - [Cannot Connect to EC2 Instance](#cannot-connect-to-ec2-instance)
  - [S3 Access Denied Errors](#s3-access-denied-errors)
  - [Lambda Function Issues](#lambda-function-issues)
  - [RDS Connection Problems](#rds-connection-problems)
  - [CloudFormation Stack Failures](#cloudformation-stack-failures)
  - [Auto Scaling Not Working](#auto-scaling-not-working)
  - [High AWS Bill Unexpectedly](#high-aws-bill-unexpectedly)
  - [API Gateway 502/504 Errors](#api-gateway-502504-errors)

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

### Scenario 8: Multi-Region Architecture for Global Application

**Situation**: A social media company is launching a new photo-sharing application that needs to serve users across North America, Europe, and Asia. They expect rapid growth and need to provide low-latency access to content while maintaining data consistency.

**Current State**:
- Single-region deployment in us-east-1
- 200ms+ latency for users in Asia and Europe
- Customer complaints about slow image loading
- Growing user base: 100K users → 5M expected in 6 months

**Requirements**:

**Functional Requirements**:
- Users can upload/view photos from any region
- Social features: likes, comments, follows
- User profile and settings
- Search functionality
- Mobile and web access

**Non-Functional Requirements**:
- Latency: <100ms for content delivery
- Availability: 99.95%
- Data residency compliance (GDPR for EU)
- RPO: 1 hour, RTO: 2 hours
- Support 10M concurrent users
- Cost-effective scaling

**Question**: How should they architect a multi-region solution?

**Recommended Architecture**:

#### 1. Global Content Delivery

**Amazon CloudFront**:
- Deploy CloudFront distributions with edge locations worldwide
- Cache static assets (images, CSS, JavaScript)
- Regional edge caches for large files
- Configure custom origins pointing to regional endpoints
- Enable HTTP/2 and compression

**Amazon S3**:
- Create S3 buckets in each primary region (us-east-1, eu-west-1, ap-southeast-1)
- Enable S3 Transfer Acceleration for faster uploads
- Use S3 Intelligent-Tiering for automatic cost optimization
- Implement lifecycle policies for old content

**Cross-Region Replication**:
- Enable S3 Cross-Region Replication for disaster recovery
- Replicate photos bidirectionally between regions
- Use replication time control for predictable replication
- Replicate only active content (photos <30 days)

#### 2. Database Architecture

**Amazon DynamoDB Global Tables**:
- Deploy Global Tables across 3 regions
- Tables: Users, Posts, Likes, Comments, Follows
- Multi-master replication (writes to any region)
- Typical replication latency: <1 second
- Automatic conflict resolution (last writer wins)
- Use on-demand capacity for unpredictable traffic

**Alternative: Amazon Aurora Global Database**:
- If complex queries needed
- Primary region: us-east-1
- Read replicas in eu-west-1 and ap-southeast-1
- Lag: <1 second
- Failover: <1 minute
- Better for relational data and complex joins

**Data Residency Compliance**:
- Create separate DynamoDB tables for EU users
- Store EU user data only in eu-west-1
- Use IAM policies to enforce data boundaries
- Document data flow for GDPR compliance

#### 3. API and Application Layer

**Amazon API Gateway**:
- Deploy regional API Gateway endpoints
- Edge-optimized for CloudFront integration
- Custom domain names per region
- Request throttling and caching

**AWS Lambda or ECS Fargate**:
- Lambda for event-driven, sporadic workloads
- ECS Fargate for containerized applications
- Deploy in multiple AZs per region
- Auto Scaling based on request volume

#### 4. Routing and Traffic Management

**Amazon Route 53**:
- Create geolocation routing policy
- North America → us-east-1
- Europe → eu-west-1
- Asia → ap-southeast-1
- Configure health checks for failover
- Latency-based routing for optimal performance

**Implementation**:
```
User in Germany
→ Route 53 (geolocation: Europe)
→ CloudFront (Frankfurt edge)
→ API Gateway (eu-west-1)
→ Lambda/ECS (eu-west-1)
→ DynamoDB Global Table (eu-west-1)
→ S3 (eu-west-1) via CloudFront
```

#### 5. Search Functionality

**Amazon OpenSearch Service**:
- Deploy domain in each region
- Index user data and posts
- Cross-region snapshot for backup
- Or use Amazon CloudSearch

**Alternative: Amazon Kendra**:
- For intelligent search with ML
- Natural language queries

#### 6. Monitoring and Operations

**Amazon CloudWatch**:
- Cross-region dashboards
- Unified logging with CloudWatch Logs Insights
- Alarms for latency, errors, costs
- Custom metrics for business KPIs

**AWS X-Ray**:
- Distributed tracing across regions
- Identify bottlenecks
- Service map visualization

**Step-by-Step Implementation**:

**Phase 1: Foundation (Weeks 1-2)**
1. Set up AWS Organizations and multi-account structure
2. Create VPCs in target regions
3. Deploy CloudFormation templates for infrastructure
4. Set up centralized logging and monitoring
5. Configure IAM roles and policies

**Phase 2: Data Layer (Weeks 3-4)**
1. Create DynamoDB Global Tables
2. Set up S3 buckets with replication
3. Configure Aurora Global Database (if chosen)
4. Test data replication and consistency
5. Implement backup strategies

**Phase 3: Application Deployment (Weeks 5-6)**
1. Deploy API Gateway in all regions
2. Deploy Lambda functions or ECS services
3. Configure Auto Scaling policies
4. Implement caching strategies
5. Set up CloudFront distributions

**Phase 4: Routing and DNS (Week 7)**
1. Configure Route 53 geolocation routing
2. Set up health checks and failover
3. Test routing from different regions
4. Configure SSL/TLS certificates

**Phase 5: Testing and Optimization (Week 8)**
1. Load testing from multiple regions
2. Latency measurements
3. Failover testing
4. Cost optimization
5. Security hardening

**Cost Breakdown (Monthly Estimate for 5M users)**:

| Service | Configuration | Monthly Cost |
|---------|--------------|--------------|
| CloudFront | 10 TB data transfer, 100M requests | $850 |
| S3 | 50 TB storage, Transfer Acceleration | $1,250 |
| DynamoDB Global Tables | 1 billion requests, 500 GB | $1,800 |
| Lambda | 500M requests, 1GB memory | $900 |
| API Gateway | 500M requests | $1,750 |
| Route 53 | Hosted zones, health checks | $100 |
| CloudWatch | Logs, metrics, alarms | $250 |
| Data Transfer | Cross-region replication | $450 |
| **Total** | | **~$7,350/month** |

**Cost Optimization Strategies**:
1. Use S3 Intelligent-Tiering for automatic storage class transitions
2. Enable CloudFront compression to reduce data transfer
3. Implement DynamoDB on-demand pricing for variable workloads
4. Use reserved capacity for predictable base load
5. Set up AWS Budgets alerts
6. Archive old content to S3 Glacier

**Benefits**:
- **Performance**: <100ms latency worldwide
- **Availability**: 99.99% with multi-region failover
- **Scalability**: Seamlessly handles traffic spikes
- **Data Sovereignty**: GDPR compliance with regional data storage
- **User Experience**: Fast content delivery regardless of location
- **Business Continuity**: Automatic failover between regions

**Trade-offs**:
- **Complexity**: Managing multi-region infrastructure
- **Cost**: Higher than single-region deployment (3-4x)
- **Data Consistency**: Eventual consistency with Global Tables
- **Development**: More complex testing and deployment
- **Operational Overhead**: Multi-region monitoring and troubleshooting

**Alternative Approaches**:

**Option 1: Hybrid Approach**
- Primary region with CloudFront for content delivery
- Lower cost but higher latency for writes
- Best for read-heavy applications

**Option 2: Active-Passive Multi-Region**
- Active region handles all traffic
- Passive region for disaster recovery only
- Lower cost, simpler but longer failover time

**Option 3: Regional Isolation**
- Completely separate deployments per region
- No data replication between regions
- Best for data residency requirements

**Common Pitfalls to Avoid**:
1. **Not testing failover**: Regularly practice region failover
2. **Ignoring data transfer costs**: Can be 30-40% of total costs
3. **Synchronous replication assumptions**: DynamoDB Global Tables are eventually consistent
4. **Over-engineering**: Start with 2 regions, expand as needed
5. **Neglecting monitoring**: Set up comprehensive CloudWatch dashboards early
6. **Hardcoded endpoints**: Use service discovery or configuration
7. **Ignoring data residency laws**: Consult legal team for compliance
8. **Not considering latency for writes**: Global Tables have ~1s replication lag

---

### Scenario 9: Security Incident Response and Prevention

**Situation**: A healthcare technology company experienced a security incident where an S3 bucket containing patient data was briefly exposed publicly. The CISO has mandated a comprehensive security overhaul to prevent future incidents and improve detection and response capabilities.

**Current State**:
- 25 AWS accounts with inconsistent security practices
- No centralized security monitoring
- Manual security reviews
- Limited visibility into configuration changes
- Reactive security approach

**Incident Impact**:
- 10,000 patient records potentially exposed
- 4 hours until detection
- HIPAA violation investigation
- Reputation damage
- Potential fines up to $1.5M

**Requirements**:

**Functional Requirements**:
- Detect security threats in real-time
- Prevent unauthorized access
- Automated incident response
- Continuous compliance monitoring
- Audit trail for all actions
- Encryption at rest and in transit

**Non-Functional Requirements**:
- Detection time: <5 minutes
- Automated response: <1 minute
- 100% configuration compliance
- 7-year log retention
- SOC 2, HIPAA compliance
- Zero trust architecture

**Question**: How should they implement comprehensive security controls?

**Recommended Security Architecture**:

#### 1. Detective Controls - Threat Detection

**Amazon GuardDuty**:
- Enable in all accounts and regions
- Monitors VPC Flow Logs, CloudTrail, DNS logs
- ML-based anomaly detection
- Detects:
  - Compromised EC2 instances (cryptocurrency mining)
  - Reconnaissance activity
  - Unauthorized access attempts
  - Data exfiltration
  - Malicious IP communications

**AWS Security Hub**:
- Centralized security dashboard
- Aggregates findings from:
  - GuardDuty
  - Amazon Inspector
  - Amazon Macie
  - IAM Access Analyzer
  - AWS Config
  - Third-party tools
- Compliance checks against:
  - CIS AWS Foundations Benchmark
  - PCI DSS
  - HIPAA
  - AWS Foundational Security Best Practices

**Amazon Macie**:
- Automated sensitive data discovery
- Scans S3 buckets for PII, PHI
- Machine learning classification
- Identifies:
  - Credit card numbers
  - Social Security numbers
  - Patient health records
  - API keys and secrets

**AWS CloudTrail**:
- Enable in all regions
- Record all API calls
- Multi-region trail
- Log file integrity validation
- Centralized logging to dedicated security account
- S3 bucket with MFA Delete enabled
- Lifecycle policy: 7-year retention

#### 2. Preventive Controls - Access Management

**AWS Organizations with SCPs**:
- Organizational hierarchy:
  - Root
    - Security OU
    - Production OU
    - Development OU
    - Sandbox OU

**Service Control Policies**:
```json
// Prevent disabling security services
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "guardduty:DeleteDetector",
        "securityhub:DisableSecurityHub",
        "cloudtrail:StopLogging",
        "cloudtrail:DeleteTrail",
        "config:DeleteConfigRule",
        "config:StopConfigurationRecorder"
      ],
      "Resource": "*"
    }
  ]
}

// Enforce encryption
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "s3:PutObject",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": [
            "AES256",
            "aws:kms"
          ]
        }
      }
    }
  ]
}

// Prevent public S3 access
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "s3:PutAccountPublicAccessBlock"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "s3:PublicAccessBlock": "true"
        }
      }
    }
  ]
}
```

**IAM Access Analyzer**:
- Continuously monitors IAM policies
- Identifies resources shared with external entities
- Validates policies against best practices
- Generates policy recommendations

**AWS IAM Identity Center (SSO)**:
- Centralized user access management
- Multi-factor authentication mandatory
- Integration with corporate identity provider (Okta, Azure AD)
- Time-bound elevated access
- Attribute-based access control

#### 3. Continuous Compliance Monitoring

**AWS Config**:
- Enable in all regions and accounts
- Configuration recording for all resources
- Compliance rules:
  - `s3-bucket-public-read-prohibited`
  - `s3-bucket-public-write-prohibited`
  - `s3-bucket-server-side-encryption-enabled`
  - `rds-encryption-enabled`
  - `ec2-encrypted-volumes`
  - `cloudtrail-enabled`
  - `multi-region-cloudtrail-enabled`
  - `root-account-mfa-enabled`
  - `iam-password-policy`
  - `vpc-flow-logs-enabled`

**AWS Config Aggregator**:
- Centralized compliance view across all accounts
- Deployed in security account
- Cross-account access via IAM roles

#### 4. Automated Incident Response

**AWS Lambda for Auto-Remediation**:

**Scenario: S3 Bucket Made Public**
```python
# Lambda function triggered by Config rule violation
import boto3

def lambda_handler(event, context):
    s3 = boto3.client('s3')
    config = boto3.client('config')

    # Extract bucket name from Config event
    bucket_name = event['configRuleEvaluations'][0]['resourceId']

    # Block all public access
    s3.put_public_access_block(
        Bucket=bucket_name,
        PublicAccessBlockConfiguration={
            'BlockPublicAcls': True,
            'IgnorePublicAcls': True,
            'BlockPublicPolicy': True,
            'RestrictPublicBuckets': True
        }
    )

    # Send SNS notification
    sns = boto3.client('sns')
    sns.publish(
        TopicArn='arn:aws:sns:us-east-1:123456789012:SecurityAlerts',
        Subject='SECURITY: Public S3 Bucket Auto-Remediated',
        Message=f'Bucket {bucket_name} was made public and has been automatically secured.'
    )

    return {
        'statusCode': 200,
        'body': f'Remediated public access for {bucket_name}'
    }
```

**Amazon EventBridge Rules**:
- Trigger Lambda on GuardDuty findings
- Automated responses:
  - Isolate compromised EC2 instances (change security group)
  - Revoke IAM user credentials
  - Snapshot EBS volumes for forensics
  - Block malicious IPs in NACLs

**AWS Systems Manager Incident Manager**:
- Automated incident response plans
- Escalation policies
- On-call schedules
- Post-incident analysis

#### 5. Data Protection

**Encryption at Rest**:
- S3: Default encryption with KMS
- EBS: Encrypted volumes mandatory
- RDS: Encryption enabled for all databases
- DynamoDB: Encryption enabled
- EFS: Encryption enabled

**AWS Key Management Service (KMS)**:
- Customer-managed keys (CMKs)
- Automatic key rotation
- Key policies restricting access
- CloudTrail logging of key usage
- Separate keys per environment

**Encryption in Transit**:
- TLS 1.2+ for all communication
- AWS Certificate Manager for SSL/TLS
- VPC endpoints for private communication
- PrivateLink for service access

**AWS Secrets Manager**:
- Rotate database credentials automatically
- Store API keys and secrets
- Integration with RDS, Redshift, DocumentDB
- Audit secret access via CloudTrail

#### 6. Network Security

**VPC Security**:
- Private subnets for application and database tiers
- Public subnets only for load balancers
- VPC Flow Logs enabled (all VPCs)
- Network ACLs for subnet-level filtering

**AWS Network Firewall**:
- Stateful firewall at VPC level
- Intrusion prevention system (IPS)
- Block malicious domains
- Custom rule groups

**AWS WAF (Web Application Firewall)**:
- Protect web applications from common exploits
- Managed rules:
  - OWASP Top 10
  - Known bad inputs
  - SQL injection
  - Cross-site scripting (XSS)
- Rate limiting
- Geo-blocking

**Step-by-Step Implementation**:

**Phase 1: Foundation (Week 1)**
1. Enable CloudTrail in all accounts
2. Create security account
3. Set up centralized logging S3 bucket
4. Enable GuardDuty in all accounts/regions
5. Document current security posture

**Phase 2: Detection and Monitoring (Week 2)**
1. Enable Security Hub
2. Enable Macie for S3 scanning
3. Deploy Config with compliance rules
4. Set up Config Aggregator
5. Create CloudWatch dashboards

**Phase 3: Preventive Controls (Week 3)**
1. Implement SCPs in AWS Organizations
2. Enable S3 Block Public Access organization-wide
3. Deploy IAM Access Analyzer
4. Enforce MFA for all users
5. Implement IAM Identity Center

**Phase 4: Automated Response (Week 4)**
1. Create Lambda remediation functions
2. Set up EventBridge rules
3. Configure SNS topics for alerts
4. Deploy Systems Manager Incident Manager
5. Test automated responses

**Phase 5: Data Protection (Week 5)**
1. Enable default encryption on S3
2. Encrypt all EBS volumes
3. Deploy KMS CMKs with rotation
4. Migrate secrets to Secrets Manager
5. Implement AWS Backup

**Phase 6: Testing and Validation (Week 6)**
1. Conduct security drills
2. Penetration testing
3. Red team exercises
4. Update incident response playbooks
5. Train security team

**Cost Breakdown (Monthly for 25 accounts)**:

| Service | Configuration | Monthly Cost |
|---------|--------------|--------------|
| GuardDuty | 25 accounts, 500GB VPC Flow Logs | $650 |
| Security Hub | 25 accounts, 10K compliance checks | $200 |
| Macie | 1TB S3 data scanned | $300 |
| CloudTrail | Multi-region, 1M events | $50 |
| Config | 25 accounts, 500 rules | $800 |
| AWS WAF | 5 web ACLs, 100M requests | $150 |
| KMS | 100 CMKs | $100 |
| Lambda | Auto-remediation functions | $50 |
| S3 | Log storage (1TB) | $25 |
| **Total** | | **~$2,325/month** |

**Benefits**:
- **Rapid Detection**: Security threats detected in <5 minutes
- **Automated Response**: Incidents remediated in <1 minute
- **Compliance**: Continuous monitoring against standards
- **Visibility**: Centralized view of security posture
- **Prevention**: Proactive controls prevent incidents
- **Audit Trail**: Complete history for compliance
- **Cost of Prevention**: $2,325/month vs. potential $1.5M fine

**Trade-offs**:
- **Initial Complexity**: Setting up centralized security takes time
- **False Positives**: GuardDuty may flag legitimate activity
- **Operational Changes**: Teams must adapt to new security controls
- **Cost**: Ongoing security spend vs. risk mitigation

**Alternative Approaches**:

**Option 1: Third-Party SIEM**
- Splunk, Sumo Logic, or Datadog
- More advanced analytics
- Higher cost
- Additional maintenance

**Option 2: Manual Response**
- Lower cost
- Slower response times
- Not recommended for compliance

**Common Pitfalls to Avoid**:
1. **Security Hub alert fatigue**: Start with critical findings only
2. **Not testing auto-remediation**: Test in dev first
3. **Overly restrictive SCPs**: Can block legitimate operations
4. **Ignoring GuardDuty findings**: Review and act on all findings
5. **No incident response plan**: Document procedures before incidents
6. **Single region deployment**: Enable security services in all regions
7. **No security training**: Educate developers on secure practices
8. **Forgetting about insider threats**: Monitor privileged user activity

---

### Scenario 10: Modernizing Legacy Monolith Application

**Situation**: An insurance company runs a 15-year-old .NET Framework application on on-premises servers. The application handles policy management, claims processing, and customer portal. They want to migrate to AWS and modernize the architecture to improve scalability, reduce costs, and accelerate feature development.

**Current State**:
- Monolithic .NET Framework 4.8 application
- SQL Server 2014 database (2TB)
- Windows Server 2012 R2
- 10 application servers behind hardware load balancer
- Peak load: 5,000 concurrent users
- Deployment: Manual, monthly releases, 4-hour downtime
- No automated testing
- Average response time: 2-3 seconds
- Annual infrastructure cost: $500K

**Current Challenges**:
- Slow development cycles
- Difficult to scale individual components
- High infrastructure costs
- Frequent production issues
- Aging technology stack
- Recruitment challenges (old tech)

**Requirements**:

**Functional Requirements**:
- Migrate all functionality to AWS
- Maintain feature parity during migration
- Support existing integrations (SOAP, REST APIs)
- Preserve data integrity
- Windows authentication integration

**Non-Functional Requirements**:
- Zero downtime during migration
- Response time: <1 second
- 99.9% availability
- Support 10,000 concurrent users
- Reduce infrastructure costs by 40%
- Weekly deployments with zero downtime
- Automated testing and rollback

**Question**: How should they approach migration and modernization?

**Recommended Migration Strategy: Strangler Fig Pattern**

#### Phase 1: Lift and Shift (Foundation)

**Step 1: Database Migration**

**AWS Database Migration Service (DMS)**:
- Migrate SQL Server to Amazon RDS for SQL Server
- Minimal downtime using continuous replication
- Or migrate to RDS with Aurora PostgreSQL-Compatible (if licensing costs are high)

**Database Configuration**:
- RDS SQL Server Enterprise Edition
- Multi-AZ deployment for high availability
- db.r5.4xlarge (16 vCPU, 128 GB RAM)
- 3TB storage with Provisioned IOPS (10,000 IOPS)
- Automated backups (7-day retention)
- Automated patching in maintenance window

**Migration Process**:
1. Set up DMS replication instance
2. Create source endpoint (on-premises SQL Server)
3. Create target endpoint (RDS)
4. Create migration task with full load + CDC
5. Monitor replication lag
6. Perform cutover during low-traffic period

**Step 2: Application Migration with App2Container**

**AWS App2Container**:
- Analyzes .NET Framework applications
- Creates container image
- Generates ECS task definitions
- Creates CloudFormation templates
- Minimal code changes required

**Process**:
1. Install App2Container on application server
2. Run inventory: `app2container inventory`
3. Analyze application: `app2container analyze --application-id <id>`
4. Customize deployment (app2container-config.json)
5. Generate artifacts: `app2container containerize`
6. Push to Amazon ECR

**Step 3: Container Orchestration**

**Amazon ECS on Fargate**:
- Serverless compute for containers
- No EC2 instances to manage
- Automatic scaling
- Integrated with Application Load Balancer

**ECS Configuration**:
- Task definition:
  - 4 vCPU, 8 GB memory per task
  - Windows Server 2019 Core container
  - Environment variables for configuration
  - Secrets from AWS Secrets Manager
- Service:
  - Desired count: 10 tasks
  - Auto Scaling: 10-50 tasks based on CPU
  - Spread across 3 AZs
  - Health check grace period: 60 seconds

#### Phase 2: Modernization (Incremental)

**Strangler Fig Pattern Implementation**:
1. Identify bounded contexts in monolith
2. Extract one service at a time
3. Route traffic to new service
4. Gradually replace monolith components

**Priority Services to Extract**:

**1. Authentication Service**
- High reuse across features
- Extract first for shared use
- Technology: ASP.NET Core Web API
- Database: Amazon Aurora PostgreSQL
- Deployment: ECS Fargate

**2. Claims Processing Service**
- CPU-intensive
- Independent scaling needs
- Benefits from queue-based processing
- Technology: ASP.NET Core + AWS Lambda
- Queue: Amazon SQS
- Database: DynamoDB for claims status

**3. Document Storage Service**
- Large file uploads (claim documents, policy PDFs)
- Extract to reduce monolith load
- Technology: ASP.NET Core API
- Storage: Amazon S3
- OCR: Amazon Textract

**4. Notification Service**
- Email, SMS notifications
- High volume, sporadic
- Technology: AWS Lambda
- Email: Amazon SES
- SMS: Amazon SNS
- Queue: Amazon SQS

**5. Reporting Service**
- Resource-intensive queries
- Extract to dedicated read replica
- Technology: ASP.NET Core + Lambda
- Database: RDS read replica
- Caching: Amazon ElastiCache

**Architecture Evolution**:

```
Initial (Month 0-3):
Monolith (ECS) → RDS SQL Server

Phase 1 (Month 3-6):
ALB → Authentication Service (ECS)
    ↓
    → Monolith (ECS) → RDS SQL Server

Phase 2 (Month 6-9):
ALB → Authentication Service (ECS)
    → Claims Service (ECS + Lambda + SQS)
    → Monolith (ECS) → RDS SQL Server

Phase 3 (Month 9-12):
ALB → Authentication Service (ECS)
    → Claims Service (ECS + Lambda + SQS)
    → Document Service (ECS + S3 + Textract)
    → Notification Service (Lambda + SQS + SES/SNS)
    → Reporting Service (Lambda + ElastiCache)
    → Monolith (ECS) → RDS SQL Server (reduced functionality)
```

#### Phase 3: Supporting Infrastructure

**Caching Layer**:

**Amazon ElastiCache for Redis**:
- Cache frequently accessed data
- Session storage
- Reduce database load by 60%
- Configuration:
  - cache.r5.large (2 nodes)
  - Multi-AZ with automatic failover
  - Encryption in transit and at rest

**Application Load Balancer**:
- Path-based routing
- Example routes:
  - `/api/auth/*` → Authentication Service
  - `/api/claims/*` → Claims Service
  - `/api/documents/*` → Document Service
  - `/*` → Monolith (default)
- Sticky sessions for monolith compatibility
- SSL/TLS termination
- WAF integration

**API Gateway**:
- For external partners accessing APIs
- Rate limiting and quotas
- API key management
- Request/response transformation
- CloudWatch logging

**Observability**:

**AWS X-Ray**:
- Distributed tracing
- Identify performance bottlenecks
- Service map visualization
- Request flow analysis

**Amazon CloudWatch**:
- Centralized logging
- Custom metrics (business KPIs)
- Dashboards for each service
- Alarms for errors and latency

**AWS CloudTrail**:
- Audit trail for all API calls
- Compliance and security

#### Phase 4: CI/CD Pipeline

**AWS CodePipeline**:
```
Source (CodeCommit)
  ↓
Build (CodeBuild)
  - Compile .NET Core
  - Run unit tests
  - Build Docker image
  - Push to ECR
  ↓
Test (CodeBuild)
  - Integration tests
  - Security scanning (Snyk, Aqua)
  ↓
Deploy to Dev (CodeDeploy + ECS)
  - Blue/green deployment
  - Smoke tests
  ↓
Manual Approval
  ↓
Deploy to Prod (CodeDeploy + ECS)
  - Blue/green deployment
  - Gradual traffic shifting (10% → 50% → 100%)
  - Automatic rollback on errors
```

**AWS CodeBuild buildspec.yml**:
```yaml
version: 0.2
phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com
  build:
    commands:
      - echo Build started on `date`
      - echo Building the Docker image...
      - docker build -t $IMAGE_REPO_NAME:$IMAGE_TAG .
      - docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
  post_build:
    commands:
      - echo Build completed on `date`
      - echo Pushing the Docker image...
      - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG
      - echo Writing image definitions file...
      - printf '[{"name":"app-container","imageUri":"%s"}]' $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG > imagedefinitions.json
artifacts:
  files: imagedefinitions.json
```

**Step-by-Step Implementation**:

**Phase 1: Preparation (Months 1-2)**
1. Assess application architecture
2. Identify dependencies and integrations
3. Set up AWS accounts and networking
4. Create migration plan
5. Train team on AWS services

**Phase 2: Database Migration (Month 3)**
1. Set up RDS instance
2. Test DMS replication
3. Migrate database with CDC
4. Verify data integrity
5. Update connection strings

**Phase 3: Containerize Monolith (Month 4)**
1. Use App2Container
2. Test containerized application
3. Deploy to ECS Fargate
4. Parallel run with on-premises
5. Gradual traffic shift (20% → 50% → 100%)

**Phase 4: Extract Services (Months 5-12)**
1. Extract authentication service (Month 5)
2. Extract claims service (Month 6-7)
3. Extract document service (Month 8-9)
4. Extract notification service (Month 10)
5. Extract reporting service (Month 11)
6. Decommission monolith components (Month 12)

**Phase 5: Optimization (Ongoing)**
1. Implement caching strategies
2. Optimize database queries
3. Right-size compute resources
4. Implement auto-scaling
5. Cost optimization

**Cost Breakdown Comparison**:

**On-Premises (Annual)**:
- Hardware amortization: $200K
- Maintenance and support: $150K
- Datacenter costs: $100K
- Personnel (4 FTEs): $400K (partially allocated)
- **Total: $500K/year**

**AWS Modernized Architecture (Annual)**:
| Service | Configuration | Monthly | Annual |
|---------|--------------|---------|--------|
| ECS Fargate | 30 tasks average, Windows | $3,600 | $43,200 |
| RDS SQL Server | Multi-AZ, db.r5.4xlarge | $5,500 | $66,000 |
| Application Load Balancer | 2 ALBs | $150 | $1,800 |
| ElastiCache | Redis, 2 nodes | $250 | $3,000 |
| S3 | 10 TB storage, requests | $300 | $3,600 |
| Lambda | 10M requests | $200 | $2,400 |
| CloudWatch | Logs, metrics | $400 | $4,800 |
| Data Transfer | Outbound | $500 | $6,000 |
| **Total** | | **~$10,900/month** | **~$131K/year** |

**Additional Costs**:
- Migration tools and professional services: $50K (one-time)
- Training: $20K (one-time)

**Total Year 1**: $200K
**Total Year 2+**: $131K/year

**Savings**:
- Year 1: $300K (60% reduction)
- Year 2+: $369K (74% reduction)

**Benefits**:
- **Cost Reduction**: 74% infrastructure cost savings
- **Scalability**: Auto-scaling handles 2x traffic without manual intervention
- **Performance**: Response time reduced from 3s to <1s
- **Deployment Speed**: Monthly → weekly deployments
- **Availability**: 99.5% → 99.9%
- **Innovation**: Development team focuses on features, not infrastructure
- **Recruitment**: Modern tech stack attracts talent
- **Disaster Recovery**: Built-in with multi-AZ deployment

**Trade-offs**:
- **Migration Time**: 12-month project
- **Learning Curve**: Team must learn AWS, containers, microservices
- **Complexity**: Distributed systems more complex than monolith
- **Operational Changes**: New monitoring and deployment processes
- **Initial Investment**: Time and resources for migration

**Alternative Approaches**:

**Option 1: Full Rewrite**
- Rebuild application from scratch
- Pros: Latest technology, clean architecture
- Cons: High risk, 2-3 years, expensive
- Recommendation: Avoid unless absolutely necessary

**Option 2: Lift and Shift Only**
- Migrate to EC2 without containerization
- Pros: Fastest migration (3 months)
- Cons: Limited benefits, still managing VMs
- Recommendation: Only if time-constrained

**Option 3: Serverless-First**
- Convert to Lambda + API Gateway + DynamoDB
- Pros: Maximum scalability, lowest operational overhead
- Cons: Requires significant rewrite, cold starts
- Recommendation: For new features, not existing monolith

**Common Pitfalls to Avoid**:
1. **Big Bang Migration**: Incremental migration reduces risk
2. **Ignoring Data Migration Complexity**: DMS testing is critical
3. **Not Modernizing Architecture**: Lift-and-shift alone provides limited benefits
4. **Underestimating Team Training**: Budget time for learning
5. **No Rollback Plan**: Always have a way to revert
6. **Skipping Load Testing**: Test at 2x expected peak load
7. **Not Involving Business Stakeholders**: Get buy-in early
8. **Ignoring Observability**: Implement monitoring from day one

---

### Scenario 11: Big Data Analytics Platform

**Situation**: A retail company collects massive amounts of data from online transactions, mobile app usage, IoT sensors in stores, and social media. They want to build a comprehensive analytics platform to gain real-time insights into customer behavior, optimize inventory, and improve marketing effectiveness.

**Current State**:
- Multiple data sources generating 5 TB/day
- Data scattered across different systems
- Manual reporting (takes 2-3 days)
- No real-time analytics
- Limited data science capabilities
- Expensive third-party analytics tools ($500K/year)

**Data Sources**:
- Web/mobile clickstream: 2 billion events/day
- Transaction logs: 10 million transactions/day
- IoT sensors (foot traffic, temperature): 50 million readings/day
- Social media mentions: APIs and web scraping
- CRM data: Customer profiles and interactions
- Inventory systems: Stock levels and shipments

**Requirements**:

**Functional Requirements**:
- Ingest data from multiple sources
- Real-time dashboards for operations
- Batch processing for daily/weekly reports
- Ad-hoc SQL queries for analysts
- Machine learning for recommendations
- Data retention: Hot (90 days), Warm (1 year), Cold (7 years)

**Non-Functional Requirements**:
- Real-time latency: <1 minute
- Query performance: <5 seconds for interactive queries
- Scalability: Handle 10x data growth
- Cost-effective at scale
- Data governance and security
- Self-service analytics for business users

**Question**: How should they architect a big data analytics platform?

**Recommended Architecture**:

#### 1. Data Ingestion Layer

**Real-Time Streaming Data**:

**Amazon Kinesis Data Streams**:
- For clickstream, IoT sensors
- Shards: 50 (1 MB/s per shard = 50 MB/s total)
- Retention: 7 days for replay capability
- Producers: Web/mobile apps, IoT devices via Kinesis Agent

**Amazon Kinesis Data Firehose**:
- Deliver streams to S3, Redshift, OpenSearch
- Automatic batching and compression
- Transform data with Lambda
- Buffer size: 5 MB or 60 seconds

**Batch Data Ingestion**:

**AWS Glue ETL Jobs**:
- Extract from source databases
- Transform and clean data
- Load to S3 data lake
- Schedule: Nightly for transaction logs, CRM data

**AWS Database Migration Service (DMS)**:
- Continuous replication from transactional databases
- Change Data Capture (CDC)
- Minimal impact on source systems

**API-Based Ingestion**:

**AWS Lambda**:
- Fetch data from social media APIs
- Parse and normalize
- Write to Kinesis or S3
- Schedule with EventBridge (hourly)

#### 2. Storage Layer - Data Lake

**Amazon S3**:
- Central data lake repository
- Organized by:
  - Data source
  - Date partitioning (year/month/day)
  - File format (Parquet, ORC for analytics)

**S3 Bucket Structure**:
```
s3://retail-datalake-raw/
  ├── clickstream/year=2025/month=01/day=15/
  ├── transactions/year=2025/month=01/day=15/
  ├── iot-sensors/year=2025/month=01/day=15/
  └── social-media/year=2025/month=01/day=15/

s3://retail-datalake-processed/
  ├── customer-360/
  ├── sales-analytics/
  └── inventory-metrics/

s3://retail-datalake-curated/
  ├── marketing-reports/
  └── executive-dashboards/
```

**S3 Storage Classes**:
- Standard: Last 90 days (hot data)
- Standard-IA: 91 days - 1 year (warm data)
- Glacier Flexible Retrieval: 1-7 years (cold data)
- Lifecycle policies for automatic transitions

**S3 Features**:
- Versioning enabled for data protection
- Server-side encryption (SSE-S3 or SSE-KMS)
- S3 Object Lock for compliance
- S3 Access Points for different teams
- S3 Inventory for data catalog

#### 3. Data Processing Layer

**Real-Time Processing**:

**Amazon Kinesis Data Analytics**:
- SQL queries on streaming data
- Tumbling/sliding windows
- Real-time aggregations
- Anomaly detection
- Output to Lambda, Kinesis, S3

**AWS Lambda**:
- Process individual events
- Enrich with reference data (DynamoDB)
- Real-time alerts via SNS
- Trigger downstream workflows

**Batch Processing**:

**AWS Glue**:
- Serverless Spark-based ETL
- Discovers schema automatically
- Glue Data Catalog (metadata repository)
- Glue Studio for visual ETL
- Glue DataBrew for data preparation

**Amazon EMR (Elastic MapReduce)**:
- For complex Spark, Hadoop jobs
- EMR on EKS for containerized workloads
- Spot Instances for cost savings (70% reduction)
- Cluster configuration:
  - Master: m5.xlarge (1 instance)
  - Core: r5.2xlarge (5 instances, On-Demand)
  - Task: r5.2xlarge (20 instances, Spot)

**Typical Glue ETL Job**:
```python
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job

args = getResolvedOptions(sys.argv, ['JOB_NAME'])
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args['JOB_NAME'], args)

# Read from Data Catalog
datasource0 = glueContext.create_dynamic_frame.from_catalog(
    database = "retail_raw",
    table_name = "clickstream"
)

# Transform
applymapping1 = ApplyMapping.apply(
    frame = datasource0,
    mappings = [
        ("user_id", "string", "customer_id", "string"),
        ("event_timestamp", "long", "event_time", "timestamp"),
        ("page_url", "string", "page_url", "string"),
        ("session_id", "string", "session_id", "string")
    ]
)

# Filter out invalid records
filtered = Filter.apply(
    frame = applymapping1,
    f = lambda x: x["customer_id"] is not None
)

# Write to S3 in Parquet format
glueContext.write_dynamic_frame.from_options(
    frame = filtered,
    connection_type = "s3",
    connection_options = {
        "path": "s3://retail-datalake-processed/customer-sessions/",
        "partitionKeys": ["year", "month", "day"]
    },
    format = "parquet"
)

job.commit()
```

#### 4. Data Catalog and Governance

**AWS Glue Data Catalog**:
- Centralized metadata repository
- Schema registry
- Integration with Athena, Redshift, EMR
- Glue Crawlers for automatic schema discovery

**AWS Lake Formation**:
- Fine-grained access control
- Column-level security
- Row-level security
- Data filtering
- Audit logging
- Governed tables for ACID transactions

**Access Control Example**:
```
Data Lake Administrator:
  - Full access to all tables

Marketing Team:
  - Read access to: customer_360, campaign_analytics
  - Column filtering: Hide PII (SSN, credit card)

Data Science Team:
  - Read access to: all tables
  - Write access to: ml_models bucket

Finance Team:
  - Read access to: sales_analytics, inventory_metrics
  - Row filtering: Only their region's data
```

#### 5. Analytics and Querying

**Amazon Athena**:
- Interactive SQL queries on S3 data
- Serverless (no infrastructure)
- Pay per query ($5 per TB scanned)
- Integration with QuickSight
- Workgroups for cost control
- Query result caching

**Optimization Techniques**:
- Partition data by date
- Use columnar formats (Parquet, ORC)
- Compress data (Snappy, ZSTD)
- Limit columns in SELECT
- Use approximate functions (approx_distinct vs COUNT DISTINCT)

**Query Performance Comparison**:
- CSV, uncompressed: $5/TB, 45 seconds
- Parquet, Snappy: $0.50/TB, 5 seconds
- **90% cost reduction, 9x faster**

**Amazon Redshift**:
- Data warehouse for complex queries
- Massively parallel processing
- Configuration:
  - Node type: ra3.4xlarge
  - Nodes: 5 (640 GB RAM, 128 TB storage)
  - Redshift Spectrum for S3 queries
  - Concurrency Scaling for peak loads
  - Materialized views for aggregations

**Use Cases**:
- Athena: Ad-hoc queries, exploration, infrequent queries
- Redshift: Regular reports, complex joins, consistent performance

#### 6. Business Intelligence and Visualization

**Amazon QuickSight**:
- Serverless BI service
- Connect to Athena, Redshift, S3
- SPICE (in-memory engine) for fast visuals
- ML-powered insights
- Embedded analytics for applications
- Pricing: $5/author/month, $0.30/reader/session

**Dashboards**:
1. **Executive Dashboard**:
   - Daily sales trends
   - Revenue by region
   - Top products
   - Customer acquisition cost

2. **Operations Dashboard**:
   - Real-time store foot traffic
   - Inventory levels
   - Stockout alerts
   - Supply chain metrics

3. **Marketing Dashboard**:
   - Campaign performance
   - Customer segmentation
   - Conversion funnels
   - Social media sentiment

4. **Data Science Dashboard**:
   - Model performance metrics
   - A/B test results
   - Recommendation effectiveness

#### 7. Machine Learning Pipeline

**Amazon SageMaker**:
- Train recommendation models
- Fraud detection
- Demand forecasting
- Customer churn prediction

**ML Workflow**:
1. **Data Preparation**: Glue DataBrew or SageMaker Data Wrangler
2. **Feature Engineering**: SageMaker Processing Jobs
3. **Model Training**: SageMaker Training Jobs (Spot Instances)
4. **Model Evaluation**: SageMaker Experiments
5. **Model Registry**: SageMaker Model Registry
6. **Deployment**: SageMaker Endpoints (real-time or batch)
7. **Monitoring**: SageMaker Model Monitor

**Amazon Personalize**:
- Pre-built recommendation engine
- No ML expertise required
- Real-time and batch recommendations
- Use cases:
  - Product recommendations
  - Personalized rankings
  - Similar items

#### 8. Orchestration and Workflow

**AWS Step Functions**:
- Coordinate multi-step data pipelines
- Visual workflow designer
- Error handling and retry logic
- Integration with Lambda, Glue, EMR, SageMaker

**Example Daily Pipeline**:
```
1. Ingest data (Lambda, Glue)
   ↓
2. Data quality checks (Lambda)
   ↓
3. ETL processing (Glue or EMR)
   ↓
4. Load to Redshift (Glue)
   ↓
5. Refresh materialized views (Redshift)
   ↓
6. Update ML models (SageMaker)
   ↓
7. Refresh QuickSight datasets
   ↓
8. Send completion notification (SNS)
```

**Amazon Managed Workflows for Apache Airflow (MWAA)**:
- Alternative to Step Functions
- For complex DAGs (Directed Acyclic Graphs)
- Python-based workflow definitions
- Better for data engineering teams familiar with Airflow

#### 9. Monitoring and Optimization

**Amazon CloudWatch**:
- Glue job metrics
- EMR cluster utilization
- Kinesis stream metrics
- Athena query performance
- Custom business metrics

**AWS Cost Explorer**:
- Analyze spending by service
- Identify optimization opportunities
- Reserved Instance recommendations

**AWS Trusted Advisor**:
- Cost optimization checks
- Security best practices

**Step-by-Step Implementation**:

**Phase 1: Foundation (Months 1-2)**
1. Set up AWS accounts and networking
2. Create S3 data lake structure
3. Deploy AWS Glue Data Catalog
4. Set up Lake Formation permissions
5. Implement data governance policies

**Phase 2: Ingestion (Month 3)**
1. Deploy Kinesis streams for real-time data
2. Set up Glue ETL jobs for batch data
3. Implement Lambda for API ingestion
4. Test data flow end-to-end
5. Monitor data quality

**Phase 3: Processing (Months 4-5)**
1. Build Glue ETL pipelines
2. Deploy EMR clusters for complex processing
3. Implement data quality checks
4. Set up Step Functions orchestration
5. Optimize job performance

**Phase 4: Analytics (Month 6)**
1. Create Athena tables
2. Deploy Redshift cluster
3. Build initial dashboards in QuickSight
4. Train business users on self-service
5. Gather feedback and iterate

**Phase 5: ML (Months 7-8)**
1. Set up SageMaker environment
2. Build recommendation model
3. Deploy fraud detection
4. Implement demand forecasting
5. Monitor model performance

**Phase 6: Optimization (Ongoing)**
1. Right-size resources
2. Implement caching strategies
3. Optimize data formats
4. Use Spot Instances
5. Continuous cost monitoring

**Cost Breakdown (Monthly)**:

| Service | Configuration | Monthly Cost |
|---------|--------------|--------------|
| Kinesis Data Streams | 50 shards, 5TB ingestion | $1,200 |
| Kinesis Firehose | 5TB delivery | $125 |
| S3 Storage | 100TB (tiered) | $2,000 |
| AWS Glue | 200 DPU-hours ETL | $880 |
| Amazon EMR | 25 nodes, 8 hrs/day, 70% Spot | $2,400 |
| Redshift | 5 ra3.4xlarge nodes | $12,000 |
| Athena | 10TB scanned/month | $50 |
| QuickSight | 50 authors, 500 readers | $400 |
| SageMaker | Training + endpoints | $1,500 |
| Lambda | 50M invocations | $100 |
| Data Transfer | Outbound | $500 |
| CloudWatch | Logs and metrics | $300 |
| **Total** | | **~$21,455/month** |

**Cost Optimization Strategies**:
1. Use Spot Instances for EMR (70% savings)
2. Convert data to Parquet (90% storage reduction)
3. Partition data effectively (80% query cost reduction)
4. Use S3 Intelligent-Tiering
5. Right-size Redshift with pause/resume
6. Use Athena for infrequent queries vs. Redshift
7. Implement S3 lifecycle policies

**Optimized Cost**: ~$14,000/month (35% reduction)

**Benefits**:
- **Real-Time Insights**: <1 minute latency for operational decisions
- **Cost Savings**: $500K/year (third-party tools) → $168K/year (50% savings)
- **Scalability**: Handle 10x data growth without architectural changes
- **Self-Service**: Business users run their own queries
- **Data-Driven Decisions**: ML-powered recommendations increase revenue 15%
- **Time to Insight**: 2-3 days → <1 hour for reports
- **Compliance**: Fine-grained access control and audit trails

**Trade-offs**:
- **Complexity**: Distributed systems require skilled team
- **Learning Curve**: Training required for Spark, SQL, ML
- **Initial Cost**: Higher upfront investment
- **Data Quality**: Garbage in, garbage out - need robust quality checks

**Alternative Approaches**:

**Option 1: Redshift-Centric**
- Load all data into Redshift
- Simpler architecture
- Higher cost for storage
- Best for: Smaller datasets (<10TB)

**Option 2: EMR-Centric**
- Use EMR for all processing
- More control and flexibility
- More operational overhead
- Best for: Teams with Hadoop/Spark expertise

**Option 3: Third-Party (Snowflake, Databricks)**
- Managed services
- Excellent performance
- Higher cost
- Less control
- Best for: Teams wanting minimal operational burden

**Common Pitfalls to Avoid**:
1. **Not Partitioning Data**: Results in slow queries and high costs
2. **Ignoring Data Formats**: CSV vs. Parquet makes 10x difference
3. **Over-Provisioning**: Start small, scale as needed
4. **No Data Governance**: Implement access controls from day one
5. **Ignoring Data Quality**: Build validation into pipelines
6. **Not Using Spot Instances**: 70% cost savings for EMR
7. **Storing Everything in Redshift**: Use S3 data lake + Redshift Spectrum
8. **No Monitoring**: Implement CloudWatch alarms and dashboards early

---

### Scenario 12: DevOps CI/CD Pipeline Implementation

**Situation**: A SaaS company with 20 developers is struggling with manual deployment processes. Code is deployed to production once a month, with frequent rollbacks due to bugs. Deployments take 4-6 hours and require manual steps. The team wants to implement modern DevOps practices with automated CI/CD pipelines.

**Current State**:
- Manual deployments via SSH and scripts
- No automated testing
- Deployment frequency: Monthly
- Deployment duration: 4-6 hours
- Rollback rate: 30%
- Production incidents: 2-3 per month
- Developer frustration: High
- Time to market: 4-6 weeks for features

**Current Process**:
1. Developers commit to shared Git branch
2. Manual code review (informal)
3. QA team tests for 1 week
4. Operations team deploys on weekends
5. Frequent production issues on Monday

**Problems**:
- Long feedback loops
- Manual error-prone deployments
- No deployment consistency
- Difficult rollbacks
- Fear of deploying
- Bottleneck at operations team

**Requirements**:

**Functional Requirements**:
- Automated build and test on every commit
- Automated deployment to dev/staging/prod
- Code quality checks (linting, security scanning)
- Automated rollback capability
- Infrastructure as Code
- Secrets management
- Multi-environment support

**Non-Functional Requirements**:
- Deployment frequency: Multiple times per day
- Deployment duration: <15 minutes
- Automated rollback: <5 minutes
- Rollback rate: <5%
- Zero-downtime deployments
- Audit trail for compliance
- Cost-effective

**Question**: How should they implement a modern CI/CD pipeline?

**Recommended CI/CD Architecture**:

#### 1. Source Control and Branching Strategy

**AWS CodeCommit**:
- Git-based source control
- Integration with AWS services
- Encryption at rest and in transit
- IAM-based access control
- Supports Git LFS for large files
- Pull request workflows

**Alternative**: GitHub, GitLab, Bitbucket
- If already using these platforms
- CodePipeline integrates with all

**Branching Strategy (Trunk-Based Development)**:
```
main (production)
  ├── feature/user-auth (short-lived)
  ├── feature/payment-integration (short-lived)
  └── hotfix/critical-bug (short-lived)
```

**Trunk-Based Guidelines**:
- Small, frequent commits to main
- Feature flags for incomplete features
- Short-lived feature branches (<2 days)
- Pull requests with automated checks
- Merge only if tests pass

#### 2. Continuous Integration Pipeline

**AWS CodeBuild**:
- Fully managed build service
- Docker-based build environments
- Pay per build minute
- Scales automatically
- Integration with security scanning tools

**Build Specification (buildspec.yml)**:
```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 18
      docker: 20
    commands:
      - echo Installing dependencies...
      - npm install

  pre_build:
    commands:
      - echo Running pre-build checks...
      - npm run lint
      - npm run security-check
      - echo Logging in to Amazon ECR...
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com

  build:
    commands:
      - echo Build started on `date`
      - echo Running unit tests...
      - npm test -- --coverage
      - echo Building application...
      - npm run build
      - echo Building Docker image...
      - docker build -t $IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION .
      - docker tag $IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION
      - docker tag $IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:latest

  post_build:
    commands:
      - echo Build completed on `date`
      - echo Pushing Docker image...
      - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION
      - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:latest
      - echo Generating build artifacts...
      - printf '[{"name":"app-container","imageUri":"%s"}]' $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$CODEBUILD_RESOLVED_SOURCE_VERSION > imagedefinitions.json

artifacts:
  files:
    - imagedefinitions.json
    - appspec.yml
    - taskdef.json
    - '**/*'
  discard-paths: no

reports:
  test-results:
    files:
      - 'test-results/**/*'
    file-format: 'JUNITXML'
  coverage-report:
    files:
      - 'coverage/clover.xml'
    file-format: 'CLOVERXML'

cache:
  paths:
    - '/root/.npm/**/*'
    - 'node_modules/**/*'
```

**Automated Checks in CI**:
1. **Unit Tests**: Jest, Mocha, pytest
2. **Code Coverage**: Minimum 80% threshold
3. **Linting**: ESLint, Prettier, Black
4. **Security Scanning**:
   - Snyk for dependency vulnerabilities
   - OWASP Dependency-Check
   - SonarQube for code quality
5. **Container Scanning**: Amazon ECR image scanning
6. **Infrastructure Validation**: cfn-lint, terraform validate

#### 3. Continuous Deployment Pipeline

**AWS CodePipeline**:
- Orchestrates CI/CD workflow
- Visual pipeline editor
- Integration with third-party tools
- Parallel and sequential stages
- Approval gates
- Automated rollback

**Pipeline Stages**:

```
┌─────────────────────────────────────────────────────────────────┐
│                           SOURCE STAGE                          │
│  - CodeCommit/GitHub trigger on push to main                    │
│  - Fetch source code                                            │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                           BUILD STAGE                           │
│  - CodeBuild compiles, tests, builds Docker image              │
│  - Push to ECR                                                  │
│  - Generate artifacts                                           │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                       TEST STAGE (Dev)                          │
│  - Deploy to Dev environment (ECS/EKS)                          │
│  - CodeBuild: Integration tests                                │
│  - CodeBuild: API tests (Postman/Newman)                        │
│  - CodeBuild: Performance tests (k6, JMeter)                    │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                     DEPLOY STAGE (Staging)                      │
│  - CodeDeploy to Staging environment                            │
│  - Blue/green deployment                                        │
│  - Smoke tests                                                  │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                      MANUAL APPROVAL                            │
│  - SNS notification to approvers                                │
│  - Review test results                                          │
│  - Approve or reject production deployment                      │
└────────────────────┬────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                  DEPLOY STAGE (Production)                      │
│  - CodeDeploy to Production                                     │
│  - Blue/green deployment                                        │
│  - Traffic shifting: 10% → 50% → 100%                          │
│  - Automatic rollback on CloudWatch alarms                      │
└─────────────────────────────────────────────────────────────────┘
```

#### 4. Deployment Strategy

**AWS CodeDeploy**:
- Automated deployments
- Multiple deployment types:
  - In-place
  - Blue/green
  - Canary
  - Linear
- Automatic rollback
- Integration with ECS, Lambda, EC2, on-premises

**Blue/Green Deployment (ECS)**:

**AppSpec File (appspec.yml)**:
```yaml
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: <TASK_DEFINITION>
        LoadBalancerInfo:
          ContainerName: "app-container"
          ContainerPort: 8080
        PlatformVersion: "LATEST"
        NetworkConfiguration:
          AwsvpcConfiguration:
            Subnets:
              - subnet-12345678
              - subnet-87654321
            SecurityGroups:
              - sg-12345678
            AssignPublicIp: "DISABLED"

Hooks:
  - BeforeInstall: "LambdaFunctionToValidateBeforeInstall"
  - AfterInstall: "LambdaFunctionToValidateAfterInstall"
  - AfterAllowTestTraffic: "LambdaFunctionToRunIntegrationTests"
  - BeforeAllowTraffic: "LambdaFunctionToWarmUpCache"
  - AfterAllowTraffic: "LambdaFunctionToValidateProduction"
```

**Traffic Shifting Strategy**:
- **Canary**: 10% of traffic for 5 minutes, then 100%
- **Linear**: Increase by 10% every 5 minutes
- **All-at-once**: Immediate switch (not recommended for prod)

**Automatic Rollback Triggers**:
- CloudWatch Alarm: Error rate >5%
- CloudWatch Alarm: Response time >2 seconds
- CloudWatch Alarm: CPU utilization >80%
- Deployment failure

#### 5. Infrastructure as Code

**AWS CloudFormation**:
- Define infrastructure in YAML/JSON
- Version control infrastructure
- Stack updates with rollback
- Drift detection

**Alternative: AWS CDK (Cloud Development Kit)**:
- Define infrastructure in programming languages
- Python, TypeScript, Java, C#
- Higher level abstractions
- Synthesizes to CloudFormation

**Example CDK Stack (TypeScript)**:
```typescript
import * as cdk from 'aws-cdk-lib';
import * as ec2 from 'aws-cdk-lib/aws-ec2';
import * as ecs from 'aws-cdk-lib/aws-ecs';
import * as ecsPatterns from 'aws-cdk-lib/aws-ecs-patterns';
import * as codedeploy from 'aws-cdk-lib/aws-codedeploy';

export class AppInfraStack extends cdk.Stack {
  constructor(scope: cdk.App, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // VPC
    const vpc = new ec2.Vpc(this, 'AppVPC', {
      maxAzs: 3,
      natGateways: 2
    });

    // ECS Cluster
    const cluster = new ecs.Cluster(this, 'AppCluster', {
      vpc: vpc,
      containerInsights: true
    });

    // Fargate Service with ALB
    const fargateService = new ecsPatterns.ApplicationLoadBalancedFargateService(
      this,
      'AppService',
      {
        cluster: cluster,
        cpu: 512,
        desiredCount: 3,
        taskImageOptions: {
          image: ecs.ContainerImage.fromRegistry('amazon/amazon-ecs-sample'),
          containerPort: 8080,
          environment: {
            ENVIRONMENT: 'production'
          }
        },
        memoryLimitMiB: 1024,
        publicLoadBalancer: true,
        deploymentController: {
          type: ecs.DeploymentControllerType.CODE_DEPLOY
        }
      }
    );

    // Auto Scaling
    const scaling = fargateService.service.autoScaleTaskCount({
      minCapacity: 3,
      maxCapacity: 20
    });

    scaling.scaleOnCpuUtilization('CpuScaling', {
      targetUtilizationPercent: 70
    });

    scaling.scaleOnMemoryUtilization('MemoryScaling', {
      targetUtilizationPercent: 80
    });

    // CodeDeploy Deployment Group
    const deploymentGroup = new codedeploy.EcsDeploymentGroup(
      this,
      'AppDeploymentGroup',
      {
        service: fargateService.service,
        blueGreenDeploymentConfig: {
          blueTargetGroup: fargateService.targetGroup,
          greenTargetGroup: fargateService.targetGroup,
          listener: fargateService.listener,
          terminationWaitTime: cdk.Duration.minutes(5)
        },
        deploymentConfig: codedeploy.EcsDeploymentConfig.CANARY_10PERCENT_5MINUTES,
        autoRollback: {
          failedDeployment: true,
          stoppedDeployment: true,
          deploymentInAlarm: true
        },
        alarms: [
          new cloudwatch.Alarm(this, 'ErrorAlarm', {
            metric: fargateService.targetGroup.metrics.httpCodeTarget(
              elb.HttpCodeTarget.TARGET_5XX_COUNT
            ),
            threshold: 10,
            evaluationPeriods: 2
          })
        ]
      }
    );
  }
}
```

#### 6. Secrets Management

**AWS Secrets Manager**:
- Store database credentials, API keys
- Automatic rotation
- Encryption with KMS
- Fine-grained access control
- Integration with RDS, Redshift

**Alternative: AWS Systems Manager Parameter Store**:
- Free for standard parameters
- Hierarchical storage
- No automatic rotation
- Good for configuration values

**Example Usage in ECS Task**:
```json
{
  "containerDefinitions": [
    {
      "name": "app-container",
      "secrets": [
        {
          "name": "DB_PASSWORD",
          "valueFrom": "arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/db/password-AbCdEf"
        },
        {
          "name": "API_KEY",
          "valueFrom": "arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/api/key-XyZaBc"
        }
      ]
    }
  ]
}
```

#### 7. Monitoring and Observability

**Amazon CloudWatch**:
- Unified logging from all environments
- Custom metrics
- Dashboards for pipeline health
- Alarms for deployment failures

**AWS X-Ray**:
- Distributed tracing
- Identify performance bottlenecks
- Request flow visualization

**Key Metrics to Monitor**:
- **Deployment Frequency**: Deploys per day
- **Lead Time**: Commit to production time
- **Mean Time to Recovery (MTTR)**: Time to fix production issue
- **Change Failure Rate**: % of deployments causing failure
- **Build Duration**: Time for build pipeline
- **Test Coverage**: % of code covered by tests

**CloudWatch Dashboard**:
```json
{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "metrics": [
          [ "AWS/CodePipeline", "PipelineExecutionSuccess", { "stat": "Sum" } ],
          [ ".", "PipelineExecutionFailure", { "stat": "Sum" } ]
        ],
        "period": 300,
        "stat": "Sum",
        "region": "us-east-1",
        "title": "Pipeline Executions"
      }
    },
    {
      "type": "metric",
      "properties": {
        "metrics": [
          [ "AWS/ECS", "CPUUtilization", { "stat": "Average" } ],
          [ ".", "MemoryUtilization", { "stat": "Average" } ]
        ],
        "period": 300,
        "stat": "Average",
        "region": "us-east-1",
        "title": "ECS Resource Utilization"
      }
    }
  ]
}
```

#### 8. Multi-Environment Strategy

**Account Structure**:
- Dev Account: Frequent deployments, lower-cost resources
- Staging Account: Production-like environment
- Production Account: Strict change control

**Environment-Specific Configuration**:
```
config/
  ├── dev.json
  ├── staging.json
  └── production.json
```

**Parameter Store Hierarchy**:
```
/app/dev/database/host
/app/dev/database/port
/app/staging/database/host
/app/staging/database/port
/app/production/database/host
/app/production/database/port
```

#### 9. Testing Strategy

**Test Pyramid**:
```
         ┌─────────────┐
         │  E2E Tests  │  ← Fewer, slower, expensive
         │   (Cypress) │
         └─────────────┘
       ┌─────────────────┐
       │Integration Tests│
       │  (API, Database)│
       └─────────────────┘
    ┌──────────────────────┐
    │     Unit Tests       │  ← Many, fast, cheap
    │ (Jest, pytest, JUnit)│
    └──────────────────────┘
```

**Test Types**:
1. **Unit Tests**: 80% of tests, <100ms each
2. **Integration Tests**: 15% of tests, <5s each
3. **E2E Tests**: 5% of tests, <30s each

**CodeBuild Test Stage**:
```yaml
phases:
  build:
    commands:
      - echo Running unit tests...
      - npm test -- --coverage --maxWorkers=4
      - echo Running integration tests...
      - npm run test:integration
      - echo Running E2E tests...
      - npm run test:e2e

  post_build:
    commands:
      - echo Checking test coverage threshold...
      - npm run coverage:check -- --lines 80 --functions 80 --branches 75
```

#### 10. Rollback Strategy

**Automatic Rollback**:
- CloudWatch alarms trigger rollback
- CodeDeploy automatically reverts to previous version
- <5 minutes to rollback

**Manual Rollback**:
```bash
# Rollback to previous deployment
aws deploy stop-deployment \
  --deployment-id d-1234567890 \
  --auto-rollback-enabled

# Or redeploy previous version
aws ecs update-service \
  --cluster my-cluster \
  --service my-service \
  --task-definition my-task:42  # Previous version
```

**Feature Flags**:
- Use AWS AppConfig or Launch Darkly
- Toggle features without redeployment
- Gradual rollout to users
- Quick disable if issues arise

**Step-by-Step Implementation**:

**Phase 1: Source Control (Week 1)**
1. Migrate code to CodeCommit/GitHub
2. Define branching strategy
3. Set up pull request workflows
4. Configure branch protection rules
5. Train team on Git best practices

**Phase 2: CI Pipeline (Week 2)**
1. Create buildspec.yml
2. Set up CodeBuild project
3. Integrate linting and testing
4. Add security scanning
5. Configure build notifications

**Phase 3: Containerization (Week 3)**
1. Create Dockerfile
2. Set up Amazon ECR
3. Build container images in pipeline
4. Test locally with Docker Compose
5. Document container configuration

**Phase 4: Infrastructure as Code (Week 4)**
1. Define infrastructure in CloudFormation/CDK
2. Create VPC, subnets, security groups
3. Deploy ECS cluster and services
4. Set up Application Load Balancer
5. Test infrastructure provisioning

**Phase 5: CD Pipeline (Week 5)**
1. Create CodePipeline
2. Add deployment stages (Dev, Staging, Prod)
3. Configure CodeDeploy
4. Set up approval gates
5. Test end-to-end deployment

**Phase 6: Monitoring (Week 6)**
1. Set up CloudWatch dashboards
2. Configure alarms
3. Integrate X-Ray tracing
4. Set up log aggregation
5. Define KPIs and metrics

**Phase 7: Testing and Optimization (Weeks 7-8)**
1. Test failure scenarios
2. Practice rollback procedures
3. Optimize build times
4. Tune auto-scaling parameters
5. Document runbooks

**Cost Breakdown (Monthly)**:

| Service | Configuration | Monthly Cost |
|---------|--------------|--------------|
| CodeCommit | 5 active users, 10 GB | $2 |
| CodeBuild | 500 build minutes (general1.small) | $25 |
| CodePipeline | 10 pipelines, 200 executions | $10 |
| CodeDeploy | Free for ECS, Lambda | $0 |
| ECR | 50 GB storage | $5 |
| ECS Fargate | 6 tasks (0.5 vCPU, 1 GB) | $140 |
| ALB | 2 load balancers | $40 |
| S3 | Artifact storage | $5 |
| CloudWatch | Logs, metrics, dashboards | $50 |
| Secrets Manager | 20 secrets | $8 |
| **Total** | | **~$285/month** |

**Benefits**:
- **Deployment Frequency**: Monthly → Multiple times per day
- **Deployment Duration**: 4-6 hours → <15 minutes
- **Rollback Time**: Hours → <5 minutes
- **Rollback Rate**: 30% → <5%
- **Developer Productivity**: +40% (less time on deployments)
- **Mean Time to Recovery**: 4 hours → <30 minutes
- **Production Incidents**: 2-3/month → <1/month
- **Time to Market**: 4-6 weeks → 1-2 weeks
- **Developer Satisfaction**: Significantly improved

**Trade-offs**:
- **Initial Setup**: 6-8 weeks for full implementation
- **Learning Curve**: Team must learn new tools and practices
- **Cultural Change**: Shift from manual to automated processes
- **Responsibility Shift**: Developers more involved in operations

**Alternative Approaches**:

**Option 1: Jenkins on EC2**
- Open-source CI/CD
- More plugins and flexibility
- Requires server management
- Higher operational overhead
- Best for: Teams with existing Jenkins expertise

**Option 2: GitHub Actions**
- Native GitHub integration
- Easy to set up
- Limited AWS integration compared to CodePipeline
- Best for: GitHub-centric workflows

**Option 3: GitLab CI/CD**
- All-in-one DevOps platform
- Built-in container registry
- Requires separate hosting
- Best for: Teams wanting single platform

**Option 4: Third-Party (CircleCI, Travis CI)**
- Easy to set up
- Great developer experience
- Additional cost
- Limited control
- Best for: Startups wanting quick setup

**Common Pitfalls to Avoid**:
1. **No Rollback Testing**: Practice rollbacks regularly
2. **Skipping Integration Tests**: Catch issues before production
3. **Manual Steps in Pipeline**: Automate everything
4. **Ignoring Build Times**: Optimize for <10 minute builds
5. **No Monitoring**: Implement comprehensive monitoring early
6. **Over-Engineering**: Start simple, add complexity as needed
7. **Ignoring Security**: Scan for vulnerabilities in pipeline
8. **No Documentation**: Document architecture and runbooks
9. **Forgetting Notifications**: Alert team on pipeline failures
10. **Not Measuring**: Track DORA metrics (deployment frequency, lead time, MTTR, change failure rate)

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

### RDS Connection Problems

**Symptoms**: Cannot connect to RDS database from application

**Troubleshooting Steps**:

#### 1. Verify Endpoint and Port
- Check endpoint hostname is correct
- Default ports: MySQL (3306), PostgreSQL (5432), SQL Server (1433)
- Verify database is available (not stopped or in maintenance)

#### 2. Security Group Configuration
- Inbound rule must allow traffic on database port
- Source should be application security group or IP range
- Example: MySQL on 3306 from application SG

#### 3. Network Accessibility
- **Public Accessibility**: Set to Yes if connecting from internet
- **Private Subnet**: Application must be in same VPC or have connectivity
- **VPC Peering/VPN**: Required for cross-VPC or on-premises access

#### 4. Database Credentials
- Verify username and password
- Check if password has special characters needing escaping
- Master user vs. database-specific users
- For Aurora, use cluster endpoint for writes, reader endpoint for reads

#### 5. Network ACLs
- Check subnet NACL allows traffic on database port
- Both inbound and outbound rules needed (stateless)

#### 6. SSL/TLS Requirements
- Some databases require SSL connections
- Download RDS certificate bundle
- Configure application to use SSL

#### 7. Connection Limits
- RDS has maximum connections based on instance class
- MySQL: `{DBInstanceClassMemory/12582880}`
- Check CloudWatch `DatabaseConnections` metric
- If maxed out, scale up instance or optimize connection pooling

**Testing Connection**:
```bash
# From EC2 instance in same VPC
# MySQL
mysql -h mydb.abc123.us-east-1.rds.amazonaws.com -P 3306 -u admin -p

# PostgreSQL
psql -h mydb.abc123.us-east-1.rds.amazonaws.com -p 5432 -U admin -d mydb

# Test connectivity
telnet mydb.abc123.us-east-1.rds.amazonaws.com 3306
```

**Common Solutions**:
- Add application security group to RDS security group inbound rules
- Enable public accessibility (for testing only, not production)
- Check VPC routing and internet gateway configuration
- Verify database is in same VPC as application
- Use AWS Systems Manager Session Manager to connect to EC2, then test RDS connection

---

### CloudFormation Stack Failures

**Symptoms**: CloudFormation stack creation or update fails, rolls back

**Common Failure Reasons**:

#### 1. Insufficient IAM Permissions
**Error**: `User is not authorized to perform: [action]`

**Solutions**:
- User/role needs permissions for all resources being created
- CloudFormation also needs permissions via service role
- Add required permissions to IAM policy
- Use CloudFormation service role with necessary permissions

**Example IAM Policy**:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:*",
        "ec2:*",
        "iam:*",
        "s3:*"
      ],
      "Resource": "*"
    }
  ]
}
```

#### 2. Resource Limits Exceeded
**Error**: `LimitExceeded` or `ResourceLimitExceeded`

**Solutions**:
- Check service quotas (VPCs, Elastic IPs, EC2 instances)
- Request limit increase via Service Quotas console
- Use existing resources instead of creating new ones
- Deploy to different region with available capacity

#### 3. Parameter Validation Errors
**Error**: `Parameters: [parameter] must match pattern [regex]`

**Solutions**:
- Verify parameter values match constraints
- Check AllowedValues, MinLength, MaxLength
- Ensure CIDR blocks don't overlap
- Validate AMI IDs exist in target region

#### 4. Resource Already Exists
**Error**: `Resource already exists`

**Solutions**:
- Delete existing resource or import it
- Use different resource names/logical IDs
- Check for resources from previous failed stacks
- Use DeletionPolicy: Retain to keep resources on stack deletion

#### 5. Circular Dependencies
**Error**: `Circular dependency between resources`

**Solutions**:
- Review DependsOn attributes
- Remove unnecessary dependencies
- Restructure template to break circular references
- Use nested stacks to separate dependent resources

#### 6. Insufficient Capacity
**Error**: `Insufficient capacity` for EC2 instances

**Solutions**:
- Try different availability zone
- Use different instance type
- Try multiple instance types with launch templates
- Deploy across multiple AZs

#### 7. Timeout Issues
**Error**: Resource creation timed out

**Solutions**:
- Increase CreationPolicy timeout
- Check resource is actually being created (CloudWatch Logs)
- For ASG, verify instances can reach metadata service
- Use cfn-signal from instance user data

**Troubleshooting Tools**:

**CloudFormation Events**:
- View stack events for detailed error messages
- Identify which resource failed
- Check status reason for failure cause

**Change Sets**:
- Preview changes before executing
- Identify resources that will be replaced
- Validate template before stack update

**Stack Drift Detection**:
- Detect if resources were manually modified
- Compare actual configuration vs. template
- Resolve drift before updating stack

**Template Validation**:
```bash
# Validate template syntax
aws cloudformation validate-template --template-body file://template.yaml

# Use cfn-lint for advanced validation
pip install cfn-lint
cfn-lint template.yaml
```

**Best Practices**:
1. Always validate templates before deployment
2. Use change sets for stack updates
3. Implement rollback triggers with CloudWatch alarms
4. Set appropriate timeouts for resource creation
5. Use DeletionPolicy: Retain for critical resources
6. Test templates in dev environment first
7. Use nested stacks for complex infrastructure
8. Enable termination protection for production stacks

---

### Auto Scaling Not Working

**Symptoms**: Auto Scaling Group not launching or terminating instances as expected

**Troubleshooting Steps**:

#### 1. Verify Scaling Policies

**Check Policy Configuration**:
- Target tracking vs. step scaling vs. simple scaling
- Metric being monitored (CPU, memory, custom)
- Target value or step adjustments
- Cooldown periods preventing rapid scaling

**Example Issue**:
- Target: 70% CPU utilization
- Current: 85% CPU
- But no scale-out occurring

**Solutions**:
- Check if in cooldown period (default 300 seconds)
- Verify CloudWatch alarm state is ALARM
- Check alarm has datapoints exceeding threshold
- Ensure policy is enabled

#### 2. Check Auto Scaling Group Configuration

**Capacity Limits**:
- Minimum capacity: Can't scale below this
- Maximum capacity: Can't scale above this
- Desired capacity: Current target

**Common Issue**: Max capacity reached
```
Current: 10 instances
Max capacity: 10
Result: Cannot scale out, even if CPU is high
```

**Solutions**:
- Increase max capacity
- Review if capacity limits are appropriate
- Check service quotas for EC2 instances

#### 3. Launch Template/Configuration Issues

**Invalid AMI**:
- AMI deleted or not available in region
- AMI shared from another account no longer accessible

**Insufficient IAM Permissions**:
- Instance profile missing required permissions
- Cannot access S3, Parameter Store, Secrets Manager

**Invalid User Data**:
- Syntax errors in user data script
- Script fails causing instance initialization to fail

**Solutions**:
- Check AMI exists: `aws ec2 describe-images --image-ids ami-xxx`
- Review CloudWatch Logs for user data script output
- Test launch template manually by launching instance
- Verify security groups and key pairs are valid

#### 4. Availability Zone Issues

**No Capacity**:
- EC2 capacity not available in specified AZs
- Only some AZs have capacity

**Solutions**:
- Distribute across multiple AZs
- Use multiple instance types (mixed instances policy)
- Enable capacity rebalancing

#### 5. Health Check Failures

**Instances Terminating Immediately**:
- Health check type: EC2 vs. ELB
- Health check grace period too short
- Instances failing health checks

**Symptoms**:
- Instances launch, then terminate repeatedly
- CloudWatch shows instances unhealthy

**Solutions**:
- Increase health check grace period (300-600 seconds)
- Fix application issues causing health check failures
- Verify ELB target group health check settings
- Check security groups allow health check traffic

#### 6. Service Quotas

**EC2 Instance Limits**:
- On-Demand vCPU limits
- Spot Instance limits
- Per-region limits

**Check Current Usage**:
```bash
# Check service quotas
aws service-quotas get-service-quota \
  --service-code ec2 \
  --quota-code L-1216C47A  # Running On-Demand Standard instances
```

**Solutions**:
- Request quota increase
- Use different instance types
- Deploy to different region

#### 7. Scaling Suspended

**Check Suspended Processes**:
- ReplaceUnhealthy
- Launch
- Terminate
- AddToLoadBalancer

**Resume Processes**:
```bash
aws autoscaling resume-processes \
  --auto-scaling-group-name my-asg
```

#### 8. CloudWatch Alarm Issues

**Alarm Not Triggering**:
- Insufficient data
- Metric not published
- Threshold not exceeded for required evaluation periods
- Alarm in INSUFFICIENT_DATA state

**Solutions**:
- Check CloudWatch metrics are being published
- Verify alarm configuration (threshold, periods)
- Review alarm history
- Test with lower threshold temporarily

**Debugging Commands**:
```bash
# Describe Auto Scaling Group
aws autoscaling describe-auto-scaling-groups \
  --auto-scaling-group-names my-asg

# View scaling activities
aws autoscaling describe-scaling-activities \
  --auto-scaling-group-name my-asg \
  --max-records 20

# Check scaling policies
aws autoscaling describe-policies \
  --auto-scaling-group-name my-asg

# View CloudWatch alarms
aws cloudwatch describe-alarms \
  --alarm-names my-cpu-alarm
```

**Common Solutions**:
1. Ensure min/max/desired capacity are appropriate
2. Verify CloudWatch alarms are in ALARM state
3. Check for suspended processes
4. Increase health check grace period
5. Fix launch template issues (AMI, security groups, user data)
6. Distribute across multiple AZs for availability
7. Use multiple instance types to improve capacity availability
8. Monitor with CloudWatch and set up alerting

---

### High AWS Bill Unexpectedly

**Symptoms**: AWS bill significantly higher than expected or usual

**Common Cost Culprits**:

#### 1. Untagged or Orphaned Resources

**EC2 Instances Running**:
- Instances left running after testing
- Auto Scaling not scaling down
- Spot requests creating instances

**Check**:
```bash
# List all running instances
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query 'Reservations[].Instances[].[InstanceId,InstanceType,Tags[?Key==`Name`].Value|[0]]'
```

**EBS Volumes**:
- Volumes detached from terminated instances
- Snapshots accumulating over time
- Volumes larger than needed

**Check**:
```bash
# Find unattached volumes
aws ec2 describe-volumes \
  --filters "Name=status,Values=available" \
  --query 'Volumes[].[VolumeId,Size,CreateTime]'
```

**Solutions**:
- Terminate unused EC2 instances
- Delete unattached EBS volumes
- Set up lifecycle policies for snapshots
- Use AWS Resource Groups Tag Editor to find untagged resources

#### 2. Data Transfer Costs

**Cross-Region Transfer**:
- Data transfer between regions ($0.02/GB)
- Not using same-region resources

**Internet Data Transfer**:
- Data transfer out to internet ($0.09/GB for first 10TB)
- Large file downloads
- Streaming video/audio

**Solutions**:
- Keep resources in same region
- Use CloudFront for content delivery (cheaper egress)
- Compress data before transfer
- Use VPC endpoints to avoid NAT Gateway data processing charges
- Review CloudFront, S3, and EC2 data transfer in Cost Explorer

#### 3. NAT Gateway Costs

**High Data Processing**:
- NAT Gateway charges for data processed ($0.045/GB)
- Instances in private subnets accessing internet frequently

**Solutions**:
- Use VPC endpoints for AWS services (S3, DynamoDB)
- Consolidate NAT Gateways (one per AZ sufficient)
- Review what traffic is going through NAT Gateway
- Consider switching to NAT instances for high-volume use cases

#### 4. CloudWatch Logs

**Large Log Ingestion**:
- Application logging too verbosely
- Retention period too long
- Many log groups

**Check Costs**:
- Ingestion: $0.50/GB
- Storage: $0.03/GB/month
- Insights queries: $0.005/GB scanned

**Solutions**:
- Reduce log verbosity
- Set retention policies (7-30 days typical)
- Export old logs to S3 (cheaper storage)
- Use sampling for high-volume logs
- Delete unnecessary log groups

#### 5. Elastic Load Balancers

**Idle Load Balancers**:
- Load balancer running with no traffic
- Using multiple load balancers when one suffices

**Costs**:
- ALB/NLB: ~$0.0225/hour (~$16/month) + data processing
- Classic LB: ~$0.025/hour (~$18/month)

**Solutions**:
- Delete unused load balancers
- Consolidate applications behind fewer load balancers
- Use path-based routing on ALB

#### 6. RDS Instances

**Over-Provisioned**:
- Database instance too large
- Multi-AZ when not needed for dev/test
- Not using Reserved Instances

**Solutions**:
- Right-size instance based on CloudWatch metrics
- Use Single-AZ for non-production
- Stop RDS instances when not in use (dev/test)
- Purchase Reserved Instances for production (save up to 72%)

#### 7. S3 Storage Costs

**Incorrect Storage Class**:
- Using Standard for infrequently accessed data
- Not using Intelligent-Tiering

**Many Small Objects**:
- S3 charges per request
- Millions of tiny files more expensive

**Solutions**:
- Use lifecycle policies to transition to cheaper storage classes
- Enable S3 Intelligent-Tiering for unknown access patterns
- Consolidate small objects
- Delete incomplete multipart uploads
- Use S3 Storage Lens for insights

#### 8. Lambda Costs

**High Invocations**:
- Infinite loop or recursive calls
- Too frequent CloudWatch Events triggers
- Over-allocated memory

**Solutions**:
- Review CloudWatch Logs for errors causing retries
- Optimize function execution time
- Right-size memory allocation
- Use reserved concurrency to limit costs
- Implement exponential backoff for retries

**Cost Analysis Tools**:

**AWS Cost Explorer**:
- View costs by service, region, tag
- Identify trends and anomalies
- Filter by time period

**AWS Budgets**:
- Set budget alerts
- Get notified when exceeding threshold
- Forecast spending

**AWS Cost Anomaly Detection**:
- ML-based anomaly detection
- Automatic alerts for unusual spending
- Root cause analysis

**AWS Trusted Advisor**:
- Cost optimization recommendations
- Identify idle resources
- Right-sizing suggestions (with Business/Enterprise support)

**Tag-Based Cost Allocation**:
- Tag resources by: Project, Environment, Owner
- Enable tag-based cost allocation reports
- Identify costs by business unit

**Investigation Steps**:

1. **Open Cost Explorer**:
   - Group by service
   - Identify top cost services
   - Compare to previous month

2. **Check for Anomalies**:
   - Look for sudden spikes
   - Identify specific days/hours

3. **Review Top Services**:
   - EC2: Running instances, EBS volumes
   - S3: Storage, requests, data transfer
   - Data Transfer: Cross-region, internet egress
   - RDS: Running databases

4. **Tag Analysis**:
   - Identify untagged resources
   - Track costs by project/team

5. **Enable Detailed Billing**:
   - Resource-level granularity
   - Understand what's driving costs

**Prevention**:
1. Set up AWS Budgets with email alerts
2. Tag all resources appropriately
3. Set up Cost Anomaly Detection
4. Review Cost Explorer monthly
5. Implement least-privilege IAM policies (prevent accidental expensive resource creation)
6. Use CloudFormation with budget constraints
7. Enable AWS Cost Optimization Hub
8. Regular cost review meetings with stakeholders

---

### API Gateway 502/504 Errors

**Symptoms**: API Gateway returns 502 Bad Gateway or 504 Gateway Timeout

**Error Types**:

#### 1. 502 Bad Gateway

**Causes**:
- Backend endpoint (Lambda, HTTP) returning invalid response
- Lambda function error/exception
- Malformed response from integration
- Certificate validation failure (for HTTP integration)

**Solutions**:

**Lambda Integration**:
- Check CloudWatch Logs for Lambda errors
- Ensure Lambda returns proper response format:
```json
{
  "statusCode": 200,
  "headers": {
    "Content-Type": "application/json"
  },
  "body": "{\"message\":\"Success\"}"
}
```
- Verify Lambda execution role has required permissions
- Check if Lambda is in VPC and can reach required resources

**HTTP Integration**:
- Verify backend endpoint is accessible
- Check SSL certificate is valid
- Test endpoint directly from EC2 in same VPC
- Verify security groups allow API Gateway to reach backend
- Check if using correct HTTP method

**VPC Link Issues**:
- Network Load Balancer health checks failing
- Security groups blocking traffic
- Target group has no healthy targets

#### 2. 504 Gateway Timeout

**Causes**:
- Backend taking longer than API Gateway timeout (29 seconds maximum)
- Lambda function timeout
- HTTP endpoint not responding
- Network connectivity issues

**Solutions**:

**Lambda Timeout**:
- Check Lambda timeout setting (max 15 minutes, but API Gateway limit is 29 seconds)
- Set Lambda timeout to <29 seconds for synchronous invocations
- For long-running tasks, use asynchronous invocation or Step Functions
- Optimize Lambda performance

**HTTP Endpoint Timeout**:
- Reduce backend processing time
- Implement caching at backend
- Use asynchronous processing for long operations
- Return immediate response, process in background

**VPC Configuration**:
- If Lambda in VPC, check it can reach endpoints (NAT Gateway for internet)
- Verify DNS resolution working
- Check VPC Flow Logs for dropped packets

#### 3. Integration Response Issues

**Invalid Response Transformation**:
- VTL (Velocity Template Language) mapping error
- Headers not properly formatted
- Response body invalid JSON

**Solutions**:
- Test mapping templates in API Gateway console
- Verify response structure matches defined model
- Check for syntax errors in VTL templates
- Enable CloudWatch logging for API Gateway

#### 4. Resource Policy or Authorization

**403 Forbidden disguised as 502**:
- Resource policy denying request
- Lambda authorizer denying access but not returning proper response

**Solutions**:
- Review resource policy
- Check Lambda authorizer CloudWatch Logs
- Ensure authorizer returns proper policy document

#### 5. Throttling

**TooManyRequestsException**:
- Account-level throttle (10,000 RPS default)
- Burst limit exceeded (5,000 default)
- Stage-level or method-level throttles

**Solutions**:
- Implement client-side retry with exponential backoff
- Request limit increase
- Use usage plans to control access
- Implement caching to reduce backend calls

**Debugging Steps**:

**1. Enable CloudWatch Logs**:
```bash
# Enable execution logging
aws apigateway update-stage \
  --rest-api-id abc123 \
  --stage-name prod \
  --patch-operations \
    op=replace,path=/*/logging/loglevel,value=INFO \
    op=replace,path=/*/logging/dataTrace,value=true
```

**2. Check CloudWatch Metrics**:
- 4XXError: Client errors
- 5XXError: Server errors
- IntegrationLatency: Backend response time
- Latency: Total request latency
- Count: Number of requests

**3. Test Endpoint**:
```bash
# Test API directly
curl -X POST https://api-id.execute-api.region.amazonaws.com/stage/path \
  -H "Content-Type: application/json" \
  -d '{"key":"value"}' \
  -v

# Check for specific error codes
# 502: Bad Gateway (integration error)
# 504: Gateway Timeout (backend timeout)
```

**4. Review Lambda Logs** (if Lambda integration):
```bash
# Get latest log stream
aws logs describe-log-streams \
  --log-group-name /aws/lambda/my-function \
  --order-by LastEventTime \
  --descending \
  --max-items 1

# View logs
aws logs tail /aws/lambda/my-function --follow
```

**5. Test Lambda Directly**:
```bash
# Invoke Lambda with test event
aws lambda invoke \
  --function-name my-function \
  --payload '{"key":"value"}' \
  response.json
```

**Common Solutions**:

1. **Lambda Response Format**:
   - Use proxy integration for simple cases
   - Ensure statusCode, headers, body are properly formatted
   - Stringify JSON body

2. **Timeout Configuration**:
   - Lambda timeout: <29 seconds
   - Use async invocation for long-running tasks
   - Implement caching

3. **VPC Configuration**:
   - Add NAT Gateway for internet access
   - Use VPC endpoints for AWS services
   - Verify security groups

4. **Error Handling**:
   - Implement try-catch in Lambda
   - Return proper error responses
   - Log errors to CloudWatch

5. **Monitoring**:
   - Set up CloudWatch alarms for 5XX errors
   - Enable X-Ray tracing for detailed analysis
   - Regular review of CloudWatch Logs

**Best Practices**:
1. Always enable CloudWatch Logs (at least for errors)
2. Implement proper error handling in backend
3. Set appropriate timeouts (Lambda < 29 seconds)
4. Use X-Ray for distributed tracing
5. Test API thoroughly before production
6. Monitor latency and error rates
7. Implement caching to reduce backend load
8. Use usage plans to control access and prevent abuse

---

## Key Takeaways

1. **Cost Optimization**: Combine Reserved Instances, Spot Instances, and On-Demand based on workload patterns
2. **High Availability**: Always design across multiple Availability Zones
3. **Data Migration**: Use AWS physical devices (Snowball/Snowmobile) for large datasets
4. **Serverless**: Ideal for unpredictable workloads and minimal operational overhead
5. **Compliance**: Use AWS Organizations, SCPs, and AWS Config for governance at scale
6. **Disaster Recovery**: Choose DR strategy based on RPO/RTO requirements and budget
7. **Hybrid Connectivity**: Direct Connect for production, VPN for dev/test
8. **Multi-Region**: Use Global Tables, CloudFront, and Route 53 for low-latency global access
9. **Security**: Implement defense in depth with GuardDuty, Security Hub, Config, and automated remediation
10. **Modernization**: Use strangler fig pattern for incremental migration from monoliths
11. **Big Data**: Build data lakes with S3, process with Glue/EMR, analyze with Athena/Redshift
12. **CI/CD**: Automate deployments with CodePipeline, implement blue/green deployments, enable fast rollbacks
13. **Troubleshooting**: Follow systematic approaches for connectivity, security, and performance issues
14. **Cost Management**: Use Cost Explorer, Budgets, and tagging to track and optimize spending

---

[← Previous: Exam Preparation](08-exam-preparation.md) | [Next: Additional Resources →](10-additional-resources.md)
