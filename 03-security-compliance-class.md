# Lesson 3: Security and Compliance

---

Welcome to the security domain. This is the second-heaviest domain on the exam at thirty percent of your score, and it is also the domain where most people lose the most points — not because the material is hard, but because the distinctions between services are subtle and the exam exploits that. By the end of this lesson you will be able to answer any security question by applying a small set of clear mental models. Let us build those models now.

---

## The Shared Responsibility Model

Everything in AWS security starts here. The shared responsibility model is the most fundamental concept in this domain, and I can almost guarantee you will see it on the exam in multiple questions, sometimes directly and sometimes embedded in service-specific scenarios.

The core idea is simple: AWS and you share the responsibility for security, but you share it by dividing it along a clear boundary. The phrase to memorize is this: AWS is responsible for security OF the cloud. You, the customer, are responsible for security IN the cloud.

Security of the cloud means the infrastructure itself. AWS owns and operates the physical data centers. AWS installs and maintains the servers, the storage systems, the networking equipment, and the hypervisors. AWS handles the physical guards, the environmental safeguards like power and cooling, and the access controls on the buildings. AWS manages the underlying hardware for its managed services — when you use RDS, for example, AWS patches and maintains the database engine and the underlying server. When you use Lambda, AWS manages the execution environment. You never touch any of that, and you never have to worry about it. AWS is responsible.

Security in the cloud means everything you put on top of that infrastructure. Your data is your responsibility. You are responsible for deciding what data to store, how to classify it, and how to protect it. IAM is your responsibility — you create the users, the groups, the roles, and the policies that control who can do what in your account. Security groups and network ACLs are your responsibility — you define the firewall rules that allow or deny traffic. If you launch an EC2 instance, you are responsible for patching the operating system. Encryption of your data at rest and in transit is your responsibility. Managing your own users and their passwords and MFA is your responsibility.

The place where students get tripped up is managed services. When RDS is involved, the answer to "who patches the operating system and database engine?" is AWS. But the answer to "who manages the data in the database and who can access it?" is the customer. That distinction — AWS manages the infrastructure of managed services, you manage the data and access controls — comes up constantly.

Here is the decision tree for any shared responsibility question: Is it about physical infrastructure, hardware, data centers, or the managed service runtime? That is AWS. Is it about customer data, encryption choices, IAM configuration, security group rules, or OS patching on EC2? That is the customer.

---

## Identity and Access Management

IAM is the foundational security service for controlling who can do what in your AWS account. It is a global service — it does not operate in a specific region. It is free to use. And it is tested heavily.

IAM has four components you must understand: users, groups, roles, and policies.

A user represents a person or an application that needs long-term access to your AWS account. Users can have a password for console access and access keys for programmatic access. A new user has no permissions by default — they can log in but cannot do anything until permissions are granted.

A group is a collection of users. You manage permissions at the group level. You attach policies to the group, and all users in that group inherit those permissions. A user can belong to multiple groups. Groups cannot be nested inside other groups. When the exam describes a team of developers who all need the same permissions, the answer involves creating a group and attaching the appropriate policy to the group, not attaching policies to each individual user.

A role is fundamentally different from a user. A role does not have a username, a password, or access keys. Instead, it is a set of permissions that can be temporarily assumed by any trusted entity — a user, an application, an AWS service, or even a user from another AWS account. When an EC2 instance needs to access S3, you attach an IAM role to the EC2 instance. The instance assumes the role, receives temporary credentials automatically, and uses those credentials to call S3. The temporary credentials rotate automatically. No one ever needs to store access keys in the application code. This is the best practice for any application running on AWS. The exam will present scenarios where an application needs AWS permissions, and the correct answer is almost always to use a role.

A policy is a JSON document that defines what actions are allowed or denied on which resources. You attach policies to users, groups, or roles. The fundamental rule of policy evaluation is this: an explicit Deny always wins. If any policy in the evaluation says Deny, access is denied regardless of any Allow that might exist elsewhere. If no explicit Deny exists and an explicit Allow exists, access is granted. If there is no explicit statement either way, access is denied by default — this is the implicit Deny. So the evaluation goes: explicit Deny wins, then explicit Allow wins, then implicit Deny wins by default.

---

## IAM Best Practices

Let me walk through the best practices that come up on the exam.

The root account is the account you created when you first opened AWS. It has unrestricted access to everything and cannot have its permissions limited. The rule is: use the root account only for initial setup, then lock it away. Create an IAM admin user and use that for daily administration. Enable MFA on the root account. Do not create access keys for root. The exam will never present a scenario where using the root account is the correct answer for a day-to-day task.

