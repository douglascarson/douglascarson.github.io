# AZ-900 Study Notes

## 1.1
---
### Cloud Computing
#### Cloud models
* The public cloud
    * you use shared infrastructure that is accessible to the public Internet
    * the network, storage and VMs are provided by the cloud provider
    * Easy and fast to move to the cloud
    * you only pay for what you use
* The private cloud
    * private cloud can be hosted on-prem or in a third party hosting provider
    * two of the main reasons people choose the private cloud are privacy and regulatory requirements
    * Operate on a private network accessible to only one organisation
* The hybrid Cloud
    * mixture of private and public clouds
    Some companies adopt a hybrid cloud as they have legacy systems on-prem or legally can't move the data to the cloud
#### consumption based model
* by only using the resources you require at the time is called a consumption based model
#### compare cloud models
`Public Cloud`
* Advantage
    * public cloud is the most adopted as you shift the resource responsibility to the cloud provider and only pay for what you use.
* Disadvantage
    * give up contol of the infrastructure
    * security concerns about having the application publicly available
    * lock you into the configuration from the cloud provider

`Private Cloud`
* Advantage
    * secure data away from the public Internet and meet regulation
* Disadvantage 
    * more than likely spending money on IT and you will have to pay for hardware and virtualised systems

`Hybrid Cloud`
* Advantage
    * you can accelarate velocity but keep data in secure environmets on premises
    * can use Azure Stack which allows you to run azure services on-premises, which makes it easier to transfer applications to the cloud
* Disadvantage
    * can be complex to set up
    * network complexities
    * speading resource between the cloud and on-prem increases the complexity when troubleshooting
## 1.2 Describe the benefits of using cloud computing
---
### High availability and scalability
* cloud providers offer Service Level Agreements (SLAs)
* Scalability
    * cloud providers provide the ability to scale up or out
    * in azure you can automatically scale by configuring auto-scale groups
    * cloud providers design and monitor their environments to be highly resilient
## 1.3 describe cloud service types
---
### Infrastructure as a Service
* Iaas is the virtualised infrastructure offered by the cliud provider
* You control the OS and other services
* with IaaS the cloud provider is responsible for the VM but you are responsible from the OS, patches, security, DR etc
* IaaS is good if you need a VM for a short period and you can shut it down after. This is an efficient way to manage cost
* Iaas best if you want the cloud provider to manage the infrastructure, but you want to maintain control of what's installed in the OS.
* Good if you occasionally need high-end VMs for specific needs
### Platform as a Service
* In a PaaS model the cloud provider still provides the infrastructure, but they also provide the OS and software installed
* good if all you want to do is deploy application code as PaaS provides the infrastructure, OS and frameworks for the application
* Azure App Service is an example of a PaaS service
* PaaS also provides fault tolerance, elasticity and quick scaling, backup and DR
### Software as a service
* SaaS provider controls the infrastructure, OS and middleware
* They manage the patching, security, compliance, availability and DR
* can adopt a `pay as you go model`
---
## 2.1 Describe the core architectural componentd of azure
## azure regions, regional pairs and soverign regions
* a geography boundry is often a country
* each georgraphy is broken into two or more `regions` each of which are hundreds of miles apart
* MS also operate isolated regions which are government specific
* within each georgraphy MS has created another logical boundry called a `regional pair`
* to meet government requirements about data soverence they have created soverign clouds that are `seperate` from the public cloud
* within the azure government soverign cloud there are a subnset of datacentres that are DoD approved and are approved for DoD usage
## availability zones
The fact regions are physically seperated by hundreds of miles protects azure users from data loss and application outages caused by disasters in a particular region
* To ensure data and apps are available, Azure created `availabilty zones` to safe guard against a building outage in that region
* there is at least three az's within each region
* each AZ has seperate cooling, power and networking
* by deploying into multiple AZ's you can ensure high availability and fault tolerance
* MS supports a 99.99 SLA if you deploy into multiple Az's
* there are two categories of services that support az's
    * zonal services
    * zone-redundant services
* zonal services are services where you must `explicitly` deploy zonal services into two or more zones
* Zone redundant services are services that you have to make `zone redundant` when you `deploy them`.
Azure takes care of the zone redundancy as part of the service 
Azure storage account where yoou select ZRS is an example
## Azure resources and resource groups
* a resource group is a logical container for azure resources
* Resource groups give the benefits of:
    * ability to apply defined permissions
    * ability to breakdown cost based on departments
    * easily setup deplyments usinf a feature using ARM templates. This usually targets a single resource group
    * If you delete a resource group you will `delete all` resources within it
