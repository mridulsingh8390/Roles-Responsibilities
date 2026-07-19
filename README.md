# Roles & Responsibilities: Cloud and DevOps Engineering Career Track

This document provides an in-depth explanation of each role within the **Cloud Engineering** and **DevOps Engineering** career tracks — covering responsibilities, day-to-day activities, required skills, tools, and what it takes to grow into the next level.

---

## Table of Contents

- [How to Read This Document](#how-to-read-this-document)
- [Cloud Platforms: AWS, Azure & GCP](#cloud-platforms-aws-azure--gcp)
- [Core DevOps Toolchain](#core-devops-toolchain)
- [Cloud Engineering Track](#cloud-engineering-track)
  - [1. Jr. Cloud Engineer](#1-jr-cloud-engineer)
  - [2. Cloud Engineer](#2-cloud-engineer)
  - [3. Sr. Cloud Engineer](#3-sr-cloud-engineer)
  - [4. Lead Cloud Engineer](#4-lead-cloud-engineer)
  - [5. Cloud Architect](#5-cloud-architect)
- [DevOps Engineering Track](#devops-engineering-track)
  - [1. Jr. DevOps Engineer](#1-jr-devops-engineer)
  - [2. DevOps Engineer](#2-devops-engineer)
  - [3. Sr. DevOps Engineer](#3-sr-devops-engineer)
  - [4. Lead DevOps Engineer](#4-lead-devops-engineer)
  - [5. DevOps Architect](#5-devops-architect)
- [Cloud vs DevOps: What's the Difference?](#cloud-vs-devops-whats-the-difference)
- [Career Progression Summary](#career-progression-summary)
- [How Promotion Decisions Are Typically Made](#how-promotion-decisions-are-typically-made)

---

## How to Read This Document

Each role below is broken into six parts so you understand not just *what* the role does, but *why* it exists and *how* someone in it spends their time:

1. **Overview** — the purpose of the role on the team.
2. **Responsibilities** — the concrete duties owned by this role.
3. **A Typical Day/Week Looks Like** — practical, everyday activities.
4. **Required Skills & Knowledge** — the technical and non-technical competencies expected.
5. **Tools Commonly Used** — the real-world software/platforms used in this role.
6. **How to Grow Into the Next Level** — the capabilities you need to demonstrate to be promoted.

> **A note on HLD vs. LLD:** Architect-level roles (and, to a lesser extent, Lead roles) are responsible for producing architecture diagrams at two levels of detail:
> - **High-Level Design (HLD):** The "big picture" view — major components, how systems/services connect, data flow between them, and how they map to cloud regions/environments. Intended for both technical and non-technical stakeholders.
> - **Low-Level Design (LLD):** The detailed, implementation-ready view — specific resource configurations (VPC/subnet CIDRs, IAM policies, pipeline stages, cluster topology) that engineers use to actually build the system. Intended for engineers doing the implementation.
>
> This is called out explicitly in the Cloud Architect and DevOps Architect sections below.

---

## Cloud Platforms: AWS, Azure & GCP

Cloud Engineers (and DevOps Engineers working on infrastructure) are expected to work with one or more of the three major public cloud providers. While the underlying concepts are similar, the service names and ecosystems differ. Most professionals specialize in one platform and gain working familiarity with the others.

### Amazon Web Services (AWS)
The market-leading cloud platform, known for the breadth and maturity of its service catalog.

| Category | Key Services |
|---|---|
| Compute | EC2, Lambda, ECS, EKS, Fargate |
| Storage | S3, EBS, EFS, Glacier |
| Networking | VPC, Route 53, CloudFront, Direct Connect, ELB/ALB/NLB |
| Database | RDS, DynamoDB, Aurora, Redshift |
| IAM & Security | IAM, KMS, Secrets Manager, GuardDuty, Security Hub |
| Monitoring | CloudWatch, CloudTrail, X-Ray |
| IaC & Automation | CloudFormation, CDK, Systems Manager |
| Common Certifications | AWS Certified Solutions Architect (Associate/Professional), AWS Certified DevOps Engineer Professional, AWS Certified SysOps Administrator |

### Microsoft Azure
Widely adopted in enterprises already invested in the Microsoft ecosystem (Windows Server, Active Directory, Office 365).

| Category | Key Services |
|---|---|
| Compute | Virtual Machines, Azure Functions, AKS (Azure Kubernetes Service), App Service |
| Storage | Blob Storage, Azure Files, Disk Storage |
| Networking | Virtual Network (VNet), Azure DNS, Front Door, ExpressRoute, Load Balancer |
| Database | Azure SQL Database, Cosmos DB, Azure Database for PostgreSQL/MySQL |
| IAM & Security | Azure Active Directory (Entra ID), Key Vault, Defender for Cloud |
| Monitoring | Azure Monitor, Log Analytics, Application Insights |
| IaC & Automation | ARM Templates, Bicep, Azure DevOps |
| Common Certifications | AZ-104 (Administrator), AZ-305 (Solutions Architect Expert), AZ-400 (DevOps Engineer Expert) |

### Google Cloud Platform (GCP)
Known for strengths in data analytics, machine learning, and Kubernetes (GCP created and open-sourced Kubernetes).

| Category | Key Services |
|---|---|
| Compute | Compute Engine, Cloud Functions, GKE (Google Kubernetes Engine), Cloud Run |
| Storage | Cloud Storage, Persistent Disk, Filestore |
| Networking | VPC, Cloud DNS, Cloud CDN, Cloud Load Balancing, Cloud Interconnect |
| Database | Cloud SQL, Firestore, Bigtable, BigQuery |
| IAM & Security | Cloud IAM, Cloud KMS, Secret Manager, Security Command Center |
| Monitoring | Cloud Monitoring, Cloud Logging (formerly Stackdriver), Cloud Trace |
| IaC & Automation | Deployment Manager, Config Connector |
| Common Certifications | Google Associate Cloud Engineer, Google Professional Cloud Architect, Google Professional Cloud DevOps Engineer |

### How the Roles Use Each Platform
- **Jr./Cloud Engineers** typically work hands-on inside one platform's console/CLI to provision and manage resources (EC2/VM instances, S3/Blob/Cloud Storage buckets, VPC/VNet networking).
- **Sr. Cloud Engineers** design multi-service architectures within a platform and start comparing tradeoffs across platforms (e.g., EKS vs. AKS vs. GKE for container orchestration).
- **Lead Cloud Engineers / Cloud Architects** often work across **multiple platforms** (multi-cloud or hybrid-cloud strategy), deciding which provider is best suited for a given workload based on cost, compliance, and existing investments.

---

## Core DevOps Toolchain

DevOps Engineers use a consistent set of tool categories across the software delivery lifecycle — from writing code to running it reliably in production. Below is the toolchain referenced throughout this document, grouped by purpose.

### 1. Version Control — Git
- **Purpose:** Tracks code changes, enables collaboration, and is the foundation for all CI/CD automation.
- **Platforms:** GitHub, GitLab, Bitbucket.
- **Used by:** All levels — from Jr. Engineers learning branching/PR workflows to Architects defining branching strategy and GitOps standards org-wide.
- **Key concepts:** branching (feature/release/hotfix), pull/merge requests, code review, tagging/releases.

### 2. Continuous Integration / Continuous Deployment (CI/CD) — Jenkins (and equivalents)
- **Purpose:** Automates building, testing, and deploying code whenever changes are pushed, removing manual release steps.
- **Tools:** Jenkins, GitLab CI/CD, GitHub Actions, Azure DevOps Pipelines, CircleCI, Argo CD (GitOps-style CD).
- **Used by:** DevOps Engineers build and maintain pipelines; Sr./Lead DevOps Engineers architect pipeline strategy (parallelization, caching, approval gates, canary/blue-green releases).
- **Key concepts:** pipeline-as-code (Jenkinsfile/YAML), build stages, automated testing gates, artifact promotion across environments.

### 3. Configuration Management — Ansible (and equivalents)
- **Purpose:** Automates server/OS-level configuration so environments are consistent, repeatable, and not manually configured ("snowflake servers").
- **Tools:** Ansible, Chef, Puppet, SaltStack.
- **Used by:** DevOps Engineers write and maintain playbooks/roles; Sr. Engineers design reusable, idempotent configuration standards across environments.
- **Key concepts:** playbooks, roles, idempotency, inventory management, agentless architecture (Ansible's key differentiator).

### 4. Infrastructure as Code (IaC) — Terraform (and equivalents)
- **Purpose:** Defines and provisions cloud infrastructure (networks, compute, storage, IAM) through version-controlled code instead of manual console clicks.
- **Tools:** Terraform, Pulumi, AWS CloudFormation, Azure ARM/Bicep, Google Deployment Manager.
- **Used by:** Cloud Engineers write and maintain Terraform modules; Sr./Lead Cloud Engineers and Architects define module standards, state management strategy, and multi-environment/multi-account patterns.
- **Key concepts:** declarative configuration, state files, modules, providers, plan/apply workflow.

### 5. Containerization — Docker
- **Purpose:** Packages an application with all its dependencies into a portable, consistent unit that runs identically across environments.
- **Used by:** All DevOps levels — from Jr. Engineers writing basic Dockerfiles to Sr. Engineers optimizing multi-stage builds and image security.
- **Key concepts:** images, containers, Dockerfiles, layers, registries (Docker Hub, ECR, ACR, GCR/Artifact Registry).

### 6. Container Orchestration — Kubernetes (K8s)
- **Purpose:** Manages the deployment, scaling, networking, and self-healing of containerized applications across a cluster of machines.
- **Managed offerings:** Amazon EKS, Azure AKS, Google GKE.
- **Used by:** DevOps Engineers deploy and manage workloads; Sr. DevOps Engineers design advanced patterns (Helm charts, custom operators, service mesh with Istio/Linkerd); Architects define cluster strategy at the organizational level.
- **Key concepts:** pods, deployments, services, ConfigMaps/Secrets, namespaces, autoscaling (HPA/VPA), Helm (package manager for K8s).

### 7. Monitoring, Logging & Observability
- **Purpose:** Provides visibility into system health, performance, and errors so issues are detected and resolved quickly — often before users notice.
- **Tools:**
  - **Metrics:** Prometheus, Grafana, Datadog, New Relic, CloudWatch, Azure Monitor, Google Cloud Monitoring.
  - **Logging:** ELK/EFK Stack (Elasticsearch, Logstash/Fluentd, Kibana), CloudWatch Logs, Azure Log Analytics, Google Cloud Logging.
  - **Tracing:** Jaeger, AWS X-Ray, OpenTelemetry.
  - **Alerting/Incident Response:** PagerDuty, Opsgenie.
- **Used by:** All levels — Jr. Engineers monitor dashboards, DevOps/Cloud Engineers configure alerts, Sr. Engineers design full observability strategy (the "three pillars": metrics, logs, traces), Leads/Architects define SLOs/SLIs and error budgets.

### 8. Security & Secrets Management (DevSecOps)
- **Purpose:** Embeds security checks and credential protection directly into the delivery pipeline rather than treating security as an afterthought.
- **Tools:** HashiCorp Vault, AWS Secrets Manager, Azure Key Vault, SonarQube (code quality/SAST), Trivy/Snyk (container/dependency scanning).
- **Used by:** Sr. DevOps Engineers and above typically own DevSecOps integration into pipelines.

### Toolchain-to-Role Quick Reference

| Tool Category | Jr. Level | Mid Level | Sr./Lead/Architect Level |
|---|---|---|---|
| Git | Learns branching/PRs | Manages branching strategy for a project | Defines org-wide Git/GitOps standards |
| Jenkins/CI-CD | Runs existing pipelines | Builds new pipelines | Architects pipeline strategy at scale |
| Ansible | Assists with playbooks | Writes/maintains playbooks | Designs reusable configuration standards |
| Terraform | Learns basics | Writes modules for real environments | Owns state strategy & module governance |
| Docker | Writes basic Dockerfiles | Optimizes images, manages registries | Sets image/security standards org-wide |
| Kubernetes | Learns pod/deployment basics | Deploys & manages workloads | Designs cluster strategy, service mesh, multi-cluster architecture |
| Monitoring Tools | Watches dashboards | Configures alerts/dashboards | Defines SLOs, observability strategy, incident process |

---

## Cloud Engineering Track

### 1. Jr. Cloud Engineer

**Experience Level:** 0–2 years

**Overview:**
This is the entry point into cloud infrastructure work. The role exists so that experienced engineers aren't burdened with repetitive operational tasks, while giving newcomers safe, supervised exposure to real cloud environments. A Jr. Cloud Engineer is not expected to make architectural decisions — their job is to execute clearly-defined tasks correctly, ask good questions, and build foundational muscle memory with cloud platforms.

**Responsibilities:**
- Assist in provisioning and configuring basic cloud resources (VMs, storage buckets, networking) under guidance.
- Monitor cloud infrastructure using dashboards and alerting tools, and escalate anomalies.
- Perform routine tasks such as backups, snapshots, and basic troubleshooting.
- Document infrastructure configurations and standard operating procedures (SOPs).
- Support ticket resolution for infrastructure-related incidents (L1 support).
- Learn and apply cloud provider best practices (AWS/Azure/GCP).
- Assist senior engineers with deployment tasks and testing.

**A Typical Day/Week Looks Like:**
- Checking overnight alerts/dashboards for anomalies and reporting findings.
- Spinning up or resizing a VM/storage bucket per a ticket, following a documented runbook.
- Shadowing a Cloud Engineer during a deployment to learn the process.
- Updating internal documentation after completing a task.
- Attending stand-ups and asking clarifying questions on assigned tickets.

**Required Skills & Knowledge:**
- Basic understanding of at least one cloud platform (AWS, Azure, or GCP).
- Fundamentals of networking (DNS, VPC/VNet, subnets, firewalls, IP addressing).
- Basic scripting (Bash/Python) to automate small repetitive tasks.
- Familiarity with Linux/Windows server administration (users, permissions, processes).
- Understanding of virtualization concepts (hypervisors, VMs, snapshots).

**Tools Commonly Used:** AWS/Azure/GCP Console, basic CLI tools (AWS CLI, Azure CLI), ticketing systems (Jira, ServiceNow), Git basics.

**How to Grow Into the Next Level:**
- Demonstrate you can independently complete provisioning tasks without step-by-step supervision.
- Start writing simple automation scripts instead of doing tasks manually.
- Show reliability — tickets closed accurately and on time, minimal rework.
- Begin learning Infrastructure as Code (Terraform basics) and containers (Docker basics).

---

### 2. Cloud Engineer

**Experience Level:** 2–4 years

**Overview:**
The Cloud Engineer is the backbone of day-to-day infrastructure delivery. Unlike the Jr. role, they don't need hand-holding — they're trusted to take a requirement, design a reasonable implementation, and deliver it using Infrastructure as Code. This role exists to translate application/business needs into working, reliable cloud infrastructure.

**Responsibilities:**
- Design, deploy, and manage cloud infrastructure (compute, storage, networking, databases).
- Implement Infrastructure as Code (IaC) using Terraform/CloudFormation instead of manual console changes.
- Configure and maintain identity and access management (IAM) policies following least-privilege principles.
- Set up monitoring, logging, and alerting solutions for the systems they own.
- Optimize cloud costs and resource utilization (rightsizing, reserved instances).
- Troubleshoot infrastructure issues independently, escalating only complex/ambiguous problems.
- Collaborate with development teams to support application deployments.
- Ensure basic security compliance (encryption at rest/in transit, security groups, patch management).

**A Typical Day/Week Looks Like:**
- Writing/updating Terraform modules to provision a new environment for a dev team.
- Investigating a cost spike flagged by the billing dashboard and identifying the root cause.
- Reviewing a pull request for an infrastructure change from a teammate.
- Setting up a new CloudWatch/Azure Monitor alert for a service that recently had an incident.
- Pairing with an application developer to debug a deployment failure.

**Required Skills & Knowledge:**
- Strong hands-on experience with a major cloud provider (certifications like AWS Associate/Azure Administrator are common at this stage).
- Proficiency in IaC tools (Terraform, CloudFormation, ARM templates).
- Scripting/automation skills (Python, Bash, PowerShell) beyond simple one-off scripts.
- Understanding of containers (Docker) and orchestration basics (Kubernetes).
- Knowledge of cloud networking, security groups, load balancers, and VPNs.
- Basic CI/CD pipeline knowledge (how infrastructure fits into deployment pipelines).

**Tools Commonly Used:** Terraform, Docker, CloudWatch/Azure Monitor/Stackdriver, Git, Jenkins/GitLab CI, AWS/Azure cost management tools.

**How to Grow Into the Next Level:**
- Take ownership of a whole system/environment rather than individual tasks.
- Start proposing architectural improvements, not just implementing what's asked.
- Build expertise in high availability, disaster recovery, and security hardening.
- Begin mentoring Jr. Cloud Engineers informally.

---

### 3. Sr. Cloud Engineer

**Experience Level:** 4–7 years

**Overview:**
The Sr. Cloud Engineer is where technical depth turns into technical judgment. This role exists to own the hard problems — migrations, scaling, security posture, disaster recovery — that require weighing tradeoffs rather than following a runbook. They are also expected to multiply their impact by mentoring others and improving how the whole team works, not just their own output.

**Responsibilities:**
- Architect and implement scalable, highly available, and fault-tolerant cloud solutions.
- Lead migration projects (on-premise to cloud, or cloud-to-cloud) including planning, risk assessment, and execution.
- Define and enforce cloud governance, security, and compliance standards.
- Optimize infrastructure performance, reliability, and cost at scale (across many services/accounts).
- Mentor Jr. and mid-level Cloud Engineers through pairing, code reviews, and knowledge-sharing sessions.
- Conduct architecture and code reviews for infrastructure changes.
- Design disaster recovery (DR) and business continuity (BCP) strategies, including RTO/RPO targets.
- Collaborate with cross-functional teams (security, DevOps, application teams) on end-to-end solutions.
- Evaluate and recommend new cloud services/tools.

**A Typical Day/Week Looks Like:**
- Designing the target architecture for a legacy application migration, including a rollback plan.
- Leading an incident retrospective and driving the resulting action items.
- Reviewing another engineer's Terraform module for security and scalability issues before merge.
- Presenting a cost-optimization proposal (e.g., savings plans, autoscaling policy) to leadership.
- Running a brown-bag session teaching the team about a new AWS/Azure service.

**Required Skills & Knowledge:**
- Deep expertise in multi-service cloud architecture (compute, storage, database, networking) — typically holds a Professional-level certification.
- Advanced IaC and automation skills, including reusable module design.
- Strong understanding of security best practices (IAM, encryption, compliance frameworks like ISO 27001, SOC2, HIPAA).
- Experience with multi-region/multi-cloud setups.
- Strong troubleshooting and performance tuning capabilities under production pressure.
- Leadership and mentoring capabilities — the ability to explain "why," not just "what."

**Tools Commonly Used:** Terraform/Pulumi, Kubernetes, Prometheus/Grafana, AWS Well-Architected Tool, Azure/GCP equivalents, cost tools (CloudHealth).

**How to Grow Into the Next Level:**
- Show you can influence multiple teams' technical direction, not just your own project.
- Demonstrate people-management aptitude — coaching, delegating, giving feedback.
- Start owning budget/roadmap-level conversations, not just technical execution.
- Build a track record of successful large-scale initiatives delivered on time.

---

### 4. Lead Cloud Engineer

**Experience Level:** 7–10 years

**Overview:**
The Lead Cloud Engineer role is a pivot point from "doing the work" to "ensuring the work gets done well by a team." This person balances hands-on technical credibility with people leadership. They exist to make sure the team is working on the right things, in the right way, and that engineers are growing. They are accountable for outcomes delivered by others, not just their own code.

**Responsibilities:**
- Lead and manage a team of Cloud Engineers, assigning tasks and reviewing deliverables.
- Own the technical roadmap for cloud infrastructure projects.
- Drive standardization of cloud practices, tooling, and governance across teams.
- Partner with stakeholders (product, security, finance) to align cloud strategy with business goals.
- Oversee large-scale infrastructure projects from planning to execution.
- Establish KPIs and SLAs for infrastructure reliability and performance.
- Conduct capacity planning and long-term cost forecasting.
- Drive incident management processes and post-mortem reviews.
- Ensure team skill development through training and mentorship.

**A Typical Day/Week Looks Like:**
- 1:1s with team members to discuss career growth and blockers.
- Reviewing quarterly cloud spend against forecast and adjusting plans.
- Facilitating a planning session to prioritize the next quarter's infrastructure initiatives.
- Escalation point for a major incident — coordinating response across teams.
- Presenting infrastructure roadmap and risk assessment to senior leadership.

**Required Skills & Knowledge:**
- Proven leadership and team management experience (formal or informal).
- Strong stakeholder communication and project management skills.
- Expert-level knowledge of cloud architecture, security, and cost optimization.
- Experience in setting technical direction and driving adoption of best practices across teams.
- Budget management and vendor negotiation experience (enterprise cloud contracts).

**Tools Commonly Used:** Enterprise cloud management platforms, FinOps tools (CloudHealth, Cloudability), project management tools (Jira, Confluence, Asana).

**How to Grow Into the Next Level:**
- Shift focus from team-level execution to enterprise-level strategy.
- Build credibility with executive stakeholders (CTO/VP Engineering level).
- Demonstrate ability to design architecture that spans the entire organization, not one team.
- Take ownership of cloud governance and standards at a company-wide scale.

---

### 5. Cloud Architect

**Experience Level:** 10+ years

**Overview:**
The Cloud Architect is the highest technical authority on cloud strategy in the organization. This role exists because, at scale, technical decisions (which cloud provider, which architecture patterns, how much to centralize vs. decentralize) have enormous long-term cost and risk implications — someone needs to own that vision end-to-end and defend it to the business. Cloud Architects usually don't manage people directly; their influence comes from technical authority and cross-organizational relationships.

**Responsibilities:**
- Define enterprise cloud strategy, architecture standards, and best practices.
- Design complex, multi-cloud/hybrid-cloud architectures aligned with business objectives.
- Own the overall security, compliance, and governance framework for cloud environments.
- Evaluate emerging cloud technologies and drive innovation initiatives.
- Provide architectural guidance and approval for major projects across the organization.
- Collaborate with C-level executives and business leaders on technology investments.
- Lead Cloud Center of Excellence (CCoE) initiatives.
- Define disaster recovery, business continuity, and high-availability architecture at an organizational level.
- Act as the final escalation point for critical architectural decisions.
- **Create and own High-Level Design (HLD) diagrams** — overall system architecture showing major components, data flow, integration points, and how they map onto cloud services (e.g., how a new platform's compute, network, database, and security layers fit together).
- **Create and review Low-Level Design (LLD) diagrams** — detailed technical design for individual components, including VPC/subnet layouts, IAM role/policy structures, resource sizing, failover mechanisms, and specific service configurations that engineers will implement from.
- Maintain architecture diagrams as living documentation (using tools like Lucidchart, draw.io, Visio, or diagrams-as-code like Structurizr/PlantUML) and keep them synced with the actual deployed environment.
- Present HLD/LLD diagrams to stakeholders (engineering teams, security, leadership) for review, feedback, and sign-off before implementation begins.

**A Typical Day/Week Looks Like:**
- Reviewing and approving the architecture for a new business-critical platform.
- Drawing a High-Level Design diagram for a new multi-region deployment, showing traffic flow from CDN → load balancer → application tier → database tier.
- Breaking an approved HLD into Low-Level Design diagrams for individual teams — e.g., detailed VPC/subnet CIDR layout, security group rules, and IAM role structure.
- Meeting with the CTO to discuss a potential multi-cloud strategy shift.
- Chairing a CCoE meeting to ratify a new company-wide Terraform module standard.
- Evaluating a vendor's new managed service for potential adoption.
- Writing/reviewing a whitepaper on the company's cloud security posture for an audit.

**Required Skills & Knowledge:**
- Mastery of cloud architecture across multiple providers (AWS, Azure, GCP).
- Deep understanding of enterprise security, compliance, and risk management.
- Strong business acumen — ability to translate business needs into technical strategy and vice versa.
- Excellent communication skills for engaging with executive leadership and non-technical stakeholders.
- Certifications such as AWS Solutions Architect Professional, Azure Solutions Architect Expert, or Google Professional Cloud Architect are typically expected.

**Tools Commonly Used:** Enterprise architecture frameworks (TOGAF), multi-cloud management platforms, FinOps and governance tooling, diagramming/design tools (Lucidchart, draw.io, Microsoft Visio, Structurizr/PlantUML for diagrams-as-code).

**Note:** This is typically the terminal individual-contributor (IC) role in the cloud track. Further growth usually branches into Principal Architect, Distinguished Engineer, or Director/VP of Infrastructure (management track).

---

## DevOps Engineering Track

### 1. Jr. DevOps Engineer

**Experience Level:** 0–2 years

**Overview:**
This role exists to give newcomers hands-on exposure to the software delivery lifecycle — build, test, release — under close supervision. The focus is on learning tools and conventions, not making pipeline design decisions.

**Responsibilities:**
- Assist in maintaining CI/CD pipelines under supervision.
- Perform routine build, test, and deployment tasks.
- Monitor application and infrastructure logs for basic issues.
- Support version control practices (Git branching, merging, resolving simple conflicts).
- Help maintain configuration management scripts.
- Document deployment processes and troubleshooting steps.
- Learn containerization basics (Docker).

**A Typical Day/Week Looks Like:**
- Triggering and monitoring a build pipeline, reporting failures to a senior engineer.
- Writing a Dockerfile for a simple internal tool with guidance.
- Fixing a broken link or outdated step in deployment documentation.
- Helping merge a feature branch and resolving a minor merge conflict.
- Watching a Sr. DevOps Engineer debug a failed deployment and taking notes.

**Required Skills & Knowledge:**
- Basic knowledge of Git and version control workflows (branching, PRs).
- Fundamentals of Linux/Unix systems (file permissions, processes, package managers).
- Basic scripting (Bash/Python).
- Understanding of CI/CD concepts (build, test, deploy stages).
- Basic knowledge of Docker (images, containers, Dockerfiles).

**Tools Commonly Used:** Git, Jenkins/GitLab CI/GitHub Actions (basic use), Docker, Linux CLI.

**How to Grow Into the Next Level:**
- Start writing/modifying pipeline configuration yourself instead of just running existing pipelines.
- Build comfort with containerization beyond "Hello World" examples.
- Demonstrate you can troubleshoot common pipeline failures independently.

---

### 2. DevOps Engineer

**Experience Level:** 2–4 years

**Overview:**
The DevOps Engineer owns the automation and delivery pipeline for a set of applications. This role exists to remove friction between "code is written" and "code is running safely in production," and to make that process fast, repeatable, and observable.

**Responsibilities:**
- Design, build, and maintain CI/CD pipelines for multiple applications.
- Automate infrastructure provisioning using IaC tools.
- Implement containerization and orchestration (Docker, Kubernetes).
- Configure monitoring, logging, and alerting for applications and infrastructure.
- Collaborate with development teams to streamline build and release processes.
- Troubleshoot pipeline failures and deployment issues.
- Implement configuration management (Ansible, Chef, Puppet).
- Support on-call rotations for production incidents.

**A Typical Day/Week Looks Like:**
- Building a new CI/CD pipeline for a microservice, including automated testing gates.
- Debugging why a Kubernetes deployment is stuck in `CrashLoopBackOff`.
- Writing an Ansible playbook to standardize server configuration across environments.
- Being paged for a production alert and walking through triage steps.
- Reviewing dashboards to identify a service with degrading latency.

**Required Skills & Knowledge:**
- Strong scripting and automation skills (Python, Bash, Groovy).
- Hands-on experience with CI/CD tools (Jenkins, GitLab CI, GitHub Actions, Azure DevOps).
- Working knowledge of Docker and Kubernetes (deployments, services, ConfigMaps, Secrets).
- Experience with configuration management tools.
- Understanding of cloud platforms and basic networking.
- Familiarity with monitoring tools (Prometheus, Grafana, ELK stack).

**Tools Commonly Used:** Jenkins, Docker, Kubernetes, Ansible, Terraform, Prometheus/Grafana, ELK/EFK stack.

**How to Grow Into the Next Level:**
- Move from maintaining pipelines to architecting them for scale and resilience.
- Learn GitOps and advanced Kubernetes patterns (Helm, operators).
- Take ownership of incident response and root-cause analysis, not just triage.
- Start integrating security scanning into the pipeline (DevSecOps mindset).

---

### 3. Sr. DevOps Engineer

**Experience Level:** 4–7 years

**Overview:**
The Sr. DevOps Engineer owns the reliability and scalability of the entire delivery platform, not just individual pipelines. This role exists to prevent outages before they happen, design deployment strategies that minimize risk, and embed security into every stage of delivery (DevSecOps).

**Responsibilities:**
- Architect scalable and resilient CI/CD pipelines for enterprise applications.
- Design and implement advanced Kubernetes deployments (Helm charts, operators, service mesh).
- Drive Infrastructure as Code adoption and GitOps practices.
- Implement robust monitoring, observability, and incident response strategies.
- Lead root cause analysis for critical production incidents.
- Establish security best practices in the DevOps pipeline (DevSecOps — SAST/DAST, secrets management).
- Mentor Jr. and mid-level DevOps Engineers.
- Optimize deployment strategies (blue-green, canary, rolling deployments).
- Collaborate with Cloud and Security teams on infrastructure hardening.

**A Typical Day/Week Looks Like:**
- Designing a canary deployment strategy for a high-traffic service to reduce release risk.
- Leading a post-incident review for a major outage and driving preventive action items.
- Integrating a secrets manager (Vault) into the deployment pipeline to remove hardcoded credentials.
- Setting up a service mesh (Istio) to improve traffic control and observability.
- Mentoring a DevOps Engineer through designing their first production-grade pipeline.

**Required Skills & Knowledge:**
- Advanced expertise in CI/CD tooling and pipeline architecture.
- Deep knowledge of Kubernetes, Helm, and service mesh (Istio/Linkerd).
- Strong IaC and GitOps experience (Terraform, ArgoCD, Flux).
- Understanding of DevSecOps practices and tools (Vault, SonarQube, Trivy).
- Strong troubleshooting and incident management skills under pressure.
- Leadership and mentoring capabilities.

**Tools Commonly Used:** Kubernetes, Helm, ArgoCD/Flux, Vault, SonarQube, Terraform, Prometheus/Grafana, PagerDuty.

**How to Grow Into the Next Level:**
- Demonstrate ability to set direction for a whole team's tooling and process, not just your own projects.
- Build people-leadership skills — coaching, delegation, performance feedback.
- Start engaging with budget, vendor, and roadmap decisions.
- Show consistent delivery of large cross-team initiatives.

---

### 4. Lead DevOps Engineer

**Experience Level:** 7–10 years

**Overview:**
The Lead DevOps Engineer balances technical ownership with people management. This role exists to ensure the DevOps function scales with the organization — that tooling, processes, and team capability all grow together, and that reliability doesn't degrade as the company adds more services and engineers.

**Responsibilities:**
- Lead and manage a team of DevOps Engineers, assigning work and conducting performance reviews.
- Own the DevOps roadmap, including tooling standardization and process improvement.
- Drive adoption of DevOps culture and practices across engineering teams.
- Define and monitor SLAs/SLOs for deployment reliability and system uptime.
- Oversee incident management processes, conduct post-mortems, and drive continuous improvement.
- Collaborate with Cloud Architects and Engineering Managers on infrastructure strategy.
- Manage toolchain evaluation, licensing, and budget for DevOps tools.
- Establish disaster recovery and business continuity processes for CI/CD systems.
- Drive automation-first culture to reduce manual operational overhead.

**A Typical Day/Week Looks Like:**
- Reviewing team SLO/error-budget reports and deciding whether to prioritize reliability work over features.
- Negotiating a renewal/upgrade with a CI/CD or observability vendor.
- Running a hiring interview loop for a new DevOps Engineer.
- Presenting a proposal to standardize deployment tooling across five product teams.
- Coordinating a major incident bridge call and ensuring communication to stakeholders.

**Required Skills & Knowledge:**
- Proven leadership and cross-team collaboration experience.
- Deep technical expertise across CI/CD, containerization, and cloud infrastructure.
- Strong understanding of DevSecOps, compliance, and governance.
- Project and program management skills.
- Excellent communication skills for engaging with technical and non-technical stakeholders.

**Tools Commonly Used:** Enterprise CI/CD platforms, Kubernetes at scale, observability platforms (Datadog, New Relic), project management tools.

**How to Grow Into the Next Level:**
- Move from team-level ownership to enterprise-wide platform strategy.
- Build relationships with executive stakeholders and represent DevOps in company-wide planning.
- Champion platform engineering / internal developer platform initiatives.
- Demonstrate the ability to define technology direction that spans multiple teams and years.

---

### 5. DevOps Architect

**Experience Level:** 10+ years

**Overview:**
The DevOps Architect is the highest technical authority on delivery and automation strategy in the organization. This role exists to ensure that as the company scales, every team can ship software quickly *and* safely — by designing the platforms, golden paths, and standards that make good practices the path of least resistance.

**Responsibilities:**
- Define enterprise-wide DevOps strategy, standards, and best practices.
- Architect end-to-end CI/CD ecosystems spanning multiple teams, products, and cloud environments.
- Drive DevSecOps and compliance-by-design across the organization.
- Evaluate and introduce emerging tools/technologies (platform engineering, internal developer platforms).
- Lead organizational transformation initiatives (DevOps culture, SRE practices).
- Partner with executive leadership to align automation strategy with business objectives.
- Own the architecture for observability, incident management, and reliability engineering at scale.
- Act as the final escalation and decision-making authority for DevOps architecture.
- Define golden paths/templates for engineering teams to ensure consistency and speed.
- **Create and own High-Level Design (HLD) diagrams** for CI/CD ecosystems and platform architecture — showing how source control, build systems, artifact repositories, deployment pipelines, and environments (dev/staging/prod) connect end-to-end.
- **Create and review Low-Level Design (LLD) diagrams** for individual pipeline and platform components — e.g., detailed Kubernetes cluster topology, service mesh traffic rules, pipeline stage-by-stage flow, or secrets-management integration points.
- Maintain pipeline/platform architecture diagrams as living documentation, kept in sync with actual tooling using diagrams-as-code where possible.
- Present HLD/LLD diagrams during design reviews to get sign-off from engineering leads, security, and platform teams before rollout.

**A Typical Day/Week Looks Like:**
- Designing the blueprint (High-Level Design) for a company-wide internal developer platform (IDP).
- Drilling that blueprint down into Low-Level Design diagrams for individual squads — e.g., exact pipeline stages, approval gates, and rollback mechanisms for a given service type.
- Presenting a business case to leadership for investing in platform engineering.
- Reviewing a proposed architecture change to the company's incident response system.
- Setting the standard for how all teams handle secrets management and compliance scanning.
- Mentoring Lead DevOps Engineers on architectural decision-making.

**Required Skills & Knowledge:**
- Mastery of DevOps, CI/CD, containerization, and cloud-native architecture.
- Strong understanding of Site Reliability Engineering (SRE) principles.
- Deep knowledge of security, compliance, and governance frameworks.
- Strong business acumen and executive communication skills.
- Experience building internal developer platforms (IDPs) and platform engineering initiatives.
- Relevant certifications (CKA/CKAD, AWS/Azure DevOps Engineer Expert) are typically expected.

**Tools Commonly Used:** Backstage (IDP), Kubernetes, ArgoCD, service mesh, enterprise observability suites, FinOps/governance platforms, diagramming/design tools (Lucidchart, draw.io, Structurizr/PlantUML for diagrams-as-code).

**Note:** Like the Cloud Architect, this is typically a terminal IC role. Further growth branches into Principal/Distinguished Engineer or Director/VP of Platform Engineering (management track).

---

## Cloud vs DevOps: What's the Difference?

These two tracks overlap heavily in practice, but their core focus differs:

| Aspect | Cloud Engineering | DevOps Engineering |
|---|---|---|
| **Primary focus** | The infrastructure itself — compute, storage, networking, security | The *flow* of code from commit to production |
| **Core question they answer** | "Where and how does this run, reliably and cost-effectively?" | "How do we get changes into production quickly and safely?" |
| **Key artifacts owned** | VPCs, IAM policies, Terraform for infra, DR/BCP plans | CI/CD pipelines, deployment strategies, GitOps workflows |
| **Typical escalation for** | Outages caused by infrastructure capacity, networking, or cloud provider issues | Failed deployments, broken pipelines, release rollbacks |
| **Overlap** | Both use IaC, Kubernetes, monitoring tools, and collaborate closely — in smaller organizations these roles often merge into one combined "Cloud/DevOps Engineer" |

---

## Career Progression Summary

| Level | Cloud Track | DevOps Track | Focus |
|-------|-------------|---------------|-------|
| 1 | Jr. Cloud Engineer | Jr. DevOps Engineer | Learning & supervised execution |
| 2 | Cloud Engineer | DevOps Engineer | Independent implementation |
| 3 | Sr. Cloud Engineer | Sr. DevOps Engineer | Ownership & mentorship |
| 4 | Lead Cloud Engineer | Lead DevOps Engineer | Team leadership & strategy |
| 5 | Cloud Architect | DevOps Architect | Enterprise vision & innovation |

**General Progression Themes:**
- **Levels 1–2:** Building technical foundation and hands-on execution skills. Success is measured by *task completion quality and speed*.
- **Level 3:** Transitioning from execution to ownership, architecture, and mentoring. Success is measured by *the quality of decisions and the growth of people around you*.
- **Level 4:** Shifting focus to team leadership, process, and cross-functional collaboration. Success is measured by *team output and organizational alignment*.
- **Level 5:** Operating at an enterprise/strategic level, influencing organization-wide technology decisions. Success is measured by *long-term technical strategy and business impact*.

---

## How Promotion Decisions Are Typically Made

Organizations generally evaluate four dimensions when considering someone for the next level:

1. **Scope** — Is the person already operating at the scope of the next level (e.g., a Cloud Engineer already making architecture-level decisions)?
2. **Consistency** — Have they demonstrated this capability repeatedly, not just once?
3. **Impact** — Does their work measurably improve reliability, cost, velocity, or security?
4. **Influence** — Do others (peers, other teams, leadership) already look to them for guidance at that level?

A good rule of thumb: **promotion follows demonstrated behavior, not the reverse.** Engineers are usually already doing next-level work before the title changes.
