# Lesson 5: Billing, Pricing, and Support

---

Welcome to Domain Four — Billing, Pricing, and Support. This domain represents twelve percent of your exam score. Twelve percent might sound small compared to Domain Three's thirty-four percent, but do not underestimate it. The questions in this domain have very clear right and wrong answers. If you know the support plan response times, the pricing model definitions, the cost management tools, and the consolidated billing rules, you will answer every billing question correctly. That is a reliable block of points you do not want to leave on the table.

Let me tell you how we are going to approach this lesson. We will start with the foundational principles of how AWS charges money, move through the specific pricing models for EC2, then cover the cost management tools, consolidated billing, support plans, and cost optimization strategies. At the end, we will work through review questions so you can test your recall.

---

## The Three Principles of AWS Pricing

Everything about AWS pricing flows from three core principles, and you should be able to state all three from memory.

The first is pay-as-you-go. You pay only for the resources you consume, with no upfront commitments required and no minimum fees for most services. When you stop using a resource, you stop paying for it. There are no termination fees unless you chose a Reserved Instance commitment. This is the fundamental shift from capital expenditure — buying servers — to operational expenditure — paying a usage-based bill.

The second is pay less when you reserve. If you commit to using a resource for one or three years, AWS gives you a significant discount compared to on-demand pricing. This is the Reserved Instance and Savings Plan model. The trade-off is commitment for savings.

The third is pay less with volume. As your usage grows, the price per unit decreases automatically through tiered pricing. You do not need to negotiate — the discount applies automatically. This is most visible with data transfer and storage, where the first block of usage costs the most per unit and each additional block costs less.

These three principles explain virtually every pricing decision you will encounter. When a question asks why a particular pricing model makes sense for a particular scenario, one of these three principles is usually the answer.

---

## The AWS Free Tier

AWS offers three types of free tier offerings, and the exam distinguishes between them.

The first type is always free. These are services where a certain amount of usage never costs anything, regardless of how long your account has existed. Lambda gives you one million free requests per month and a generous amount of compute time — always free. DynamoDB gives you twenty-five gigabytes of storage — always free. SNS gives you one million publishes per month — always free. CloudWatch gives you ten custom metrics and alarms — always free. These do not expire.

The second type is twelve months free. Starting from the date you create your AWS account, you have twelve months of free access to a limited amount of certain services. EC2 gives you seven hundred fifty hours per month of a t2.micro or t3.micro instance — that is enough to run one instance continuously for an entire month. S3 gives you five gigabytes of standard storage. RDS gives you seven hundred fifty hours per month of a db.t2.micro instance. CloudFront gives you fifty gigabytes of data transfer out. These expire after twelve months from account creation.

The third type is trials. Some services offer short-term free access for new users — SageMaker for a period after you first use it, Inspector for ninety days, Lightsail for the first month. These are meant to let you evaluate a service before committing to paying for it.

The exam tests one important detail about the twelve-month EC2 free tier: the seven hundred fifty hours applies to your entire account, not per instance. If you run two t2.micro instances simultaneously, you are consuming one thousand four hundred sixty hours per month, which exceeds the limit and incurs charges for the excess.

The practical lesson is always to set up billing alerts when experimenting with the free tier, so that unexpected charges do not catch you off-guard.

---

## EC2 Pricing Models

You covered the EC2 pricing models in depth in Lesson Four. Let me reinforce the key exam patterns here because this topic appears in the billing domain as well as the technology domain.

On-Demand instances are the default. You pay by the second with no commitment. They are the most expensive option per hour but require no upfront investment and no minimum term. Use On-Demand for short-term, unpredictable workloads and for anything you are testing for the first time.

Reserved Instances require a one-year or three-year commitment in exchange for up to seventy-five percent savings compared to On-Demand. The three payment options are all upfront, which gives the deepest discount; partial upfront, which balances the upfront payment with a lower monthly rate; and no upfront, which has no upfront payment but the lowest discount of the three. Among these, all upfront three-year provides the maximum possible discount.

