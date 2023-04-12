# AZ-104 Study Notes
## Describe AzureAD
* AzureAD is MS multi tenat cloud identify service
* Some of the authentication protocols AzureAD uses are:
    * OpenID
    * WS-Fed
    * SAML
    * Oauth
## AzureAD features
* SSO
* Ubiqitious device support
    * Support Windows, IOS, Android and macOS
* Secure access to on-prem and cloud apps using:
    * MFA
    * conditional access
    * group based access management
* cloud extensibiliy
    * Connect your on-prem AD DS to AZure AD which allows you to manage users, groups and devices across environments
* Sensitive data protection
    * Can monitor suspicious sign-in activities and potential vulnerabilities from a central location
* self-service support
    * delegate tasks to employees such as password reset or app access
## AzureAD Concepts
* Identity
    * An object that can be `authenticated` 
    * Can be applications, servers that require authentication by using a secret key or cert
* Account
    * An account is an `identity with data associated`. You must have an idenity to have an account
* Azure AD Account
    * An Azure AD account is an identity that was created through a Microsoft service. The Azure AD account is also called a work or school account.
* Azure AD Tenant
    * An Azure tenant is a single dedicated and trusted instance of AzureAD. A tenant (`directory`) represents a single organisation. An Organisation can create more than one tenant
* Azure Subscription
    * This is a biling construct.  You can have multiple subscriptions linked to one AzureAD tenant
## Compare AzureAD to AD DS
* Azure AD is designed for Internet based applications that use HTTP or HTTPS
* AD DS supports Kerberos, NTLM where as AzureAD supports HTTP bases authentication protocols such as OpenID, WS-Fed, SAML and Oauth
* AzureAD includes `fderation Services` for many third-party services
* AzureAD is  flat structure and does not include OU's or GPOs
## AzureAD Editions
* AzureAD comes in 4 editions:
    * Free
    * M365 Apps
    * Premium P1
    * Premium P2
* Free comes with all sign ups
* Premium comes with an `Enterprie Agreement`, `Open Volume License Program`, and the `Cloud Solution Providers`
### AzureAD Free
* User & roup Management
* On-prem directory sync through AzureAD Connect
* Basic Reports
* SSO
### AzureAD M365 Apps
* Included with M365
* Free + Identity and Access Management for M365 Apps
* Branding
* MFA
* Group Access Management
* Self Service Password reset for cloud users
### AzureAD Premium P1
* Free + Access to on-prem and cloud resources
* Dynamic Groups
* Self Service group management
* Cloud write back capabilities
* Includes `Identity Manager`
* on-prem identity and managemnt suite
* The extra features in P1 allow self-service password reset for your on-premises users
### AzureAD Premium P2
* Free + P1
* Azure AD Identity Protection to help provide risk-based Conditional Access
* PIM
## AZureAD SelfServie Password Reset
* Global Admin account can always SSPR
* SSPR uses a `security group` to limit the users who have SSPR enabled
* All users in the org must have a valid license for SSPR
* When enabling SSPR there are three options:
    * None
    * Selected (select the security group that enables SSPR)
    * All
* Select the amount of authenticaton methods and the type a person has to satisfy to reset their password
    * Options include:
        * email notification
        * text message,
        * security code sent to the user's mobile or office phone
        * User a set of security questions


## Skill 1.1 Manage Azure identities and governance
* there are three types of objects in Azure AD
    * Users, Groups, Devices
* There are two types if User Objects
    * Cloud Only
    * Synchronised
* To create cloud only users within AzureAD you must have `Global Administator` or User `Administrator roles`
* Non-admin users can set some of their own profile data, but they can't change their display name or account name
* AzureAD groups can contain Users, Groups, Devices and `Service Principles`
* When creating AzureAD groups there are two types:
    * Security
    * Office 365
        * An 0365 group is to give access to a shared mailbox, calendar or sharepoint site
* when using dynamic groups you can populate the group with users or devices but not both
* You can transition from a dynamic group to an `assigned group` and back
* changing the group type from assigned to dynamic will affect access to it members if they have been granted access to services
### Manage Device Settings
* To enable, disable or delete devices you must be a global admin
* deleting a device will delete the bitlocker keys etc
* Disabling the device only prevents the user from accessing resources `from that device`
### Bulk user updates
* you can create, update and delete users in bulk
* you can download a csv and populate the csv with what ever info is required
* mandatory fields are:
    * Name
    * User Name
    * Initial Password
    * Block Sign-In
