# Lesson 7: Hands-On Labs — Introduction and Concepts

---

Welcome. Today's session is a little different from the previous lessons. We are not covering theory or exam topics directly. What we are doing today is preparing you to get your hands into the AWS Console and actually use the services you have been reading about.

Let me be honest with you about something first. Reading about AWS services and using them are two very different experiences. When you read that S3 is object storage, you understand it as a concept. When you create a bucket, upload a file, configure permissions, and see it appear in a browser, you understand it as something real. The exam tests comprehension, and comprehension comes from experience. That is why these labs exist.

What I am going to do today is walk you through what each lab is designed to teach you, why it matters for your exam, and what you should be able to say confidently after you complete it. The actual step-by-step instructions are in the companion lab guide. When you sit down at a computer with your AWS Free Tier account open, follow the lab guide. Come back to this lesson when you want to understand the purpose behind what you are doing.

---

## Before You Begin Any Lab

There are a few things that apply to every single lab, and I want you to hear them now so you do not have to be reminded repeatedly.

The first is account access. Every lab requires an active AWS account. Use the AWS Free Tier account you set up during your earlier studies. Do not use a work or company account unless you have explicit permission. The labs are designed for personal learning accounts.

The second is regions. AWS organizes resources by region. Before you start any lab, check that the region selector in the top right of the Console shows the region you intend to use. Resources created in one region do not appear when you switch to another. Many students spend ten minutes confused about missing resources that are simply in the wrong region. Make this a habit: check your region every time you open a new service.

The third is cost awareness. AWS offers a Free Tier that covers most of what these labs require, but the Free Tier has limits. Some things are not covered. There are labs in this set that involve services like NAT Gateways and Application Load Balancers that do cost real money if left running. Every lab includes cleanup instructions. Take cleanup seriously. After you complete a lab, follow the cleanup steps before you close your browser. A terminated EC2 instance cannot charge you. A running one can.

The fourth is the billing alarm. Before you do anything else in AWS, complete Lab One. A billing alarm is your financial safety net. It alerts you by email if your charges exceed a threshold you set. Set it to one dollar if you want to be cautious. Setting this up first means you will know immediately if something goes wrong with your spending.

---

## Lab One: Billing Alerts and Budget

This is the first lab you should complete, even before touching any other AWS service.

The purpose of Lab One is to teach you how AWS tracks and reports costs, and how to set up alerts so you never face a surprise bill. You will explore the Billing Dashboard, which gives you a real-time view of your account spending. You will set up a CloudWatch billing alarm, which sends you an email when your charges cross a specific dollar amount. And you will create an AWS Budget, which is a more sophisticated tool that can track spending against a monthly target and alert you at multiple thresholds.

Why does this matter for the exam? The exam tests your knowledge of cost management tools, specifically the difference between the AWS Pricing Calculator, Cost Explorer, Budgets, and billing alarms. After completing this lab, you will have personally used the Billing Dashboard and Budgets, which makes those questions concrete rather than abstract.

There is one important technical detail about billing alarms that the exam likes to test. Billing alarm metrics are only available in the US East North Virginia region. It does not matter where the rest of your resources live. Billing alarms must be created in US East North Virginia. The lab will guide you there, but I want you to understand why, not just where to click.

After this lab, you should be able to describe the difference between a billing alarm and a budget. You should know that a billing alarm is a simple threshold trigger, while a budget is a more flexible tool that can send alerts at fifty percent, eighty percent, and one hundred percent of a target and can even apply to forecasted spending, not just actual spending. Those distinctions are testable.

---

## Lab Two: IAM Users, Groups, Roles, and MFA

Lab Two is one of the most important labs in this entire set, because IAM is one of the most important services on the exam.

The goal of this lab is to give you direct experience with the Shared Responsibility Model in practice. AWS manages the infrastructure. You manage access to what runs on top of it. IAM is the mechanism through which you fulfill your side of that responsibility.

