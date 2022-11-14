# AZ-900 Study Notes

## 1.1
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
### High availability and scalability
* cloud providers offer Service Level Agreements (SLAs)
* Scalability
    * cloud providers provide the ability to scale up or out
    * in azure you can automatically scale by configuring auto-scale groups
    * cloud providers design and monitor their environments to be highly resilient
## 1.3 describe cloud service types
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
 