## Azure subscriptions
* additonal azure subscriptions are useful when you want to:
    * have logical groupings for azure resources
    * report on azure resources used by specific groups
    * seperate costs
    * are hitting limits on one subscription
* each subscription has quotas on how many resources or resource groups it supports. In some cases MS support can increase those quoteas but not all
* each subscription is given a global ID called a `subscription ID`
## Management Groups
* management groups are a way to organise, apply policies and access control to your subscriptions or other management groups
* management group limitations
    * limited to a total of 10,000 management groups
    * a max of 6 management groups in a hierarchy
    you cannot have multiple parents for a single management group or subscription
### Hierarchy of management groups, subscriptions and resource groups
* generally you have management group --> Subscription --> resource group
* if you haven't created a management group there is a default one called the `tenant root group`. This is automatically created as part of the initial Azure AD deplyment
---
## 2.2 Describe Azure compute and network services
---
### Compute Types
#### Container Instances
* container instances refer to an app that runs in a docker container runtime
* Two services available for container orchestration is Azure Container Instances (ACI) and Azure Kubernetes Service (AKS)
* ACI is a simplfied way to run a container. You tell it where to get the image (through a docker tag or URL) with some basic config information.
Azure created the server resources and `you don't pay for the VM but only the cpu and memory your container uses`
* ACI is best suited for simple apps and a few containers. If you need to scale you should look at AKS
* When creating an ACI Instance you can't change it's DNS label or the image you pulled. If you want to do this you have to delete and recreate it

#### Virtual Machines
* VMs are used when you need persistent machine availability
* With a VM you pay for a VM if it's allocated to you
* A VM requires an `Azure subscription`, `Custom Management Group` or `Tenant Root group` and `Resource group`
#### Azure Functions
* Azure functions is an app service that runs on Azure app services 
* you only pay for the execution time

#### Availability Sets
* Availability sets consist of two components:
    * Update domains
    * Fault domains
* fault domains as for unplanned failures (like anti-affinity rules)
* fault domains are a logical construct describing a single rack in a datacentre
* If you put two VMs into an availabiility set. You are, in the background creating two fault domains. Azure will place a single VM in a seperate fault domain (Rack)
* you can have a maximum of `3 fault domains`
* Update domains are for planned maintenance
* Update domain is related to the Hyper-V hosts and it will force azure to update the hosts in an organised manner
* maximum of `20 update domains`
* you should always seperate your availability sets into tiers. Front-end, middle, backend
This ensures there is availability for each tier
in the event of an unexpected rack outage
#### Availability set disadvantages
* If you deploy multiple servers into an availability set you have to manually place the servers into that
* an availability set does not deal with load balancing. It only deals with fault and update domains
* Not compatible with availability zones
* to resolve the issues of load balancing and auto scaling Azure introduced `scale sets`
#### Scale sets
* scale sets `in` availability sets automatically. You get the benefit of fault and update domains
* scale sets `are compatible` with availability zones

### Application Hosting Options
#### Webapps on App Service
* Webapps run on VMs in the background
* Basic diagram is Azure LB --> Front-End --> App service Plan
* Front-end is a software that distributes traffic to the VMs running the webapps
#### App Service Plan
There are three tiers available in app service:
* Free
    * No cost tier that runs on a shared VM
* Shared
    * low cost tier for testing with some addtional features but still using a shared VM
* Basic, Standard, Premium and Premium v2
    * Higer cost tiers that run on dedicated VMs
* The only way to stop being billed for an ASP is to delete it
* If you are running Basic, Standard, Premium or Premium V2 you can scale out or in
#### ASP Scaling Limits
* Basic = Max 3 Instances
* Standard = Max 10 Instances
* Premium / Premium V2 = Max 20 Instances
#### App Service Plans Runtimes
* Webapps support PHP, .Net, Java application runtimes
* Webapps support docker
* webapps support static webapps

Static webapps are webapps that are linked to a code repo and are updated when there is a commit to the repo