Standard Reserved Instances lock in the instance type, but Convertible Reserved Instances allow you to exchange them for a different instance type, family, or operating system. Convertible RIs offer a slightly lower discount in exchange for this flexibility.

Savings Plans work like Reserved Instances but with more flexibility. Instead of committing to a specific instance type, you commit to spending a certain dollar amount per hour. Compute Savings Plans cover EC2 in any Region, any size, any OS, plus Fargate and Lambda. EC2 Instance Savings Plans provide a higher discount but commit you to a specific instance family in a specific Region, with flexibility within that family.

Spot Instances let you use unused AWS capacity at up to ninety percent off On-Demand pricing. The risk is that AWS can reclaim Spot Instances with only two minutes of warning. Use Spot for batch processing jobs, data analytics, rendering, and any fault-tolerant workload where you can checkpoint progress and restart if interrupted. Never use Spot for anything that must stay running — databases, web servers under active load, or anything where an unexpected interruption would cause data loss or service outages.

Dedicated Hosts give you a physical server dedicated to your use, where you can bring your own per-socket or per-core software licenses. This is the answer for Microsoft and Oracle license compliance, where the license terms count physical processor sockets or cores.

---

## Data Transfer Pricing

Data transfer costs are often overlooked and frequently appear on the exam. The rule is simple to state but important to remember.

Data transfer into AWS from the internet is free. When users upload files to S3, when your office connects to AWS, when an external service sends data to your application — all of that inbound transfer costs nothing.

Data transfer out of AWS to the internet is charged, and it follows tiered pricing. The first ten terabytes per month cost a certain amount per gigabyte, and each subsequent tier costs a little less. This is the volume discount principle in action.

Data transfer between AWS Regions is charged. If your application in the US East Region queries a database in the EU West Region on every request, every query incurs inter-region data transfer charges. The solution is to keep resources in the same Region.

Data transfer between Availability Zones within the same Region is charged, but at a lower rate. Data transfer within the same AZ using private IP addresses is free.

Data transfer from AWS services to CloudFront is free. CloudFront's data transfer out to end users is charged, but at rates lower than direct data transfer from EC2 or S3 to the internet. This is one reason using CloudFront for content delivery saves money even when you account for CloudFront's own costs.

VPC Endpoints eliminate the need to route traffic through a NAT Gateway for AWS services like S3 and DynamoDB. Because a NAT Gateway charges per gigabyte of data it processes, removing it from the path for large S3 transfers can significantly reduce costs.

---

## Reserved Instances Versus Savings Plans

The exam sometimes asks you to distinguish between these two commitment mechanisms. Here is how to think about the trade-off.

Reserved Instances are tied to a specific service. You buy a Reserved Instance for RDS, ElastiCache, Redshift, or EC2. Standard Reserved Instances for EC2 are tied to a specific instance type, Region, and platform. Convertible Reserved Instances allow more changes but provide a lower discount. Standard Reserved Instances can be sold on the Reserved Instance Marketplace if you no longer need them, which provides an exit option that Savings Plans do not have.

Savings Plans are dollar commitments per hour that automatically apply to eligible compute usage. Compute Savings Plans are the most flexible — they apply to any EC2 instance family, size, Region, or operating system, as well as to Fargate and Lambda. EC2 Instance Savings Plans provide a higher discount than Compute Savings Plans but require you to commit to a specific instance family in a specific Region.

The practical decision rule is this. If your workload is completely stable — you know exactly which instance type you will run in which Region for the next three years, and you want the maximum possible discount — a Standard Reserved Instance for a three-year term with all upfront payment is the right choice. If your workload uses a mix of EC2 types, or you use Fargate or Lambda, or you expect to change instance types over time, a Compute Savings Plan gives you nearly the same discount with far more flexibility.

For RDS, ElastiCache, and Redshift, Reserved Instances are still the right commitment mechanism because Savings Plans do not cover those services.

---

## Cost Management Tools

