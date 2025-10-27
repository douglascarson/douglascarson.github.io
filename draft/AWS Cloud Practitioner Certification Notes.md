# AWS Fundamentals
* AWS region is a physical geographic location
* AWS availability zone is physical datacenters within a region
* AWS Outpost is the equivalent of Azure Stack where the hardware runs within an on-prem datacenter
* AWS outpost has a subset of services with connectivity back to an AWS region
* AWS local zone is a small availaibity zone that is closer to a metro area than a region. This is good for latency sensitive environment
* AWS wavelength zones are to deploy applications close to 5G users with connectivity back to AWS regions. Key requirement is to lower latency
## AWS Region
* Within a region you have multiple availability zones
* Within availability zone there are Public and Private subnets
* Between AZs there is low latency redundant network connections
## AWS Cloudfront
* Cloudfront is an AWS CDN
* required for network performance and placing the data as close to the geographic region
## AWS Shared Responsibility Model

| Customer Responsibility | Customer Data   |
|-------------------------|-----------------|
| Customer Responsibility | Platform, Applications, Identity & Access Management|
| Customer Responsibility | Operating System, Network & Firewall Configuration|
| Customer Responsibility | Client-Side Data, Encryption & Data Integrity & Authentication|
| Customer Responsibility | Server-Side Encryption|
| Customer Responsibility | Networking Traffic Protection (Encryption, Integrity, Identitiy)|
| AWS | Software|
| AWS | Compute / Storage / Database / Networking|
| AWS | Hardware / AWS Global Infrastructure|
| AWS | Regions / Availability Zones / Edge Locations|

## AWS Pricing Fundamentals

* Compute: Charged for duration. Turn it off you don't get charged
* Storage: charged for amounf of data or allocated space
* Network: egress transfers

* Pay-as-You-Go
    * easy to adaprt to changing business needs
    * Adapt based on needs not forecasts
    * Only pay for what you need
* Save when you reserve
    * Services where you can reserve capacity. Commiting to 1 or 3 years (can pay up to 75%)
    * Pay up front
* Pay less by using more
    * Pay less as volume increases
    * Tiered pricing means the more you use the lower the unit price
## Advantages of Cloud Computing
* Trade capital expense with operational costs
* Massive economies of scale
* scale when required
* Increaae of speed and agility
* Stop costs of running datacenters
* Go global in minutes

# AWS Authentication & Access Control
* within AWS IAM you can create and manage:
    * Users
    * Roles
    * Federated User
    * Application
* Within AWS IAM you have:
    * Authentication
    * Authorisation
* Authentication: Is a person who they say they are: This is validated by use of username / Passord
* Authorisation: Determines whether to authorise the request and what can you do
## Authentication (Authn)

* IAM Principles must be <span style=color:yellow>authentcated </span>to send requests
* A priciple is a <span style=color:yellow>person </span> or <span style=color:yellow>application </span>that can make a request for an API action or operation on an AWS resource

## Autorisation (Authz)
* Allow / Deny Access to API actions on AWS Resources. This is applied by:
    * Identity based Policies
    * Resource based Policies
### Core components
* Users
* User Groups
* Role (similar to user assigned managed account for AWS services)
* Policy (Azure role)
* Polcies are applied to User Groups
* <span style=color:yellow>Identity-based policies</span> are applied to users, groups and roles

## Users
* You can create up to 5000 user accounts
* By <span style=color:red>default a normal user has no permissions to perform any action</span> until a policy has been applied
* Amazon Resource Number is composed of arn.aws.iam::account_number:user/username
## User Groups
* Usually apply Usr policies to uswer groups
* User permissions are cumulative
## IAM Authentication Methods
* AWS Console you would use username / Password and or MFA
* CLI or API: Access Key. Acceskey is composed of <span style=color:yellow>Access Key ID</span> and <span style=color:yellow>Secret Access Key</span>
* Access Keys are for programmatic access
## IAM Roles and Policies
* IAM role is an IAM identity  that has specific permissions
* Roles are <span style=color:yellow>assumed by users, applications and services</span>.
* Once the identity "becomes" the role it gains the roles permissions
* IAM Policies are documents that define <span style=color:yellow>permissions</span> are are written in JSON
* There are two types of policies:
    * Identity-based policies that can be applied to users, groups and roles
    * Resource-based policies apply to resources. Not all resources support resource based policies