#### Azure Kubernetes Service (AKS)
* Kubernetes creates containers in `pods`
* A pod is a group of related containers, and containers within a pod share resources
* A container in one pod cannot share resources with a container in another pod
* The computer that pods run on are called a `node` or a `worker`
* The `node` or `worker` has to have the container runtime installed such as containerd or docker
* All nodes are controlled by a node called the `kubernetes control plane`
* The entire environment of the kubernetes control plane and nodes is called a kubernetes cluster
* AKS creates the Kubernetes cluster and control plane for you
* AKS cluster is free. You only pay for the compute resources you use within you cluster not the management cluster
#### Azure Spring Cloud
* Azure Spring Cloud is a PaaS service that to host and run spring apps easily
* Spring Cloud also integrated with Azure SQL, Azure Storage and other PaaS services
 ### Virtual Networking
 #### vNet Peering
 * To allow vNets to communicate with each other you can use vNet peering
 * vNet peers travel over Microsofts private backbone
 * You can peer vNets in the same region
 * You can use `global vNet peering` to peer vNets across regions
 * You have to perform vnet peering on both sides
 * A limitation of global vnet peering is if you have to connect to resources that are behind a `basic tier Azure load balancer` you won't be able to connect to those resources using the public IP. If this is a requirement you have to upgrade the load balancer to a `standard tier`
 #### Azure DNS
 * Azure DNS allows you to host Public and Private zones
 * A public zone contains enteries for the `public endpoints` and private zone contains entries for `private endpoints`
* When creating a DNS zone. It is be default Public
* Once you create a private zone you have to `link` the zone to the vNet
#### Azure VPN Gateway
* VPN gateway can also be referred to as Virtual Network gateway
* A VPN Gateway uses two or more VMs inside a `subnet` called the `gateway subnet`
* There are three connection types supported by VPN Gateway:
    * vNet-to-vNet
    * Site-to-site
    * Point-to-Site
* A vNet-to-vNet connects `two vnets together`. Each subnet needs a VPN Gateway and they don't have to be in the same Azure Region or subscription
* When using a vNet-to-vNet VPN Gateway make sure you have scaled the size to ensure you meet the bandwidth requirements. If you need to avoid bandwidth restrictions look at global / vnet peering
* A site-to-site connection allows you to connect your vnet to an on-prem network
* site-to-site connections can authenticate using Azure certificate authentication, RADIUS, Azure AD or OpenVPN
* A point-to-site connection connects your vNet to a single end user device like client laptop, mobile, tablet
* Azure VPN Gateway has a cap of `1.25 Gbps`
#### Azure Express Route
* Express route is a good option when you require more than 1.25Gbps and you don't want to send your traffic over the Internet
* Express Route can be up to 10Gbps over dedicated fiber connections
* MS can refer to an Express Route connection as a `circuit`
* when you connect to Azures network you will be connecting to a `Microsoft Enterprise Edge router (MSEE)`
* The most common scenario is the customer will connect to a Microsoft Enterprise Edge Router through a service provider or telco

Customer --> Service Provider --> MSEE (Router) --> Azure

* `Express Route Direct` is when you `remove the Service Provider`. This allows you to connect to the physical port on the MSEE Router
* All express routes are dual paired. If you provision a 5Gbps circuit. MS will provision 2 x 5Gbps links. You can burst for a short time and use more than 5Gbps
---
## 2.3 Describe Azure Storage Services
---
### Azure Blob Storage
* Azure blob storage is designed to store data with no defined structure
* An entity stored in blob storage is referred to as a `blob`
* There are three types of blobs:
    * Block Blobs
    * Append Blobs
    * Page Blobs
* Block blobs store files .png, .tiff, .txt
* Block blobs can have hot, cool and archive tiers
* Append blobs are like block blobs but are optimised for append operations. It's good for storing logs.
You can only have a hot tier for append blobs
* Page blobs are used to store vhd files
* Page blobs can only be hot tier

### Azure Disks
* Azure Disks refer to disks that are used in VMs
* Azure disks are available in HDD and SSD
    * Azure SSD standard is for light use
    * Azure premium SSD for heavy use
* You can snapshot the OS or data disk. With the OS disk you can create a VM from the copy
* A snapshot is a disk copy
* The amoount of disks a VM can support is determined by the VM type

### Azure Files
* Azure file shares are backed by Azure Storage
* You can mount Azure Files on Azure VMs, on-prem windows, linux and MacOS machines
* Azure Files supports SMB 2.1
* You can't use windows 7 and windows 2008
* To have a copy of files on premises and sync it with Azure files you can use an application called `Azure File Sync`

### Azure Queues (Azure Queue Storage)
* Azure Queue storage is designed for queueing many messages and processing those items
* Messages in Azure Queues are accessible using a URL and requests are authenticated
* A message has a default TTL of 7 days but you can customise this
* After the expiry of the message TTL it is deleted