AWS provides several tools for understanding, tracking, and optimizing costs. The exam tests which tool is appropriate for each scenario.

The AWS Pricing Calculator is a free tool for estimating costs before you deploy anything. You do not even need an AWS account to use it. You configure the services you plan to use, select the options you intend to configure, and the calculator estimates your monthly cost. It is used for planning new deployments, comparing Reserved Instance pricing against On-Demand, and building business cases for migration. The key phrase for the exam is "estimate before deploying."

AWS Cost Explorer is for analyzing historical costs and usage. It provides up to twelve months of historical data and forecasts costs up to twelve months into the future. You can filter and group costs by service, Region, account, tag, or instance type. Cost Explorer has a free user interface, though programmatic API access costs per request. The key phrase is "understand and visualize past spending."

AWS Budgets lets you set custom cost and usage alerts. You define a budget amount and thresholds, and AWS sends notifications via email or SNS when you approach or exceed those thresholds. You can set multiple thresholds — for example, notify at eighty percent, at one hundred percent, and at one hundred twenty percent of budget. You can also configure Budget Actions, which trigger automated responses like stopping instances when a budget is exceeded. The first two budgets per account are free. The key phrase is "alert when spending crosses a threshold."

The AWS Cost and Usage Report is the most detailed billing data available. It provides line-item records for every charge, broken down by service, operation, resource, and tag, delivered to an S3 bucket in CSV or Parquet format. You can query it with Athena using SQL or load it into Redshift for analysis. It is the right tool when you need to do deep analysis, build custom billing reports, or implement chargeback reporting for multiple teams. It is free — you only pay for S3 storage. The key phrase is "deepest detail for analysis and reporting."

AWS Cost Anomaly Detection uses machine learning to automatically identify unusual spending patterns. You do not set manual thresholds — the service learns your normal spending patterns and sends alerts when spending deviates significantly. It identifies the root cause of anomalies and provides recommendations. It has no additional cost. The key phrase is "detect unexpected cost spikes automatically."

The exam distinguishes these tools by use case. If a scenario mentions estimating before deploying, it is the Pricing Calculator. If it mentions reviewing historical costs, it is Cost Explorer. If it mentions alerting when spending exceeds a limit, it is Budgets. If it mentions detailed line-item analysis or chargeback reporting, it is the Cost and Usage Report. If it mentions automatic anomaly detection without manual thresholds, it is Cost Anomaly Detection.

---

## Total Cost of Ownership

Total Cost of Ownership, or TCO, is the concept of comparing the full cost of running infrastructure on-premises versus in the cloud. AWS offers a TCO Calculator, now integrated into the Migration Evaluator, that helps organizations build the business case for migration.

The key insight is that on-premises infrastructure has many hidden costs beyond the hardware purchase. These include the physical data center space, power and cooling, physical security, hardware maintenance contracts, operating system and software licenses, and — often the largest category — the personnel required to manage all of it. Systems administrators, storage administrators, and network administrators represent ongoing labor costs that organizations often undercount when comparing on-premises to cloud.

When you move to AWS, you eliminate the hardware purchase, the data center costs, most of the software licensing overhead, and a significant portion of the operational staff burden, because AWS manages the underlying infrastructure. You still need cloud-focused engineers, but their skills shift toward architecture and optimization rather than hardware maintenance.

The TCO comparison frequently shows cloud migration providing meaningful savings over three years, even after factoring in migration costs and ongoing AWS support. For the exam, you do not need to calculate specific TCO figures — you need to understand that TCO analysis accounts for the full cost of ownership including hidden on-premises costs, and that it is used to justify migration business cases.

---

## Cost Allocation Tags

Tags are key-value pairs you attach to AWS resources. When you activate tags as cost allocation tags in the Billing Console, they appear in Cost Explorer and the Cost and Usage Report, allowing you to filter and group costs by tag. This is how organizations track which team, project, department, or application is responsible for each dollar of AWS spending.