* All permissions within AWS are <span style=color:yellow>implicitly denied</span> by default
* A trust policy within a role defines who can assume the role
* Within AWS console you can switch role to a role with a higher privilege
* To allow a user to switch role you need the Action below
```json
{
    "Version": "2012-10-17"
    "Statement": {
        "Effect": "Allow",
        "Action": "sts:AssumeRole",
        "Resource":"arn:aws:iam:account-id:role/<role Name*"

    }
}
```

## Identity Center
* IAM identity center is the AWS singlesign-on SSO renamed 
* Primarily used for <span style=color:yellow>single Sign-On</span>
* You connect on-prem ADDS you require the AD connector or AWS Managed Microsofft AD
* Identity has builtin integrations such as Entra ID and Okta etc
* Provide SSO for AWS Orgs and Accounts
## IAM v Identity Center
| Feature / Ise Case | AWS IAM   |IAM Identity Center|
|-------------------------|-----------------|-------------------|
|Primary Purpose|Manages access to AWS services and resources|centralised identity management and provide SSO to AWS accounts and business apps|
|Identity Federation|supports federation with external IdPs using SAML and OpenID Connect|Built-in federation with external IdPs, streamlined for ease of setup and management|
|Multi-Account Access|Requires more complex setup for cross-tenant access|simplifies granting users access to multiople AWS accounts and applications with a single login|
|Integration with Business Applications|LLimited to AWS services: requires custom configuration for non-AWS applications|provides SSO access to commonly used business apps <span style=color:yellow>in addition to AWS accounts</span>|
|Single Sign-On (SSO)|Enables SSO through federation, but setup is complex and manual|offers a more user friendly and simplified SSO setup for both AWS and non-AWS apps|
|Programatic Access|Allows for programatic access through Access keys|Not Applicable|
|Fine-grained access control|Allowed through Identity and Resource policies|Not Applicable|
## AWS Organisations
* Within AWS there is the concept of a Organisational root. By default there is a dedicated AWS Management Account linked to the AWS root Org

<img src="https://learn.microsoft.com/en-us/azure/architecture/aws-professional/images/aws-accounts.jpg#lightbox" alt="AWS Org">

|Azure|AWS|
|--|--|
|Azure Entra ID Tenant|Dedicated Management Account|
|Root Management Group Linked to Entra ID tenant|Organisation Root contained within the management account|
|Child Management groups|Child Organisational Units|
|Subscription|AWS Account|
|Subscription|Resource Groups|
* The maximum OU hierarchy can be 5 levels deep
### Management Account
* The management account is the AWS account used to create your organisation
* The management account is the ultimate owner of the organisation, having ultimate control over the security, infrastructure and finance policies
* The account has the role of payer which is responsible for paying all charges accrued by the account within it's Org
### Member Accounts
* member accounts are accounts that are part of the Org that is not the management account
* A member account can apply to one Org only

