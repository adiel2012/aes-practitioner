# Lesson 9: Exam Scenarios and Real-World Solutions

---

Welcome to the scenario lesson. This is where everything comes together. In the previous lessons you learned what each AWS service does, how services compare to each other, and how to choose between them. Now we are going to use all of that knowledge in the context of real-world situations.

The AWS Cloud Practitioner exam is heavily scenario-based. It does not just ask you to define services. It describes a situation, gives you constraints, and asks which approach is correct. This lesson walks you through twelve realistic scenarios, and for each one I will explain the situation, identify the key insight that unlocks the right answer, and describe what AWS recommends and why.

After the twelve main scenarios, I will cover the most common troubleshooting situations the exam tests. These are the scenarios where something has gone wrong and you need to identify the cause and the fix.

Read through each scenario carefully. The goal is not to memorize solutions. The goal is to train your thinking so that when you see a similar situation on the exam, you recognize the pattern and know which direction to move.

---

## Scenario One: Cost Optimization for Predictable Workloads

The situation: A company runs a web application on EC2 instances. Traffic follows a very predictable pattern — busy Monday through Friday during business hours, quiet on nights and weekends. They are currently paying for ten instances running twenty-four hours a day, seven days a week, using On-Demand pricing.

The key insight: On-Demand pricing is designed for variable and unpredictable workloads. When usage is predictable and consistent, it is almost always the most expensive option. This scenario has two distinct optimization opportunities: the base capacity that runs during business hours, and the zero-or-minimal capacity needed overnight and on weekends.

The AWS recommendation is to combine Reserved Instances for the minimum baseline capacity with scheduled Auto Scaling to adjust the number of instances based on the time of day. For the minimum number of instances that will always need to run, Reserved Instances offer up to seventy-two percent savings compared to On-Demand. For the additional capacity needed only during business hours, Auto Scaling can be configured to launch instances before the workday starts and terminate them after it ends.

The exam lesson from this scenario: whenever you see the words predictable traffic or consistent workload, think Reserved Instances or Savings Plans. Whenever you see business hours only or known schedule, think scheduled Auto Scaling. Whenever you see weekend spikes with unpredictable timing, think Spot Instances for fault-tolerant workloads.

---

## Scenario Two: Designing for High Availability

The situation: An e-commerce company runs their application on a single EC2 instance with a single MySQL database instance. The company has received a requirement to design the application so that it stays available even if an entire Availability Zone fails.

The key insight: Single points of failure must be eliminated. In this scenario there are at least three: the single EC2 instance, the single database, and the fact that if session data is stored on the instance, it is lost when that instance fails. A single Availability Zone failure will take down all three.

The AWS recommendation follows a four-part pattern that appears in many high availability architectures. First, place an Application Load Balancer in front of multiple EC2 instances spread across at least two Availability Zones. If one zone goes down, the load balancer continues to send traffic to the healthy zone. Second, replace the single MySQL instance with Amazon RDS configured with Multi-AZ. RDS Multi-AZ maintains a synchronous standby replica in a different Availability Zone and automatically promotes it if the primary fails. The DNS endpoint stays the same, so applications reconnect without needing to change their configuration. Third, store session data in ElastiCache or DynamoDB rather than on the EC2 instance, so that any instance can serve any user. This makes the application stateless, meaning any instance can handle any request. Fourth, serve static assets from S3 and CloudFront, both of which are inherently highly available.

The exam lesson from this scenario: high availability almost always involves spreading resources across multiple Availability Zones, removing single points of failure, and making applications stateless. When a question asks how to survive an Availability Zone failure, the answer will involve multiple AZs, a load balancer, and typically RDS Multi-AZ.

---

## Scenario Three: Large Data Migration Under a Deadline

The situation: A healthcare company needs to migrate eighty terabytes of medical imaging data from their on-premises storage to S3. Their internet connection is one hundred megabytes per second. They have a two-week deadline. HIPAA compliance requires encryption.

