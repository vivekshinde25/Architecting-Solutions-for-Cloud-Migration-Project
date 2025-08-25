# 🚀 Insurance Company Cloud Migration Project

## 📖 Introduction
This project focuses on migrating an insurance company’s applications and databases from on-premises data centers to **AWS Cloud**. The migration plan ensures security, scalability, and reliability while supporting hybrid connectivity during the transition period.

## 🎯 Customer Requirements
The insurance company needed:
- 🖥️ **App Migration** – Shift containerized applications without rewriting.  
- 🗄️ **Database Migration** – Move PostgreSQL databases securely.  
- ⏳ **Phased Migration** – Migrate half of workloads now, rest later (due to contract expiry).  
- 🔗 **Hybrid Setup** – Maintain on-prem + AWS connectivity during transition.  
- 🛡️ **High Resilience** – Fault tolerance for critical insurance workloads.  
- 📊 **Scalability** – Enable future growth without infrastructure limits.  

## ⚡ Customer Problems
The customer faced several key issues:
- 🏢 **Data Center Expiry** – Contracts ending soon, forcing quick migration decisions.  
- 🔄 **Complex Migration** – Needed to move apps + databases without downtime.  
- 💰 **High Costs** – Running both on-prem and cloud temporarily increased expenses.  
- 🧩 **Integration Issues** – Ensuring seamless connection between on-prem systems and AWS.  
- 🛑 **Risk of Downtime** – Insurance apps require continuous availability.  
- 🔒 **Security Concerns** – Protecting sensitive customer and policyholder data during migration.  

## 🛠️ Problems vs Solutions (Detailed)

### 1️⃣ Data Center Expiry
**Problem:** The customer’s existing data center contracts were about to expire. If they didn’t move quickly, they risked losing access to infrastructure or facing very high renewal costs. A rushed full migration could lead to errors and unplanned downtime.  

**Solution:** They adopted a phased migration strategy on AWS – moving half of the workloads immediately, while planning a structured timeline for the remaining ones.  

**Why:** This gave the business breathing room. Instead of trying to “lift and shift” everything under pressure, they reduced risks, controlled costs, and ensured that critical insurance applications stayed stable during the process.  

---

### 2️⃣ Complex Migration (Apps + Databases)
**Problem:** The environment had containerized applications and PostgreSQL databases that needed to be migrated together. Rebuilding apps or databases from scratch would be time-consuming and risky.  

**Solution:** They chose **Amazon ECS/EKS** for running containers and **Amazon RDS for PostgreSQL** for the database. ECS/EKS handled their container workloads directly, and RDS provided a managed, reliable PostgreSQL service.  

**Why:** This minimized rework. Instead of rewriting or re-architecting applications, they could continue using their containerized setup. RDS reduced operational burden like patching, scaling, and backups, letting the team focus on business instead of infrastructure.  

---

### 3️⃣ High Costs of Dual Infrastructure
**Problem:** During migration, the company had to keep on-prem servers running while also paying for AWS resources. This overlap drove up costs.  

**Solution:** They leveraged **Amazon S3** for cost-effective storage and **ECS/EKS Auto Scaling** so resources only scaled when traffic required it.  

**Why:** Instead of paying for large, always-on infrastructure, they only paid for what they used. S3 offered very cheap and durable storage for backups and logs, and auto scaling meant containers spun up only when workloads demanded, reducing waste.  

---

### 4️⃣ Integration Issues
**Problem:** Some systems had to remain on-prem temporarily, but they still needed to integrate with new AWS-hosted applications. Poor integration could cause data flow interruptions or inconsistent user experiences.  

**Solution:** The team used **AWS Direct Connect** and **VPN** to create a secure hybrid network. Direct Connect offered dedicated high-speed connectivity, while VPN provided encrypted links.  

**Why:** This gave them reliability and security. Instead of depending on the public internet with unpredictable latency, Direct Connect and VPN ensured smooth and secure communication between AWS and on-prem.  

---

### 5️⃣ Risk of Downtime
**Problem:** Insurance applications must run 24/7. Even short downtime could lead to transaction failures, compliance issues, or customer dissatisfaction.  

**Solution:** They used **Multi-AZ deployment in RDS** (databases replicated across availability zones) and **Elastic Load Balancers** for ECS/EKS applications.  

**Why:** If one zone went down, RDS automatically failed over to another. Load balancers distributed traffic across healthy application instances, ensuring apps remained online even if one container failed. This design provided resilience and business continuity.  

---

