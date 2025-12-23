# Domain 2: Security and Compliance (30%)

[← Previous: Cloud Concepts](02-cloud-concepts.md) | [Back to Main](README.md) | [Next: Cloud Technology and Services →](04-technology-services.md)

---

## AWS Shared Responsibility Model

> **EXAM CRITICAL:** This is one of the most critical concepts for the exam. Understand what AWS manages versus what the customer manages.

The Shared Responsibility Model divides security responsibilities between AWS and the customer. Think of it as "Security OF the Cloud" (AWS) vs "Security IN the Cloud" (Customer).

### AWS Responsibility: Security OF the Cloud

AWS is responsible for protecting the infrastructure that runs all services:

- **Physical security of data centers**
  - Building access controls
  - Security personnel
  - Environmental safeguards

- **Hardware and networking components**
  - Physical servers
  - Storage devices
  - Network equipment

- **Compute, storage, database, and networking infrastructure**
  - Hypervisor layer
  - Managed service infrastructure

- **AWS global infrastructure**
  - Regions
  - Availability Zones
  - Edge Locations

- **Managed services**
  - RDS, DynamoDB, Lambda, etc.
  - AWS handles OS patching and maintenance for these services

### Customer Responsibility: Security IN the Cloud

Customers are responsible for:

- **Customer data**
  - All data you store in AWS
  - Classification and protection

- **Platform, applications, Identity and Access Management (IAM)**
  - Application code
  - IAM users, groups, roles, and policies

- **Operating system, network, and firewall configuration**
  - OS patches and updates (for EC2)
  - Security group rules
  - Network ACLs

- **Client-side data encryption and data integrity authentication**
  - Encrypting data before upload
  - Data validation

- **Server-side encryption (file system and/or data)**
  - Encryption at rest
  - Key management choices

- **Network traffic protection**
  - Encryption in transit (HTTPS, TLS)
  - Network security

- **Security group configuration**
  - Firewall rules
  - Access controls

- **User access management**
  - Creating and managing users
  - Password policies
  - MFA enforcement

### Shared Controls

Both AWS and customers have responsibilities for:

| Control | AWS Responsibility | Customer Responsibility |
|---------|-------------------|------------------------|
| **Patch Management** | Patches infrastructure components | Patches guest OS and applications |
| **Configuration Management** | Configures infrastructure devices | Configures databases and applications |
| **Awareness and Training** | Trains AWS employees | Trains their own staff |

> **Exam Tip:** For any security question, ask: "Who is responsible?" AWS handles the infrastructure; you handle what you put in the cloud.

---

## AWS Identity and Access Management (IAM)

IAM enables you to securely control access to AWS services and resources. It's a **global service** (not region-specific) and is **free** to use.

### Core Components

#### Users

- **Individual people or services**
- Permanent named operators
- Can have long-term credentials:
  - Password (for console access)
  - Access keys (for programmatic access)
- Should represent a physical person or application
- By default, new users have NO permissions

#### Groups

- **Collection of users**
- Simplifies permission management
- Key characteristics:
  - Groups cannot be nested
  - Users can belong to multiple groups
  - Apply policies to groups for easier management
  - No default groups

#### Roles

- **Temporary credentials** for users, applications, or services
- No username/password or access keys
- Can be assumed by anyone who needs it
- **Best practice** for EC2 instances accessing AWS services
- Can be used for cross-account access
- Temporary security credentials are automatically rotated

#### Policies

- **JSON documents** defining permissions
- Attached to users, groups, or roles
- Define what actions are allowed or denied on which resources
- Follow **principle of least privilege**
- Two main types:
  - **AWS Managed Policies:** Created and maintained by AWS
  - **Customer Managed Policies:** Created and maintained by you

**Example Policy Structure:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### Detailed IAM Policy Examples

Understanding IAM policies is critical for the exam. Here are common policy scenarios you should know.

#### Example 1: S3 Read-Only Access to Specific Bucket

