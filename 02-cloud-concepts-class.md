# Lesson 2: Cloud Concepts

---

Welcome back. This lesson covers the foundational ideas behind cloud computing — not just definitions, but the mental models that will help you think correctly on the exam and beyond. I want you to come away from this lesson understanding not just what cloud computing is, but why it exists, what problem it solves, and how AWS has structured its thinking about well-built systems.

Let us start at the beginning.

---

## Why Cloud Computing Exists

Before cloud computing, every company that wanted to run software had to build and maintain its own infrastructure. You bought servers. You built data centers. You hired people to manage them. If you expected high demand — maybe your product was about to launch, or you were entering a busy season — you had to buy enough hardware to handle that peak load. And then once the peak passed, that hardware sat there doing nothing, fully paid for, slowly becoming outdated.

The fundamental problem with that model is that capacity decisions had to be made years in advance, and getting them wrong was expensive in both directions. Buy too much and you waste money. Buy too little and your application falls over when customers need it most.

Cloud computing solves that problem by pooling resources across millions of customers and letting each customer draw from that pool on demand, paying only for what they actually use. AWS built the infrastructure. You use it. You pay for usage. You stop guessing.

That shift is what the six advantages of cloud computing are describing, and those six advantages appear on the exam regularly. Let me walk through each one so it sticks.

---

## The Six Advantages of Cloud Computing

The first advantage is trading capital expense for variable expense. In the traditional model, buying servers is a capital expenditure — a large upfront payment that shows up on the balance sheet and depreciates over time. In the cloud model, you pay as you go. No upfront commitment. No depreciation. You spend money when you use resources, and you stop spending when you stop. For a startup, this is transformative. For any company, it changes the conversation from "can we afford to build this" to "let us build it and see if it works."

The second advantage is benefiting from massive economies of scale. Because AWS serves millions of customers, it can negotiate far better prices on hardware, power, and data center space than any single company could. Those savings get passed on to you as lower prices. The more customers use AWS, the lower prices go. You benefit from scale that no individual company could achieve on its own.

The third advantage is the ability to stop guessing capacity. You no longer have to predict your infrastructure needs six months in advance. If traffic spikes, you add capacity. If traffic drops, you release it. Services like Auto Scaling do this automatically without any human intervention. You never pay for idle capacity and you never run out.

The fourth advantage is increased speed and agility. In the old model, getting new servers took weeks — purchasing approval, vendor lead time, physical installation, configuration. In the cloud, you provision resources in minutes through an API or a console click. Teams can experiment, iterate, and deploy without waiting for infrastructure to catch up with ideas.

The fifth advantage is the ability to stop spending money running and maintaining data centers. Every dollar a company spends on power, cooling, physical security, and hardware maintenance is a dollar not being spent on the product itself. Cloud computing lets you redirect that money and that mental energy toward things that actually differentiate your business.

The sixth advantage is going global in minutes. AWS has infrastructure in regions around the world. Deploying your application to a new region takes minutes, not months. Your users in Europe, Asia, and South America get fast, local access to your application without you having to build or lease physical data centers in those locations.

The exam will present scenarios and ask you to identify which advantage is being described. When you see a scenario about a startup avoiding large upfront costs, that is trading capital expense for variable expense. When you see a scenario about handling seasonal traffic spikes without overprovisioning, that is stopping guessing capacity. When you see a scenario about deploying to multiple countries quickly, that is going global in minutes. Match the scenario to the right advantage.

---

## The Three Cloud Service Models

Now let us talk about how cloud computing is delivered. There are three service models, and the exam will test whether you can identify which model a given service or scenario represents. The models are Infrastructure as a Service, Platform as a Service, and Software as a Service. The key question with each model is who manages what.

Infrastructure as a Service is the most control you can have without owning physical hardware. AWS provides the virtualized compute, storage, and networking. You are responsible for everything that runs on top of it — the operating system, the runtime, the middleware, the applications, and the data. EC2 is the canonical example. When you launch an EC2 instance, you choose the operating system, you install software, you configure networking, you patch the OS. AWS keeps the physical servers running. You manage everything else. S3 and VPC are also IaaS. This model gives you maximum flexibility and maximum responsibility.