### Storage Tiers
* There are several tiers of blob storage:
    * Hot - frequently accessed and most expensive. 
    * cool - Less frequently accessed but not as expensive. Must keep the data in for `min 30 days`
    * archive - Long term storage. Access costs the dearest. Must be kept for a `min 180 days`. Access to the first byte is within 15 hours.
* If you want data back from the archive tier you have to `rehydrate it to the hot or cool tiers`
### Storage tier Redundancy Options
#### Data redundancy
* The storage account is the underlying resource for all storage services
* You `cannot change the redundancy option once the storage account has been created`
* Primary Region Redundancy (Where storage account is created)
    * Locally Redundant Storage (LRS)
        * LRS makes three copies within a single datacentre
        * Least expensive but least redundant
        * Only protects your data from a disk or rack failure
    * Zone Redundant Storage (ZRS)
        * You have three copies of your data in multiple datacentres in a region
        * protects your data from a single datacentre outage
        * `doesn't protect against a region failure`
* Data writes to Azure Storage in LRS and ZRS are synchronous. This means a commit is received once Azure has written all three copies successfully
* Multi-region redundancy
    * Geo-redundant Storage
        * create three copies in a datacentre in the primary region and three copies in a single datacentre in the secondary region
    * Geo-Zone Redundant Storage
        * creates three copies in multiple datacentres in the primary region and three copies in multiple datacentres in the secondary region
        * In the event of a region failure Azure will change the DNS information to point you to the secondary region
        * If you require read-only access to the data in the secondary region you can select `Make read access to data available in the event of regional unavailability`. This changes the Replication to Read Access Geo-Zone Redundant Storage
* All writes `within a region are synchronus` and `Inter region is asynchronous`
#### Storage accounts and Storage Types
* There are several storage account types and you cannot change this after it has been deployed
* Storage account Types:
    * Standard Tier General-purpose V2
        * This a general purpose tier
    * Premium Tier Block and append Blobs
        * This is used when there is a requirement for many transactions
    * Premium Tier File Shares
        * Used if you need NFS
    * Premium Tier Page Blobs
        Only support Page blobs
* Premium storage types only support SSD for enhanced performance

## Migrating to Azure
* To migrate VMs, databases, webapps and VDI to the Azure cloud you can use the Azure Migrate service
* There are three phases to Azure Migrate:
    * Discover
        * You can use a CSV file to disover all the servers, databases etc
        * You can tell the azure migrate appliance to perform a discovery
    * Assess
        * The assessment phase can look at server dependancies and resource consumption
        * Ypu can also create an expected resource pricing estimate based on the recommended sizings
    * Migrate
        * You can replicate data to the cloud
        * If you have large amounts of data you can use Azure Data Box. Data Box comes in three options:
            * Data Box Disk
                * Can only copy data to a single storage account
                * Max capacity of 35TB
                * transfers through USB
            * Data Box
                * Can copy data across `10` seperate storage accounts
                * Rugged device containing max 80TB is RAID 5
                * 2 x 1 Gbps NICs
                * data is encrypted
            * Data Box Heavy
                * Can copy data across `10` seperate storage accounts
                * Max capacity 770TB
                * 4 x network cards
                * 2 x 40 Gbps
---
Skill 2.4 Describe Azure Identity, access and security
---
## Azure AD
* Azure AD is an Identity source that is created automatically once you create a new subscription
* It performs `authentication` and `authorisation`
## Azure Active Directory Domain Services (Azure AD Domain Services)
* Azure AD DS provides the same services as on-prem MS AD DS such as:
    * Ability to join domains
    * GPOs
    * kerberos
    * NTLM
* There are three primary reasons for using Azure AD DS:
    * Use of older apps that don't support the authentication mechanisms in use today
    * Integrate Azure resources into your on-premises Windows AD DS
    * You want to lift-and-shift on-prem applications to the cloud that rely on MS AD DS
* When you create a domain in MS AD DS it is the equivalent of a domain on-prem
* MS manages the DCs for you and ensure they are backed up
* when you create a new domain Azure will create two DCs called a `replica set`
* `Azure AD Connect` allows you to connect your on-prem AD DS to Azure AD DS. It can also ensure the cloud and on-prem domains are in sync
* Azure AD DS offers three SKUs:
    * Standard
    * Enterprise
    * Premium