Common tag keys include Environment — with values like Production, Staging, Development, and Test — Owner, CostCenter, Project, Application, and BillingGroup. Organizations typically define a set of required tags and enforce them through AWS Config rules that flag resources missing required tags, or through Service Control Policies that prevent resource creation without the required tags in place.

The value of tagging for billing is that it enables chargeback — actual billing to departments from their budget — and showback — informational reporting that makes teams aware of their consumption without affecting their budget directly. Most organizations start with showback to build cost awareness and then move toward chargeback as tagging discipline matures.

---

## Consolidated Billing and AWS Organizations

When you manage multiple AWS accounts under AWS Organizations, consolidated billing combines all accounts into a single payment. This has several important implications the exam tests directly.

First, you receive one bill for the entire organization, paid by the management account, even though each member account's costs are tracked separately. This simplifies the payment process without losing visibility into per-account costs.

Second, usage from all accounts in the organization is aggregated for volume discounts. If one account uses eight terabytes of S3 storage and another uses five terabytes, AWS prices the combined thirteen terabytes together, potentially placing the organization in a lower-cost tier than either account would reach individually.

Third, Reserved Instances and Savings Plans can be shared across accounts in the organization by default. If the production account purchases Reserved Instances and does not use all of them in a given hour, the unused capacity automatically provides the RI discount to matching instances in other member accounts. This maximizes utilization of your commitments across the organization. You can disable this sharing if you want each account's costs to be completely isolated.

Fourth, the AWS Free Tier applies once per organization, not once per member account. Adding new accounts to an existing organization does not give you additional free tier allowances.

Consolidated billing is free — it is a feature of AWS Organizations with no additional charge.

---

## AWS Support Plans

The four AWS Support plans are one of the most reliably tested topics in Domain Four. You should memorize the key attributes of each plan and be able to answer scenario questions about which plan a company needs based on their requirements.

Basic Support is included for every AWS account at no charge. It gives you access to customer service for billing and account questions, AWS documentation and whitepapers, the AWS community forum called re:Post, the seven core Trusted Advisor checks, and the Personal Health Dashboard. There is no technical support for architecture or troubleshooting under Basic Support.

Developer Support has a minimum cost of twenty-nine dollars per month, or three percent of your monthly AWS usage, whichever is greater. It allows one primary contact to open technical support cases via email during business hours. Response times are less than twenty-four hours for general guidance and less than twelve hours for a system impaired situation. It provides general architectural guidance — advice about how to think about your architecture, but not specific to your particular use case. Developer is appropriate for accounts in testing or development where occasional technical guidance during business hours is sufficient.

Business Support has a minimum cost of one hundred dollars per month, with a tiered percentage of AWS spend above that minimum. It allows unlimited contacts to open support cases through email, phone, and chat available twenty-four hours a day, seven days a week. Response times are less than four hours for a production system impaired situation and less than one hour for a production system down or business-critical system down situation. It provides all Trusted Advisor checks, not just the seven core checks. It includes contextual architectural guidance specific to your use case. It also covers third-party software support — if you are running MySQL, Apache, or other common software on AWS and have an issue with how it interacts with AWS services, Business Support will help. Business Support is appropriate for any account running production workloads.

Enterprise Support has a minimum cost of fifteen thousand dollars per month, with a tiered percentage of AWS spend. It includes everything from Business Support plus several additional capabilities. The most important is a dedicated Technical Account Manager, or TAM, who serves as a proactive partner in managing your AWS environment, conducting regular reviews, helping with operational planning, and coordinating resources within AWS when you need escalation. The fifteen-minute response time for mission-critical system down situations is unique to Enterprise Support — Business Support's fastest response is one hour. Enterprise also includes the Concierge Support Team for billing and account questions, Infrastructure Event Management for planned events like product launches and migrations, and facilitated Well-Architected Reviews. Enterprise is appropriate for organizations running mission-critical workloads where the cost of downtime is very high.

