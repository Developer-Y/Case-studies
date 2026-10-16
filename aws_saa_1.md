# AWS Solutions Architect Associate (SAA-C03)

## Topic-wise Console Hands-on Case Study Workbook

**Study method:** Watch the matching section of Stephane Maarek's
course, implement the lab in the AWS Management Console, test both
success and failure paths, draw the architecture from memory, and
explain why the chosen service fits the scenario.

**Safety first** - Use a dedicated lab environment and the smallest
suitable resources. - Create an AWS Budget alert. Alerts are not hard
spending caps. - NAT Gateways, load balancers, public IPv4 addresses,
databases, ECS/Fargate, and running compute can incur charges. - Avoid
real production data. Delete lab resources after testing, including
related networking resources, snapshots, logs, and Elastic IPs where
appropriate. - Some labs require a domain, a second AWS account, a
second Region, or specific service permissions. Treat those as optional
if unavailable.

## Recommended learning order

1.  IAM and security
2.  S3 and storage
3.  VPC and networking
4.  EC2, EBS, and AMIs
5.  Load balancing and Auto Scaling
6.  RDS, Aurora, DynamoDB, and caching
7.  Route 53, CloudFront, and ACM
8.  Lambda and serverless
9.  SNS, SQS, and EventBridge
10. ECS, ECR, and Fargate
11. Disaster recovery and backup
12. Monitoring, auditing, and security services
13. Cost optimization
14. Additional services
15. End-to-end architecture projects

------------------------------------------------------------------------

## 1. IAM, security, and access management

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  IAM-01                  Create developer and    Users, groups,
                          administrator groups,   policies, least
                          create separate IAM     privilege
                          users, and attach       
                          managed policies to     
                          groups.                 

  IAM-02                  Create a custom policy  Identity policy,
                          that permits read-only  resource ARN
                          access to one S3 bucket 
                          but not another.        

  IAM-03                  Create an EC2 instance  Roles, instance
                          role that can read a    profiles, temporary
                          specific S3 object;     credentials
                          access S3 from EC2      
                          without static          
                          credentials.            

  IAM-04                  Add an S3 bucket policy Resource-based vs
                          granting access to a    identity-based policies
                          specific principal;     
                          test it with IAM        
                          permissions.            

  IAM-05                  Configure a role for a  STS, cross-account
                          second AWS account to   access
                          assume, including trust 
                          and permissions         
                          policies.               

  IAM-06                  Configure MFA and       MFA, federation,
                          secure the root         centralized access
                          account; explore IAM    
                          Identity Center if      
                          available.              

  IAM-07                  Add an explicit Deny    Policy evaluation;
                          and compare it with an  explicit Deny
                          Allow from another      
                          policy.                 

  IAM-08                  Restrict an S3          IAM conditions
                          operation using a       
                          condition such as       
                          secure transport or an  
                          encryption header.      

  IAM-09                  Give Lambda permission  Service roles, least
                          to write logs but not   privilege
                          access unrelated S3     
                          buckets.                

  IAM-10                  Investigate an          Auditing and
                          AccessDenied error with troubleshooting
                          IAM Policy Simulator    
                          and CloudTrail Event    
                          history.                
  -----------------------------------------------------------------------

**Challenge:** A developer has `s3:GetObject` but cannot download an
object. Find at least three possible causes, such as a bucket policy
denial, missing KMS permissions, or an incorrect object ARN.

**Console areas:** IAM → Users, User groups, Roles, Policies, Access
Analyzer; CloudTrail → Event history; S3 → Permissions.

**Exam distinction:** Authentication identifies a principal;
authorization determines what it can do. A role's trust policy controls
who can assume it; its permissions policies control what the role can
do.