Platform as a Service sits in the middle. AWS manages the underlying infrastructure and the runtime environment. You focus on your application and your data. You do not worry about patching the operating system or managing the server. AWS Elastic Beanstalk is a good PaaS example — you upload your application code, and Elastic Beanstalk handles provisioning EC2 instances, load balancing, auto scaling, and health monitoring. RDS is another — you manage your database and your data, but AWS handles patching, backups, and the underlying server. Lambda is also PaaS in this sense. You write a function, and AWS handles all the infrastructure needed to run it.

Software as a Service is the least control and the least responsibility. AWS manages everything — the infrastructure, the runtime, the application itself. You just use the software. Amazon WorkMail is SaaS — it is fully managed email. Amazon Chime is SaaS — it is a fully managed communications platform. Amazon QuickSight is SaaS — it is a fully managed business intelligence tool. You provide your data and your users. AWS handles the rest.

The memory trick is simple: IaaS gives you the most control. SaaS gives you the least. PaaS is in the middle. When the exam describes maximum control over the operating system, the answer involves IaaS. When the exam describes not needing to worry about any underlying infrastructure, the answer involves SaaS. When the exam describes a managed runtime where you still manage the application, it is PaaS.

Real company examples help here. Netflix uses both IaaS and PaaS — EC2 for compute, RDS for managed databases, and Lambda for serverless functions. A law firm that moves from running its own Exchange email server to Amazon WorkMail is moving from on-premises to SaaS. A company that moves from manually managing web servers to using Elastic Beanstalk is moving from IaaS to PaaS.

---

## The Three Cloud Deployment Models

Related to service models are deployment models. These describe where your infrastructure lives and how it connects to other infrastructure you might have.

The public cloud model means everything runs in AWS. There is no on-premises component. Your entire application lives in AWS infrastructure, managed by AWS, accessible over the internet. This is the simplest model and the most common for new applications being built from scratch.

The hybrid cloud model means you have a combination of AWS infrastructure and on-premises infrastructure, connected together. Your data might stay on-premises for regulatory reasons while your compute runs in AWS. Or your existing on-premises systems communicate with new cloud-based services over a VPN connection or AWS Direct Connect. Hybrid is common in regulated industries — healthcare, finance, government — where certain data cannot leave specific geographic locations or must remain under direct organizational control. The exam is very clear on this: hybrid always means AWS plus on-premises. Hybrid does not mean multiple cloud providers. Multi-cloud is a separate concept that the exam does not emphasize.

The on-premises or private cloud model means your infrastructure stays in your own data center. AWS supports this scenario through AWS Outposts, which is a service that brings AWS hardware and services physically into your data center. You get AWS APIs and services running on equipment that you host, for use cases where data must never leave your facility.

The exam distinguishes these clearly. When a question describes a company that must keep data on-premises for regulatory compliance but wants to use AWS for processing, the answer is hybrid cloud. When a question describes everything running in AWS, it is public cloud. When a question describes AWS-compatible services running in the customer's own data center, it is AWS Outposts.

---

## The AWS Well-Architected Framework

The Well-Architected Framework is AWS's documented approach to building reliable, secure, efficient, and cost-effective systems in the cloud. It consists of six pillars, and the exam tests each of them. Know all six pillars, what each one focuses on, and which services represent each pillar.

The first pillar is Operational Excellence. This pillar is about running and improving systems effectively. The key design principles are performing operations as code, making frequent small reversible changes, refining operations procedures frequently, anticipating failure, and learning from operational events. The services most associated with this pillar are AWS CloudFormation for infrastructure as code, AWS CloudWatch for monitoring, and AWS Systems Manager for operational tasks. Bad architecture under this pillar looks like manual deployments with no automation, big-bang releases that are hard to roll back, and no post-incident review process. Good architecture looks like automated deployments, infrastructure defined as code, and runbooks for every operational process. Exam questions about this pillar often involve a company that wants to automate its deployment process or use infrastructure as code to manage its resources.

