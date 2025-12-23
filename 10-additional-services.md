# Chapter 10: Additional AWS Services and Advanced Topics

[← Previous: Chapter 9](09-management-governance.md) | [Next: Appendix →](appendix.md)

---

## Developer Tools and CI/CD

### AWS CodeCommit
- Git-based source control repository
- Secure, highly available
- No size limits on repositories
- Integrates with existing Git tools
- Encrypted at rest and in transit
- Free tier: 5 active users per month

### AWS CodeBuild
- Fully managed build service
- Compiles source code, runs tests, produces packages
- Scales automatically
- Pay only for build time
- Pre-configured environments or custom Docker images
- Integrates with CodeCommit, GitHub, Bitbucket

### AWS CodeDeploy
- Automated deployment service
- Deploy to EC2, Lambda, on-premises servers
- Deployment strategies: In-place, blue/green
- Automatic rollback on failure
- Integration with existing CI/CD tools
- No additional charge (pay for resources)

### AWS CodePipeline
- Continuous delivery service
- Automates release pipeline
- Integrates with CodeCommit, CodeBuild, CodeDeploy
- Third-party integrations (GitHub, Jenkins)
- Visual workflow builder
- $1 per active pipeline per month

### AWS CodeStar
- Unified user interface for development activities
- Quickly develop, build, and deploy applications
- Project templates for various languages and platforms
- Integrated dashboard for monitoring
- Team collaboration features
- No additional charge

### AWS Cloud9
- Cloud-based IDE (Integrated Development Environment)
- Write, run, and debug code in browser
- Supports 40+ programming languages
- Built-in terminal with AWS CLI
- Collaborative coding features
- Pay only for underlying EC2 instance

---

## Application Integration Services

### Amazon EventBridge
- Serverless event bus service
- Connect applications using events
- Formerly CloudWatch Events
- Event sources: AWS services, custom applications, SaaS apps
- Event patterns for filtering
- Multiple targets per rule
- Pay per event

### Amazon MQ
- Managed message broker service
- Supports Apache ActiveMQ and RabbitMQ
- For migrating existing applications using message brokers
- Alternative to SNS/SQS for specific protocols
- Industry-standard APIs and protocols (MQTT, AMQP, STOMP)
- Single-instance or active/standby deployment

### AWS App Mesh
- Service mesh for microservices
- Monitor and control communications
- Works with ECS, EKS, EC2
- Provides observability, traffic management
- Based on Envoy proxy
- No additional charge (pay for resources)

---

## End User Computing

### Amazon WorkSpaces
- Managed Desktop-as-a-Service (DaaS)
- Virtual Windows or Linux desktops
- Access from any device
- Persistent desktop storage
- Integrated with Active Directory
- Pricing: Monthly or hourly
- Use cases: Remote work, contractors, BYOD

### Amazon AppStream 2.0
- Application streaming service
- Stream desktop applications to browser
- No need to install applications locally
- Scales automatically
- Pay as you go
- Use cases: Training, POC, software trials

### Amazon WorkDocs
- Secure document storage and collaboration
- Similar to Dropbox or Google Drive
- 1 TB storage per user
- File comments and feedback
- Active Directory integration
- Mobile and desktop apps

### Amazon WorkLink
- Secure mobile access to internal websites
- No VPN required
- Renders content in browser on AWS
- Only pixels sent to device
- Protects corporate network
- Per user per month pricing

---

## IoT Services

### AWS IoT Core
- Connect IoT devices to cloud
- Supports billions of devices
- MQTT, HTTPS, WebSockets protocols
- Device shadow for state management
- Rules engine for data processing
- Integration with other AWS services

### AWS IoT Greengrass
- Extend AWS to edge devices
- Local compute, messaging, and data caching
- Run Lambda functions at the edge
- Operate offline
- Secure communication with cloud
- ML inference at edge

### AWS IoT Analytics
- Analytics for IoT data
- Process and analyze IoT data
- Pre-built analytics templates
- Integration with QuickSight
- SQL queries on time-series data
- Machine learning integration

---

## Media Services

### Amazon Elastic Transcoder
- Convert media files between formats
- Scalable media transcoding
- Pre-configured presets
- Pay per minute of transcoding
- Integration with S3, CloudFront