Let me state the response time structure clearly because the exam tests it directly. Under Developer: general guidance is less than twenty-four business hours, system impaired is less than twelve business hours, and there are no faster response categories available. Under Business: general guidance is less than twenty-four hours, system impaired is less than twelve hours, production system impaired is less than four hours, and production or business-critical system down is less than one hour. Enterprise adds: mission-critical system down in less than fifteen minutes, which is not available on any other plan.

The Trusted Advisor distinction is also tested. Basic and Developer plans get seven core checks, covering the most critical security and service limit items — S3 bucket permissions, Security Groups with unrestricted ports, IAM use, MFA on the root account, publicly accessible EBS snapshots, publicly accessible RDS snapshots, and service limits. Business and Enterprise plans get all Trusted Advisor checks across all five categories: cost optimization, performance, security, fault tolerance, and service limits, covering many more services and configurations.

---

## Cost Optimization Strategies

The exam tests your ability to recommend cost optimization approaches for various scenarios. Here are the primary strategies and when to recommend each one.

Right-sizing means matching the instance type and size to the actual workload requirements. Many organizations over-provision — they buy large instances to be safe, then discover those instances run at ten to twenty percent CPU utilization. AWS Compute Optimizer analyzes CloudWatch utilization data using machine learning and recommends right-sized alternatives. Cost Explorer also provides right-sizing recommendations based on your historical usage. The important sequence is right-size before you buy commitments, because Reserved Instances on over-provisioned instances waste money at a discounted rate rather than saving it.

Reserved capacity and Savings Plans are the primary mechanism for reducing cost on steady-state workloads. Analyze thirty to sixty days of usage patterns before purchasing commitments. Start with one-year terms to reduce risk. Purchase commitments to cover seventy to eighty percent of your baseline usage, and handle the remaining variable capacity with On-Demand or Spot.

Spot Instances provide up to ninety percent savings for workloads that tolerate interruption. The ideal use cases are batch processing, big data analytics, CI/CD pipeline runners, rendering, and containerized workloads with auto-restart capability. The key requirement is that the application must be able to save its state and resume from where it left off if the instance is reclaimed.

Auto Scaling eliminates waste during low-demand periods and prevents under-provisioning during high-demand periods. For a web application that receives traffic during business hours and is quiet overnight, Auto Scaling can run two instances at night and ten during the day, paying for average load rather than peak capacity all the time.

Storage optimization uses S3 Lifecycle Policies to move objects to lower-cost storage classes as they age. Data that is actively accessed belongs in S3 Standard. Data accessed less than once a month belongs in S3 Standard-IA. Archive data that is rarely needed belongs in S3 Glacier Flexible Retrieval. Data that must be retained for years and will almost never be retrieved belongs in S3 Glacier Deep Archive. Setting lifecycle rules to automatically transition data through these tiers as it ages is one of the simplest and most reliable cost optimizations available.

Shutting down non-production resources outside business hours is often the single highest-impact action for development and test environments. An instance scheduler running via EventBridge can stop development EC2 instances and RDS databases at seven in the evening and start them again at eight in the morning. For a twelve-hour business day with weekends off, this reduces runtime from seven hundred thirty hours per month to approximately two hundred forty hours — a sixty-seven percent reduction in compute time with essentially no engineering effort.

Using CloudFront instead of serving content directly from EC2 or S3 reduces data transfer costs because CloudFront's transfer-out rates are lower, and its caching reduces the volume of data that must be served from the origin for repeated requests.

VPC Endpoints for S3 and DynamoDB route traffic through AWS's internal network rather than through a NAT Gateway, eliminating the per-gigabyte NAT Gateway data processing charge for those services — which can be significant for applications that read or write large volumes of data to S3 or DynamoDB.

---

## Review Questions

Let me work through the review questions for this domain. I will ask each one, pause for you to think, then walk through the answer.

Question One. Which AWS pricing principle allows customers to pay only for the compute resources they consume, with no upfront costs and no minimum commitments?