Apply the principle of least privilege everywhere. Grant only the permissions actually needed to perform the task, nothing more. Start with minimal permissions and add as needed. Review permissions regularly and remove access that is no longer being used. IAM Access Analyzer can identify permissions that have not been used in ninety or more days.

Always use groups to assign permissions. Attach policies to groups, add users to groups. This makes permissions easier to manage and easier to audit. When an employee changes roles, move them to the appropriate group rather than editing individual policy attachments.

Enable MFA for all users who have console access, and make it mandatory for users with administrative privileges. MFA adds a second factor — something you have — on top of the password, which is something you know. If a password is compromised, MFA prevents the attacker from logging in.

Use roles for applications, not access keys. Access keys embedded in application code are a major source of security incidents. Code gets committed to version control. Environment variables get exposed. The correct approach is to use IAM roles so that AWS handles credential management and rotation automatically.

Rotate credentials regularly. Access keys should be rotated every ninety days. Set a password expiration policy.

---

## Encryption: Protecting Your Data

The exam tests encryption in two dimensions: data at rest and data in transit. Know the options for both.

Encryption at rest protects data stored on disk. For Amazon S3, there are four encryption options. The first is Server-Side Encryption with S3-Managed Keys, known as SSE-S3. AWS manages the encryption keys using AES-256. There is no additional cost, and it requires a single configuration setting. This is the simplest option, appropriate when you need encryption but do not need to control the keys yourself.

The second S3 encryption option is Server-Side Encryption with KMS, known as SSE-KMS. AWS Key Management Service manages the encryption keys, but you control the key policies, who can use the keys, and whether keys rotate. SSE-KMS provides an audit trail via CloudTrail — every time a key is used to encrypt or decrypt data, that activity is logged. This is the right choice when compliance requirements demand an audit trail of key usage or when you need to control key rotation and access policies. It costs slightly more per request.

The third option is Server-Side Encryption with Customer-Provided Keys, known as SSE-C. You manage your own encryption keys completely outside of AWS. You must provide the key with every request. AWS performs the encryption but never stores your key. This is for scenarios where organizational policy requires keys to be managed entirely outside of AWS.

The fourth option is client-side encryption, where you encrypt the data yourself before uploading it to S3. You control the entire encryption process. The data arrives at S3 already encrypted.

For EBS volumes, encryption is managed through KMS. You enable it at volume creation time. An encrypted EBS volume encrypts data at rest on the disk and data in transit between the instance and the volume, and all snapshots created from the volume are also encrypted. You can enable EBS encryption by default at the account level.

For RDS, encryption must be enabled at database creation time and cannot be added to an existing unencrypted database after the fact. The workaround is to create a snapshot of the unencrypted database, copy that snapshot with encryption enabled, and restore from the encrypted snapshot. All backups, read replicas, logs, and snapshots of an encrypted RDS instance are also encrypted.

Encryption in transit protects data moving between systems. The standard is TLS, Transport Layer Security. HTTPS traffic uses TLS. AWS Certificate Manager provides free SSL and TLS certificates for use with CloudFront, Application Load Balancers, and API Gateway. ACM handles automatic certificate renewal, eliminating the manual process of buying, installing, and renewing certificates.

---

## AWS Key Management Service

AWS KMS creates and manages the cryptographic keys used to encrypt your data. It is integrated with over one hundred AWS services and backed by hardware security modules that are validated under FIPS 140-2, a US government cryptography standard.

There are three types of KMS keys. AWS Owned Keys are managed entirely by AWS and are used across multiple customers. You have no visibility into them and no control over them, but you also pay nothing for them. AWS Managed Keys are created and managed by AWS but are specific to your account. They automatically rotate once per year, and you cannot delete them, but they are free. Customer Managed Keys are created and managed by you. You define the key policies, you control who can use the key, you choose whether to enable automatic rotation annually, and you can enable, disable, or schedule deletion of the key with a waiting period of seven to thirty days. Customer Managed Keys cost one dollar per month per key plus charges for API usage.

When the exam describes a scenario requiring full control over key lifecycle, audit trails of key usage, and compliance requirements, the answer is a Customer Managed KMS Key.

---

## Network Security

Network security in AWS centers on two tools: security groups and network access control lists.

A security group is a virtual firewall that operates at the instance level. Security groups are stateful, which means that if you allow inbound traffic on port 443, the corresponding return traffic is automatically allowed outbound without you needing to write a separate outbound rule. By default, security groups deny all inbound traffic and allow all outbound traffic. You add rules to allow specific traffic in. You cannot add explicit Deny rules to a security group — you can only add Allow rules. To prevent traffic, you simply do not allow it.