## 2. S3, storage, and data protection

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  S3-01                   Create a private        Buckets, objects,
                          bucket, upload files,   access controls
                          and test authorized and 
                          unauthorized access.    

  S3-02                   Enable versioning,      Versioning, delete
                          overwrite an object,    markers
                          delete it, and restore  
                          a prior version.        

  S3-03                   Configure lifecycle     Storage classes,
                          transitions and         lifecycle
                          expiration for test     
                          objects.                

  S3-04                   Explore SSE-S3 and      Encryption at rest, KMS
                          SSE-KMS using a         permissions
                          customer-managed KMS    
                          key.                    

  S3-05                   Configure cross-Region  Replication roles and
                          replication between     versioning
                          versioned buckets.      

  S3-06                   Explore Object Lock in  WORM, retention
                          a separate test bucket, 
                          including retention     
                          modes and legal holds.  

  S3-07                   Compare S3 static       Static hosting, origin
                          website hosting with    protection
                          private S3 behind       
                          CloudFront.             

  S3-08                   Generate and test a     Temporary delegated
                          time-limited presigned  access
                          URL.                    

  S3-09                   Send S3 object-created  Event-driven processing
                          events to Lambda or     
                          SQS.                    

  S3-10                   Compare archive         Retrieval trade-offs
                          retrieval delays and    
                          costs across storage    
                          classes.                
  -----------------------------------------------------------------------

**Challenge:** Keep records for seven years, prevent deletion during the
retention period, and reduce storage costs. Design lifecycle rules and
Object Lock settings. Explain when archived data can be retrieved.

**Console areas:** S3 → Properties, Management, Permissions; KMS →
Customer managed keys.

## 3. VPC and networking

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  VPC-01                  Create a VPC with a     CIDR and IP addressing
                          `/16` CIDR and multiple 
                          smaller subnets.        

  VPC-02                  Create a public subnet, Public routing
                          attach an Internet      
                          Gateway, configure a    
                          route table, and launch 
                          a test EC2 instance.    

  VPC-03                  Create a private subnet NAT vs Internet Gateway
                          with a NAT Gateway in a 
                          public subnet and test  
                          outbound connectivity.  

  VPC-04                  Create public and       Multi-AZ networking
                          private subnets in two  
                          AZs with separate route 
                          tables.                 

  VPC-05                  Permit web traffic to a Stateful security
                          web tier and database   groups
                          traffic only from the   
                          application tier.       

  VPC-06                  Configure a network ACL Stateless filtering,
                          and compare it with     ephemeral ports
                          security groups.        

  VPC-07                  Troubleshoot            Network diagnosis
                          unreachable EC2 by      
                          checking routes, public 
                          IP, security groups,    
                          NACLs, and host         
                          firewall.               

  VPC-08                  Connect two VPCs with   Peering and
                          peering and configure   non-transitivity
                          routes on both sides.   

  VPC-09                  Create an S3 Gateway    Gateway endpoints,
                          VPC Endpoint and access endpoint policies
                          S3 without routing that 
                          traffic through NAT.    

  VPC-10                  Explore an Interface    PrivateLink, endpoint
                          VPC Endpoint for a      security groups
                          supported service.      

  VPC-11                  Enable VPC Flow Logs    Network visibility
                          and inspect             
                          accepted/rejected       
                          traffic.                

  VPC-12                  Explore Route 53        Private DNS
                          private hosted zones    
                          and VPC DNS settings.   
  -----------------------------------------------------------------------

**Challenge:** A private EC2 instance can download software updates but
cannot access S3 privately. Explain NAT Gateway versus S3 Gateway
Endpoint.

**Exam traps:** A public subnet needs a route to an Internet Gateway; a
public IP alone is not enough. NAT supports outbound connectivity but
does not ordinarily allow unsolicited inbound internet connections.
Security groups are stateful; NACLs are stateless. VPC peering is not
transitive.

**Cost note:** NAT Gateways incur hourly and data-processing charges.
Delete the NAT Gateway and related Elastic IP after the lab.

## 4. EC2, EBS, AMIs, and compute

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  EC2-01                  Launch a Linux          AMIs, instance types,
                          instance, connect       key pairs
                          securely, and host a    
                          basic web page.         

  EC2-02                  Attach, format, mount,  Block storage and
                          detach, and reattach an persistence
                          EBS volume with test    
                          data.                   

  EC2-03                  Create a snapshot and   Snapshots and recovery
                          restore a new volume    
                          from it.                

  EC2-04                  Create a custom AMI and Golden images
                          launch another instance 
                          from it.                

  EC2-05                  Compare stop, start,    Instance lifecycle
                          reboot, and terminate;  
                          observe public IPv4 and 
                          EBS behavior.           

  EC2-06                  Use an instance role to Secure credentials
                          access S3 without       
                          embedded credentials.   

  EC2-07                  Create an encrypted EBS KMS and encryption
                          volume and explore      
                          snapshot encryption.    

  EC2-08                  Compare EBS, EFS, and   Storage selection
                          S3 for block,           
                          shared-file, and object 
                          storage.                

  EC2-09                  Explore instance        Metadata security
                          metadata and IMDSv2     
                          settings.               

  EC2-10                  Compare On-Demand,      Pricing and
                          Spot, and Savings Plans interruption tolerance
                          for suitable workloads. 
  -----------------------------------------------------------------------

