# AWS Solutions Architect Associate (SAA-C03): Hands-On Case Study Workbook

**A topic-wise, console-first lab guide to pair with Stephane Maarek's course**

Every case study is a mini real-world project. Read the topic section in the video course, then come back and build the case study in the AWS Management Console. The goal is not to click through a tutorial but to **make design decisions, break things, and fix them**, which is exactly what the exam tests.

---

## Table of Contents

- **MODULE 1: Account Foundations and Governance**
    - CS-01: Secure Account Baseline with Billing Guardrails
    - CS-02: Multi-Account Governance with Organizations and SCPs
- **MODULE 2: IAM Deep Dive**
    - CS-03: Least Privilege for a Development Team (ABAC)
    - CS-04: Cross-Account Access with Roles, External ID and Permission Boundaries
    - CS-05: "Why Am I Getting Access Denied?" Policy-Evaluation Troubleshooting Lab
- **MODULE 3: VPC and Networking**
    - CS-06: Build a Production-Grade 3-Tier VPC From Scratch
    - CS-07: Private Connectivity to AWS Services with VPC Endpoints and Flow Log Forensics
    - CS-08: Connect Many Networks: Peering vs Transit Gateway
    - CS-09: The Broken Network: SG vs NACL Troubleshooting Lab
- **MODULE 4: EC2, Storage and Instance Design**
    - CS-10: Build a Hardened, Self-Healing EC2 Platform (AMI, IMDSv2, Session Manager)
    - CS-11: Storage Showdown: EBS, Instance Store, EFS and FSx
- **MODULE 5: Load Balancing and Auto Scaling**
    - CS-12: Highly Available, Self-Healing Web Tier Across 3 AZs
    - CS-13: Advanced Routing, PrivateLink-Ready NLB and Edge Security
- **MODULE 6: Route 53 and Edge Delivery**
    - CS-14: Global Traffic Management with Every Route 53 Routing Policy
    - CS-15: CloudFront and Global Accelerator for a Global, Secure Static and Dynamic Site
- **MODULE 7: Amazon S3 in Depth**
    - CS-16: S3 Security and Data Protection for Regulated Data
    - CS-17: S3 Replication, Lifecycle, Events and Analytics
- **MODULE 8: Databases on AWS**
    - CS-18: Resilient RDS: Multi-AZ, Read Replicas, Proxy and Encryption Migration
    - CS-19: Aurora: Serverless v2, Clones, Backtrack and Global Database Failover
    - CS-20: DynamoDB Data Modeling, Streams, DAX and Global Tables
    - CS-21: Caching Strategies in Front of a Database with ElastiCache
- **MODULE 9: Serverless Architectures**
    - CS-22: Secure Serverless REST API with Cognito, Throttling and Canary Releases
    - CS-23: Order Processing Workflow with Step Functions and Saga Compensation
- **MODULE 10: Decoupling and Event-Driven Design**
    - CS-24: SQS and SNS Fan-Out with Filtering, DLQs and FIFO Ordering
    - CS-25: Real-Time Streaming Pipeline with Kinesis, Firehose, EventBridge and Athena
- **MODULE 11: Containers on AWS**
    - CS-26: Microservices on ECS Fargate with ALB, Service Discovery and Secrets
    - CS-27: ECS on EC2 with Capacity Providers, plus an EKS Walk-Through
- **MODULE 12: Security, Encryption and Threat Detection**
    - CS-28: KMS Mastery: Key Policies, Grants, Envelope Encryption and Cross-Account Sharing
    - CS-29: Secrets, Parameters and Certificates: Rotate Without Downtime
    - CS-30: Threat Detection and Automated Remediation
- **MODULE 13: Monitoring, Auditing and Operations**
    - CS-31: Full-Stack Observability and Audit Trail
    - CS-32: Fleet Operations with Systems Manager (No SSH, No Bastions)
- **MODULE 14: Data, Analytics and AI Services**
    - CS-33: Build a Serverless Data Lake with Fine-Grained Governance
    - CS-34: AI Services Pipeline: Rekognition, Transcribe, Comprehend, Polly and Textract
- **MODULE 15: Migration and Hybrid Connectivity**
    - CS-35: Simulated On-Premises Migration: Databases, Files and SFTP
    - CS-36: Hybrid Network: Site-to-Site VPN, Hybrid DNS and a Direct Connect Design
- **MODULE 16: Disaster Recovery and High Availability**
    - CS-37: Backup and Restore Strategy with AWS Backup, Cross-Region and Cross-Account Copies
    - CS-38: Pilot Light and Warm Standby Across Regions
    - CS-39: Active-Active Multi-Region Game Day
- **MODULE 17: Infrastructure as Code and Deployment Strategies**
    - CS-40: CloudFormation Deep Dive: Nested Stacks, Change Sets, Drift and StackSets
    - CS-41: Deployment Strategies: Elastic Beanstalk, CodeDeploy and Blue/Green
- **MODULE 18: Cost Optimization and the Well-Architected Framework**
    - CS-42: Cost Optimization Audit of a Wasteful Architecture
- **CAPSTONE PROJECTS**
    - CAP-1: Secure Multi-Account, Multi-Region E-Commerce Platform
    - CAP-2: Event-Driven Image and Document Intelligence Platform (Serverless)
    - CAP-3: Whiteboard Drills and Exam-Style Design Reviews
- **APPENDIX A: Console Muscle-Memory Checklist**
- **APPENDIX B: Quick "Which Service?" Decision Table**
- **APPENDIX C: Final Two-Week Plan**

---

## 0. How to Use This Workbook

**The loop for every case study**

1. **Watch** the matching course section (the "Course section" line tells you which).
2. **Design first**: spend 5 minutes on paper. Draw the architecture and write down your RPO/RTO, security and cost reasoning *before* opening the console.
3. **Build** in the console (not CloudFormation, unless the case study says so). Console work builds the muscle memory the exam relies on.
4. **Break it** using the "Break it" challenges. Most learning happens here.
5. **Explain it** out loud: "Why this service and not the alternative?" Answer the "Exam angles" bullets in your own words.
6. **Clean up** immediately. Tick the cleanup checklist.

**Difficulty:** `[*]` guided, `[**]` intermediate, `[***]` advanced, `[****]` expert/capstone.

### 0.1 Cost and Safety Ground Rules (read before you start)

- Enable **MFA on the root user**, and never use root for daily work (you will do this in CS-01).
- Create a **Budget alert at a low threshold** (for example USD 5 and USD 20) before building anything.
- **Resources that silently burn money if forgotten:** NAT Gateways (hourly plus data), Application/Network Load Balancers, Aurora/RDS instances, Elastic IPs not attached, Transit Gateway attachments, Site-to-Site VPN connections, interface VPC endpoints, Global Accelerator, Redshift/OpenSearch/EMR clusters, EKS control plane, Macie, Kinesis shards, cross-region replication and data transfer.
- Use **one region** (for example `us-east-1` or `eu-west-1`) for most labs. A second region is used only for DR labs.
- **Tag everything**: `Project=saa-lab`, `CaseStudy=CS-xx`. At the end of each lab filter by tag in **Resource Groups & Tag Editor** to find leftovers.
- Prices change; check the AWS pricing pages and the **Free Tier / credits** page for your account. The cost notes in this workbook are only rough guides.
- Lab accounts only. Never run these labs in an employer's production account.

### 0.2 Suggested Order (about 8 to 10 weeks at 1 to 2 hours/day)

| Week | Modules | Why in this order |
|------|---------|-------------------|
| 1 | M1 Account foundations, M2 IAM | Everything else depends on identity and billing safety |
| 2 | M3 VPC, M4 EC2 and storage | Networking and compute are the base of most exam questions |
| 3 | M5 ELB/ASG, M6 Route 53 and edge | High availability and scalability patterns |
| 4 | M7 S3, M8 Databases | Core data services; very high exam weight |
| 5 | M9 Serverless, M10 Decoupling | Event-driven design |
| 6 | M11 Containers, M12 Security | Container patterns; encryption and threat detection |
| 7 | M13 Monitoring, M14 Analytics/AI | Operations, data lakes, AI services |
| 8 | M15 Migration/hybrid, M16 Disaster recovery | Scenario-heavy topics |
| 9 | M17 IaC/deployments, M18 Cost, Well-Architected | Operational excellence and cost |
| 10 | Capstones | Integrate everything and rehearse the exam mindset |

### 0.3 Exam Domains This Workbook Covers

| SAA-C03 domain | Approx. weight | Main modules |
|----------------|----------------|--------------|
| Design Secure Architectures | 30% | M1, M2, M3, M7, M12 |
| Design Resilient Architectures | 26% | M5, M6, M8, M10, M16 |
| Design High-Performing Architectures | 24% | M4, M6, M8, M9, M11, M14 |
| Design Cost-Optimized Architectures | 20% | M4, M7, M18 |

> The course section names listed in each module follow the Stephane Maarek SAA-C03 course layout. Names can differ slightly between course versions; match by topic.

### 0.4 Template Used in Every Case Study

- **Scenario**: the business problem
- **Build**: checklist of what to implement in the console
- **Break it**: failure injections and troubleshooting drills
- **Exam angles**: the design trade-offs the exam loves
- **Cost and cleanup**: what to watch and what to delete

---

# MODULE 1: Account Foundations and Governance

**Course section:** IAM and AWS CLI (intro), AWS Organizations and Control Tower (in IAM Advanced), Billing and Cost Management.

## CS-01: Secure Account Baseline with Billing Guardrails `[*]`

**Scenario:** You have just been hired as the first cloud engineer at a startup. The CEO has handed you a fresh AWS account with only a root login. Leadership fears two things: an account takeover and a surprise bill.

**Build**

- [ ] Enable MFA on root. Delete any root access keys. Set an account alias and a strong password policy (length, rotation, reuse prevention).
- [ ] Create an **IAM Identity Center** instance (or, if you prefer, an IAM admin user) with an admin permission set. Log in with it and stop using root.
- [ ] Create **AWS Budgets**: one monthly cost budget with email alerts at 50%, 80% and 100% (actual) and one forecasted alert. Add a **zero-spend** or **credit** budget if available.
- [ ] Turn on **Cost Explorer**, **Cost Anomaly Detection**, and activate user-defined **cost allocation tags** (`Project`, `CaseStudy`).
- [ ] Create a **multi-Region CloudTrail trail** delivering to a new S3 bucket (with log file validation on). Confirm that the bucket policy was written for you and read it line by line.
- [ ] Enable **IAM Access Analyzer** and generate the **credential report**.
- [ ] Add alternate contacts (billing, security, operations).

**Break it**

- Log in as a non-admin user and try to view the Billing page. Why is it denied? Find the account-level setting "IAM user and role access to Billing information" and enable it.
- Make the bucket policy for CloudTrail too strict and watch trail delivery fail. Read the error in the trail status.
- Search CloudTrail event history for your own `ConsoleLogin` event and note which fields prove MFA was used.

**Exam angles**

- Root-only tasks (changing account settings, closing account, restoring IAM permissions when locked out, enabling MFA Delete on S3 versioning, CloudFront key pairs in legacy flows).
- Event history (90 days, management events) versus a **trail** (long-term, S3, can be organization-wide).
- Budgets (alerts and actions) versus Cost Explorer (analysis) versus Cost and Usage Report (raw data).

**Cost and cleanup:** Keep this baseline permanently. It is nearly free; the trail's first management-event copy is free, and S3 storage is small.

---

## CS-02: Multi-Account Governance with Organizations and SCPs `[***]`

**Scenario:** The startup now has Dev, Prod and Security teams. The CTO wants hard guardrails: nobody may disable CloudTrail, nobody may launch resources outside two approved Regions, and Dev accounts must not be able to use expensive instance families.

**Build**

- [ ] Enable **AWS Organizations** in the management account. Create OUs: `Security`, `Workloads/Dev`, `Workloads/Prod`.
- [ ] Create (or invite) at least **two member accounts** (use email aliases such as `you+dev@gmail.com`).
- [ ] Write three **SCPs**: (1) deny `cloudtrail:StopLogging`/`DeleteTrail`, (2) deny all actions when `aws:RequestedRegion` is not in your approved list (with exceptions for global services such as IAM, Route 53, CloudFront, Organizations, Support), (3) deny `ec2:RunInstances` unless `ec2:InstanceType` is in an allowed list (Dev OU only).
- [ ] Attach each SCP to the right OU. Test with an admin user in the Dev account.
- [ ] Set up **IAM Identity Center** with groups (`Developers`, `Auditors`) and permission sets assigned to specific accounts.
- [ ] Share a resource across accounts using **AWS RAM** (for example a subnet or a Transit Gateway later).
- [ ] Try **consolidated billing** reports: view cost per member account.
- [ ] Optional: explore **Control Tower** landing zone (read what it would create; deploying it creates many resources, so do it only if you accept the cost).

**Break it**

- As the Dev account's *root user*, try to launch a disallowed instance type. Does the SCP apply to the root user of a member account? (Yes.)
- Try applying an SCP to the management account and observe that **SCPs never affect the management account**.
- Remove the default `FullAWSAccess` SCP from an OU and watch everything stop. Why? (SCPs filter, they do not grant.)
- Give a Dev IAM user `AdministratorAccess`, then show that the SCP still blocks the denied actions.

**Exam angles**

- SCP = maximum permission boundary for accounts; it never grants anything. Effective permission = intersection of SCP, permission boundary and identity policy (plus resource policy rules).
- Organization trail, backup policies, tag policies and AI opt-out policies.
- Why multiple accounts (blast radius, billing separation, compliance) versus multiple VPCs.

**Cost and cleanup:** Organizations is free. Close or remove test member accounts only if you no longer need them (account closure has a waiting period).

---

# MODULE 2: IAM Deep Dive

**Course section:** IAM and AWS CLI, IAM Advanced (STS, policy evaluation, permission boundaries, Identity Center, Directory Services).

## CS-03: Least Privilege for a Development Team (ABAC) `[**]`

**Scenario:** A team of 10 developers need to start/stop only *their own* EC2 instances, read from one S3 prefix and never touch IAM. Admins want to avoid writing one policy per developer.

**Build**