A network access control list, or NACL, operates at the subnet level. Every subnet in a VPC is associated with a NACL. NACLs are stateless — if you allow inbound traffic on port 443, you must also explicitly allow the outbound return traffic. Rules are processed in numerical order, and the first matching rule is applied. NACLs can have explicit Deny rules, which security groups cannot. This makes NACLs useful when you need to block a specific IP address — something security groups cannot do. The exam will ask when to use a NACL versus a security group: when you need to explicitly deny traffic from a specific IP, or when you need subnet-level protection in addition to instance-level security groups, the answer is NACL.

In a well-architected VPC, you layer these tools. Public subnets hold resources that need internet access — load balancers, NAT gateways, and bastion hosts. Private subnets hold application servers that should not be directly accessible from the internet. Even more restricted private subnets hold databases that should only accept traffic from the application tier. Security groups enforce these tiers by referencing other security groups — the database security group allows traffic only from the application security group, not from any IP address.

VPC Endpoints are worth understanding for the exam. A Gateway Endpoint is a free route-table entry that allows your EC2 instances in private subnets to communicate with S3 or DynamoDB without their traffic going through the internet, a NAT gateway, or a VPN. It keeps traffic entirely within the AWS network. Interface Endpoints use PrivateLink technology and support many other AWS services — they create an elastic network interface in your subnet with a private IP address, and your traffic to that AWS service uses that private IP, never touching the internet.

---

## Security Services

Let me walk through the security services you must know for the exam, organized by what they do.

AWS Organizations manages multiple AWS accounts under a single hierarchy. You create organizational units — OUs — that group accounts by environment, department, or function. Organizations provides consolidated billing, meaning all accounts share a single bill and you can apply volume discounts and Reserved Instance sharing across the organization. The key governance feature is Service Control Policies, or SCPs. An SCP sets the maximum permissions available in an account or OU. It does not grant permissions — it limits them. Even if an IAM policy inside the account tries to allow something that the SCP denies, the SCP wins. SCPs apply to every user and role in the account, including the root user of that account. Use Organizations when the exam describes a company managing multiple AWS accounts with centralized governance.

AWS KMS I have already covered. Use it when the question involves encryption keys, audit trails of key usage, or compliance-driven key management.

AWS Shield protects against distributed denial of service attacks. Shield Standard is free and automatic for all AWS customers. It protects against common Layer 3 and Layer 4 attacks — SYN floods, UDP floods, reflection attacks — with always-on detection and automatic mitigation. Shield Advanced costs three thousand dollars per month per organization and provides enhanced protection for EC2, Elastic Load Balancing, CloudFront, Route 53, and Global Accelerator. It includes access to the DDoS Response Team twenty-four hours a day, protection against cost spikes caused by attack-driven scaling, and detailed real-time attack metrics. The exam distinction is clear: free and automatic is Shield Standard. Enhanced protection with expert support and a monthly cost is Shield Advanced.

Amazon GuardDuty is an intelligent threat detection service. It continuously monitors CloudTrail events, VPC Flow Logs, DNS logs, and other data sources using machine learning to identify suspicious and malicious activity. GuardDuty does not require you to install agents or change your network configuration. You enable it with a few clicks. It detects compromised EC2 instances communicating with command-and-control servers, unusual API call patterns that suggest a compromised IAM credential, cryptocurrency mining on your instances, and reconnaissance activity. GuardDuty findings can trigger automated responses via EventBridge and Lambda. There is a thirty-day free trial. When the exam describes detecting threats automatically using machine learning, the answer is GuardDuty.

Amazon Inspector is an automated security assessment service. It scans EC2 instances, container images in Elastic Container Registry, and Lambda functions for known software vulnerabilities and unintended network exposure. Inspector continuously monitors these resources and produces findings with severity ratings and remediation recommendations. The distinction from GuardDuty: GuardDuty detects active threats and suspicious behavior. Inspector finds vulnerabilities and configuration weaknesses — the door was left unlocked, not that someone tried to walk through it.

AWS WAF is a web application firewall. It protects web applications from common exploits like SQL injection and cross-site scripting. WAF is deployed on CloudFront distributions, Application Load Balancers, API Gateway, and AppSync. You create web access control lists — web ACLs — with rules that inspect incoming web requests and either allow or block them. WAF can filter based on IP addresses, geographic location, request content, and request rate. It is the correct answer when the exam describes protecting a web application from OWASP-style attacks.