### 6️⃣ Security Concerns
**Problem:** The company handled sensitive customer data like personal details, policy info, and financial records. Any data leak or breach would cause major trust and compliance issues.  

**Solution:** They implemented **IAM roles** (least-privilege access), **KMS encryption** (for data at rest and in transit), and **Security Groups** (network firewalls at instance level).  

**Why:** This layered approach ensured protection at multiple levels: IAM stopped unnecessary user access, KMS encrypted all sensitive data, and Security Groups restricted traffic only to trusted sources. Together, these controls made the system secure and compliant.  

## 🌐 Hybrid System Workflow (On-Prem + AWS Integration)

### 1️⃣ User Request Flow
- **End Users (Customers/Agents)** → Access the insurance applications (web/mobile).  
- Requests can hit either **on-prem servers (old system)** or **AWS-hosted apps (new system)**, depending on migration progress.  

---

### 2️⃣ Network Connectivity (On-Prem ↔ AWS)
- **VPN** → Provides encrypted communication for sensitive data flows.  
- **AWS Direct Connect** → Delivers low-latency, high-bandwidth connectivity between the on-prem data center and AWS.  
- **Combined Effect** → Both together form a **secure hybrid network**, making on-prem + AWS behave like one private environment.  

---

### 3️⃣ Application Layer (Containers)
- **On-Prem:** Some older container workloads still run in the data center (legacy apps not yet migrated).  
- **AWS ECS/EKS:** Migrated applications run in containers managed by Amazon ECS/EKS.  
- **Elastic Load Balancer (ELB):** Distributes traffic across healthy AWS containers.  
- **Hybrid Handling:** If an AWS app depends on an on-prem system (e.g., claims DB not yet migrated), ECS/EKS securely calls back via Direct Connect/VPN.  

---

### 4️⃣ Database Layer
- **On-Prem Databases:** Some PostgreSQL databases still run locally until phased migration completes.  
- **AWS RDS for PostgreSQL:** Migrated databases run on Multi-AZ RDS for high availability.  
- **Data Sync:** **AWS DMS** keeps on-prem and AWS databases synchronized to prevent inconsistencies.  

---

### 5️⃣ Storage Layer
- **On-Prem Storage:** Legacy file systems continue supporting older applications.  
- **AWS S3:** Central storage hub for logs, backups, and documents (e.g., claims files, scanned documents).  
- **Flow:** On-prem apps can push files directly into S3 via secure links, enabling hybrid storage usage.  

---

### 6️⃣ Security & Access Control
- **AWS IAM Roles:** Control access for apps and users in the cloud.  
- **On-Prem Active Directory (AD):** Manages identity for legacy apps.  
- **Integration:** Federation/SSO ensures employees log in once and securely access both environments.  

---

### 7️⃣ End-to-End Flow Example (Insurance Claim Submission)
1. Customer uploads a **claim document** via the insurance portal.  
2. The **front-end app** runs in ECS/EKS on AWS.  
3. The document is stored in **Amazon S3**.  
4. Claim details are written to **RDS PostgreSQL (AWS)** if migrated, or securely routed to the **on-prem DB** if not yet moved.  
5. If DB is on-prem → data travels over **Direct Connect** (low latency, secure).  
6. An **on-prem fraud check module** (legacy system) may be triggered → sends results back to AWS app.  
7. Customer immediately sees **claim status updated**, even though parts of the workflow spanned AWS + On-Prem.  

---

### ✅ In Short
- **AWS** → Scalability, storage, and new app deployments.  
- **On-Prem** → Legacy apps and databases not yet migrated.  
- **Direct Connect/VPN** → Stitches both environments together securely.  
- **User Experience** → Seamless and unified, despite hybrid backend.  

## 🌐 Hybrid System Detailed Workflow (With Services)

### 1️⃣ User Access Layer
**Actor:** Customers / Agents (via Web/Mobile portal)  

**Services involved:**
- **Amazon Route 53** → DNS resolution for the portal.  
- **AWS WAF (Web Application Firewall)** → Filters malicious traffic.  
- **Elastic Load Balancer (ELB)** → Distributes incoming requests across containers running in ECS/EKS.  

**Flow:**  
User enters insurance portal → DNS (Route 53) → WAF check → Load Balancer routes request → App container.  

---

### 2️⃣ Application Layer
**Services involved:**
- **Amazon ECS / EKS** → Runs containerized insurance applications.  
- **Amazon EC2 (if needed)** → For legacy workloads lifted into cloud.  
- **On-Prem Servers** → Still host older monolithic apps not migrated yet.  