**Challenge:** Design a server that can be rebuilt quickly after failure
while retaining application configuration and persistent data. Use an
AMI, EBS snapshot, and an appropriate backup strategy.

## 5. Load balancing, Auto Scaling, and high availability

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  HA-01                   Create an ALB, target   Listeners and target
                          group, and two EC2 web  groups
                          servers.                

  HA-02                   Place targets in        Multi-AZ resilience
                          different AZs and test  
                          health checks.          

  HA-03                   Create a Launch         Elastic fleet
                          Template and Auto       management
                          Scaling Group across    
                          two AZs.                

  HA-04                   Configure target        Scaling policies,
                          tracking based on CPU   CloudWatch
                          utilization.            

  HA-05                   Make a test target      Health checks,
                          unhealthy and observe   self-healing
                          removal and recovery.   

  HA-06                   Compare ALB and NLB for Layer 7 vs Layer 4
                          HTTP applications and   
                          TCP/UDP workloads.      

  HA-07                   Configure ALB           Host/path routing
                          path-based routing to   
                          two target groups.      

  HA-08                   Test sticky sessions    Stateless design
                          and consider moving     
                          session state outside   
                          web servers.            
  -----------------------------------------------------------------------

**Challenge:** Keep a web application available when an instance fails
and handle unpredictable traffic. Build an ALB and Auto Scaling Group
across at least two AZs.

**Distinction:** Vertical scaling increases instance size; horizontal
scaling adds instances. Multi-AZ placement and Auto Scaling solve
different problems.

## 6. RDS, Aurora, DynamoDB, and caching

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  DB-01                   Create RDS in private   Managed databases,
                          subnets using a DB      private access
                          subnet group; connect   
                          from an allowed         
                          application instance.   

  DB-02                   Enable Multi-AZ on a    Availability vs read
                          suitable test database  scaling
                          and explore failover.   

  DB-03                   Create a read replica   Read scaling and
                          where supported and     replication
                          compare it with a       
                          Multi-AZ standby.       

  DB-04                   Configure backups and   Backup retention and
                          perform point-in-time   recovery
                          restore where           
                          supported.              

  DB-05                   Create a DynamoDB table NoSQL access patterns
                          with partition and sort 
                          keys; insert, query,    
                          and scan items.         

  DB-06                   Compare on-demand and   Capacity modes
                          provisioned capacity;   
                          explore throttling      
                          safely.                 

  DB-07                   Enable DynamoDB         Data recovery
                          point-in-time recovery  
                          and restore to a new    
                          table.                  

  DB-08                   Explore DynamoDB Global Multi-Region
                          Tables.                 replication

  DB-09                   Explore ElastiCache and Latency and database
                          design a cache-aside    load
                          pattern.                

  DB-10                   Compare RDS, Aurora,    Data-service selection
                          DynamoDB, and           
                          ElastiCache for example 
                          requirements.           
  -----------------------------------------------------------------------

**Challenge:** A read-heavy application needs a relational database and
AZ-failure tolerance. Explain when to use Multi-AZ, read replicas,
Aurora, and a cache.

**Exam distinctions:** RDS Multi-AZ primarily supports availability and
failover; read replicas primarily support read scaling. DynamoDB
supports key-based NoSQL access patterns. ElastiCache is a caching
service, not a general replacement for a durable database.