Amazon Macie uses machine learning to discover, classify, and protect sensitive data stored in S3. It identifies personally identifiable information — names, addresses, Social Security numbers, credit card numbers, passport numbers — and generates findings when sensitive data is found in places it should not be. Macie also monitors S3 bucket configurations for public access settings and unusual access patterns. When the exam describes finding sensitive data in S3 or monitoring S3 for data privacy compliance, the answer is Macie.

AWS Artifact is not a security detection or protection service — it is a compliance document portal. Through Artifact, you can download AWS compliance reports and certifications on demand: ISO certifications, SOC reports, PCI DSS documentation, FedRAMP authorization packages, and agreements like Business Associate Agreements for HIPAA. It is free. When the exam describes a company that needs to provide evidence of AWS's compliance to an auditor, the answer is Artifact.

AWS Config is not primarily a security tool, but it plays a critical role in compliance. Config continuously records the configuration state of your AWS resources and tracks how those configurations change over time. Config Rules evaluate whether your resources comply with desired configurations — for example, whether all S3 buckets have encryption enabled, whether all EC2 instances have approved AMIs, whether the root account has MFA. When a resource falls out of compliance, Config can trigger automated remediation. CloudTrail tells you who made a change and when. Config tells you what the configuration was before and after the change. Use Config when the exam describes continuous compliance monitoring or tracking configuration history.

Security Hub provides a centralized view of security findings from GuardDuty, Inspector, Macie, Config, and other security services across your AWS accounts. Instead of checking each service separately, Security Hub aggregates findings and prioritizes them in a single dashboard.

Amazon Detective analyzes and visualizes security data to help you investigate the root cause of security findings. It integrates with GuardDuty findings and builds a graph of relationships between resources, users, and API activity over time, making investigation faster than manually correlating CloudTrail logs.

---

## Identity Federation

Not every user who needs to access AWS will have an IAM user. Large organizations have thousands of employees, and creating an IAM user for each one and managing those credentials separately from the corporate directory is impractical. Identity federation solves this by letting employees use their existing corporate credentials to access AWS.

AWS IAM Identity Center, formerly called AWS Single Sign-On, is the recommended solution for giving employees access to multiple AWS accounts and business applications with a single set of credentials. You connect IAM Identity Center to your corporate identity provider — Microsoft Active Directory, Okta, Azure AD, or others — and employees log in once and get access to all the AWS accounts they are authorized for. When the exam describes a company with multiple AWS accounts that wants employees to use their corporate credentials for access, IAM Identity Center is the answer.

SAML 2.0 federation is the underlying mechanism for connecting corporate identity providers to AWS. The flow is: the user authenticates with their corporate identity provider, the IdP returns a SAML assertion, and AWS Security Token Service exchanges that SAML assertion for temporary AWS credentials. This allows the company's existing identity infrastructure to gate access to AWS without creating IAM users.

Amazon Cognito handles identity for web and mobile applications, not corporate employees. Cognito has two parts. User Pools handle authentication — they store user accounts and handle sign-up, sign-in, and multi-factor authentication. Identity Pools handle authorization — they exchange identity tokens for temporary AWS credentials so authenticated users can call AWS services directly from the application. Cognito also supports social identity providers like Google and Facebook, and SAML providers. When the exam describes a mobile application that needs users to authenticate and then access AWS resources, the answer is Cognito.

AWS Directory Service manages Microsoft Active Directory in the cloud. AWS Managed Microsoft AD runs a full Active Directory in multiple Availability Zones managed by AWS. AD Connector acts as a proxy to an on-premises Active Directory without caching any data — all authentication still happens against the on-premises AD. Simple AD is a lower-cost directory for basic LDAP needs. When the exam describes Windows workloads or applications that require Active Directory integration, Directory Service is the answer.

---

## Compliance Programs

The exam expects you to know what each major compliance framework covers and which industry it applies to. You do not need to memorize the technical details of each framework, but you need to match industry keywords to the right compliance program.

HIPAA stands for the Health Insurance Portability and Accountability Act. It protects health information — specifically, Protected Health Information or PHI — in the United States. Healthcare providers, health insurers, and their business partners must comply with HIPAA. If you store or process PHI in AWS, you must sign a Business Associate Agreement with AWS, which you can obtain through AWS Artifact, and you must use only HIPAA-eligible services. When the exam mentions healthcare data, patient records, or PHI, think HIPAA.