In this lab you will create an IAM user and experience what it feels like to log in as that user versus logging in as the root account. You will create a group, attach a policy to the group, and add your user to the group so they inherit permissions through group membership. You will then test that the user can do what the policy allows and cannot do what the policy does not allow. You will also enable multi-factor authentication, which adds a second form of verification beyond a password.

Here is what you need to understand conceptually before you start. The root account is the account created when you first signed up for AWS. It has unlimited permissions and cannot be restricted by policies. Best practice is to use the root account only for account-level tasks like billing setup and account closure, then create an IAM user for daily work. Never share root credentials. Never create access keys for root.

IAM users are individual identities with specific permissions. Groups are collections of users that make permission management scalable. Policies are the documents that define what is allowed or denied. Roles are different from users in a critical way: roles are assumed temporarily by services or people who need elevated permissions for a task. An EC2 instance that needs to read from S3 uses an IAM role, not a user account.

After this lab, you should be able to explain the difference between a user, a group, a role, and a policy without hesitation. You should understand why permissions should follow the principle of least privilege, meaning you grant only what is needed and nothing more. And you should understand why MFA is a best practice for all accounts, especially root.

---

## Lab Three: EC2 Instance Launch and Configuration

EC2 is the most tested service in Domain Three. Lab Three gives you direct experience with the service that underlies most AWS compute.

In this lab you will launch a virtual server, choose an instance type, configure its security, access it remotely, and run a simple web server on it. You will make real decisions about the same settings that appear on the exam: the Amazon Machine Image, which is the operating system template; the instance type, which determines CPU and memory; the key pair, which is how you authenticate; the security group, which controls inbound and outbound traffic; and the network settings, which determine whether the instance is reachable from the internet.

The most important concept to internalize from this lab is how security groups work. Security groups are virtual firewalls that control traffic at the instance level. They are stateful, meaning if you allow inbound traffic on a port, the response traffic is automatically allowed without needing a separate outbound rule. By default, a security group denies all inbound traffic and allows all outbound traffic. When you set up a web server in this lab, you will add a rule to allow HTTP traffic on port eighty from anywhere. That single rule is what makes your web server reachable from a browser.

You will also encounter the difference between stopping and terminating an EC2 instance. Stopping is like powering down a computer. The instance is not running, so you are not billed for compute time, but the underlying storage and configuration are preserved and the instance can be started again. Terminating is permanent. The instance and its attached storage are deleted and cannot be recovered. The exam tests this distinction, particularly in the context of cost optimization. Stopping a dev instance overnight saves money. Terminating it when a project is complete saves even more.

After this lab, you should be able to describe the EC2 launch process, explain what each major configuration setting does, and articulate the difference between stopping and terminating an instance.

---

## Lab Four: S3 Storage and Static Website Hosting

S3 is the most versatile storage service in AWS and appears constantly on the exam. Lab Four gives you experience with bucket creation, object storage, permissions, and one of S3's most useful features: static website hosting.

In this lab you will create an S3 bucket, upload files, configure access permissions, enable static website hosting, and access a simple web page through S3's website endpoint. You will also explore versioning, which protects you against accidental overwrites and deletes by preserving every version of every object.

The most important concept to understand from this lab is how S3 access control works. By default, S3 buckets and objects are private. Nothing is publicly accessible. To make a static website work, you need to do two things: turn off the block public access setting that protects the bucket, and then attach a bucket policy that explicitly grants read permission to anyone on the internet. Both steps are required. The exam likes to test scenarios where a student enables website hosting but forgets one of these steps and wonders why they get an access denied error.

The other thing to pay attention to in this lab is the distinction between S3's two different URL formats. The regular S3 URL gives you direct access to individual objects using the REST API. The website endpoint URL is different and is what you use when you want S3 to serve a website with index documents and custom error pages. These are not interchangeable. The exam sometimes tests whether you understand this difference.