## IAM Best Practises
* Require human users to use federation with an identity provider to access AWS using temporary credentials
* Require workloads to use temporary credentials with IAM roles to access AWS
* Require MFA
* Rotate Access keys regularly 
* Safeguard root user credentials
* Apply least privilege
* Start with AWS <span style=color:yellow>managed policies</span> and move to least privilege permissions (managed policies are the built-in policies)
* Use <span style=color:yellow>IAM access analyser</span> to generate leat-privilege policies on access activity
* REview and remobe unused users, roles, permissions, policies and credentials
* Use conditions in IAM policies to further restrict access
* verify public and cross-account access to resources with IAM access analyser
* Establish permissions guardrails across multiple accounts. <span style=color:yellow>You can use AWS organisations and Control Tower to implement those security measures</span>
We recommend that you use AWS Organizations service control policies (SCPs) to establish permissions guardrails to control access for all principals (IAM roles and users) across your accounts. We recommend that you use AWS Organizations resource control policies (RCPs) to establish permissions guardrails to control access for AWS resources across your organization. SCPs and RCPs are types of organization policies that you can use to manage permissions in your organization at the AWS organization, organizational unit (OU), or account level.
* Use permissions boundaries to delegate permissions management within an account (permissions boundary is the maximum permissions within an account)
Use Case: This means you can delegate the permission to developers to create and manage roles for their workloads. A permissions boundary is an advanced feature for using a managed policy to set the maximum permissions that an identity-based policy can grant to an IAM role
* Human users that are members of your organization are also known as workforce identities

## Service Control Policies
* SCPs are like Azure Policy are only applied at the Organisation OU level 
* Service control policies are a type of organisation policy that you can use to manage permissions in your organisation
* SCP policies only apply the members within one account. Not cross-account users
* SCPs can limit the AWS services, resources and individual API operations that users and roles in each member account can access. They attach at the OU level
* <span style=color:yellow>SCP policies override IAM policies</span>
* <span style=color:yellow>SCPs don't affect users and roles in the Management account</span>. Only the member accounts in the organisation OUs

### Example - This limits the regions used by accounts
**Statement:** Serves as the container for policy elements. You can have multiple statements in SCPs

* **Sid:** Provides a friendly name (optional)
* **Effect:** defines if the statement is allowed or denied
* **Action:** Specifies AWS service and actions
* **NotAction:** Sepecifies AWS service and actions that are <span style=color:yellow>exempt</span> from SCP
* **Resource:** Specifies the AWS resources that the SCP applies to
* **SCP Condition:** Specifies conditions for when the statement is in effect

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DenyAllOutsideEU",
            "Effect": "Deny",
            "NotAction": [
                "a4b:*",
                "acm:*",
               "aws-marketplace-management:*",
                "aws-marketplace:*",
                "aws-portal:*",
                "budgets:*",
                "ce:*",
                "chime:*",
                "cloudfront:*",
                "config:*",
                "cur:*",
                "directconnect:*",
               "ec2:DescribeRegions",
                "ec2:DescribeTransitGateways",
               "ec2:DescribeVpnGateways",
                "fms:*",
               "globalaccelerator:*",
                "health:*",
                "iam:*",
                "importexport:*",
                "kms:*",
                "mobileanalytics:*",
                "networkmanager:*",
                "organizations:*",
                "pricing:*",
                "route53:*",
                "route53domains:*",
               "s3:GetAccountPublic*",
               "s3:ListAllMyBuckets",
               "s3:PutAccountPublic*",
                "shield:*",
                "sts:*",
                "support:*",
                "trustedadvisor:*",
                "waf-regional:*",
                "waf:*",
                "wafv2:*",
                "wellarchitected:*"
            ],
            "Resource": "*",
            "Condition": {
                "StringNotEquals": {
                   "aws:RequestedRegion": [
                       "eu-central-1",
                        "eu-west-1"
                    ]
                },
                "ArnNotLike": {
                   "aws:PrincipalARN": [
                       "arn:aws:iam::*:role/Role1AllowedToBypassThisSCP",
                       "arn:aws:iam::*:role/Role2AllowedToBypassThisSCP"
                    ]
                }
            }
        }
    ]
}
```
### Example - Require Amazon EC2 instances to use a specific type
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RequireMicroInstanceType",
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": [
       "arn:aws:ec2:::instance/*"
      ],
      "Condition": {
       "StringNotEquals": {
         "ec2:InstanceType": "t2.micro"
        }
      }
    }
  ]
 }
```
## Resource Control Policies
* RCPs are limiting control policies. The <span style=color:yellow>  only </span> restrict
* RCPs define the maximum available permissions for <span style=color:yellow>resources </span>in your organisation
* RCP policies are for controlling access to RESOURCES for IAM principles within the organisation and externally
<img src=./AWS-policy-hierarchy.png alt="policy_hierarchy" />