PCI DSS stands for the Payment Card Industry Data Security Standard. It protects cardholder data — credit card numbers and related payment information — across merchants and payment processors. AWS infrastructure is PCI DSS compliant at the highest level, Level 1, but customer applications that process card data may require their own compliance validation. When the exam mentions credit card data, payment processing, or cardholder information, think PCI DSS.

SOC stands for Service Organization Controls. There are three reports. SOC 1 covers controls relevant to financial reporting. SOC 2 covers controls related to security, availability, processing integrity, confidentiality, and privacy, and comes in two types: Type 1 evaluates the design of controls at a specific point in time, while Type 2 evaluates whether those controls are operating effectively over a period of six to twelve months. SOC 3 is a simplified, publicly available version of SOC 2. You download these reports from AWS Artifact.

ISO 27001 is an international standard for information security management systems. It provides a risk management framework with fourteen control domains covering everything from access control and cryptography to incident management and compliance. AWS holds ISO 27001 certification as well as ISO 27017 for cloud security and ISO 27018 for cloud privacy.

FedRAMP stands for the Federal Risk and Authorization Management Program. It is the US government's cloud security standard, mandatory for federal agencies procuring cloud services. FedRAMP has three authorization levels — Low, Moderate, and High — based on the potential impact of a data breach. AWS GovCloud regions are authorized at the High impact level.

GDPR stands for the General Data Protection Regulation. It is the European Union's data privacy regulation, effective since 2018. It applies to any organization that processes the personal data of EU residents, regardless of where the organization is based. Key requirements include lawful basis for processing, the right of individuals to access and delete their data, data minimization, and mandatory breach notification to the supervisory authority within seventy-two hours. Non-compliance can result in fines of up to four percent of annual global revenue or twenty million euros. When the exam mentions EU data, data privacy, or the right to be forgotten, think GDPR.

The quick-recall pattern for the exam: healthcare and PHI maps to HIPAA. Payment cards and cardholder data maps to PCI DSS. US federal agencies maps to FedRAMP. EU residents and data privacy maps to GDPR. Audit reports and financial controls maps to SOC. International security standard maps to ISO 27001.

---

## Security Incident Response

The exam tests the basic framework for responding to a security incident. The framework has six phases, and for the exam, knowing the sequence and the tools used at each phase is what matters.

The first phase is Preparation. Before an incident occurs, you set up your tools, document your response procedures, define severity levels, identify who is responsible for what, and practice with tabletop exercises. GuardDuty should be enabled. CloudTrail should be running in all regions. VPC Flow Logs should be enabled. You should know what normal looks like before an incident so you can recognize what abnormal looks like during one.

The second phase is Detection and Analysis. Something triggers an alert — a GuardDuty finding, a CloudWatch alarm, an unusual CloudTrail pattern, or a Config rule violation. You confirm whether the alert is real or a false positive, determine the scope and severity, identify which resources are affected, and begin collecting evidence.

The third phase is Containment. You stop the situation from getting worse. Short-term containment means isolating the affected resource — for example, moving a compromised EC2 instance to a security group that denies all traffic. You revoke any compromised credentials. You block malicious IP addresses with NACLs. You take snapshots of affected resources for forensic investigation before making changes that might destroy evidence.

The fourth phase is Eradication. Once the threat is contained, you eliminate it — remove malware, close backdoors, patch the vulnerability that was exploited, and rebuild compromised systems from known-good AMIs.

The fifth phase is Recovery. You restore systems to normal operation, monitor closely for signs of reinfection, and gradually restore services while validating that everything is working correctly.

The sixth phase is the Post-Incident Review, or lessons learned. You document what happened, what worked, what did not work, and what changes you will make to prevent recurrence. This is where you improve your preparation for the next incident.

The AWS services most important for incident response are GuardDuty for automated threat detection, CloudTrail for forensic analysis of API activity, VPC Flow Logs for network traffic analysis, AWS Config for understanding what the configuration was at the time of the incident, Amazon Detective for visualizing relationships and investigating root causes, and AWS Systems Manager for automated remediation actions.

---

## Common Security Mistakes

The exam sometimes presents scenarios where a practice is described and you must identify whether it is correct or incorrect. These are the most frequently tested mistakes.

The most dangerous and most tested mistake is using the root account for daily tasks. The root account has unlimited access and cannot be restricted. Create IAM users for daily work and lock the root credentials away with MFA enabled.

Overly permissive IAM policies are the second most common mistake. A policy that allows all actions on all resources grants administrator-equivalent access to whoever it is attached to. Always scope policies to the specific services, actions, and resources actually needed.

