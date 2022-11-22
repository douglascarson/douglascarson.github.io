# AZ-900 Study Notes

## 1.1
---
### Cloud Computing
#### Cloud models
* The public cloud
    * you use shared infrastructure that is accessible to the public Internet
    * the network, storage and VMs are provided by the cliud provider
    * Easy and fast to move to the cloud
    * you only pay for what you use
* The private cloud
    * provate cloud can be hosted on-prem or in a third party hosting provider
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
* a geography boundry is oten a country
* each georgraphy is broken into two or more `regions` each of which are hundreds of miles apart
* MS also operate isolated regions which are government specific
* within each georgraphy MS has created another logical boundry called a `regional pair`
* to meet government requiremts about data soverence they have created soverign clouds that are `seperate` from the public cloud
* within the azure government soverign cloud there are a subnset of datacentres that are DoD approved and are approved for DoD usage
## availability zones
The fact regions are physically seperated by hundreds of miles protects azure user fromadat loss and application outages caused by disasters in a particulare region
* To ensure data and apps are available, Azure created `availabilty zones` to safe guard against a building outage in that region
* there is at least three az's within each region
* each AZ has seperate cooling, power and networking
* by deploying into multiple AZ's you can ensure high availability and fault tolerance
* MS support a 99.99 SLA is you deploy into multiple Az's
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
* Append blobs are loke block blobs but are optimised for append operations. It's good for storing logs.
You can only have a hot tier for append blobs
* Page blobs are used to store vhd files
* Page blobs can only be hot tier

### Azure Disks
* Azure Disks refer to disks that are used in VMs
* Azure disks are available in HDD and SSD
    * Azure SSD standard is for light use
    * Azure premium SSD for heavy use