## AWS Compute Services
* EC2 supports Windows, Linux and MacOS
### EC2 IP Address Options
| Type | Description |
|---|--|
|Public IP|<span style=color:yellow>Lost when instance is stopped</span>|
||Used in Public subnets|
||No Charge|
||Associated with a private IP address on the Instance|
||Cannot be moved between instances|
|Private IP |retained when instance is stopped|
||used in Public & Private subnets|
|Elastic IP |Static Public IP|
||Charged if not used|
||Associated with private IP address on the Instance|
||Allows movement between instances and Elastic network adapters|

### Outbound Traffic from a private Subnet
* To allow EC2 instances outbound connectivity from a public subnet you need to deploy a NAT gateway.
* The NAT Gateway will have an Elastic Public IP
* The EC2 instance and NAT gateway will communicate over private IP addresses

<img src=./AWS-Private-Internet-Outbound-Traffic-Flow.png alt="policy_hierarchy" />

## AWS AMI
* The AMI Image defines the:
    * configuration of the instance
    * OS with application installed
    * EBS volumes defined
    * You can create your own custom AMI
## EC2 User Data
* User Data is scripts or commands that runs the first time an instance starts
* Windows server you can run powershell scripts
## EC2 Metadata Service
* metadata provides details about the instance and can help authenticate against the STS service and allow communication through a role to AWS services
* Meatadata address is a local URL http://169.254.169.254/latest/meta-data

## AWS Batch
* AWS Batch is a managed service to run Batch Jobs
* Allows to run a unit of work such as shell script, executable or docker container image
* Batch lainches, manages and termintaes resources as required (EC2, ECS/Fargate)

<span style=color:orange>Exam Tip</span>: Typical scenario is where they want to run batch processes without manging the underlying compute
## AWS Lightsail
* Provides the abaility to run virtual servers.
* AWS called lightsail the Simple Cloud Server
* Low cost and idela for users with less technical expertise
* Services supported are: compute, storage, network, database and loadbalancing
* Can connect to VPC instances

<span style=color:orange>Exam Tip</span>: Typical scenario is hwere an easy method of deplying a virtual server is required by a user with little or no AWS experience

## Amazon ECS (Elastic Container Service)
* Can run docker containers within ECS
### ECS components

|Name|Detail|
|-------|-------|
|ECS Cluster|An Amazon ECS cluster is a logical grouping of tasks or services which can span AZs|
|ECS Task|A running docker container|
|Task Definition|An ECS task is crrated from a task definition. This defined what needs to run|
|Amazon Elastic Container Registry|Docker Image repository|
|ECS Services|This is used to maintain a desired count on the number of tasks(container instances) to run|
|ECS Container Instance|ECS container instances are EC2 instances running the ECS agent and they get joined to the cluster and the tasks get scheduled onto those instances|
||You can configure the Container Instances to scale on demand using the <span style=color:yellow>Amazon EC2 Auto Scaling</span>|
||Amazon ECS uses the <span style=color:yellow>Amazon ECS autoscaling to scale Tasks</span>|
### Amazon ECS Key Features
|Feature|Detail|
|---|---|
|AWS Fargate|Serverless solution that is a fully managed scalable |
|Container Orchestration|Fully managed container orchestration - control plane |
|Docker Support| Run and manage docker containers with integration into docker Compse CLI|
|Windows container support| ECS supports managemenr of windows containers|
|ELB Integration| distribute traffic across containers using ALB or NLB. This is defined as part of the service definitiion|
|ECS Anywhere| ECS control plane to manage on-prem implementations|
### ECS Images
* Containers are created from Read-Only templates called an image which has the instructions for creating a docker container
* Images are built from a dockerfile
* <span style=color:yellow>Only docker images</span> are supported on ECS
* Images are stored in ECR (Elastic Container Registry)
* ECR supports private Docker repos with resource based permissions using AWS IAM roles
### ECS Tasks and Task Definition
* Task definition is required to run docker containers in ECS
* Task definition is a test file in JSON format that describes one of more containers <span style=color:yellow>up to a max of 10</span>
#### Launch Types
* Two types of launch types are:
    * EC2
    * Fargate