Hardcoding credentials in application code is a critical mistake. Access keys in source code get committed to version control and exposed. Use IAM roles so that AWS provides and rotates credentials automatically.

Leaving S3 buckets publicly accessible has caused many high-profile data breaches. Enable S3 Block Public Access at the account level to prevent any bucket in your account from being made public accidentally.

Not enabling MFA means that a stolen password alone is enough to compromise an account. Enable MFA for all users, especially administrators.

Ignoring CloudTrail logs means you have no audit trail when an incident occurs. Enable CloudTrail in all regions, store logs in a separate secured account, and set up alerts for critical events like root account usage, IAM policy changes, and security group modifications.

Poor security group configuration — opening port 22 or port 3389 to all IP addresses — exposes instances to brute-force attacks. Restrict SSH and RDP access to specific IP ranges or use Systems Manager Session Manager to eliminate the need to expose these ports at all.

Not encrypting data violates the principle of data protection and many compliance requirements. Enable encryption by default for all new EBS volumes, S3 buckets, and RDS databases.

Sharing IAM credentials between people or applications destroys accountability. You cannot tell who did what if multiple people share a single user. Create individual accounts and use roles for service-to-service access.

Neglecting security updates leaves known vulnerabilities open. Use AWS Systems Manager Patch Manager to automate OS patching across your fleet.

Not applying least privilege and accumulating permissions over time creates accounts with far more access than they need. Review permissions regularly using IAM Access Analyzer.

Poor network segmentation — putting all resources in public subnets with no separation between tiers — means that a compromise of one resource can affect everything. Use public subnets for load balancers only, private subnets for application servers, and even more restricted private subnets for databases.

---

## Review Questions

Let me work through the review questions with you now. I will read each question, give you a moment to think, and then explain the reasoning.

Question one: According to the Shared Responsibility Model, which security aspect is AWS responsible for? Option A, security group configuration. Option B, physical security of data centers. Option C, customer data encryption. Option D, IAM user management.

The answer is B. Physical security of data centers is security OF the cloud — AWS's responsibility. Security groups, data encryption, and IAM user management are all security IN the cloud — the customer's responsibility.

Question two: Which service provides DDoS protection at no additional cost? Option A, AWS WAF. Option B, AWS Shield Advanced. Option C, AWS Shield Standard. Option D, Amazon GuardDuty.

The answer is C. Shield Standard is free and automatic for all customers. Shield Advanced costs three thousand dollars per month. WAF has its own pricing. GuardDuty is a threat detection service, not a DDoS protection service.

Question three: What is the best practice for granting permissions to a group of developers? Option A, attach policies directly to each user. Option B, create an IAM group, attach policies to the group, add users to the group. Option C, share the root account credentials. Option D, create one IAM user that everyone shares.

The answer is B. Groups are the correct mechanism for managing permissions for multiple people with the same job function. Attaching policies to individual users is harder to maintain. Sharing any credentials, root or otherwise, is never correct.

Question four: Which service uses machine learning to discover and protect sensitive data in S3? Option A, Amazon GuardDuty. Option B, Amazon Inspector. Option C, Amazon Macie. Option D, AWS Config.

The answer is C, Macie. GuardDuty detects threats. Inspector finds software vulnerabilities. Config tracks configuration changes. Macie specifically discovers and classifies sensitive data like PII in S3.

Question five: Which IAM entity provides temporary security credentials? Option A, IAM User. Option B, IAM Group. Option C, IAM Role. Option D, IAM Policy.

The answer is C, a Role. Roles provide temporary credentials that are automatically rotated. Users have long-term credentials. Groups are just collections of users. Policies define permissions but do not generate credentials.

Question six: Where can you download AWS compliance reports and certifications? Option A, AWS Config. Option B, AWS Artifact. Option C, AWS Inspector. Option D, AWS Organizations.

The answer is B, AWS Artifact. This is the self-service compliance document portal. It is free and available to all AWS customers.

Question seven: What is the primary purpose of AWS Config? Option A, encrypt data at rest. Option B, track configuration changes and compliance. Option C, detect threats using machine learning. Option D, protect against DDoS attacks.

The answer is B. Config is the configuration historian. KMS handles encryption. GuardDuty detects threats. Shield handles DDoS.

Question eight: Which authentication factor does MFA add to a username and password? Option A, something you know. Option B, something you have. Option C, something you are. Option D, somewhere you are.

The answer is B. A password is something you know. MFA adds something you have — the physical or virtual device that generates the one-time code. Biometrics would be something you are, and location-based access would be somewhere you are.