- [ ] Create IAM groups `Developers` and `ReadOnlyAuditors`. Create 3 test users (programmatic and console access as needed) and add them.
- [ ] Write a **customer managed policy** that allows `ec2:StartInstances`/`StopInstances` **only when** `aws:ResourceTag/Owner` equals `${aws:username}` (tag-based access control).
- [ ] Add a condition that requires **MFA** (`aws:MultiFactorAuthPresent`) for any stop/terminate action.
- [ ] Add an explicit deny for any action outside two Regions using `aws:RequestedRegion`.
- [ ] Add S3 access limited to `arn:aws:s3:::lab-bucket/${aws:username}/*` using policy variables, and test `ListBucket` with an `s3:prefix` condition.
- [ ] Add an inline policy example, then compare when inline versus managed is appropriate.
- [ ] Use the **IAM Policy Simulator** to prove each rule.
- [ ] Configure the **AWS CLI** with named profiles for two users and run the same commands under both.

**Break it**

- Tag an instance `Owner=bob` and try to stop it as `alice`. Confirm the denial in CloudTrail (`errorCode=AccessDenied`) and read the encoded authorization message using `sts decode-authorization-message`.
- Let a developer change their own `Owner` tag. How would this break the design? Fix it by denying `ec2:CreateTags`/`DeleteTags` for tag key `Owner`.
- Remove the `ListBucket` rule and see why `s3:GetObject` alone is not enough to list objects.

**Exam angles**

- Identity-based versus resource-based policies; policy elements (`Principal`, `NotPrincipal`, `Condition`).
- Explicit deny overrides everything; implicit deny is the default.
- ABAC scales better than adding a policy per project.

**Cost and cleanup:** Free. Delete test users and access keys. Never commit keys to Git.

---

## CS-04: Cross-Account Access with Roles, External ID and Permission Boundaries `[***]`

**Scenario:** A third-party monitoring vendor needs read-only access to your Production account. Separately, a junior admin should be able to create IAM roles for applications but must never be able to create an admin role.

**Build**

- [ ] In Account B (Prod), create a role `VendorReadOnly` with a trust policy trusting Account A (Security/Vendor) and requiring an **External ID**. Attach `ReadOnlyAccess` (or a tighter custom policy).
- [ ] In Account A, create a user/role allowed to `sts:AssumeRole` on that ARN. Assume it using the console **Switch Role** and using the CLI (`aws sts assume-role`). Inspect the temporary credentials (access key, secret, session token, expiry).
- [ ] Set a **maximum session duration** and a **session policy** to further reduce permissions at assume time.
- [ ] Create a **permission boundary** policy that allows only specific services (S3, DynamoDB, CloudWatch). Create a `DelegatedAdmin` user who may `iam:CreateRole`/`AttachRolePolicy` **only if** the boundary is attached (`iam:PermissionsBoundary` condition).
- [ ] As `DelegatedAdmin`, create an app role *with* the boundary and attach `AdministratorAccess` to it. Prove the app role still cannot call EC2.
- [ ] Set up **role chaining** (A to B to C) and observe the 1-hour limit on chained sessions.
- [ ] Use the **aws:PrincipalOrgID** condition in an S3 bucket policy so any account in your Organization can read, without listing account IDs.

**Break it**

- Remove the External ID from the assume-role call and read the error. Explain the **confused deputy problem** it prevents.
- Have DelegatedAdmin attempt to create a role without the boundary. Confirm denial.
- Try to cross-account access an S3 bucket using **only** an identity policy on the other side. Explain why both sides must allow.

**Exam angles**

- Cross-account: trust policy plus permission policy; resource-based policies let the caller keep its own permissions (no role switch), assume-role means switching to the role's permissions.
- Permission boundary versus SCP versus session policy: all are filters, none grant.
- IAM Roles for services (EC2, Lambda, ECS tasks) and why they beat long-lived keys.

**Cost and cleanup:** Free. Remove cross-account trust after the lab.

---

## CS-05: "Why Am I Getting Access Denied?" Policy-Evaluation Troubleshooting Lab `[***]`

**Scenario:** An application on EC2 cannot read an SSE-KMS encrypted object from S3. Four different teams own the IAM role, the bucket, the KMS key and the Organization SCP. You are the architect who must find the layer that is blocking access.

**Build**

- [ ] Create a bucket with SSE-KMS (customer managed key), a bucket policy with a **deny unless `aws:SecureTransport`** statement, and an object under `/reports/`.
- [ ] Create an EC2 instance with an **instance profile** role. Install nothing; use **Session Manager** or **CloudShell** with the role's temporary credentials to run `aws s3 cp`.
- [ ] Sequentially introduce one blocker at a time and diagnose each: (1) role lacks `s3:GetObject`; (2) role lacks `kms:Decrypt`; (3) key policy does not trust IAM policies / does not list the role; (4) bucket policy explicit deny; (5) SCP deny; (6) permission boundary; (7) VPC endpoint policy blocking the bucket (add this after CS-07).
- [ ] For each, use **CloudTrail**, **IAM Policy Simulator**, **IAM Access Analyzer policy validation** and the **access-denied error message details** to isolate the cause.
- [ ] Use IAM **last accessed** information to trim an over-broad policy to what the role actually uses.
- [ ] Use **Access Analyzer external-access findings** to detect a bucket intentionally shared outside the account.

**Break it**

- Make the S3 bucket public via ACL, then enable **Block Public Access** at the account level and observe the effect on existing ACLs and policies.
- Rotate the role's credentials mentally: how long do instance-profile credentials last and how are they refreshed? (Check the instance metadata service, IMDSv2.)

**Exam angles**

- Evaluation logic: explicit deny, then SCPs, then resource policy / boundary / session policy, then identity policy.
- KMS key policy is **required** to allow access; IAM policy alone is not enough unless the key policy delegates to IAM.
- Use IAM roles, not embedded access keys, for any compute service.

**Cost and cleanup:** KMS key (about USD 1/month if left, plus requests), small EC2. Delete the instance, schedule key deletion (7 to 30 days waiting period).

---

# MODULE 3: VPC and Networking

**Course section:** VPC (fundamentals, endpoints, flow logs, peering, Transit Gateway, VPN, Direct Connect, PrivateLink), Route 53, Classic Solutions Architecture.

## CS-06: Build a Production-Grade 3-Tier VPC From Scratch `[**]`

**Scenario:** You are building the network for a payment web application. It needs a public web tier, a private application tier and an isolated database tier, spread over three Availability Zones, with no single point of failure, and the database must have no route to the Internet.

**Build** *(do NOT use the "VPC and more" wizard on the first pass; build each piece manually)*

- [ ] Create a VPC with CIDR `10.0.0.0/16`. Enable DNS hostnames and DNS resolution. Plan a subnet CIDR table (public `/24`s, private app `/22`s, database `/24`s) across **3 AZs** and write it down.
- [ ] Create 9 subnets (3 public, 3 private-app, 3 private-db). Create and attach an **Internet Gateway**.
- [ ] Create **three route tables**: public (default route to IGW), private-app (default route to a **NAT Gateway in the same AZ**), database (no default route at all). Associate each subnet explicitly.
- [ ] Create **one NAT Gateway per AZ** (or one for cost-saving mode, noting the trade-off) with Elastic IPs.
- [ ] Create security groups: `alb-sg` (80/443 from the Internet), `app-sg` (app port **only from `alb-sg`**), `db-sg` (3306/5432 **only from `app-sg`**), `bastion-sg` (optional).
- [ ] Create **NACLs**: a custom NACL for the DB subnets that allows only app-subnet CIDRs on the DB port plus ephemeral return ports, and denies the rest.
- [ ] Launch an EC2 in a private-app subnet without a public IP; reach it with **Session Manager** (needs the SSM role plus NAT or SSM VPC endpoints). Launch a test instance in the DB subnet and confirm there is no Internet access.
- [ ] Enable **VPC Flow Logs** to CloudWatch Logs or S3.
- [ ] Recreate the same network with the **"VPC and more" wizard** and compare what it generates.

**Break it**

- Delete the NAT Gateway in AZ-a and see which instances lose Internet access. What would a "highly available NAT" design look like?
- Add a deny rule for port 443 in the NACL and explain why security groups did not stop it (NACLs are stateless; remember ephemeral ports 1024-65535).
- Create an overlapping CIDR VPC and try to peer it with the first one. Note the failure.
- Add a secondary CIDR block to your VPC and create a new subnet in it.

**Exam angles**

- Public subnet = route to an IGW (not simply "has a public IP"). A NAT Gateway lives in a **public** subnet and is **AZ-scoped**.
- Security groups: stateful, allow rules only, can reference other SGs. NACLs: stateless, allow and deny, rules evaluated in number order.
- Five reserved IPs per subnet; the smallest subnet is `/28`.
- Egress-only Internet Gateway for IPv6; NAT Gateway versus NAT instance.

**Cost and cleanup:** NAT Gateways cost hourly plus per-GB. **Delete NAT Gateways and release Elastic IPs right after each session**; you can recreate them in minutes.

---

## CS-07: Private Connectivity to AWS Services with VPC Endpoints and Flow Log Forensics `[***]`

**Scenario:** A compliance team requires that traffic from private subnets to S3 and DynamoDB never traverses the public Internet and that only your company's buckets can be accessed from those subnets (to prevent data exfiltration).

**Build**

- [ ] Remove the NAT route for the private-app subnets (from CS-06). Confirm the instance can no longer reach S3.
- [ ] Create an **S3 Gateway Endpoint**; attach it to the private route tables. Verify the route table now has a prefix-list route. Re-test S3.
- [ ] Add an **endpoint policy** restricting access to one named bucket. Test copying to a different (allowed) bucket in another account, and see it fail.
- [ ] Add a **bucket policy** that denies all access unless `aws:SourceVpce` equals your endpoint ID. Try accessing the bucket from your laptop and from the console (what happens to you?).
- [ ] Create a **DynamoDB Gateway Endpoint**.
- [ ] Create **Interface Endpoints** (PrivateLink) for `ssm`, `ssmmessages`, `ec2messages` and `logs`. Turn on **private DNS** and show that the service hostname resolves to private IPs.
- [ ] Publish your own service: put a simple web app behind a **Network Load Balancer**, create an **Endpoint Service** and consume it from a second VPC (or account) through an interface endpoint.
- [ ] Query **VPC Flow Logs with Athena** (partitioned table). Find (a) all rejected SSH attempts, (b) top talkers by bytes, (c) traffic that reached the DB subnet.

**Break it**

- Remove the endpoint from the route table and watch S3 calls hang. Which setting did you forget?
- Disable private DNS on an interface endpoint and see the SDK still calling the public endpoint.

**Exam angles**

- Gateway endpoints (S3, DynamoDB only; free; route-table based) versus interface endpoints (ENI, cost, security groups, most other services).
- PrivateLink exposes one service one-way without peering or CIDR concerns.
- Flow logs capture metadata, not packet contents, and can be sent to S3, CloudWatch Logs or Kinesis Data Firehose.

**Cost and cleanup:** Interface endpoints bill per AZ per hour. Delete them after the lab.

---

## CS-08: Connect Many Networks: Peering vs Transit Gateway `[***]`

**Scenario:** After an acquisition, you have three VPCs (Shared Services, App, Acquired Company). Two of them use overlapping CIDRs. Leadership wants a design that scales to 50 VPCs and keeps Prod isolated from Dev.

**Build**

- [ ] Create three VPCs: `Shared 10.1.0.0/16`, `App 10.2.0.0/16`, `Acquired 10.2.0.0/16` (overlap on purpose).
- [ ] Create a **VPC peering** connection between Shared and App. Update route tables on both sides, SGs, and test ping/SSH across the peering. Try Shared to Acquired and see the overlap error.
- [ ] Verify **non-transitive routing**: peer Shared with App and Shared with a fourth VPC; show that App cannot reach the fourth VPC via Shared.
- [ ] Create a **Transit Gateway**, attach `Shared` and `App`. Use **two TGW route tables** (`prod`, `shared`) to implement isolation: App can reach Shared, but a `Dev` VPC cannot reach `Prod`.
- [ ] Test **cross-Region TGW peering** to a second Region (optional).
- [ ] Solve the overlap problem: re-address the Acquired VPC, or expose only the needed application through **PrivateLink** (NLB plus endpoint service) rather than routing.
- [ ] Use **Reachability Analyzer** to verify and troubleshoot a path (for example App instance to Shared DB).

**Break it**

- Remove one return route and observe that ping works in one direction only.
- Disable route propagation on the TGW route table and see what changes.

**Exam angles**

- Peering: simple, cheap, no transitive routing, no overlapping CIDRs, cross-Region and cross-account supported.
- TGW: hub-and-spoke, transitive, route tables for segmentation, supports VPN and Direct Connect attachments, billed per attachment and per GB.
- Overlapping IPs, expose a service with PrivateLink instead of routing.
- Sharing TGW and subnets across accounts with RAM.

**Cost and cleanup:** TGW attachments cost hourly. Delete attachments, then the TGW (deletion can take minutes).

---

## CS-09: The Broken Network: SG vs NACL Troubleshooting Lab `[**]`

**Scenario:** A teammate "improved" the network last night, and now the application is unreachable. You must diagnose and repair it using only the console.

**Build the broken environment, then repair it**

- [ ] Build: a public ALB with two private instances, an RDS in the DB subnet. Verify it works.
- [ ] Inject 8 faults, one at a time, and restore after each: (1) remove IGW route; (2) detach IGW; (3) SG outbound rule removed; (4) NACL missing ephemeral port outbound; (5) NACL deny rule with lower number than allow; (6) instance without a public IP in a public subnet (for direct access); (7) SG referencing wrong SG; (8) DNS resolution disabled on VPC.
- [ ] For each fault, write down the **symptom** and the **tool** that exposed it (Reachability Analyzer, Flow Logs "REJECT", `curl -v`, `nslookup`, ALB target health reason codes).
- [ ] Add **AWS Network Firewall** or a **Gateway Load Balancer** appliance (conceptually or hands-on if budget allows) for centralized inspection.
- [ ] Use **VPC Traffic Mirroring** (needs Nitro instances) to capture and inspect traffic.

**Exam angles**

- Order of evaluation: NACL at the subnet boundary, then SG at the ENI.
- Return traffic through NACLs needs ephemeral ports; SGs are stateful.
- Which AWS tool answers "can A reach B?" (Reachability Analyzer) versus "what traffic was accepted/rejected?" (Flow Logs).

**Cost and cleanup:** ALB, NAT and RDS hourly costs apply. Teardown in reverse order: ALB, instances, RDS, endpoints, NAT, IGW, subnets, VPC.

---

# MODULE 4: EC2, Storage and Instance Design

**Course section:** EC2 Basics, EC2 Solutions Architect and Associate Level, EC2 Instance Storage (EBS, EFS), AWS Storage Extras (FSx).

## CS-10: Build a Hardened, Self-Healing EC2 Platform (AMI, IMDSv2, Session Manager) `[**]`