## 7. Route 53, CloudFront, ACM, and edge delivery

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  CDN-01                  Create a Route 53       DNS records
                          hosted zone if you have 
                          a domain and configure  
                          records.                

  CDN-02                  Compare simple,         DNS routing policies
                          weighted, latency,      
                          failover, and           
                          geolocation routing.    

  CDN-03                  Create CloudFront with  CDN and origin
                          a private S3 origin and protection
                          Origin Access Control   
                          (OAC).                  

  CDN-04                  Change an origin object Caching
                          and test TTL and        
                          invalidation behavior.  

  CDN-05                  Explore ACM             TLS and certificate
                          certificates for        scope
                          CloudFront or an        
                          eligible load balancer. 

  CDN-06                  Compare CloudFront with Global delivery
                          direct S3 access and a  
                          load-balanced app.      

  CDN-07                  Design Route 53         DNS-based DR
                          failover between        
                          primary and secondary   
                          endpoints.              
  -----------------------------------------------------------------------

**Challenge:** Deliver static content globally using HTTPS while keeping
the S3 origin private. Explain CloudFront, OAC, ACM, and Route 53.

**Note:** CloudFront viewer certificates generally need to be
provisioned in `us-east-1`. DNS routing policies depend on health checks
and DNS caching; they do not all provide identical failover guarantees.

## 8. Lambda and serverless

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  LAMBDA-01               Create a Python Lambda  Runtime and execution
                          function, run a test    role
                          event, and inspect      
                          logs.                   

  LAMBDA-02               Give Lambda access to   Least privilege
                          one S3 bucket; test     
                          allowed and denied      
                          requests.               

  LAMBDA-03               Trigger Lambda when an  Event-driven processing
                          object is uploaded to   
                          S3.                     

  LAMBDA-04               Connect API Gateway     Serverless APIs
                          HTTP API to Lambda and  
                          return JSON.            

  LAMBDA-05               Connect Lambda to       Serverless application
                          DynamoDB for basic      design
                          create/read operations. 

  LAMBDA-06               Configure environment   Runtime configuration
                          variables, timeout,     and error handling
                          memory, and a supported 
                          failure destination.    

  LAMBDA-07               Explore asynchronous    Retry behavior
                          retries and             
                          destinations;           
                          deliberately raise an   
                          exception.              

  LAMBDA-08               Explore reserved        Capacity protection
                          concurrency and         
                          throttling.             

  LAMBDA-09               Put Lambda in a VPC and VPC connectivity and
                          test access to a        endpoints
                          private resource.       

  LAMBDA-10               Explore versions,       Safer deployments
                          aliases, and weighted   
                          traffic shifting.       
  -----------------------------------------------------------------------

**Challenge:** Process uploaded images, store metadata in DynamoDB, and
notify users when processing completes. Explain duplicate events,
retries, and failures.

**Compare:** Lambda vs EC2; Lambda vs ECS Fargate; synchronous vs
asynchronous invocation; event-driven processing vs polling.

## 9. SNS, SQS, EventBridge, and decoupling

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  MSG-01                  Create an SNS topic,    Pub/sub notifications
                          subscribe email,        
                          confirm subscription,   
                          and publish a message.  

  MSG-02                  Send, receive, and      Queueing and visibility
                          delete messages in an   timeout
                          SQS Standard queue.     

  MSG-03                  Subscribe two SQS       Independent consumers
                          queues to an SNS topic  
                          and verify fan-out.     

  MSG-04                  Configure an SQS        Poison messages and
                          dead-letter queue and   recovery
                          redrive policy;         
                          simulate repeated       
                          failures.               

  MSG-05                  Create an SQS FIFO      Ordering and
                          queue and test message  deduplication
                          groups and              
                          deduplication.          

  MSG-06                  Trigger Lambda from SQS Event source mappings
                          and test batch          
                          processing and partial  
                          batch failures.         

  MSG-07                  Create an EventBridge   Event routing and
                          rule and route matching filtering
                          events to a target.     

  MSG-08                  Compare SNS, SQS, and   Service selection
                          EventBridge for         
                          notifications,          
                          buffering, and event    
                          routing.                

  MSG-09                  Place SQS between a web Backpressure and
                          app and a slow          independent scaling
                          processor; simulate     
                          bursts.                 
  -----------------------------------------------------------------------

**Challenge:** An order system must notify billing, inventory, and
analytics independently. Build SNS-to-multiple-SQS fan-out and explain
DLQs and idempotent consumers.

