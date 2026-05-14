# Lesson 10: Additional AWS Services

---

Welcome to the final content lesson. Today we are covering the services that did not fit neatly into the four main exam domains but still appear on the exam, sometimes in direct questions and sometimes as answer choices in scenario-based questions. You need to know what each of these services does and when you would use it, even if you will not be tested on it in depth.

I want to be honest with you about this lesson. The services here are not the primary focus of the exam. The core domains — Cloud Concepts, Security, Cloud Technology and Services, and Billing and Support — represent one hundred percent of the exam. But within those domains, especially Domain Three, additional services appear as both correct answers and plausible distractors. If you do not recognize a service name, you cannot evaluate whether it is the right answer for a scenario. That is what this lesson prepares you for.

We are going to move through several categories of services. For each category, I will explain what the services do, how they relate to each other, and what the exam is most likely to test about them.

---

## Developer Tools and the CI/CD Pipeline

These are the services AWS provides for software development and automated deployment. They do appear on the exam, and you covered the pipeline scenario in Lesson Nine.

AWS CodeCommit is a fully managed Git-based source control repository. It works exactly like GitHub or GitLab but runs inside AWS, with encryption at rest and in transit and integration with IAM for access control. There are no size limits on repositories. When the exam describes a team that wants to store code securely in AWS without managing a version control server, CodeCommit is the answer.

AWS CodeBuild is a fully managed build service. It compiles source code, runs automated tests, and produces deployable packages. You do not manage build servers — CodeBuild scales automatically and you pay only for the time your builds are actually running. It supports common programming languages and can use custom Docker images for specialized build environments. It integrates with CodeCommit, GitHub, and Bitbucket.

AWS CodeDeploy automates the deployment of applications to EC2 instances, Lambda functions, and on-premises servers. It supports in-place deployments, where the application is updated in the same environment, and blue/green deployments, where a new environment is created and traffic is shifted over. Automatic rollback triggers can revert a deployment if CloudWatch alarms detect problems after deployment. There is no additional charge for CodeDeploy — you pay only for the AWS resources being deployed to.

AWS CodePipeline is the orchestration layer that connects all the other Code services into a continuous delivery pipeline. A pipeline has stages: source, build, test, and deploy. When code is pushed to the repository, CodePipeline automatically starts the pipeline, runs the build in CodeBuild, runs any tests, and deploys through CodeDeploy. Third-party services like GitHub and Jenkins can also integrate into CodePipeline stages.

When the exam asks about automating the release process from code commit to production deployment, the answer is CodePipeline. When it asks specifically about the build step, it is CodeBuild. When it asks about the deployment step, it is CodeDeploy. When it asks about storing source code in AWS, it is CodeCommit.

AWS CodeStar provides a unified user interface for managing development activities across CodeCommit, CodeBuild, CodeDeploy, and CodePipeline. It offers project templates for various languages and platforms and includes an integrated dashboard for monitoring and team collaboration. CodeStar does not add cost beyond the underlying services — it is a management layer on top of them.

AWS Cloud9 is a cloud-based integrated development environment that runs in your browser. You write, run, and debug code without installing anything locally. It includes a terminal with the AWS CLI pre-installed, making it convenient for working with AWS resources directly from the IDE. Cloud9 runs on an underlying EC2 instance that you pay for, though the IDE itself does not carry an additional charge. It supports over forty programming languages and includes collaborative coding features.

---

## Application Integration Services

Application integration services allow different components of an application to communicate without being tightly coupled to each other.

Amazon EventBridge is a serverless event bus. An event bus receives events from sources — AWS services, your own applications, or third-party SaaS providers — and routes them to target services based on rules you define. For example, an event that says an EC2 instance was terminated could trigger a Lambda function to send an alert. An event from a third-party application like Salesforce or Zendesk could trigger a workflow in your own systems. EventBridge was formerly known as CloudWatch Events, and you will see both names referenced. When the exam describes event-driven integration between services or automatic responses to state changes, EventBridge is often the answer.

Amazon MQ is a managed message broker service. It supports the Apache ActiveMQ and RabbitMQ brokers and implements standard messaging protocols including MQTT, AMQP, and STOMP. Amazon MQ is not the same as SQS or SNS. SQS and SNS are AWS-native services built for new cloud applications. Amazon MQ is for migrating existing on-premises applications that already use these industry-standard message broker protocols and need a managed version of the same broker in the cloud. If a question describes an application already using RabbitMQ or ActiveMQ that needs to move to AWS without rewriting the messaging layer, Amazon MQ is the answer.

AWS App Mesh is a service mesh for microservices architectures. It monitors and controls communications between services and works with ECS, EKS, and EC2. App Mesh is based on the Envoy proxy and provides traffic management and observability across your services. You pay only for the underlying resources — App Mesh itself has no additional charge.

---

## End User Computing