**Scenario:** Security demands that no server has SSH open, no instance exposes long-lived credentials, every server boots from an approved image, and the finance team wants to know which purchase option fits each workload.

**Build**

- [ ] Launch an EC2 (Amazon Linux 2023) in a private subnet with **no key pair** and **no inbound rules**. Access it with **Session Manager** only. Enable session logging to S3/CloudWatch.
- [ ] Use **user data** to install and configure a web server and pull a config value from **SSM Parameter Store**.
- [ ] Require **IMDSv2** (`HttpTokens=required`). From the instance, query the metadata service with and without a token and compare. Show that an SSRF-style request would fail under IMDSv2.
- [ ] Build a **golden AMI**: customize an instance, create an AMI, copy it to another Region, and launch a new instance from it. Share the AMI privately with another account (and understand what happens to encrypted AMIs and their snapshots).
- [ ] Try **EC2 Image Builder** to automate the golden AMI pipeline with a test stage.
- [ ] Practice **purchase options** by listing, for a 3-year steady database, a nightly batch job, a CI fleet and a 1-week stress test: which of On-Demand, Reserved/Savings Plans, Spot, Dedicated Hosts, Capacity Reservations? Request a **Spot instance** and observe the interruption notice via metadata (or simulate via the Spot console "interrupt" feature in AWS Fault Injection Service).
- [ ] Create **placement groups**: cluster (low latency), spread (max 7 instances per AZ), partition (big data). Launch into each and note error messages when capacity is limited.
- [ ] Set **instance stop protection / termination protection**, and enable **EC2 instance auto-recovery** with a CloudWatch status-check alarm. Hibernate an instance and resume.

**Break it**

- Stop and start an instance: what changes (public IP, instance store data) and what persists (EBS, private IP, Elastic IP)?
- Terminate an instance with `DeleteOnTermination` on and off for the root and extra volumes.
- Block IMDS hop limit at 1 and test whether a container on the host can reach credentials.

**Exam angles**

- Instance families (compute, memory, storage, accelerated), Nitro, burstable T-series credits.
- Savings Plans (Compute versus EC2 Instance) versus Reserved Instances (Standard versus Convertible), Spot (up to 90% off, 2-minute warning), Dedicated Host (per-socket licensing, BYOL).
- Elastic IP is a regional, static public IPv4 and costs when idle; prefer DNS names or load balancers.

**Cost and cleanup:** Terminate instances, deregister AMIs **and delete their snapshots**.

---

## CS-11: Storage Showdown: EBS, Instance Store, EFS and FSx `[***]`

**Scenario:** You must support four workloads: a transactional database needing 64,000 IOPS, a temporary scratch area for rendering, a shared content directory used by 30 web servers across AZs, and a Windows file share for a legacy app.

**Build**

- [ ] Create **gp3** and **io2** volumes. Resize a live volume, change type, and extend the file system on Linux. Benchmark using `fio` and compare IOPS and throughput to gp3's tunable settings.
- [ ] **Encrypt an unencrypted volume**: snapshot, copy snapshot with encryption (KMS key), restore, swap. Enable **EBS encryption by default** in the Region.
- [ ] Take snapshots, create a **Data Lifecycle Manager** policy, enable **Recycle Bin** for snapshots, and use **Fast Snapshot Restore** conceptually.
- [ ] Move a volume across AZs through a snapshot. Attach an **io2 Multi-Attach** volume to two instances in the same AZ (clustered app concept).
- [ ] Launch an instance type with **instance store**; write data, stop the instance, and see the data vanish.
- [ ] Create an **EFS** file system with mount targets in 3 AZs and an **access point** restricting a user to `/app/uploads`. Mount from multiple instances over NFS (security group on port 2049). Compare **General Purpose versus Max I/O**, **bursting, provisioned and elastic throughput**, and set a **lifecycle policy to IA**.
- [ ] Create an **FSx for Windows File Server** (Multi-AZ, needs AD, so use AWS Managed Microsoft AD; do this only if you accept the cost) or, at minimum, create **FSx for Lustre** linked to an S3 bucket and read the data lazily-loaded.
- [ ] Compare FSx options: Windows, Lustre, NetApp ONTAP, OpenZFS in a one-page decision table.

**Break it**

- Fill an EBS volume and extend it live with no downtime.
- Mount EFS from a different AZ with no mount target in that AZ: what is the effect on latency and cost?
- Delete the S3-linked FSx for Lustre file system without exporting changes: what is lost?

**Exam angles**

- EBS: AZ-locked, snapshots stored in S3 and region-scoped; gp3 decouples IOPS from size; io1/io2 for sustained IOPS; st1/sc1 are HDD (cannot be boot volumes).
- EFS: Linux only, multi-AZ, pay for what you use. FSx for Windows: SMB, AD. FSx for Lustre: HPC, S3 integration.
- Instance store: highest IOPS, ephemeral, ideal for caches and scratch.

**Cost and cleanup:** FSx and provisioned IOPS are expensive. Delete file systems, volumes and snapshots after the lab.


---

# MODULE 5: Load Balancing and Auto Scaling

**Course section:** High Availability and Scalability: ELB and ASG, Classic Solutions Architecture.

## CS-12: Highly Available, Self-Healing Web Tier Across 3 AZs `[***]`

**Scenario:** An online retailer expects 10x traffic during a flash sale. The web tier must survive the loss of an AZ, scale out automatically within minutes, and deploy new versions without downtime.

**Build**

- [ ] Create a **Launch Template** (AMI from CS-10, user data, IAM role, IMDSv2). Create an **ASG** across three private subnets with min 3, desired 3, max 12.
- [ ] Create an internet-facing **ALB** in the public subnets, two target groups (blue/green), **health checks** on `/health`, deregistration delay of 30 seconds and a listener on 443 using an **ACM certificate** (redirect 80 to 443).
- [ ] Add **scaling policies**: target tracking on average CPU (50%) and on **ALBRequestCountPerTarget**; a **scheduled action** for the sale start; try **predictive scaling** in forecast-only mode.
- [ ] Enable **ELB health checks** on the ASG (not just EC2 status). Set **health check grace period** appropriately.
- [ ] Generate load (`hey`, `ab` or `stress`) and watch the group scale out and in. Capture the CloudWatch graphs and the **Activity history**.
- [ ] Do an **instance refresh** with a minimum healthy percentage of 90% and a new AMI.
- [ ] Add a **lifecycle hook** (Terminating:Wait) that sends an SNS notification and pauses so a Lambda can upload logs before termination.
- [ ] Configure **mixed instances policy** with On-Demand base capacity and Spot for the rest, using multiple instance types.
- [ ] Add a **warm pool** and compare launch times.

**Break it**

- Use the console to **terminate an instance** and verify automatic replacement. Then terminate all instances in one AZ, simulating an AZ outage (also try the AZ rebalancing process by suspending it).
- Return HTTP 500 from `/health` on one instance and watch the ALB and ASG react in order.
- Set cooldown/warm-up too low and cause scale thrashing. Fix with the right estimated warm-up time.
- Use **AWS Fault Injection Service** to inject CPU stress or terminate instances.

**Exam angles**

- ALB (L7, HTTP/HTTPS, WebSocket, routing rules) versus NLB (L4, ultra-low latency, static IP) versus GWLB (L3 appliances, GENEVE).
- Cross-zone load balancing: on by default for ALB (free), off by default for NLB (inter-AZ data charges).
- Sticky sessions, connection draining (deregistration delay), SNI for multiple certificates on one listener.
- Target tracking versus step versus scheduled versus predictive scaling.

**Cost and cleanup:** ALB, instances, Spot. Set ASG min/desired to 0 then delete the ASG, ALB and target groups.

---

## CS-13: Advanced Routing, PrivateLink-Ready NLB and Edge Security `[***]`

**Scenario:** A SaaS provider is splitting a monolith into microservices. Traffic for `/api/*` must go to an app fleet, `/img/*` to a Lambda function, a canary release must receive 10% of traffic, and a customer demands static IP addresses for allow-listing.

**Build**

- [ ] Build one **ALB** with **listener rules**: path-based (`/api/*`, `/img/*`), host-based (`admin.example.com`), header-based and query-string-based. Use a **Lambda function as a target** for `/img/*`.
- [ ] Add **weighted target groups** (90/10) for a canary and shift gradually. Add **stickiness** at the target group level.
- [ ] Add **authentication** at the ALB through **Amazon Cognito** (user pool) for `/admin/*`.
- [ ] Attach an **AWS WAF** web ACL to the ALB with a rate-based rule and the AWS managed SQLi/XSS rule groups. Test with `curl` payloads.
- [ ] Build an **NLB** with **Elastic IPs** (one per AZ), a TLS listener and a target group of IP targets. Show that a customer can allow-list the 3 static IPs.
- [ ] Put the NLB behind an **ALB** (ALB-type target group on an NLB) to get both static IPs and L7 routing.
- [ ] Deploy a **Gateway Load Balancer** with a small appliance (a simple Linux instance) and route traffic with a GWLB endpoint (or follow along conceptually if cost is a concern).
- [ ] Enable **ALB access logs** to S3 and query them with Athena.

**Break it**

- Make the weighted rule send 100% to the new target group; check CloudWatch `HTTPCode_Target_5XX_Count` and roll back in seconds.
- Remove the certificate for a hostname and watch the SNI fall back to the default certificate.
- Hit the ALB with 1,000 requests/min and confirm the WAF rate rule blocks the source.

**Exam angles**

- ALB features: host/path routing, redirects, fixed responses, OIDC/Cognito auth, Lambda targets, HTTP/2, gRPC.
- NLB: static/Elastic IPs, preserves client source IP, TCP/UDP/TLS, millions of requests per second, required for PrivateLink.
- WAF attaches to ALB, API Gateway, CloudFront, AppSync, Cognito user pools (not NLB). Shield Advanced adds DDoS response support.

**Cost and cleanup:** Delete load balancers, WAF web ACLs and Elastic IPs.

---

# MODULE 6: Route 53 and Edge Delivery

**Course section:** Route 53, CloudFront and Global Accelerator.

## CS-14: Global Traffic Management with Every Route 53 Routing Policy `[**]`

**Scenario:** A media company serves users in North America, Europe and Asia. They want users routed to the nearest Region, a Region automatically removed if unhealthy, and certain countries served localized content or blocked.

**Build**

- [ ] Register or use an existing domain (or create a **private hosted zone** if you do not own one; you can also use a cheap domain for the labs).
- [ ] Deploy a tiny web app (an EC2 or S3-hosted page that shows its Region name) in **three Regions**.
- [ ] Create records using each policy and test with `dig`/`nslookup` and a VPN or the **Route 53 test record** tool: **Simple, Weighted, Latency, Failover (active-passive), Geolocation, Geoproximity (with bias), Multi-value answer**.
- [ ] Create **health checks**: endpoint (HTTP/HTTPS/TCP), **calculated** health check combining others, and a health check based on a **CloudWatch alarm** (for private resources). Observe health-checker IPs hitting your SG; allow them.
- [ ] Use an **Alias record** to an ALB, CloudFront distribution, S3 website and API Gateway. Compare with CNAME (apex domain, cost, health evaluation).
- [ ] Set **TTL** values and measure DNS caching behavior during a failover.
- [ ] Configure a **private hosted zone** associated with two VPCs and split-horizon DNS (same name, different answers inside and outside).
- [ ] Optional: DNSSEC signing, and **Route 53 Resolver** with inbound/outbound endpoints (also used in CS-35).

**Break it**

- Stop the web server in Region 1 and time how long until Route 53 fails traffic over (health check interval x threshold + TTL).
- Create a **Failover** record without a health check on the primary and see why it never fails over.
- Create Geolocation records with no **default** record and observe the "no answer" outcome for unmapped countries.

**Exam angles**

- Which policy for which need: latency (performance), geolocation (compliance/localization), geoproximity (shift traffic with bias), weighted (blue/green and A/B), failover (DR), multi-value (client-side load balancing with health checks).
- Alias records are free for AWS resources, support the zone apex and evaluate target health.
- Route 53 is a **global** service with 100% SLA.

**Cost and cleanup:** Hosted zone (about USD 0.50/month), health checks cost extra. Delete the records and health checks.

---

## CS-15: CloudFront and Global Accelerator for a Global, Secure Static and Dynamic Site `[***]`

**Scenario:** A global e-learning platform needs fast static assets, private paid videos (only subscribers), protection against origin overload, automatic origin failover, and a TCP/UDP gaming-style endpoint with fixed IPs.

**Build**

- [ ] Create an **S3 bucket** (private) and a **CloudFront distribution** using **Origin Access Control (OAC)**. Lock the bucket policy to the distribution only and verify direct S3 URLs are denied.
- [ ] Add a second origin: your **ALB**. Create behaviors: `/static/*` to S3 (long TTL, cache policy), `/api/*` to ALB (caching disabled, forward headers/cookies via an **origin request policy**).
- [ ] Add an **origin group** (primary S3 in Region 1, secondary S3 in Region 2) and test failover by blocking the primary.
- [ ] Use **signed URLs** (single file, using a trusted key group) and **signed cookies** (multiple files) for paid content. Compare with **S3 pre-signed URLs**.
- [ ] Create a **CloudFront Function** for URL rewrite/header manipulation and a **Lambda@Edge** function for an auth check. Compare the limits and capabilities.
- [ ] Configure **geo restriction**, **custom error pages**, **HTTPS with an ACM certificate (must be in us-east-1)** and a **custom domain** via Route 53 alias.
- [ ] Add **field-level encryption** and **security headers** response policy.
- [ ] Perform **cache invalidation** and compare with versioned file names.
- [ ] Build a **Global Accelerator** with two ALBs in different Regions. Observe the two static anycast IPs, endpoint weights and traffic dials, and health-based failover. Compare latency with CloudFront.

**Break it**

- Deploy an updated file and see stale content from the CDN. Fix via invalidation and via cache keys.
- Create a bucket policy that allows public read, then enforce OAC and measure the difference.

**Exam angles**

- CloudFront: caches content at edge locations (HTTP/S); Global Accelerator: no caching, TCP/UDP, static anycast IPs, fast regional failover.
- OAC replaces OAI. Signed URL (one file) versus signed cookie (multiple files).
- CloudFront ACM certs live in **us-east-1**; ALB certs live in the ALB's Region.

**Cost and cleanup:** Delete distributions (disable first, deployment takes minutes) and accelerators.

---

# MODULE 7: Amazon S3 in Depth