The second pillar is Security. This pillar is about protecting information and systems. The key principles are implementing a strong identity foundation, enabling traceability, applying security at all layers, automating security best practices, protecting data in transit and at rest, keeping people away from data, and preparing for security events. The foundational services here are IAM for identity and access management, KMS for encryption key management, GuardDuty for threat detection, Shield for DDoS protection, and CloudTrail for audit logging. The principle of least privilege runs through everything in this pillar — every user and every service should have only the permissions they need to do their job, nothing more. Bad architecture looks like sharing root account credentials, using a single account for everything, and never auditing permissions. Good architecture looks like fine-grained IAM policies, MFA on all privileged accounts, encryption everywhere, and regular access reviews. When the exam describes a scenario that conflicts security with cost or convenience, security always wins.

The third pillar is Reliability. This pillar is about ensuring a system performs its intended function correctly and consistently, including the ability to recover from failures. The key principles are testing recovery procedures, automatically recovering from failure, scaling horizontally to increase aggregate availability, stopping guessing capacity, and managing change through automation. The key services are Multi-AZ deployments for redundancy, Auto Scaling for automatic capacity adjustment, and Elastic Load Balancing for distributing traffic. The foundational concept is that failure is not a question of if but when — reliable systems are designed to handle failure gracefully. Bad architecture puts everything in a single Availability Zone and relies on manual intervention to recover. Good architecture distributes across multiple Availability Zones, automates failover, and regularly tests disaster recovery procedures. The exam frequently tests the difference between Multi-AZ and Multi-Region: Multi-AZ is for high availability and protection against a single data center failure; Multi-Region is for disaster recovery and protection against an entire geographic region becoming unavailable.

The fourth pillar is Performance Efficiency. This pillar is about using computing resources efficiently to meet system requirements and maintaining efficiency as demand changes and technology evolves. The principles are democratizing advanced technologies, going global in minutes, using serverless architectures, experimenting more often, and using tools the way they are designed to be used. Key services are CloudFront for content delivery, Lambda for serverless compute, and instance type selection for right-sizing. Bad architecture uses the same large instance type for everything regardless of workload, runs unused resources around the clock, and never evaluates whether newer and better options have become available. Good architecture matches resource types to workload characteristics, uses caching to reduce load, and continuously evaluates new services and instance types.

The fifth pillar is Cost Optimization. This pillar is about running systems to deliver business value at the lowest possible price point. The principles are adopting a consumption model, measuring overall efficiency, stopping spending money on undifferentiated heavy lifting, analyzing and attributing expenditure, and using managed services to reduce cost of ownership. Key services are AWS Cost Explorer for visualizing spending, AWS Budgets for cost alerts, and Reserved Instances for predictable workloads. The critical practices are tagging resources so costs can be attributed to specific teams or projects, right-sizing instances rather than overprovisioning, and using Spot Instances for fault-tolerant workloads. Bad architecture involves instances running at five percent utilization, no cost visibility by team or project, and paying on-demand pricing for workloads that run constantly. Good architecture eliminates idle resources, uses purchase commitments where appropriate, and reviews spending regularly.

The sixth pillar is Sustainability. This is the newest pillar, added in 2021. It focuses on minimizing the environmental impact of running cloud workloads. The principles are understanding your impact, establishing sustainability goals, maximizing utilization, anticipating and adopting more efficient hardware and software offerings, using managed services, and reducing the downstream impact of your cloud workloads. AWS Graviton processors are relevant here — they provide better performance per watt than comparable x86 processors. Auto Scaling improves sustainability by eliminating idle capacity. When the exam asks which pillar focuses on environmental impact or carbon footprint, the answer is Sustainability.

---

## Cloud Economics: CapEx, OpEx, and Total Cost of Ownership

Understanding cloud economics means understanding two accounting concepts: capital expenditure and operational expenditure.

Capital expenditure is money spent on acquiring or improving fixed assets — things like servers, storage arrays, and data center equipment. CapEx requires large upfront payments. The assets depreciate over time. The decision to spend must be made well before the capacity is needed. Getting it wrong in either direction is expensive.