End user computing services allow organizations to provide desktop experiences and applications to users without those users needing traditional on-premises desktops.

Amazon WorkSpaces provides managed virtual desktops in the cloud. Each user gets a persistent Windows or Linux desktop that they can access from any device, anywhere, using a client application or a web browser. WorkSpaces integrates with Microsoft Active Directory for centralized user management. It is priced either on a monthly basis for consistent daily users or on an hourly basis for users who only need access occasionally. When the exam describes remote workers, contractors who need temporary desktop access, or a bring-your-own-device environment where corporate data should not be stored on personal machines, WorkSpaces is the answer.

Amazon AppStream 2.0 is different from WorkSpaces in an important way. WorkSpaces gives users a full persistent desktop. AppStream 2.0 streams individual applications to a web browser. The application itself runs on AWS, and only the visual output is sent to the user's screen. Users do not need to install the application. This is useful for distributing specialized software like CAD tools or enterprise applications to users who should not have a full desktop but need access to specific software. Common use cases include software trials, training environments, and proof-of-concept demonstrations.

The exam distinction between WorkSpaces and AppStream is this: WorkSpaces is for a full persistent desktop. AppStream is for streaming specific applications without giving users a full desktop.

Amazon WorkDocs is a secure document storage and collaboration service, similar in concept to Dropbox or Google Drive. It provides one terabyte of storage per user, supports file comments and feedback, integrates with Active Directory, and includes mobile and desktop apps. When a company wants cloud-hosted document collaboration managed by AWS, WorkDocs is the appropriate service.

Amazon WorkLink provides secure mobile access to internal corporate websites and web applications without requiring a VPN. WorkLink renders content in a browser running on AWS, then sends only the visual output as pixels to the user's mobile device. The device never has direct access to the corporate network or the content itself, which protects internal systems from potentially compromised mobile devices.

---

## Internet of Things Services

The IoT services connect physical devices to the cloud and process the data they generate.

AWS IoT Core is the foundational service for connecting IoT devices to AWS. It can support billions of devices communicating over standard protocols including MQTT, HTTPS, and WebSockets. IoT Core includes a rules engine that can route incoming device data to other AWS services like Lambda, DynamoDB, S3, or Kinesis based on the content of the messages. Device Shadow is a feature that maintains a persistent virtual representation of each device's state in the cloud, so applications can interact with a device's last known state even when the device is offline.

AWS IoT Greengrass extends AWS capabilities to edge devices. Instead of sending all data from a device to the cloud for processing, Greengrass allows Lambda functions to run directly on the edge device. This is useful when devices need to operate without internet connectivity, when latency from sending data to the cloud is unacceptable, or when only processed results rather than raw data need to be sent to the cloud. Machine learning inference can also run at the edge using Greengrass.

AWS IoT Analytics is a service for processing and analyzing data generated by IoT devices. It applies SQL queries to time-series data, supports pre-built analytics templates, and integrates with QuickSight for visualization and with machine learning services for predictive analytics. When a scenario describes analyzing the streams of data coming from connected devices, IoT Analytics is the service that processes and makes sense of that data.

The exam distinction: IoT Core connects devices to AWS and processes data in the cloud. IoT Greengrass brings compute and machine learning to the edge device itself. When a scenario mentions edge computing, offline operation, or local processing on IoT devices, Greengrass is the answer.

---

## Media Services

AWS provides services for converting, processing, and streaming video content.

Amazon Elastic Transcoder converts media files from one format to another. A company that needs to convert uploaded videos into formats compatible with different devices and screen sizes uses Elastic Transcoder. It provides pre-built presets for common output formats and charges per minute of video transcoded. It integrates directly with S3 and CloudFront.

AWS Elemental MediaConvert is the more advanced, broadcast-grade video transcoding service. It supports a wider range of formats, codecs, and features required for professional media production and broadcast distribution. When the scenario involves professional video production or broadcast-quality output, MediaConvert is the appropriate choice over Elastic Transcoder.

Amazon Kinesis Video Streams captures streaming video from cameras and other video sources, stores it durably, and makes it available for playback and analysis. Security cameras, smart doorbells, and manufacturing inspection cameras are typical sources. It integrates with machine learning services like Rekognition for real-time video analysis. You pay for data ingested and consumed.

---

## Additional AI and Machine Learning Services

You covered the core AI services in previous lessons. Let me reinforce the key ones here because these are tested by use case, and knowing which service to choose based on a one-sentence description is the entire skill.

Amazon Polly converts text to lifelike speech. It supports over sixty voices across thirty or more languages and includes a neural text-to-speech option that produces the most natural-sounding output. Polly is used for accessibility features in applications, e-learning narration, interactive voice response systems, and any application that needs to speak to users. It charges per character of text converted.

Amazon Transcribe converts speech to text. It processes both real-time audio streams and recorded audio files. It can identify multiple speakers in a recording, supports custom vocabulary for domain-specific terms, and has specialized versions for medical transcription and call center analytics. You pay per second of audio processed.

