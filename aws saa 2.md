# AWS Certified Solutions Architect – Associate (SAA-C03)
## Advanced Hands-On Case Studies & Implementation Guide

---

## Overview
This document provides a set of topic-wise, scenario-driven case studies designed to reinforce the concepts taught in **Stephane Maarek’s AWS Solutions Architect Associate** course. Implementing these architectures using the AWS Management Console will help solidify your practical understanding of AWS services, security, scalability, and disaster recovery.

---

## Module 1: Identity & Access Management (IAM) & Governance

### Case Study: Multi-Tier Enterprise Access Control & Least Privilege Architecture

* **Scenario:**  
  FinTechCorp requires a secure access management strategy for three distinct personas: **Developers**, **Data Analysts**, and **Third-Party Security Auditors**. Developers need access to manage EC2 instances tagged as `Environment=Dev`, but strictly read-only access to S3. Data Analysts require query capabilities on S3 data lakes via Athena, but no direct infrastructure access. Third-party auditors need cross-account access for a limited time frame.

* **Key Objectives & Console Tasks:**
  1. **Cross-Account Role Setup:**
     * Configure an IAM Role for Third-Party Audit access using an external ID to mitigate the confused deputy problem.
  2. **Tag-Based Access Control (ABAC):**
     * Draft and attach an IAM Policy granting `ec2:StartInstances`, `ec2:StopInstances`, and `ec2:RebootInstances` *only* when the tag `Environment=Dev` matches.
  3. **Multi-Factor Authentication (MFA) Enforcement:**
     * Construct a policy that explicitly denies all actions except IAM password/MFA updates unless the user logged in with MFA.
  4. **Service Control Policies (SCPs) / Boundaries:**
     * Create an IAM Permission Boundary to limit developer permissions, preventing them from elevating their privileges or attaching `AdministratorAccess`.

---

## Module 2: Virtual Private Cloud (VPC) & Networking

### Case Study: Secure Multi-Tier Hybrid Network Architecture

* **Scenario:**  
  You are architecting a enterprise web network across two Availability Zones (AZs). The application requires public facing Load Balancers, private Application servers, and isolated Database servers with no internet access. Additionally, administrators need secure access to private instances without exposing SSH ports (Port 22) publicly.

```
+-------------------------------------------------------------------------------+
| AWS VPC (10.0.0.0/16)                                                         |
|                                                                               |
|  +---------------------------+       +---------------------------+            |
|  | Public Subnet 1 (AZ-A)     |       | Public Subnet 2 (AZ-B)     |            |
|  |  [ Application Load Balancer (ALB) ]                          |            |
|  +--------------+------------+       +--------------+------------+            |
|                 |                                   |                         |
|  +--------------v------------+       +--------------v------------+            |
|  | Private App Subnet (AZ-A)  |       | Private App Subnet (AZ-B)  |            |
|  |  [ EC2 Instance ]         |       |  [ EC2 Instance ]         |            |
|  +--------------+------------+       +--------------+------------+            |
|                 |                                   |                         |
|  +--------------v------------+       +--------------v------------+            |
|  | Isolated DB Subnet (AZ-A) |       | Isolated DB Subnet (AZ-B) |            |
|  |  [ Multi-AZ RDS Instance ]|       |  [ Standby RDS ]          |            |
|  +---------------------------+       +---------------------------+            |
+-------------------------------------------------------------------------------+
```

* **Key Objectives & Console Tasks:**
  1. **VPC Subnet & Gateway Design:**
     * Create a VPC (`10.0.0.0/16`) with 6 subnets across 2 AZs (2 Public, 2 Private App, 2 Isolated Database).
     * Deploy an Internet Gateway (IGW) for Public Subnets and NAT Gateways in each AZ for redundancy.
  2. **Route Table Management:**
     * Configure separate route tables:
       * **Public RT:** Route `0.0.0.0/0` to IGW.
       * **Private RT:** Route `0.0.0.0/0` to NAT Gateway.
       * **Database RT:** Local routes only (No internet routing).
  3. **Security Groups & Network ACLs:**
     * Layer Security Groups so ALB accepts HTTP/HTTPS traffic from `0.0.0.0/0`, App EC2 instances accept HTTP only from the ALB Security Group, and Database accepts TCP 3306/5432 only from the App Security Group.
  4. **SSM Session Manager & Endpoint Integration:**
     * Provision VPC Interface Endpoints for `ssm`, `ec2messages`, and `ssmmessages` to allow shell access to private EC2 instances without a Bastion Host or public IP.

---

## Module 3: Compute & Scaling (EC2, Auto Scaling, & Load Balancing)

### Case Study: High-Availability Dynamic Web Fleet with Auto Scaling & Custom Health Checks

* **Scenario:**  
  An E-commerce platform experiences drastic spikes in traffic during sales events. The application must scale automatically based on metric thresholds while ensuring zero downtime during application deployments or instance failures.

* **Key Objectives & Console Tasks:**
  1. **Golden AMI Creation & Launch Template:**
     * Launch a baseline EC2 instance, install Apache/Nginx, set up a custom metadata landing page using `user-data`, and build an Amazon Machine Image (AMI).
     * Create a Launch Template specifying AMI, instance type, IAM Role (S3 read access), and Security Group.
  2. **Application Load Balancer (ALB) Setup:**
     * Deploy a Multi-AZ ALB with path-based routing rules (e.g., `/api/*` routed to API target group, `/*` routed to Web target group).
     * Enable access logging to an S3 bucket with proper bucket policies.
  3. **Auto Scaling Group (ASG) & Target Tracking:**
     * Configure an ASG spanning multiple AZs with Target Tracking Scaling Policies based on Average CPU Utilization (50%) and ALB Request Count Per Target.
     * Implement dynamic scaling termination policies and lifecycle hooks for graceful shutdown logging.