After this lab, you should be able to explain how to configure a bucket for public access, describe what a bucket policy does, and explain the difference between S3 as object storage and S3 as a static website host.

---

## Lab Five: VPC, Subnets, and Network Configuration

Lab Five is the most complex lab in this set. It covers Virtual Private Cloud, which is the networking foundation for everything you run in AWS. This lab is rated advanced because networking concepts build on each other and require careful attention.

In this lab you will build a custom VPC from scratch. You will create subnets, configure route tables, attach an internet gateway, and set up network access control lists. You will understand the difference between a public subnet, which can communicate with the internet, and a private subnet, which cannot.

Here is the architecture you need to understand before you start. A VPC is your private network within AWS. It spans all Availability Zones in a region. Inside the VPC you create subnets, which are subdivisions of the VPC's IP address range. Each subnet is associated with exactly one Availability Zone. A subnet becomes public when its route table has a route that sends internet-bound traffic to an internet gateway. A subnet without that route is private.

The internet gateway is what connects your VPC to the public internet. Without one, nothing inside your VPC can communicate with the outside world. The route table is the navigation system that tells traffic where to go. A route table entry that says send all non-local traffic to the internet gateway is what defines a public subnet.

Security groups and network access control lists are both firewall mechanisms, but they operate differently. Security groups apply at the individual resource level and are stateful. Network ACLs apply at the subnet level and are stateless, meaning you must create both inbound and outbound rules for the same traffic. The exam tests this distinction regularly. Remember: security groups are stateful and operate at the instance level. Network ACLs are stateless and operate at the subnet level.

After this lab, you should be able to explain the relationship between a VPC, subnets, route tables, and internet gateways. You should be able to describe what makes a subnet public versus private, and you should understand the difference between security groups and network ACLs.

---

## Lab Six: RDS Managed Database

Lab Six introduces you to Amazon RDS, the managed relational database service. This lab matters because the exam frequently asks you to choose between RDS and DynamoDB for a given scenario, and making that choice correctly requires understanding what each service is designed for.

In this lab you will create an RDS database instance, configure its settings, and connect to it. You will see firsthand that RDS sits inside your VPC and is accessed through a DNS endpoint, not a fixed IP address. You will configure the security group rules that allow a specific resource to connect to the database on the standard database port.

The key insight from this lab is understanding what managed means in a managed database service. When you run a database on your own EC2 instance, you are responsible for everything: installing the database software, patching it, setting up backups, handling failover, and monitoring performance. When you use RDS, AWS handles all of that. You choose the database engine, the instance size, and the storage, and AWS handles the underlying infrastructure, automated backups, software patching, and optionally a standby replica for failover.

Multi-AZ deployment is a feature you will configure in this lab. It automatically creates a standby replica of your database in a different Availability Zone. If the primary database fails, RDS automatically promotes the standby. The failover is transparent to your application because the DNS endpoint stays the same. Multi-AZ is about availability, not performance. It does not serve read traffic.

Read Replicas are different. They serve actual read traffic, allowing you to scale read performance across multiple copies of your database. The exam distinguishes between Multi-AZ for high availability and Read Replicas for read scaling. Know that distinction.

After this lab, you should be able to explain why RDS is preferred over a self-managed database on EC2, describe the difference between Multi-AZ and Read Replicas, and explain how RDS fits within a VPC.

---

## Lab Seven: CloudWatch Monitoring

Lab Seven is about visibility. One of the principles from the Well-Architected Framework is operational excellence, and monitoring is central to that. CloudWatch is your primary monitoring service in AWS.

In this lab you will set up CloudWatch to monitor an EC2 instance. You will view metrics like CPU utilization, explore how to create alarms that trigger when a metric crosses a threshold, and configure notifications that alert you when something needs attention. You will also look at CloudWatch Logs, which is where application and service logs are collected and stored.

