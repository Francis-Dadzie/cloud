# AWS Certified Cloud Practitioner (CLF-C02)

---

## Table of Contents

- [Cloud Concepts](#cloud-concepts)
- [AWS Global Infrastructure](#aws-global-infrastructure)
- [Compute](#compute)
- [Networking](#networking)
- [Storage](#storage)
- [Databases](#databases)
- [Security and Compliance](#security-and-compliance)
- [Monitoring and Management](#monitoring-and-management)
- [Pricing and Billing](#pricing-and-billing)
- [Migration](#migration)
- [AI and Machine Learning](#ai-and-machine-learning)
- [Developer and Application Services](#developer-and-application-services)
- [Well-Architected Framework](#well-architected-framework)
- [Support Plans](#support-plans)

---

## Cloud Concepts

**Cloud computing:** on-demand delivery of IT resources over the internet with pay-as-you-go pricing. Instead of buying and maintaining physical infrastructure, you access compute, storage, and databases as needed.

### Deployment models

| Model | Description | Example |
|---|---|---|
| Public cloud | Resources owned and operated by a third-party provider | AWS, Azure, GCP |
| Private cloud | Resources used exclusively by one organisation | On-premises data centre |
| Hybrid cloud | Mix of public and private | AWS Outposts |

### Key characteristics

- **Scalability** — ability to handle increased load by adding resources. Scale up = more power to existing resource. Scale out = more machines.
- **Elasticity** — automatically scale resources up or down in real-time based on demand.
- **High availability** — systems designed to remain operational with minimal downtime, typically across multiple AZs.
- **Fault tolerance** — ability to continue operating despite component failures.
- **Agility** — quickly access new resources, reduce time to experiment and deploy.
- **Multi-tenancy** — multiple customers share underlying infrastructure while remaining isolated from each other.

### Benefits of cloud computing

- Trade capital expense (CapEx) for operational expense (OpEx)
- Benefit from massive economies of scale
- Stop guessing capacity
- Increase speed and agility
- Stop spending money maintaining data centres
- Go global in minutes

### Shared Responsibility Model

A critical exam concept — who is responsible for what:

| AWS responsibility (security **of** the cloud) | Customer responsibility (security **in** the cloud) |
|---|---|
| Physical data centres | IAM users, groups, roles and policies |
| Hardware and networking | Operating system patches (for EC2) |
| Hypervisor | Application-level security |
| Managed service infrastructure | Data encryption and classification |
| Global infrastructure | Network and firewall configuration |

> **Memory tip**: AWS owns everything below the hypervisor. You own everything above it.

---

## AWS Global Infrastructure

### Regions

A **Region** is a physical geographic area containing multiple data centres. AWS has 30+ regions globally.

Factors to consider when choosing a region:
- **Compliance** — data sovereignty and legal requirements
- **Proximity** — latency to your users
- **Feature availability** — not all services are available in every region
- **Pricing** — varies by region

### Availability Zones (AZs)

- Each region contains **2–6 AZs** (typically 3)
- Each AZ is one or more discrete data centres with redundant power, networking, and connectivity
- AZs within a region are connected via high-bandwidth, low-latency private links
- Deploying across multiple AZs protects against single data centre failure

### Edge Locations

- Used by **CloudFront** (CDN) and **Route 53**
- More numerous than Regions — 400+ points of presence globally
- Cache content closer to end users to reduce latency

### Key infrastructure terms

| Term | Description |
|---|---|
| Local Zone | Extension of a Region into a metro area for ultra-low latency |
| Wavelength Zone | Infrastructure embedded in telecom providers' 5G networks |
| AWS Outposts | AWS infrastructure delivered on-premises at your data centre |

---

## Compute

> In AWS, everything is an API call. You interact with services via the Management Console, CLI, or SDK.

### EC2 (Elastic Compute Cloud)

Virtual servers in the cloud. Compute refers to the processing power needed to run applications, manage data, and perform calculations.

**AMIs (Amazon Machine Images)** — templates for EC2 instances containing pre-configured OS, storage, software and settings. Launch multiple identical instances from one AMI.

#### EC2 Instance Families

| Family | Optimised for | Example use case |
|---|---|---|
| General purpose | Balance of compute, memory, network | Web servers, dev environments |
| Compute optimised | High-performance processors | Batch processing, gaming servers |
| Memory optimised | Large in-memory datasets | In-memory databases, real-time analytics |
| Accelerated computing | GPUs, FPGAs | ML inference, video encoding |
| Storage optimised | High sequential read/write | Data warehouses, distributed file systems |

> **Exam tip**: `t2.micro` and `t3.micro` are free-tier eligible general-purpose instances.

#### EC2 Pricing Options

| Option | How it works | Best for |
|---|---|---|
| On-demand | Pay per hour/second, no commitment | Unpredictable workloads, short-term |
| Reserved | 1 or 3 year commitment, up to 72% discount | Steady-state, predictable workloads |
| Spot | Bid on unused capacity, up to 90% discount, can be reclaimed with 2-min warning | Fault-tolerant, flexible timing |
| Savings Plans | Commit to consistent usage ($/hour) for 1 or 3 years | Flexible across instance types |
| Dedicated hosts | Reserve a physical server | Compliance, BYOL (bring your own licence) |
| Dedicated instances | Hardware dedicated to you, no physical server reservation | Isolation from other customers |

#### EC2 Auto Scaling

Automatically adds or removes EC2 instances based on demand. Set minimum, desired, and maximum capacity. Works with ELB to distribute traffic across scaled instances.

#### Elastic Load Balancing (ELB)

Distributes incoming traffic across multiple EC2 instances, AZs, or containers. Works hand-in-hand with Auto Scaling.

Types:
- **Application Load Balancer (ALB)** — HTTP/HTTPS, Layer 7, content-based routing
- **Network Load Balancer (NLB)** — TCP/UDP, Layer 4, ultra-high performance
- **Gateway Load Balancer** — for deploying third-party virtual appliances

### Serverless Compute

No infrastructure to manage — you focus on code.

| Service | Description |
|---|---|
| **Lambda** | Run code in response to events (FaaS). Ideal for event-driven applications. Supported runtimes: Python, Node.js, Java, Go, and others. |
| **Fargate** | Serverless compute engine for containers. Run ECS or EKS without managing EC2 instances. |

### Container Services

| Service | Description |
|---|---|
| **ECS** (Elastic Container Service) | AWS-native container orchestration |
| **EKS** (Elastic Kubernetes Service) | Managed Kubernetes |
| **ECR** (Elastic Container Registry) | Store and manage Docker images |

### Purpose-Built Compute Services

| Service | Use case |
|---|---|
| **Elastic Beanstalk** | Deploy and manage web apps with automated scaling — less admin than EC2 |
| **AWS Batch** | Large-scale parallel batch workloads (scientific computing, financial modelling) |
| **Amazon Lightsail** | Simple VPS — good for small websites, blogs, dev/test |
| **AWS Outposts** | AWS infrastructure on-premises for hybrid cloud or data residency requirements |

---

## Networking

### VPC (Virtual Private Cloud)

Your own isolated network within AWS. You control IP ranges, subnets, route tables, and gateways.

| Component | Description |
|---|---|
| **VPC** | Logically isolated section of AWS cloud. You define the IP range (CIDR block). |
| **Subnet** | Segment of a VPC's IP range. Public subnets have a route to the internet; private subnets do not. |
| **Internet Gateway** | Allows public traffic in/out of a VPC. Attach to a VPC and add a route to the route table. |
| **NAT Gateway** | Allows private subnet instances to initiate outbound internet traffic without being reachable from the internet. |
| **Route Table** | Set of rules that determine where network traffic is directed. |
| **Virtual Private Gateway** | Entry point for VPN connections from an on-premises network. |
| **Direct Connect** | Dedicated physical private connection from your data centre to AWS. More consistent than VPN. |

### Security Groups vs NACLs

| | Security Groups | Network ACLs |
|---|---|---|
| Operates at | Instance level | Subnet level |
| State | **Stateful** — return traffic automatically allowed | **Stateless** — must explicitly allow return traffic |
| Rules | Allow rules only | Allow and deny rules |
| Default | Deny all inbound, allow all outbound | Allow all inbound and outbound |
| Evaluation | All rules evaluated | Rules evaluated in number order |

> **Memory tip**: Security groups are stateful (smart), NACLs are stateless (strict).

### DNS

**Amazon Route 53** — AWS's DNS service. Routes users to applications globally. Supports health checks and failover routing.

### Content Delivery

**Amazon CloudFront** — CDN that delivers content from Edge Locations close to users. Reduces latency for static and dynamic content. Integrates with S3, EC2, and ALB.

---

## Storage

### Block Storage

| Service | Persistence | Notes |
|---|---|---|
| **EC2 Instance Store** | Non-persistent | Physically attached to host, lost when instance stops |
| **EBS (Elastic Block Store)** | Persistent | Attach to EC2 like a hard drive. Use Amazon Data Lifecycle Manager to automate snapshot creation, retention, and deletion. |

EBS volume types:
- **gp2/gp3** — general purpose SSD (most common)
- **io1/io2** — provisioned IOPS SSD (high performance, databases)
- **st1** — throughput optimised HDD (big data, logs)
- **sc1** — cold HDD (infrequent access, lowest cost)

### Object Storage

**Amazon S3 (Simple Storage Service)** — store and retrieve any amount of data. Objects stored in buckets. Unlimited storage. Objects up to 5TB.

#### S3 Storage Classes

| Class | Use case |
|---|---|
| S3 Standard | Frequently accessed data |
| S3 Intelligent-Tiering | Unknown or changing access patterns (auto-moves between tiers) |
| S3 Standard-IA | Infrequent access, rapid retrieval |
| S3 One Zone-IA | Infrequent access, single AZ, lower cost |
| S3 Glacier Instant Retrieval | Archive, millisecond retrieval |
| S3 Glacier Flexible Retrieval | Archive, minutes to hours retrieval |
| S3 Glacier Deep Archive | Long-term archive, 12 hour retrieval, lowest cost |

> **Exam tip**: Know the retrieval times. Glacier Deep Archive is the cheapest but slowest. Intelligent-Tiering is the answer when access patterns are unknown.

#### S3 Features worth knowing

- **Versioning** — keep multiple versions of an object
- **Lifecycle policies** — automatically transition objects between storage classes
- **Cross-region replication** — replicate objects to another region
- **Transfer Acceleration** — faster uploads via CloudFront edge locations
- **Static website hosting** — host a static site directly from an S3 bucket
- **Presigned URLs** — time-limited access to private objects

### File Storage

| Service | Description |
|---|---|
| **EFS (Elastic File System)** | Managed NFS. Attach to multiple EC2 instances simultaneously. Auto-scales. |
| **FSx** | Managed file systems — FSx for Windows File Server, FSx for Lustre (HPC) |

### Hybrid Storage

| Service | Description |
|---|---|
| **AWS Storage Gateway** | Connect on-premises environments to AWS cloud storage |
| **AWS Elastic Disaster Recovery** | Recover on-premises and cloud workloads into AWS |

---

## Databases

### Relational Databases

**Amazon RDS (Relational Database Service)** — managed relational database. Handles backups, patching, and failover. Supports MySQL, PostgreSQL, Oracle, SQL Server, MariaDB.

**Amazon Aurora** — AWS-native relational database. MySQL and PostgreSQL compatible. Replicates across 3 AZs automatically. Up to 5x faster than MySQL.

### NoSQL

**Amazon DynamoDB** — fully managed NoSQL key-value and document database. Single-digit millisecond performance at any scale. Serverless — no servers to manage.

### In-Memory

**Amazon ElastiCache** — managed in-memory caching. Supports Redis and Memcached. Used to reduce database load for read-heavy workloads.

### Other Database Services

| Service | Description |
|---|---|
| **Amazon Redshift** | Managed data warehouse for analytics at petabyte scale |
| **Amazon Neptune** | Managed graph database |
| **Amazon DocumentDB** | MongoDB-compatible document database |
| **Amazon QLDB** | Quantum Ledger Database — immutable, cryptographically verifiable transaction log |
| **Amazon Keyspaces** | Managed Apache Cassandra-compatible database |

> **Exam tip**: Know which database to choose for which use case. DynamoDB = NoSQL/key-value. Aurora = relational with high availability. Redshift = analytics/data warehouse. ElastiCache = caching layer.

---

## Security and Compliance

> AWS's default stance: **all actions are denied by default**. You explicitly grant permissions.

### IAM (Identity and Access Management)

| Concept | Description |
|---|---|
| **Users** | Individual identity with long-term credentials |
| **Groups** | Collection of users. Attach policies to groups, not users. |
| **Roles** | Temporary credentials, assumed by users, services, or applications |
| **Policies** | JSON documents defining allowed/denied actions on resources |

**IAM best practices**:
- Enable MFA for the root account — immediately
- Never use the root account for day-to-day work
- Apply least privilege — grant only what is needed
- Use roles for EC2 instances to access other AWS services
- Rotate access keys regularly

### Additional Security Services

| Service | Description |
|---|---|
| **AWS Organizations** | Centrally manage and govern multiple AWS accounts. Apply Service Control Policies (SCPs) across accounts. |
| **AWS Control Tower** | Enforce governance rules for security, operations, and compliance across your organization at scale |
| **AWS Shield** | DDoS protection. Standard (free, automatic) and Advanced (paid, 24/7 response team). |
| **AWS WAF** | Web Application Firewall. Filter malicious web traffic (SQL injection, XSS). |
| **Amazon GuardDuty** | Intelligent threat detection using ML. Analyses CloudTrail, VPC Flow Logs, DNS logs. |
| **Amazon Inspector** | Automated security assessments for EC2 instances and container images. |
| **Amazon Macie** | Uses ML to discover and protect sensitive data (PII) in S3. |
| **AWS Secrets Manager** | Store, rotate, and retrieve credentials, API keys, and other secrets. |
| **AWS KMS (Key Management Service)** | Create and manage encryption keys. Integrates with most AWS services. |
| **AWS Certificate Manager (ACM)** | Provision and manage SSL/TLS certificates. |
| **AWS Artifact** | Access AWS compliance reports and agreements (SOC, ISO, PCI). |
| **AWS Config** | Track resource configurations and compliance over time. |

### Compliance

**Security progression**: Securing → Monitoring → Auditing → Compliance

Key compliance frameworks AWS supports: SOC 1/2/3, ISO 27001, PCI DSS, HIPAA, GDPR.

---

## Monitoring and Management

| Service | Description |
|---|---|
| **Amazon CloudWatch** | Monitor AWS resources and applications. Metrics, logs, dashboards, alarms. |
| **AWS CloudTrail** | Every API call logged. Who did what, when, and from where. Enabled by default, 90-day history. |
| **AWS Config** | Continuous recording of resource configuration. Evaluate against compliance rules. |
| **AWS Systems Manager** | Operational hub — patch management, run commands, parameter store, session manager. |
| **AWS Trusted Advisor** | Analyses your account against best practices across five categories: cost, performance, security, fault tolerance, service limits. |
| **AWS Health Dashboard** | Personalised view of AWS service health affecting your resources. |
| **AWS X-Ray** | Distributed tracing — debug and analyse performance of distributed applications. |

### CloudWatch key concepts

- **Metrics** — time-series data points (CPU, network, disk)
- **Alarms** — trigger notifications or actions when a metric crosses a threshold
- **Logs** — collect, monitor, and store log files
- **Events / EventBridge** — respond to state changes in AWS resources

---

## Pricing and Billing

### Main cost drivers

- **Compute** — EC2 instance hours
- **Storage** — GB stored per month
- **Outbound data transfer** — data leaving AWS to the internet (inbound is free)

### Pricing models

See [EC2 Pricing Options](#ec2-pricing-options) above for compute. The general principle across AWS is pay-as-you-go with no upfront commitment required.

### Cost management tools

| Tool | Description |
|---|---|
| **AWS Pricing Calculator** | Estimate costs before deploying |
| **AWS Cost Explorer** | Visualise and analyse historical spending |
| **AWS Budgets** | Set cost or usage budgets and receive alerts |
| **AWS Cost and Usage Report** | Most detailed billing data, delivered to S3 |
| **AWS Savings Plans** | Flexible pricing model committing to consistent usage |

### Free Tier

Three types:
- **Always free** — e.g. Lambda 1M requests/month, DynamoDB 25GB storage
- **12 months free** — e.g. EC2 t2.micro 750 hours/month, S3 5GB
- **Trials** — short-term free trials of specific services

### Consolidated Billing

With AWS Organizations, use a single management account to consolidate billing across all member accounts. Benefits: one invoice, combined usage for volume discounts, shared Reserved Instance pricing.

---

## Migration

### Migration phases

| Phase | Description | Key tools |
|---|---|---|
| **Assess** | Evaluate readiness for cloud | Migration Evaluator |
| **Mobilize** | Create migration plan, address gaps | AWS Application Discovery Service, AWS Migration Hub |
| **Migrate and Modernize** | Execute the migration | Application Migration Service, Database Migration Service, DataSync, Transfer Family, Snow Family |

### The 7 Rs of Migration

| Strategy | Description |
|---|---|
| **Rehost** | Lift and shift — move as-is to AWS |
| **Replatform** | Lift, tinker, and shift — minor optimisations (e.g. move to RDS) |
| **Refactor / Re-architect** | Redesign using cloud-native features |
| **Repurchase** | Move to a different product (e.g. switch to SaaS) |
| **Retain** | Keep on-premises for now |
| **Retire** | Decommission what is no longer needed |
| **Relocate** | Move to AWS with minimal changes (e.g. VMware Cloud on AWS) |

### AWS Cloud Adoption Framework (CAF)

Six perspectives:

| Perspective | Focus |
|---|---|
| Business | Align cloud investments with business goals |
| People | Culture, change management, skills |
| Governance | Risk, compliance, portfolio management |
| Platform | Architecture, engineering, infrastructure |
| Security | Protection of data and workloads |
| Operations | Service management, observability |

### Data Transfer Services

| Service | Description |
|---|---|
| **AWS DataSync** | Automate moving data between on-premises and AWS storage |
| **AWS Transfer Family** | SFTP, FTP, FTPS transfer directly into S3 or EFS |
| **AWS Snow Family** | Physical devices to move large volumes of data offline |
| — Snowcone | Smallest — up to 14TB |
| — Snowball Edge | Medium — up to 80TB |
| — Snowmobile | Exabyte-scale — a literal truck |

---

## AI and Machine Learning

### AI services (pre-built, no ML knowledge needed)

| Service | Function |
|---|---|
| **Amazon Rekognition** | Image and video analysis — detect objects, faces, activities |
| **Amazon Textract** | Extract text and data from scanned documents |
| **Amazon Comprehend** | Natural language processing — sentiment analysis, entity recognition |
| **Amazon Transcribe** | Speech-to-text |
| **Amazon Translate** | Text translation |
| **Amazon Polly** | Text-to-speech |
| **Amazon Kendra** | Intelligent enterprise search |
| **Amazon Personalize** | Real-time personalisation and recommendations |
| **Amazon Forecast** | Time-series forecasting |
| **Amazon Lex** | Build conversational chatbots (same tech as Alexa) |

### ML services

**Amazon SageMaker AI** — fully managed platform to build, train, and deploy ML models. Includes notebooks, training jobs, and model hosting.

**Amazon SageMaker JumpStart** — pre-trained foundation models and ML solutions you can deploy or fine-tune with your own data.

### Generative AI

| Service | Description |
|---|---|
| **Amazon Bedrock** | Access pre-trained foundation models from Amazon and third parties (Anthropic, Cohere, Meta) via API |
| **Amazon Q** | AI assistant tailored to your business — integrates with your data sources and tools |
| **Amazon CodeWhisperer** | AI coding companion (now part of Amazon Q Developer) |

### ML tiers

1. **AI services** — pre-built, call via API, no ML expertise needed
2. **ML services** — build custom models with SageMaker
3. **ML frameworks and infrastructure** — custom with purpose-built chips (Trainium for training, Inferentia for inference)

---

## Developer and Application Services

### Messaging and decoupling

Loosely coupled architectures use messaging to reduce dependencies between components.

| Service | Type | Description |
|---|---|---|
| **Amazon SQS** | Queue | Buffer between producers and consumers. Send, store, receive messages. Decouples components. |
| **Amazon SNS** | Pub/Sub | Push notifications in real time. One publisher, many subscribers. |
| **Amazon EventBridge** | Event bus | Route events between AWS services and applications. Replaces CloudWatch Events. Good for scheduled jobs (cron). |
| **Amazon Kinesis** | Streaming | Real-time data streaming from applications, sensors, and logs |

> **Exam tip**: SQS = queue (async, one consumer per message). SNS = broadcast (one message to many). EventBridge = event routing and scheduling.

### CI/CD and Developer Tools

| Service | Description |
|---|---|
| **AWS CodeBuild** | Compile code, run tests, produce deployment packages |
| **AWS CodePipeline** | Orchestrate full CI/CD pipelines (build, test, deploy) |
| **AWS CodeDeploy** | Automate application deployments to EC2, Lambda, or on-premises |
| **AWS X-Ray** | Distributed tracing — debug and analyse performance |
| **AWS AppSync** | Build GraphQL APIs, real-time data sync |
| **AWS Amplify** | Build, deploy, and host full-stack web and mobile apps |

### Business Applications

| Service | Description |
|---|---|
| **Amazon Connect** | Cloud-based contact centre |
| **Amazon SES** | Send large volumes of email (transactional and marketing) |

### End User Computing

| Service | Description |
|---|---|
| **Amazon AppStream 2.0** | Stream desktop applications to any device |
| **Amazon WorkSpaces** | Managed virtual desktops (DaaS) |
| **Amazon WorkSpaces Web** | Browser-based access to internal web applications |

### IoT

**AWS IoT Core** — connect and manage IoT devices at scale. Route messages between devices and AWS services.

### Data and Analytics

| Service | Description |
|---|---|
| **Kinesis Data Streams** | Real-time data ingestion |
| **Kinesis Data Firehose** | Near real-time delivery to S3, Redshift, OpenSearch |
| **AWS Glue** | Managed ETL service — discover, prepare, and transform data |
| **Amazon EMR** | Big data processing using Apache Spark, Hadoop |
| **Amazon Athena** | Query S3 data with SQL — serverless, pay per query |
| **Amazon Redshift** | Data warehouse for analytics at scale |
| **Amazon QuickSight** | Business intelligence and data visualisation |
| **Amazon OpenSearch** | Search and analytics engine (successor to Elasticsearch) |

### IaC

| Service | Description |
|---|---|
| **AWS CloudFormation** | Define AWS infrastructure in JSON or YAML templates |
| **AWS CDK** | Define infrastructure using familiar programming languages |

---

## Well-Architected Framework

Six pillars that guide building secure, high-performing, resilient, and efficient infrastructure:

| Pillar | Key concern |
|---|---|
| **Operational excellence** | Run and monitor systems, continually improve processes |
| **Security** | Protect data, systems, and assets |
| **Reliability** | Recover from failures, meet demand |
| **Performance efficiency** | Use resources efficiently as demand changes |
| **Cost optimisation** | Avoid unnecessary costs |
| **Sustainability** | Minimise environmental impact |

> **Exam tip**: Know what each pillar covers. Questions often describe a scenario and ask which pillar is being addressed.

### AWS Well-Architected Tool

Free tool in the console that evaluates your workloads against the six pillars and provides improvement recommendations.

---

## Support Plans

| Plan | Who it's for | Response time (critical) | Key features |
|---|---|---|---|
| **Basic** | All accounts (free) | — | Trusted Advisor (7 checks), Health Dashboard |
| **Developer** | Testing and development | 12 hours (business hours) | Email support, 1 contact |
| **Business** | Production workloads | 1 hour | Full Trusted Advisor, phone/chat, AWS Support API |
| **Enterprise On-Ramp** | Production/business critical | 30 minutes | Pool of TAMs, concierge support |
| **Enterprise** | Mission critical | 15 minutes | Dedicated TAM, concierge, training credits |

> **Exam tip**: Only Business and above get full Trusted Advisor checks. Enterprise is the only plan with a dedicated Technical Account Manager (TAM).

---

## Quick Revision — Exam Day Reminders

- **Shared responsibility model** — AWS owns the infrastructure, you own your data and configurations
- **IAM** — deny by default, least privilege, MFA the root account immediately
- **S3** — Intelligent-Tiering for unknown access patterns, Glacier Deep Archive for lowest cost long-term storage
- **EC2** — Spot for flexible fault-tolerant workloads, Reserved for steady-state, On-demand for unpredictable
- **Security groups** — stateful, instance level. NACLs — stateless, subnet level
- **CloudTrail** — logs all API calls. CloudWatch — metrics and monitoring. Config — compliance tracking
- **RDS** — managed relational. DynamoDB — NoSQL. Redshift — data warehouse. ElastiCache — caching
- **SQS** — queue (async decoupling). SNS — pub/sub broadcast. EventBridge — event routing and scheduling
- **Well-Architected Framework** — six pillars: operational excellence, security, reliability, performance efficiency, cost optimisation, sustainability

---

*These notes are intended as a final exam crackdown reference. For full coverage, supplement with AWS Skill Builder practice exams and the official exam guide.*