The answer is pay-as-you-go. This is the foundational pricing principle that enables customers to start using AWS with no capital expenditure, scale up and down as needed, and stop paying the moment they stop consuming resources. The other two core pricing principles — pay less when you reserve, and pay less with volume — build on top of pay-as-you-go but require either a commitment or a usage threshold to activate their discounts.

Question Two. A company needs to reduce EC2 costs for a steady-state production workload that runs twenty-four hours a day, seven days a week. They want the highest possible discount and are willing to commit for three years. Which purchasing option provides the most cost savings?

The answer is three-year Reserved Instances with all upfront payment. For a completely steady workload where you are confident the same instance type will be needed for three years, all upfront three-year Reserved Instances provide the maximum possible discount — up to seventy-two to seventy-five percent savings compared to On-Demand. Spot Instances offer higher discounts in theory, but they are not appropriate for production workloads that cannot tolerate interruption.

Question Three. Which AWS Support plan includes a Technical Account Manager as a dedicated point of contact?

The answer is Enterprise Support. The TAM is unique to Enterprise — no other plan includes this dedicated proactive relationship. This is one of the most frequently tested support plan facts on the exam.

Question Four. A company's production application goes completely offline on a Friday evening. They have the Business Support plan. What is the maximum guaranteed response time for this situation?

The answer is less than one hour. A production system down is covered under Business Support's one-hour response time commitment. If the company had Enterprise Support, a mission-critical system down would receive a fifteen-minute response. Developer Support does not provide faster-than-twelve-hour response and does not cover production system down scenarios at all.

Question Five. A company is planning to migrate fifteen servers from their on-premises data center to AWS. They want to estimate their monthly AWS costs before starting the migration, without deploying any resources yet. Which tool should they use?

The answer is the AWS Pricing Calculator. The Pricing Calculator is specifically designed for pre-deployment cost estimation. You can configure virtual instances, storage, databases, and networking services and get an estimated monthly cost before committing to any resources. Cost Explorer analyzes costs that have already been incurred — it cannot estimate future deployments that do not yet exist.

Question Six. A large enterprise has twelve separate AWS accounts for different business units and wants to receive a single monthly invoice for all accounts while still seeing costs broken down per account. Which AWS feature provides this?

The answer is AWS Organizations with consolidated billing. Consolidated billing aggregates all linked accounts into a single payment from the management account while maintaining per-account cost visibility. It also aggregates usage for volume discounts and allows Reserved Instance sharing across accounts. There is no additional charge for this feature.

Question Seven. A company's AWS bill unexpectedly doubled last month. The operations team wants to be automatically notified in the future if spending increases significantly and abnormally, without having to set a specific dollar threshold themselves. Which service should they configure?

The answer is AWS Cost Anomaly Detection. Cost Anomaly Detection uses machine learning to understand your normal spending patterns and alerts you when spending deviates from those patterns. You do not configure a specific threshold — the service determines what constitutes an anomaly based on historical data. This is distinct from AWS Budgets, which requires you to define a specific dollar or percentage threshold manually.

Question Eight. How many Trusted Advisor checks are available under the Basic and Developer support plans?

The answer is seven core checks. These seven cover the most critical security and service limit items: S3 bucket permissions, Security Groups with unrestricted ports, IAM use, MFA on the root account, publicly accessible EBS snapshots, publicly accessible RDS snapshots, and service limits. Business and Enterprise plans unlock all Trusted Advisor checks across all five categories — cost optimization, performance, security, fault tolerance, and service limits.

Question Nine. Which AWS Free Tier offering is truly permanent and never expires?

The answer is Lambda's one million requests per month. Lambda is part of the always-free category of Free Tier offerings — that allocation never expires regardless of how long your account has existed. EC2's seven hundred fifty hours per month, S3's five gigabytes, RDS's seven hundred fifty hours per month, and CloudFront's fifty gigabytes of transfer are all twelve-month offers that expire one year after account creation.

Question Ten. A development team spends around fifteen hundred dollars per month on AWS and wants to receive an email alert if they are on track to exceed two thousand dollars in any given month. Which service should they use?