### Guest Users
* Once a guest user has been created they will receive an invite
* By default `all users and admins` can invite guest users
* To restrict all users and admins being able to invite guest users you can go to `manage external collaboration settings` under the users blade under user settings
* until the user accepts the invite they will show as `invited user`
### Configure AzureAD Join
* when associating a device with AzureAD you have three options:
    * Register a device (best for BYOD)
    * join a device (best for corporate)
    * Hybrid-AD (joined to AD DS and replicated)
* device registration settings are in Devices > device settings
* Device settings:
    * Users may join devices to azure AD
        * by default all users are allowed to join their device to AzureAD. You can select `all`, `group` or `none`
    * Additional local admins on Azure AD joined devices
        * with AzureAD premium or Enterprise mobility suite you can choose which users or groups have local admin to the device. By default the 'global admin' and `device owner` has local admin right
    * Users may register their devices with AzureAD
        * allow user to register their devices with AzureAD (Workplace Join)
        * if you use InTune or MDM this will be set to `all` and will be greyed out
    * Require MFA to join devices
        * You must have the users approved to do this configured with MFA
    * Max number of devices per user
        * 5, 10, 20, 50, 100 and unlimited
    * Manage Enterprise state roaming settings
        * Users May Sync Settings And App Data Across Devices
        * only applicable to windows 10
* AzureAD Join
    * there are two types of AD Join:
        * Non-Hybrid
        * Hybrid
    * Non-Hybrid Join
        * requires windows 10 pro and enterprise
    * Hybrid
        * requires windows 10 or server 2016
        * windows 7, 2012, 2012R2 2008, 2008R2
### Configure SSPR (Self service password reset)
* You can deligate users to reset their own passwords. This can we writte back to on-prem as long as you have the correct `license` and Azure AD Connect is configured.
* Password Change - cloud only account
    * All versions and all licenses
* Password reset - Cloud Only
    * M365 Business Standard
    * M365 Business Premium
    * Azure AD Premim P1 & P2
* Password Change/Unlock/Reset - Hybrid
    * M365 Business Premium
    * Azure AD Premim P1 & P2
* Authentication Methods for SSPR
    * Mobile App Notification
    * Mobile App Code
    * Email
    * Mobile Phone
    * office Phone
    * Security Questions
* Notification can be configured
* On-prem Integrations
    * You can cnfigure the write back configurations within this blade
    * You have the option to
        * write passwords back using Azure AD Connect cloud sync
        * Enable password write back for syncd users
        * allow users to unlock their accounts without password reset
# 
# Skill 1.2 Manage RBAC
* RBAC allows you to manage the identies or `security principles` that have `access to azure resources` and the `actions` they can perform
* In Azure RBAC can be applied to users, groups, service principals and managed identities that are applied at a scope
* within azure there is role inheritance
* In azure RBAC model is additive. This means the `most privilegdged takes precedence`
## RBAC Scope
* There is a scope hierarchy
    * management group
        - Subscriptions
            - Resource Group
                - Resource
* to create and remove role assignmnts you need the `Microsoft.Authorisation/role assignments/*` permissions
* This permission is included in the role `Owner` and `User access addmin`
## Custom Roles
* There are three ways to create a custom role:
    * clone an existing role
    * JSON file 
    * start from scratch
* When creating a role there are two types of actions:  
    * Actions - Actions are operations an action can perform
    * Data Actions - actions the can be performed on the data of the resource
* you can also exclide actions and dat actions by using the `not actions` and `not data actions`
#
# Skill 1.3 Manage Subscriptions and Governance
* A subscription is a billin boundry
## Azure Policy
* Azure Policy is a service that is used to:
    * create, assign and manage policies that enforce governance
* to implement policy you need to:
    * authour a definantion
    * assign it to a scope
* a policy definition describes your desired behavour at resource creation and update
* through a policy definition you declare what resources and resource features are considered compliant and what should happen when something isn't compliant
* to apply multiple policies you would create an `initative` and assign multiple policies to it
* you can scope policy definitions and initiatives to:
    * Management Groups
    * Subscriptions
    * Resource Groups
* You can `exclude` from scopes. You can exclude:
    * subscriptions
    * resource groups
    * resources