Here is the conceptual distinction you need to carry into the exam. CloudWatch monitors performance. CloudTrail records API calls. AWS Config tracks configuration changes. Students frequently confuse these three services, and the exam counts on it. Let me give you a memory anchor. CloudWatch watches over your running services and tells you how they are performing. CloudTrail is the audit log that tells you who did what and when, recording every API call made in your account. AWS Config is the configuration historian that tells you how your resources were configured at any point in time and whether they comply with your rules.

If a question asks which service would alert you when CPU usage on an EC2 instance spikes above eighty percent, the answer is CloudWatch. If a question asks which service would tell you who deleted an S3 bucket at two in the morning, the answer is CloudTrail. If a question asks which service tracks whether your EC2 instances have specific tags applied or whether encryption is enabled, the answer is AWS Config.

After this lab, you should be able to describe what a CloudWatch metric is, explain how an alarm works, and confidently distinguish CloudWatch from CloudTrail and AWS Config.

---

## Lab Eight: Cost Management Tools

Lab Eight builds directly on the billing concepts from Domain Four of the exam. This lab gives you practical experience with the five main cost management tools AWS provides.

The tools you will explore are the AWS Pricing Calculator, Cost Explorer, AWS Budgets, Cost and Usage Report, and Cost Anomaly Detection. Each one has a distinct job, and the exam asks you to match a scenario to the right tool.

The Pricing Calculator is for estimates. Use it before you build anything to estimate what a proposed architecture will cost. It is entirely hypothetical and does not require an AWS account. Cost Explorer is for analysis. It shows you your actual historical spending, broken down by service, region, account, or tag, and it can project future spending based on trends. Budgets is for proactive alerts. You set a target spending amount and tell Budgets to alert you when you hit fifty percent, eighty percent, or one hundred percent of that target. The Cost and Usage Report is the most detailed billing document AWS produces, with line-item data that can be analyzed with tools like Athena or loaded into a data warehouse. Cost Anomaly Detection uses machine learning to identify spending patterns that look unusual and alerts you when something does not match your normal behavior.

The exam pattern you will encounter most often is a scenario that says something like a company wants to know how much a new AWS deployment will cost before they build it. That is the Pricing Calculator. A different question might say a company noticed an unexpected spike in their bill and wants to understand which service is responsible. That is Cost Explorer or Cost Anomaly Detection depending on whether it is after the fact or in real time.

After this lab, you should be able to describe the primary purpose of each cost management tool and match the right tool to a given scenario.

---

## Lab Nine: Lambda Serverless Functions

Lab Nine introduces you to Lambda, the serverless compute service that represents a fundamentally different model from EC2.

In this lab you will create a Lambda function, configure it to respond to a trigger, and test it. You will see how Lambda executes code without you managing any server infrastructure. You will also experience how Lambda integrates with other services, because Lambda rarely operates alone. It is typically triggered by events from other services and interacts with other services to do its work.

The concept to internalize from this lab is what serverless actually means. Serverless does not mean there are no servers. It means you do not see or manage servers. AWS runs the function on whatever infrastructure is needed, scales it automatically based on the number of invocations, and charges you only for the time your code is actually executing. There is no idle cost. A Lambda function that is never triggered costs nothing. A Lambda function that runs for one second costs a fraction of a cent.

Lambda has some important constraints that the exam tests. Functions can run for a maximum of fifteen minutes. If a task takes longer than fifteen minutes, Lambda is not the right tool. Lambda is ideal for short-duration event-driven processing: resizing an image when it is uploaded to S3, processing a message from an SQS queue, responding to an API request through API Gateway. It is not ideal for long-running batch jobs or always-on workloads.

You will also encounter IAM roles in this lab. Lambda functions need permissions to access other AWS services. Those permissions are defined in an execution role that you attach to the function. The function assumes that role when it runs and gets whatever permissions the role grants.

After this lab, you should be able to explain what serverless means, describe the Lambda execution model including triggers and execution roles, and list the scenarios where Lambda is a good fit versus where it is not.