Amazon Translate performs neural machine translation between languages. It supports over seventy-five languages and can be called via API to translate text on demand. Custom terminology lets you define specific translations for branded terms or technical vocabulary. When the exam describes a company that needs to translate user-generated content or localize an application into multiple languages automatically, Translate is the answer.

Amazon Forecast is a time-series forecasting service. You provide historical time-series data, Forecast applies machine learning to identify patterns, and it produces predictions. Common use cases are demand forecasting for retail inventory, energy consumption forecasting, and financial planning. No machine learning expertise is required — you provide the data and Forecast handles the model training. Forecast is typically more accurate than traditional statistical forecasting methods.

Amazon Kendra is an intelligent enterprise search service. Where a traditional keyword search returns documents containing the searched words, Kendra uses natural language understanding to return the specific answer to a question. Kendra learns from user interactions over time to improve relevance. It can connect to a variety of data sources including S3, SharePoint, and databases. When the exam describes an enterprise that wants employees to search internal documents, wikis, or knowledge bases using natural language questions and get precise answers rather than a list of documents, Kendra is the answer.

Amazon Personalize provides recommendation engines using the same technology that powers recommendations on Amazon.com. You provide user interaction data and a catalog of items, and Personalize produces real-time or batch recommendations. No machine learning expertise is required. Use cases include product recommendations for e-commerce, content recommendations for media streaming, and personalized search results.

Amazon Textract goes beyond simple optical character recognition. It extracts structured data from scanned documents including forms, tables, and handwriting, preserving the relationships between fields and values. When the exam describes extracting data from invoices, insurance forms, government documents, or any scanned paperwork where the structure matters — not just the text — Textract is the answer. You pay per page processed.

---

## Business Applications

These services support business communication and customer engagement.

Amazon Connect is a cloud-based contact center service. It handles inbound and outbound customer calls, supports omnichannel interactions including voice and chat, and includes artificial intelligence features like chatbots and automatic transcription of calls. Organizations pay based on usage rather than per agent per month. When the exam describes a company setting up a customer support center in the cloud, Amazon Connect is the answer.

Amazon Simple Email Service, known as SES, is an email sending and receiving service. It is designed for high-volume transactional and marketing email at low cost. SES handles deliverability, bounce handling, and email validation. Common use cases are order confirmations, password reset emails, notification emails, and bulk marketing campaigns. It integrates directly with applications through an API. There is a generous free tier for email sent from EC2 instances.

The exam distinguishes SES from SNS: SES is for sending formatted email messages to people, while SNS is for sending notifications to application endpoints, SMS, or email as one of many delivery channels. When the requirement is specifically about email to end users, SES is the right service. When the requirement is about notifying multiple systems or subscribers simultaneously, SNS is more appropriate.

Amazon Pinpoint is a customer engagement platform for multi-channel marketing campaigns. It supports email, SMS, push notifications, and voice messages, with tools for customer segmentation, campaign scheduling, and analytics on engagement rates and conversions. You pay for the messages sent.

AWS WorkMail is a fully managed business email and calendar service with its own mail server infrastructure. It supports standard email clients and protocols and integrates with Active Directory. When a company wants AWS to host their corporate email rather than running their own Exchange server or using a third-party provider, WorkMail is the answer. It is priced per user per month.

Amazon Chime is a communications service that includes video meetings, screen sharing, and team chat. It serves as an alternative to services like Zoom or Microsoft Teams. Chime uses per-user per-day pricing, which can make it cost-effective for organizations that use video conferencing infrequently.

---

## Advanced Management and Governance

AWS License Manager helps organizations track and control software license usage across their AWS resources. It works with licenses from vendors like Microsoft, Oracle, and SAP. When a company has license agreements that limit how many instances can run specific software, License Manager enforces those limits and prevents accidental overage that could trigger costly audit findings.

AWS Service Catalog allows organizations to create a curated catalog of approved IT services that employees can deploy themselves. Instead of letting every user deploy any AWS resource in any configuration, Service Catalog defines standardized products with approved configurations, governance controls, and version management. An employee can deploy a pre-approved database configuration without needing to understand all the underlying AWS settings. This enforces organizational standards while giving teams self-service access. When the exam describes centralized governance with self-service deployment of approved configurations, Service Catalog is the answer.

The AWS Well-Architected Tool is a free service that guides you through assessing your workload architecture against the six pillars of the Well-Architected Framework. You answer questions about your architecture and the tool identifies areas of risk and provides improvement recommendations. Organizations can use it periodically to review workloads and track improvement over time.

The AWS Personal Health Dashboard provides a personalized view of the health of AWS services as they affect your specific account and resources. It is different from the AWS Service Health Dashboard, which shows the global status of all AWS services for all customers. The Personal Health Dashboard filters to show only events that are relevant to the resources in your account and provides guidance on what actions to take. The exam tests this distinction directly: the Service Health Dashboard is for everyone, while the Personal Health Dashboard is specific to your account.