**Flow:**  
ELB forwards request → Application logic runs in ECS/EKS container → If it needs legacy functionality, securely calls On-Prem servers via VPN/Direct Connect.  

---

### 3️⃣ Networking & Hybrid Connectivity
**Services involved:**
- **AWS Direct Connect** → Low-latency private network between AWS & On-Prem DC.  
- **VPN Gateway (Site-to-Site)** → Backup secure tunnel for encrypted communication.  
- **Amazon VPC** → Provides isolated cloud network environment.  

**Flow:**  
App in ECS → Needs data from On-Prem DB → Route through VPC → Direct Connect (primary) or VPN (backup).  

---

### 4️⃣ Database Layer
**Services involved:**
- **Amazon RDS for PostgreSQL (Multi-AZ)** → Cloud database for migrated apps.  
- **On-Prem PostgreSQL DB** → Still serves legacy apps.  
- **AWS DMS (Database Migration Service)** → Synchronizes On-Prem DB with RDS.  

**Flow:**  
- If DB migrated → ECS/EKS writes directly to RDS.  
- If DB still On-Prem → ECS/EKS writes through Direct Connect → On-Prem PostgreSQL.  
- DMS keeps both in sync during transition.  

---

### 5️⃣ Storage Layer
**Services involved:**
- **Amazon S3** → Stores documents, claim files, backups, logs.  
- **Amazon S3 Glacier** → Archive storage for old insurance documents.  
- **On-Prem Storage (NAS/SAN)** → Still used by legacy apps.  

**Flow:**  
Customer uploads claim document → App stores file in S3 → App stores metadata in RDS or On-Prem DB → On-Prem apps can also push/pull from S3 via Direct Connect.  

---

### 6️⃣ Security & Identity Layer
**Services involved:**
- **AWS IAM** → Access control for AWS apps and services.  
- **AWS KMS** → Encryption for DB and S3.  
- **On-Prem Active Directory (AD)** → Handles identity for legacy apps.  
- **AWS SSO / Federation** → Bridges On-Prem AD with AWS IAM → single login for employees.  

**Flow:**  
Employee logs in → Authentication handled by AD → Federated access to AWS → Works across both environments seamlessly.  

---

### 7️⃣ Monitoring & Reliability
**Services involved:**
- **Amazon CloudWatch** → Logs & metrics for ECS/EKS, RDS.  
- **AWS CloudTrail** → API logging & auditing.  
- **On-Prem Monitoring Tools (Nagios/Splunk)** → Still watching legacy systems.  

**Flow:**  
- Events from AWS → CloudWatch dashboards → Alerts sent to Ops.  
- Events from On-Prem → Fed into central monitoring for unified visibility.  

---

### 📊 End-to-End Example (Insurance Claim Submission)
1. Customer uploads claim form → Route 53 → WAF → ELB → ECS container.  
2. Document saved in **Amazon S3**.  
3. Metadata written to **RDS PostgreSQL (if migrated)** OR **On-Prem PostgreSQL** via Direct Connect.  
4. Fraud detection (still On-Prem app) triggered → Processes data → Sends result back to ECS app.  
5. ECS app updates claim status → Shown instantly to user.  

---

### ✅ In Short
- **AWS** → Scalability, storage, and new app deployments.  
- **On-Prem** → Legacy apps and databases not yet migrated.  
- **Direct Connect/VPN** → Stitches both environments together securely.  
- **User Experience** → One seamless system, even though the backend is hybrid.  


## Architecture Diagram