This policy grants read-only access to objects in a specific S3 bucket.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3ReadOnlyAccess",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:GetObjectVersion",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::company-reports/*",
        "arn:aws:s3:::company-reports"
      ]
    }
  ]
}
```

**Key Points:**
- `Sid`: Statement ID (optional, for documentation)
- `Effect`: Can be "Allow" or "Deny"
- `Action`: What operations are permitted
- `Resource`: Which AWS resources the policy applies to

#### Example 2: EC2 Instance Management with Conditions

This policy allows starting and stopping EC2 instances only during business hours.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances",
        "ec2:DescribeInstances"
      ],
      "Resource": "*",
      "Condition": {
        "DateGreaterThan": {
          "aws:CurrentTime": "2024-01-01T08:00:00Z"
        },
        "DateLessThan": {
          "aws:CurrentTime": "2024-12-31T18:00:00Z"
        },
        "IpAddress": {
          "aws:SourceIp": [
            "203.0.113.0/24",
            "198.51.100.0/24"
          ]
        }
      }
    }
  ]
}
```

**Condition Elements:**
- Time-based restrictions
- IP address restrictions
- MFA requirements
- Source VPC restrictions

#### Example 3: Deny Policy for Sensitive Actions

This policy explicitly denies deletion of production resources (deny always overrides allow).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "rds:DeleteDBInstance",
        "ec2:TerminateInstances",
        "s3:DeleteBucket"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Environment": "Production"
        }
      }
    }
  ]
}
```

**Important:** Explicit Deny always wins over Allow in IAM policy evaluation.

#### Example 4: MFA-Required Policy

This policy requires MFA for sensitive operations.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:TerminateInstances",
        "rds:DeleteDBInstance"
      ],
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "true"
        }
      }
    }
  ]
}
```

#### Example 5: Cross-Account Access Policy

This policy allows assuming a role from another AWS account.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "UniqueExternalId123"
        }
      }
    }
  ]
}
```

**Use Case:** Allow users from Account A to access resources in Account B.

#### Example 6: Full Administrator Access

This policy grants full access to all AWS services (use with extreme caution).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

**Warning:** Only assign to trusted administrators. This is equivalent to root access.

#### Example 7: Read-Only Access Across All Services

This policy provides read-only access for auditing purposes.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "s3:Get*",
        "s3:List*",
        "rds:Describe*",
        "cloudwatch:Get*",
        "cloudwatch:List*",
        "cloudtrail:LookupEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

#### Example 8: S3 Bucket Policy for Public Read Access

This is a resource-based policy attached to an S3 bucket.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-public-website/*"
    }
  ]
}
```

**Use Case:** Hosting a static website or public content.

#### Example 9: Service Control Policy (SCP)

This SCP prevents anyone in an OU from leaving the organization.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "organizations:LeaveOrganization"
      ],
      "Resource": "*"
    }
  ]
}
```

**Important:** SCPs affect all users and roles, including the account root user.

#### Example 10: Tag-Based Access Control

This policy allows actions only on resources with specific tags.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Owner": "${aws:username}",
          "aws:ResourceTag/Department": "Engineering"
        }
      }
    }
  ]
}
```

**Use Case:** Users can only manage their own resources in their department.

### IAM Best Practices

1. **Root account protection**
   - Use only for initial setup, then lock it away
   - Enable MFA on root account
   - Do not create access keys for root
   - Create individual IAM users instead

2. **Principle of Least Privilege**
   - Grant only the permissions required to perform a task
   - Start with minimum permissions and add as needed
   - Regularly review and remove unnecessary permissions

3. **Use Groups for permission management**
   - Assign permissions to groups, not individual users
   - Add users to appropriate groups
   - Easier to manage and audit

4. **Enable MFA (Multi-Factor Authentication)**
   - Especially for privileged users
   - Required for root account
   - Add extra layer of security

5. **Use Roles for applications**
   - For applications running on EC2
   - Better than embedding credentials
   - Automatic credential rotation

6. **Rotate Credentials regularly**
   - Change passwords periodically
   - Rotate access keys
   - Set password expiration policies

7. **Remove Unnecessary Credentials**
   - Delete unused users
   - Remove unused roles
   - Deactivate old access keys

8. **Use Policy Conditions for extra security**
   - IP address restrictions
   - Time-based access
   - MFA requirements
   - Source VPC restrictions

---

## Security Best Practices - Deep Dive

Comprehensive security best practices you must know for the exam and real-world AWS usage.

### 1. Identity and Access Management Security

#### Implement Least Privilege Access

**What it means:** Grant only the permissions necessary to perform required tasks, nothing more.

**How to implement:**
- Start with zero permissions and add what's needed
- Use AWS managed policies as a starting point, then customize
- Regularly review and audit permissions
- Use IAM Access Analyzer to identify unused permissions
- Remove permissions that haven't been used in 90+ days

**Example Scenario:** A developer needs to read application logs from S3. Give them `s3:GetObject` on the specific bucket, not full S3 access or administrator rights.

**Exam Tip:** Questions will test whether you can identify overly permissive policies.

#### Implement Strong Password Policies

**Requirements:**
- Minimum length: 14+ characters (AWS allows 8-128)
- Require uppercase, lowercase, numbers, and symbols
- Prevent password reuse (remember at least 24 previous passwords)
- Enforce password expiration (60-90 days recommended)
- Prevent users from changing their password too frequently

**AWS Password Policy Settings:**
```
- Minimum password length: 14 characters
- Require at least one uppercase letter: Yes
- Require at least one lowercase letter: Yes
- Require at least one number: Yes
- Require at least one non-alphanumeric character: Yes
- Allow users to change their own password: Yes
- Enable password expiration: Yes (90 days)
- Password expiration requires administrator reset: No
- Number of passwords to remember: 24
```

#### Credential Management Best Practices

**Never do this:**
- Hard-code credentials in application code
- Store credentials in version control (Git)
- Share credentials between users or applications
- Email or message credentials
- Use long-term credentials when temporary ones are available

**Always do this:**
- Use IAM roles for EC2 instances and Lambda functions
- Use AWS Secrets Manager or Systems Manager Parameter Store for secrets
- Rotate credentials regularly (access keys every 90 days)
- Use temporary credentials via AWS STS
- Delete unused credentials immediately

#### Enable AWS CloudTrail in All Regions

**Why it's critical:**
- Provides audit trail of all API calls
- Helps with compliance requirements
- Enables security analysis and troubleshooting
- Detects unauthorized access attempts
- Required for incident response

**Configuration:**
- Enable in all regions (even ones you don't use)
- Enable log file validation for integrity
- Store logs in a separate, secured S3 bucket
- Enable S3 bucket versioning for log storage
- Restrict access to CloudTrail logs
- Set up CloudWatch Logs integration for real-time monitoring

### 2. Network Security Best Practices

#### Use Security Groups Properly

**Key Principles:**
- Security groups are stateful (return traffic automatically allowed)
- Default deny: Allow only what's needed
- Use descriptive names and tags
- Reference other security groups instead of IP addresses when possible
- Separate security groups by tier (web, application, database)

**Common Patterns:**

**Web Tier Security Group:**
```
Inbound:
- Port 80 (HTTP) from 0.0.0.0/0
- Port 443 (HTTPS) from 0.0.0.0/0
- Port 22 (SSH) from Bastion-SG only

Outbound:
- All traffic (default)
```

**Application Tier Security Group:**
```
Inbound:
- Port 8080 from Web-Tier-SG
- Port 22 from Bastion-SG only

Outbound:
- Port 3306 to Database-SG
- Port 443 to 0.0.0.0/0 (for API calls)
```

**Database Tier Security Group:**
```
Inbound:
- Port 3306 (MySQL) from App-Tier-SG only
- Port 22 from Bastion-SG only

Outbound:
- None (most restrictive)
```

#### Network ACL (NACL) Best Practices

**Differences from Security Groups:**
- Stateless (must allow return traffic explicitly)
- Applies at subnet level
- Rules are processed in numerical order
- Can have explicit DENY rules

**When to use NACLs:**
- Block specific IP addresses (security groups can't deny)
- Add an additional layer of defense
- Comply with regulatory requirements for network segmentation

**Best Practice:**
- Use security groups as primary defense
- Use NACLs for additional subnet-level protection
- Leave room between rule numbers (100, 200, 300) for insertions
- Document all custom NACL rules

#### Implement VPC Flow Logs

**What they capture:**
- Accepted and rejected traffic
- Source and destination IP addresses
- Ports and protocols
- Packet and byte counts

**Use cases:**
- Troubleshoot connectivity issues
- Monitor traffic patterns
- Detect anomalous behavior
- Meet compliance requirements
- Security forensics

**Configuration:**
- Enable at VPC, subnet, or ENI level
- Publish to CloudWatch Logs or S3
- Use for analysis with Amazon Athena
- Integrate with security tools

### 3. Data Protection Best Practices

#### Encrypt Data at Rest

**Services with encryption:**
- S3: SSE-S3, SSE-KMS, SSE-C
- EBS: Encrypted volumes
- RDS: Encrypted databases
- DynamoDB: Encryption at rest
- Redshift: Encrypted clusters

**Best practices:**
- Enable encryption by default for all new resources
- Use AWS KMS for key management
- Implement automatic key rotation
- Separate keys for different data classifications
- Grant minimal permissions to decrypt

#### Encrypt Data in Transit

**How to implement:**
- Use HTTPS/TLS for all web traffic
- Use SSL/TLS for database connections
- Use VPN or Direct Connect for hybrid connectivity
- Enable encryption for all data transfers
- Use AWS Certificate Manager for SSL/TLS certificates

**Services that enforce encryption in transit:**
- CloudFront (can require HTTPS)
- API Gateway (HTTPS only)
- Application Load Balancer (SSL/TLS termination)
- S3 Transfer Acceleration (HTTPS)

#### Implement Backup and Recovery

**Best practices:**
- Enable automated backups for databases
- Use AWS Backup for centralized backup management
- Store backups in different region for disaster recovery
- Test restore procedures regularly
- Implement versioning for S3 objects
- Use lifecycle policies to manage backup retention

### 4. Monitoring and Logging Best Practices

#### Implement Comprehensive Logging

**Essential logs to enable:**
- CloudTrail: API activity
- VPC Flow Logs: Network traffic
- S3 Server Access Logs: S3 bucket access
- ELB Access Logs: Load balancer requests
- CloudFront Access Logs: CDN requests
- RDS Logs: Database queries and errors

**Log management:**
- Centralize logs in a dedicated account
- Enable log file integrity validation
- Implement log retention policies
- Protect logs from deletion or modification
- Use CloudWatch Logs Insights for analysis

#### Set Up Security Alerts

**Critical alerts to configure:**
- Root account usage
- IAM policy changes
- Security group changes
- Network ACL changes
- Failed login attempts (multiple)
- Unauthorized API calls
- Changes to CloudTrail configuration
- S3 bucket policy changes
- Encryption key deletions

**Alerting mechanisms:**
- CloudWatch Alarms
- SNS notifications
- EventBridge rules
- GuardDuty findings
- Security Hub alerts

### 5. Compliance and Governance Best Practices

#### Implement Automated Compliance Checking

**Tools to use:**
- AWS Config Rules for continuous compliance
- AWS Security Hub for centralized security view
- AWS Systems Manager for patch compliance
- Trusted Advisor for best practice checks

**Common compliance rules:**
- Ensure S3 buckets are not publicly accessible
- Ensure encryption is enabled on all volumes
- Ensure MFA is enabled for root account
- Ensure CloudTrail is enabled in all regions
- Ensure unused IAM credentials are removed

#### Tag Everything for Governance

**Essential tags:**
- Environment (Production, Staging, Dev)
- Owner (team or individual)
- Cost Center (for billing)
- Project (application or project name)
- Compliance (required compliance programs)
- Data Classification (Public, Internal, Confidential)

**Benefits:**
- Cost allocation and tracking
- Automated policy enforcement
- Resource organization
- Compliance reporting
- Lifecycle management

### 6. Incident Response Best Practices

#### Prepare for Security Incidents

**Have a plan:**
- Document incident response procedures
- Define roles and responsibilities
- Maintain contact lists
- Establish communication channels
- Practice with simulation exercises

**AWS tools for incident response:**
- CloudWatch for monitoring and alerts
- CloudTrail for forensic analysis
- VPC Flow Logs for network analysis
- AWS Systems Manager for automated remediation
- EC2 snapshot for forensic investigation

**Isolation procedures:**
- Change security group to deny all traffic
- Snapshot affected resources before investigation
- Isolate in separate VPC or subnet
- Preserve logs and evidence
- Follow chain of custody procedures

### 7. Application Security Best Practices

#### Implement Defense in Depth

**Multiple layers of security:**
1. Edge security: CloudFront, AWS WAF, Shield
2. Network security: VPC, Security Groups, NACLs
3. Application security: IAM roles, encryption
4. Data security: Encryption at rest, access controls
5. Monitoring: CloudTrail, GuardDuty, CloudWatch

**Benefits:**
- No single point of failure
- Multiple chances to detect and stop attacks
- Reduces blast radius of breaches
- Compliance requirement for many frameworks

#### Secure API Endpoints

**API Gateway security:**
- Use API keys for identification
- Implement throttling and rate limiting
- Enable AWS WAF for protection
- Use Lambda authorizers for custom authentication
- Implement request validation
- Enable CloudWatch Logs for monitoring

**Best practices:**
- Use HTTPS only
- Implement proper authentication and authorization
- Validate all inputs
- Use least privilege for Lambda execution roles
- Enable CORS correctly (don't use *)
- Implement API versioning

### 8. Third-Party Security Best Practices

#### Manage Third-Party Access Securely

**Use external IDs for cross-account access:**
- Prevents confused deputy problem
- Unique identifier per customer
- Include in AssumeRole policy condition

**Best practices:**
- Use IAM roles instead of sharing credentials
- Implement least privilege access
- Require MFA for sensitive operations
- Monitor third-party access with CloudTrail
- Regularly audit and review access
- Remove access when no longer needed

#### Secure Container and Serverless Workloads

**Container security:**
- Scan images for vulnerabilities (Amazon ECR scanning)
- Use minimal base images
- Don't run containers as root
- Implement least privilege for task roles
- Use secrets management for credentials
- Enable CloudTrail logging for ECR

**Lambda security:**
- Use separate execution roles per function
- Store secrets in Secrets Manager or Parameter Store
- Enable VPC access only when needed
- Implement function-level encryption
- Use environment variables for configuration
- Monitor with CloudWatch and X-Ray

### Multi-Factor Authentication (MFA)

MFA adds an extra layer of protection beyond username and password.

**Authentication Factors:**
- **Something you know:** Password
- **Something you have:** MFA device

**MFA Device Options:**

| Type | Description | Use Case |
|------|-------------|----------|
| **Virtual MFA Device** | Mobile app (Google Authenticator, Authy) | Most common, convenient |
| **Hardware MFA Device** | Physical token (YubiKey) | High security environments |
| **SMS Text Message** | Code sent via SMS | Not recommended for root account |

> **Best Practice:** Always enable MFA on the root account and for all users with console access, especially those with administrative privileges.

---

## Data Encryption Best Practices

### Encryption at Rest

Encryption at rest protects data stored on disk from unauthorized access.

#### Amazon S3 Encryption Options

**Server-Side Encryption with S3-Managed Keys (SSE-S3):**
- AWS manages encryption keys
- AES-256 encryption
- Enabled with one click
- No additional cost
- Each object encrypted with unique key
- Best for: Simple encryption requirements

**Server-Side Encryption with KMS (SSE-KMS):**
- AWS KMS manages encryption keys
- You control key policies and rotation
- Audit trail via CloudTrail
- Additional cost per request
- Envelope encryption for large files
- Best for: Compliance requirements, audit needs

**Server-Side Encryption with Customer-Provided Keys (SSE-C):**
- You manage encryption keys outside AWS
- AWS performs encryption but doesn't store keys
- You must provide key with each request
- Best for: When you must control keys outside AWS

**Client-Side Encryption:**
- Encrypt data before uploading to S3
- You manage entire encryption process
- AWS stores encrypted data
- Best for: Maximum control over encryption

**Configuration Example:**
```json
{
  "Rules": [
    {
      "ApplyServerSideEncryptionByDefault": {
        "SSEAlgorithm": "aws:kms",
        "KMSMasterKeyID": "arn:aws:kms:region:account:key/key-id"
      },
      "BucketKeyEnabled": true
    }
  ]
}
```

#### EBS Encryption

**Features:**
- Encrypts data at rest inside volume
- Encrypts data in transit between instance and volume
- Encrypts all snapshots created from volume
- Uses AWS KMS for key management
- Minimal performance impact
- Can't encrypt root volume of existing instance (must create AMI)

**How to enable:**
- Enable during volume creation
- Enable account-level encryption by default
- Copy unencrypted snapshot and enable encryption
- Create encrypted AMI from unencrypted instance

#### RDS Encryption

**What gets encrypted:**
- Database storage
- Automated backups
- Read replicas
- Snapshots
- Logs

**Important notes:**
- Must enable at database creation time
- Cannot encrypt existing unencrypted database
- Workaround: Create snapshot, copy with encryption, restore
- Same key used for instance and snapshots in same region
- Cross-region snapshots use different key

#### DynamoDB Encryption

**Features:**
- Encryption at rest enabled by default
- Uses AWS owned keys (default, no cost)
- Can use AWS managed key (aws/dynamodb)
- Can use customer managed KMS key
- Encrypts tables, indexes, streams, backups

**Encryption types:**
- AWS owned CMK: Default, no cost, no CloudTrail logs
- AWS managed CMK: Free, CloudTrail logs available
- Customer managed CMK: You control, costs apply, full audit trail

### Encryption in Transit

Encryption in transit protects data moving between systems.

#### TLS/SSL Best Practices

**Use TLS 1.2 or higher:**
- TLS 1.0 and 1.1 are deprecated
- Configure minimum TLS version
- Use strong cipher suites
- Regularly update SSL/TLS certificates

**AWS Certificate Manager (ACM):**
- Free SSL/TLS certificates
- Automatic renewal
- Integration with CloudFront, ALB, API Gateway
- Easy deployment
- No certificate management overhead

**Use cases:**
- HTTPS for websites (CloudFront, ALB)
- Secure API endpoints (API Gateway)
- Database connections (RDS with SSL)
- Email encryption (SES)

#### VPN Encryption

**AWS Site-to-Site VPN:**
- IPsec VPN connection
- Encrypted tunnel over internet
- Uses Internet Key Exchange (IKE)
- Supports multiple encryption algorithms
- Dead Peer Detection for availability

**AWS Client VPN:**
- Managed client-based VPN
- OpenVPN-based
- TLS encryption
- Integration with Active Directory
- Multi-factor authentication support

#### Direct Connect Encryption

**MACsec for Direct Connect:**
- Layer 2 encryption
- Point-to-point encryption
- 10 Gbps and 100 Gbps connections
- Minimal latency impact

**VPN over Direct Connect:**
- IPsec VPN over DX connection
- End-to-end encryption
- Combines DX reliability with VPN security
- Industry-standard encryption

### Key Management with AWS KMS

#### KMS Key Types

**Symmetric Keys (default):**
- Same key for encryption and decryption
- 256-bit keys
- Never leaves KMS unencrypted
- Used for most AWS services
- Envelope encryption for large data

**Asymmetric Keys:**
- Public and private key pair
- RSA or Elliptic Curve keys
- Public key can be downloaded
- Private key never leaves KMS
- Used for signing and verification

#### Customer Master Keys (CMKs)

**AWS Managed CMK:**
- Created and managed by AWS
- Used by AWS services
- Automatic rotation every year
- Cannot delete
- No cost for the key (only usage)
- Key alias: aws/service-name

**Customer Managed CMK:**
- You create and manage
- Full control over key policies
- Optional automatic rotation (annual)
- Can enable/disable
- Can delete (with 7-30 day waiting period)
- Cost: $1/month plus usage

**AWS Owned CMK:**
- AWS owns and manages
- Used across multiple accounts
- No visibility or control
- No cost
- No CloudTrail logs

#### KMS Key Policies

**Default key policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Enable IAM policies",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    }
  ]
}
```

**Custom key policy with specific permissions:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Allow encryption",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:role/EncryptionRole"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:GenerateDataKey"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow key management",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/KeyAdmin"
      },
      "Action": [
        "kms:Create*",
        "kms:Describe*",
        "kms:Enable*",
        "kms:List*",
        "kms:Put*",
        "kms:Update*",
        "kms:Revoke*",
        "kms:Disable*",
        "kms:Get*",
        "kms:Delete*",
        "kms:ScheduleKeyDeletion",
        "kms:CancelKeyDeletion"
      ],
      "Resource": "*"
    }
  ]
}
```

#### Key Rotation Best Practices

**Automatic rotation:**
- Enable for customer managed keys
- Rotates every 365 days
- Old key versions retained for decryption
- Transparent to applications
- No need to re-encrypt data

**Manual rotation:**
- Create new CMK
- Update applications to use new key
- Re-encrypt data with new key
- Maintain old key for decrypting old data
- More control but more complex

---

## Network Security Deep Dive

### VPC Security Architecture

#### Multi-Tier Architecture Example

**Public Subnet (DMZ):**
- Internet Gateway attached
- Public IP addresses
- Bastion hosts / Jump boxes
- NAT Gateways
- Load balancers
- Route to Internet Gateway

**Private Subnet (Application Tier):**
- No direct internet access
- EC2 instances for applications
- Auto Scaling groups
- Route to NAT Gateway for outbound
- Access via load balancer only

**Private Subnet (Database Tier):**
- Most restrictive security
- RDS, DynamoDB endpoints
- No internet access (inbound or outbound)
- Access from application tier only
- Multi-AZ for high availability

#### Network Segmentation Best Practices

**Subnet Strategy:**
- Separate subnets by tier (web, app, data)
- Separate subnets by environment (prod, staging, dev)
- Separate subnets by compliance requirements
- Use at least 2 AZs for high availability
- Plan IP address ranges carefully

**Example CIDR allocation:**
```
VPC: 10.0.0.0/16

Availability Zone A:
- Public Subnet:  10.0.1.0/24
- Private Subnet: 10.0.2.0/24
- Data Subnet:    10.0.3.0/24

Availability Zone B:
- Public Subnet:  10.0.11.0/24
- Private Subnet: 10.0.12.0/24
- Data Subnet:    10.0.13.0/24
```

#### VPC Endpoints for Security

**Interface Endpoints (PrivateLink):**
- Private IP addresses in your VPC
- Elastic Network Interface (ENI)
- Supports many AWS services
- No internet gateway needed
- Charged per hour + data processed

**Gateway Endpoints:**
- Route table entry
- Free of charge
- Supports S3 and DynamoDB
- No ENI required
- Scalable

**Benefits:**
- Keep traffic within AWS network
- No internet exposure
- Better performance
- Lower data transfer costs
- Enhanced security

**Example use case:**
```
S3 Gateway Endpoint:
- Application accesses S3 privately
- No internet gateway required
- No NAT gateway charges
- Traffic stays on AWS network
```

#### Network Access Control

**Security Group Best Practices:**

**Layered security groups:**
```
ALB Security Group:
- Inbound: 80, 443 from 0.0.0.0/0
- Outbound: 8080 to App-SG

App Security Group:
- Inbound: 8080 from ALB-SG
- Outbound: 3306 to DB-SG, 443 to 0.0.0.0/0

DB Security Group:
- Inbound: 3306 from App-SG
- Outbound: None
```

**NACL Configuration Example:**

**Public Subnet NACL:**
```
Inbound Rules:
100 - HTTP (80) - 0.0.0.0/0 - ALLOW
110 - HTTPS (443) - 0.0.0.0/0 - ALLOW
120 - SSH (22) - YOUR_IP/32 - ALLOW
130 - Ephemeral (1024-65535) - 0.0.0.0/0 - ALLOW
* - ALL - 0.0.0.0/0 - DENY

Outbound Rules:
100 - HTTP (80) - 0.0.0.0/0 - ALLOW
110 - HTTPS (443) - 0.0.0.0/0 - ALLOW
120 - Ephemeral (1024-65535) - 0.0.0.0/0 - ALLOW
* - ALL - 0.0.0.0/0 - DENY
```

**Private Subnet NACL:**
```
Inbound Rules:
100 - Custom (8080) - 10.0.1.0/24 - ALLOW
110 - SSH (22) - 10.0.1.0/24 - ALLOW
120 - Ephemeral (1024-65535) - 0.0.0.0/0 - ALLOW
* - ALL - 0.0.0.0/0 - DENY

Outbound Rules:
100 - HTTPS (443) - 0.0.0.0/0 - ALLOW
110 - MySQL (3306) - 10.0.3.0/24 - ALLOW
120 - Ephemeral (1024-65535) - 10.0.1.0/24 - ALLOW
* - ALL - 0.0.0.0/0 - DENY
```

#### AWS PrivateLink

**What it is:**
- Private connectivity to services
- No internet gateway, NAT, VPN
- Traffic stays on AWS network
- Powered by interface VPC endpoints

**Use cases:**
- Access SaaS applications privately
- Share services across VPCs
- Hybrid cloud connectivity
- Compliance requirements

**Architecture:**
```
Service Provider VPC (Your Service)
    ↓
Network Load Balancer
    ↓
VPC Endpoint Service
    ↓
Interface Endpoint (Consumer VPC)
    ↓
Consumer Application
```

#### VPN and Direct Connect Security

**Site-to-Site VPN Security:**
- IPsec encryption
- Pre-shared keys or certificates
- Perfect Forward Secrecy (PFS)
- Dead Peer Detection
- IKEv2 support

**VPN Configuration Best Practices:**
- Use strong encryption (AES-256)
- Enable Perfect Forward Secrecy
- Configure health checks
- Use BGP for dynamic routing
- Monitor tunnel status

**Direct Connect Security:**
- Dedicated network connection
- Not encrypted by default
- Options for encryption:
  - MACsec (Layer 2)
  - VPN over DX (Layer 3)
  - Application-level encryption
- Physical security at co-location
- Redundancy with multiple connections

---

## Identity Federation and SSO

### AWS IAM Identity Center (formerly AWS SSO)

**What it provides:**
- Single sign-on to multiple AWS accounts
- Single sign-on to business applications
- Centralized user management
- Multi-factor authentication
- Integration with external identity providers

**Key Features:**
- One set of credentials for all accounts
- Temporary credentials for AWS access
- Built-in MFA support
- Integration with AWS Organizations
- Permission sets for access control

**Setup Process:**

1. Enable IAM Identity Center
2. Connect identity source (built-in directory or external)
3. Create permission sets
4. Assign users to AWS accounts
5. Users access via SSO portal

**Permission Set Example:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "s3:List*",
        "cloudwatch:Get*"
      ],
      "Resource": "*"
    }
  ]
}
```

### Federation with SAML 2.0

**SAML Federation Architecture:**
```
User → Identity Provider (IdP) → AWS STS → Temporary Credentials → AWS Resources
```

**Identity Providers:**
- Microsoft Active Directory Federation Services (ADFS)
- Okta
- Azure AD
- Google Workspace
- Auth0
- OneLogin

**How it works:**
1. User authenticates with corporate IdP
2. IdP returns SAML assertion
3. User presents SAML assertion to AWS STS
4. STS returns temporary security credentials
5. User accesses AWS resources

**Trust Relationship Policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::123456789012:saml-provider/MyIdP"
      },
      "Action": "sts:AssumeRoleWithSAML",
      "Condition": {
        "StringEquals": {
          "SAML:aud": "https://signin.aws.amazon.com/saml"
        }
      }
    }
  ]
}
```

### Web Identity Federation

**For mobile and web applications:**
- Users authenticate with Web IdP (Google, Facebook, Amazon)
- Application receives ID token
- Token exchanged for AWS credentials via STS
- Used with Amazon Cognito

**Amazon Cognito:**
- User pools for authentication
- Identity pools for AWS credentials
- Support for social identity providers
- Support for SAML providers
- Custom authentication flows

**Cognito Architecture:**
```
Mobile App → Cognito User Pool → Cognito Identity Pool → AWS STS → Temporary Credentials
```

**Benefits:**
- No AWS credentials in application
- Fine-grained access control
- Scales automatically
- Built-in security features

### Active Directory Integration

**AWS Directory Service Options:**

**AWS Managed Microsoft AD:**
- Full Microsoft AD in AWS cloud
- Multi-AZ deployment
- Patch and monitoring by AWS
- Trust relationships with on-premises AD
- Best for: Lift-and-shift scenarios

**AD Connector:**
- Proxy to on-premises AD
- No caching, always redirects to AD
- Users authenticate against on-premises AD
- No data stored in AWS
- Best for: Using existing on-premises AD

**Simple AD:**
- Standalone directory powered by Samba 4
- Basic AD features
- Small and large sizes
- Cannot join to on-premises AD
- Best for: Simple LDAP needs

### Cross-Account Access Strategies

**Method 1: IAM Roles (Recommended):**
```
Account A (Trusting) creates role
Account B (Trusted) assumes role
No credentials to manage
Temporary credentials only
```

**Trust Policy Example:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111122223333:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "UniqueSecretString"
        }
      }
    }
  ]
}
```

**Method 2: Resource-based Policies:**
- S3 bucket policies
- SNS topic policies
- SQS queue policies
- Lambda function policies

**Best Practices:**
- Always use IAM roles over shared credentials
- Use external IDs for third-party access
- Implement MFA for sensitive cross-account access
- Monitor with CloudTrail
- Use least privilege permissions

---

## Security Incident Response Procedures

### Incident Response Framework

#### 1. Preparation Phase

**Before an incident occurs:**

**Document procedures:**
- Create incident response plan
- Define severity levels
- Establish communication protocols
- Document escalation paths
- Identify team members and roles

**Setup tools and access:**
- Configure CloudTrail in all regions
- Enable VPC Flow Logs
- Setup GuardDuty
- Configure Security Hub
- Prepare forensics tools

**Establish baselines:**
- Normal traffic patterns
- Typical API usage
- Standard configurations
- Regular user behavior

**Training:**
- Regular tabletop exercises
- Simulate attack scenarios
- Test response procedures
- Update runbooks

#### 2. Detection and Analysis

**Detection methods:**
- GuardDuty findings
- CloudWatch alarms
- Security Hub alerts
- Config rule violations
- Unusual CloudTrail activity
- VPC Flow Log anomalies

**Initial analysis:**
- Confirm the incident is real (not false positive)
- Determine scope and severity
- Identify affected resources
- Document timeline
- Collect evidence

**Severity Classification:**

**Critical (P1):**
- Data breach confirmed
- Production systems compromised
- Ongoing active attack
- Wide-scale service disruption
- Response time: Immediate

**High (P2):**
- Suspected data access
- System compromise detected
- Compliance violation
- Response time: 1 hour

**Medium (P3):**
- Policy violations
- Suspicious activity detected
- Non-production compromise
- Response time: 4 hours

**Low (P4):**
- Security alerts to investigate
- Anomalous but benign activity
- Response time: 24 hours

#### 3. Containment Strategies

**Short-term containment:**

**Isolate compromised instances:**
```bash
# Change security group to deny all traffic
aws ec2 modify-instance-attribute \
  --instance-id i-1234567890abcdef0 \
  --groups sg-isolation-group
```

**Revoke compromised credentials:**
```bash
# Deactivate access key
aws iam update-access-key \
  --access-key-id AKIAIOSFODNN7EXAMPLE \
  --status Inactive \
  --user-name CompromisedUser
```

**Block malicious IP addresses:**
```bash
# Add NACL deny rule
aws ec2 create-network-acl-entry \
  --network-acl-id acl-12345678 \
  --ingress \
  --rule-number 50 \
  --protocol -1 \
  --port-range From=0,To=65535 \
  --cidr-block 198.51.100.5/32 \
  --rule-action deny
```

**Snapshot for forensics:**
```bash
# Create snapshot of compromised instance
aws ec2 create-snapshot \
  --volume-id vol-1234567890abcdef0 \
  --description "Forensic snapshot - Incident 2024-001"
```

**Long-term containment:**
- Patch vulnerabilities
- Change all passwords
- Rotate all access keys
- Update security group rules
- Apply least privilege policies
- Enable additional monitoring

#### 4. Eradication

**Remove the threat:**
- Delete malware
- Close backdoors
- Remove unauthorized access
- Patch vulnerabilities
- Update configurations

**Rebuild compromised systems:**
- Launch from known-good AMIs
- Apply all security patches
- Harden configurations
- Implement additional controls

**Verify clean state:**
- Scan for malware
- Review configurations
- Check for persistence mechanisms
- Validate logs show no malicious activity

#### 5. Recovery

**Restore operations:**
- Restore from clean backups
- Gradually bring systems online
- Monitor closely for reinfection
- Validate functionality

**Enhanced monitoring:**
- Increased logging verbosity
- More frequent reviews
- Additional alerting
- Closer scrutiny of anomalies

**Communication:**
- Update stakeholders
- Provide status reports
- Document changes made
- Coordinate with teams

#### 6. Post-Incident Activity

**Lessons learned meeting:**
- What happened?
- What was done?
- What worked well?
- What needs improvement?
- How to prevent recurrence?

**Update documentation:**
- Incident report
- Timeline of events
- Actions taken
- Evidence collected
- Lessons learned

**Improve defenses:**
- Implement preventive controls
- Update detection mechanisms
- Enhance response procedures
- Additional training
- Technology improvements

### AWS Tools for Incident Response

**Amazon GuardDuty:**
- Automated threat detection
- ML-powered analysis
- Continuous monitoring
- Integration with EventBridge for automated response

**AWS CloudTrail:**
- Complete audit log of API calls
- Who did what and when
- Source IP addresses
- Request parameters
- Essential for forensics

**VPC Flow Logs:**
- Network traffic analysis
- Source and destination IPs
- Identify scanning attempts
- Detect data exfiltration

**AWS Config:**
- Configuration history
- Compliance checking
- Resource relationships
- Change tracking

**Amazon Detective:**
- Analyze and investigate security findings
- Visualize relationships
- Identify root cause
- Integrated with GuardDuty

**AWS Systems Manager:**
- Automated remediation
- Patch management
- Run commands across fleet
- Session Manager for secure access

### Automated Response Examples

**Lambda function for isolation:**
```python
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')

    # Extract instance ID from GuardDuty finding
    instance_id = event['detail']['resource']['instanceDetails']['instanceId']

    # Change to isolation security group
    ec2.modify_instance_attribute(
        InstanceId=instance_id,
        Groups=['sg-isolation']
    )

    # Create forensic snapshot
    instance_details = ec2.describe_instances(InstanceIds=[instance_id])
    volume_id = instance_details['Reservations'][0]['Instances'][0]['BlockDeviceMappings'][0]['Ebs']['VolumeId']

    ec2.create_snapshot(
        VolumeId=volume_id,
        Description=f'Forensic snapshot for {instance_id}'
    )

    # Tag instance as compromised
    ec2.create_tags(
        Resources=[instance_id],
        Tags=[{'Key': 'SecurityStatus', 'Value': 'Isolated'}]
    )

    return {'statusCode': 200, 'body': f'Instance {instance_id} isolated'}
```

**EventBridge rule for GuardDuty findings:**
```json
{
  "source": ["aws.guardduty"],
  "detail-type": ["GuardDuty Finding"],
  "detail": {
    "severity": [7, 8, 9]
  }
}
```

### Communication Plan

**Notification hierarchy:**
1. Security team (immediate)
2. System administrators (immediate for high severity)
3. Management (within 1 hour for critical incidents)
4. Legal/compliance (for data breaches)
5. Customers (if required by regulations)

**Communication channels:**
- PagerDuty / Opsgenie for alerting
- Slack / Teams for coordination
- Email for formal notifications
- Status page for customer communication

---

## Security Services

### AWS Organizations - Detailed

Centrally manage and govern multiple AWS accounts.

**Key Features:**

- **Centrally manage multiple AWS accounts**
  - Single pane of glass for all accounts
  - Organizational hierarchy
  - Up to 4 levels of nesting for OUs

- **Consolidated billing across all accounts**
  - One bill for entire organization
  - Volume discounts apply across all accounts
  - Easier cost tracking and allocation
  - Shared volume pricing tiers

- **Hierarchical grouping of accounts (Organizational Units)**
  - Organize by department, environment, project
  - Apply policies at different levels
  - Inherit policies from parent OUs

- **Service Control Policies (SCPs) for governance**
  - Control maximum available permissions
  - Even limits account root user
  - Acts as a permission boundary

- **Automate account creation**
  - Programmatic account provisioning
  - Standardized setup
  - Integration with AWS Control Tower

- **Centralize security and compliance**
  - Enforce policies across organization
  - Consistent security posture
  - Delegated administration for AWS services

**Consolidated Billing Benefits:**
- One bill for all accounts
- Volume pricing discounts (S3, EC2, etc.)
- Reserved Instance sharing across accounts
- Savings Plans sharing
- Free tier applies once per organization
- Combined usage for tiered pricing

**Service Control Policies (SCPs):**
- Control maximum available permissions
- Do not grant permissions (only limit them)
- Affect all users and roles in accounts
- Do not affect service-linked roles
- Must enable before use
- Evaluation logic: explicit deny always wins

**Use Case Examples:**

**Example 1: Multi-Environment Organization**
```
Root
├── Production OU
│   ├── Prod-App-Account
│   └── Prod-Data-Account
├── Development OU
│   ├── Dev-Account
│   └── Test-Account
└── Sandbox OU
    └── Sandbox-Account
```

**SCP for Production OU (prevents accidental deletions):**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "ec2:TerminateInstances",
        "rds:DeleteDBInstance",
        "s3:DeleteBucket"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:PrincipalArn": "arn:aws:iam::*:role/AdminRole"
        }
      }
    }
  ]
}
```

**Example 2: Restricting Regions**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": [
            "us-east-1",
            "us-west-2",
            "eu-west-1"
          ]
        }
      }
    }
  ]
}
```

### AWS Key Management Service (KMS) - Detailed

Create and control cryptographic keys used to encrypt your data.

**Key Features:**

- **Create and manage cryptographic keys**
  - Symmetric and asymmetric keys
  - Hardware Security Modules (HSMs) backed
  - FIPS 140-2 validated

- **Control use of keys across AWS services**
  - Centralized key management
  - Integration with CloudTrail
  - Key policies for fine-grained control

- **Integrated with most AWS services**
  - S3, EBS, RDS, DynamoDB, and more
  - Transparent encryption
  - Over 100 AWS services integrated

- **Customer Master Keys (CMKs)**
  - **AWS Managed CMKs:** Created and managed by AWS, free
  - **Customer Managed CMKs:** You create and manage, $1/month
  - **AWS Owned CMKs:** Used by AWS services, no visibility

- **Automatic key rotation available**
  - Annual rotation for customer managed keys (optional)
  - Automatic for AWS managed keys (mandatory)
  - Old key material retained for decryption

- **Audit key usage via CloudTrail**
  - Who used which key
  - When and for what purpose
  - Complete audit trail

**Use Case Examples:**

**Use Case 1: Encrypt S3 Bucket with Customer Managed Key**
```
Scenario: Healthcare company storing patient records
Requirement: Control encryption keys, audit access, rotate annually
Solution: Create customer managed KMS key with strict key policy

Benefits:
- Full control over key lifecycle
- Audit trail in CloudTrail
- Can disable key if needed
- Automatic rotation
```

**Use Case 2: Cross-Account Data Sharing**
```
Scenario: Share encrypted data between AWS accounts
Setup:
1. Create KMS key in Account A
2. Update key policy to allow Account B
3. Share encrypted S3 objects
4. Account B can decrypt with permission

Key Policy Addition:
{
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::222222222222:root"
  },
  "Action": [
    "kms:Decrypt",
    "kms:DescribeKey"
  ],
  "Resource": "*"
}
```

**Use Case 3: Envelope Encryption**
```
Large file encryption process:
1. KMS generates data encryption key (DEK)
2. DEK encrypts the actual data
3. KMS encrypts the DEK with CMK
4. Store encrypted data + encrypted DEK
5. To decrypt: KMS decrypts DEK, DEK decrypts data

Benefits:
- Better performance for large files
- Reduced KMS API calls
- Data doesn't pass through KMS
```

**Pricing:**
- Customer managed CMK: $1/month per key
- API requests: $0.03 per 10,000 requests
- Free tier: 20,000 requests/month
- AWS managed CMKs: No charge for the key

### AWS Shield

DDoS (Distributed Denial of Service) protection service.

#### AWS Shield Standard

- **Automatic protection** for all AWS customers
- **No additional cost**
- Protects against **common Layer 3/4 attacks**
  - SYN/ACK floods
  - Reflection attacks
  - UDP floods
- Always-on detection
- Automatic inline mitigations

#### AWS Shield Advanced

- **$3,000/month** per organization
- **Enhanced protection** for:
  - Amazon EC2
  - Elastic Load Balancing (ELB)
  - Amazon CloudFront
  - Amazon Route 53
  - AWS Global Accelerator

- **24/7 access to DDoS Response Team (DRT)**
  - Expert support during attacks
  - Attack diagnostics

- **Cost protection**
  - Protection against usage spikes during attacks
  - Cost reimbursement for scaled resources

- **Real-time attack notifications**
  - CloudWatch metrics
  - Health-based detection

### Amazon GuardDuty - Detailed

Intelligent threat detection service using machine learning.

**Key Features:**

- **Intelligent threat detection service**
  - Continuous monitoring (24/7)
  - ML-powered analysis
  - Threat intelligence feeds

- **Uses machine learning**
  - Anomaly detection
  - Known threat patterns
  - Behavioral analysis

- **Monitors multiple data sources**
  - VPC Flow Logs (network traffic)
  - CloudTrail event logs (API activity)
  - DNS logs (DNS queries)
  - Kubernetes audit logs (EKS protection)
  - S3 data events (S3 Protection)
  - RDS login activity (RDS Protection)
  - EBS volume snapshots (Malware Protection)

- **Identifies unauthorized or malicious activity**
  - Compromised instances
  - Reconnaissance attempts
  - Account compromise
  - Cryptocurrency mining
  - Data exfiltration attempts

- **No software to deploy**
  - Fully managed service
  - Enable with a few clicks
  - No impact on performance

- **30-day free trial**

- **Integrates with EventBridge**
  - Automated responses to findings
  - Lambda function triggers
  - SNS notifications

**Common Threat Findings:**

| Finding Type | Description | Example |
|-------------|-------------|---------|
| **Backdoor:EC2/...** | Backdoor on EC2 instance | C&C server communication |
| **Behavior:EC2/...** | Unusual instance behavior | Traffic to unusual port |
| **CryptoCurrency:EC2/...** | Cryptocurrency mining | Bitcoin mining detected |
| **Trojan:EC2/...** | Trojan detected | DNS query to known bad domain |
| **UnauthorizedAccess:EC2/...** | Unauthorized access attempt | SSH brute force attack |
| **Recon:IAMUser/...** | Reconnaissance by IAM user | Listing resources unusually |
| **Stealth:IAMUser/...** | Stealth techniques | CloudTrail logging disabled |
| **CredentialAccess:IAMUser/...** | Credential access attempts | Password policies weakened |

**Use Case Examples:**

**Use Case 1: Detecting Compromised Instance**
```
Scenario: EC2 instance starts communicating with known C&C server
GuardDuty Detection:
- Finding: Backdoor:EC2/C&CActivity.B
- Severity: High
- Details: Instance communicating with command and control server

Automated Response:
1. EventBridge rule triggers Lambda
2. Lambda isolates instance (change security group)
3. Lambda creates snapshot for forensics
4. SNS notification to security team
5. Ticket created in ticketing system
```

**Use Case 2: Unusual API Call Pattern**
```
Scenario: IAM user making unusual API calls
GuardDuty Detection:
- Finding: Recon:IAMUser/NetworkPermissions
- Severity: Medium
- Details: User listing network resources unusually

Response:
1. Alert security team
2. Review CloudTrail logs
3. Interview user about activity
4. If compromised: rotate credentials
```

**Use Case 3: Cryptocurrency Mining**
```
Scenario: EC2 instance performing DNS queries to mining pools
GuardDuty Detection:
- Finding: CryptoCurrency:EC2/BitcoinTool.B
- Severity: High
- Details: DNS queries to Bitcoin mining pools

Response:
1. Immediately isolate instance
2. Snapshot for investigation
3. Terminate compromised instance
4. Launch replacement from clean AMI
5. Investigate how compromise occurred
```

**Pricing:**
- Based on volume of data analyzed
- CloudTrail events: $4.00 per million events
- VPC Flow Logs: $1.00 per GB
- DNS logs: $0.40 per million events
- First 30 days free
- No upfront commitment

### Amazon Inspector

Automated security assessment service for applications.

**Key Features:**

- **Automated security assessment service**
  - Continuous scanning
  - Scheduled assessments

- **Assesses applications for vulnerabilities**
  - CVE vulnerabilities
  - Network exposure

- **Checks for:**
  - Exposure to external threats
  - Vulnerabilities in applications
  - Deviations from best practices

- **Generates detailed security findings**
  - Severity ratings
  - Remediation recommendations

- **Prioritized list of security findings**
  - Risk-based prioritization
  - Context-aware scoring

- **Supports:**
  - EC2 instances
  - Container images (ECR)
  - Lambda functions

**Assessment Types:**
- Network assessments
- Host assessments
- Package vulnerability scanning

### AWS WAF (Web Application Firewall)

Protects web applications from common web exploits.

**Key Features:**

- **Protects web applications** from common exploits

- **Deployed on:**
  - Amazon CloudFront
  - Application Load Balancer (ALB)
  - Amazon API Gateway
  - AWS AppSync

- **Create custom rules** to block attack patterns
  - Define conditions
  - Action on matches (Allow, Block, Count)

- **Protection against:**
  - SQL injection
  - Cross-site scripting (XSS)
  - Size constraints violations
  - Geo-blocking

- **IP-based filtering**
  - IP sets (allow/deny lists)
  - IP rate limiting

- **Geo-blocking capabilities**
  - Block traffic from specific countries

- **Rate-based rules**
  - Prevent DDoS
  - Limit requests per IP

**Web ACL (Access Control List):**
- Collection of rules
- Applies to CloudFront distribution or ALB
- Rules evaluated in order

### Amazon Macie

Data security and privacy service using machine learning.

**Key Features:**

- **Data security and privacy service**
  - Sensitive data discovery
  - Data protection

- **Uses machine learning**
  - Intelligent pattern matching
  - Anomaly detection

- **Discovers and protects sensitive data**
  - Personally Identifiable Information (PII)
  - Financial data
  - Credentials

- **Identifies PII**
  - Names, addresses
  - Credit card numbers
  - Social Security numbers
  - Passport numbers

- **Monitors S3 buckets**
  - Data inventory
  - Security findings
  - Bucket policies

- **Provides dashboards and alerts**
  - Security findings
  - Data classification

- **Helps meet compliance requirements**
  - GDPR
  - HIPAA
  - PCI DSS

**Use Cases:**
- Discover sensitive data in S3
- Monitor for suspicious access patterns
- Compliance auditing
- Data classification

### AWS Artifact

Self-service portal for on-demand access to AWS compliance reports.

**Key Features:**

- **On-demand access** to AWS compliance reports

- **Self-service portal** for audit artifacts

- **Download AWS security and compliance documents**
  - Instant access
  - No waiting for support

- **Examples of available reports:**
  - ISO certifications (27001, 27017, 27018)
  - SOC reports (SOC 1, 2, 3)
  - PCI DSS reports
  - FedRAMP documentation

- **No cost**
  - Free to use
  - Available to all AWS customers

- **Support compliance and regulatory requirements**
  - Audit evidence
  - Third-party attestations

**Two Main Sections:**
1. **Artifact Reports:** Compliance reports and certifications
2. **Artifact Agreements:** Review and accept agreements (BAA, GDPR DPA)

---

## Compliance

### AWS Compliance Programs

AWS complies with numerous industry-specific compliance programs and regulations:

| Program | Description | Industry |
|---------|-------------|----------|
| **HIPAA** | Health Insurance Portability and Accountability Act | Healthcare |
| **PCI DSS** | Payment Card Industry Data Security Standard | Payment Processing |
| **SOC 1, 2, 3** | Service Organization Controls | Various |
| **ISO 27001** | Information Security Management | Various |
| **FedRAMP** | Federal Risk and Authorization Management Program | US Government |
| **GDPR** | General Data Protection Regulation | EU Data Privacy |

#### HIPAA (Health Insurance Portability and Accountability Act)

- **Healthcare industry** compliance
- Protects **Protected Health Information (PHI)**
- Requires **Business Associate Agreement (BAA)** with AWS
- HIPAA-eligible services include:
  - S3, EC2, RDS, DynamoDB
  - And many others (check AWS documentation)

#### PCI DSS (Payment Card Industry Data Security Standard)

- **Payment card processing** compliance
- Protects **cardholder data**
- Multiple compliance levels
- AWS infrastructure is PCI DSS compliant
- Customer applications may need separate certification

#### SOC (Service Organization Controls)

- **SOC 1:** Financial reporting controls
- **SOC 2:** Security, availability, confidentiality controls
  - Type I: Design of controls
  - Type II: Operating effectiveness
- **SOC 3:** General use report (public)

#### ISO 27001

- **International standard** for information security
- Information Security Management System (ISMS)
- Risk management framework
- Demonstrates security commitment

#### FedRAMP (Federal Risk and Authorization Management Program)

- **US Government** cloud compliance
- Standardized approach to security assessment
- Authorization levels:
  - Low Impact
  - Moderate Impact
  - High Impact

#### GDPR (General Data Protection Regulation)

- **EU data privacy** regulation
- Applies to processing of EU residents' data
- Key requirements:
  - Data protection by design
  - Right to erasure
  - Data portability
  - Breach notification
- AWS provides GDPR-compliant services and features

> **Exam Tip:** You don't need to memorize all compliance programs in detail, but know what they stand for and which industries they apply to.

---

## Compliance Programs - Deep Dive

### HIPAA Compliance Details

**What is HIPAA?**
- US legislation protecting patient medical records and PHI
- Enacted in 1996
- Applies to covered entities and business associates
- Requires safeguards for PHI confidentiality, integrity, availability

**AWS and HIPAA:**
- AWS infrastructure is HIPAA-compliant
- Must sign Business Associate Agreement (BAA) with AWS
- BAA is free, request through AWS Artifact
- Only HIPAA-eligible services can store PHI

**HIPAA-Eligible Services (common ones):**
- Compute: EC2, Lambda, Elastic Beanstalk
- Storage: S3, EBS, EFS, Glacier
- Database: RDS, DynamoDB, Redshift
- Networking: VPC, Direct Connect, Route 53
- Analytics: EMR, Kinesis, Athena

**Customer Responsibilities:**
- Execute BAA before processing PHI
- Use only HIPAA-eligible services for PHI
- Implement proper access controls
- Encrypt PHI at rest and in transit
- Maintain audit logs
- Implement breach notification procedures
- Regular risk assessments

**Technical Safeguards Required:**
- Access controls (IAM, MFA)
- Audit controls (CloudTrail, Config)
- Integrity controls (checksums, versioning)
- Transmission security (TLS, VPN)
- Encryption (KMS, SSL/TLS)

### PCI DSS Compliance Details

**What is PCI DSS?**
- Payment Card Industry Data Security Standard
- Protects cardholder data
- Applies to merchants and service providers
- 12 requirements across 6 control objectives

**Six Control Objectives:**
1. Build and maintain secure network
2. Protect cardholder data
3. Maintain vulnerability management program
4. Implement strong access control measures
5. Regularly monitor and test networks
6. Maintain information security policy

**AWS PCI DSS Compliance:**
- AWS infrastructure: PCI DSS Level 1 compliant
- Highest level of compliance
- Applies to compute, storage, network services
- Customer applications may need separate validation

**Compliance Levels:**
- **Level 1:** 6+ million transactions/year
- **Level 2:** 1-6 million transactions/year
- **Level 3:** 20,000-1 million e-commerce transactions/year
- **Level 4:** <20,000 e-commerce transactions/year

**AWS Services for PCI DSS:**
- Cardholder Data Environment (CDE) can run on EC2
- Segment CDE in separate VPC or subnet
- Use encryption for data at rest (KMS)
- Use TLS for data in transit
- Implement logging (CloudTrail, VPC Flow Logs)
- Use AWS WAF for application protection

**Key Requirements:**
- Network segmentation (VPC, security groups)
- Access controls (IAM, MFA)
- Encryption (KMS, TLS)
- Logging and monitoring (CloudTrail, CloudWatch)
- Vulnerability management (Inspector)
- Penetration testing (with AWS permission)

### SOC Reports Details

**SOC 1 (SSAE 18):**
- Focus: Financial reporting controls
- Audience: Financial auditors
- Content: Controls relevant to financial statements
- AWS provides: SOC 1 Type II report

**SOC 2 (AT-C 105):**
- Focus: Security, availability, processing integrity, confidentiality, privacy
- Audience: Management, regulators, stakeholders
- Two types:
  - **Type I:** Design of controls at specific point in time
  - **Type II:** Operating effectiveness over period (usually 6-12 months)
- AWS provides: SOC 2 Type II report

**SOC 3:**
- Simplified version of SOC 2
- General use report
- Publicly available
- Does not include detailed testing results
- Good for marketing and general assurance

**Five Trust Service Principles:**
1. **Security:** Protection against unauthorized access
2. **Availability:** System accessibility as agreed
3. **Processing Integrity:** Complete, valid, accurate processing
4. **Confidentiality:** Confidential information protection
5. **Privacy:** Personal information protection per commitments

**How to Access:**
- AWS Artifact for SOC 1, 2, 3 reports
- No cost
- Requires AWS account
- NDA acceptance required

### ISO 27001 Details

**What is ISO 27001?**
- International standard for ISMS
- Published by ISO/IEC
- Specifies requirements for establishing, implementing, maintaining ISMS
- Risk-based approach

**Key Components:**
- 14 control domains
- 114 controls
- Continuous improvement cycle (Plan-Do-Check-Act)

**14 Control Domains:**
1. Information security policies
2. Organization of information security
3. Human resource security
4. Asset management
5. Access control
6. Cryptography
7. Physical and environmental security
8. Operations security
9. Communications security
10. System acquisition, development, maintenance
11. Supplier relationships
12. Incident management
13. Business continuity
14. Compliance

**AWS ISO Certifications:**
- ISO 27001 (Information Security Management)
- ISO 27017 (Cloud Security)
- ISO 27018 (Cloud Privacy)
- ISO 27701 (Privacy Information Management)
- ISO 9001 (Quality Management)
- ISO 22301 (Business Continuity)

**Access Reports:**
- Download from AWS Artifact
- Available to all AWS customers
- Updated annually

### FedRAMP Details

**What is FedRAMP?**
- Federal Risk and Authorization Management Program
- US Government cloud security standard
- Standardizes security assessment and authorization
- Mandatory for federal agencies

**Authorization Levels:**

**Low Impact:**
- Data loss: Limited impact
- Examples: Static websites, public information
- Controls: 125 security controls

**Moderate Impact:**
- Data loss: Serious impact
- Examples: Most federal applications
- Controls: 325 security controls
- Most common baseline

**High Impact:**
- Data loss: Severe/catastrophic impact
- Examples: National security systems
- Controls: 421 security controls
- Highest security requirements

**AWS FedRAMP Compliance:**
- FedRAMP Authorized at High impact level
- Covers AWS GovCloud (US) regions
- Covers select services in commercial regions
- Continuous monitoring required

**FedRAMP Authorization Process:**
1. Preparation (package development)
2. Assessment by 3PAO (Third Party Assessment Organization)
3. Authorization by JAB or Agency
4. Continuous monitoring

**AWS Services FedRAMP Authorized:**
- 100+ services authorized
- Check FedRAMP Marketplace for current list
- New services regularly added

### GDPR Details

**What is GDPR?**
- General Data Protection Regulation
- EU regulation effective May 2018
- Applies to processing of EU residents' data
- Extraterritorial scope (applies globally)
- Heavy fines for non-compliance (up to 4% of revenue or €20M)

**Key Principles:**
1. **Lawfulness, fairness, transparency**
2. **Purpose limitation**
3. **Data minimization**
4. **Accuracy**
5. **Storage limitation**
6. **Integrity and confidentiality**
7. **Accountability**

**Data Subject Rights:**
- Right to access
- Right to rectification
- Right to erasure ("right to be forgotten")
- Right to restrict processing
- Right to data portability
- Right to object
- Rights related to automated decision-making

**AWS GDPR Compliance:**
- AWS Data Processing Addendum (DPA) available
- Supports customer GDPR compliance
- Data residency options (choose regions)
- Encryption capabilities
- Access controls and logging
- Data portability features

**Technical Measures for GDPR:**
- **Encryption:** KMS, SSL/TLS for data protection
- **Access Control:** IAM for limiting data access
- **Logging:** CloudTrail for accountability
- **Data Residency:** Region selection for data location
- **Deletion:** S3 lifecycle policies for right to erasure
- **Portability:** Data export capabilities
- **Anonymization:** Services for de-identification

**Breach Notification:**
- Must notify supervisory authority within 72 hours
- Must notify affected individuals without undue delay
- AWS notifies customers of breaches affecting them
- Customer responsible for notifying authorities/individuals

**AWS Tools for GDPR:**
- IAM for access control
- KMS for encryption
- CloudTrail for audit logs
- Config for compliance monitoring
- Macie for PII discovery
- S3 versioning and lifecycle for data retention

---

## Common Security Mistakes and How to Avoid Them

### Mistake 1: Using Root Account for Daily Tasks

**Why it's dangerous:**
- Root account has unrestricted access
- Cannot limit permissions
- If compromised, entire account at risk
- Difficult to track who did what

**How to avoid:**
- Create IAM users for daily tasks
- Use root account only for initial setup
- Enable MFA on root account
- Never create access keys for root account
- Lock away root account credentials
- Set up billing alerts on root account

**Best practice:**
```
1. Create IAM admin user immediately after account creation
2. Enable MFA on root account
3. Store root credentials in secure location (password manager)
4. Use IAM admin user for all tasks
5. Monitor root account usage with CloudWatch alarm
```

### Mistake 2: Overly Permissive IAM Policies

**Common patterns:**
```json
{
  "Effect": "Allow",
  "Action": "*",
  "Resource": "*"
}
```
**Why it's dangerous:**
- Grants unlimited access
- Violates least privilege
- Increases blast radius of compromise
- Hard to audit what's actually used

**How to avoid:**
- Start with minimal permissions
- Add permissions as needed
- Use AWS managed policies as starting point
- Regularly review and remove unused permissions
- Use IAM Access Analyzer
- Implement permission boundaries

**Better approach:**
```json
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject"
  ],
  "Resource": "arn:aws:s3:::specific-bucket/*"
}
```

### Mistake 3: Hardcoding Credentials in Code

**Examples of what NOT to do:**
```python
# NEVER DO THIS
aws_access_key = "AKIAIOSFODNN7EXAMPLE"
aws_secret_key = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
```

**Why it's dangerous:**
- Credentials exposed in version control
- Difficult to rotate
- Can be discovered by attackers
- Violates security best practices

**How to avoid:**
- Use IAM roles for EC2 instances
- Use environment variables
- Use AWS Secrets Manager
- Use Systems Manager Parameter Store
- Use temporary credentials via STS

**Better approach:**
```python
# Use IAM role (credentials automatically provided)
import boto3
s3 = boto3.client('s3')  # Credentials from instance role

# Or use Secrets Manager
import json
secretsmanager = boto3.client('secretsmanager')
secret = secretsmanager.get_secret_value(SecretId='MySecret')
credentials = json.loads(secret['SecretString'])
```

### Mistake 4: Leaving S3 Buckets Publicly Accessible

**Why it's dangerous:**
- Data exposed to internet
- Source of many data breaches
- Compliance violations
- Potential for data loss or ransomware

**How to avoid:**
- Enable S3 Block Public Access (account-level)
- Use bucket policies to restrict access
- Enable S3 server access logging
- Use AWS Macie to find sensitive data
- Regular audits with AWS Config
- Use VPC endpoints for private access

**Configuration:**
```
Enable S3 Block Public Access Settings:
✓ Block public access to buckets through new ACLs
✓ Block public access to buckets through any ACLs
✓ Block public access to buckets through new public bucket policies
✓ Block public and cross-account access through any public bucket policies
```

### Mistake 5: Not Enabling MFA

**Why it's dangerous:**
- Password-only authentication is weak
- Vulnerable to phishing
- Credential stuffing attacks
- Account takeover

**How to avoid:**
- Enable MFA on root account (mandatory)
- Enable MFA for all IAM users
- Require MFA for sensitive operations
- Use hardware MFA for high-privilege users
- Enforce MFA with IAM policies

**MFA enforcement policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "false"
        }
      }
    }
  ]
}
```

### Mistake 6: Ignoring CloudTrail Logs

**Why it's dangerous:**
- No audit trail
- Can't investigate incidents
- Compliance violations
- Unable to detect unauthorized access

**How to avoid:**
- Enable CloudTrail in all regions
- Send logs to S3 bucket
- Enable log file validation
- Set up CloudWatch Logs integration
- Create alarms for suspicious activity
- Restrict access to CloudTrail logs
- Enable in separate security account

**Critical events to monitor:**
- Root account usage
- IAM policy changes
- Security group changes
- CloudTrail being disabled
- Unauthorized API calls
- Failed login attempts

### Mistake 7: Poor Security Group Configuration

**Common mistakes:**
- Opening 0.0.0.0/0 on all ports
- Allowing RDP/SSH from anywhere
- Overly permissive outbound rules
- Not using security group references

**Why it's dangerous:**
- Exposes resources to internet
- Increases attack surface
- Brute force attacks
- Lateral movement if compromised

**How to avoid:**
- Use principle of least privilege
- Restrict SSH/RDP to specific IPs
- Use security group references
- Regular audits
- Use AWS Config rules
- Implement bastion hosts

**Bad configuration:**
```
Inbound: 0.0.0.0/0 on port 22 (SSH)
```

**Good configuration:**
```
Inbound: YOUR_IP/32 on port 22 (SSH)
Or better: Bastion-SG on port 22
```

### Mistake 8: Not Encrypting Data

**Why it's dangerous:**
- Data exposed if storage compromised
- Compliance violations
- Data breaches
- Regulatory fines

**How to avoid:**
- Enable encryption by default
- Use KMS for key management
- Encrypt data in transit (TLS/SSL)
- Encrypt data at rest
- Use S3 bucket encryption
- Enable EBS encryption by default
- Use RDS encryption

**Enable encryption by default:**
```
Account Settings:
✓ EBS encryption enabled by default
✓ S3 default encryption enabled
✓ RDS encryption required

✓ TLS 1.2+ enforced
✓ HTTPS required for CloudFront
```

### Mistake 9: Sharing IAM Credentials

**Examples:**
- Multiple people using same IAM user
- Sharing access keys
- Using one "service account" for everything

**Why it's dangerous:**
- No accountability
- Can't track who did what
- Difficult to rotate
- Violates compliance requirements

**How to avoid:**
- Create individual IAM users
- Use IAM roles for services
- Implement federation for user access
- No shared credentials ever
- Use temporary credentials
- Monitor and alert on concurrent logins

### Mistake 10: Neglecting Security Updates

**What's neglected:**
- OS patches
- Application updates
- Security patches
- AMI updates

**Why it's dangerous:**
- Known vulnerabilities exploited
- Malware infections
- Compliance violations
- Security breaches

**How to avoid:**
- Use AWS Systems Manager Patch Manager
- Enable automatic security updates
- Regularly update AMIs
- Use Amazon Inspector
- Implement patch compliance monitoring
- Schedule regular maintenance windows

**Patch management strategy:**
```
1. Test patches in dev environment
2. Schedule maintenance windows
3. Use Systems Manager for patching
4. Monitor patch compliance with Config
5. Automate where possible
6. Maintain patch documentation
```

### Mistake 11: Not Using Least Privilege

**Common patterns:**
- Giving admin access to everyone
- Using wildcard (*) in policies
- Not reviewing permissions
- Adding permissions but never removing

**How to avoid:**
- Start with zero permissions
- Add only what's needed
- Regular access reviews
- Use IAM Access Analyzer
- Remove unused permissions
- Use permission boundaries

### Mistake 12: Poor Network Segmentation

**Mistakes:**
- All resources in public subnet
- No separation between tiers
- Flat network architecture

**How to avoid:**
- Use multiple subnets
- Separate by tier (web, app, data)
- Use private subnets for databases
- Implement defense in depth
- Use NACLs and security groups
- Follow well-architected principles

**Proper architecture:**
```
Public Subnet: Load balancers, bastion hosts
Private Subnet: Application servers
Private Subnet: Databases (no internet access)
```

---

## Security Checklist for Exam Preparation

### IAM Security Checklist

- [ ] Root account has MFA enabled
- [ ] Root account has no access keys
- [ ] Individual IAM users created (no sharing)
- [ ] IAM users have MFA enabled
- [ ] IAM password policy is strong
- [ ] IAM users grouped by role
- [ ] Policies attached to groups, not users
- [ ] Least privilege principle applied
- [ ] Unused credentials removed
- [ ] Access keys rotated every 90 days
- [ ] IAM roles used for EC2 instances
- [ ] Cross-account access uses roles
- [ ] Service Control Policies implemented (Organizations)
- [ ] Permission boundaries used where appropriate

### Data Protection Checklist

- [ ] S3 buckets have encryption enabled
- [ ] S3 Block Public Access enabled
- [ ] S3 versioning enabled for important data
- [ ] EBS encryption enabled by default
- [ ] RDS databases encrypted
- [ ] Data encrypted in transit (TLS/SSL)
- [ ] KMS used for key management
- [ ] Automatic key rotation enabled
- [ ] Sensitive data classified
- [ ] DLP policies implemented (Macie)
- [ ] Backup strategy defined
- [ ] Backup testing performed regularly

### Network Security Checklist

- [ ] VPC created for resources
- [ ] Public/private subnets separated
- [ ] Security groups follow least privilege
- [ ] NACLs configured for subnet protection
- [ ] VPC Flow Logs enabled
- [ ] No 0.0.0.0/0 on SSH/RDP
- [ ] Bastion hosts used for access
- [ ] VPC endpoints used for AWS services
- [ ] Network segmentation implemented
- [ ] WAF enabled for web applications
- [ ] Shield Standard active (automatic)
- [ ] DDoS response plan documented

### Monitoring and Logging Checklist

- [ ] CloudTrail enabled in all regions
- [ ] CloudTrail log file validation enabled
- [ ] CloudTrail logs in separate account
- [ ] VPC Flow Logs enabled
- [ ] S3 access logging enabled
- [ ] ELB access logs enabled
- [ ] CloudWatch alarms configured
- [ ] GuardDuty enabled
- [ ] Security Hub enabled
- [ ] Config rules enabled
- [ ] Automated remediation configured
- [ ] Incident response plan documented

### Compliance Checklist

- [ ] Compliance requirements identified
- [ ] AWS Artifact reports reviewed
- [ ] BAA signed (if HIPAA required)
- [ ] Compliance documentation maintained
- [ ] Regular compliance audits performed
- [ ] Config rules for compliance checking
- [ ] Tags applied for governance
- [ ] Resource inventory maintained

### Exam Readiness Checklist

- [ ] Understand Shared Responsibility Model
- [ ] Know IAM components (users, groups, roles, policies)
- [ ] Understand difference between authentication and authorization
- [ ] Know when to use each security service
- [ ] Understand compliance programs and industries
- [ ] Know security best practices
- [ ] Understand encryption (at rest and in transit)
- [ ] Know network security concepts
- [ ] Understand monitoring and logging services
- [ ] Know incident response basics

### AWS Config

Assess, audit, and evaluate AWS resource configurations.

**Key Features:**

- **Assess, audit, and evaluate configurations**
  - Configuration history
  - Configuration snapshots

- **Continuous monitoring** of resource configurations
  - Real-time tracking
  - Change detection

- **Track configuration changes over time**
  - Who made changes
  - When changes occurred
  - What changed

- **Compliance auditing and security analysis**
  - Configuration compliance
  - Security posture assessment

- **Config Rules** define desired configurations
  - AWS managed rules
  - Custom rules (Lambda)
  - Automatic or triggered evaluation

- **Automated remediation** of non-compliant resources
  - SSM Automation documents
  - Automatic or manual remediation

**Use Cases:**
- Continuous compliance monitoring
- Security analysis
- Change management
- Troubleshooting
- Configuration history

**How It Works:**
1. Enable AWS Config in your account
2. Select resources to monitor
3. Define Config Rules
4. Review compliance dashboard
5. Set up automated remediation (optional)

**Integration:**
- CloudTrail (who made the change)
- SNS (notifications)
- S3 (configuration snapshots)
- Systems Manager (remediation)

---

## Review Questions

Test your knowledge of Domain 2: Security and Compliance.

### Question 1

**According to the Shared Responsibility Model, which security aspect is AWS responsible for?**

A. Security group configuration
B. Physical security of data centers
C. Customer data encryption
D. IAM user management

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** AWS is responsible for security OF the cloud, which includes physical security of data centers, hardware, and infrastructure. The customer is responsible for security IN the cloud, including security groups (A), data encryption (C), and IAM user management (D).

</details>

---

### Question 2

**Which service provides DDoS protection at no additional cost?**

A. AWS WAF
B. AWS Shield Advanced
C. AWS Shield Standard
D. Amazon GuardDuty

<details>
<summary>Click to reveal answer</summary>

**Answer: C**

**Explanation:** AWS Shield Standard provides automatic DDoS protection for all AWS customers at no additional cost. Shield Advanced (B) costs $3,000/month, WAF (A) has its own pricing, and GuardDuty (D) is for threat detection, not DDoS protection.

</details>

---

### Question 3

**What is the best practice for granting permissions to a group of developers?**

A. Attach policies directly to each user
B. Create an IAM group, attach policies to the group, add users to the group
C. Share the root account credentials
D. Create one IAM user that everyone shares

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** The best practice is to create IAM groups, attach policies to the groups, and then add users to appropriate groups. This simplifies management and follows security best practices. Sharing credentials (C and D) is never recommended, and attaching policies to individual users (A) is harder to manage.

</details>

---

### Question 4

**Which service uses machine learning to discover and protect sensitive data in S3?**

A. Amazon GuardDuty
B. Amazon Inspector
C. Amazon Macie
D. AWS Config

<details>
<summary>Click to reveal answer</summary>

**Answer: C**

**Explanation:** Amazon Macie uses machine learning to discover, classify, and protect sensitive data (like PII) in Amazon S3. GuardDuty (A) is for threat detection, Inspector (B) is for vulnerability assessment, and Config (D) is for configuration compliance.

</details>

---

### Question 5

**Which IAM entity provides temporary security credentials?**

A. IAM User
B. IAM Group
C. IAM Role
D. IAM Policy

<details>
<summary>Click to reveal answer</summary>

**Answer: C**

**Explanation:** IAM Roles provide temporary security credentials that are automatically rotated. Users (A) have long-term credentials, Groups (B) are collections of users, and Policies (D) define permissions but don't provide credentials.

</details>

---

### Question 6

**Where can you download AWS compliance reports and certifications?**

A. AWS Config
B. AWS Artifact
C. AWS Inspector
D. AWS Organizations

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** AWS Artifact is the self-service portal where you can download AWS compliance reports, certifications (ISO, SOC, PCI), and agreements. It's available at no cost to all AWS customers.

</details>

---

### Question 7

**What is the primary purpose of AWS Config?**

A. Encrypt data at rest
B. Track configuration changes and compliance
C. Detect threats using machine learning
D. Protect against DDoS attacks

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** AWS Config tracks configuration changes over time and evaluates compliance against desired configurations. KMS handles encryption (A), GuardDuty detects threats (C), and Shield protects against DDoS (D).

</details>

---

### Question 8

**Which authentication factor does MFA add to username/password?**

A. Something you know
B. Something you have
C. Something you are
D. Somewhere you are

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** MFA adds "something you have" (the MFA device) to "something you know" (the password), providing two-factor authentication. The password is "something you know" (A), biometrics would be "something you are" (C), and location would be "somewhere you are" (D).

</details>

---

### Question 9

**Which service would you use to centrally manage multiple AWS accounts and apply governance policies?**

A. IAM
B. AWS Organizations
C. AWS Config
D. AWS Control Tower

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** AWS Organizations allows you to centrally manage multiple AWS accounts, provide consolidated billing, and apply Service Control Policies (SCPs) for governance. IAM (A) manages access within a single account, Config (C) tracks configurations, and while Control Tower (D) can also manage accounts, Organizations is the core service tested at the Cloud Practitioner level.

</details>

---

### Question 10

**Which compliance program is specifically for healthcare data in the United States?**

A. PCI DSS
B. GDPR
C. HIPAA
D. SOC 2

<details>
<summary>Click to reveal answer</summary>

**Answer: C**

**Explanation:** HIPAA (Health Insurance Portability and Accountability Act) is the US regulation for protecting healthcare data and PHI (Protected Health Information). PCI DSS (A) is for payment cards, GDPR (B) is EU data privacy, and SOC 2 (D) is for general security controls.

</details>

---

### Question 11

**Which AWS service should you use to discover and protect sensitive data like credit card numbers in S3?**

A. AWS Config
B. Amazon Macie
C. AWS WAF
D. Amazon Inspector

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** Amazon Macie uses machine learning to discover, classify, and protect sensitive data like PII, credit card numbers, and other confidential information in S3 buckets. Config (A) tracks configurations, WAF (C) protects web applications, and Inspector (D) assesses vulnerabilities.

</details>

---

### Question 12

**Your company needs to encrypt data at rest in S3 with full control over the encryption keys, including rotation. Which solution should you use?**

A. SSE-S3 (Server-Side Encryption with S3-Managed Keys)
B. SSE-KMS with customer managed CMK
C. SSE-C (Server-Side Encryption with Customer-Provided Keys)
D. Client-side encryption

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** SSE-KMS with customer managed CMK gives you full control over encryption keys, including rotation, while AWS handles the encryption process. SSE-S3 (A) doesn't give you control over keys, SSE-C (C) requires you to provide keys with each request, and client-side encryption (D) requires you to manage the entire encryption process.

</details>

---

### Question 13

**Which service provides automated vulnerability assessment for EC2 instances and container images?**

A. Amazon GuardDuty
B. AWS Security Hub
C. Amazon Inspector
D. AWS Systems Manager

<details>
<summary>Click to reveal answer</summary>

**Answer: C**

**Explanation:** Amazon Inspector is an automated security assessment service that checks for vulnerabilities in EC2 instances, container images in ECR, and Lambda functions. GuardDuty (A) is for threat detection, Security Hub (B) is a centralized security view, and Systems Manager (D) is for operational management.

</details>

---

### Question 14

**According to the Shared Responsibility Model, who is responsible for patching the guest operating system on an EC2 instance?**

A. AWS
B. Customer
C. Both AWS and Customer
D. Neither, it's automated

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** The customer is responsible for patching the guest OS on EC2 instances. This falls under "security IN the cloud." AWS is responsible for patching the hypervisor and infrastructure (security OF the cloud). For managed services like RDS, AWS handles the patching.

</details>

---

### Question 15

**Which feature of AWS Organizations allows you to restrict actions across all accounts in your organization?**

A. IAM Policies
B. Resource Access Manager
C. Service Control Policies (SCPs)
D. Permission Boundaries

<details>
<summary>Click to reveal answer</summary>

**Answer: C**

**Explanation:** Service Control Policies (SCPs) allow you to set maximum available permissions across accounts in AWS Organizations. They act as guardrails and can even restrict the root user. IAM policies (A) work within a single account, RAM (B) is for resource sharing, and permission boundaries (D) set maximum permissions for IAM entities.

</details>

---

### Question 16

**What is the primary purpose of AWS CloudTrail?**

A. Monitor resource utilization
B. Log API activity for auditing
C. Detect security threats
D. Track configuration changes

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** CloudTrail logs API activity in your AWS account, providing an audit trail of who did what, when, and from where. CloudWatch (A) monitors resource utilization, GuardDuty (C) detects threats, and Config (D) tracks configuration changes.

</details>

---

### Question 17

**Which of the following is NOT a valid MFA device option for AWS?**

A. Virtual MFA device (smartphone app)
B. Hardware MFA device (YubiKey)
C. SMS text message
D. Fingerprint scanner

<details>
<summary>Click to reveal answer</summary>

**Answer: D**

**Explanation:** AWS does not support fingerprint scanners or other biometric authentication for MFA. Valid options include virtual MFA devices (A), hardware MFA devices (B), and SMS text messages (C), although SMS is not recommended for root accounts.

</details>

---

### Question 18

**A startup is building a web application that needs to authenticate users via Facebook and Google. Which AWS service should they use?**

A. AWS IAM
B. Amazon Cognito
C. AWS Directory Service
D. AWS Single Sign-On

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** Amazon Cognito supports web identity federation, allowing users to authenticate with social identity providers like Facebook, Google, and Amazon. IAM (A) is for AWS resource access, Directory Service (C) is for Microsoft AD integration, and SSO (D) is for AWS account and business application access.

</details>

---

### Question 19

**Which encryption option for S3 provides an audit trail of when keys were used and by whom?**

A. SSE-S3
B. SSE-KMS
C. SSE-C
D. Client-side encryption

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** SSE-KMS integrates with CloudTrail, providing an audit trail of when encryption keys were used and by whom. SSE-S3 (A) doesn't provide this visibility, SSE-C (C) means you manage keys outside AWS, and client-side encryption (D) is entirely managed by you.

</details>

---

### Question 20

**What is the purpose of VPC Flow Logs?**

A. Log API calls in your VPC
B. Capture network traffic information
C. Monitor VPC configuration changes
D. Detect malware in network traffic

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** VPC Flow Logs capture information about IP traffic going to and from network interfaces in your VPC, including source/destination IPs, ports, and protocols. CloudTrail (A) logs API calls, Config (C) monitors configuration changes, and while flow logs can help security analysis, they don't directly detect malware (D).

</details>

---

### Question 21

**Which compliance program is specifically designed for US federal government agencies?**

A. HIPAA
B. PCI DSS
C. FedRAMP
D. SOC 2

<details>
<summary>Click to reveal answer</summary>

**Answer: C**

**Explanation:** FedRAMP (Federal Risk and Authorization Management Program) is the US government's cloud security standard. HIPAA (A) is for healthcare, PCI DSS (B) is for payment cards, and SOC 2 (D) is a general security audit framework.

</details>

---

### Question 22

**A company wants to share an encrypted S3 bucket with another AWS account. What must be configured?**

A. S3 bucket policy only
B. KMS key policy and S3 bucket policy
C. IAM role only
D. VPC peering

<details>
<summary>Click to reveal answer</summary>

**Answer: B**

**Explanation:** To share an encrypted S3 bucket cross-account, you need to update both the KMS key policy (to allow the other account to decrypt) and the S3 bucket policy (to allow access to objects). Just one or the other won't work. VPC peering (D) is not required for S3 access.

</details>

---

### Question 23

**Which AWS service protects against DDoS attacks at no additional cost?**

A. AWS WAF
B. AWS Shield Advanced
C. AWS Shield Standard
D. Amazon GuardDuty

<details>
<summary>Click to reveal answer</summary>

**Answer: C**

**Explanation:** AWS Shield Standard provides DDoS protection at no additional cost and is automatically enabled for all AWS customers. Shield Advanced (B) costs $3,000/month, WAF (A) has its own pricing for rule complexity and requests, and GuardDuty (D) is for threat detection, not DDoS protection.

</details>

---

### Question 24

**What is the difference between security groups and Network ACLs?**

A. Security groups are stateful; NACLs are stateless
B. Security groups are stateless; NACLs are stateful
C. Both are stateful
D. Both are stateless

<details>
<summary>Click to reveal answer</summary>

**Answer: A**

**Explanation:** Security groups are stateful (return traffic is automatically allowed), while Network ACLs are stateless (you must explicitly allow both inbound and outbound traffic). This is a critical difference for the exam.

</details>

---

### Question 25

**A company must ensure that all data stored in AWS is encrypted at rest and in transit. Which services should they use? (Choose TWO)**

A. AWS KMS for encryption at rest
B. AWS CloudHSM for encryption in transit
C. TLS/SSL for encryption in transit
D. AWS Certificate Manager for encryption at rest

<details>
<summary>Click to reveal answer</summary>

**Answer: A and C**

**Explanation:** AWS KMS provides encryption at rest for services like S3, EBS, and RDS (A). TLS/SSL provides encryption in transit for data moving between systems (C). CloudHSM (B) is for hardware-based key storage but not specifically for transit encryption, and ACM (D) provides certificates for TLS/SSL but doesn't encrypt at rest.

</details>

---

## Advanced Exam Tips and Scenarios

### Exam Tip 1: Shared Responsibility Model Questions

**How to identify:** Questions ask "who is responsible for..."

**Decision tree:**
1. Is it infrastructure (physical, network hardware, data centers)? → **AWS**
2. Is it a managed service (RDS, Lambda, DynamoDB)? → **AWS manages infrastructure, you manage data and access**
3. Is it EC2? → **AWS manages hypervisor, you manage OS, applications, data**
4. Is it customer data, encryption, or IAM? → **Always customer**

**Common tricky scenarios:**
- "Who patches RDS database engine?" → **AWS**
- "Who patches EC2 operating system?" → **Customer**
- "Who configures security groups?" → **Customer**
- "Who secures AWS data centers?" → **AWS**

### Exam Tip 2: IAM Policy Evaluation Logic

**Order of evaluation:**
1. Explicit DENY → Always wins
2. Explicit ALLOW → If no deny exists
3. Implicit DENY → Default if no allow

**Remember:** One explicit deny overrules all allows!

**Example scenario:**
```
User has policy: Allow s3:*
Group has policy: Deny s3:DeleteBucket
Result: User can do everything EXCEPT delete buckets
```

### Exam Tip 3: Encryption Service Selection

**Question type:** "Which encryption service should you use when..."

**Decision matrix:**

| Requirement | Solution |
|------------|----------|
| Simple S3 encryption | SSE-S3 |
| Need audit trail of key usage | SSE-KMS |
| Must control keys outside AWS | SSE-C or Client-side |
| Encrypt EBS volumes | KMS (default) |
| Encrypt in transit | TLS/SSL, ACM |
| Meet compliance requirements | KMS with customer managed CMK |
| Hardware-based key storage | CloudHSM |

### Exam Tip 4: Security Service Selection

**Question type:** "Which service detects/protects/monitors..."

**Quick reference:**

| Need | Service |
|------|---------|
| Detect threats with ML | GuardDuty |
| Find vulnerabilities | Inspector |
| Discover sensitive data | Macie |
| Protect against DDoS | Shield |
| Protect web applications | WAF |
| Manage encryption keys | KMS |
| Audit API calls | CloudTrail |
| Track configurations | Config |
| Compliance reports | Artifact |
| Centralized security view | Security Hub |

### Exam Tip 5: Compliance Program Matching

**Pattern recognition for exam:**
- "Healthcare data" or "PHI" → **HIPAA**
- "Credit card" or "payment data" → **PCI DSS**
- "Government" or "federal agency" → **FedRAMP**
- "EU residents" or "data privacy" → **GDPR**
- "Audit report" or "financial controls" → **SOC reports**
- "International security standard" → **ISO 27001**

### Exam Tip 6: MFA Scenarios

**When MFA is the answer:**
- Question mentions "additional layer of security"
- Scenario involves privileged users or root account
- Compliance requirement for sensitive operations
- "Something you have" factor is mentioned

**MFA is NOT the answer for:**
- Service-to-service authentication (use roles)
- Programmatic access from applications (use roles)
- Long-term credential storage (use IAM roles)

### Exam Tip 7: Network Security Scenarios

**Security Group vs NACL:**

| Scenario | Use |
|----------|-----|
| Need to explicitly deny an IP | NACL |
| Need stateful filtering | Security Group |
| Subnet-level protection | NACL |
| Instance-level protection | Security Group |
| Process rules in order | NACL |
| Simple allow rules | Security Group |

### Exam Tip 8: Identity Federation

**Scenario patterns:**
- "Corporate users need AWS access" → **SAML federation or IAM Identity Center**
- "Mobile app users" → **Cognito**
- "Social login (Facebook, Google)" → **Cognito**
- "Active Directory integration" → **Directory Service or IAM Identity Center**
- "Multiple AWS accounts, single login" → **IAM Identity Center**

### Exam Tip 9: Data Protection Scenarios

**Pattern matching:**
- "Prevent public access to S3" → **S3 Block Public Access**
- "Track who accesses S3 objects" → **S3 Server Access Logging + CloudTrail**
- "Find sensitive data in S3" → **Macie**
- "Encrypt data before upload" → **Client-side encryption**
- "AWS manages encryption" → **SSE-S3 or SSE-KMS**

### Exam Tip 10: Cost Considerations

**Free services/features:**
- IAM (completely free)
- CloudTrail (first trail free)
- Shield Standard (free DDoS protection)
- S3 SSE-S3 encryption (no extra cost)
- VPC (core features free)
- AWS managed CMKs (free, pay for API calls only)

**Paid services:**
- Shield Advanced ($3,000/month)
- GuardDuty (pay per GB analyzed)
- Macie (pay per GB scanned)
- Inspector (pay per assessment)
- WAF (pay per rule and requests)
- Customer managed CMKs ($1/month + API calls)

### Exam Tip 11: Incident Response Questions

**Scenario:** "What should you do first when..."
1. Isolate affected resources
2. Preserve evidence (snapshots, logs)
3. Investigate and analyze
4. Remediate
5. Document lessons learned

**Key services:**
- CloudTrail for forensics
- VPC Flow Logs for network analysis
- GuardDuty for threat detection
- Systems Manager for remediation

### Exam Tip 12: Common Exam Traps

**Watch out for:**
1. "Most cost-effective" → Usually the simpler, managed option
2. "Least operational overhead" → Usually the fully managed service
3. "Most secure" → Usually involves encryption, MFA, least privilege
4. "Best practice" → Follow AWS recommendations (IAM roles, not keys)

**Red flags:**
- Hardcoding credentials → ❌ Never correct
- Root account for daily tasks → ❌ Never correct
- Wildcard (*) permissions → ❌ Usually incorrect
- Public access to production data → ❌ Usually incorrect

---

## Key Takeaways

> **Remember for the Exam:**
>
> - **Shared Responsibility Model:** AWS = infrastructure; Customer = data and configuration
> - **IAM Best Practices:** Root account protection, least privilege, use groups and roles, enable MFA
> - **Shield Standard:** Free DDoS protection for everyone
> - **GuardDuty:** Threat detection with ML
> - **Macie:** Sensitive data discovery in S3
> - **Artifact:** Download compliance reports
> - **Config:** Track configuration changes and compliance
> - **Organizations:** Multi-account management with consolidated billing and SCPs

---

[← Previous: Cloud Concepts](02-cloud-concepts.md) | [Back to Main](README.md) | [Next: Cloud Technology and Services →](04-technology-services.md)