AWS Compute Optimizer analyzes the actual utilization of your EC2 instances, EBS volumes, Lambda functions, and Auto Scaling groups using machine learning and recommends the optimal resource type and size. It identifies instances that are over-provisioned and wasting money, as well as instances that are under-provisioned and potentially causing performance problems. Compute Optimizer is free to use.

---

## Migration and Transfer Services

These services assist with planning and executing migrations from on-premises to AWS.

AWS Application Discovery Service collects information about on-premises servers before migration. It can operate in agentless mode by integrating with virtualization platforms like VMware, or in agent mode by installing a lightweight agent on each server. It collects data about server specifications, utilization, and dependencies between applications, which is used to plan the migration. The collected data exports to Migration Hub for analysis.

AWS Migration Hub provides a central place to track the progress of application migrations across multiple migration tools and AWS services. Rather than checking the status of each migration in a different tool, Migration Hub aggregates the status into a single dashboard. There is no additional charge for Migration Hub.

AWS Server Migration Service, or SMS, migrates on-premises servers to AWS with incremental replication to minimize downtime. It supports VMware, Hyper-V, and Azure environments. You should know that SMS is being replaced by the AWS Application Migration Service, which is the recommended path for lift-and-shift server migrations going forward.

AWS DataSync transfers data between on-premises storage and AWS storage services like S3, EFS, and FSx. It is up to ten times faster than standard open-source transfer tools because it uses a purpose-built network protocol with parallelization and compression. DataSync validates data integrity during transfer and can be scheduled for recurring transfers or used for a one-time migration. It supports NFS and SMB protocols on the source side and charges per gigabyte transferred.

AWS Transfer Family provides a fully managed service for transferring files using SFTP, FTPS, and FTP protocols. Organizations that have existing workflows built around these protocols can migrate file transfer operations to AWS without changing the client-side tools or user workflows. Files are stored in S3 or EFS on the AWS side. You pay per protocol enabled plus per gigabyte of data transferred.

---

## Advanced Networking Services

AWS Global Accelerator improves the availability and performance of your applications for users around the world. It provides two static anycast IP addresses that serve as fixed entry points to your application globally. Traffic enters the AWS global network at the edge location closest to the user, then travels over AWS's internal fiber network to the application endpoint, which is faster and more reliable than routing over the public internet. Global Accelerator supports automatic failover and health checks.

The exam sometimes asks about the difference between Global Accelerator and CloudFront. Both improve performance for global users, but they work differently. CloudFront is a content delivery network that caches content at edge locations so that repeated requests are served locally without going to the origin. Global Accelerator does not cache content — it routes all requests over the AWS global network to the application. CloudFront is optimal for cacheable content like images, videos, and static web assets. Global Accelerator is optimal for dynamic content, applications that require static IP addresses, and non-HTTP applications like gaming, IoT, or Voice over IP.

AWS Cloud Map is a service discovery service. In microservices architectures where many services need to find and communicate with each other, Cloud Map maintains a registry of service endpoints with health checking. Services register themselves in Cloud Map and discover other services through DNS or API queries. It integrates with ECS and EKS.

---

## Additional Storage Services

Amazon FSx provides fully managed file systems for specific use cases that go beyond what S3 and EFS cover.

FSx for Windows File Server provides a native Windows file system with Server Message Block protocol support and full Active Directory integration. It is designed for Windows applications that require a Windows-compatible file share, such as applications that depend on Windows file system features, home directories for Windows users, or content management systems built on Windows. When the exam describes Windows applications needing shared file storage with Active Directory integration, FSx for Windows File Server is the answer rather than EFS, which does not support SMB or Active Directory natively.

FSx for Lustre provides a high-performance parallel file system designed for compute-intensive workloads like machine learning training, high-performance computing simulations, and video processing. It can process massive datasets at sub-millisecond latency and integrates directly with S3 so that data stored in S3 can be presented as a Lustre file system to compute jobs.

AWS Backup is a centralized service for managing and automating backups across AWS services. Instead of configuring backup policies separately for each service, AWS Backup provides a single place to define backup schedules, retention periods, and lifecycle rules that apply across EC2, RDS, DynamoDB, EFS, FSx, and other services. It also provides compliance reporting to demonstrate that backup policies are being followed. You pay for the storage used by your backups.

---

## Blockchain and Quantum Computing

Amazon Managed Blockchain creates and manages blockchain networks without requiring you to set up and manage the underlying infrastructure. It supports Hyperledger Fabric for private permissioned blockchain networks and Ethereum for public blockchain participation. Use cases include supply chain tracking, financial transactions requiring shared record-keeping, and multi-party business processes where no single party should control the record. The network scales automatically as participants join.