Question nine: Which service would you use to centrally manage multiple AWS accounts and apply governance policies? Option A, IAM. Option B, AWS Organizations. Option C, AWS Config. Option D, AWS Control Tower.

The answer is B, AWS Organizations. IAM manages access within a single account. Config tracks configurations. Organizations provides the multi-account hierarchy and the Service Control Policies that enforce governance across all accounts.

Question ten: Which compliance program is specifically for healthcare data in the United States? Option A, PCI DSS. Option B, GDPR. Option C, HIPAA. Option D, SOC 2.

The answer is C, HIPAA. PCI DSS covers payment card data. GDPR covers EU data privacy. SOC 2 is a general security audit. HIPAA specifically covers US healthcare data and Protected Health Information.

Question eleven: Which AWS service should you use to discover and protect sensitive data like credit card numbers in S3? Option A, AWS Config. Option B, Amazon Macie. Option C, AWS WAF. Option D, Amazon Inspector.

The answer is B, Macie. This distinction is important enough that the exam asks it multiple ways. Macie is for finding sensitive data in S3. The other services do not do this.

Question twelve: Your company needs to encrypt data at rest in S3 with full control over the encryption keys, including rotation. Which solution should you use? Option A, SSE-S3. Option B, SSE-KMS with customer managed CMK. Option C, SSE-C. Option D, client-side encryption.

The answer is B. Customer managed CMKs give you full control over key policies and rotation while AWS handles the encryption process. SSE-S3 gives you no key control. SSE-C requires you to provide the key with every request, which is more complex and does not give you built-in rotation. Client-side encryption requires you to manage everything.

Question thirteen: Which service provides automated vulnerability assessment for EC2 instances and container images? Option A, Amazon GuardDuty. Option B, AWS Security Hub. Option C, Amazon Inspector. Option D, AWS Systems Manager.

The answer is C, Inspector. GuardDuty is for threat detection based on behavioral analysis. Security Hub aggregates findings from multiple services. Systems Manager handles operational tasks. Inspector specifically scans for software vulnerabilities and network exposure.

Question fourteen: According to the Shared Responsibility Model, who is responsible for patching the guest operating system on an EC2 instance? Option A, AWS. Option B, Customer. Option C, Both. Option D, Neither.

The answer is B, the Customer. EC2 is IaaS — AWS manages the hypervisor and physical infrastructure, and you manage the operating system and everything above it. Compare this to RDS, where AWS patches the database engine because RDS is a managed service.

Question fifteen: Which feature of AWS Organizations allows you to restrict actions across all accounts in your organization? Option A, IAM Policies. Option B, Resource Access Manager. Option C, Service Control Policies. Option D, Permission Boundaries.

The answer is C, Service Control Policies. SCPs set the maximum permissions available in an account, limiting what even the root user of that account can do. IAM Policies operate within a single account. Resource Access Manager shares resources between accounts. Permission Boundaries limit individual IAM entities within an account.

Question sixteen: What is the primary purpose of AWS CloudTrail? Option A, monitor resource utilization. Option B, log API activity for auditing. Option C, detect security threats. Option D, track configuration changes.

The answer is B. CloudTrail is the API audit log — who did what, when, and from where. CloudWatch monitors performance. GuardDuty detects threats. Config tracks configuration history. Know these four apart cold.

Question seventeen: Which of the following is NOT a valid MFA device option for AWS? Option A, virtual MFA device. Option B, hardware MFA device. Option C, SMS text message. Option D, fingerprint scanner.

The answer is D. AWS does not support biometric authentication for MFA. Virtual MFA devices like Google Authenticator and Authy, hardware tokens like YubiKey, and SMS text messages are all supported AWS MFA options, though SMS is not recommended for the root account.

Question eighteen: A startup is building a web application that needs to authenticate users via Facebook and Google. Which AWS service should they use? Option A, AWS IAM. Option B, Amazon Cognito. Option C, AWS Directory Service. Option D, AWS IAM Identity Center.

The answer is B, Cognito. Cognito supports social identity provider authentication for web and mobile applications. IAM is for controlling access to AWS resources by people and services, not for authenticating application end users. Directory Service is for Microsoft Active Directory integration. IAM Identity Center is for employees accessing AWS accounts.

Question nineteen: Which encryption option for S3 provides an audit trail of when keys were used and by whom? Option A, SSE-S3. Option B, SSE-KMS. Option C, SSE-C. Option D, client-side encryption.

The answer is B, SSE-KMS. Because KMS integrates with CloudTrail, every use of a KMS key is logged — who used it, when, and for what purpose. SSE-S3 manages keys internally without this visibility. SSE-C and client-side encryption give you the keys but not the built-in audit trail.