### AWS Elemental MediaConvert
- File-based video transcoding
- Broadcast-grade features
- Supports various formats and codecs
- On-demand or reserved pricing
- More advanced than Elastic Transcoder

### Amazon Kinesis Video Streams
- Capture, process, and store video streams
- Millions of devices
- Playback, analytics, ML integration
- Use cases: Security cameras, live streaming
- Pay for data ingested and consumed

---

## Additional AI/ML Services

### Amazon Polly
- Text-to-speech service
- Natural sounding speech
- Multiple languages and voices
- Neural TTS for more natural sound
- Pay per character
- Use cases: E-learning, accessibility

### Amazon Transcribe
- Automatic speech recognition (ASR)
- Convert speech to text
- Real-time and batch processing
- Speaker identification
- Custom vocabulary
- Pay per second of audio

### Amazon Translate
- Neural machine translation
- Translate text between languages
- 75+ languages supported
- Custom terminology
- Pay per character
- Use cases: Localization, content creation

### Amazon Forecast
- Time-series forecasting service
- Uses machine learning
- No ML expertise required
- More accurate than traditional methods
- Use cases: Demand forecasting, resource planning

### Amazon Kendra
- Intelligent search service
- ML-powered enterprise search
- Natural language queries
- Learns from user interactions
- Connects to various data sources

### Amazon Personalize
- Real-time recommendations
- Same technology as Amazon.com
- No ML expertise required
- Real-time and batch recommendations
- Use cases: Product recommendations, personalized content

### Amazon Textract
- Extract text and data from documents
- OCR and form recognition
- Preserves structure and relationships
- Works with PDFs, images
- Pay per page

---

## Business Applications

### Amazon Connect
- Cloud-based contact center
- Omnichannel customer service
- AI-powered chatbots
- Pay as you go
- Integration with other AWS services
- Use cases: Customer support, helpdesk

### Amazon Simple Email Service (SES)
- Email sending and receiving
- Transactional and marketing emails
- High deliverability
- Pay per email sent
- Free tier: 62,000 emails/month (from EC2)
- Email validation and filtering

### Amazon Pinpoint
- Marketing communication service
- Email, SMS, push notifications
- Customer segmentation
- Campaign management
- Analytics and engagement metrics
- Pay for messages sent

### AWS WorkMail
- Managed email and calendar service
- Alternative to Exchange/Gmail
- Web-based access
- Mobile and desktop clients
- Active Directory integration
- $4 per user per month

### Amazon Chime
- Video conferencing and communication
- Meetings, chat, business calling
- Screen sharing
- Per user per day pricing
- Alternative to Zoom, Teams

---

## Advanced Management and Governance

### AWS License Manager
- Manage software licenses
- Track license usage
- Set usage limits
- Prevent license violations
- Works with Microsoft, Oracle, SAP, etc.

### AWS Service Catalog
- Create and manage catalogs of IT services
- Standardized products
- Control which services users can deploy
- Version control for products
- Governance and compliance
- Self-service portal for end users

### AWS Well-Architected Tool
- Review workload architecture
- Compare against best practices
- Six pillars assessment
- Get improvement plan
- Free service
- Periodic reviews recommended

### AWS Personal Health Dashboard
- Personalized view of AWS service health
- Alerts for service events affecting you
- Proactive notifications
- Remediation guidance
- Available to all customers
- Different from Service Health Dashboard (global status)

### AWS Compute Optimizer
- Recommends optimal AWS resources
- ML-based recommendations
- Analyzes historical utilization
- Suggests EC2 instance types, EBS volumes, Lambda memory
- Cost savings opportunities
- No additional charge

---

## Migration and Transfer Services

### AWS Application Discovery Service
- Discover on-premises applications
- Plan migrations
- Collect server utilization and dependency data
- Agentless or agent-based discovery
- Export data for analysis
- Integration with Migration Hub

### AWS Migration Hub
- Track application migrations
- Single location to monitor migrations
- Works with migration tools
- Visualize migration progress
- No additional cost

### AWS Server Migration Service (SMS)
- Migrate on-premises servers to AWS
- Incremental replication
- Minimal downtime
- Supports VMware, Hyper-V, Azure
- No additional charge
- Being replaced by Application Migration Service