---

## Module 4: Containerization (ECS, ECR, & Fargate)

### Case Study: Microservices Deployment using AWS ECS Fargate & ECR

* **Scenario:**  
  A modern SaaS startup is transitioning from monolith EC2 deployments to serverless microservices running Docker containers. The company wants to minimize infrastructure management while securing container image lifecycle management.

* **Key Objectives & Console Tasks:**
  1. **Amazon ECR Management:**
     * Build a Dockerized web app locally or via CloudShell. Create an ECR repository and push the image.
     * Implement Lifecycle Rules to retain only the last 5 tagged images and scan on push for vulnerabilities.
  2. **ECS Cluster & Fargate Task Definitions:**
     * Define an ECS Cluster utilizing AWS Fargate (Serverless compute).
     * Write Task Definitions specifying container definitions, port mappings, environment variables, and AWS Fargate IAM Task Execution Roles.
  3. **Service Routing via ALB:**
     * Provision an ECS Service with auto-scaling configured between 2 and 8 tasks.
     * Link the service to an ALB Target Group using dynamic port mapping and health checks.

---

## Module 5: Event-Driven & Serverless Architectures (Lambda, SNS, SQS, API Gateway)

### Case Study: Decoupled Order Processing & Notification System

* **Scenario:**  
  An online retail platform needs to decouple order placement from payment processing and email notifications. High volume during peak hours must not crash downstream invoice systems or delay user confirmations.

```
+-------------+     +-------------+     +---------------+     +----------------+
| API Gateway | --> | AWS Lambda  | --> |  Amazon SNS   | --> | Amazon SQS     |
| (REST API)  |     | (Order API) |     |  (Order Topic)|     | (Order Queue)  |
+-------------+     +-------------+     +-------+-------+     +-------+--------+
                                                |                     |
                                                v                     v
                                        +---------------+     +----------------+
                                        | Email Sub /   |     | AWS Lambda     |
                                        | Subscriber    |     | (Worker)       |
                                        +---------------+     +----------------+
```

* **Key Objectives & Console Tasks:**
  1. **SNS Topic & SQS Fan-out:**
     * Create an SNS Topic (`OrderEvents`).
     * Create two SQS Queues: `BillingQueue` and `InventoryQueue`. Subscribe both queues to the SNS Topic (Fan-out pattern).
  2. **Lambda Trigger & Event Source Mapping:**
     * Create a Python/Node.js Lambda Function triggered by `BillingQueue` messages.
     * Configure Dead Letter Queues (DLQ) on SQS to capture failed message invocations after 3 retry attempts.
  3. **API Gateway Endpoint Integration:**
     * Expose a REST API using API Gateway that routes `POST /orders` requests directly to an Ingestion Lambda function or directly to SNS via Service Proxy Integration.

---

## Module 6: Storage & Content Delivery (S3, EFS, CloudFront)

### Case Study: Global Content Delivery with Edge Encryption & S3 Tiering

* **Scenario:**  
  A media publishing firm hosts high-resolution images and videos accessed globally. Downloads must be fast, content must be restricted to authorized users, and storage costs must auto-optimize over time.

* **Key Objectives & Console Tasks:**
  1. **S3 Bucket Security & Lifecycle Management:**
     * Enforce strict S3 Bucket Policies to block public access (`BlockPublicAccess`).
     * Create S3 Lifecycle Policies: Transition objects to S3 Standard-IA after 30 days, Glacier Flexible Retrieval after 90 days, and expire after 365 days.
  2. **CloudFront CDN & OAC:**
     * Set up a CloudFront Distribution with the private S3 bucket as origin.
     * Restrict origin access strictly through **Origin Access Control (OAC)**, ensuring users cannot bypass CloudFront using direct S3 URLs.
  3. **AWS EFS Setup for Shared Storage:**
     * Create an AWS EFS file system with mount targets across multiple private subnets.
     * Attach the EFS file system to multiple EC2 instances concurrently to share configuration files.

---

## Module 7: Disaster Recovery & Database Resiliency (RDS, Aurora, Route 53)

### Case Study: Cross-Region Multi-AZ Failover & Disaster Recovery Implementation

* **Scenario:**  
  A health information platform requires a strict RTO (Recovery Time Objective) of < 15 minutes and RPO (Recovery Point Objective) of < 5 minutes. The architecture must withstand a full AWS Region outage.

* **Key Objectives & Console Tasks:**
  1. **Multi-AZ Aurora/RDS Infrastructure:**
     * Provision an Amazon Aurora PostgreSQL or RDS Instance with Multi-AZ deployment enabled in Region A (Primary).
     * Setup Cross-Region Read Replicas in Region B (Secondary).
  2. **Automated Route 53 Health Checks & DNS Failover:**
     * Configure a Route 53 Failover Routing Policy.
     * Create Route 53 Health Checks targeting the Primary Region application endpoint.
     * Point domain records to Secondary infrastructure upon health check failure.
  3. **Simulated DR Execution:**
     * Test promote operations on the Cross-Region Read Replica to elevate it to a standalone primary database.

---

## Verification & Architecture Checklists
To complete each case study successfully, verify your deployments using the following steps:
* [ ] Verify least privilege via IAM Policy Simulator.
* [ ] Confirm no public IPs are assigned to database and private application instances.
* [ ] Perform failover testing on load balancers and Multi-AZ database instances.
* [ ] Clean up resources (Delete NAT Gateways, ALB, EC2, RDS) to prevent unnecessary charges on your account.