**Distinction:** SNS publishes notifications; SQS buffers messages for
consumers; EventBridge routes events using rules and patterns. Standard
queues favor scale and at-least-once delivery. FIFO provides ordering
and deduplication features subject to service constraints.

## 10. ECS, ECR, Fargate, and containers

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  ECS-01                  Build/use a sample      Image registry
                          container image and     
                          push it to ECR.         

  ECS-02                  Create an ECS cluster   Cluster, task
                          and deploy a simple     definition, service
                          Fargate service.        

  ECS-03                  Configure task CPU,     Runtime sizing
                          memory, ports, and      
                          logging.                

  ECS-04                  Put an ECS service      Containerized web apps
                          behind an ALB.          

  ECS-05                  Configure desired task  Horizontal scaling
                          count and service auto  
                          scaling.                

  ECS-06                  Separate task execution IAM role separation
                          role from task role;    
                          grant the task access   
                          to a specific S3        
                          bucket.                 

  ECS-07                  Explore secret          Secrets and permissions
                          injection and private   
                          image pulls.            

  ECS-08                  Stop a task and observe Desired state and
                          ECS replacement.        self-healing

  ECS-09                  Compare ECS on EC2, ECS Orchestration choices
                          on Fargate, and EKS.    
  -----------------------------------------------------------------------

**Challenge:** Deploy a containerized web application behind HTTPS with
automatic scaling and no server management. Build ECS Fargate behind an
ALB and document IAM roles and network paths.

**Distinction:** ECR stores images; ECS orchestrates containers; Fargate
runs containers without managing the underlying server fleet; EKS
provides a managed Kubernetes control plane.

## 11. Disaster recovery, backup, and business continuity

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  DR-01                   Snapshot an EBS volume, Backup and restore
                          delete a test volume,   
                          and restore it.         

  DR-02                   Configure RDS backups   Point-in-time recovery
                          and restore a database  
                          to a new instance.      

  DR-03                   Enable S3 versioning    Replication vs backup
                          and cross-Region        
                          replication; inspect    
                          deletion recovery       
                          options.                

  DR-04                   Explore AWS Backup      Centralized backup
                          plans, vaults,          
                          retention, and          
                          cross-Region copy for   
                          supported resources.    

  DR-05                   Design a pilot-light    Low-cost DR
                          secondary Region with   
                          essential               
                          data/configuration and  
                          compute provisioned     
                          during recovery.        

  DR-06                   Design warm standby     Faster recovery at
                          with scaled-down        higher cost
                          compute and replicated  
                          data.                   

  DR-07                   Explore Route 53        Traffic redirection
                          failover between        
                          endpoints.              

  DR-08                   Implement a small       Regional resilience
                          non-production          
                          secondary-Region        
                          recovery exercise if    
                          feasible.               

  DR-09                   Write a recovery        Recovery dependencies
                          runbook covering        
                          networking,             
                          permissions, compute,   
                          database, and DNS.      

  DR-10                   Compare AWS Backup,     Recovery design
                          snapshots, replication, 
                          and point-in-time       
                          restore for different   
                          failures.               
  -----------------------------------------------------------------------

**Challenge:** The business can tolerate at most 15 minutes of data loss
and needs service restored within one hour after a regional outage.
Propose a design and identify which components must be replicated,
pre-provisioned, or restored.

**Definitions:** RPO is the acceptable amount of data loss measured in
time. RTO is the target time to restore service. Backup and restore is
often lower cost but slower; pilot light keeps core components ready;
warm standby keeps a scaled-down working environment; active/active
serves traffic from multiple locations with higher complexity.

**Important:** A replica is not necessarily a backup. Deletion or
corruption can propagate, so use independent recovery points where
required.