Operational expenditure is money spent on day-to-day operations — ongoing costs that recur based on usage. Cloud computing converts infrastructure cost from CapEx to OpEx. Instead of buying a server for forty thousand dollars that will depreciate over five years, you pay a few hundred dollars per month for the compute you actually use, and you adjust that payment up or down as your needs change.

For accounting and tax purposes, this distinction matters: CapEx spending is capitalized and depreciated over multiple years, while OpEx spending is typically deducted in the year it occurs. The cloud model often improves cash flow and simplifies financial planning because costs are predictable, pay-as-you-go, and align with actual usage.

Total Cost of Ownership is a framework for comparing the true cost of on-premises infrastructure versus cloud infrastructure. Many companies underestimate the cost of on-premises because they only count the hardware. The full picture includes power, cooling, physical space, network connectivity, hardware refresh cycles, security hardware, and the people required to manage all of it. When you add all of that up and compare it to cloud costs, the cloud is often less expensive even before you account for the flexibility and reduced operational burden. AWS provides a TCO Calculator to help with this comparison.

---

## The Six Migration Strategies

When companies move workloads from on-premises to AWS, they choose from six migration strategies. These are often called the six R's. The exam will present migration scenarios and ask you to identify the right strategy. Match the scenario to the strategy that fits.

The first strategy is Rehosting, also called lift and shift. You move the application to AWS without making any changes to the application itself. The same code, the same architecture — just running on AWS infrastructure instead of your data center. This is the fastest migration approach. The benefits are modest — you gain cloud infrastructure and pay-as-you-go pricing, but the application has not been redesigned to take advantage of cloud-native features. Rehosting is the right choice when speed is the priority — for example, when a data center is closing and applications need to move quickly. A company needing to migrate two hundred applications in three months would rehost most of them.

The second strategy is Replatforming, also called lift, tinker, and shift. You make some modest optimizations during migration to take advantage of cloud capabilities, without changing the core architecture. For example, migrating from a self-managed database on EC2 to Amazon RDS so that AWS handles backups and patching, while keeping the same application code. The work involved is low to moderate, the benefits are meaningful, and the risk is manageable. Replatforming is the right choice when you want some cloud benefits without a full redesign.

The third strategy is Repurchasing, also called drop and shop. Instead of migrating your existing application, you move to a different product — typically a SaaS solution. A company running its own email server might repurchase by switching to Amazon WorkMail or Microsoft 365. A company running a licensed CRM system might move to Salesforce. The old application is abandoned. A new SaaS product replaces it. Repurchasing makes sense when a better SaaS alternative exists, the licensing cost of the old system is high, or the company does not want to maintain the application at all.

The fourth strategy is Refactoring, also called re-architecting. You redesign the application to be cloud-native, taking full advantage of cloud capabilities that were not possible on-premises. A monolithic application might be broken into microservices. A batch process might be redesigned as an event-driven serverless workflow. This approach delivers the most cloud benefit — scalability, resilience, cost optimization — but it takes the most time and effort. Refactoring is the right choice when the existing architecture is not meeting business needs and a fundamental redesign is justified.

The fifth strategy is Retiring. Some applications, when you examine them honestly, are not worth migrating at all. They may be redundant, unused, or superseded by other applications. Retiring means shutting them down and removing them from the migration list. This frees up budget and attention for the applications that matter.

The sixth strategy is Retaining. Some applications are not ready to migrate — perhaps because they were recently upgraded on-premises, because dependencies make migration complex, or because regulatory requirements prevent it. Retaining means deliberately keeping those applications on-premises for now, with a plan to revisit the decision in the future.

The exam pattern for migration questions works like this: when the scenario says quickly or limited time, rehosting. When the scenario says cloud-native or modernize, refactoring. When the scenario says replace with a SaaS product, repurchasing. When the scenario says modest optimization without redesign, replatforming. When the scenario says the application is no longer needed, retiring. When the scenario says regulatory requirements prevent migration right now, retaining.

---

## Review Questions

Let me work through the review questions with you now. I will read each question, pause, and then explain the reasoning behind the correct answer. Use these to test whether the concepts have clicked.

Question one: Which of the following are advantages of cloud computing? Choose two. Option A, trade variable expense for capital expense. Option B, benefit from massive economies of scale. Option C, stop guessing capacity. Option D, increase spending on data center maintenance. Option E, maintain physical security of data centers.