**Course section:** Amazon S3 Introduction, S3 Advanced, S3 Security, AWS Storage Extras (Snow Family, DataSync, Storage Gateway) in M15.

## CS-16: S3 Security and Data Protection for Regulated Data `[***]`

**Scenario:** A healthcare analytics company stores sensitive reports in S3. Auditors require encryption with customer-controlled keys, protection against deletion and overwrite for 7 years, no public exposure, and time-limited sharing with external partners.

**Build**

- [ ] Create a bucket with **Block Public Access** (all four settings) and enforce **bucket owner enforced** object ownership (ACLs disabled).
- [ ] Enable default encryption with **SSE-KMS** (customer managed key) and **Bucket Keys**. Write a bucket policy denying uploads that are not encrypted with that key and any request that is not over **TLS**.
- [ ] Turn on **versioning**. Overwrite and delete objects, restore a previous version, and observe delete markers. Try **MFA Delete** (CLI, root only) if you wish.
- [ ] Create a second bucket with **S3 Object Lock** in **compliance mode** (retention) and another test in **governance mode** with a legal hold. Prove who can and cannot delete.
- [ ] Use **pre-signed URLs** (GET and PUT) with short expiries and confirm they inherit the signer's permissions. Test an expired URL.
- [ ] Create **S3 Access Points** (one per team) with different policies and one **VPC-restricted** access point. Use a **Multi-Region Access Point** (optional).
- [ ] Enable **CloudTrail data events** and **S3 server access logs** and compare them.
- [ ] Use **Macie** (small sample bucket) to detect fake PII and review findings.
- [ ] Use **S3 Object Lambda** to redact fields on read (conceptual walk-through or hands-on).
- [ ] Add **CORS** configuration and test with a web page hosted on a different origin.

**Break it**

- Try to upload without the KMS header and observe the denial. Then try as a role that lacks `kms:GenerateDataKey`.
- Attempt to shorten a compliance-mode retention period as admin or root. What happens?
- Make a bucket public via ACL **before** enabling Block Public Access, then enable it and observe the effect.

**Exam angles**

- SSE-S3 versus SSE-KMS (audit trail, key policy, request limits) versus SSE-C (customer provides key; HTTPS required) versus client-side encryption versus DSSE-KMS.
- Compliance mode (nobody, not even root, can delete), governance mode (privileged users can bypass), Glacier Vault Lock for archives.
- Pre-signed URLs, CORS, and S3 Access Points for per-team policy management.

**Cost and cleanup:** Object Lock buckets with compliance mode **cannot be emptied until retention expires**. Use a **1-day** retention for the lab. Delete the KMS key after waiting period.

---

## CS-17: S3 Replication, Lifecycle, Events and Analytics `[***]`

**Scenario:** A photo-sharing app stores billions of images. Data must be replicated to a second Region for DR and to a logging account for audit, aged into cheaper tiers automatically, and each upload must trigger a thumbnail pipeline.

**Build**

- [ ] Create source and destination buckets in two Regions with versioning. Configure **Cross-Region Replication** (CRR) with an IAM role, filter by prefix, enable **delete marker replication** and **replication time control** (optional). Run **S3 Batch Replication** for existing objects.
- [ ] Add **Same-Region Replication** to a bucket in a different account, with **ownership override** to the destination owner.
- [ ] Add **lifecycle rules**: Standard to Standard-IA (30 days) to Glacier Instant/Flexible (90 days) to Deep Archive (365 days), expire noncurrent versions after 30 days, clean up **incomplete multipart uploads**.
- [ ] Try **S3 Intelligent-Tiering** with the archive tiers. Compare to lifecycle.
- [ ] Configure **S3 Event Notifications** to an **SQS queue**, **SNS topic** and a **Lambda function**; then try **EventBridge** integration with advanced filtering.
- [ ] Build the **thumbnail pipeline**: S3 upload to Lambda (Pillow layer or SDK) to second bucket (watch for recursive triggers!).
- [ ] Use **S3 Transfer Acceleration** and **multipart upload** with a large file via CLI; use **byte-range fetches**.
- [ ] Do **S3 Select**/**Athena** queries over CSV and Parquet data in S3. Set up **S3 Storage Lens** and **S3 Inventory**.
- [ ] Practice **Glacier retrieval tiers** (expedited, standard, bulk) and **Restore** operations.
- [ ] Host a **static website** from S3 (public) and compare with the OAC-secured CloudFront approach.

**Break it**

- Create an event notification that writes to the same bucket/prefix as its trigger and see the infinite loop (stop it quickly and set a Lambda reserved concurrency of 1 as a safety brake).
- Upload objects before replication was enabled; confirm they are not replicated until Batch Replication.
- Delete an object in the source and confirm default behavior regarding delete markers.

**Exam angles**

- Replication needs versioning on both sides; only new objects replicate by default; no replication chaining.
- Lifecycle versus Intelligent-Tiering; minimum storage durations; retrieval times and costs.
- Event destinations (SNS, SQS, Lambda, EventBridge) and delivery guarantees.
- Baseline performance: 3,500 PUT / 5,500 GET requests per second per prefix.

**Cost and cleanup:** Empty both buckets (all versions) before deletion. Glacier early-deletion fees apply.

---

# MODULE 8: Databases on AWS

**Course section:** RDS, Aurora and ElastiCache; Databases in AWS (DynamoDB, DocumentDB, Neptune, Keyspaces, QLDB, Timestream).

## CS-18: Resilient RDS: Multi-AZ, Read Replicas, Proxy and Encryption Migration `[***]`

**Scenario:** An existing unencrypted single-AZ MySQL database serves a growing reporting workload and must become highly available, encrypted and able to absorb a connection storm from Lambda functions.

**Build**

- [ ] Launch a **Single-AZ RDS MySQL/PostgreSQL** in the DB subnets (CS-06) with a **DB subnet group**, no public access, and automated backups (retention 7 days).
- [ ] Insert sample data using a client on an EC2 in the app subnet.
- [ ] Convert to **Multi-AZ** (observe the process) and then **force a failover** by rebooting with failover. Measure downtime, and note that the endpoint DNS name is unchanged.
- [ ] Create a **Read Replica** and route reporting queries to it. Observe **replica lag** in CloudWatch. Create a **cross-Region read replica** and **promote** it.
- [ ] **Encrypt the unencrypted database**: snapshot, copy snapshot with encryption, restore, repoint the application. Document the downtime and discuss how to reduce it using a read replica approach (for Aurora/MySQL).
- [ ] Perform a **point-in-time restore** to a new instance at a chosen second. Take a manual snapshot and share it with another account.
- [ ] Create an **RDS Proxy** with Secrets Manager credentials and IAM authentication; connect Lambda or an EC2 through it and watch connection counts.
- [ ] Use **IAM database authentication**; enforce **SSL/TLS** via parameter group; review **Performance Insights**, **Enhanced Monitoring** and **parameter groups/option groups**.
- [ ] Try the **Multi-AZ DB cluster** deployment (two readable standbys) and compare.

**Break it**

- Fill the storage and watch **storage autoscaling** react (set a max).
- Change a static parameter and observe the pending-reboot state.
- Delete the DB without a final snapshot with **deletion protection** on, then off.

**Exam angles**

- Multi-AZ: synchronous, for **HA** (not scaling), automatic failover. Read replicas: asynchronous, for **scaling reads** (up to 15 for Aurora, 5 for others), can be cross-Region.
- Backups: automated (PITR, within the retention window) versus manual snapshots (kept until deleted).
- You cannot enable encryption on an existing instance in place; use snapshot copy. Encrypted source implies encrypted replicas.
- RDS Proxy: pools connections, reduces failover time, ideal for Lambda.

**Cost and cleanup:** Multi-AZ doubles the cost. Delete instances, replicas, snapshots and proxy.

---

## CS-19: Aurora: Serverless v2, Clones, Backtrack and Global Database Failover `[****]`

**Scenario:** A fintech needs a relational database with sub-minute regional failover, spiky workloads without over-provisioning, safe test copies of production, and the ability to undo a catastrophic `DELETE`.

**Build**

- [ ] Create an **Aurora MySQL/PostgreSQL cluster** with a writer and two readers across AZs. Review the **cluster endpoint, reader endpoint and instance endpoints**.
- [ ] Add a **custom endpoint** that routes analytics to a subset of larger readers.
- [ ] Switch to **Aurora Serverless v2** (min/max ACUs) and run a load to see scaling.
- [ ] Create an **Aurora clone** (copy-on-write) and compare with a snapshot restore.
- [ ] **Backtrack** (Aurora MySQL): corrupt data intentionally, then rewind the cluster.
- [ ] Add **Aurora Replicas auto scaling** based on CPU.
- [ ] Create an **Aurora Global Database** with a secondary Region. Run a managed planned failover and an unplanned "detach and promote". Record RPO and RTO.
- [ ] Use **Aurora Machine Learning** or **Data API** conceptually, and export a snapshot to S3.
- [ ] Review **Database Activity Streams** and **encryption** with KMS multi-Region keys.

**Break it**

- Force a failover of the writer; measure the time for reader promotion.
- Stop the cluster (it auto-restarts after 7 days) and discuss why you cannot stop it indefinitely.

**Exam angles**

- Aurora storage: 6 copies across 3 AZs, self-healing, up to 128 TiB. Failover typically under a minute.
- Global Database: replication lag typically under a second, RTO of about a minute for Regional failover.
- When to choose Aurora versus RDS versus DynamoDB, plus Serverless v2 versus provisioned.

**Cost and cleanup:** Aurora and Global Database are costly. Do it in a single session and delete everything, including automated backups and snapshots.

---

## CS-20: DynamoDB Data Modeling, Streams, DAX and Global Tables `[***]`

**Scenario:** A gaming company needs single-digit-millisecond leaderboards and player profiles at any scale, a real-time feed of changes, automatic data expiry, and multi-Region active-active.

**Build**

- [ ] Design a **single table** (`PK=PLAYER#id`, `SK=...`) or multiple tables; justify the **partition key** choice to avoid hot partitions.
- [ ] Create a table with **on-demand** capacity, load data, then switch to **provisioned with auto scaling** and compare the cost model (RCU/WCU maths: 1 RCU = one strongly consistent 4 KB read per second).
- [ ] Add a **GSI** (for leaderboard by game and score) and an **LSI** (must be created at table creation). Observe **eventual versus strongly consistent reads**.
- [ ] Enable **DynamoDB Streams** and trigger a **Lambda** that writes an audit item and publishes high scores to **SNS**.
- [ ] Enable **TTL** for session items; **PITR** and **on-demand backups**; **export to S3** and query with Athena.
- [ ] Place **DAX** in front of the table and measure read latency; understand what it does and does not cache.
- [ ] Create **Global Tables** in two Regions. Write in both and observe last-writer-wins conflict resolution.
- [ ] Use **transactions** (`TransactWriteItems`) and **conditional writes**/optimistic locking.
- [ ] Use **PartiQL** and the console's **item explorer**.

**Break it**

- Write to a single hot partition key at high rate to trigger throttling (`ProvisionedThroughputExceededException`) and fix with write sharding.
- Query with a filter expression and see that you still pay to read all scanned items.

**Exam angles**

- Capacity modes, DAX (microsecond reads, write-through) versus ElastiCache, Streams (24 hours) versus Kinesis Data Streams for DynamoDB.
- Global Tables need Streams and are multi-active. TTL deletes are free.
- Access patterns drive design; Scan is expensive.

**Cost and cleanup:** DAX nodes bill hourly. Delete DAX clusters and global table replicas.

---

## CS-21: Caching Strategies in Front of a Database with ElastiCache `[**]`

**Scenario:** A news site's article page makes 40 database queries and its login sessions are lost when a web server is replaced. Introduce a cache for both problems.

**Build**

- [ ] Create an **ElastiCache for Redis** replication group (Multi-AZ with automatic failover) in private subnets; allow access only from `app-sg`. Enable encryption in transit/at rest and **AUTH/RBAC**.
- [ ] Write a small app (Lambda or EC2) that implements **lazy loading (cache-aside)** with TTL, then **write-through**, and compare hit rate and staleness.
- [ ] Store web **sessions** in Redis so any instance can serve any user (stateless web tier).
- [ ] Create a Redis **cluster mode enabled** setup with shards and test failover. Compare with **Memcached** (multi-threaded, no persistence).
- [ ] Use **Redis sorted sets** for a leaderboard or **pub/sub** for notifications.
- [ ] Monitor `CacheHits`, `CacheMisses`, `Evictions`, `CPUUtilization` and set alarms.

**Break it**

- Flush the cache and observe the "cold cache" spike on the database. Design **cache warming**.
- Set a TTL that is too long and demonstrate stale reads; then define an invalidation strategy.

**Exam angles**

- Redis: persistence, replication, Multi-AZ, backups, sorted sets, pub/sub. Memcached: simple, multi-node sharding, no replication.
- Caching patterns: lazy loading, write-through, TTL.
- Where to place caching (CloudFront, API Gateway cache, DAX, ElastiCache).

**Cost and cleanup:** Delete the replication group and take no final backup if not needed.

---

# MODULE 9: Serverless Architectures

**Course section:** Serverless Overview (Lambda, API Gateway, Cognito, Step Functions), Serverless Solution Architecture Discussions.

## CS-22: Secure Serverless REST API with Cognito, Throttling and Canary Releases `[***]`

**Scenario:** A mobile app needs a backend that scales from 0 to 10,000 requests per second with no servers to patch, per-user authentication, partner-specific rate limits and safe rollouts.

**Build**

- [ ] Create a **DynamoDB** table and **Lambda** functions (`createItem`, `getItem`, `listItems`) with least-privilege execution roles.
- [ ] Create an **API Gateway REST API** with Lambda proxy integrations, **request validation**, **models** and CORS. Compare with **HTTP API** (cheaper, simpler) and a **WebSocket API**.
- [ ] Create a **Cognito User Pool** (sign-up, MFA optional) and add a **Cognito authorizer**; test with a JWT from the hosted UI. Add a **Lambda authorizer** for a custom token and compare.
- [ ] Add a **Cognito Identity Pool** to give authenticated users temporary AWS credentials for direct S3 uploads to `uploads/${cognito-identity.amazonaws.com:sub}/*`.
- [ ] Configure **usage plans and API keys** with throttling and quotas for two partners. Add **stage variables** and **stages** (dev, prod). Enable **API caching** and **CloudWatch/X-Ray** tracing and access logging.
- [ ] Use a **canary release** on the prod stage (10% traffic to new deployment).
- [ ] Tune Lambda: **memory/CPU**, **timeout**, **reserved concurrency**, **provisioned concurrency**, **environment variables with KMS**, **layers**, **versions and aliases** (weighted alias for canary), **Lambda in a VPC** (and why it then needs a NAT or endpoints), **function URLs**.
- [ ] Add a **dead-letter queue/destinations** for async invocations and test retry behavior.
- [ ] Attach **AWS WAF** to the API stage.

