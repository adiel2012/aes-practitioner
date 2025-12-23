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

## Security Services

### AWS Organizations

Centrally manage and govern multiple AWS accounts.

**Key Features:**

- **Centrally manage multiple AWS accounts**
  - Single pane of glass for all accounts
  - Organizational hierarchy

- **Consolidated billing across all accounts**
  - One bill for entire organization
  - Volume discounts apply across all accounts
  - Easier cost tracking and allocation

- **Hierarchical grouping of accounts (Organizational Units)**
  - Organize by department, environment, project
  - Apply policies at different levels

- **Service Control Policies (SCPs) for governance**
  - Control maximum available permissions
  - Even limits account root user
  - Acts as a permission boundary

- **Automate account creation**
  - Programmatic account provisioning
  - Standardized setup

- **Centralize security and compliance**
  - Enforce policies across organization
  - Consistent security posture

**Consolidated Billing Benefits:**
- One bill for all accounts
- Volume pricing discounts
- Reserved Instance sharing
- Savings Plans sharing
- Free tier applies once per organization

**Service Control Policies (SCPs):**
- Control maximum available permissions
- Do not grant permissions (only limit them)
- Affect all users and roles in accounts
- Do not affect service-linked roles

### AWS Key Management Service (KMS)

Create and control cryptographic keys used to encrypt your data.

**Key Features:**

- **Create and manage cryptographic keys**
  - Symmetric and asymmetric keys
  - Hardware Security Modules (HSMs) backed

- **Control use of keys across AWS services**
  - Centralized key management
  - Integration with CloudTrail

- **Integrated with most AWS services**
  - S3, EBS, RDS, and more
  - Transparent encryption

- **Customer Master Keys (CMKs)**
  - **AWS Managed CMKs:** Created and managed by AWS
  - **Customer Managed CMKs:** You create and manage
  - **AWS Owned CMKs:** Used by AWS services

- **Automatic key rotation available**
  - Annual rotation for customer managed keys
  - Automatic for AWS managed keys

- **Audit key usage via CloudTrail**
  - Who used which key
  - When and for what purpose

**Use Cases:**
- Encrypt data at rest
- Envelope encryption for large files
- Sign and verify digital signatures
- Compliance requirements (HIPAA, PCI DSS)

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

### Amazon GuardDuty

Intelligent threat detection service using machine learning.

**Key Features:**

- **Intelligent threat detection service**
  - Continuous monitoring
  - ML-powered analysis

- **Uses machine learning**
  - Anomaly detection
  - Known threat patterns

- **Monitors multiple data sources**
  - VPC Flow Logs
  - CloudTrail event logs
  - DNS logs
  - Kubernetes audit logs

- **Identifies unauthorized or malicious activity**
  - Compromised instances
  - Reconnaissance attempts
  - Account compromise
  - Cryptocurrency mining

- **No software to deploy**
  - Fully managed service
  - Enable with a few clicks

- **30-day free trial**

- **Integrates with EventBridge**
  - Automated responses to findings
  - Lambda function triggers

**Common Findings:**
- Unusual API calls
- Potentially compromised instances
- Reconnaissance activities
- Port scanning
- Bitcoin mining

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