### AWS DataSync
- Transfer data between on-premises and AWS
- Up to 10x faster than open-source tools
- Automated data transfer
- Supports NFS, SMB protocols
- Transfers to S3, EFS, FSx
- Pay per GB transferred

### AWS Transfer Family
- Fully managed SFTP, FTPS, FTP
- Transfer files to/from S3 or EFS
- No infrastructure to manage
- Integration with existing auth systems
- Pay per protocol enabled + data transfer

---

## Advanced Networking Services

### AWS Global Accelerator
- Improve availability and performance
- Uses AWS global network
- Static anycast IP addresses
- Automatic failover
- Health checks
- Use cases: Gaming, IoT, VoIP
- Different from CloudFront (not caching)

### AWS App Mesh
- Service mesh for microservices
- Monitor and control communications
- Works with ECS, EKS, EC2
- Based on Envoy proxy
- Traffic management, observability

### AWS Cloud Map
- Service discovery
- Register application resources
- Discover services via API or DNS
- Health checking
- Integration with ECS, EKS

---

## Additional Storage Services

### Amazon FSx
Fully managed file systems with two main options:

**FSx for Windows File Server**:
- Windows native file system
- SMB protocol
- Active Directory integration
- For Windows applications

**FSx for Lustre**:
- High-performance file system
- ML, HPC, video processing
- Integration with S3
- Sub-millisecond latencies

### AWS Backup
- Centralized backup service
- Automate and manage backups
- Backup across AWS services
- Backup policies and retention
- Compliance reporting
- Pay for storage used

---

## Blockchain and Quantum Computing

### Amazon Managed Blockchain
- Create and manage blockchain networks
- Supports Hyperledger Fabric and Ethereum
- Fully managed
- Scales automatically
- Use cases: Supply chain, finance

### Amazon Braket
- Quantum computing service
- Develop quantum algorithms
- Test on quantum simulators
- Run on quantum hardware
- Pay for simulation and quantum tasks

---

## Exam Tips for Service Selection

### Database Selection Decision Tree

**Choose Database Based on Requirements**:

#### Need SQL and ACID transactions?
- **Traditional app, existing database** → **RDS**
- **Need extreme performance** → **Aurora**
- **Specific needs: Oracle/SQL Server** → **RDS** with that engine

#### Need NoSQL?
- **Key-value, scale to millions of requests/sec** → **DynamoDB**
- **Document database, MongoDB compatible** → **DocumentDB**
- **Graph relationships** → **Neptune**

#### Need caching?
- **In-memory cache** → **ElastiCache**
- **Redis features needed** → ElastiCache for Redis
- **Simple caching** → ElastiCache for Memcached

#### Need data warehouse/analytics?
- **OLAP, BI, big data** → **Redshift**
- **Query data in S3** → **Athena**
- **Real-time analytics** → **Kinesis Data Analytics**

#### Time-series data?
- **IoT, metrics, logs** → **Timestream**

---

### Compute Selection Decision Tree

| Requirement | Best Choice | Why |
|-------------|-------------|-----|
| Full OS control | EC2 | Complete flexibility |
| Serverless, event-driven | Lambda | No servers, pay per use |
| Deploy app quickly | Elastic Beanstalk | PaaS, managed |
| Containers, Kubernetes | EKS | Kubernetes compatible |
| Containers, AWS native | ECS | Simpler than EKS |
| Containers, no servers | Fargate | Serverless containers |
| Batch processing | AWS Batch | Optimized for batch |
| Simple website, blog | Lightsail | Predictable pricing |

---

### Storage Selection Guide

| Use Case | Service | Reason |
|----------|---------|--------|
| Backups, archives | S3 Glacier | Lowest cost |
| Website content | S3 + CloudFront | Scalable, global |
| EC2 boot volume | EBS | Block storage |
| Shared file system | EFS | Multi-attach |
| Windows file shares | FSx for Windows | SMB, AD integration |
| HPC workloads | FSx for Lustre | High performance |
| Temporary data | Instance Store | Highest IOPS |
| Offline transfer | Snow Family | Large data volumes |

---

[← Previous: Chapter 9](09-management-governance.md) | [Next: Appendix →](appendix.md)