<img src=./AWS_launch_types.png alt="launch types" />
<img src=./AWS_Fargate_Launch_Types.png alt="launch types" />

### ECS IAM Roles
* To allow the EC2 host instance to have access to the ECS Service you need create a container instance IAM role that provides permissions to the Host
* To allow a Task to run on a container you must create a ECS Task IAM Role that provides permissions to the container

## AWS Storage Services S3

### Amazon Elastci Block Store (EBS)
* EBS volumes connect to EC2 Instances
* OS sees this as a block based device
* Volumes attach over the network
* EBS volumes <span style=color:yellow>exist within an Az </span>
* EBS volume is automatically replicated <span style=color:yellow>within the Az</span>
* Instance Store volumes are <span style=color:yellow>physically attached</span> to the host and offer high-performance
* Instance store are ephemeral
* EBS volumes offer persistent storage
#### EBS Snapshot
* You can take a EBS snapshot which is a point in time of that volume
* snapshot is stored in amazon S3 not in the Az. S3 is a regional service
* snapshots are incremental and only snap the difference from the last snapshot
* You can move an EBS volume to another Az using a EBS snapshot
* You can create an AMI from a snapshot
#### Amazon EBS Data Lifecycle Manager DLM
*  DLM automates the creation, retention and deletion of EBS snapshots and EBS-backed AMIs
* Can schedule regular backups
* You can retain backups to comply with auditors and internal compliance
* reduce storage costs by deleting outdated backups
* create DR backup policies that backup data to isolated AWS accounts
### Amazon Elastic File System (EFS)
* EFS is <span style=color:yellow>linux Only</span>
* EFS is a shared filesystem
* You can connect to EFS from <span style=color:yellow>multiple Azs</span>
* EFS has an ENI within multiple Azs
* NFS protocol
* One zone filesystem is only available within one Az. You can connect to a mount point from other Azs
* EFS data consistency - write operations for regional file systems are stored <span style=color:yellow>across Azs</span>
* File Locking - NFS client applications use <span style=color:yellow>NFS v4</span> file locking 
* Storage classes
    * EFS standard: Uses SSD for low latency
    * EFS Infrequent Access (IA) - Cost effective
    * EFS Archive - lowest cost
* All storage classes support 11 9s of durability
#### EFS deployment Options
* EFS file systems can be deployed to one region and replicate across regions
* Second region is Read-Only
<img src=./EFS_Architecture_Options.png alt="AWS EFS Architecture Options" />
* EFS Replication - Data is replicated across regions for DR
* EFS DR RTO/RPO is minutes
* Automatic Backup - EFS integrates with AWS backup for automatic file system backups
* Performance options:
1. You have storage classes to choose from which are:
    * EFS Standard
    * EFS Infrequent (IA)
    * EFS Archive
2. Within the storage class you choose provisioned throughput. - This specifies a level of throughput that the file system can drive independant of the file system size
3. Bursting throughput - Throughput scales with the amount of storage and supports bursting to higher levels

### Amazon Simple Storage Service (S3)
* S3 is object based storage
* S3 bucket is the container where you store objects (files)
* Backets can store millions of objects
* Buckets don't have a hierarchy. A hierarchy can be mimicked by using prefixes which extend the key in the URL

The object URL is as below
```
https://bucket.s3.aws-region.amazonaws.com/key
https://s3.aws-region.amazonaws.com/bucket/key
```
* The key is the object or file
To get access or place a file on the bucket you use HTTP verbs like GET, PUT, POST, DELETE
* Bucket names <span style=color:yellow>must be unique across AWS</span>
#### Objects
* Objects consist of:
    * Key (name of object)
    * Version ID
    * Value (actual data)
    * Metadata
    * Subresources
    * ACLs
