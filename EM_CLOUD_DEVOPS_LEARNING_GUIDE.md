# Engineering Manager: Cloud & DevOps Learning Guide

This document is a companion to README.md, focused specifically on what an Engineering Manager should know across AWS, Azure, GCP, and the core DevOps toolchain — plus a practical guide to High-Level and Low-Level Design diagrams.

## Table of Contents

- [Why an Engineering Manager Needs This Differently Than an IC](#why-an-engineering-manager-needs-this-differently-than-an-ic)
- [Cloud Services by Core Category (20+ Core Services, With Sub-Services)](#cloud-services-by-core-category-20-core-services-with-sub-services)
  - [AWS Services Every EM Should Know](#aws-services-every-em-should-know)
  - [Azure Services Every EM Should Know](#azure-services-every-em-should-know)
  - [GCP Services Every EM Should Know](#gcp-services-every-em-should-know)
- [Open-Source DevOps Tools Every EM Should Know](#open-source-devops-tools-every-em-should-know)
- [Cloud-Native (Provider-Specific) DevOps Tools](#cloud-native-provider-specific-devops-tools)
- [How to Draw High-Level Design (HLD) and Low-Level Design (LLD) Diagrams](#how-to-draw-high-level-design-hld-and-low-level-design-lld-diagrams)

---

### Why an Engineering Manager Needs This Differently Than an IC

As an **Engineering Manager (EM)**, you don't need implementation-level depth in every service (that's what your Sr./Lead Engineers and Architects are for). What you *do* need is enough working knowledge to:
- Evaluate technical proposals and challenge them intelligently.
- Have informed conversations about cost, risk, and tradeoffs with Architects and Leads.
- Understand what your team is building well enough to unblock them, hire for the right skills, and represent the work to your own leadership.
- Approve/reject architecture at a **high level** without needing to review every line of Terraform.

So the depth target for an EM is: **"I know what this service does, why we'd use it, roughly what it costs, and what questions to ask."** — not "I can configure it myself from scratch."

**How to use the tables below:** Each row has a **Status** column. Mark it as you go:
- `☐ Not Started`
- `◐ Familiar` (know what it is and when to use it)
- `● Learned` (can discuss tradeoffs, review a design using it, ask the right questions)

Simply edit the Status cell in this file as you progress — treat it as a living personal tracker.

---

### Cloud Services by Core Category (20+ Core Services, With Sub-Services)

Rather than a flat list, services are grouped under **24 core service categories** — the same categories apply across AWS, Azure, and GCP so you can directly compare equivalents. Each core category lists its key **sub-services** with status tracking.

---

### AWS Services Every EM Should Know

#### 1. Compute

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **EC2 (Elastic Compute Cloud)** | Virtual machines in the cloud — rentable servers billed by time. | Teams launch EC2 instances to run applications, backend services, or self-managed software. | Default choice when you need full control over the OS/runtime, or for workloads not suited to containers/serverless. |
| ☐ | **EC2 Auto Scaling** | Automatically adjusts the number of EC2 instances based on demand. | Configured with min/max/desired instance counts and scaling policies (CPU, request count, schedule). | Any workload with variable traffic where you want to avoid over- or under-provisioning. |
| ☐ | **EC2 Spot Instances** | Spare AWS compute capacity offered at up to 90% discount, reclaimable by AWS with short notice. | Used for fault-tolerant, flexible workloads that can handle interruption. | Batch processing, CI/CD build agents, big data jobs — anywhere cost matters more than guaranteed uptime. |
| ☐ | **Lightsail** | Simplified VM + hosting bundle with predictable, flat pricing. | Used for simple websites/apps without needing full AWS complexity. | Small projects, prototypes, or teams new to AWS wanting a simpler on-ramp. |

#### 2. Serverless Compute

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Lambda** | Runs small pieces of code in response to events, with no server management. | Developers upload a function; AWS runs it on-demand and scales automatically. | Event-driven workloads (file uploads, API backends, scheduled jobs) with unpredictable or bursty traffic. |
| ☐ | **Lambda Layers** | Reusable packages of code/dependencies shared across multiple Lambda functions. | Reduces duplication by centralizing common libraries used by several functions. | Any org running many Lambda functions that share common dependencies. |

#### 3. Containers & Orchestration

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **ECS (Elastic Container Service)** | AWS's own, simpler container orchestrator. | Teams define "task definitions" describing containers to run; ECS schedules them onto compute. | Simpler AWS-native container needs without the complexity of Kubernetes. |
| ☐ | **EKS (Elastic Kubernetes Service)** | Managed Kubernetes control plane on AWS. | Teams run standard Kubernetes workloads without managing the K8s control plane themselves. | Organizations standardizing on Kubernetes across multi-cloud or hybrid environments. |
| ☐ | **Fargate** | Serverless compute engine for containers — works with both ECS and EKS. | Removes the need to provision/manage the underlying EC2 instances for containers. | Teams wanting containers without managing servers at all. |
| ☐ | **ECR (Elastic Container Registry)** | Managed Docker container image registry. | Teams push/pull container images as part of their CI/CD pipeline. | Anywhere you're running containers on AWS — the default private image store. |

#### 4. Object Storage

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **S3 (Simple Storage Service)** | Object storage for files of any size and type — highly durable, virtually unlimited. | Used to store static assets, backups, logs, data lake files, and application uploads. | Anytime you need durable, cheap, scalable file storage not tied to a specific server. |
| ☐ | **S3 Glacier** | Low-cost, long-term archival storage tier of S3. | Data is moved here via lifecycle policies once it's rarely accessed. | Compliance archives, long-term backups where retrieval time (minutes-hours) is acceptable. |
| ☐ | **S3 Lifecycle Policies** | Rules that automatically transition or delete objects based on age. | Set up once; S3 automatically moves data to cheaper tiers or deletes it over time. | Cost optimization for any bucket with predictable data-aging patterns. |

#### 5. Block & File Storage

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **EBS (Elastic Block Store)** | Block storage volumes attached to EC2 instances, like a virtual hard drive. | Provides persistent storage for a VM's OS and application data. | Whenever an EC2 instance needs a persistent disk (databases, file systems). |
| ☐ | **EFS (Elastic File System)** | Managed, scalable NFS file system shareable across multiple instances. | Mounted by multiple EC2/container instances simultaneously for shared file access. | Workloads needing a shared file system across many compute instances (e.g., content management systems). |

#### 6. Relational Database

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **RDS (Relational Database Service)** | Managed relational database service (MySQL, PostgreSQL, SQL Server, MariaDB, Oracle). | AWS handles patching, backups, and failover for the database engine you choose. | Standard choice for relational/transactional data without wanting to manage DB servers yourself. |
| ☐ | **Aurora** | AWS's own high-performance, cloud-native relational database (MySQL/PostgreSQL compatible). | Used like RDS but with higher throughput, auto-scaling storage, and better failover. | High-performance transactional workloads where standard RDS engines aren't fast/scalable enough. |

#### 7. NoSQL & Caching Database

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **DynamoDB** | Fully managed, serverless NoSQL key-value/document database. | Used for applications needing very high throughput and single-digit-millisecond latency at scale. | High-scale applications (e.g., gaming, IoT, e-commerce carts) where relational structure isn't required. |
| ☐ | **ElastiCache (Redis/Memcached)** | Managed in-memory caching service. | Sits in front of a database to serve frequently accessed data much faster. | Reducing database load and latency for read-heavy applications. |

#### 8. Data Warehouse & Analytics

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Redshift** | Managed, petabyte-scale data warehouse for analytics. | Business intelligence teams run complex analytical queries across huge datasets. | Large-scale reporting/analytics needs separate from transactional databases. |
| ☐ | **Athena** | Serverless query service that runs SQL directly against data in S3. | Teams query raw files in S3 without needing to load them into a database first. | Ad-hoc analytics on data lake files without standing up infrastructure. |

#### 9. Virtual Networking

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **VPC (Virtual Private Cloud)** | A private, isolated network within AWS where you place your resources. | Defines subnets, route tables, and gateways that control how resources communicate and reach the internet. | Every AWS deployment — it's the networking foundation everything else sits inside. |
| ☐ | **Subnets (Public/Private)** | Segments of a VPC's IP range, isolated for different tiers of an application. | Public subnets host internet-facing resources; private subnets isolate databases/internal services. | Standard security pattern — separating what's exposed to the internet from what isn't. |
| ☐ | **NAT Gateway** | Allows resources in private subnets to reach the internet (e.g., for updates) without being reachable from it. | Placed in a public subnet; private subnet traffic routes through it outbound only. | Any private subnet resource that needs outbound internet access (e.g., pulling OS patches). |
| ☐ | **Direct Connect / Site-to-Site VPN** | Dedicated network connection (Direct Connect) or encrypted tunnel (VPN) between on-premises and AWS. | Used to securely connect a company's data center network to AWS. | Hybrid-cloud environments or gradual on-prem-to-cloud migrations. |

#### 10. DNS

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Route 53** | AWS's domain name system (DNS) and domain registration service. | Routes user requests (e.g., www.company.com) to the correct AWS resources. | Whenever you need custom domains, DNS failover, or traffic routing policies. |
| ☐ | **Route 53 Health Checks** | Monitors endpoint health and can automatically reroute DNS away from failing resources. | Combined with routing policies for automatic failover to healthy regions/resources. | High-availability, multi-region architectures. |

#### 11. Content Delivery (CDN)

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **CloudFront** | Content Delivery Network that caches content at edge locations close to users. | Speeds up delivery of static/dynamic content by serving it from a nearby edge location. | Public-facing websites/APIs where latency and load reduction matter. |

#### 12. Load Balancing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Application Load Balancer (ALB)** | Layer 7 (HTTP/HTTPS) load balancer with routing rules. | Routes traffic to different target groups based on URL path, host header, etc. | Web applications and microservices needing content-based routing. |
| ☐ | **Network Load Balancer (NLB)** | Layer 4 (TCP/UDP) load balancer for extreme performance. | Handles millions of requests/second with ultra-low latency. | High-performance, low-latency workloads (gaming, financial systems). |

#### 13. API Management

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **API Gateway** | Fully managed service to create, publish, and secure APIs. | Sits in front of Lambda/backend services, handling auth, throttling, and request routing. | Any team exposing APIs publicly or internally that needs rate limiting, auth, and monitoring built in. |

#### 14. Identity & Access Management

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **IAM (Identity and Access Management)** | Controls who/what can access which AWS resources. | Defines users, roles, and policies following least-privilege principles. | Every AWS account — it's the security backbone; EMs should understand roles vs. users vs. policies conceptually. |
| ☐ | **IAM Roles / Instance Profiles** | Temporary, assumable permissions (vs. permanent user credentials). | Attached to EC2/Lambda/services so they can access other AWS resources securely without hardcoded keys. | Best practice for any service-to-service AWS access — avoids storing long-lived credentials. |

#### 15. Secrets & Key Management

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **KMS (Key Management Service)** | Manages encryption keys used to encrypt data across AWS services. | Services like S3, RDS, and EBS use KMS keys to encrypt data at rest. | Anywhere encryption at rest is required (almost always, for compliance). |
| ☐ | **Secrets Manager** | Securely stores and automatically rotates credentials/API keys. | Applications retrieve secrets at runtime instead of hardcoding them. | Anywhere sensitive credentials are involved — a compliance/security must-have. |

#### 16. Monitoring & Metrics

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **CloudWatch Metrics** | AWS's native metrics collection service. | Teams set up dashboards to track application/infrastructure health (CPU, latency, error rates). | Default monitoring layer for any AWS workload. |
| ☐ | **CloudWatch Alarms** | Triggers actions/notifications when metrics cross defined thresholds. | Configured to alert on-call engineers or trigger auto-scaling when thresholds are breached. | Any production workload needing proactive alerting. |

#### 17. Logging & Auditing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **CloudWatch Logs** | Centralized log collection and storage service. | Applications/services stream logs here for search and retention. | Default log aggregation point for AWS workloads. |
| ☐ | **CloudTrail** | Logs every API call made in your AWS account for auditing and security investigation. | Used to answer "who did what, when" — critical for compliance and incident investigation. | Required in regulated environments; useful for any security-conscious organization. |

#### 18. Messaging & Queueing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **SQS (Simple Queue Service)** | Fully managed message queuing service. | Decouples application components — a producer sends messages, a consumer processes them independently. | Anytime you need reliable, asynchronous processing between services (e.g., order processing). |

#### 19. Event Bus & Notifications

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **SNS (Simple Notification Service)** | Managed pub/sub messaging and notification service. | Publishes a message once; fans it out to many subscribers (email, SMS, Lambda, SQS). | Broadcasting events to multiple downstream consumers at once. |
| ☐ | **EventBridge** | Serverless event bus connecting AWS services, SaaS apps, and custom applications. | Routes events between services based on rules, enabling event-driven architectures. | Building loosely-coupled, event-driven systems across many services. |

#### 20. Infrastructure as Code (Native)

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **CloudFormation** | AWS-native Infrastructure as Code service using JSON/YAML templates. | Teams define infrastructure in template files and deploy/update it repeatably. | AWS-only shops that prefer a native IaC tool over third-party options like Terraform. |
| ☐ | **AWS CDK (Cloud Development Kit)** | Lets you define infrastructure using real programming languages (Python, TypeScript, etc.), which compiles to CloudFormation. | Developers write infrastructure in familiar code instead of YAML/JSON. | Teams wanting IaC with the expressiveness of a real programming language. |

#### 21. CI/CD (Native)

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **CodePipeline** | Fully managed CI/CD orchestration service. | Chains together source, build, test, and deploy stages. | AWS-centric orgs wanting an end-to-end managed pipeline without hosting Jenkins. |
| ☐ | **CodeBuild** | Fully managed build service that compiles code and runs tests. | Triggered as part of a pipeline to produce build artifacts. | AWS-native builds, especially when paired with CodePipeline. |
| ☐ | **CodeDeploy** | Automates code deployments to EC2, Lambda, or ECS. | Handles rolling, blue-green, or canary deployments automatically. | AWS-native deployment automation, particularly for EC2 fleets. |

#### 22. Backup & Disaster Recovery

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **AWS Backup** | Centralized backup service across multiple AWS resource types. | Defines backup policies (schedule, retention) applied automatically across EBS, RDS, DynamoDB, etc. | Any org wanting a single, auditable backup policy instead of managing backups per-service. |

#### 23. Cost Management / FinOps

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cost Explorer** | Visualizes AWS spend trends over time by service, tag, or account. | Used to identify cost drivers and forecast future spend. | Every EM should check this regularly — cost accountability is a core EM responsibility. |
| ☐ | **AWS Budgets** | Sets custom cost/usage budgets with automated alerts. | Configured to notify teams when spend approaches or exceeds a threshold. | Proactive cost control before a bill surprise happens. |

#### 24. Compliance & Governance

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **AWS Organizations** | Manages multiple AWS accounts centrally under one umbrella. | Used to apply policies (SCPs) and consolidate billing across many accounts/teams. | Any company running more than a handful of AWS accounts (e.g., per team/environment). |
| ☐ | **AWS Config** | Tracks resource configurations and evaluates them against compliance rules. | Alerts when a resource drifts from an approved configuration (e.g., an S3 bucket becomes public). | Regulated environments needing continuous compliance monitoring. |

---

### Azure Services Every EM Should Know

#### 1. Compute

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Virtual Machines** | Azure's IaaS virtual servers. | Run applications/services needing full OS-level control. | Default when workloads need full control or lift-and-shift from on-prem. |
| ☐ | **Virtual Machine Scale Sets (VMSS)** | Group of identical, auto-scaling VMs. | Automatically adds/removes VM instances based on load. | Workloads needing horizontal scaling of IaaS VMs. |

#### 2. Serverless Compute

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Functions** | Event-driven, serverless code execution. | Runs code in response to triggers (HTTP, timers, queue messages) without managing servers. | Event-driven or intermittent workloads, similar use case to AWS Lambda. |
| ☐ | **Logic Apps** | Low-code workflow automation connecting apps/services. | Business users/developers build workflows visually connecting APIs and services. | Integration-heavy automation without writing custom glue code. |

#### 3. Containers & Orchestration

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **AKS (Azure Kubernetes Service)** | Azure's managed Kubernetes offering. | Teams deploy containerized applications to a managed K8s cluster. | Standard choice for container orchestration in Azure-centric organizations. |
| ☐ | **Azure Container Instances (ACI)** | Runs a single container without managing any orchestration. | Quickly spins up a container for a short-lived or simple task. | Simple, isolated container workloads not needing full Kubernetes. |
| ☐ | **Azure Container Registry (ACR)** | Managed private Docker container image registry. | Teams push/pull container images as part of their CI/CD pipeline. | Anywhere you're running containers on Azure — the default private image store. |

#### 4. Platform-as-a-Service (Web Hosting)

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **App Service** | Fully managed platform for hosting web apps and APIs. | Developers deploy code directly without managing underlying infrastructure. | Web applications/APIs where you want PaaS simplicity over IaaS control. |

#### 5. Object Storage

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Blob Storage** | Azure's object storage service (equivalent to AWS S3). | Stores unstructured data — files, images, backups, logs. | Anytime you need scalable, durable object storage. |
| ☐ | **Blob Lifecycle Management** | Rules that automatically move/delete blobs based on age. | Set once; Azure automatically transitions data to cheaper access tiers over time. | Cost optimization for storage accounts with predictable data-aging patterns. |

#### 6. Block & File Storage

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Managed Disks** | Block storage volumes attached to Azure VMs. | Provides persistent storage for a VM's OS and application data. | Whenever a VM needs a persistent disk (databases, file systems). |
| ☐ | **Azure Files** | Managed, shareable file storage accessible via SMB/NFS. | Mounted by multiple VMs/services simultaneously for shared file access. | Workloads needing a shared file system across many compute instances. |

#### 7. Relational Database

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure SQL Database** | Managed relational database (SQL Server engine). | Azure handles patching, backups, and failover automatically. | Standard choice for relational/transactional data in Microsoft-centric shops. |
| ☐ | **Azure Database for PostgreSQL/MySQL** | Managed open-source relational database engines. | Same managed experience as Azure SQL DB, for teams preferring open-source engines. | Relational workloads where PostgreSQL/MySQL is preferred over SQL Server. |

#### 8. NoSQL & Caching Database

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cosmos DB** | Globally distributed, multi-model NoSQL database. | Used for applications needing global scale, low latency, and flexible data models. | High-scale, globally distributed applications where relational structure isn't required. |
| ☐ | **Azure Cache for Redis** | Managed in-memory caching service. | Sits in front of a database to serve frequently accessed data much faster. | Reducing database load and latency for read-heavy applications. |

#### 9. Data Warehouse & Analytics

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Synapse Analytics** | Unified analytics platform combining data warehousing and big data processing. | Analysts/data engineers run large-scale queries across structured and unstructured data. | Large-scale enterprise reporting/analytics and data warehousing needs. |

#### 10. Virtual Networking

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Virtual Network (VNet)** | Azure's private network construct for isolating and connecting resources. | Defines subnets, network security groups, and routing for all Azure resources. | Networking foundation for every Azure deployment. |
| ☐ | **Network Security Groups (NSG)** | Firewall rules controlling inbound/outbound traffic at the subnet/NIC level. | Attached to subnets/VMs to allow or deny specific traffic. | Standard security control for isolating tiers of an application. |
| ☐ | **VPN Gateway / ExpressRoute** | VPN Gateway = encrypted tunnel; ExpressRoute = dedicated private connection to Azure. | Connects on-premises networks securely to Azure. | Hybrid-cloud environments or gradual on-prem-to-cloud migrations. |

#### 11. DNS

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure DNS** | Azure's domain name system hosting service. | Routes user requests (e.g., www.company.com) to the correct Azure resources. | Whenever you need custom domains managed within Azure. |

#### 12. Content Delivery (CDN) & Global Routing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Front Door** | Global traffic routing service combined with CDN and WAF capabilities. | Routes and accelerates user traffic across regions with built-in security. | Public-facing, globally distributed, highly available applications. |
| ☐ | **Azure CDN** | Content Delivery Network caching content at edge locations. | Speeds up delivery of static content by serving it from a nearby edge location. | Public-facing websites/APIs where latency and load reduction matter. |

#### 13. Load Balancing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Load Balancer** | Regional Layer 4 (TCP/UDP) load balancer. | Distributes traffic across VMs/instances within a region. | High-performance, low-latency workloads within a single region. |
| ☐ | **Application Gateway** | Layer 7 (HTTP/HTTPS) load balancer with WAF capabilities. | Routes traffic based on URL path/host header and can block common web attacks. | Web applications needing content-based routing plus built-in security. |

#### 14. API Management

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **API Management (APIM)** | Fully managed service to publish, secure, and monitor APIs. | Sits in front of backend services, handling auth, throttling, and versioning. | Any team exposing APIs publicly or internally needing governance built in. |

#### 15. Identity & Access Management

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Active Directory (Entra ID)** | Azure's identity and access management platform (also used for enterprise SSO). | Manages user identities, application permissions, and access policies. | Central to every Azure environment, especially enterprises already using Microsoft 365. |
| ☐ | **Role-Based Access Control (RBAC)** | Assigns granular permissions to users/groups/apps on Azure resources. | Applied at subscription, resource group, or resource level following least-privilege principles. | Every Azure environment — the core access-control mechanism. |

#### 16. Secrets & Key Management

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Key Vault** | Secure storage for secrets, keys, and certificates. | Applications retrieve credentials/secrets securely at runtime instead of hardcoding them. | Anywhere sensitive credentials or encryption keys need centralized, auditable management. |

#### 17. Monitoring & Metrics

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Monitor** | Azure's native monitoring, metrics, and alerting platform. | Teams build dashboards and alerts to track application/infrastructure health. | Default monitoring layer for any Azure workload. |
| ☐ | **Application Insights** | Application performance monitoring (APM) built on Azure Monitor. | Tracks request rates, response times, and failure rates for applications specifically. | Any application where you need deep performance/error visibility, not just infra metrics. |

#### 18. Logging & Auditing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Log Analytics** | Centralized log storage and query service (uses KQL query language). | Teams run log queries to investigate issues across many resources. | Default log aggregation and investigation point for Azure workloads. |
| ☐ | **Azure Activity Log** | Logs subscription-level events (who created/modified/deleted resources). | Used to answer "who did what, when" — critical for compliance and incident investigation. | Required in regulated environments; useful for any security-conscious organization. |

#### 19. Messaging & Queueing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Queue Storage** | Simple, managed message queuing service. | Decouples application components — a producer sends messages, a consumer processes them independently. | Anytime you need reliable, asynchronous processing between services. |
| ☐ | **Service Bus** | Enterprise-grade messaging with advanced features (topics, sessions, dead-lettering). | Used for complex messaging patterns beyond simple queues (pub/sub, ordered delivery). | Enterprise integration scenarios needing guaranteed ordering or complex routing. |

#### 20. Event Bus & Notifications

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Event Grid** | Serverless event routing service connecting Azure services and custom apps. | Routes events between services based on rules, enabling event-driven architectures. | Building loosely-coupled, event-driven systems across many services. |

#### 21. Infrastructure as Code (Native)

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **ARM Templates** | Azure-native Infrastructure as Code using JSON. | Teams define infrastructure in template files and deploy/update it repeatably. | Azure-only shops using the original native IaC format. |
| ☐ | **Bicep** | Modern, simplified DSL that compiles down to ARM templates. | Developers write cleaner, more readable syntax than raw ARM JSON. | Azure-only shops preferring native IaC over Terraform, with better developer experience than raw ARM. |

#### 22. CI/CD (Native)

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Pipelines** | Managed CI/CD service, part of the Azure DevOps suite. | Builds, tests, and deploys code to any platform (not limited to Azure). | Very popular even outside pure-Azure shops due to strong flexibility and a generous free tier. |
| ☐ | **Azure Repos** | Managed private Git repository hosting. | Source control tightly integrated with Azure DevOps boards and pipelines. | Microsoft-centric orgs already using Azure DevOps for planning and tracking. |

#### 23. Backup & Disaster Recovery

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Backup** | Centralized backup service across Azure resource types. | Defines backup policies (schedule, retention) applied automatically across VMs, databases, etc. | Any org wanting a single, auditable backup policy across resources. |
| ☐ | **Azure Site Recovery** | Disaster recovery service that replicates workloads to a secondary region. | Used to fail over entire applications/VMs to another region during an outage. | Business-critical workloads needing a formal DR plan with defined RTO/RPO. |

#### 24. Cost Management / FinOps

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Azure Cost Management** | Tool for tracking, analyzing, and controlling Azure spend. | Used to monitor budgets, forecast costs, and identify savings opportunities. | Regular EM review — same purpose as AWS Cost Explorer. |
| ☐ | **Azure Advisor** | Provides personalized recommendations for cost, security, and performance. | Reviewed periodically to catch quick-win optimizations (e.g., resize underused VMs). | Ongoing cost/performance hygiene across an Azure environment. |

---

### GCP Services Every EM Should Know

#### 1. Compute

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Compute Engine** | GCP's virtual machine service. | Runs applications needing full OS-level control. | Standard IaaS choice, similar to EC2/Azure VMs. |
| ☐ | **Managed Instance Groups (MIG)** | Group of identical, auto-scaling VMs. | Automatically adds/removes VM instances based on load or health checks. | Workloads needing horizontal scaling of IaaS VMs. |

#### 2. Serverless Compute

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud Functions** | Event-driven, serverless function execution. | Runs code in response to events without managing servers. | Lightweight, event-driven workloads — GCP's equivalent of Lambda/Azure Functions. |
| ☐ | **Cloud Run** | Runs containerized applications in a fully managed, serverless environment. | Teams deploy a container image and GCP handles scaling (including scaling to zero). | Ideal middle ground between Cloud Functions and full Kubernetes — great for stateless web services/APIs. |

#### 3. Containers & Orchestration

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **GKE (Google Kubernetes Engine)** | GCP's managed Kubernetes service — the most mature K8s offering, since Google created Kubernetes. | Teams deploy and manage containerized workloads at scale. | Preferred choice for organizations heavily standardized on Kubernetes. |
| ☐ | **Artifact Registry** | Managed repository for container images and language packages. | Teams push/pull container images and packages as part of their CI/CD pipeline. | Anywhere you're running containers or publishing packages on GCP. |

#### 4. Object Storage

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud Storage** | GCP's object storage service (equivalent to S3/Blob Storage). | Stores unstructured files — backups, media, data lake files. | Anytime durable, scalable object storage is needed. |
| ☐ | **Cloud Storage Lifecycle Rules** | Rules that automatically transition or delete objects based on age. | Set up once; automatically moves data to cheaper storage classes over time. | Cost optimization for buckets with predictable data-aging patterns. |

#### 5. Block & File Storage

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Persistent Disk** | Block storage volumes attached to Compute Engine VMs. | Provides persistent storage for a VM's OS and application data. | Whenever a VM needs a persistent disk (databases, file systems). |
| ☐ | **Filestore** | Managed, high-performance NFS file storage. | Mounted by multiple VMs/GKE pods simultaneously for shared file access. | Workloads needing a shared file system across many compute instances. |

#### 6. Relational Database

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud SQL** | Managed relational database service (MySQL, PostgreSQL, SQL Server). | GCP handles patching, backups, and failover for the database engine you choose. | Standard choice for relational/transactional data without managing DB servers yourself. |
| ☐ | **AlloyDB** | GCP's high-performance, PostgreSQL-compatible database. | Used like Cloud SQL but with higher throughput for demanding transactional/analytical workloads. | High-performance workloads needing PostgreSQL compatibility with better performance. |

#### 7. NoSQL & Caching Database

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Firestore** | Serverless, flexible NoSQL document database. | Used for applications needing flexible schemas and real-time data sync (e.g., mobile/web apps). | App backends needing flexible, scalable document storage, especially mobile apps. |
| ☐ | **Memorystore (Redis/Memcached)** | Managed in-memory caching service. | Sits in front of a database to serve frequently accessed data much faster. | Reducing database load and latency for read-heavy applications. |

#### 8. Data Warehouse & Analytics

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **BigQuery** | Serverless, highly scalable data warehouse for analytics — a major GCP differentiator. | Analysts run SQL queries across massive datasets without managing any infrastructure. | Large-scale reporting/analytics needs, especially when speed and zero-ops matter. |
| ☐ | **Dataflow** | Managed service for stream and batch data processing (based on Apache Beam). | Used to build data pipelines that transform and move data at scale. | Complex ETL/streaming data pipelines feeding into BigQuery or other stores. |

#### 9. Virtual Networking

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **VPC** | GCP's private network construct — notably GCP VPCs are global by default (unlike AWS/Azure's regional VPCs). | Defines subnets, firewall rules, and routing for GCP resources. | Networking foundation for every GCP deployment. |
| ☐ | **Firewall Rules** | Controls inbound/outbound traffic at the VPC level. | Applied to allow/deny specific traffic between resources or from the internet. | Standard security control for isolating tiers of an application. |
| ☐ | **Cloud VPN / Cloud Interconnect** | Cloud VPN = encrypted tunnel; Cloud Interconnect = dedicated private connection to GCP. | Connects on-premises networks securely to GCP. | Hybrid-cloud environments or gradual on-prem-to-cloud migrations. |

#### 10. DNS

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud DNS** | GCP's scalable, managed domain name system service. | Routes user requests (e.g., www.company.com) to the correct GCP resources. | Whenever you need custom domains managed within GCP. |

#### 11. Content Delivery (CDN)

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud CDN** | Content Delivery Network caching content at edge locations close to users. | Speeds up delivery of static/dynamic content by serving it from a nearby edge location. | Public-facing websites/APIs where latency and load reduction matter. |

#### 12. Load Balancing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud Load Balancing** | Global, fully distributed load balancing service. | Distributes traffic across instances/regions for availability and performance. | Public-facing applications needing global reach with low latency. |

#### 13. API Management

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Apigee** | Full-featured API management platform. | Sits in front of backend services, handling auth, throttling, analytics, and monetization. | Enterprise-grade API programs needing advanced governance and analytics. |

#### 14. Identity & Access Management

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud IAM** | GCP's identity and access management system. | Defines who/what can access which GCP resources, following least-privilege principles. | Core to every GCP project — EMs should understand roles and service accounts conceptually. |
| ☐ | **Service Accounts** | Non-human identities used by applications/VMs to authenticate to GCP APIs. | Attached to compute resources so they can access other GCP services securely. | Best practice for any service-to-service GCP access — avoids storing long-lived credentials. |

#### 15. Secrets & Key Management

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Secret Manager** | Securely stores credentials/API keys. | Applications retrieve secrets securely at runtime instead of hardcoding them. | Anywhere sensitive data or credentials are involved. |
| ☐ | **Cloud KMS** | Manages encryption keys used to encrypt data across GCP services. | Services use KMS keys to encrypt data at rest. | Anywhere encryption at rest is required (almost always, for compliance). |

#### 16. Monitoring & Metrics

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud Monitoring** | GCP's native observability suite (formerly Stackdriver Monitoring). | Teams build dashboards and alerts to track application/infrastructure health. | Default monitoring layer for any GCP workload. |

#### 17. Logging & Auditing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud Logging** | GCP's native centralized logging service (formerly Stackdriver Logging). | Teams search and analyze logs from all GCP services in one place. | Default log aggregation point for GCP workloads. |
| ☐ | **Cloud Audit Logs** | Logs every admin activity and API call in a GCP project. | Used to answer "who did what, when" — critical for compliance and incident investigation. | Required in regulated environments; useful for any security-conscious organization. |

#### 18. Messaging & Queueing

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Pub/Sub** | Fully managed, globally scalable messaging service (combines queueing and pub/sub patterns). | Decouples application components — publishers send messages, subscribers process them independently. | Anytime you need reliable, asynchronous, or event-driven processing between services. |

#### 19. Event Bus & Notifications

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Eventarc** | Event routing service connecting GCP services and custom applications. | Routes events between services based on rules, enabling event-driven architectures. | Building loosely-coupled, event-driven systems across many services on GCP. |

#### 20. Infrastructure as Code (Native)

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Deployment Manager** | GCP's original native Infrastructure as Code tool. | Defines infrastructure declaratively using YAML/Python templates. | GCP-only shops preferring native tooling (though Terraform is more commonly used on GCP today). |
| ☐ | **Config Connector** | Lets you manage GCP resources as Kubernetes objects. | Used by teams already managing everything via Kubernetes/GitOps workflows. | Organizations wanting to manage GCP infrastructure the same way they manage K8s workloads. |

#### 21. CI/CD (Native)

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud Build** | Fully managed CI/CD service on GCP. | Executes build steps defined in a config file, often triggered by a Git push. | GCP-centric orgs wanting native CI/CD without managing Jenkins infrastructure. |
| ☐ | **Cloud Deploy** | Managed continuous delivery service for GKE and other GCP compute targets. | Automates progressive delivery (e.g., dev → staging → prod) with approval gates. | GCP-native CD, especially for teams standardized on GKE. |

#### 22. Backup & Disaster Recovery

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Backup and DR Service** | Centralized backup and disaster recovery service across GCP resources. | Defines backup policies (schedule, retention) applied automatically across VMs, databases, etc. | Any org wanting a single, auditable backup/DR policy across resources. |

#### 23. Cost Management / FinOps

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Cloud Billing Reports** | Tools for tracking and controlling GCP spend. | Used to monitor budgets, forecast costs, and set alerts. | Regular EM review, same purpose as AWS/Azure cost tools. |
| ☐ | **Recommender** | Provides personalized recommendations for cost, security, and performance. | Reviewed periodically to catch quick-win optimizations (e.g., rightsizing VMs). | Ongoing cost/performance hygiene across a GCP environment. |

#### 24. Compliance & Governance

| Status | Sub-Service | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|
| ☐ | **Resource Manager** | Manages multiple GCP projects centrally under organizations/folders. | Used to apply policies and organize billing across many projects/teams. | Any company running more than a handful of GCP projects (e.g., per team/environment). |
| ☐ | **Security Command Center** | Centralized security and risk management platform for GCP. | Surfaces misconfigurations, vulnerabilities, and threats across a GCP environment. | Regulated environments needing continuous security/compliance monitoring. |

---

### Open-Source DevOps Tools Every EM Should Know

These tools are cloud-agnostic and widely used regardless of which cloud provider(s) your organization runs on.

| Status | Tool | Category | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|---|
| ☐ | **Git** | Version Control | Distributed version control system for tracking code changes. | Every engineer commits code to Git repositories (hosted on GitHub/GitLab/Bitbucket); it's the foundation for all CI/CD. | Universal — every software team uses it, in every environment. |
| ☐ | **Jenkins** | CI/CD | Open-source automation server for building, testing, and deploying code. | Teams define pipelines (as code, via Jenkinsfile) that run automatically on code changes. | Mature, highly customizable choice when you need full control over your CI/CD server, often in on-prem or hybrid setups. |
| ☐ | **Ansible** | Configuration Management | Agentless automation tool for configuring servers and deploying applications. | Teams write YAML "playbooks" describing the desired state of servers; Ansible applies it via SSH. | Configuring/standardizing servers at scale without installing agents — popular for its simplicity. |
| ☐ | **Terraform** | Infrastructure as Code | Cloud-agnostic IaC tool for provisioning infrastructure across any provider. | Teams write declarative `.tf` files describing desired infrastructure; Terraform plans and applies the changes. | The de facto standard for multi-cloud or cloud-agnostic infrastructure provisioning. |
| ☐ | **Docker** | Containerization | Packages an application and its dependencies into a portable container image. | Developers build images from a Dockerfile and run them consistently across any environment. | Universal — the standard packaging format for modern applications. |
| ☐ | **Kubernetes (K8s)** | Container Orchestration | Open-source system for automating deployment, scaling, and management of containerized applications. | Teams describe desired application state (replicas, resources, networking); Kubernetes maintains that state automatically. | Standard for running containerized workloads at scale, across any cloud or on-prem. |
| ☐ | **Helm** | Kubernetes Package Manager | "The package manager for Kubernetes" — bundles K8s manifests into reusable, versioned charts. | Teams install/upgrade complex applications on Kubernetes with a single command instead of managing many YAML files. | Anytime you're deploying non-trivial applications to Kubernetes repeatedly across environments. |
| ☐ | **Prometheus** | Monitoring | Open-source metrics collection and alerting toolkit, especially popular with Kubernetes. | Scrapes and stores time-series metrics from applications/infrastructure; powers alerting rules. | The de facto standard for Kubernetes-native monitoring. |
| ☐ | **Grafana** | Monitoring (Visualization) | Open-source dashboarding and visualization tool. | Connects to data sources like Prometheus to build visual dashboards of system health. | Paired with Prometheus (or other data sources) anywhere teams need visual, shareable dashboards. |
| ☐ | **ELK / EFK Stack** | Logging | Elasticsearch (search/storage) + Logstash/Fluentd (log processing) + Kibana (visualization). | Centralizes logs from many services into one searchable, visual platform. | Anytime you need centralized log search/analysis across a distributed system. |
| ☐ | **ArgoCD** | GitOps / CD | Declarative, GitOps-style continuous delivery tool for Kubernetes. | Continuously syncs the live state of a Kubernetes cluster to match what's defined in a Git repository. | Teams practicing GitOps — where Git is the single source of truth for what's deployed. |
| ☐ | **HashiCorp Vault** | Secrets Management | Open-source tool for securely storing and controlling access to secrets/credentials. | Applications request secrets from Vault at runtime instead of storing them in code or config files. | Anywhere strict secrets management/rotation is needed, especially multi-cloud environments. |
| ☐ | **SonarQube** | Code Quality / Security (SAST) | Static code analysis tool that scans code for bugs, vulnerabilities, and code smells. | Integrated into CI pipelines to block merges/releases that fail quality or security thresholds. | Any team wanting automated code-quality gates as part of their DevSecOps practice. |
| ☐ | **Trivy / Snyk** | Container/Dependency Scanning | Open-source (Trivy) / commercial (Snyk) tools that scan container images and dependencies for known vulnerabilities. | Run in CI pipelines to catch vulnerable packages/images before deployment. | Any team shipping containers or using third-party dependencies — essentially all modern teams. |
| ☐ | **Istio / Linkerd** | Service Mesh | Infrastructure layer that manages service-to-service communication in Kubernetes (traffic control, security, observability). | Adds features like mutual TLS, traffic splitting (canary releases), and fine-grained observability between microservices. | Larger microservice architectures on Kubernetes needing advanced traffic control and security. |

---

### Cloud-Native (Provider-Specific) DevOps Tools

Each cloud provider also offers its own first-party DevOps tooling as an alternative — or complement — to the open-source options above.

| Status | Tool | Provider | What It Is | How It's Used | Where/When To Use It |
|---|---|---|---|---|---|
| ☐ | **AWS CodeCommit** | AWS | Managed private Git repository hosting. | Teams host source code directly within AWS instead of GitHub/GitLab. | AWS-centric orgs wanting source control tightly integrated with AWS IAM. |
| ☐ | **AWS CodeBuild** | AWS | Fully managed build service that compiles code and runs tests. | Triggered as part of a pipeline to produce build artifacts. | AWS-native CI, especially when paired with CodePipeline. |
| ☐ | **AWS CodeDeploy** | AWS | Automates code deployments to EC2, Lambda, or ECS. | Handles rolling, blue-green, or canary deployments automatically. | AWS-native CD, particularly for EC2 fleets. |
| ☐ | **AWS CodePipeline** | AWS | Fully managed CI/CD orchestration service. | Chains together source, build, test, and deploy stages using AWS-native or third-party tools. | AWS-centric orgs wanting an end-to-end managed pipeline without hosting Jenkins. |
| ☐ | **Azure Repos** | Azure | Managed private Git repository hosting (part of Azure DevOps). | Source control tightly integrated with Azure DevOps boards and pipelines. | Microsoft-centric orgs already using Azure DevOps for planning and tracking. |
| ☐ | **Azure Pipelines** | Azure | Managed CI/CD service, part of the Azure DevOps suite. | Builds, tests, and deploys code to any platform (not limited to Azure). | Very popular even outside pure-Azure shops due to strong flexibility and generous free tier. |
| ☐ | **Azure Artifacts** | Azure | Managed package/artifact repository (npm, NuGet, Maven, Python packages). | Teams publish and consume internal packages/libraries securely. | Organizations needing a private, managed package feed integrated with Azure Pipelines. |
| ☐ | **Google Cloud Build** | GCP | Fully managed CI/CD service on GCP. | Executes build steps defined in a config file, often triggered by a Git push. | GCP-centric orgs wanting native CI/CD without managing Jenkins infrastructure. |
| ☐ | **Google Cloud Deploy** | GCP | Managed continuous delivery service for GKE and other GCP compute targets. | Automates progressive delivery (e.g., dev → staging → prod) with approval gates. | GCP-native CD, especially for teams standardized on GKE. |
| ☐ | **Google Artifact Registry** | GCP | Managed repository for container images and language packages. | Stores and manages versioned build artifacts and container images. | Any GCP-based CI/CD pipeline needing a secure artifact store. |

---

## How to Draw High-Level Design (HLD) and Low-Level Design (LLD) Diagrams

As an Engineering Manager, you won't necessarily draw these diagrams yourself day-to-day — but you need to know how to **read, review, and ask good questions about them**, and ideally sketch a rough HLD yourself when scoping a new initiative with your Architects. Below is a practical, step-by-step approach to both.

### High-Level Design (HLD)

**Purpose:** Communicate the overall shape of a system to a mixed audience (engineers, other managers, sometimes non-technical stakeholders). Answers: *"What are the major pieces, and how do they fit together?"*

**What an HLD typically includes:**
1. **Major system components** — e.g., web app, API layer, database, cache, message queue, third-party integrations.
2. **User/data flow** — how a request travels from the user through the system and back (arrows showing direction).
3. **Environment boundaries** — which components live in which environment/region/cloud account (often shown as boxes/containers).
4. **External dependencies** — third-party APIs, SaaS tools, other internal systems it talks to.
5. **High-level technology choices** — e.g., "Kubernetes cluster," "managed relational database," "CDN" — without configuration-level detail.

**Step-by-step process to draw an HLD:**
1. **Identify the actors** — who/what initiates activity? (end users, other services, scheduled jobs).
2. **List the major components** — write each as a box: e.g., "Load Balancer," "Web App," "API Service," "Database," "Cache," "Message Queue," "CDN."
3. **Draw the request flow** — connect the boxes with arrows in the order a request/data actually flows, left to right or top to bottom.
4. **Group by boundary** — draw larger rectangles around components that live in the same environment (e.g., "VPC," "Region," "Kubernetes Cluster") to show logical/physical separation.
5. **Add external systems** — place third-party services (payment gateway, email provider, auth provider) outside the main boundary with arrows showing integration points.
6. **Label each connection** — briefly note the protocol/purpose (e.g., "HTTPS," "async via queue," "read replica").
7. **Keep it to one page** — if it's getting cluttered, you're adding LLD-level detail; push that detail into a separate LLD diagram instead.

**Example structure (described, not drawn):**
```
[User] → [CDN] → [Load Balancer] → [Web App (K8s)] → [API Service (K8s)] → [Database (Managed RDS)]
                                                              ↓
                                                     [Cache (Redis)]
                                                              ↓
                                                     [Message Queue] → [Worker Service]
```

**Tools:** Lucidchart, draw.io (free), Microsoft Visio, Miro, or diagrams-as-code tools like Structurizr or the Python `diagrams` library (great for keeping diagrams version-controlled alongside code).

**Audience check:** If you handed this diagram to someone outside engineering, could they roughly follow how a user's request moves through the system? If not, simplify further.

---

### Low-Level Design (LLD)

**Purpose:** Give engineers implementation-ready detail for a *specific component* from the HLD. Answers: *"Exactly how is this piece built and configured?"*

**What an LLD typically includes:**
1. **Detailed component internals** — e.g., specific microservices within "API Service," their responsibilities, and how they communicate.
2. **Network-level detail** — VPC/VNet CIDR ranges, subnet layout (public/private), security group/firewall rules, NAT gateways.
3. **Resource-level detail** — instance types/sizes, autoscaling thresholds, storage sizing, database schema/indexing considerations.
4. **IAM/security detail** — specific roles, policies, and permissions each component needs (least privilege).
5. **Failure handling** — retry logic, timeout values, circuit breakers, failover targets, backup/restore procedures.
6. **Sequence of operations** — often shown as a sequence diagram for a specific flow (e.g., "user login" step-by-step, including error paths).

**Step-by-step process to draw an LLD:**
1. **Pick one component from the HLD** — e.g., "API Service" — don't try to detail the whole system at once.
2. **Break it into sub-components** — e.g., "Auth Module," "Order Module," "Notification Module," each as its own box.
3. **Detail the network layout** — draw the subnets it lives in, what can reach it (and what it can reach), and any firewall/security group rules as labeled lines.
4. **Specify resource configuration** — annotate boxes with actual sizing decisions: instance types, replica counts, autoscaling min/max, storage type and size.
5. **Draw a sequence diagram for key flows** — for the most important operation (e.g., checkout), show the exact order of calls between sub-components, including what happens on failure (timeouts, retries, fallback).
6. **Document IAM/permissions** — list, next to each component, exactly what roles/policies it's granted and why (this becomes the basis for the actual Terraform/IAM code).
7. **Cross-reference back to the HLD** — make sure the LLD box maps clearly to the corresponding box in the HLD, so anyone can trace from big picture to implementation detail.

**Example structure (described, not drawn):**
```
[API Service] (within Private Subnet 10.0.2.0/24)
  ├── Auth Module → calls → Identity Provider (external, HTTPS, retries=3, timeout=5s)
  ├── Order Module → reads/writes → RDS (private subnet, port 5432, encrypted, IAM role: order-service-role)
  ├── Notification Module → publishes → SQS Queue → consumed by → Worker Service
  └── Security Group: allow inbound 443 from ALB only; allow outbound 5432 to DB subnet only
```

**Tools:** Same diagramming tools as HLD (Lucidchart, draw.io, Visio), plus sequence-diagram-specific tools like PlantUML, Mermaid (renders directly in Markdown/GitHub), or Miro for collaborative sessions.

**Audience check:** Could an engineer who has never seen this system pick up the LLD and start implementing it correctly without needing to ask you clarifying questions? If not, add more detail.

---

### HLD vs. LLD — Quick Comparison

| Aspect | High-Level Design (HLD) | Low-Level Design (LLD) |
|---|---|---|
| **Audience** | Engineers, managers, sometimes non-technical stakeholders | Engineers implementing the system |
| **Scope** | Entire system or major initiative | One component/service at a time |
| **Detail level** | Boxes and arrows — components and flow | Configurations, sizing, IAM, sequence of calls |
| **Typical owner** | Architect (Cloud/DevOps), reviewed by EM | Sr. Engineer or Architect, implemented by team |
| **Common tools** | Lucidchart, draw.io, Visio, Miro | Same tools, plus PlantUML/Mermaid for sequence diagrams |
| **Question it answers** | "What are we building and how does it fit together?" | "Exactly how is each piece configured and built?" |

**As an EM, your practical role in this process:**
- Review HLDs for **scope, cost, and risk** — does this design fit the budget and timeline, and does it avoid unnecessary complexity?
- Use HLDs in conversations with your own leadership to explain what's being built, without getting into implementation weeds.
- Spot-check LLDs when a decision has major cost/security/compliance implications (e.g., "why are we opening this port publicly?"), but generally trust your Sr. Engineers/Architects to own that level of detail.
- Encourage your team to keep HLD/LLD diagrams versioned and updated — stale diagrams are worse than no diagrams, because they actively mislead.