* Standard has a maximum of `25,000` objects and `3000` authentications
* Standard is designed for a single forest
* Enterprise has a maximum of `100,000` objects and `10,000` authentications
* Enterprise allows up to `five` resource forest trusts
* Premium has a maxium of `500,000` objects and `70,000` authentications
* Premium allows up to `10` forest trusts

## Azure Authenication
### SSO
* For a device to work with SSO it must be joined to Azure AD
* Access to on-prem resources with SSO is accomplished by syncing MS AD DS objects using `azure AD Connect`
* SSO supports two sign-in methods:
    * Password Hash Sync
    * Pass-through authentication
* Password Hash Sync syncronises a users password has to Azure AD using Azure AD Connect
* Pass-through auth:
    1. passes a users login to Azure AD to an on-prem pass-through authenication agent
    2. The agrnt send the authentication to the on-prem Windows AD DS domain
    3. Once authenticated Azure AD Connect is used to pass that authentication through to Azure AD
### MFA
* MFA is a combination of:
    * something you know (username/Password)
    * Something you have (phone/mobile phone)
    * Something you are (facial recongnition / fingerprint)
* Azure is 2FA
* To enable MFA you need to click `per-User MFA` within Azure AD Users
### Passwordless Authentication
* passwordless uses multi facor authentication such as:
    * something they have (device / security key) & something they are (biometrics)
* there is a passwordless preperation wizard at http://aka.ms/passwordlesswizard
### External Identities and guest access
* People who are in your org called called members
* You can invite people from outside your org and they are called guests
* When you invite a user from outside your org Azure AD uses a feature called `Azure AD B2B`
* When you invite a guest user you can specify which `enterprise applications` that can access
### Azure AD Conditional Access
* Requires `Azure AD Premium` 
* Azure AD conditional access allows you to create policies and apply them to users
* Azure AD conditional access uses `assignments` and `access controls` to configure acess to your resources
* Assignments define who a policy applies to. It can apply to:
    * users
    * groups
    * roles
    * guest users
    * applications
* Assignments can also define conditions that must be met:
    * require a certain platfom (android, IOS, windows etc)
    * specific locations
    * time of day
* access conrol detemines how an `access control policy is enforced`
* You can use access controls to block, enforce a certain device type, 
### RBAC
* RBAC is the process of authorising users to a system that is based on defined roles
* There are four elements to RBAC:
    * Security Principal
    * Role
    * Scope
    * Role assignments
* Service Principal represents and identity it can be a:
    * user
    * group
    * application (which is called a service principal)
    * AAD entity called a managed identity. A managed identitiy is how you authorise an azure service to access your azure resoure
* Role is what defines how the security principle can interact with an azure resource
* scope defines what level the role is applied
* Role assignments: roles are defined to security principles at a certain scope
#### RBAC Generic Built-in Roles
* Owner: Full access
* Contributor: create, edit and delete resource but not adjust permissions
* Reader: Read-Only
* all other roles are specific to the resource
* RBAC roles can be scoped to:
    * management level
    * Subscription
    * Resource Group
    * resource
* Your RBAC roles are cumulative unless a deny is introduced
* all RBAC and interactions through API, Portal, cli are through ARM

### Defence-in-Depth Zero Trust
* MS has developed a defense in depth strategy called `zero trust`
* zero trust relies on identifying users and uses conditional access to control access to resources
### MS Defender for cloud
* Defender for cloud can protect you cloud resource in azure, AWS and GCP
* Defender for cloud provides insights into four areas:
    * Security Posture
    * Regulatory Compliance
    * Workload protection
    * Firewall manager
* Security Posture provides a security score
* Regulatory Compliance provides high level overview of the compliance for azure resources
* Workload Protection shows the protection for each of your service types
* Firewall manager provides insights into the security of your networks in Azure
---
# Describe cost management and governance
## Skill 3.1 Describe cost management in Azure
### Factors that can affect costs
* Regions have different pricing based on how expensive it is to run the MS datacentres in that region
* How you architect the solution can have significant cost differences
* Long term agreements can be used to reduce costs if you agree to pay up front
* Azure regions are broken out into four seperate groups for billing purposes. The groups are called `billing zones`. MS costs for egress traffic out of each zone might differ
### Reduce costs
* If you are using VMs month over month `azure reservations`
* Consistent usage of Azure SQL database, Cosmos DB or Azure Synapse Analytics you can use `reserved capacity pricing`. Reserved capacity pricing is like a reservation for VMs
* Hybrid use benefit where you use your own licensing
* For workloads that only run for a short time or are disposable you can use spot instances
### Pricing Calculator and TCO Calculator
* TCO Calculator is helpful to estimate your expense for new applications in azure
* When you use the TCO Calc you enter your on-prem:
    * Servers (Cores/memory)
    * Storage (space/IOPS/Bandwidth)
    * Databases
    * Network (bandwidth)