![App Screenshot](https://github.com/vivekshinde25/Architecting-Solutions-for-Cloud-Migration-Project/blob/4d76ed50501d8c338505600f62afb2fa4e8ed916/Screenshot%202025-08-15%20155410.png)

# 🚀 Solutions Chosen for Customer’s Requirements

## 1️⃣ AWS Direct Connect
- **Customer Requirement:** High-volume, consistent transfer between Data Center ↔ AWS.  
- **Why Chosen:** Dedicated private network → lower latency, reliable throughput vs VPN.  
- **Example:** Like leasing your own highway lane instead of public roads.  

## 2️⃣ Amazon ECS (Cloud)
- **Customer Requirement:** Run and manage containerized insurance applications in AWS.  
- **Why Chosen:** Automates deployment, scaling, and management of containers.  
- **Example:** Like a factory that assembles and monitors apps automatically.  

## 3️⃣ Amazon ECS Anywhere (On-Premises)
- **Customer Requirement:** Retain some containerized workloads on-premises but manage them with the same AWS tools.  
- **Why Chosen:** Extends ECS management to on-prem servers, keeping a single control plane.  
- **Example:** Like controlling both your home kitchen and a restaurant with the same recipes.  

## 4️⃣ Amazon ECR (Elastic Container Registry)
- **Customer Requirement:** Private and secure container image storage.  
- **Why Chosen:** Integrated with ECS, more secure than public registries.  
- **Example:** Like moving from a public recipe book to your own private vault.  

## 5️⃣ Amazon EC2 Cluster for ECS
- **Customer Requirement:** Full control over compute environment with custom AMIs and SSH access.  
- **Why Chosen:** EC2-based ECS clusters give flexibility and compatibility with legacy workloads.  
- **Example:** Like choosing your own kitchen appliances instead of renting defaults.  

## 6️⃣ Amazon EC2 Auto Scaling
- **Customer Requirement:** Manage workload spikes efficiently and reduce costs.  
- **Why Chosen:** Automatically scales EC2 instances up or down.  
- **Example:** Like hiring more cooks during dinner rush and letting them go later.  

## 7️⃣ Application Load Balancer
- **Customer Requirement:** Distribute customer traffic reliably across applications.  
- **Why Chosen:** Smart request routing with built-in health checks.  
- **Example:** Like a host directing diners to the right table.  

## 8️⃣ Amazon RDS (PostgreSQL, Multi-AZ)
- **Customer Requirement:** Highly available and reliable relational database.  
- **Why Chosen:** Managed PostgreSQL with automatic failover and backups.  
- **Example:** Like having two synced bank branches—if one closes, the other works.  

## 9️⃣ AWS Systems Manager
- **Customer Requirement:** Unified operations and management for AWS + On-Premises.  
- **Why Chosen:** Provides automation, patching, and monitoring from a single console.  
- **Example:** Like a universal remote for all your devices.  

## 🔟 AWS Backup
- **Customer Requirement:** Centralized, policy-based backups for hybrid infrastructure.  
- **Why Chosen:** Supports AWS and on-prem services under one backup strategy.  
- **Example:** Like one safe storing both home & office documents.  

## 1️⃣1️⃣ AWS Storage Gateway
- **Customer Requirement:** Enable on-prem apps to store files in AWS seamlessly.  
- **Why Chosen:** Provides low-latency local cache while asynchronously syncing to S3.  
- **Example:** Like keeping files on your desk but archiving them in the cloud.  

## 1️⃣2️⃣ Amazon S3
- **Customer Requirement:** Long-term, durable storage for claims documents, logs, and backups.  
- **Why Chosen:** Virtually unlimited capacity with 99.999999999% durability.  
- **Example:** Like an indestructible filing cabinet in the cloud.  

## 1️⃣3️⃣ AWS DMS (Database Migration Service)
- **Customer Requirement:** Migrate databases with minimal downtime and data sync during cutover.  
- **Why Chosen:** Keeps on-prem and cloud DBs in sync until migration is complete.  
- **Example:** Like moving to a new store while keeping the old one open.  

## 1️⃣4️⃣ NAT Gateway
- **Customer Requirement:** Secure outbound internet access for private workloads.  
- **Why Chosen:** Enables instances to reach the internet without allowing inbound traffic.  
- **Example:** Like letting employees shop outside but blocking outsiders from entering.  

---

# 🏗️ How It All Works Together

- **Direct Connect** → The private bridge between Data Center & AWS.  
- **ECS & ECS Anywhere** → Unified container management across both on-prem and cloud.  
- **EC2 & Auto Scaling** → Elastic compute for dynamic workloads.  
- **ALB** → Distributes traffic efficiently.  
- **RDS (Multi-AZ)** → Ensures database resilience and reliability.  
- **Storage Gateway + S3** → Balances low-latency file access with long-term durability.  
- **Systems Manager + Backup** → Centralized operations and protection.  
- **DMS** → Smooth migration with minimal downtime.  
- **NAT Gateway** → Secure, controlled internet access.  

# 💡 Suggestions & Recommendations for Improving Customer’s Architecture

## 1️⃣ Add VPN as Backup for Direct Connect
- **What they said:** Direct Connect is reliable, but if something fails at the corporate data center (like the router), the connection can go down.  
- **Recommendation:** Add a VPN over the internet as a backup.  
- **Why:** Ensures there’s always another way to reach AWS if Direct Connect fails.  
- **Example:** Like having a backup road route (VPN) in case your private highway (Direct Connect) is blocked.  

---

## 2️⃣ Enable RDS Auto Storage Scaling
- **What they said:** RDS doesn’t scale compute automatically, but you can turn on storage auto-scaling.  
- **Recommendation:** Enable RDS Auto Storage Scaling.  
- **Why:** Prevents downtime or manual work when storage runs out—storage grows automatically.  
- **Example:** Like your phone adding more cloud storage when photos fill up, instead of you having to delete files or buy space manually.  

---

## 3️⃣ Use Multi-Region Disaster Recovery (DR) Strategies
- **What they said:** The customer is an enterprise → needs high resiliency. Running in multiple AWS Regions is an option.  
- **Options discussed:**
  - **Active/Active Multi-Site:** Full deployment in 2 Regions at once (most expensive).  
  - **Pilot Light:** Keep a smaller copy running in another Region → scale it up if disaster happens.  
  - **Warm Standby:** Reduced fleet running in another Region.  
- **Why:** Protects against entire Region failure.  
- **Example:** Like running two offices in different cities. One could be full-sized (Active/Active) or a smaller backup office (Pilot Light).  

---

## 4️⃣ Use Infrastructure as Code (IaC) for Multi-Region Setup
- **What they said:** To copy entire architecture into another Region, you need IaC (like CloudFormation or Terraform).  
- **Why:** Makes sure deployments are identical in both Regions.  
- **Example:** Like having detailed blueprints so you can rebuild the exact same house in another city.  

---

## 5️⃣ Enable S3 Lifecycle Policies & Cross-Region Replication
- **What they said:** If using S3 across Regions, costs can rise since data is duplicated.  
- **Recommendation:**
  - **Use Cross-Region Replication (CRR):** Replicate S3 data automatically to another Region.  
  - **Use Lifecycle Policies:** Move old/rarely used data to cheaper storage classes.  
- **Why:** Improves resiliency & saves cost.  
- **Example:** Like keeping active files in your desk drawer but moving old files to a cheap warehouse across the country.  

---

## 6️⃣ Use AWS DMS for Multi-Region Database Replication
- **What they said:** If not using Aurora Global DB, use Database Migration Service (DMS) for continuous replication.  
- **Why:** Keeps data synced across Regions without downtime.  
- **Example:** Like keeping two shops updated with the same inventory system, so stock levels match everywhere.  

---

## 7️⃣ ECS Container Scaling (Beyond EC2 Scaling)
- **What they said:** EC2 Auto Scaling manages VM scaling, but ECS containers have their own scaling mechanisms.  
- **Recommendation:** Configure ECS Service Auto Scaling for container tasks.  
- **Why:** Ensures not just the servers, but also the containers themselves scale based on demand.  
- **Example:** Like hiring more delivery drivers (containers) when orders increase, not just buying more trucks (EC2s).  

---

## 8️⃣ Use RDS Read Replicas Across Regions
- **What they said:** PostgreSQL in RDS supports read replicas across Regions.  
- **Recommendation:** Deploy cross-region read replicas.  
- **Why:** Provides both disaster recovery and performance benefits (read scaling). If needed, replicas can become the main database.  
- **Example:** Like opening branch offices in other cities that can also become HQ if the main office shuts down.  

---

# 📝 Summary
The key improvements focused on:  

- **Resiliency:** Backup VPN, Multi-Region DR, RDS replicas.  
- **Scalability:** RDS auto storage scaling, ECS container scaling.  
- **Cost Optimization:** S3 lifecycle policies + tiering.  
- **Automation:** Infrastructure as Code, continuous replication with DMS.  

✅ Together → these recommendations would take the **customer’s requirements** from **“strong hybrid” → “enterprise-grade, resilient, and future-ready hybrid system.”**

# ✅ Conclusion  

This hybrid cloud migration successfully balanced **speed, cost, and security** for the customer. By leveraging AWS services such as **ECS/EKS for containers, RDS for databases, S3 for backups, Direct Connect & VPN for connectivity, IAM & KMS for security**, the company ensured that workloads could transition smoothly **without downtime**.  

The hybrid model allowed **on-premise systems and AWS cloud services** to operate seamlessly together during the migration phase. This approach reduced risks, improved scalability, and safeguarded sensitive customer data while providing the flexibility to **gradually shift all workloads to the cloud** in the future.  

In short, this solution gave the customer:  

🚀 **Agility** to migrate at their own pace  
💰 **Cost-efficiency** with optimized cloud resources  
🔒 **Security & Compliance** for sensitive insurance data  
⚡ **High Availability** to keep services always online  