#### S3 VPC Gateway Endpoint
* This allows devices within a VPC to access S3 using private addresses
* You can also use a S3 VPC Interface also to access S3 privately from a VPC

### S3 Storage Classes
* Durability is protection against data loss and data corruption
* S3 supports 11 9s
* Availability is a measure of:
    * The amount of time the data is available to you
    * Expressed as a percent of time per year using the 5 nines system

<img src=./AWS_S3_Storage_Classes.png alt="AWS Storage Classes Options" />

* S3 Standard is for frequently accessed data with low latency
* S3 Intelligent Tiering - Data will migrate between tiers to optimise cost
* S3 Standard-IA 
    * Used for infrequently Access data but you will be charged:
    * Data retrieval costs per GB
    * Min capacity charge per object of 128KB
    * First Byte retrieval milliseconds
    * There is a min storage duration charge of 30 days
* S3 One zone-IA
    * One in one availability zone which reduces cost and drops the availability to 99.5%
    * First Byte retrieval milliseconds
* S3 Glacier 
    * For archival data
    * must have min storage duration charge of 90 days
    * First Byte retrieval minutes or hours
* S3 Glacier Deep Archive 
    * For archival data
    * cheapest in terms of storage cost
    * must have min storage duration charge of 180 days
    * First byte latency is Hours
### S3 versioning, Replication and Lifecycle Rules
* versioning is a means to lkeep multiple versions of the object in the same bucket
* versioning enabled buckets allow recovery from an accidental deletion or overwrite
### S3 Replication
The different replication options are:
* cross-Region Replication (CRR)
* Same-Region Replication (SRR)

* You <span style=color:yellow>must enable versioning</span> when configuring replication
### S3 Lifecycle Management
* Two types of actions:
    * Transition actions - define when objects transition to another storage class
    * Expiration action - Define when objects expire (deleted)
* With lifecycle management there are supported transiitons as to what tier an object can move from and to

<img src=./AWS_S3_Lifecycle_Management_Transitions.png alt="AWS S3 LM transitions" />

### Amazon FSx
Amazon FSx provides a fully managed third-party file system
* FSx provides you with two file systems to choose from:
    * Amazon FSx for windows file server for windows apps
    * Amazon FSx for Luster for compute-intensive workloads

#### FSx for Windows File Server
* fully managed native MS Windows File system
* fully supports SMB protocol, NTFS, ADDS Integration
* Supports Windows-native file system features:
    * ACLs, shadow copies and user quotas
    * NTFS file systems that can be accesses from up to thousands of compute instances using SMB
* Replicates data <span style=color:yellow>within an Az</span>
* Multi-Az is active and standby fileserver in separate AZs
#### Amazon FSx for Lustre
* optimised for compute intensive workloads
    * Machine Learning
    * HPC
    * Video Processing
    * Financial Modeling
    * Electronic Design Automation (EDA)
* Works natively with S3 allowing you to transparently access S3 objects as files
* S3 objects are presented as files in your file system and you can write your results back to S3
* You get a POSIX style Interface

<span style=color:orange>Exam Tip:</span>  If the exam mentions filesystem with S3 integration it will likely be Lustre

### Amazon S3 Glacier
* S3 Glacier is a secure and durable service for Low cost data archiving and long-term backup
* Extremely low cost and you pay only for what you need with no commmitment of upfront fees
* Three classes are:
    * S3 Glacier Instant Retrieval - Use for archiving data that is rarely accessed and requires millisecond retrieval
    * S3 Glacier Flexible Retrieval - Use for archibes where portions of the data might need to be retrieved in minutes. Lower cost than Instant retrieval
    * S3 Glacier Deep Archive - Use for archiving data that rarely needs to be accessed. Lowest Cost and retrieval time is hours