The key insight: A one-hundred-megabyte-per-second connection sounds fast, but transferring eighty terabytes over the internet at that speed would take over seventy days. The deadline is two weeks. Internet-based transfer is not physically possible within the timeframe.

This is the exact situation the AWS Snow Family was designed for. When the math shows that transferring data over the internet would take weeks or months, the Snow Family allows you to physically transfer the data. AWS ships a Snowball Edge device to the company. The team copies the data onto the device over their local network, which is many times faster than their internet connection. The device is then shipped back to AWS, where the data is loaded into S3. The entire process takes one to two weeks.

Snowball Edge devices include hardware encryption built in, satisfying the HIPAA encryption requirement. For HIPAA compliance specifically, the company would also need to sign a Business Associate Addendum with AWS and enable CloudTrail for audit logging.

The general rule for the Snow Family: if the data volume would take more than a week to transfer over the available internet connection, consider Snowball or Snowball Edge. If the data exceeds eighty terabytes or there are multiple locations, consider using more than one device. If the data reaches petabyte or exabyte scale, Snowmobile is the answer — a truck-sized container that can hold up to one hundred petabytes.

The exam lesson: whenever a scenario describes a large data migration with a tight deadline and a slow or limited internet connection, Snow Family is the answer. Calculate the transfer time when the numbers are provided. If the math shows it would take longer than the deadline, the internet is not the right path.

---

## Scenario Four: Serverless Mobile App Backend

The situation: A startup wants to build the backend for a mobile application. They want to minimize operational overhead, pay only for actual usage, and scale from zero to millions of users without managing infrastructure.

The key insight: this scenario describes the ideal use case for serverless architecture. The company does not want to manage servers. They want automatic scaling. They want pay-per-use pricing. Each of those requirements points directly to managed and serverless services.

The recommended architecture for this type of application follows a well-established pattern. API Gateway handles all incoming API requests from the mobile application. It manages request throttling, API keys, caching, and CORS. Lambda functions contain the business logic and run in response to each API request, with no servers to manage and automatic scaling built in. Amazon Cognito handles user sign-up, sign-in, and authentication. It supports social login providers like Google and Facebook and provides both user pools for authentication and identity pools for granting access to AWS resources. DynamoDB stores the application data, providing single-digit millisecond latency at any scale, with on-demand capacity mode so you pay per request rather than provisioning capacity. S3 stores user-uploaded content like images and files. CloudFront distributes that content globally.

This pattern is sometimes called the serverless web application pattern. It appears frequently on the exam and in real architectures.

The exam lesson: when a question describes no server management, pay per use, automatic scaling, and a web or mobile backend, the answer almost always involves some combination of API Gateway, Lambda, DynamoDB, Cognito, and S3. The specific mix depends on the requirements.

---

## Scenario Five: Compliance and Governance Across Many Accounts

The situation: A financial services company has fifty AWS accounts. They need to ensure that no S3 bucket in any account is publicly accessible. They need to monitor this continuously and demonstrate compliance to auditors.

The key insight: enforcing and monitoring policies at scale across many accounts requires centralized governance tools. Individual account-by-account management does not scale and creates gaps. The question is about which AWS services provide organization-wide enforcement and visibility.

AWS Organizations is the management structure for multiple accounts. Within Organizations, Service Control Policies can be applied to the entire organization or to subsets of accounts called Organizational Units. A Service Control Policy that denies the ability to create publicly accessible S3 buckets will apply regardless of what individual account administrators do. Even an administrator with full administrator permissions in their account cannot override a Service Control Policy. It defines the maximum permissions that anyone in the account can have.

S3 Block Public Access can be enabled at the organizational level through AWS Organizations, preventing any bucket in any account from being made public.

AWS Config with its compliance rules continuously monitors every S3 bucket in every account and flags any that are publicly accessible. Config can be aggregated across all accounts through a Config Aggregator deployed in a central management account.