Question twenty: What is the purpose of VPC Flow Logs? Option A, log API calls in your VPC. Option B, capture network traffic information. Option C, monitor VPC configuration changes. Option D, detect malware in network traffic.

The answer is B. VPC Flow Logs capture metadata about IP traffic flowing through your VPC network interfaces — source IP, destination IP, ports, protocol, and whether the traffic was accepted or rejected. CloudTrail logs API calls. Config monitors configurations. GuardDuty analyzes flow logs to detect threats, but the logs themselves do not detect malware.

Question twenty-one: Which compliance program is specifically designed for US federal government agencies? Option A, HIPAA. Option B, PCI DSS. Option C, FedRAMP. Option D, SOC 2.

The answer is C, FedRAMP. HIPAA is healthcare. PCI DSS is payment cards. SOC 2 is a general security audit. FedRAMP is the US government's cloud security standard.

Question twenty-two: A company wants to share an encrypted S3 bucket with another AWS account. What must be configured? Option A, S3 bucket policy only. Option B, KMS key policy and S3 bucket policy. Option C, IAM role only. Option D, VPC peering.

The answer is B. To share an encrypted S3 bucket cross-account, you need both pieces: the KMS key policy must allow the other account to decrypt, and the S3 bucket policy must allow the other account to access the objects. Either one alone is insufficient. VPC peering is not involved in S3 access control.

Question twenty-three: Which service protects against DDoS attacks at no additional cost? Option A, AWS WAF. Option B, AWS Shield Advanced. Option C, AWS Shield Standard. Option D, Amazon GuardDuty.

The answer is C, Shield Standard. This question is a repeat of question two with slightly different wording, which is exactly what the exam does. Shield Standard is free. Shield Advanced is paid. GuardDuty is threat detection, not DDoS protection.

Question twenty-four: What is the difference between security groups and Network ACLs? Option A, security groups are stateful; NACLs are stateless. Option B, security groups are stateless; NACLs are stateful. Option C, both are stateful. Option D, both are stateless.

The answer is A. Security groups are stateful — return traffic is automatically allowed without an explicit outbound rule. NACLs are stateless — you must explicitly allow both inbound and outbound traffic. This distinction is critical for the exam.

Question twenty-five: A company must ensure all data stored in AWS is encrypted at rest and in transit. Which services should they use? Choose two. Option A, AWS KMS for encryption at rest. Option B, AWS CloudHSM for encryption in transit. Option C, TLS and SSL for encryption in transit. Option D, AWS Certificate Manager for encryption at rest.

The answers are A and C. KMS manages encryption keys for data at rest across S3, EBS, RDS, and other services. TLS and SSL protect data in transit. CloudHSM provides hardware-based key storage but is not specifically for transit encryption. ACM provides certificates that enable TLS but does not encrypt data at rest.

---

## Key Patterns to Carry Forward

For the Shared Responsibility Model: everything about infrastructure, physical security, and managed service runtimes is AWS. Everything about customer data, IAM configuration, security groups, OS patching on EC2, and encryption choices is the customer.

For IAM: root account is for initial setup only. Users for people, groups for permission management, roles for services and temporary access. Explicit Deny always wins over Explicit Allow, which wins over the implicit Deny default.

For encryption: SSE-S3 is simple with no key control. SSE-KMS gives you control and audit trails. Customer Managed KMS gives you full lifecycle control. TLS and ACM handle in-transit encryption.

For security services: GuardDuty detects threats with machine learning. Inspector finds software vulnerabilities. Macie finds sensitive data in S3. Shield Standard is free DDoS protection. Shield Advanced is paid with expert support. WAF protects web applications from OWASP attacks. Config tracks configuration compliance. Artifact provides compliance documents. Security Hub centralizes all findings.

For compliance: healthcare is HIPAA. Payment cards is PCI DSS. US government is FedRAMP. EU data privacy is GDPR. Audit reports are SOC. International security standard is ISO 27001.

For network security: security groups are stateful, instance-level, and can only Allow. NACLs are stateless, subnet-level, and can explicitly Deny. Use NACLs when you need to block a specific IP.

For incident response: Prepare, Detect, Contain, Eradicate, Recover, and review lessons learned. GuardDuty detects. CloudTrail investigates. Flow Logs analyze network activity. Systems Manager remediates.

Security always wins over cost and convenience in exam questions. When a scenario presents a tradeoff and one option is more secure, choose the more secure option.

---