**Break it**

- Exceed the throttle limit and see `429`. Differentiate account-level, stage-level and usage-plan throttling.
- Put a Lambda in a private subnet with no route out and watch an API call to the internet time out.
- Set a Lambda timeout longer than API Gateway's 29-second default integration timeout. What does the client see?
- Cause a cold start and measure it; then reduce using provisioned concurrency/SnapStart (Java).

**Exam angles**

- Lambda limits: 15 minutes, up to 10 GB memory, 10 GB `/tmp`, 6 MB sync payload, 1,000 default regional concurrency.
- Cognito user pools (authentication, JWT) versus identity pools (AWS credentials).
- API Gateway endpoint types: Edge-optimized, Regional and Private. REST versus HTTP API differences.

**Cost and cleanup:** Mostly pay-per-use. Delete API, Cognito pools and DynamoDB tables.

---

## CS-23: Order Processing Workflow with Step Functions and Saga Compensation `[***]`

**Scenario:** An e-commerce checkout involves reserving stock, charging a card, and shipping. If any step fails, previous steps must be undone, and a human must approve orders above USD 1,000.

**Build**

- [ ] Create Lambda functions: `reserveInventory`, `chargePayment`, `createShipment`, `releaseInventory`, `refundPayment`.
- [ ] Define a **Step Functions state machine** (Standard) using the visual designer: **Task, Choice, Parallel, Map, Wait, Catch/Retry with exponential backoff, Fail, Succeed**.
- [ ] Implement **saga compensation**: on payment failure run `releaseInventory`; on shipment failure run `refundPayment` then `releaseInventory`.
- [ ] Add a **human approval** step using the **callback pattern** (`.waitForTaskToken`) that sends an SNS email and waits.
- [ ] Integrate directly with **DynamoDB, SNS and SQS** through the SDK integrations (no Lambda glue code).
- [ ] Compare **Standard vs Express** workflows (duration, pricing, exactly-once versus at-least-once) and trigger via API Gateway or EventBridge.
- [ ] Inspect execution history, redrive a failed execution, and add CloudWatch alarms on `ExecutionsFailed`.

**Break it**

- Make `chargePayment` fail 50% of the time and watch retries and compensation.
- Exceed the history size with a large Map and learn about **distributed Map** and payload limits (256 KB).

**Exam angles**

- Step Functions for orchestration, SQS/SNS/EventBridge for decoupling (choreography).
- Standard (up to 1 year, exactly-once) versus Express (up to 5 minutes, high volume).
- Use Step Functions instead of Lambda-calls-Lambda chains.

**Cost and cleanup:** Cheap; delete state machines and functions.

---



---

# MODULE 10: Decoupling and Event-Driven Design

**Course section:** Decoupling Applications: SQS, SNS, Kinesis, Amazon MQ; Serverless Solution Architecture Discussions.

## CS-24: SQS and SNS Fan-Out with Filtering, DLQs and FIFO Ordering `[***]`

**Scenario:** When a customer places an order, five systems care: billing, inventory, analytics, email and a fraud engine. A traffic spike must never overwhelm the billing system, poison messages must not block the queue, and payment events must be processed in order, exactly once.

**Build**

- [ ] Create an **SNS topic** `order-events` and subscribe **three SQS queues** (billing, inventory, analytics), an **email** endpoint and a **Lambda**. Fix the **SQS access policy** so SNS can send.
- [ ] Add **SNS subscription filter policies** (for example `{"orderType":["premium"]}`) so queues receive only relevant messages. Use **message attributes**.
- [ ] Configure each queue with **visibility timeout**, **long polling** (20 s), **delay seconds**, **message retention**, and a **dead-letter queue** (`maxReceiveCount=3`). Use **DLQ redrive** to push messages back after a fix.
- [ ] Build a **consumer Lambda** with an **event source mapping**: batch size, batch window, **partial batch response** (`ReportBatchItemFailures`) and **maximum concurrency**.
- [ ] Create a **FIFO queue** with **message group IDs** (per customer) and **deduplication** (content-based and explicit ID). Create an **SNS FIFO topic** feeding it. Measure throughput with and without **high-throughput mode**.
- [ ] Create an **ASG scaled on queue depth** (`ApproximateNumberOfMessagesVisible` per instance via a custom metric/backlog-per-instance) for an EC2 worker fleet.
- [ ] Practice **large payloads** using the pattern: store in S3 and send a pointer (Extended Client Library).
- [ ] Compare with **Amazon MQ** (ActiveMQ/RabbitMQ) for protocol-compatible migrations.

**Break it**

- Set the visibility timeout shorter than processing time and observe duplicate processing. Design **idempotency** (conditional DynamoDB write).
- Send a "poison pill" message and watch it land in the DLQ. Set an alarm on DLQ depth.
- Stop the billing consumers for 10 minutes while the producer keeps sending. Show that the queue absorbs the spike and the backlog drains after recovery.

**Exam angles**

- SQS standard (at-least-once, best-effort ordering, nearly unlimited throughput) versus FIFO (ordering per group, exactly-once processing, 300/3,000 msg/s with batching).
- SNS (push, pub/sub, fan-out) versus SQS (pull, queue) versus Kinesis (ordered streams, replay) versus EventBridge (rules-based routing and SaaS events).
- Scale consumers on queue length. Max message size 256 KB (up to 1 MiB for some newer configurations; verify current limits), retention 1 minute to 14 days.

**Cost and cleanup:** Almost free. Purge/delete queues and topics.

---

## CS-25: Real-Time Streaming Pipeline with Kinesis, Firehose, EventBridge and Athena `[***]`

**Scenario:** A ride-sharing app emits GPS pings and trip events at thousands per second. Operations need real-time anomaly alerts, the data science team needs a queryable history, and several SaaS and internal systems must react to business events.

**Build**

- [ ] Create a **Kinesis Data Stream** (provisioned with 2 shards, then try on-demand). Write records with a **partition key** (driver ID) using the console/CLI/KPL sample and watch **shard-level metrics**.
- [ ] Build a **consumer** (Lambda with event source mapping, parallelization factor and bisect-on-error) that detects anomalies and publishes to **SNS**.
- [ ] Create a **Kinesis Data Firehose** delivery stream: source the Kinesis stream, **transform with Lambda**, convert to **Parquet** using a Glue table schema, buffer by size/time, and deliver to **S3** with a partitioned prefix.
- [ ] Query the S3 data with **Athena** (partitions, `MSCK REPAIR TABLE` or partition projection).
- [ ] Demonstrate **replay**: use a new consumer with `TRIM_HORIZON` and a timestamp iterator to reprocess data.
- [ ] Create an **EventBridge** custom bus: rules with **event patterns**, targets (Lambda, SQS, Step Functions, API destination), **schedule** rules, **archive and replay**, **cross-account** event bus, and the **schema registry**.
- [ ] Try **EventBridge Pipes** (SQS to enrichment to target) and **EventBridge Scheduler** for one-time schedules.
- [ ] Compare against **Amazon MSK** (managed Kafka), and decide when to use it (Kafka API compatibility).

**Break it**

- Use a single partition key for all records and see a "hot shard" and `WriteProvisionedThroughputExceeded`. Fix with a better key or on-demand mode.
- Add a slow consumer and observe `GetRecords.IteratorAgeMilliseconds` rising.
- Deliver malformed records and inspect the **Firehose error output prefix** in S3.

**Exam angles**

- Kinesis Data Streams: ordering per shard, retention 24 hours to 365 days, replay, multiple consumers (enhanced fan-out 2 MB/s per consumer per shard). Firehose: near-real-time, fully managed, no replay, delivers to S3, Redshift, OpenSearch, Splunk, HTTP.
- Streams versus SQS: when ordering, multiple readers and replay are needed, choose Kinesis.
- EventBridge versus SNS: EventBridge offers content-based filtering, many SaaS sources and schema discovery.

**Cost and cleanup:** Shards are billed hourly. Delete streams, Firehose, and archives.

---

# MODULE 11: Containers on AWS

**Course section:** Containers on AWS (ECS, Fargate, ECR, EKS, App Runner), Classic Solutions Architecture.

## CS-26: Microservices on ECS Fargate with ALB, Service Discovery and Secrets `[***]`

**Scenario:** Your company is breaking a monolith into three containerized services (`web`, `orders`, `payments`). They must scale independently, communicate privately by name, fetch secrets securely, and have images scanned for vulnerabilities.

**Build**

- [ ] Create **ECR** repositories, push images (build locally or via CloudShell). Enable **scan on push** (basic or enhanced with Inspector), **immutable tags**, a **lifecycle policy** and **cross-Region replication**.
- [ ] Create an **ECS cluster** and **task definitions** with CPU/memory sizes, **awsvpc** network mode, **task role** (what the app can call) versus **task execution role** (what ECS needs to pull images, write logs and fetch secrets), **awslogs** driver, and **secrets from Secrets Manager/Parameter Store** injected as environment variables.
- [ ] Create **ECS services** on **Fargate** in private subnets behind an **ALB** (path routing, dynamic ports not needed with awsvpc) with **health checks** and deployment settings (**rolling update**, min healthy percent, max percent, **deployment circuit breaker with rollback**).
- [ ] Enable **ECS Service Connect** or **Cloud Map service discovery** so `web` calls `http://orders.local` without a load balancer.
- [ ] Configure **Service Auto Scaling** (target tracking on CPU and `ALBRequestCountPerTarget`; step scaling on SQS depth for a worker service).
- [ ] Use **Fargate Spot** through a **capacity provider strategy** (base on regular Fargate, burst on Spot).
- [ ] Add **EFS** volumes to tasks for shared persistent storage.
- [ ] Try **blue/green deployment** with **CodeDeploy** for ECS and a test listener.
- [ ] Run a scheduled task via **EventBridge** and a one-off task with `run-task`.
- [ ] Review **FireLens, Container Insights** metrics and **ECS Exec** for debugging.

**Break it**

- Remove the execution role's permission to read a secret; read the task stopped reason.
- Deploy a bad image tag; confirm the circuit breaker rolls back automatically.
- Let the tasks in private subnets lose a route to ECR; note which VPC endpoints are required (ECR API, ECR DKR, S3 gateway, CloudWatch Logs).

**Exam angles**

- ECS (AWS-native) versus EKS (Kubernetes) versus App Runner (simplest) versus Lambda container images. Fargate (serverless compute) versus EC2 launch type (control, GPU, cost at scale).
- Task role versus execution role; awsvpc gives each task its own ENI and security group.
- ECS on EC2 needs the **ECS agent** and ECS-optimized AMI; dynamic port mapping with ALB in bridge mode.

**Cost and cleanup:** Scale services to 0, delete services, cluster, ALB and ECR images.

---

## CS-27: ECS on EC2 with Capacity Providers, plus an EKS Walk-Through `[****]`

**Scenario:** A data-heavy workload needs GPU or large-memory instances and a 60% cost saving using Spot. The platform team also wants to evaluate Kubernetes.

**Build**

- [ ] Create an **ECS cluster with an ASG capacity provider** (managed scaling, target capacity 80%, managed termination protection). Run services in **bridge** and **awsvpc** modes and understand **dynamic port mapping**.
- [ ] Use **task placement strategies and constraints** (spread across AZs, binpack on memory, `distinctInstance`).
- [ ] Use **Spot instances with multiple instance types** in the ASG. Handle Spot interruption draining (`ECS_ENABLE_SPOT_INSTANCE_DRAINING`).
- [ ] Add a **mixed capacity provider strategy**: base 2 tasks on On-Demand, rest on Spot.
- [ ] Optional **EKS** exercise (control plane costs money, so keep it to about an hour): create a cluster with **managed node groups** or **Fargate profile**, deploy a sample app, expose it with the **AWS Load Balancer Controller**, and use **IRSA / EKS Pod Identity** for pod-level IAM.
- [ ] Summarize in a table: ECS vs EKS vs Fargate vs App Runner by control, portability, operational overhead and cost.

**Break it**

- Scale tasks beyond cluster capacity and observe how the capacity provider adds instances.
- Terminate a Spot instance and watch tasks rescheduled.

**Exam angles**

- Capacity providers connect tasks to ASGs or Fargate; managed scaling avoids manual EC2 scaling.
- EKS: data plane options (managed node groups, self-managed, Fargate); storage (EBS CSI, EFS CSI); networking (VPC CNI gives pods VPC IPs).
- Multi-AZ spreading for HA.

**Cost and cleanup:** Delete the EKS cluster and node groups first, then load balancers created by the controller, then the VPC.

---

# MODULE 12: Security, Encryption and Threat Detection

**Course section:** AWS Security and Encryption (KMS, SSM Parameter Store, Secrets Manager, CloudHSM, ACM, WAF, Shield, Firewall Manager, GuardDuty, Inspector, Macie), IAM Advanced.

## CS-28: KMS Mastery: Key Policies, Grants, Envelope Encryption and Cross-Account Sharing `[***]`

**Scenario:** A bank must encrypt data at rest with keys that it controls, rotate them annually, restrict who can use versus administer a key, and share an encrypted database snapshot with a DR account in another Region.

**Build**

- [ ] Create a **customer managed symmetric key** with a custom **key policy** separating **key administrators** from **key users**. Enable **automatic rotation**; learn what rotation changes (backing key) and what stays (key ID/ARN).
- [ ] Use the **AWS CLI** to `generate-data-key`, encrypt a file locally with the plaintext data key, store the encrypted key with the data (**envelope encryption**), and decrypt. Note why `Encrypt` is limited to 4 KB.
- [ ] Use **encryption context** and see a decrypt fail when it does not match.
- [ ] Create **grants** for a service and show how a **grant** differs from a key policy.
- [ ] Encrypt an **EBS volume, S3 bucket, RDS database, SQS queue and a Secrets Manager secret** with the key. Look at the CloudTrail `kms:Decrypt` events.
- [ ] **Share an encrypted RDS snapshot** with another account: share the CMK (key policy) and the snapshot; in the target account **copy the snapshot re-encrypting** with its own key. Show why the default AWS-managed key cannot be shared.
- [ ] Create a **multi-Region key**, replicate it and use it in two Regions (DynamoDB global tables or S3 CRR with SSE-KMS). Compare with separate keys per Region.
- [ ] Import your **own key material** (BYOK) and observe the deletion/expiry behavior. Compare with **CloudHSM** and **external key store** at a conceptual level.
- [ ] **Schedule key deletion** (7 to 30 days), then cancel it. Show that data encrypted by a deleted key is unrecoverable.