Security Hub aggregates findings from Config, GuardDuty, Inspector, Macie, and other services into a single dashboard, providing a unified view of security and compliance posture across the entire organization. CloudTrail provides the audit trail showing every API call made in every account.

The exam lesson: multi-account governance uses Organizations with Service Control Policies for preventive controls, Config for continuous monitoring and compliance checking, Security Hub for centralized visibility, and CloudTrail for audit logging. When a question describes enforcing a security policy across dozens of accounts, this combination of services is the pattern to recognize.

---

## Scenario Six: Choosing a Disaster Recovery Strategy

The situation: An e-commerce company needs a disaster recovery plan for their application running in US East North Virginia. The business requires that no more than one hour of data can be lost in the event of a disaster, and that the application can be restored within four hours.

The key insight: disaster recovery strategy is chosen based on two numbers. The Recovery Point Objective tells you the maximum amount of data loss that is acceptable, measured as time. One hour RPO means you can lose no more than one hour of transactions. The Recovery Time Objective tells you how quickly the application must be back online. Four hours RTO means the application must be serving users within four hours of a disaster.

AWS defines four main disaster recovery strategies, ordered from simplest and cheapest to most complex and expensive.

Backup and Restore is the simplest. Data is backed up to S3 regularly and the entire infrastructure is rebuilt from scratch when disaster strikes. This approach typically results in days of RPO and days of RTO, which does not meet the requirements here.

Pilot Light keeps the core of the application, typically just the database, running continuously in the DR region. The rest of the infrastructure is turned off but can be launched quickly from pre-built templates. This typically achieves RPO of minutes to one hour and RTO of one to four hours. This matches the requirements.

Warm Standby runs a scaled-down but fully functional version of the application in the DR region at all times. It can handle some production traffic and can scale up quickly. This achieves RPO of seconds to minutes and RTO of minutes to one hour. This exceeds the requirements and costs more than necessary.

Multi-Site Active/Active runs full production capacity in multiple regions simultaneously. Failover is nearly instantaneous. This is the most expensive option and far exceeds what the scenario requires.

For this scenario, Pilot Light is the right answer. It meets the one-hour RPO by continuously replicating data to the DR region. It meets the four-hour RTO because the scaled-down infrastructure can be launched and traffic rerouted using Route 53 health checks and failover routing within that window.

The exam lesson: match the DR strategy to the RPO and RTO requirements. Backup and Restore for the loosest requirements, Pilot Light for moderate requirements, Warm Standby for tighter requirements, and Active/Active for near-zero requirements. The strategy should meet the requirements without exceeding them, because each step up the chain significantly increases cost.

---

## Scenario Seven: Hybrid Cloud Connectivity

The situation: A manufacturing company wants to connect their on-premises data center to AWS for their ERP system. They need consistent network performance, a private connection that does not use the public internet, and one gigabit per second of bandwidth.

The key insight: there are two main options for connecting on-premises networks to AWS. A Site-to-Site VPN creates an encrypted tunnel over the public internet. It can be set up in hours, is cost-effective, and provides encryption by default. However, because it uses the internet, the bandwidth and latency vary depending on internet conditions. AWS Direct Connect is a dedicated private network connection between the data center and AWS. It does not use the public internet. It provides consistent bandwidth and predictable latency. It takes weeks to provision and costs more than a VPN.

This scenario specifies consistent network performance, private connection, and one gigabit per second. Each of those requirements points to Direct Connect. The internet-based VPN option fails on consistency and potentially on bandwidth guarantees.

Direct Connect requires working with an AWS Direct Connect partner to establish a physical cross-connect at a colocation facility. A Direct Connect Gateway can then provide access to multiple VPCs across multiple regions through a single Direct Connect connection.

For high availability, best practice is to have two Direct Connect connections from different locations, or to use a VPN as a backup connection that activates if the Direct Connect link fails.