The answer is AWS Budgets. Budgets allows you to set a monthly cost threshold and configure alerts at percentages of that threshold — for example, at eighty percent forecasted spend, at one hundred percent actual, and at one hundred twenty percent actual. You can send notifications via email or to an SNS topic. The first two budgets in an account are free.

Question Eleven. Which data transfer scenario is generally free in AWS?

The answer is data transfer into AWS from the internet. Inbound data transfer — uploads to S3, data ingestion to Kinesis, API requests arriving at your application — is free. Data transfer out to the internet, between Regions, and between Availability Zones all carry charges.

Question Twelve. A genomics research company runs large-scale DNA sequencing jobs that take twelve to forty-eight hours to complete. The jobs can be checkpointed and restarted automatically if interrupted. They want to minimize compute costs. Which EC2 purchasing option is most appropriate?

The answer is Spot Instances. The workload is fault-tolerant — jobs can be checkpointed and restarted, so interruption is acceptable. The work is large-scale batch processing with flexible timing that can take advantage of unused capacity at a significant discount. Spot Instances provide up to ninety percent savings compared to On-Demand, making them the right choice when fault tolerance is in place.

Question Thirteen. A finance team needs to perform a detailed analysis of AWS spending at the individual resource level, broken down by cost center tags, for regulatory auditing purposes. They need data delivered to S3 in a format they can query with SQL. Which AWS service provides this?

The answer is the AWS Cost and Usage Report. The CUR delivers line-item records for every charge, including resource IDs and tag values, to an S3 bucket in CSV or Parquet format. The finance team can then query it with Athena to produce the detailed breakdowns they need for auditing. Cost Explorer does not provide individual resource-level detail or S3 delivery in this format.

Question Fourteen. A startup is deploying their first production application on AWS and wants technical support available any time their application goes down, including weekends and holidays, with a response time of less than one hour for critical failures. What is the minimum support plan they need?

The answer is Business Support. Business is the minimum plan that provides twenty-four-hour, seven-day-a-week phone, email, and chat support with a one-hour response time for production system down situations. Developer Support only provides business hours email access with twelve-hour response times. Basic provides no technical support at all.

---

## Key Patterns for the Exam

Let me close with the patterns that will help you navigate billing questions quickly and confidently.

For EC2 pricing, let the workload characteristic lead you to the answer. Unpredictable short-term needs are On-Demand. Steady-state production workloads are Reserved Instances or Savings Plans. Fault-tolerant batch workloads are Spot. Per-socket software license compliance needs Dedicated Hosts.

For cost management tools, let the use case phrase in the question lead you to the answer. Estimating before you build is the Pricing Calculator. Analyzing historical spending is Cost Explorer. Setting alerts on a budget threshold is Budgets. Deep line-item detail for reporting or auditing is the Cost and Usage Report. Detecting unexpected spending automatically without manual thresholds is Cost Anomaly Detection.

For support plans, remember the two key escalation thresholds. Business Support is the minimum for twenty-four-seven phone support and one-hour critical response. Enterprise Support is the minimum for a Technical Account Manager and fifteen-minute mission-critical response. Trusted Advisor: seven core checks on Basic and Developer, all checks on Business and Enterprise.

For data transfer pricing: inbound from the internet is free. Outbound to the internet is charged. Same-AZ private traffic is free. Cross-AZ is charged. Cross-Region is charged. CloudFront transfer from AWS origin to edge is free — only the transfer from edge to user is charged.

For consolidated billing: one bill for the whole organization, usage aggregated for volume discounts, Reserved Instances shared across accounts by default, Free Tier applied once per organization not per account. All at no additional cost.

For the cost optimization sequence: right-size first before buying commitments, then purchase commitments for your baseline load, then layer Spot Instances on top for any fault-tolerant variable work. This order matters because purchasing Reserved Instances on over-provisioned resources wastes your discount.

These patterns are reliable. Domain Four rewards preparation. If you have worked through this lesson, you know enough to answer every billing question on the exam.

---