---
Skill 3.2 Describe features and tools in azure for governance and compliance

--
## Azure Blueprints
* Azure blueprints is a service that can make the deployment of cloud easier and more automated
* Items in a blueprint are called artifacts
* Artifacts can be a resource, an ARM template, policy assignment or a role assignment
* You can save a blueprint to a `subscription` or `management group`
* a blueprint saved in a `management group` can be used by any subscription within that management group
* Azure maintains a link between the blueprint and the resources created by that blueprint
* blueprints are versioned controlled and  can be checked into a VCS
* You cannot change the name or definition location after creation
## Azure Policy
* Azure Policy allows you to define rules that are used when you are creating resources
* Azure Policy doesn't apply to resources already created unless you run a remediation task
* There are six supported effects:
    * Append
    * Audit
    * AuditifNotExist
    * Deny
    * DeployifNotExists
    * Disabled
* Append add addtional properties to a resource. It can b used to add tags
* Audit logs a warning if the policy is not complied with
* AuditifNotExists. Allows you to specify an additional resource type that must exist created along with the resource being created or updated
* Deny Denies the create or update operation
* DeployIfNotExists. Allows you to specify an additional resource type you want deployed with the resource being created or updated. If that resource type is not included it is automatically deployed
* Disabled. The policy is not in effect
## Resource Locks
* To create a resource lock you must be the `owner` or `User access administrator`
* locks can be applied at the subscription, resource group or resource level
* options for resource locks are:
    * Read-only. This does not allow resource changes
    * delete. This allows changes
* locks are inherited
* The most restrictive wins. So Read-Only will override Delete
## Service Trust Portal
* MS have made all the information for tools on trust, security and compliance in a web portal called Service Trust Portal
* Service trust portal is also where you find links to `compliance manager`
---
## Skill 3.3 Describe features and tools for managing and deploying azure resources

---
## CLI
* You can list and install extensions to the azure CLI by using the command
```
# list extensions
az extension list-available --output table
# Install externsions
az extensions install extension_name
```
* You can use the cli in interactive mode by running the command
```
az interactive
```

## Azure Cloud Shell
* azure cloud shell is a cli environment running from a container within azure
* a storage account is used to store anything that was installed and settings
## Azure Arc
* Azure Arc extends management and governance features of azure to on-prem and other cloud providers
* It uses one of two methods:
    * Azure Arc enabled servers
    * azure Arc enabled Kubernetes
* To use Arc enable-servers you need to install the `Azure Connected Machine Agent`
* Arc Enabled servers can be windows and linux
* once the agent is installed it will have a `managed identity` in Azure. You can manage the server with RBAC, Azure Policy, Tags, Protection with Azure Defender for cloud
* Azure Arc Enabled Kubernetes requires an agent installed
## Azure Resource Manager and ARM Templates
* normal flow is Tool --> ARM API --> ARM Service (auth/authorises) --> Resource provider
* ARM Templates are declaritive and written in JSON
---
## Skill 3.4 Describe monitoring tools
---

## Azure Advisor
* Azure advisor can notify you on:
    * cost
    * Security
    * Reliability
    * best practises
    * performance
* Advisor can remediate the issue for you
* You can download the advisor recommendations in pdf and CSV formats
## Azure Service Health
* Azure service health shows you:
    * service issues
    * planned maintenance
## Azure Monitor
* Azure monitor aggregates metrics and alerts and places them into one view
* Alert rules can have `multiple conditions`
* When an alert is triggerd it performs an action that is specified in an `alert group`
* An alert group can alert (email/sms/push/voice) or perform an action

# Azure Support Plans
There are several support plans such as:
    * Basic
    * Developer (cheapest paid-for support)
    * Standard
    * Professional Direct
    * Premier
24/7 access to tech support by email and phone is only available with `standard, Professional direct, premier plans`
* you can submmit support tickets for all support plans

# Cloud adoption framework for Azure
The stages are:

    * Define strategy
    * Plan
    * Ready
    * Adopt
    * Govern
    * Manage
    * Govern