The exam lesson: Direct Connect is for consistent performance, high bandwidth, and private connectivity. VPN is for quick setup, lower cost, and situations where some variability is acceptable. If the question mentions variable latency, limited bandwidth, or just needs basic connectivity fast, VPN is the answer. If the question mentions consistent bandwidth, private connectivity, or replacing a dedicated circuit, Direct Connect is the answer.

---

## Scenario Eight: Multi-Region Architecture for a Global Application

The situation: A photo-sharing social media application currently runs entirely in one AWS region. Users in Asia and Europe experience over two hundred milliseconds of latency. The company expects to grow from one hundred thousand users to five million users in six months and needs to serve all regions with low latency.

The key insight: a single-region deployment cannot efficiently serve users on multiple continents. Content must be closer to the users who consume it. Data must be replicated across regions so that reads can be served locally. Traffic must be routed to the nearest healthy region.

The recommended multi-region architecture uses several AWS services that work together. CloudFront is the content delivery network that caches content at edge locations worldwide, reducing latency for static content like photos, videos, JavaScript, and CSS files from hundreds of milliseconds to tens of milliseconds. S3 Cross-Region Replication automatically copies objects from buckets in one region to buckets in other regions, so each region has a local copy of the content. DynamoDB Global Tables provides a multi-master database that replicates data across multiple regions, allowing writes to be accepted in any region and automatically synchronized to all others. Route 53 with geolocation routing directs users to the nearest regional endpoint, and with health checks configured, it automatically redirects traffic away from a region that becomes unavailable.

There are important trade-offs to understand. DynamoDB Global Tables use eventual consistency for replication, meaning there is a short window, typically under one second, where different regions might show slightly different data. This is acceptable for most social media use cases but would not be acceptable for financial transactions. Multi-region architecture also significantly increases cost compared to single-region deployment and adds operational complexity.

The exam lesson: low latency for global users involves CloudFront for content delivery and edge caching, S3 Cross-Region Replication for data distribution, Route 53 for intelligent routing, and DynamoDB Global Tables or Aurora Global Database for data that must be read from multiple regions. The specific services depend on whether the latency concern is for static content, dynamic API responses, or database reads.

---

## Scenario Nine: Security Incident Response and Overhaul

The situation: A healthcare company suffered a security incident where an S3 bucket containing patient data was briefly made publicly accessible. The company has twenty-five AWS accounts with inconsistent security practices, no centralized monitoring, and a reactive security posture. The requirement is to build a proactive, comprehensive security architecture.

The key insight: this scenario asks you to recognize which AWS security services address which types of security concerns. The company needs threat detection, preventive controls, continuous compliance monitoring, and automated incident response.

For threat detection, Amazon GuardDuty continuously analyzes VPC flow logs, DNS logs, and CloudTrail events using machine learning to identify suspicious behavior like compromised instances, unauthorized access attempts, and data exfiltration. Amazon Macie uses machine learning to scan S3 buckets and identify sensitive data such as personally identifiable information and protected health information. Amazon Inspector scans EC2 instances and container images for known software vulnerabilities.

For preventive controls, Service Control Policies in AWS Organizations prevent any account in the organization from disabling security services or creating publicly accessible S3 buckets. This creates a policy floor that no account administrator can override. AWS IAM Access Analyzer continuously monitors IAM policies and identifies resources that are shared with external entities or that grant excessive permissions.

For continuous compliance monitoring, AWS Config with compliance rules continuously checks whether S3 buckets have public access blocked, whether encryption is enabled, whether CloudTrail is active, and dozens of other security-relevant configurations. A Config Aggregator can provide this view across all twenty-five accounts from a single central security account.

For centralized visibility, AWS Security Hub aggregates findings from GuardDuty, Inspector, Macie, Config, and IAM Access Analyzer into a single dashboard. It also runs compliance checks against known frameworks including CIS, PCI DSS, and HIPAA.

For automated incident response, EventBridge rules can detect events like a public S3 bucket and trigger a Lambda function that automatically blocks public access and sends an alert. This closes the detection-to-response window from hours to seconds.