Amazon Braket is a quantum computing service that provides access to quantum hardware from different manufacturers as well as quantum circuit simulators. It is primarily a research and experimentation service for organizations exploring quantum algorithms. You pay for simulation time and for tasks run on quantum hardware. The exam is unlikely to test quantum computing in depth, but you should be able to recognize Braket as the AWS service for quantum computing.

---

## Selecting the Right Service: Decision Frameworks

Let me give you a way to think about service selection that mirrors how exam questions are structured. Exam questions describe a scenario and ask you to pick the right service. The key is to identify the distinguishing characteristic in the scenario and match it to the right service.

For developer tools, the question usually focuses on one phase of the delivery pipeline. Source control in AWS is CodeCommit. Compiling and testing code is CodeBuild. Deploying to servers is CodeDeploy. Orchestrating all of it end to end is CodePipeline.

For end user computing, the key distinction is whether the user needs a full desktop or just a specific application. Full persistent desktop is WorkSpaces. Individual application streaming is AppStream 2.0.

For IoT, the distinction is where processing happens. Cloud-based processing of device data is IoT Core. Local edge processing on the device itself is IoT Greengrass.

For AI and ML services, the trigger words in the question tell you the service. Text to speech is Polly. Speech to text is Transcribe. Language translation is Translate. Structured data extraction from scanned documents is Textract. Intelligent enterprise search with natural language is Kendra. Product recommendations is Personalize. Time-series forecasting is Forecast.

For migration services, the question focus tells you the service. Discovering what exists on-premises before migrating is Application Discovery Service. Tracking migration progress across multiple tools is Migration Hub. Transferring large amounts of data from on-premises to S3 or EFS quickly is DataSync. Moving files using existing SFTP or FTP workflows is Transfer Family.

For business applications, email to end users at scale is SES. A full-blown contact center for customer calls is Amazon Connect. Corporate email hosted on AWS is WorkMail. Multi-channel marketing campaigns with SMS and push notifications is Pinpoint.

For governance services, software license compliance is License Manager. Self-service deployment of approved infrastructure templates is Service Catalog. Right-sizing resource recommendations is Compute Optimizer.

---

## Review Questions

Let us go through review questions for this lesson. I will ask each question, give you a moment to think, and then walk through the answer.

Question One. A software development team wants to store their application code in a private, secure repository hosted entirely within AWS, with integration with IAM for access control and encryption at rest. Which service should they use?

The answer is AWS CodeCommit. CodeCommit is AWS's fully managed Git-based source control service. It is encrypted at rest and in transit, integrates with IAM, and has no size limits. The key phrase here is "hosted within AWS" — GitHub and other third-party services are outside AWS. When the requirement includes storing source code inside the AWS environment itself, CodeCommit is the answer.

Question Two. A company runs nightly build processes that compile code and run automated tests. They want to eliminate the build servers they currently manage and pay only for the time builds are actually running. Which AWS service replaces their build servers?

The answer is AWS CodeBuild. CodeBuild is a fully managed build service that scales automatically and charges only for the compute time consumed during the build. There are no servers to provision or maintain. You define the build environment and commands, and CodeBuild handles the infrastructure.

Question Three. A company uses CodePipeline to deploy applications. After a recent deployment, a bug was discovered in production. They want future deployments to automatically revert if a CloudWatch alarm triggers within ten minutes of deployment. Which service should they configure for this rollback capability?

The answer is AWS CodeDeploy. CodeDeploy supports automatic rollback based on CloudWatch alarm conditions. When you configure a deployment group with rollback rules, CodeDeploy monitors the specified alarms after deployment and automatically reverses the deployment if alarms are triggered. This is one of CodeDeploy's key exam-relevant features.

Question Four. A company is migrating an on-premises application that currently uses a RabbitMQ message broker. The team wants to move the application to AWS without rewriting the messaging layer, keeping the same protocols and APIs that existing client applications use. Which AWS service should they use?

The answer is Amazon MQ. Amazon MQ is a managed message broker that supports RabbitMQ and Apache ActiveMQ, implementing the same standard protocols those brokers use including AMQP, MQTT, and STOMP. It is specifically designed for the scenario of migrating existing applications that use standard message broker protocols — not for new applications being built for the cloud, where SQS and SNS would be more appropriate.

Question Five. A financial services company needs to provide a developer team with a cloud-based development environment without requiring any software installation on local machines. Developers should be able to write, run, and debug code using only a browser, with the AWS CLI already available. Which service meets this requirement?

The answer is AWS Cloud9. Cloud9 is a browser-based integrated development environment that includes a terminal with the AWS CLI pre-installed. Developers can write, run, and debug code in the browser with no local installation required. The underlying compute runs on EC2, which the developer pays for.