The answers are B and C. Option A has it backwards — cloud trades capital expense for variable expense, not the other way around. Options D and E describe things that the cloud eliminates, not advantages of it. Economies of scale and eliminating capacity guessing are genuine advantages.

Question two: What type of cloud deployment connects on-premises infrastructure with cloud resources? Option A, public cloud. Option B, hybrid cloud. Option C, private cloud. Option D, multi-cloud.

The answer is B, hybrid cloud. When you see on-premises connected to AWS, that is hybrid. Public cloud is entirely in AWS. Private cloud stays in your own data center. Multi-cloud means using multiple cloud providers.

Question three: Which pillar of the AWS Well-Architected Framework focuses on protecting information and systems? Option A, Operational Excellence. Option B, Security. Option C, Reliability. Option D, Performance Efficiency.

The answer is B, Security. Each pillar has a clear primary focus: Security is about protecting data and systems. Do not let Reliability or Operational Excellence confuse you — reliability is about recovering from failures, and operational excellence is about running systems efficiently.

Question four: Which migration strategy involves moving an application to the cloud without making changes? Option A, Replatforming. Option B, Refactoring. Option C, Rehosting. Option D, Repurchasing.

The answer is C, Rehosting. Lift and shift means no changes. Replatforming makes modest changes. Refactoring redesigns the application. Repurchasing switches to a different product entirely.

Question five: A company wants to track all API calls made in their AWS account for compliance. Which service should they use? Option A, Amazon CloudWatch. Option B, AWS CloudTrail. Option C, AWS Config. Option D, AWS X-Ray.

The answer is B, CloudTrail. This is a critical distinction to memorize: CloudTrail records who made what API call and when — the audit trail for compliance. CloudWatch monitors performance metrics and logs. Config tracks configuration changes over time. X-Ray traces application requests for debugging.

Question six: Which cloud computing model provides the most control over the underlying infrastructure? Option A, Software as a Service. Option B, Platform as a Service. Option C, Infrastructure as a Service. Option D, Function as a Service.

The answer is C, Infrastructure as a Service. IaaS gives you control over the operating system and everything that runs on it. SaaS provides the least control. This is one of those facts you must have immediately available.

Question seven: A startup wants to minimize upfront costs and pay only for what they use. Which advantage of cloud computing does this represent? Option A, go global in minutes. Option B, increase speed and agility. Option C, trade capital expense for variable expense. Option D, benefit from massive economies of scale.

The answer is C. Pay only for what you use with no upfront costs is the definition of trading capital expense for variable expense. Speed and agility is about deployment speed, not payment structure. Economies of scale is about lower prices overall, not specifically the pay-as-you-go model.

Question eight: Which migration strategy would be most appropriate for a company that needs to migrate two hundred applications to AWS within three months to meet a data center closure deadline? Option A, Refactoring. Option B, Rehosting. Option C, Repurchasing. Option D, Retire.

The answer is B, Rehosting. When time is the constraint, lift and shift is the answer. Refactoring takes months or years per application. Repurchasing only applies to specific applications with SaaS alternatives. Retire applies only to applications that are no longer needed.

Question nine: Which of the following are design principles of the Reliability pillar? Choose two. Option A, deploy across multiple Availability Zones. Option B, use the cheapest instance type. Option C, test recovery procedures. Option D, manually scale capacity. Option E, disable monitoring to reduce costs.

The answers are A and C. Multi-AZ deployment and testing recovery procedures are both core Reliability principles. Using the cheapest instance type is a Cost Optimization consideration. Manual scaling is the opposite of the Reliability principle of automatic recovery. Disabling monitoring is never correct for reliability.

Question ten: A company wants to optimize costs by automatically moving infrequently accessed data to cheaper storage tiers. Which service should they use? Option A, Amazon S3 Lifecycle Policies. Option B, AWS Budgets. Option C, AWS Cost Explorer. Option D, Amazon CloudWatch.