The exam lesson from this scenario: the security services each address a distinct concern. GuardDuty detects active threats and anomalous behavior. Macie identifies sensitive data exposure in S3. Inspector finds vulnerabilities before attackers exploit them. Config monitors configuration compliance. Security Hub provides the central view. CloudTrail provides the audit trail for who did what. When the exam describes a security requirement, identify which category it falls into and match the right service.

---

## Scenario Ten: Modernizing a Legacy Application

The situation: An insurance company runs a fifteen-year-old application on on-premises Windows servers. They want to move to AWS and modernize the architecture. The constraints are zero downtime during migration, maintained feature parity throughout, and a target of reducing infrastructure costs significantly.

The key insight: migrating a large, complex legacy application all at once is extremely high risk. The recommended approach is incremental modernization using a pattern called the Strangler Fig, named after a plant that gradually replaces a tree it grows around. You move pieces of the application to AWS one at a time while the rest continues to run on the original system, until eventually the original is entirely replaced.

The migration typically proceeds in phases. The first phase is lift and shift, moving the application to AWS with minimal changes. AWS Database Migration Service migrates the database to Amazon RDS with continuous replication to minimize downtime. The application servers are containerized using AWS App2Container, which analyzes existing applications and generates container images and deployment configurations. The containers run on Amazon ECS with Fargate, eliminating the need to manage EC2 instances. The original on-premises system continues to run in parallel while traffic is gradually shifted to the cloud.

The second phase extracts individual services from the monolith. High-value targets for early extraction are capabilities that have clear boundaries and would benefit most from independent scaling. A claims processing service might be extracted and rebuilt using Lambda and SQS for queue-based processing. A document handling service might be rebuilt to store documents in S3 and use Textract for data extraction. A notification service might use SNS and SES. As each service is extracted, an Application Load Balancer routes traffic to the right service based on the URL path, with all other traffic still going to the monolith.

The exam lesson: large-scale migration to AWS is not a single event. The Strangler Fig pattern is the recommended approach for incremental modernization. AWS DMS handles database migration. AWS App2Container assists with containerizing existing applications. ECS on Fargate provides container orchestration without managing servers. CodePipeline and CodeDeploy enable blue/green deployments with automatic rollback.

---

## Scenario Eleven: Big Data Analytics Platform

The situation: A retail company collects five terabytes of data per day from online transactions, mobile app usage, IoT sensors in stores, and social media. They want real-time dashboards for operations, regular batch reports, and machine learning for product recommendations.

The key insight: this scenario requires three different types of data processing: real-time streaming for operational dashboards, batch processing for scheduled reports, and machine learning for recommendations. Each type requires a different set of tools, but they all share a common foundation: a data lake built on Amazon S3.

For real-time data streaming, Amazon Kinesis Data Streams captures data as it arrives. Kinesis Data Firehose automatically delivers it to S3, Redshift, or other destinations with optional transformation using Lambda. Kinesis Data Analytics can run SQL queries on the stream in real time to power operational dashboards.

For batch processing, AWS Glue provides serverless extract, transform, and load capabilities. Glue Crawlers automatically discover the structure of data in S3 and populate the Glue Data Catalog, which is a centralized metadata repository. Amazon EMR runs complex Spark and Hadoop jobs for large-scale processing where you need the full open-source big data ecosystem.

For interactive querying, Amazon Athena lets analysts run SQL directly against data in S3 without loading it into a database. It is serverless and charges per terabyte of data scanned. Using columnar file formats like Parquet instead of CSV reduces costs by up to ninety percent and speeds up queries by a factor of nine or more. For complex analytical queries that need consistent performance, Amazon Redshift serves as the data warehouse.

For business intelligence and visualization, Amazon QuickSight creates dashboards and reports from Athena, Redshift, and other sources. It includes a built-in in-memory engine called SPICE for fast visualizations.

For machine learning, Amazon SageMaker trains and deploys custom models. Amazon Personalize provides a pre-built recommendation engine that does not require machine learning expertise.