Question Six. A manufacturing company deploys thousands of industrial sensors that monitor equipment. When an anomaly is detected, the sensor must trigger an alert and adjust equipment settings in under one hundred milliseconds — too fast to round-trip to the cloud. Some sensors operate in locations without reliable internet. Which IoT service enables local processing?

The answer is AWS IoT Greengrass. Greengrass extends AWS Lambda and other compute capabilities to run directly on IoT devices at the edge. This enables real-time local processing that does not depend on cloud connectivity, satisfying both the latency requirement and the offline operation requirement. IoT Core would require every event to round-trip to the cloud, which is too slow for this use case.

Question Seven. A company collects data from thousands of connected temperature sensors in their supply chain. They want to run SQL queries against the historical sensor data, identify anomalies, and visualize the results in QuickSight dashboards. Which service processes and prepares the IoT data for these queries?

The answer is AWS IoT Analytics. IoT Analytics is designed specifically for processing and analyzing time-series data from IoT devices. It supports SQL queries on the data, pre-built analytics templates, machine learning integration, and direct integration with QuickSight for visualization.

Question Eight. A media company allows users to upload videos in many different formats. They need to transcode uploaded videos into multiple resolutions for delivery to mobile devices, tablets, and smart TVs. The operation should be simple with pre-configured presets for common device types. Which service handles this?

The answer is Amazon Elastic Transcoder. Elastic Transcoder is designed for converting media files between formats with pre-configured presets for common output targets. If the scenario described broadcast-grade or professional video production requirements, MediaConvert would be the better answer — but for a simple consumer upload and transcode use case with pre-built presets, Elastic Transcoder fits.

Question Nine. An e-learning platform wants to add audio narration to its text-based lessons automatically, without recording human narrators. The narration should sound natural and support multiple languages. Which service provides this capability?

The answer is Amazon Polly. Polly converts text to lifelike speech using neural text-to-speech synthesis. It supports over sixty voices and thirty or more languages, and the neural TTS mode produces highly natural-sounding output. This is a core Polly use case — e-learning narration is specifically mentioned as a primary use case for the service.

Question Ten. A legal firm records all client intake calls and needs to automatically convert recordings to text so that attorneys can search call transcripts. The system must also identify which speaker said what in each recording. Which service should they use?

The answer is Amazon Transcribe. Transcribe converts recorded audio to text and supports speaker identification — the ability to label which speaker said which portions of the transcript. It processes batch audio files and real-time streams, and it supports custom vocabulary for domain-specific legal terminology.

Question Eleven. A global news organization publishes articles in English and wants to automatically make them available in Spanish, French, German, and Japanese without human translators. Which service provides this capability at API scale?

The answer is Amazon Translate. Translate performs neural machine translation across over seventy-five languages via a simple API call. Custom terminology can ensure that proper nouns, brand names, and technical terms are handled consistently across all translations.

Question Twelve. A healthcare insurance company receives millions of paper claim forms that have been scanned to PDF. They need to automatically extract the patient name, date of service, diagnosis code, and billed amount from each form, preserving the relationship between field labels and values. Which service handles this?

The answer is Amazon Textract. Textract goes beyond basic optical character recognition to extract structured data from forms and documents, preserving the relationships between labels and values. For an insurance claim form where the structure matters — knowing that a particular value is the diagnosis code rather than just an unstructured piece of text — Textract is the appropriate service.

Question Thirteen. A large retail company wants to increase online sales by showing each customer personalized product recommendations based on their browsing and purchase history, similar to how Amazon.com works. No machine learning team is available to build a custom model. Which service should they use?

The answer is Amazon Personalize. Personalize provides a managed recommendation engine that uses the same underlying technology as Amazon.com recommendations. You provide user interaction data and a product catalog, and Personalize generates real-time personalized recommendations. No machine learning expertise is required.

Question Fourteen. A technology company wants to search across its internal knowledge base, engineering documentation, and support wikis using natural language. When an employee asks "what is the process for requesting a security exception?" the system should return the specific answer, not just a list of documents containing those words. Which service provides this?

The answer is Amazon Kendra. Kendra is an intelligent enterprise search service that uses natural language understanding to return precise answers rather than just matching keyword occurrences. It connects to document repositories including S3, SharePoint, and databases. Traditional keyword search would return every document containing the words "security exception" — Kendra would find and return the specific answer.

Question Fifteen. A retail chain wants to predict how much of each product to stock at each store location for the next three months based on years of historical sales data, seasonal patterns, and promotional schedules. No data science team is available. Which service provides this forecast without requiring machine learning expertise?

The answer is Amazon Forecast. Forecast is a time-series forecasting service that applies machine learning to historical data to generate predictions. You provide the historical data — in this case, sales history — along with any additional variables like promotions, and Forecast handles the model training and generates predictions. This is demand forecasting, which is Forecast's primary use case.

