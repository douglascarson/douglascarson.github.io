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