Step Functions orchestrates the entire pipeline, coordinating Glue jobs, EMR clusters, and SageMaker training jobs into a managed workflow with error handling and retry logic.

The exam lesson: the data analytics architecture pattern on AWS follows this path: ingest with Kinesis or batch ingestion tools, store in S3 as a data lake, catalog with Glue, transform with Glue or EMR, query with Athena or Redshift, visualize with QuickSight, and build models with SageMaker or Personalize. The choice between Athena and Redshift depends on query frequency and performance requirements.

---

## Scenario Twelve: CI/CD Pipeline Implementation

The situation: A software company with twenty developers deploys code to production once a month. Deployments take four to six hours, involve many manual steps, and frequently require rollbacks. They want to deploy multiple times per day with automated testing and the ability to roll back in under five minutes.

The key insight: this scenario describes the classic DevOps transformation problem. The solution requires automating every step from code commit to production deployment, including build, test, and deployment stages, with automatic rollback capability.

AWS provides a suite of developer tools specifically for this. AWS CodeCommit is a managed Git repository. AWS CodeBuild is a fully managed build service that compiles code, runs tests, and creates deployment artifacts without managing build servers. AWS CodePipeline orchestrates the entire workflow, connecting source control to build to test to deployment, with approval gates where human review is required before production deployment. AWS CodeDeploy automates the deployment to EC2 instances, ECS services, Lambda functions, or on-premises servers.

Blue/green deployment is the key technique for zero-downtime deployments with fast rollback. In a blue/green deployment, the existing production environment, the blue environment, continues serving traffic. A new version of the application, the green environment, is deployed alongside it. Traffic is gradually shifted from blue to green, for example ten percent first, then fifty percent, then one hundred percent. CloudWatch alarms monitor error rates and response times throughout the shift. If any alarm fires, CodeDeploy automatically rolls back by shifting traffic back to the blue environment. This can happen in under five minutes because no data needs to be moved — the blue environment is still intact.

The exam lesson: CI/CD on AWS uses CodeCommit for source control, CodeBuild for build and test, CodePipeline for orchestration, and CodeDeploy for deployment. Blue/green deployments provide zero downtime and fast rollback. Automatic rollback is triggered by CloudWatch alarms. Infrastructure is defined as code using CloudFormation or AWS CDK so that environments are consistent and reproducible.

---

## Troubleshooting Scenarios

The exam includes troubleshooting questions where a configuration has a problem and you must identify the cause and the fix. Let me walk you through the most common troubleshooting patterns.

---

### Cannot Connect to an EC2 Instance

When you cannot connect to an EC2 instance via SSH or Remote Desktop, there are four layers to check in order.

The first layer is the security group. Security groups are stateful firewalls at the instance level. An SSH connection requires an inbound rule on port twenty-two. A Remote Desktop connection requires port thirty-three eighty-nine. If neither rule exists, or if the rule restricts the source to an IP address you are not connecting from, the connection will fail. This is the most common cause.

The second layer is the network ACL. Unlike security groups, network ACLs are stateless, meaning you need both inbound and outbound rules. They apply at the subnet level. If the network ACL blocks inbound traffic on port twenty-two, or if it blocks the outbound ephemeral port range that SSH responses use, the connection fails even with a correct security group.

The third layer is the network configuration. If you are connecting from the internet, the instance needs a public IP address, must be in a public subnet, and that subnet's route table must have a route to an internet gateway. If any of those three things is missing, the connection will time out.

The fourth layer is the key pair. For SSH, you must use the private key that matches the public key the instance was launched with. The private key file must have permissions that prevent other users from reading it.

---

### S3 Access Denied Errors

S3 access denied errors have four common causes.

The first is an IAM policy that does not grant the required permission. Check whether the user or role has an IAM policy that allows the S3 actions being attempted on the specific bucket.

The second is a bucket policy that denies access or that does not grant access when the bucket is not owned by the calling account. Check the bucket policy for explicit deny statements or for conditions that prevent access.