Question Sixteen. A software company is launching a cloud-based customer support center that needs to handle inbound customer calls, route them to the appropriate agent, and provide AI-powered suggested responses during calls. They want to pay per minute of usage rather than per agent seat. Which service should they use?

The answer is Amazon Connect. Connect is a cloud-based contact center service that handles inbound and outbound calls, supports omnichannel interactions, and includes AI features for routing and real-time assistance. Its usage-based pricing model, where you pay per minute of telephony rather than per agent per month, is a key distinguishing characteristic that the exam sometimes tests.

Question Seventeen. A company's application sends hundreds of thousands of transactional emails per day — order confirmations, shipping notifications, and password reset emails. They want a cost-effective AWS service to handle this email delivery with high deliverability rates. Which service should they use?

The answer is Amazon SES. SES is designed exactly for high-volume transactional email — the types of emails you send from applications, not from people. It handles deliverability, bounce management, and email validation. It is significantly less expensive than general communication platforms for pure email volume. Remember the distinction: SES is for formatted emails to people, SNS is for notifications to application subscribers.

Question Eighteen. A company wants to migrate their corporate email from a self-managed Exchange server to a fully managed AWS service. Employees should continue to use their existing email clients, and the service must integrate with their Active Directory for authentication. Which service should they use?

The answer is AWS WorkMail. WorkMail is a fully managed business email and calendar service that supports standard email protocols, integrates with Active Directory, and works with existing email clients including Outlook. It eliminates the overhead of managing Exchange servers while keeping the familiar email experience for users.

Question Nineteen. A company provides contractors with temporary access to internal systems. They do not want contractor laptops storing corporate data, and they need to revoke access immediately when a contract ends. Which end user computing service best meets this requirement?

The answer is Amazon WorkSpaces. WorkSpaces provides virtual desktops hosted on AWS. Corporate data stays in the cloud — it never resides on the contractor's device. When a contract ends, the administrator deletes or disables the WorkSpace, immediately revoking access. The contractor's personal laptop cannot access anything after that point.

Question Twenty. A legal technology company wants to distribute specialized legal research software to thousands of law firms without having those firms install anything on their computers. Lawyers should access the application directly from a web browser. Which service enables this?

The answer is Amazon AppStream 2.0. AppStream streams individual desktop applications to a web browser without requiring any local installation. The application runs on AWS infrastructure, and only the visual output is transmitted to the user's browser. This is ideal for distributing specialized software broadly without managing client-side installations.

Question Twenty-One. A company with a large number of EC2 instances suspects they are over-provisioned and paying for more compute than they use. They want AWS to automatically analyze their actual utilization patterns and recommend right-sized instance types. Which service provides these recommendations?

The answer is AWS Compute Optimizer. Compute Optimizer uses machine learning to analyze CloudWatch utilization data for EC2 instances, EBS volumes, Lambda functions, and Auto Scaling groups and recommends the optimal resource configuration. It identifies both over-provisioned resources wasting money and under-provisioned resources that may cause performance issues. It is free to use.

Question Twenty-Two. A software company has enterprise license agreements for Microsoft SQL Server that allow a limited number of processor sockets. They want to prevent their team from accidentally launching EC2 instances that would violate the license terms. Which service enforces these limits?

The answer is AWS License Manager. License Manager lets you define license rules based on the terms of your agreements and enforces those rules when instances are launched. It can prevent new EC2 instances from being started if launching them would exceed your licensed processor socket count, protecting against costly license violations.

Question Twenty-Three. An enterprise wants to allow application teams to provision their own infrastructure but only from a pre-approved set of configurations — specific instance types, approved database configurations, and compliant network settings — without requiring those teams to understand all the underlying AWS details. Which service provides this self-service capability with governance?

The answer is AWS Service Catalog. Service Catalog lets administrators define and publish approved IT service configurations as products. End users can then deploy those approved configurations through a self-service portal without needing deep AWS expertise. Governance is maintained because users can only deploy what administrators have approved.

Question Twenty-Four. A company is notified that an AWS service in their Region is experiencing degraded performance. They want to know whether this service degradation is affecting their specific resources and what steps they should take. Which tool provides this personalized information?

The answer is the AWS Personal Health Dashboard. The Personal Health Dashboard filters AWS service health events to show only those affecting your specific account and resources. It also provides remediation guidance. This is distinct from the AWS Service Health Dashboard, which shows the global status of all services for all customers without filtering to your specific account.

Question Twenty-Five. A company needs to understand which servers are currently running in their on-premises data center, how those servers are utilized, and which applications depend on each other, before planning a migration to AWS. Which service collects this information?

The answer is AWS Application Discovery Service. Application Discovery Service can run in agentless mode using VMware vCenter integration or in agent mode with a lightweight agent on each server. It collects server specifications, utilization metrics, and application dependency maps — exactly the information needed to plan an effective migration.