---

## Lab Ten: CloudFormation Infrastructure as Code

Lab Ten introduces Infrastructure as Code, which is the practice of defining your AWS resources in a template file rather than clicking through the Console. This is how professional teams manage cloud infrastructure at scale.

In this lab you will create a CloudFormation template and use it to provision real AWS resources. You will see how a single file can describe a complete infrastructure setup and how CloudFormation reads that file and creates all the resources in the right order, respecting the dependencies between them.

The exam concept to focus on here is the difference between CloudFormation and Elastic Beanstalk. Both automate the deployment of resources, but they operate at different levels of abstraction. CloudFormation is lower-level and more flexible. You define exactly what resources you want and how they should be configured. There is no assumption about what kind of application you are running. Elastic Beanstalk is higher-level and more opinionated. You give it your application code, tell it what language or runtime it uses, and it handles the underlying infrastructure on your behalf. Elastic Beanstalk uses CloudFormation internally, but you do not need to write templates yourself.

CloudFormation also introduces the concept of a stack. A stack is a collection of resources that CloudFormation manages as a single unit. When you delete a stack, CloudFormation automatically deletes all the resources that belong to it. This is why CloudFormation is so valuable for learning environments: you can build a complete architecture, use it, and then tear it all down cleanly with one action rather than hunting through multiple service consoles to delete individual resources.

After this lab, you should be able to explain what Infrastructure as Code means and why it matters, describe the difference between a template and a stack, and articulate the difference between CloudFormation and Elastic Beanstalk.

---

## Lab Eleven: Auto Scaling and Load Balancing

Lab Eleven is the architecture lab. It brings together multiple services to build a pattern that represents the foundation of scalable, highly available AWS applications.

In this lab you will set up an Application Load Balancer that distributes traffic across multiple EC2 instances, and an Auto Scaling Group that automatically adjusts the number of running instances based on demand. Together, these two services form the backbone of a resilient, cost-efficient application architecture.

The load balancer solves two problems. First, it distributes incoming traffic across healthy instances so that no single server is overwhelmed. Second, it performs health checks and automatically stops sending traffic to instances that fail those checks. From the perspective of the internet, there is one endpoint. Behind the scenes, the load balancer is managing potentially dozens of instances.

Auto Scaling solves the problem of matching capacity to demand. You define a minimum number of instances that must always be running, a maximum number that can ever run, and scaling policies that define when to add or remove instances. When traffic increases, Auto Scaling launches new instances. When traffic decreases, it terminates instances that are no longer needed. This is how you build a system that handles a traffic spike at noon without paying for those extra instances at three in the morning.

The exam will test your understanding of when to use an Application Load Balancer versus a Network Load Balancer. The Application Load Balancer operates at the application layer and understands HTTP and HTTPS traffic. It can make routing decisions based on the content of the request, like sending requests for the API to one set of instances and requests for the website to another. The Network Load Balancer operates at the transport layer and is optimized for extreme performance and low latency with TCP and UDP traffic. For most web application scenarios, Application Load Balancer is the answer.

After this lab, you should be able to explain how load balancers and Auto Scaling Groups work together, describe the difference between the Application Load Balancer and the Network Load Balancer, and explain why distributing instances across multiple Availability Zones is a high availability best practice.

---

## Lab Twelve: DynamoDB

Lab Twelve completes your hands-on coverage of AWS database services. Where Lab Six gave you experience with RDS, the relational database service, Lab Twelve gives you experience with DynamoDB, the NoSQL database service.

In this lab you will create a DynamoDB table, add items to it, query it, and explore the indexing capabilities that make DynamoDB flexible despite its schema-less nature. You will also configure features like Point-in-Time Recovery and Time to Live, which are commonly tested on the exam.