The third is Block Public Access settings. If you are trying to make a bucket publicly accessible but Block Public Access is enabled at either the bucket level or the account level, any bucket policy that grants public access will be overridden. Both settings must be disabled.

The fourth is KMS encryption. If the objects are encrypted with a customer-managed KMS key, the caller needs both S3 permissions and KMS decrypt permission on the specific key. Missing the KMS permission results in an access denied error even when the S3 permissions are correct.

CloudTrail is your debugging tool for access denied issues. Look at the CloudTrail logs for the failed API call and read the error message, which will specify exactly which permission check failed.

---

### Lambda Function Issues

The three most common Lambda problems on the exam are timeouts, permission errors, and throttling.

Timeouts occur when the function takes longer to complete than its timeout setting allows. The default timeout is three seconds. The maximum is fifteen minutes. If the function connects to a database or makes an API call, those operations add latency. If the function is inside a VPC, the cold start time is longer because Lambda must provision a network interface. Solutions include increasing the timeout setting, optimizing the code, and allocating more memory, which also increases the CPU available to the function.

Permission errors occur when the Lambda execution role does not grant the permissions the function needs. If a function needs to read from S3, write to DynamoDB, or publish to SNS, those permissions must be in the execution role. Check CloudWatch Logs for the specific permission error.

Throttling occurs when too many Lambda functions try to run simultaneously. The default concurrent execution limit is one thousand per account per region. When this limit is reached, additional invocations are rejected. Solutions include requesting a concurrency limit increase, using SQS to buffer and pace invocations, or setting reserved concurrency on critical functions to guarantee they have capacity.

---

### RDS Connection Problems

When an application cannot connect to an RDS database, check these things in order.

First, verify the endpoint hostname and port are correct. RDS uses a DNS endpoint that looks like the database instance name followed by a region-specific suffix. The default port depends on the database engine.

Second, check the RDS security group. It must have an inbound rule allowing traffic on the database port from either the specific IP address of the application server or from the security group attached to the application server. This is the most common cause of RDS connection failures.

Third, verify the application server and the RDS instance are in the same VPC. RDS is not publicly accessible by default, and cross-VPC or cross-account access requires additional configuration like VPC peering or a VPN.

Fourth, confirm the credentials. Database credentials are case-sensitive. If you are using Secrets Manager to store credentials and the password has been rotated, make sure the application is retrieving the latest value rather than using a hardcoded older version.

---

### CloudFormation Stack Failures

When a CloudFormation stack fails and rolls back, the stack events in the CloudFormation console identify which resource failed and why.

The most common causes are insufficient IAM permissions, where the user or role deploying the stack does not have permissions to create one of the resources in the template. Service limit exceeded, where you have reached the default limit for a resource type like VPCs per region or EC2 instances. Resource already exists, where a resource with the same name already exists from a previous failed deployment or manual creation. Parameter validation errors, where a parameter value does not match the allowed values or pattern specified in the template.

Before deploying a CloudFormation stack to a new environment, validate the template to catch syntax errors. Use a change set to preview what a stack update will create, modify, or delete before executing it. Test templates in a development environment before using them in production.

---

### Auto Scaling Not Working

When an Auto Scaling group is not launching instances as expected, there are several things to check.

First, verify the CloudWatch alarm that triggers scaling is actually in the Alarm state. If CPU is high but the alarm says Insufficient Data or OK, the alarm is not triggering. Check that the CloudWatch agent is running on instances and that metrics are being published.

Second, check the capacity limits. If the current number of instances equals the maximum capacity, Auto Scaling cannot add more regardless of how high CPU goes. Verify that the maximum is set appropriately.

Third, check whether the group is in a cooldown period. After a scaling action, there is a default cooldown of five minutes during which no additional scaling actions occur. This prevents rapid flapping.

Fourth, check the launch template or launch configuration for problems. If the AMI has been deleted or the instance type is unavailable in the selected Availability Zones, new instances will fail to launch.

---