Question Twenty-Six. A company is running a large migration project involving twenty-five applications moving to AWS simultaneously, using a combination of Database Migration Service, Server Migration Service, and manual re-platforming. The project manager needs a single view of which applications have been migrated, which are in progress, and which have not started. Which service provides this view?

The answer is AWS Migration Hub. Migration Hub aggregates migration status from multiple tools and services into a single dashboard. The project manager can see the overall state of the migration portfolio without having to check each individual migration service. Migration Hub has no additional cost.

Question Twenty-Seven. A company wants to transfer one hundred terabytes of backup files from their on-premises NAS to Amazon S3. Their internet connection is shared with production workloads, so they need the transfer to be as fast as possible, use optimized network protocols, and validate data integrity during transfer. Which service should they use?

The answer is AWS DataSync. DataSync is purpose-built for online data transfer from on-premises to AWS storage. It uses an optimized network protocol with parallelization that is up to ten times faster than standard transfer tools, and it validates data integrity during and after transfer. It supports NFS and SMB protocols for reading from on-premises storage and delivers to S3, EFS, or FSx.

Question Twenty-Eight. A company uses an existing file transfer workflow where business partners upload files to their on-premises SFTP server. They want to move the SFTP server to AWS without requiring partners to change their SFTP client configurations. Which service provides managed SFTP hosting on AWS?

The answer is AWS Transfer Family. Transfer Family provides fully managed SFTP, FTPS, and FTP endpoints that store transferred files directly in S3 or EFS. Business partners continue using their existing SFTP clients and workflows — they connect to the Transfer Family endpoint instead of the on-premises server, but the user-facing experience is identical.

Question Twenty-Nine. A gaming company has a global player base. Game clients connect to a central game server and require consistent low latency. The game must also support static IP addresses for firewall whitelisting by enterprise customers. Which service improves global routing performance and provides static IPs?

The answer is AWS Global Accelerator. Global Accelerator provides two static anycast IP addresses and routes traffic from the nearest AWS edge location to the application over the AWS global backbone network rather than the public internet. This reduces latency and jitter for latency-sensitive applications like gaming. The static IP addresses satisfy the enterprise firewall whitelisting requirement. CloudFront would not be appropriate here because game traffic is dynamic and non-cacheable.

Question Thirty. A company needs to centrally back up data across their EC2 instances, RDS databases, DynamoDB tables, and EFS file systems. They want a single location to define retention policies and demonstrate compliance to auditors. Which service provides centralized backup management across all these services?

The answer is AWS Backup. Backup provides a centralized console to define backup plans — schedules, retention periods, and lifecycle rules — that apply across multiple AWS services simultaneously. It generates compliance reports that demonstrate to auditors that backup policies are being followed. Without AWS Backup, each service would need to be configured separately, and demonstrating comprehensive compliance would be much more complex.

---

## Putting It All Together: Patterns for the Exam

Let me close by connecting these additional services back to the patterns you will see on the exam. The exam gives you a scenario and you need to identify the right service. Here is how to read those scenarios.

When a question mentions source control inside AWS, the answer is CodeCommit. When it mentions automating the build and test step, it is CodeBuild. When it mentions automating deployment with rollback, it is CodeDeploy. When it mentions the full pipeline from commit to production, it is CodePipeline.

When a question mentions migrating an existing RabbitMQ or ActiveMQ application without changing the messaging layer, the answer is Amazon MQ, not SQS or SNS.

When a question mentions a full virtual desktop for remote workers or contractors, the answer is WorkSpaces. When it mentions streaming a specific application to a browser, the answer is AppStream 2.0.

For IoT: processing in the cloud is IoT Core. Processing at the edge or offline is IoT Greengrass. Analyzing historical IoT data is IoT Analytics.

For media: simple format conversion with presets is Elastic Transcoder. Broadcast-grade professional transcoding is MediaConvert. Real-time video capture and storage is Kinesis Video Streams.

For AI services, go by function. Polly speaks. Transcribe listens. Translate switches languages. Textract reads documents. Kendra finds answers in enterprise content. Personalize recommends products. Forecast predicts quantities.

For business applications: transactional email to people is SES. A cloud contact center for customer calls is Connect. Corporate email on AWS is WorkMail. Multi-channel marketing campaigns are Pinpoint.

For governance: license compliance is License Manager. Self-service approved deployments are Service Catalog. Right-sizing recommendations are Compute Optimizer. Your account-specific service health is the Personal Health Dashboard.

For migration: discovering on-premises assets is Application Discovery Service. Tracking migration progress is Migration Hub. Fast online data transfer is DataSync. Managed SFTP endpoints are Transfer Family.

For global performance: Global Accelerator for non-cacheable traffic that needs static IPs and consistent routing. CloudFront for cacheable content served from edge locations.

These services appear less frequently than EC2, S3, IAM, RDS, and the other core services, but when they appear, recognizing them quickly and knowing their primary use case is the difference between a confident answer and an uncertain guess.

---