## 12. Monitoring, security, and auditing

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  SEC-01                  Review CloudTrail       API auditing
                          management events and   
                          identify who changed a  
                          test resource.          

  SEC-02                  Create a CloudWatch     Metrics and alerting
                          alarm for EC2 CPU and   
                          configure SNS           
                          notification.           

  SEC-03                  Explore CloudWatch Logs Observability
                          for EC2, Lambda, and    
                          ECS where applicable.   

  SEC-04                  Use Systems Manager     Managed access without
                          Session Manager with a  inbound SSH
                          supported EC2 instance. 

  SEC-05                  Store a test secret in  Secret management
                          Secrets Manager and     
                          explore retrieval       
                          permissions and         
                          rotation.               

  SEC-06                  Create a KMS key,       Key policies
                          configure key           
                          permissions, and test   
                          encrypted data access.  

  SEC-07                  Explore WAF rules for   Web application
                          an ALB or CloudFront    filtering
                          distribution.           

  SEC-08                  Explore GuardDuty       Threat detection
                          findings and the        
                          signals it analyzes.    

  SEC-09                  Explore AWS Config      Configuration
                          rules and resource      compliance
                          configuration history.  

  SEC-10                  Review Access Analyzer  Permission analysis
                          findings and remove     
                          unnecessary permissions 
                          from a lab role.        
  -----------------------------------------------------------------------

**Challenge:** Alert when an administrator changes security groups,
monitor unhealthy servers, and protect application secrets. Design the
logging, alerting, and access controls.

**Distinction:** CloudTrail records API activity; CloudWatch handles
metrics and logs; AWS Config tracks resource configuration; GuardDuty
detects potential threats.

## 13. Cost optimization and architecture trade-offs

  -----------------------------------------------------------------------
  ID                      Case study /            Key concepts
                          implementation          
  ----------------------- ----------------------- -----------------------
  COST-01                 Evaluate a predictable, On-Demand, Savings
                          continuously running    Plans, Reserved
                          workload.               Instances where
                                                  applicable

  COST-02                 Evaluate a restartable  Spot and interruption
                          batch job.              handling

  COST-03                 Store years of rarely   Lifecycle and Glacier
                          accessed files.         classes

  COST-04                 Give private EC2        NAT data charges vs S3
                          instances S3 access.    Gateway Endpoint

  COST-05                 Optimize a read-heavy   Read replicas, caching,
                          database workload.      right-sizing

  COST-06                 Serve global static     CloudFront caching
                          content.                

  COST-07                 Evaluate an irregular   Lambda vs always-on EC2
                          workload with long idle 
                          periods.                

  COST-08                 Handle peak traffic     Auto Scaling
                          while minimizing idle   
                          capacity.               

  COST-09                 Improve application     Cost allocation tags,
                          spending visibility.    Budgets, Cost Explorer

  COST-10                 Review every lab        Billing awareness
                          resource and its        
                          charges before cleanup. 
  -----------------------------------------------------------------------

**Challenge:** Propose the lowest-cost architecture that meets
availability, latency, security, and recovery requirements. Explain the
trade-offs; cheapest is not always correct.

## 14. Additional services to recognize and explore

  -----------------------------------------------------------------------
  Topic                               Case study / focus
  ----------------------------------- -----------------------------------
  AWS Organizations                   Explore multiple accounts,
                                      consolidated billing, and service
                                      control policies (SCPs).

  IAM Identity Center                 Explore centralized access in a
                                      suitable organization.

  AWS KMS                             Compare AWS-owned, AWS-managed, and
                                      customer-managed key concepts.

  ACM                                 Explore certificate validation and
                                      renewal for eligible services.

  AWS Transfer Family                 Understand managed file transfers
                                      with S3/EFS; avoid leaving
                                      expensive endpoints running.

  Storage Gateway                     Compare file, volume, and tape
                                      gateway use cases.

  Direct Connect and Site-to-Site VPN Study hybrid connectivity,
                                      redundancy, and encryption.

  Global Accelerator                  Compare static entry points and
                                      global routing with CloudFront and
                                      Route 53.

  EFS                                 Mount shared file storage from
                                      compatible EC2 instances across
                                      AZs.

  FSx                                 Compare managed file systems for
                                      specific workloads.

  DMS                                 Explore migration between supported
                                      databases.

  Snow Family                         Understand offline and edge
                                      data-transfer scenarios.

  Cognito                             Explore user pools and identity
                                      federation for application users.

  Shield and WAF                      Compare DDoS protection with web
                                      application filtering.

  Well-Architected Tool               Review a workload against the six
                                      pillars of the Well-Architected
                                      Framework.
  -----------------------------------------------------------------------