## Resource Locks
* There are two types of resource locks:
    * delete
    * ReadOnly
* You can set a resource lock at scope levels:
    * Subscription
    * Resource Group
    * Resource
## manage Tags
* Not all resources support tags
* You can use tag to provide metadata to help billing accounting
* resources, resource groups and tags are limited to 50 tags
* tag names can't exceed 512 characters and storage account it is 128
* tag value can't exceed 128 characters
* tags are not inherited by child resources
* tags cannot contain special characters
* to apply tags you must have write access (contributor and up)
* You can apply tags through ARM templates, powershell or CLI
```
# Create Tags
# Powershell
tags = @{Environment="dev"; Application="sap"}
$resource = get-azresource -name testserver -resourceGroup testRG
new-aztag -resourceId $resource.Id -tag $tags
# azure CLI
az tag create
```
* You can also update tags: There are two actions:
    * merge - updates existing tags and creates new ones if missing
    * replace - replaces all tags with the new ones
    * delete - deletes the specified tags from the listed resources
* to do this you use the -operation switch
```
# Update Tags
tags = @{Environment="dev"; Application="sap"}
$resource = get-azresource -name testserver -resourceGroup testRG
update-aztag -resourceId $resource.Id -tag $tags -operation replace
```
## Resource Groups
* You cannot nest resource groups
* resource for different regions can me in the same resource group
* resources can only be in `one` resource group
* a resource group can only be in one region
* resource groups can be used to as a scoping target for policy and IAM
### Moving Resources between RGs and Subscriptions
* Only certain resources can be moved between resource groups and subscriptions
* to move a resource between subscriptions they have to be using the same `azure ad tenant` 
`* check as i think that has changed`
* when moving resource between sbscriptions you need to validate:
    * the resource provider is available in the destination subscription
    * the target subscription is not restricted by the quota limits
    * You cannot move more that 800 resources
    * If there are any dependant resources all the reources must be in the `same RG and moved all together`
## Managing Subscriptions
* Thera are several types of subscriptions:
    * Free
    * Pay as you go
    * VS/MSDN Subscriptions
    * MS Reseller
    * CSP
    * MS Open Licensing
    * EA
* some subscrptions have limitations such as:
    * VS - you have limited regions and no CC associated
* within Azure there are four foundational roles:
    * Owner
    * Contributor
    * Reader
    * User Access Administrator
## Management Groups
* Management groups allow subscriptions to be organised multi-hierarchial. The benefits are:
    * Reduce Overhead - You can apply governance at the management group level instead of the subscriptio level, reducing overhead
    * Enforcement - Apply governance at the mgmt group level which the subscription admins can't change
    * Reporting
* Max levels is 6
## Cost management
* You can create budgets within cost management
* budgets are a monitoring mechanism with set budge thresholds and triggers
* User must have subscription `Read` to see cost data or `Contributor` or higher to see budgets
* There are also dedicated roles that give users access to cost management `data` called:  
    * Cost management reader
    * cost managemnt contributor
* there are three portal used for subscriptions and cost management:
    * https://ea.azure.com - Used for EA agreements and managing spend across multiple subscriptions
    * https://account.azure.com - Used for all subscribtions and accessible by account owners `This is decomissioned and is now in portal.azure.com`
    * https://portal.azure.com - Used for all subscriptions and includes `Azure Cost Management`
# Skill 2.1 Secure Storage
* An azure storage account is an entity used to store azur storage data such as:
    * blob
    * table
    * queue
    * file
    * page
## Storage Firewall
* The storage firewall allows you to limit access to a storage account from cert IP addresses or ranges
* Once applied it applies to all services (blob, queue, table)
* Service endpoints are used to restrict access to `certain subnets within a vNet`. The client is still using the PIP of the storage account
* Allow Trusted Microsoft Service to access this account is used to allow certain `trusted` sevices access, such as:
    * Azure Backup
    * ASR
    * Azure Networking
* Service Endpoints provide two benefits:
    * Restict access to only the subnet within the vNet configured
    * provide a more efficient route to access the storage account
* by default read access to anonymous users is blocked
* blob storage access levels:
    * Private - Only the storage account `owner` has access to the container and it's blobs. No one else has access
    * blob - Only blobs within this container can be access anonymously
    * container - blobs and the container can be accessed anonymously
* the access level is configured `per container`
* 