There are three options for accessing archives on Glacier:
||Expedited|Standard|Bulk|
|---|---|---|---|
|Data access time|1 - 5 minutes|3 - 5 Hours|5 - 12 Hours|
|Data access time (Glacier Deep Archive)|N/A|12 Hours|48 Hours|
* expedited is available for data stored in the S3 Glacier Flexible Retrieval storage class or the S3 Intelligent-Tiering Archive Access Tier

#### Object Lock and Glacier Vault Lock
* S3 Object Lock
    * Store Objects using a write-once-read-many (WORM) model
    * Prevents objects from being deleted or overwritten for a fixed time or indefinitely
* S3 Glacier Vault Lock
    * Also used to enfoce WORM model
    * Can apply a polcy and lock the polcy from future edits
    * Use for compiance and data retention

### Additional S3 Features
* Transfer acceleration: speeds up uploads using cloudfront network. Additional charge
* Requestor Pays - The account requesting the objects pays
* Events - an event can trigger a notification to SNS, SQS and lambda
* Static Website hosting - S3 will host a static website
* Encryption - You can encrypt objects withint he bucket


### AWS Storage gateway
* AWS storage gateway is a service that is used to connect on-prem applications to cloud storage
* enables access to object based storage (S3) using standard protocols such as file based 

Use cases:
    
    * Copying backup data to the cloud
    * using on-prem file shares backed by cloud storage
    * Low latency access to data in AWS for on-premises apps
    * Disaster Recovery

There are three types of Storage Gateway
* File gateway
    * local cache provides low latency access to recently used data
* Volume gateway
* Backup Gateway


|Storage gateway|Protocols|S3 Backend|Detail|
|---|---|---|---|
|File Gateway|NFS / SMB|S3 / S3 Standard IA / S3 One Zone IA||
|Volume Gateway|Block Based|S3 Standard IA / S3 One Zone IA|on-prem servers are mounting a block based volume with S3 backing the storage|
|Backup Gateway|Mount using Block or file protocols|S3 Standard IA / S3 One Zone IA|Appears Virtual Tape Library|

* storage gateways are Virtual Appliances that run within the on-premises datacenter

### AWS Elastic Recovery Service (AWS DRS)
* AWS DRS is a replication and disaster recovery service
* Replicates on-prem servers storage at the block level 

|AWS DRS Component|Detail|
|---|---|
|AWS Replication Agent|Replicate the on-prem disk drives to the AWS cloud|
|Replication Servers|EC2 instances running in AWS. They receive continuous block-level replication to EBS volumes|
||They receive continuous block-level replication to EBS volumes|
|AWS DRS Control Plane|Replication status reporting|
||co-ordinates the automatic creation and termination of resources|
||allows you to perform instance recovery into the recovery subnet with RTO of minutes and RPO of seconds|
|Supporting services|Cloudwatch & event bridge|

* source servers can be:
    * physical infrastructure
    * VMware vSphere
    * Microsoft Hyper-V
    * Cloud (Azure / Google)
    * AWS Cloud - Amazon EC2 instances in a different AWS region


<img src=./AWS_DRS_High_Level_Architecture.png alt="AWS DRS HLD" />

## Route 53
* 'Route 53 has routing policies that you can use to route traffic to a secondary service or server


|Routing Policy|Purpose|
|---|---|
|Simple|Simple DNS responses|
|Failover|If primary is down (based on health checks) routes to secondary destination|
|Geolocation|Uses geographic location client is in to route user to closest region|
|Geoproximity|Routes to the closest region within a geo|
|Latency|Directs based on the lowest latency route to resources|
|MultiValue Answer|Returns several IP addresses and functions as a basic load balancer|
|Weighted|Uses relative weights assigned to resources|
|IP Based|Route based on the originating IP address of the traffic|