## 15. Five end-to-end projects

### Project 1 --- Highly available three-tier web application

**Build:** VPC, public/private subnets across two AZs, ALB, Auto Scaling
Group, EC2, RDS, security groups, IAM role, and CloudWatch alarms.

**Test:** Make a web server unhealthy, verify traffic continues, and
confirm the database is not publicly reachable.

**Exam focus:** Multi-AZ, least privilege, scaling, load balancing,
fault tolerance.

### Project 2 --- Serverless image processing

**Build:** Private S3 bucket, S3 event notification, Lambda, DynamoDB,
SNS, and CloudWatch Logs.

**Test:** Upload an image, verify metadata is stored, deliberately
trigger a processing failure, and inspect logs.

**Exam focus:** Event-driven architecture, IAM roles, retries,
decoupling, serverless cost optimization.

### Project 3 --- Containerized application

**Build:** ECR, ECS Fargate, ALB, private subnets, task roles,
CloudWatch Logs, and service auto scaling.

**Test:** Stop a task, observe replacement, and verify traffic reaches
healthy targets.

**Exam focus:** Containers, managed compute, IAM separation, elastic
scaling.

### Project 4 --- Regional disaster recovery

**Build:** Primary application, backup or replicated data in another
Region, recovery instructions, and a secondary endpoint.

**Test:** Restore a backup or activate a secondary environment and
document the recovery steps.

**Exam focus:** RPO, RTO, backup vs replication, pilot light, warm
standby, DNS failover.

### Project 5 --- Secure private application

**Build:** Private EC2, IAM instance role, S3 Gateway Endpoint, KMS
encryption, Secrets Manager, and CloudTrail auditing.

**Test:** Remove a required permission, verify access fails, restore the
permission, and inspect audit events.

**Exam focus:** Least privilege, private networking, encryption,
endpoints, troubleshooting.

## 16. Progress checklist

Mark a topic complete only after you have built it and tested at least
one success and one failure scenario.

### Foundation

-   [ ] IAM users, groups, and policies
-   [ ] IAM roles and cross-account access
-   [ ] S3 permissions, versioning, and lifecycle
-   [ ] VPC CIDR, subnets, and route tables
-   [ ] Internet Gateway, NAT Gateway, and endpoints

### Compute and data

-   [ ] EC2, EBS, snapshots, and AMIs
-   [ ] ALB, health checks, and Auto Scaling
-   [ ] RDS Multi-AZ and read replicas
-   [ ] DynamoDB keys and recovery
-   [ ] Route 53 and CloudFront

### Integration and containers

-   [ ] Lambda execution role and triggers
-   [ ] SNS and SQS fan-out
-   [ ] EventBridge routing and DLQs
-   [ ] ECR, ECS, and Fargate
-   [ ] CloudWatch alarms and CloudTrail

### Resilience and architecture

-   [ ] Backup and restore
-   [ ] Cross-Region disaster recovery
-   [ ] Encryption and secret management
-   [ ] Cost optimization and billing review
-   [ ] Five end-to-end projects

## 17. Study workflow with Stephane Maarek's course

For each service section: 1. Watch the relevant lessons. 2. Write down
the service purpose, key configuration options, limitations, and
alternatives. 3. Complete the matching console labs. 4. Draw the
architecture from memory. 5. Answer five to ten scenario questions on
that topic. 6. Revisit incorrect answers and implement the missing
concept.

### SAA-C03 exam domain weights

-   Secure architectures --- **30%**
-   Resilient architectures --- **26%**
-   High-performing architectures --- **24%**
-   Cost-optimized architectures --- **20%**

These are official exam domain weights, not a required allocation of
study hours. Use them to guide revision while practicing complete
architectures across all four domains.

Official exam guide:
https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03.html

## Final readiness standard

For a business scenario, aim to draw the architecture, configure a small
version in the console, explain why each service belongs there, identify
failure modes, and justify why alternatives are less suitable.

**Suggested priority if the exam is approaching:** IAM, VPC, S3, EC2,
ALB/Auto Scaling, RDS/DynamoDB, Lambda, SNS/SQS, and disaster recovery.
Complete the first end-to-end project before your final revision phase.