**Break it**

- Remove a role from the key policy while the IAM policy allows `kms:*`. What happens? (Denied, unless the key policy delegates to IAM.)
- Disable the key and watch a dependent EBS-backed instance fail to start.

**Exam angles**

- KMS keys are **Regional**; multi-Region keys are an explicit feature. AWS-managed keys, AWS-owned keys and customer-managed keys differ in control and cost.
- Envelope encryption is how KMS encrypts large data.
- KMS (shared HSM multi-tenant, FIPS 140-2 L3 for HSMs) versus CloudHSM (dedicated single-tenant, you manage keys, FIPS 140-2 L3).

**Cost and cleanup:** Keys cost about USD 1/month each plus requests. Schedule deletion at the end.

---

## CS-29: Secrets, Parameters and Certificates: Rotate Without Downtime `[**]`

**Scenario:** Developers keep database passwords in environment variables and Git. Security demands that secrets rotate every 30 days automatically with no application changes, config values are centrally managed, and all traffic uses valid TLS.

**Build**

- [ ] Store a **database credential in Secrets Manager** for your RDS instance and enable **automatic rotation** (uses a Lambda in your VPC; inspect the multi-user rotation strategy). Retrieve it from an EC2/Lambda via IAM role and see the version stages (`AWSCURRENT`, `AWSPREVIOUS`).
- [ ] Create a **Parameter Store hierarchy** (`/prod/app/db/host`, `/prod/app/feature-flags`) with **String, StringList and SecureString** (KMS) types. Restrict access by path in IAM. Use **parameter policies** (expiration notification) on advanced parameters. Read them from a Lambda and from CloudFormation dynamic references.
- [ ] Compare **Secrets Manager** (rotation, cross-Region replication, pricing per secret) with **Parameter Store** (free standard tier, no native rotation).
- [ ] Request a **public certificate in ACM** with DNS validation (Route 53 auto-adds the record), attach to an ALB and CloudFront, and test **auto-renewal**. Import a third-party certificate and see that imported certs **do not auto-renew**.
- [ ] Set up **ACM Private CA** conceptually for internal mTLS.
- [ ] Enforce HTTPS: redirect 80 to 443, configure a **TLS security policy** on the ALB, and use **HSTS** header via CloudFront response policy.

**Break it**

- Trigger a rotation while the app caches the old password and see the connection fail. Design retry and refresh logic.
- Let a DNS validation record get deleted and observe the certificate renewal failure alarm in ACM/Health.

**Exam angles**

- Secrets Manager for credentials with rotation (RDS, Redshift, DocumentDB natively). Parameter Store for configuration and cheap secrets.
- ACM public certs are free, regional; for CloudFront they must be in **us-east-1**. ACM cannot export public certs' private keys.
- Never bake secrets into AMIs, user data or code.

**Cost and cleanup:** Each secret has a monthly price. Delete secrets (with recovery window) and certificates.

---

## CS-30: Threat Detection and Automated Remediation `[****]`

**Scenario:** The SOC wants the account to defend itself: detect compromised instances, block malicious web traffic, flag public or unencrypted resources, find sensitive data, and automatically remediate common misconfigurations.

**Build**

- [ ] Enable **GuardDuty** (in all Regions if possible). Generate **sample findings**; create a few real ones (for example, an instance querying a known test domain, a port scan from another test instance within your own account is allowed under AWS policy for your own resources).
- [ ] Create an **EventBridge rule** on GuardDuty findings (severity above 7) triggering a **Lambda** that isolates the instance (swap its SG to a deny-all SG, snapshot its volume) and notifies **SNS**.
- [ ] Enable **AWS Config** with managed rules (`s3-bucket-public-read-prohibited`, `restricted-ssh`, `encrypted-volumes`, `root-account-mfa-enabled`). Create **automatic remediation** using **SSM Automation** documents. Make a bucket public and watch it be fixed.
- [ ] Run **Amazon Inspector** against EC2, ECR images and Lambda; interpret **CVE findings** and the network reachability finding.
- [ ] Use **Macie** on a bucket seeded with fake PII.
- [ ] Turn on **Security Hub** with the **AWS Foundational Security Best Practices** standard and aggregate findings (also **Detective** for investigation, conceptual).
- [ ] Build a **WAF web ACL**: managed rule groups, **rate-based rule**, **geo-match**, **IP set** block list, **custom rule (header/size)** and **CAPTCHA** action; log to S3/Firehose. Attach to ALB/CloudFront.
- [ ] Study **Shield Standard vs Advanced** and **Firewall Manager** for org-wide WAF/SG policy (conceptual or hands-on within an Organization).
- [ ] Use **AWS Network Firewall** (stateful rule groups with domain filtering) in an inspection VPC (conceptual if budget requires).
- [ ] Create a **Trusted Advisor** report review for security checks.

**Break it**

- Disable GuardDuty or CloudTrail and verify that your SCP (CS-02) and a Config rule detect/prevent it.
- Open port 22 to the world and measure time-to-remediate.

**Exam angles**

- Service-purpose map: GuardDuty (threat detection from CloudTrail, VPC Flow Logs, DNS logs), Inspector (vulnerabilities), Macie (PII in S3), Config (configuration compliance and history), Security Hub (aggregation), Detective (root-cause), WAF (L7), Shield (DDoS), Firewall Manager (centralized policies), Network Firewall (VPC L3-L7).
- Config tracks **what changed and compliance over time**; CloudTrail tracks **who did what**.

**Cost and cleanup:** Disable Macie, Inspector, Config and Security Hub if you do not need them; delete rules and recorders.

---

# MODULE 13: Monitoring, Auditing and Operations

**Course section:** AWS Monitoring, Audit and Performance (CloudWatch, EventBridge, CloudTrail, Config, X-Ray, Systems Manager), Other Services.

## CS-31: Full-Stack Observability and Audit Trail `[***]`

**Scenario:** The on-call team learns about outages from customers. You must create dashboards, alarms that matter, centralized logs, tracing and a tamper-evident audit trail that alerts on risky API calls.

**Build**

- [ ] Install the **CloudWatch agent** on EC2 using SSM; collect **memory, disk and custom application metrics** (not collected by default) and log files. Set **log retention** and an **encryption key** for the log group.
- [ ] Create **metric filters** on logs (count `ERROR`), and **alarms** (static threshold, **anomaly detection** and **composite alarm** to reduce noise). Send actions to **SNS**, **Auto Scaling**, **EC2 actions** (recover/reboot) and **Systems Manager OpsItem**.
- [ ] Use **Logs Insights** to query across log groups (top 10 slowest requests, error rates by minute). Create **subscription filters** that stream logs to Kinesis Firehose/Lambda/OpenSearch. Export logs to S3.
- [ ] Build a **CloudWatch dashboard** with golden signals; add **Contributor Insights** and **Container Insights** or **Lambda Insights** where relevant.
- [ ] Create a **CloudWatch Synthetics canary** that tests a login flow every 5 minutes and alerts on failure.
- [ ] Enable **AWS X-Ray** (or ADOT) on a Lambda + API Gateway app and analyze the **service map** and **traces**.
- [ ] Build an **organization trail** (or multi-Region trail) with **log file validation**, **CloudTrail Lake** or **Athena** queries, **data events** for a sensitive bucket and **Insights events** (unusual API activity).
- [ ] Create **EventBridge rules** to alert on: root login, `DeleteTrail`, `AuthorizeSecurityGroupIngress` with `0.0.0.0/0`, IAM policy attachments, and console login without MFA.
- [ ] Use **AWS Health Dashboard** events with EventBridge to alert on EC2 retirement.

**Break it**

- Create a runaway process consuming memory and verify the alarm fires and the notification arrives. Check the alarm state-transition history.
- Delete a CloudTrail log file and run **log file validation** to detect tampering.

**Exam angles**

- CloudWatch (metrics, logs, alarms; standard 1-min/5-min, high-resolution 1-second custom metrics) versus CloudTrail (API audit) versus Config (resource state) versus X-Ray (tracing).
- Default EC2 metrics exclude memory and disk usage.
- Cross-account, cross-Region observability and log aggregation patterns.

**Cost and cleanup:** Detailed monitoring, custom metrics, Synthetics and log ingestion cost money. Delete canaries and set short log retention.

---

## CS-32: Fleet Operations with Systems Manager (No SSH, No Bastions) `[**]`

**Scenario:** You manage 50 servers across accounts. You need patching with maintenance windows, auditable remote access, bulk command execution, and automated remediation runbooks, without opening inbound ports.

**Build**

- [ ] Attach the **AmazonSSMManagedInstanceCore** role to several instances and verify they appear as **managed nodes** in **Fleet Manager**. Fix the ones that do not (agent, role, or no route/endpoint).
- [ ] Connect with **Session Manager**, log sessions to S3/CloudWatch, restrict by tag with IAM, and **port forward** to a private RDS endpoint.
- [ ] Use **Run Command** to run a script on instances targeted by tag and view per-instance output; use **rate control** (concurrency and error thresholds).
- [ ] Configure **Patch Manager**: a **patch baseline**, **patch groups** and a **maintenance window** with **scan then install** during off-hours.
- [ ] Use **State Manager** associations to keep a config (for example the CloudWatch agent) enforced; use **Inventory** to list software across the fleet.
- [ ] Write an **Automation runbook** (for example stop an instance, create an AMI, restart) and trigger it from **EventBridge**.
- [ ] Use **Parameter Store** and **AppConfig** for config and feature flags (gradual deployment with a rollback alarm).
- [ ] Try **Explorer/OpsCenter** and **Resource Data Sync** to see the fleet across accounts.

**Break it**

- Remove the instance profile from an instance and watch it disappear from the managed nodes list.
- Run a command with a bad script on all nodes and learn how to limit blast radius.

**Exam angles**

- Session Manager replaces bastion hosts and SSH keys. Patch Manager and Maintenance Windows for patching. Automation for runbooks.
- SSM works for hybrid servers via **hybrid activations**.

**Cost and cleanup:** Mostly free; delete instances and endpoints.

---

# MODULE 14: Data, Analytics and AI Services

**Course section:** Data and Analytics (Athena, Redshift, OpenSearch, EMR, Glue, QuickSight, Lake Formation, MSK), Machine Learning services.

## CS-33: Build a Serverless Data Lake with Fine-Grained Governance `[****]`

**Scenario:** A retailer wants analysts to query clickstream and sales data without managing servers, with column-level access control (marketing must not see email addresses), cost control on queries, and dashboards for executives.

**Build**