### Health Checks
* You can configure Route 53 to perform heartveats agains an IP address of domain name
* The resources you can select to be monitored are:
    * Endpoint
        * IP Address
        * Domain Name (HTTP/HTTPS/TCP)
    * Calculated Health Check
        * This allows Route 53 to see if the status of another health check is healthy or unhealthy
        * One situation where this might be useful is when you have multiple resources that perform the same function, such as multiple web servers, and your chief concern is whether some minimum number of your resources are healthy. You can create a health check for each resource without configuring notification for those health checks. Then you can create a health check that monitors the status of the other health checks and that notifies you only when the number of available web resources drops below a specified threshold.
        * You can create a cloudwatch alarm that monitors metrics, availablility of resources. You can create a Route 53 health check that monitors the same stream of data.
### Amazon EC2 Auto Scaling
* Automatically launches and terminates instances (Scaling Out)
* Maintain availability and scale capacity
* Works with EC2, ECS and EKS
* Integrates with many AWS services, inclusing:
    * Cloudwatch for monitoring and scaling
    * Elastic Load Balancer
    * EC2 Spot Instances for cost optimisation
    * Amazon VPC for deploying instances across AZs
* Cloudwatch can be integrated to monitor metrics at 5min and 1 min frequency.
* Cloudwatch can notify the auto scaling group to launch a new instance. This is accomplished through Cloudwatch alarms
* You can scale on demand and on a schedule
* Scaling policies define how to respond to changes in demand

#### Components of a Auto Scaling group
* Launch Template specifies the EC2 instance configuration
* Launch Templates define:
    * AMI and instance type
    * EBS Volumes
    * Security Groups
    * Key Pair
    * IAM Instance Profiles
    * User Data
    * Shutdown Behaviour
    * Termination Protection
    * Placement Group Name
    * Capacity Reservation
    * Tenancy
    * Purchasing Option

* The Second things that need to be configured are:
    
    * Purchase Options
    * VPC and Subnets across AZs
    *  Attach load balancer
    * configure health cheks for EC2 and ELB
    * Group size and scaling policies
* Amazon EC2 Auto Scaling Health Checks:
    * EC2 = EC2 Status Checks
    * ELB = Uses in addition to EC2 status checks
    * Health Check Grace Period  
    This is how long to wait before checking the health statis of the instance
* Types of auto scaling
    * manual- Makes changes to ASG size manually
    * Dynamic - automatically scales on demand
    * Predictive - Uses ML to predict demand
    * Scheduled - scales based on a schedule

## Amazon Elastic Load Balancing
* Provides HA and fault tolerance for instances
* Targets include:
    * EC2 Instances
    * ECS containers
    * IP addresses
    * lambda functions
    * Other load balancers (You can chain them together)
### Types of ELB
* Application load balancer
    * operates ate the request level HTTP / HTTPS
    * Supports path-based reouting, host-based routing
    * Supports the below as targets:
        * Instances
        * IP addresses
        * Lambda Functions
        * containers
* Network Load Balancer
    * Operates at the connection level
    * Routes connections based on IP data
    * Offers ultra high performance, low latency and TLS offload
    * Can have static / Elastic IP
    * Supports UDP and static IP addresses as targets

<span style=color:orange>Exam Tip:</span>  If the exam mentions ultra low latency or TCP offload load balancer you are looking at Network ELB

* Gateway Load Balancer  
Used in front of Network Virtual Appliances such as firewalls, IDS/IPS and deep packet inspection systems
* Can perform load balancing in front of those appliances 
* Exchanges traffic with appliances with the GENEVE protocol 
* Operates at L3 and listens on all ports
* Forwards traffic to Target gateway in the listener rules

<img src=./AWS_Gateway_ELB_Gateway.png alt="AWS Gateway ELB" />

### ELB Use Cases
* ALB
    * Web Applications with L7 Routing
    * Microservices architectures
    * Lambda Targets
* NLB
    * TCP and UDP based applications
    * Ultra-Low Latency
    * Static IP addresses
    * VPC Endpoint services
* Gateway ELB
    * Deploy, scale and manage 3rd party cirtual network appliances
    * Centralised inspection and monitoring
    * Firewalls, IDS/IPS and deep packets inspection systems


















