The answer is A, S3 Lifecycle Policies. This service automates the movement of data between S3 storage classes based on age or access patterns. Budgets sends alerts when spending thresholds are crossed. Cost Explorer visualizes spending history. CloudWatch is for metrics and monitoring — none of those three touch storage tiering.

Question eleven: Which Well-Architected Framework pillar focuses on minimizing environmental impact? Option A, Cost Optimization. Option B, Performance Efficiency. Option C, Sustainability. Option D, Operational Excellence.

The answer is C, Sustainability. This is the newest pillar, added in 2021. Cost Optimization is about spending less money, not reducing environmental impact. Performance Efficiency is about using resources efficiently, which overlaps with sustainability but is not the same thing. When you see the words environmental impact or carbon footprint, go to Sustainability.

Question twelve: A financial services company must keep certain data on-premises due to regulatory requirements but wants to use AWS for compute. What deployment model should they use? Option A, public cloud. Option B, hybrid cloud. Option C, private cloud. Option D, community cloud.

The answer is B, hybrid cloud. Data stays on-premises, compute runs in AWS — that is the definition of hybrid. This is a very common exam scenario for regulated industries. Remember: hybrid means AWS plus on-premises, not multiple cloud providers.

Question thirteen: Which service provides recommendations for cost optimization, security, and performance? Option A, AWS Config. Option B, AWS CloudTrail. Option C, AWS Trusted Advisor. Option D, Amazon Inspector.

The answer is C, Trusted Advisor. This service examines your account and makes recommendations across five categories: cost optimization, performance, security, fault tolerance, and service limits. Config tracks configuration changes. CloudTrail is the API audit log. Inspector specifically assesses EC2 instances and container images for security vulnerabilities — it does not give broad optimization advice.

Question fourteen: A company wants to replace their on-premises email server with a fully managed email service. Which migration strategy are they using? Option A, Rehosting. Option B, Replatforming. Option C, Refactoring. Option D, Repurchasing.

The answer is D, Repurchasing. They are abandoning the old product — the on-premises email server — and replacing it with a different product, a managed email service. That is the definition of repurchasing. Rehosting would mean moving the existing email server to AWS without changing it. Replatforming would mean keeping their email software but running it on managed AWS infrastructure. Refactoring would mean rewriting the email functionality from scratch as a cloud-native application, which makes no sense when a ready-made managed service exists.

---

## Key Things to Carry Forward

Let me close this lesson with the patterns that appear most often on the exam.

The six advantages appear in three to five questions. You need them memorized: trade CapEx for variable expense; benefit from massive economies of scale; stop guessing capacity; increase speed and agility; stop spending money on data centers; go global in minutes.

The six Well-Architected pillars are heavily tested. Operational Excellence is about running and improving operations through automation. Security is about protecting data and systems with least privilege and encryption everywhere. Reliability is about recovering from failures through redundancy and automated recovery. Performance Efficiency is about right-sizing resources and using caching. Cost Optimization is about eliminating waste and using the right pricing model. Sustainability is about minimizing environmental impact.

The service models come down to control. IaaS gives you the most. SaaS gives you the least. PaaS is in between.

The deployment models come down to where things live. Public cloud is entirely in AWS. Hybrid is AWS plus on-premises. On-premises with AWS capabilities is Outposts.

The six R's come down to time and transformation. Rehosting is fastest, with the least cloud benefit. Refactoring takes longest, with the most cloud benefit. Repurchasing replaces the application with a SaaS product. Replatforming makes modest improvements without a full redesign. Retiring eliminates unnecessary applications. Retaining keeps applications that are not ready to move.

CapEx versus OpEx: on-premises is CapEx, upfront and depreciating. Cloud is OpEx, pay-as-you-go and flexible.

And the CloudTrail versus CloudWatch distinction comes up constantly across many questions: CloudTrail is who did what — the API audit log for compliance. CloudWatch is how things are performing — metrics, logs, and alarms for operations. Know those two apart cold.

That is cloud concepts. The ideas in this lesson are the foundation that every other lesson builds on. When you see a domain three question about a specific service, ask yourself which cloud concept it represents. Which advantage does it deliver? Which pillar does it reinforce? Which migration strategy does it support? Those frameworks will help you reason through questions you have never seen before.

---