The core concept to understand is what makes DynamoDB different from RDS. RDS is for structured, relational data with predefined schemas, complex queries with joins, and transactions that span multiple tables. DynamoDB is for high-scale, low-latency key-value and document data where the access patterns are predictable and performance must remain consistent even as the table grows to billions of items.

DynamoDB's primary key concept is critical. Every table requires a partition key, which is the attribute DynamoDB uses to distribute data across its infrastructure. The partition key determines which physical partition stores an item. Tables can also have a sort key, which allows multiple items to share the same partition key as long as their sort keys are different. Together, the partition key and sort key form a composite primary key.

The exam also tests the distinction between Query and Scan operations. A Query is efficient because it targets a specific partition key and retrieves only the items in that partition. A Scan reads every item in the entire table and then filters. For any table with significant data, scans are slow and expensive. Always design your access patterns to use queries.

Global Secondary Indexes allow you to query by attributes other than the primary key. The exam may describe a scenario where you need to look up a user by their email address but the table uses a user ID as the partition key. A Global Secondary Index on the email attribute solves that problem.

After this lab, you should be able to describe the difference between RDS and DynamoDB, explain the purpose of the partition key and sort key, describe the difference between Query and Scan, and know when you would choose DynamoDB over RDS.

---

## Common Troubleshooting Principles

Before you start working through these labs, I want to give you a few principles that apply when things go wrong, because things will go wrong.

The first thing to check when resources are missing is your region. Always verify that you are looking in the same region where you created the resource. Switch to the wrong region and everything you built appears to have vanished.

The second principle is permissions. If you receive an error that says you are not authorized to perform an action, the issue is almost always an IAM policy. Either the user or role does not have the required permission, or there is an explicit deny statement overriding an allow. Check the policy attached to the user or role, and wait five to ten minutes after making policy changes because policy updates take a short time to propagate.

The third principle is cost awareness. If you see unexpected charges, check the Free Tier dashboard to see which limits you have approached or exceeded. The Free Tier is per account, not per region, so if you are experimenting in multiple regions, your hours of EC2 usage add up across all of them. Check for resources running in regions you forgot about, particularly NAT Gateways and load balancers, which are not covered by the Free Tier.

The fourth principle is cleanup. The most common cause of unexpected charges in lab environments is forgetting to delete resources after you finish. After every lab session, before you close your browser, stop and terminate your EC2 instances, delete your RDS databases, empty and delete your S3 buckets, and delete your CloudFormation stacks. Make this a ritual.

---

## What to Do After Each Lab

After you complete each lab and clean up your resources, I want you to do one more thing. Close the lab guide and try to explain out loud, to yourself, what you just built, why it matters, and what would happen if you had made a different choice at a key step. This is called the teach-it-back method, and it is one of the most effective ways to convert procedural memory into conceptual understanding.

For example, after Lab Three, you should be able to say something like: I launched an EC2 instance with an Amazon Linux AMI on a t2.micro, which is Free Tier eligible. I created a security group that allowed SSH on port twenty-two from my IP and HTTP on port eighty from anywhere. I accessed the instance using a key pair and installed a web server. If I had forgotten to add the HTTP rule to the security group, the web server would have been running but unreachable from my browser.

If you can narrate what you did and explain the reasoning behind each choice, you are ready for the exam questions that test those concepts. If you find yourself saying I just followed the steps without really understanding why, go back and read the relevant section from the theory lessons before moving on.

---

## Final Note

The time you invest in these labs pays dividends on the exam. AWS questions are designed to reward students who have interacted with the real services, because the right answers often feel wrong if you have only read about them. When you have actually seen a security group control access to your web server, questions about security groups stop being abstract and start being familiar.

Work through the labs in order if you can. Each one builds context for the next. If you are short on time, prioritize Lab One for billing safety, Lab Two for IAM, Lab Three for EC2, Lab Four for S3, and Lab Six for RDS. Those five services together represent a significant portion of the exam.

Good luck. The console is waiting for you.

---