- [ ] Land raw data in **S3** (`raw/`, `curated/`, `athena-results/`) via Firehose (from CS-25) and CSV uploads. Enforce encryption and lifecycle.
- [ ] Create a **Glue crawler** to discover schemas into the **Glue Data Catalog**. Write a **Glue ETL job** (visual or PySpark) converting CSV to partitioned **Parquet** with compression.
- [ ] Query with **Athena**. Compare scanned bytes (and cost) on CSV versus partitioned Parquet. Set a **workgroup** with a per-query data-scan limit and result encryption. Try **Federated queries** (conceptual).
- [ ] Register the lake in **Lake Formation**; grant **database/table/column/row-level permissions** to two IAM roles (analyst, marketing). Verify access using Athena as each role.
- [ ] Build a **QuickSight** dashboard on the Athena data set (SPICE vs direct query); share with a group.
- [ ] Load part of the data into **Redshift** (or Redshift Serverless) and use **Redshift Spectrum** to query S3 as external tables. Compare use cases: Athena (ad-hoc, serverless) versus Redshift (complex, repeated warehouse workloads). Use **Redshift snapshots with cross-Region copy**.
- [ ] Index logs into **OpenSearch** for full-text search and dashboards (use a small instance or serverless; be wary of cost).
- [ ] Try **EMR** (transient cluster with Spark on Spot) conceptually or with a tiny cluster; understand node types (primary, core, task).
- [ ] Add **Glue Data Quality**/**DataBrew** to clean data (optional).

**Break it**

- Query unpartitioned CSV with `SELECT *` and compare scanned bytes to the partitioned Parquet query.
- Remove the Lake Formation grant and see "access denied" despite IAM permissions on S3.

**Exam angles**

- Athena (serverless SQL on S3, pay per TB scanned, use Parquet/ORC plus partitions). Redshift (columnar warehouse, Spectrum, Multi-AZ options, snapshots). OpenSearch (search, log analytics). EMR (Hadoop/Spark). Glue (serverless ETL and catalog). QuickSight (BI). Lake Formation (governance).
- Kinesis Data Firehose to S3 to Athena is the classic serverless analytics pipeline.

**Cost and cleanup:** Redshift, OpenSearch, EMR and QuickSight charges accrue quickly. Delete clusters/domains and cancel the QuickSight subscription when done.

---

## CS-34: AI Services Pipeline: Rekognition, Transcribe, Comprehend, Polly and Textract `[**]`

**Scenario:** A media company needs to automatically moderate uploaded images, transcribe and translate audio, detect sentiment in comments, and extract fields from scanned invoices, without building ML models.

**Build**

- [ ] S3 upload triggers Lambda that calls **Rekognition** (labels, moderation, face detection) and stores results in DynamoDB.
- [ ] Use **Transcribe** for an audio file, **Translate** to another language and **Polly** to produce speech output. Combine with Step Functions or an event chain.
- [ ] Use **Comprehend** for sentiment, entities and PII detection on sample text.
- [ ] Use **Textract** for a sample invoice (forms and tables) and **Comprehend Medical**/**Amazon Kendra**/**Lex** conceptually.
- [ ] Explore **SageMaker** at overview level (training, endpoints, **Ground Truth**, **Canvas**) and **Amazon Bedrock** (foundation model access) as a conceptual comparison.
- [ ] Add a **human review** step for low-confidence results (for example **Amazon A2I**, conceptual).

**Break it**

- Send an unsupported file format and handle the failure with DLQ and retry.

**Exam angles**

- Memorize the one-line purpose of every AI service (Rekognition: images/video, Transcribe: speech-to-text, Polly: text-to-speech, Translate, Comprehend: NLP, Lex: chatbots, Textract: OCR for documents, Kendra: enterprise search, Forecast, Personalize, Fraud Detector, SageMaker: build/train/deploy).

**Cost and cleanup:** Pay-per-use; keep samples small.


---

# MODULE 15: Migration and Hybrid Connectivity

**Course section:** Disaster Recovery and Migrations (DMS, SCT, Application Migration Service, DataSync, Snow Family, Storage Gateway, Transfer Family), VPC (Site-to-Site VPN, Direct Connect), AWS Storage Extras.

## CS-35: Simulated On-Premises Migration: Databases, Files and SFTP `[****]`

**Scenario:** A manufacturer is retiring its data center. It has a self-managed MySQL database that cannot stay offline for more than a few minutes, 5 TB of file shares, and partners who upload files via SFTP and cannot change their tooling.

**Build** *(simulate "on-prem" with EC2 instances in a separate VPC acting as the data center)*

- [ ] Install **MySQL on an EC2** instance in the "on-prem" VPC with sample data. Create an **Aurora MySQL** target.
- [ ] Use **AWS DMS**: create a replication instance, source/target endpoints, and a task with **full load + CDC** (ongoing replication). Turn on binary logging on the source. Run live inserts during migration and verify they arrive. Perform the cutover. Use **DMS validation**.
- [ ] Try the **Schema Conversion Tool / DMS Schema Conversion** conceptually for a heterogeneous migration (Oracle to PostgreSQL).
- [ ] Use **AWS DataSync**: install the agent (as an EC2 AMI) in the "on-prem" VPC, create locations (NFS source, S3 and EFS targets), schedule a task, verify integrity, and use bandwidth throttling and filters.
- [ ] Deploy **Storage Gateway (S3 File Gateway)** on an EC2 (instead of hardware), mount it via NFS from the "on-prem" client, and see files appear in S3 with local caching. Compare **Volume Gateway** (cached versus stored) and **Tape Gateway**.
- [ ] Create **AWS Transfer Family (SFTP)** with an S3 backend, a **service-managed** user and a scoped-down IAM role per user (home directory mapping); test with a standard SFTP client. Optionally attach a custom hostname via Route 53.
- [ ] Use **AWS Application Migration Service (MGN)** conceptually: replication agent, staging area, test and cutover launch; compare with **VM Import/Export**.
- [ ] Do a **Snow Family sizing exercise** on paper: given 80 TB over a 100 Mbps link, compute transfer time and choose between network transfer and Snowball Edge. Also review **Snowcone**, **Snowmobile** and edge computing use.
- [ ] Write a **migration plan using the 7 Rs** (Retire, Retain, Rehost, Relocate, Repurchase, Replatform, Refactor) for five hypothetical apps.

**Break it**

- Stop the DMS task mid-CDC and resume; verify no data loss.
- Change a source column type and see the failure; read DMS table statistics and logs.
- Throttle DataSync bandwidth to mimic a thin WAN link.

**Exam angles**

- DMS: homogeneous or heterogeneous, **CDC for minimal downtime**, Multi-AZ replication instance; SCT for schema conversion.
- Snow Family when network transfer would take more than about a week; DataSync for online transfers (NFS/SMB/HDFS/S3/EFS/FSx); Storage Gateway for hybrid ongoing access; Transfer Family for SFTP/FTPS/FTP into S3/EFS.
- Math: `time = size / bandwidth` (8 bits per byte, usable ~80% of the link).

**Cost and cleanup:** Terminate gateway instances, stop DMS tasks, delete replication instances and endpoints.

---

## CS-36: Hybrid Network: Site-to-Site VPN, Hybrid DNS and a Direct Connect Design `[****]`

**Scenario:** A hospital network must connect its data center to AWS with encryption, hybrid name resolution for `corp.local` and `*.aws.hospital.com`, and an eventual 10 Gbps dedicated link with VPN as backup.

**Build**

- [ ] In the "on-prem" VPC, launch an EC2 running **strongSwan/Libreswan** (the customer gateway device) with an Elastic IP. Disable source/dest check.
- [ ] In the cloud VPC, create a **Virtual Private Gateway** (or attach to the Transit Gateway), a **Customer Gateway** and a **Site-to-Site VPN connection** (static first, then **BGP dynamic**). Download the configuration and configure the strongSwan side. Check both tunnels come **UP**.
- [ ] Enable **route propagation** and test ping and SSH between private IPs across the tunnel. Set up **tunnel failover** testing (down one tunnel).
- [ ] Create **Route 53 Resolver inbound endpoint** (on-prem resolves AWS private names) and **outbound endpoint + forwarding rule** (AWS resolves `corp.local` via an on-prem DNS server such as a dnsmasq EC2).
- [ ] Attach the VPN to a **Transit Gateway** and connect a second VPC to demonstrate hub-and-spoke with a VPN (and discuss **ECMP** to aggregate VPN bandwidth).
- [ ] Draw (on paper) a **Direct Connect** design: **dedicated vs hosted connection**, **private VIF, public VIF, transit VIF**, **Direct Connect Gateway** for multi-Region, **LAG**, and **VPN over DX** for encryption; and a **resilient** design with two locations and two connections. Add a **Site-to-Site VPN as backup**.
- [ ] Compare **AWS Client VPN** (users to VPC) with Site-to-Site VPN. Create a Client VPN endpoint (certificate-based) if budget allows.

**Break it**

- Remove the return route on the "on-prem" side and diagnose a one-way ping.
- Change the pre-shared key and confirm the tunnel fails.
- Use overlapping CIDRs across on-prem and VPC and note why this blocks routing.

**Exam angles**

- Direct Connect: private, consistent latency, **not encrypted by default**, takes weeks to provision (so VPN is the immediate fallback). VPN: encrypted, over the internet, quick to set up, 1.25 Gbps per tunnel.
- DX Gateway lets a single DX reach many VPCs in many Regions; transit VIF reaches a Transit Gateway.
- Hybrid DNS via Resolver endpoints.

**Cost and cleanup:** Delete VPN connections and TGW attachments immediately after testing (hourly charges).

---

# MODULE 16: Disaster Recovery and High Availability

**Course section:** Disaster Recovery and Migrations, More Solution Architecture (AWS Backup, Elastic Disaster Recovery), RDS/Aurora, Route 53, S3.

> **Before you start:** write your own table of the four DR strategies (**Backup and Restore, Pilot Light, Warm Standby, Multi-Site Active-Active**) with RPO, RTO, cost and complexity. You will fill in real measured RPO/RTO numbers at the end of CS-37 to CS-39.

## CS-37: Backup and Restore Strategy with AWS Backup, Cross-Region and Cross-Account Copies `[***]`

**Scenario:** After a ransomware near-miss, the CISO wants immutable, centrally managed backups, copies in a separate account and Region, and a tested restore process. Acceptable RPO is 24 hours; RTO is 8 hours.

**Build**

- [ ] Tag resources (`Backup=daily`) across EC2/EBS, RDS, DynamoDB, EFS and S3.
- [ ] Create **AWS Backup plans**: schedule, lifecycle (cold storage, retention), **resource assignment by tag**, **continuous backup for PITR** (RDS/S3), **cross-Region copy**, and **cross-account copy** into a dedicated **backup vault** in a Security account (use Organizations backup policy if available).
- [ ] Configure **vault access policy** and **AWS Backup Vault Lock** (compliance mode needs a cooling-off; use governance mode for the lab) to make backups immutable. Prove that even an admin cannot delete.
- [ ] **Restore tests**: restore an EBS volume, an RDS instance, a DynamoDB table (to a new table) and an EFS file system. Record the **actual restore time** for each (your real RTO).
- [ ] Enable **Backup Audit Manager** reports and alarms on failed jobs.
- [ ] Accidentally delete an EBS snapshot and recover it with the **Recycle Bin**.
- [ ] Create **AMI and snapshot copy** to a second Region and launch from it.

**Break it**

- Delete the source resource and restore from the cross-account copy.
- Revoke the KMS key permissions on the backup and see how encrypted backups depend on keys (share the key correctly).

**Exam angles**

- Backup and restore is cheapest, with highest RTO/RPO. Use AWS Backup for centralized policy-driven protection.
- Vault Lock (WORM) protects against deletion and ransomware.
- Encryption keys must be available in the DR Region/account.

**Cost and cleanup:** Backup storage and cross-Region transfer cost money. Delete recovery points (Vault Lock may block; use short retention).

---

## CS-38: Pilot Light and Warm Standby Across Regions `[****]`

**Scenario:** An insurer's claims portal must recover in under 30 minutes (RTO) with at most 1 minute of data loss (RPO) if its primary Region fails. Budget forbids running a full duplicate environment.

**Build**

- [ ] Primary Region: the 3-tier app (ALB, ASG, Aurora/RDS, S3 for uploads) from earlier case studies.
- [ ] **Pilot light** in Region B: data tier always on (**Aurora Global Database secondary** or **cross-Region RDS read replica**, **S3 CRR**, **replicated secrets** and **copied AMIs/encrypted snapshots** with a Region B KMS key), compute tier defined but **ASG at 0** (via CloudFormation or Launch Template).
- [ ] Automate **failover runbook**: promote the DB, scale the ASG to N, update **Route 53 failover** (or switch the weighted record), and warm caches. Write it as **SSM Automation** or **Step Functions**.
- [ ] Configure **Route 53 failover routing** with a **health check** of the primary ALB and a **calculated health check** that also checks DB health through a CloudWatch alarm.
- [ ] Simulate a Regional failure: block the primary ALB (SG or Route 53 health check override or stop targets), time the failover, measure **RPO** (check last replicated transaction) and **RTO**.
- [ ] **Upgrade to warm standby**: run a scaled-down fleet (min 1 per AZ) in Region B and compare the RTO improvement and the added cost.
- [ ] **Fail back**: re-establish replication in the reverse direction and return to Region A. Document the steps.

**Break it**

- Fail the DB promotion midway and use your runbook to recover.
- Forget to replicate a Parameter Store parameter or a KMS multi-Region key and find which part of the stack breaks.
- Let DNS TTL be 1 hour and measure the effect. Reduce to 60 seconds.

**Exam angles**

- Pilot light (core data always running; compute off) versus warm standby (scaled-down full copy). Both involve Route 53 failover.
- Aurora Global Database (RPO ~1 s) versus cross-Region read replica (async; promote manually) versus snapshot copy (hours).
- Elastic Disaster Recovery (DRS) for server-level continuous replication from on-prem or other clouds.

**Cost and cleanup:** Global DB and second-Region compute cost money. Complete the exercise in a single sitting and tear down both Regions.

---

## CS-39: Active-Active Multi-Region Game Day `[****]`

**Scenario:** A global ticketing platform cannot be down during on-sale events. Leadership wants active-active across two Regions, no manual failover, and proof that it works through a "game day" chaos exercise.

**Build**

- [ ] Deploy identical stateless stacks (ALB, ASG or ECS Fargate, Lambda API) in two Regions, using **CloudFormation/StackSets** for parity.
- [ ] Data layer: **DynamoDB Global Tables** (multi-active) for orders and sessions; **S3 Multi-Region Access Point** or CRR for assets; **Aurora Global Database** (single writer) as an alternative for relational data. Document the conflict-resolution and consistency trade-offs.
- [ ] Edge: **Route 53 latency-based routing with health checks**, or **Global Accelerator** with endpoint groups in each Region, and a **CloudFront origin group** for failover.
- [ ] Define **SLOs** (99.95% availability, p95 latency) and **CloudWatch alarms/dashboards** per Region.
- [ ] **Game day experiments** using **AWS Fault Injection Service** and manual drills: (1) terminate 50% of instances in a Region, (2) inject network latency, (3) fail an AZ (block subnets with a NACL), (4) **fail the entire Region** (disable the ALB listener), (5) throttle DynamoDB, (6) make a dependency (Lambda) slow.
- [ ] Capture timings in a table: detection time, mitigation time, user impact, data loss. Update runbooks and write a **blameless post-incident review**.
- [ ] Review the design against the **Well-Architected Reliability pillar** (change management, failure management, workload architecture).

**Break it**

- Write the same DynamoDB item concurrently in two Regions and observe last-writer-wins. Design around conflicts (region-affine writes or idempotent merges).
- Fail only the database in one Region while the app tier is healthy. Does your health check detect this "gray failure"?

**Exam angles**

- Active-active gives near-zero RTO/RPO but highest cost and complexity.
- Global Accelerator versus Route 53 for failover speed (no DNS caching with GA).
- Data replication choice drives RPO; app statelessness drives RTO.

**Cost and cleanup:** Two Regions of everything. Use short sessions and delete via StackSets.

---

# MODULE 17: Infrastructure as Code and Deployment Strategies

**Course section:** CloudFormation, Elastic Beanstalk, CI/CD (CodePipeline/CodeDeploy), Other Services.

## CS-40: CloudFormation Deep Dive: Nested Stacks, Change Sets, Drift and StackSets `[***]`

**Scenario:** Platform engineering must provision identical, auditable environments (dev, test, prod) in minutes, safely update live stacks, detect manual drift, and roll out guardrail resources to every account.

**Build**

- [ ] Write a template for the 3-tier network (VPC, subnets, route tables, NAT) using **Parameters** (with constraints), **Mappings**, **Conditions** (create a NAT only in prod), **Outputs with Exports**, **Metadata** and **Intrinsic functions** (`Ref`, `Fn::GetAtt`, `Fn::Sub`, `Fn::Join`, `Fn::ImportValue`, `Fn::FindInMap`).
- [ ] Add an **EC2/ASG** template using **`cfn-init`**, **`cfn-signal`**, **CreationPolicy/WaitCondition** and **UpdatePolicy** (rolling update). Use **SSM Parameter** type parameters for the latest AMI.
- [ ] Break it into **nested stacks** (network, security, app) and reference outputs.
- [ ] Update with a **Change Set**; review replacement versus in-place updates. Set **stack policy** to protect the database.
- [ ] Use **`DeletionPolicy: Retain/Snapshot`** and **`UpdateReplacePolicy`** on RDS and S3. Delete the stack and verify.
- [ ] Manually change a security group, then run **drift detection** and read the result. Remediate.
- [ ] Trigger a failure (invalid AMI) and watch **automatic rollback**; test **rollback configuration with CloudWatch alarms**.
- [ ] Use a **custom resource** (Lambda-backed) to empty an S3 bucket on delete.
- [ ] Deploy a baseline (CloudTrail, Config rules, IAM roles) to multiple accounts/Regions using **StackSets** (self-managed or service-managed with Organizations).
- [ ] Import an existing resource into a stack (**resource import**) and generate a template from existing resources with **IaC generator**. Compare with **AWS CDK** (synthesize to CloudFormation) conceptually.

**Break it**

- Delete a resource referenced by an export and read the error.
- Cause a **circular dependency** and fix with `DependsOn` or restructure.
- Use `UPDATE_ROLLBACK_FAILED` (for example manually delete a resource) and recover using **continue update rollback**.

**Exam angles**

- Change Sets (preview), Drift detection, DeletionPolicy, StackSets (multi-account/multi-Region), nested stacks (reuse), `cfn-signal` (wait for instance readiness).
- CloudFormation versus Elastic Beanstalk versus OpsWorks versus CDK versus Terraform.

**Cost and cleanup:** Delete stacks (it is the point of IaC) and confirm retained resources are removed deliberately.

---

## CS-41: Deployment Strategies: Elastic Beanstalk, CodeDeploy and Blue/Green `[***]`

**Scenario:** The release team wants zero-downtime deployments, instant rollback and the ability to test new versions with real traffic before full cutover.

**Build**

- [ ] Deploy a sample app to **Elastic Beanstalk** (web server environment with ALB and RDS decoupled from the environment). Practice **deployment policies**: All-at-once, Rolling, Rolling with additional batch, **Immutable**, **Traffic splitting (canary)**. Time each and note capacity during deployment.
- [ ] Do a **blue/green swap** with **Swap Environment URLs** in Beanstalk.
- [ ] Use **CodeDeploy** (EC2/on-prem in-place with an `appspec.yml` and lifecycle hooks, then blue/green with an ASG) and **CodeDeploy for Lambda** (linear/canary with CloudWatch alarm rollback).
- [ ] Build a **CodePipeline** (CodeCommit/GitHub source, CodeBuild with unit tests, manual approval, CodeDeploy). Add **EventBridge** triggers and **artifact encryption**.
- [ ] Implement **blue/green with Route 53 weighted records** and with **ALB weighted target groups**; compare speed of rollback.
- [ ] Use **Lambda aliases and weighted routing** for a canary.
- [ ] Add **feature flags with AppConfig** to decouple deploy from release.

**Break it**

- Deploy a failing version during an immutable deployment; confirm that the old fleet remains.
- Trigger a CloudWatch alarm during a CodeDeploy canary to force an automatic rollback.

**Exam angles**

- Beanstalk policies: **All at once** (fastest, downtime), **Rolling** (reduced capacity), **Rolling with additional batch** (full capacity), **Immutable** (new ASG, safest rollback), **Traffic splitting** (canary), **Blue/green** (separate environments).
- Database inside the Beanstalk environment is deleted with it; decouple production databases.

**Cost and cleanup:** Terminate Beanstalk environments (they own ALB, ASG and optionally RDS), pipelines and artifacts bucket.

---

# MODULE 18: Cost Optimization and the Well-Architected Framework

**Course section:** AWS Billing and Cost Management, Well-Architected Framework (Whitepapers and Architectures), More Solution Architecture.

## CS-42: Cost Optimization Audit of a Wasteful Architecture `[***]`

**Scenario:** Your lab environment from previous weeks resembles a company that overspent 40% last quarter. As FinOps lead, find the waste, model savings, and set up controls so it does not return.

**Build**

- [ ] Use **Cost Explorer**: group by service, tag, linked account and usage type; view **forecast**, **rightsizing recommendations** and **Savings Plans/RI recommendations and coverage/utilization** reports.
- [ ] Enable **Cost and Usage Report** to S3 and query it with **Athena** (find top 10 costs, unattached EBS volumes, idle ELBs, NAT data processing, cross-AZ and cross-Region data transfer).
- [ ] Run **Compute Optimizer** (EC2, ASG, EBS, Lambda, ECS on Fargate) and **Trusted Advisor** cost checks (idle, underutilized, unassociated Elastic IPs).
- [ ] Model **purchase options** for four workloads and compare monthly cost: On-Demand, 1-year and 3-year Compute Savings Plan, EC2 Instance Savings Plan, Standard vs Convertible RI, Spot. Use the **AWS Pricing Calculator** and save an estimate.
- [ ] Apply **storage optimization**: S3 Intelligent-Tiering and lifecycle, gp2 to gp3, delete orphaned snapshots/AMIs, EFS-IA, **S3 Storage Lens** recommendations, Glacier tiers.
- [ ] Apply **network cost optimization**: VPC endpoints instead of NAT for S3/DynamoDB, one NAT per AZ trade-off, CloudFront to reduce egress, same-AZ traffic, and PrivateLink costs.
- [ ] Apply **serverless versus provisioned** break-even analysis (Lambda vs Fargate vs EC2) for a steady 50 requests/second workload.
- [ ] Add **governance**: mandatory tags enforced by **tag policies** and **SCPs**, **Budgets actions** (apply an IAM/SCP policy or stop instances when over budget), **Cost Anomaly Detection** alerts, and an **instance scheduler** (EventBridge Scheduler plus Lambda or SSM Automation) to stop dev instances at night.
- [ ] Complete a **Well-Architected Tool** review (create a workload and answer questions for Cost Optimization and Reliability) and note the high-risk items.

**Break it**

- Leave a NAT Gateway and a load balancer running over a weekend and find them using Cost Explorer's daily granularity.
- Run a data-transfer experiment: transfer 100 GB between AZs/Regions in the pricing calculator and see the delta.

**Exam angles**

- Spot for fault-tolerant/batch; Savings Plans/RIs for steady state; serverless for spiky; right-size before you commit.
- Data transfer in is free; between AZs and out to Internet costs; S3 to CloudFront is free.
- The six Well-Architected pillars: **Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability**.

**Cost and cleanup:** The CUR bucket and Athena are cheap; disable the report if you do not need it.

---

# CAPSTONE PROJECTS

Do these after finishing the modules. Each one integrates many services under realistic constraints. **Write an architecture decision record (ADR)** for every major choice: context, options considered, decision, consequences.

## CAP-1: Secure Multi-Account, Multi-Region E-Commerce Platform `[****]`

**Requirements**

- Accounts: Management, Security/Log Archive, Shared Services (network), Dev, Prod. Organizations with SCPs, Identity Center SSO and an organization-wide CloudTrail and Config.
- Network: hub-and-spoke with Transit Gateway in Shared Services, central egress with NAT, VPC endpoints for AWS services, no public subnets for compute, centralized inspection (Network Firewall or GWLB, conceptual acceptable).
- Application: CloudFront + WAF, ALB, ECS Fargate (web, orders, payments), Aurora PostgreSQL (Multi-AZ, encrypted with CMK), DynamoDB for carts/sessions, ElastiCache for caching, S3 for assets, SQS/SNS/EventBridge for order events, Step Functions for checkout saga, Cognito for customer identity.
- Security: KMS keys per data classification, Secrets Manager rotation, GuardDuty/Inspector/Macie/Security Hub aggregated into Security account, least-privilege roles, ABAC.
- Resilience: pilot light or warm standby in a second Region with Route 53 failover; AWS Backup with cross-account copy and Vault Lock; RTO 30 min, RPO 5 min.
- Operations: CloudWatch dashboards and alarms, X-Ray, SSM Session Manager and Patch Manager, CloudFormation/StackSets for everything repeatable.
- Cost: tagging, budgets, Savings Plans model, Spot for non-prod, lifecycle policies.

**Deliverables:** architecture diagram, ADRs, IaC, failure-injection results, DR runbook with measured RTO/RPO, cost estimate, and a Well-Architected self-review.

## CAP-2: Event-Driven Image and Document Intelligence Platform (Serverless) `[****]`

**Requirements**

- Users upload via pre-signed URLs or Cognito-authenticated API Gateway; objects land in S3 (SSE-KMS, versioning).
- S3 event to EventBridge routes by prefix and type to: Lambda (image resize), Step Functions (document OCR with Textract, then Comprehend for PII, then human approval), Rekognition (moderation).
- Results stored in DynamoDB (with GSIs) and searchable through OpenSearch; notifications via SNS/SES; DLQs and retry policies on every async hop.
- Analytics: Firehose to S3 Parquet, Athena, QuickSight, Lake Formation column permissions.
- Observability: X-Ray, structured logs, Logs Insights queries, alarms on DLQ depth and Step Functions failures.
- Security: WAF on API, least-privilege per-function roles, VPC endpoints for private integrations, Macie scans.
- Cost target: under USD 50 per million documents (justify with a pricing model).

**Deliverables:** diagram, state machine definition, per-hop failure-handling table, cost model, load test results (Lambda concurrency and SQS backlog behavior).

## CAP-3: Whiteboard Drills and Exam-Style Design Reviews `[***]`

Without the console, answer in writing. For each, state **requirement keywords**, **candidate services**, **why the winner wins**, **why the others lose**:

1. A legacy app needs shared POSIX storage across 200 EC2 instances in 3 AZs. (EFS, why not EBS Multi-Attach or S3)
2. A Windows file server migration with AD integration. (FSx for Windows)
3. Millions of IoT devices send telemetry; need real-time dashboards and long-term analytics. (IoT Core, Kinesis, Firehose, S3, Athena)
4. A public website with a global audience and DDoS and bot concerns. (CloudFront, WAF, Shield Advanced)
5. Hundreds of VPCs and on-prem sites need to communicate. (Transit Gateway, DX Gateway, RAM)
6. Provide third-party partners with access to one S3 prefix for one hour. (pre-signed URL or STS with session policy)
7. Reduce latency for users in Asia to a US-based, non-HTTP (UDP) game server. (Global Accelerator)
8. Ensure an RDS DB survives AZ failure and keeps reads scaling. (Multi-AZ + read replicas, or Aurora)
9. Encrypt data with keys you generate and fully control, with audit of every key use. (KMS imported key material or CloudHSM, depending on the wording)
10. Run a nightly 6-hour fault-tolerant batch at the lowest cost. (Spot with Batch/EMR/ASG)
11. Process uploaded files exactly once, in order, per customer. (SQS FIFO with message group ID)
12. Prevent any account from disabling logging org-wide. (SCP + organization trail)
13. Offload read traffic and ensure sub-millisecond reads for a DynamoDB hot key. (DAX)
14. Migrate a 200 TB archive with a 50 Mbps link and a 4-week deadline. (Snowball Edge, show the math)
15. Provide an RPO of seconds and RTO of minutes across Regions for a relational workload. (Aurora Global Database)

---

# APPENDIX A: Console Muscle-Memory Checklist

Tick each when you can do it without looking at notes:

- [ ] Create a VPC with public/private subnets, IGW, NAT and correct route tables
- [ ] Write an IAM policy with a condition and test it in the simulator
- [ ] Assume a role cross-account (console and CLI)
- [ ] Launch an EC2 with user data, an instance profile and no SSH key; connect via Session Manager
- [ ] Create an ALB with an ASG and target tracking policy
- [ ] Create and encrypt an EBS snapshot; copy it cross-Region
- [ ] Configure S3 versioning, lifecycle, replication and a bucket policy
- [ ] Create an RDS Multi-AZ instance, a read replica and perform a failover
- [ ] Create a Lambda with an SQS trigger, DLQ and reserved concurrency
- [ ] Publish an API behind API Gateway with a Cognito authorizer
- [ ] Create a CloudFront distribution with OAC and an ACM certificate
- [ ] Configure Route 53 failover with health checks
- [ ] Build an SNS to SQS fan-out with filter policy
- [ ] Deploy an ECS Fargate service behind an ALB with a task role
- [ ] Create a KMS key, share an encrypted snapshot cross-account
- [ ] Build a CloudFormation stack with a change set and drift detection
- [ ] Set up AWS Backup with cross-Region copy and restore a resource
- [ ] Create a Site-to-Site VPN (or explain every step) and a Transit Gateway attachment
- [ ] Query CloudTrail or VPC Flow Logs with Athena

# APPENDIX B: Quick "Which Service?" Decision Table

| If the question says... | Think... |
|---|---|
| Decouple, buffer a spike, ordering needed | SQS (FIFO for ordering) |
| One event, many subscribers | SNS fan-out (or EventBridge for rules and SaaS sources) |
| Real-time streaming, replay, multiple consumers | Kinesis Data Streams |
| Near-real-time delivery to S3/Redshift/OpenSearch | Kinesis Data Firehose |
| Relational, HA, global, low-latency failover | Aurora (Global Database) |
| Key-value, serverless scale, single-digit ms | DynamoDB (DAX for microseconds) |
| Shared Linux file system across AZs | EFS |
| Windows file shares (SMB, AD) | FSx for Windows File Server |
| HPC, S3-linked fast file system | FSx for Lustre |
| Static IPs, TCP/UDP, extreme performance | NLB (or Global Accelerator for global) |
| HTTP routing, WebSockets, Lambda targets | ALB |
| Cache static content globally | CloudFront |
| Orchestrate multi-step workflows | Step Functions |
| Run containers without managing servers | ECS/EKS on Fargate |
| Rotate DB credentials automatically | Secrets Manager |
| Store plain config and cheap secrets | SSM Parameter Store |
| Threat detection from logs and DNS | GuardDuty |
| Find PII in S3 | Macie |
| Scan EC2/ECR/Lambda for CVEs | Inspector |
| Track resource configuration and compliance | Config |
| Who made this API call? | CloudTrail |
| Restrict permissions org-wide | SCP |
| Delegate IAM safely | Permission boundary |
| Private access to AWS services from VPC | VPC endpoints (gateway for S3/DynamoDB) |
| Expose a service privately to other VPCs/accounts | PrivateLink (NLB + endpoint service) |
| Connect many VPCs and on-prem | Transit Gateway |
| Large offline data transfer | Snow Family |
| Online bulk data transfer to S3/EFS/FSx | DataSync |
| Low-downtime database migration | DMS with CDC |
| Centralized backup policy | AWS Backup |

# APPENDIX C: Final Two-Week Plan

1. **Days 1 to 4:** Redo the three labs where you struggled most, from a blank account state and **without notes**.
2. **Days 5 to 8:** Do CAP-3 drills plus full-length practice exams (for example from Tutorials Dojo or the course's own). For every wrong answer, write one sentence on **why the right answer is better** and re-check the matching case study.
3. **Days 9 to 11:** Re-read the "Exam angles" bullets of all case studies and the decision table in Appendix B.
4. **Days 12 to 13:** Light review, whitepapers summary (Well-Architected pillars, shared responsibility, DR whitepaper), rest.
5. **Exam day:** Read each question twice, underline constraints (cost, operational overhead, latency, availability, security), eliminate options that violate a constraint, and prefer **managed, serverless and least-operational-overhead** answers unless the question demands control.

**Good luck. Build, break, fix, repeat.**