### Unexpected High AWS Bill

When an AWS bill is higher than expected, the investigation follows a systematic path.

Open Cost Explorer and group costs by service. Identify which service is driving the unexpected charges and compare to the previous billing period to understand when the spike started.

The most common culprits are EC2 instances left running after testing or development work. EBS volumes that were not deleted when their EC2 instances were terminated. NAT Gateway data processing fees, which accumulate quickly when many instances in private subnets access the internet frequently. Data transfer charges, particularly cross-region data transfer or data transfer out to the internet. CloudWatch Logs charges when applications log verbosely and logs are retained indefinitely.

Prevention is better than investigation. Set up AWS Budgets to send email alerts when spending exceeds a threshold you define. Enable Cost Anomaly Detection, which uses machine learning to identify unusual spending patterns and sends alerts. Tag all resources with meaningful tags like project name, environment, and owner so that costs can be attributed to specific teams and projects.

---

### API Gateway 502 and 504 Errors

A 502 Bad Gateway error from API Gateway means the backend responded with an error or an invalid response. A 504 Gateway Timeout means the backend did not respond within API Gateway's maximum integration timeout of twenty-nine seconds.

For a 502 error when the backend is Lambda, check CloudWatch Logs for the Lambda function. The function is probably throwing an error or returning a response in an incorrect format. Lambda responses to API Gateway must include a status code, headers, and body in a specific structure.

For a 504 timeout, the backend is taking longer than twenty-nine seconds to respond. This is important: even though Lambda functions can run for up to fifteen minutes, when Lambda is invoked synchronously through API Gateway, the response must come back within twenty-nine seconds or API Gateway will return a 504 to the caller even if Lambda eventually completes. For workloads that take longer, use asynchronous invocation patterns, Step Functions, or SQS to decouple the request from the processing.

The first debugging step for any API Gateway error is enabling CloudWatch Logs on the API Gateway stage. Logs capture the incoming request, the integration request sent to the backend, the response received, and any errors that occurred.

---

## Key Patterns to Carry Into the Exam

Let me close with the patterns that tie these twelve scenarios together.

Whenever you see predictable traffic or consistent usage, think Reserved Instances or Savings Plans. Whenever you see auto scaling or variable demand, think On-Demand with scheduled or dynamic scaling.

Whenever you see high availability or survive a failure, think multiple Availability Zones, load balancer, and either RDS Multi-AZ or Aurora.

Whenever you see tight migration deadline and too much data for the internet, think Snow Family.

Whenever you see no servers, pay per use, and mobile or web backend, think Lambda, API Gateway, DynamoDB, and Cognito.

Whenever you see enforce policy across many accounts, think Organizations with Service Control Policies and Config.

Whenever you see disaster recovery with specific RPO and RTO numbers, think Backup and Restore for loose requirements, Pilot Light for moderate, Warm Standby for tight, and Active/Active for near-zero.

Whenever you see private connection to on-premises with consistent bandwidth, think Direct Connect. Whenever you see quick setup or backup connectivity, think VPN.

Whenever you see global users and low latency, think CloudFront for content, Route 53 for routing, and DynamoDB Global Tables or Aurora Global Database for data.

Whenever you see security incident or compliance overhaul, think GuardDuty for threat detection, Macie for sensitive data in S3, Config for configuration compliance, Security Hub for centralized view, and CloudTrail for audit logging.

Whenever you see legacy migration without downtime, think strangler fig pattern, DMS for database migration, and blue/green deployment for traffic shifting.

Whenever you see big data analytics, think Kinesis for streaming, S3 data lake as the foundation, Glue for ETL and cataloging, Athena for ad-hoc queries, Redshift for complex analytics, and QuickSight for visualization.

Whenever you see automated deployments and fast rollback, think CodePipeline, CodeDeploy, and blue/green deployments with CloudWatch alarm-triggered rollback.

These patterns repeat across exam questions. Once you recognize the pattern in the scenario description, the answer almost selects itself.

---
