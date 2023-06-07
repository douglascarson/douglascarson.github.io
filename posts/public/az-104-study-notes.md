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
* User & Group Management
* Protect Azure AD tenant admin accounts with MFA
* Mobile app as a second factor
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
* Cost per user. cost is approx $9 per month per user
* Free + Access to on-prem and cloud resources
* `conditional access`
* Dynamic Groups
* Self Service group management
* Cloud write back capabilities
* Includes `Identity Manager`
* on-prem identity and management suite
* The extra features in P1 allow self-service password reset for your on-premises users
* Trusted IPs
### AzureAD Premium P2
* Free + P1
* cost is $12 per user per month
* Azure AD Identity Protection to help provide `risk-based Conditional Access`
* PIM (protect admin accounts and JIT)
* Access Reviews
## AzureAD Self Service Password Reset
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
* you can add members to groups in bulk
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
### Administrative Units
*  determines what admin users can work with what parts of the organisation.
# Skill 1.2 Manage RBAC
* RBAC allows you to manage the identies or `security principles` that have `access to azure resources` and the `actions` they can perform
* In Azure RBAC can be applied to users, groups, service principals and managed identities that are applied at a scope
* within azure there is role inheritance
* In azure RBAC model is additive. This means the `most privileged takes precedence`
### RBAC Default Roles
* Owner
* Reader
* contributor

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
* you can also exclude actions and data actions by using the `not actions` and `not data actions`
#
# Skill 1.3 Manage Subscriptions and Governance
* A subscription is a billing boundry
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
## Manage Tags
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

# Types of Storage

## Skill 2.1 Secure Storage
* An azure storage account is an entity used to store azur storage data such as:
    * blob
    * table
    * queue
    * file
    * page
### Storage Firewall
* The storage firewall allows you to limit access to a storage account from cert IP addresses or ranges
* When using the storage firewall you `must` use public IP addresses not private
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
### Storage Account Performance Tiers
* Standard
    * Supports all storage services:
        * blob
        * table
        * file
        * queues
        * Unmanaged storage disk
    * Three possible values for the Standard Tier:
        * General Purpose V1
        * General Purpose V2
        * Blob Storage
* Premium 
    * Backed by SSD
    * Only supports General purpose accounts
    * Disk Blobs and page blobs
    * Block Blobs and Append Blobs
    * Azure Files
    * Only supports `LRS` replication
    * There are 4 values for premier Tier:
        * General Purpose V1
        * General Purpose V2
        * BlockBlobStorage
        * FileStorage
* Only General Purpose V2 support Hot, Cool and Archive Access Tiers
* General Purpose V1 can be upgraded to V2 but not back

||General Purpose V2| General Purpose V1| Blob Storage| Block Blob Storage|File Storage| 
----|--------|---|--|--|--|
Services Supported| Blob, File, Queue, Table|Blob, File, Queue, Table|Blob (`Block Blobs and Append Blobs Only`)| Blob (`Block Blobs and Append Blobs Only`)| File Only
Unmanaged Disk (Page Blob)| Yes| Yes| No| No| No
Supported Perfoamance Tiers| Standard, Premium|Standard, Premium| Standard| Premium | Premium
Supported Access Tiers| Hot, Cool, Archive| NA|Hot, Cool, Archive| NA|NA
Replication Options| LRS, ZRS, GRS, RA-GRS, GZRS, RA-GZRS| LRS, GRS, RA-GRS| LRS, RDS, RA-GRS| LRS, ZRS| LRS, ZRS
* LRS (Local Redundant)
    * Replicated three times in one datacentre
    * Protects again server and rack failure
    * write request to storage happens syncronosly
* ZRS (Zone Redundant)
    * Replicates data across `three availability zones in a region` and replicates it three times in each datacentre
    * The data commit comes back only after all the data has been `synchronosly` across all availability zones
    * The Archive tier for Blob Storage isn't currently supported for ZRS, GZRS, or RA-GZRS
* GRS (Geo-Redundant Storage)
    * Copies you data `three times in a single datacentre` and `asynchronosly to a single datacentre with three copies in the paired region`
* GZRS (Zone Redundant Geo-Replicated)
    * Copies your data using ZRS in the primary region
    * Async copy to a `single datacentre` in the secondary region
    * provides `three copies in the single datacentre in the secondary region`
* RA-GRS (Read-access geo-redundant storage)
    * Primary region has three copies on the data in one AZ
    * Secondary region has three copies on the data in one AZ
    * Read-Only Access to data replicated data in the secondary region
* RA-GZRS (read-access geo-zone-redundant storage)
    * Read-Only Access to data replicated to a all AZ's in the secondary region
### Replication RPO
* Geo-Replication is async replication. The expected maxium RPO is less than 15 minutes, but there is no SLA
## Access Tiers
* Hot
    * Access tier is used to store `frequently accessed` data
* Cool
    * storage large amounts of data
    * data is not accessed for at least `30 days`
    * SLA lower than the hot tier
    * Storage costs are lower than hot tier
    * Access costs are higher than the hot tier
* Archive
    * Accessed rarely
    * several hours of retreival latency
    * data should remain in the archive tier for at east `180 days`
    * cheapest storage cost
    * most expensive access cost
    * You have to set the archive tier at the `blob level`
### Shared Access Signature
* A SAS token is a way to granularly control how a client can access data in an azure storage account
#### Types of shared access signatures
* Account SAS
    * delegate access to resources in one or more storage services (blob, tablem queue and file)
    * you can specify IP whitelists
    * specify HTTP protocol HTTP/HTTPS
    * The account sas URI consists of:
        * URI to the resource
        * SAS Token
    * SAS Token consists of:
        * api-version
        * SignedVersion (sv) - Specifies the signed storage version
        * SignedServices (ss) - Specifies the signed services that are accessible
            * Blob (b)
            * Table (t)
            * Queue (q)
            * File (f)
        * SignedResourceTypes (str) - Specifies the signed resource types that are accessible
            * Service (s): Access to `service level` APIs
            * Container (c): Access to container level APIs such as create/delete container, Table, Queue, list Blobs
            * Object (o): Access to object-level APIs such as blobs, queue messages, table entries, files
        * SignedPermissions (sp) - specifies the signed permissions
            * Read (r): Read permissions for all resource types (service, container, objects)
            * Write (w): Permits write access for specified resource types. Allows a user to create and update
            * Delete (d): Permits delete for container and Object not queue
            * List (l): Valid for `Service and Container resources only`
            * Add (a): Valid for the following Object resource types only: queue messages, table entities, and append blobs
            * Create (c): Valid for Container resource types and the following Object resource types: blobs and files
            * Update (u): Valid for the following Object resource types only: queue messages and table entities
            * Process (p): Valid for the following Object resource type only: queue messages
            * Tag (t): Valid for the following Object resource type only: blobs. Permits blob tag operations
            * Filter (f): Valid for the following Object resource type only: blob. Permits filtering by blob tag
            * Set Immutability Policy (i): Valid for the following Object resource type only: blob. Permits set/delete immutability policy and legal hold on a blob
        * SignedStart (st) - Time SAS Token is valid from
        * SignedExpiry (se) - Time tocken become invalid
        * SignedIP (sip) - IP range the token can come from
        * SignedProtocol (spr) - details if communication can be http, https or http/https
        * Signed EncryptionScope (ses) - EncryptionScope
        * Signature (sig) - Signature
* Authorising Access to Blobs using Azure AD
    * Using Azure AD is only supported on `blob and queue` storage
    * If an application is running from an azure resource you can use a managed identity
    * To use Azure AD to authorise the access to blob storage the service needs to return an oData token
    * There are two client libraries to acquire a token:
        * Azure Identity Client Library
        * MS Authentication Library (MSAL)
    * You can assign Azure RBAC permissions at two levels;
        * Storage Account level
        * Container
        * Queue
    * The RBAC roles you can use to access the data component are:
        * Storage Blob Data Owner
        * Storage Blob Data Contributor
        * Storage Blob Data Reader
        * Storage Queue Data Contributor
        * Storage Queue Data Reader
        * Storage Queue Data Message Processor
        * Storage Queue Data Message Sender
### Stored Access Policy
* You can create a stored access policy only on the `container`
    * You can specify:
        * Identifier
        * Permissions (list, Read, Add, Create, Write, Delete)
    * Start and End Time
* You can have a max of `5` access policies
* You refernce the access policy when creating a SAS token using cli or storage explorer
* To revoke the sas keys linked to the Storaged Access Policy you would delete the policy
### Access keys
* With an access key to a storage account you have full rights over data in all services
* As there is two access keys you can roll the keys without issues
* rolling a storage account access key will `invalidate any sas tokens that were generated using that key`
### Azure Files
* Standard file shares support LRS, ZRS, GRS and GZRS
* Premium shares support LRS and ZRS
### Configuring Access to Files
* Azure files allows two types of identity based authentication to access shares:
    * On-Prem AD DS
    * Azure AD DS
    * Azure AD Kerberos for hybrid user identities
* Azure files uses `kerberos` tokens to authenticate users
### On-prem AD DS authentication and authirisation to Azure Files
* You need to sync identites from on-prem to Azure AD with AD connect
* To register the Azure Storage account with your on-prem AD DS. There are two ways to do this:
    * Run the AzFilesHybrid powershell module
    * Manually
#### Share level Permissions
* Share-level permissions on Azure file shares are configured for Azure Active Directory (Azure AD) users, groups, or service principals
* directory and file-level permissions are enforced using Windows access control lists (ACLs)
* There are a number of Azure AD Built in roles:
    * Storage File Data SMB Share Reader - RO over SMB Shares
    * Storage File Data SMB Share Contributor - RO/RW and delete over Azure Storage File Shares over SMB
    * Storage File Data SMB Share Elevated Contributor - RO/RW/Delete and modify NTFS over Azure Storage File Shares over SMB
* T apply superuser ACLs on the directories and files you need to mount a drive using your `storage accounts key` from a domain joined machine
```
net use <drive letter>:\\storage-account.file.core.windows.net\fileshare /user:azure\<storage account name> <storage account key>
```
* You can use file explorer or icacls to grant permissions to all directories and fies under the file share
---
## Skill 2.2 manage storge

If you dataset is large you can sip your data and import it into azure using the `Azure Import/Export` service.
* Azure import/export service is only used with blob storage and aure files
### Azure Export
* An Azure Export Job is a job that allows yout o ship a large volume of data from Azure Storage to your on-prem environment by shipping hard disks
* Only supports export of blobs
### Azure Import Job
* You can crete an import job and ship data on disk to Azure
* all storage accounts are supported
* Azure files is only supported performing an `import job`
1. The first step is to install the Azure Import/Export tool called `WAImportExport`
* There are two vrsions of this tool:
    * WAImportExporttool v1 - Azure Blob
    * WAImportExporttool v2 - Azure Files
* There are some pre-requisites for using the WAImportExport Tool
    * Win 7 or Win 2008 upward
    * .Net 4 upward
    * Storage Account Key
    * Bitlocker `must be enabled on the machine you are running the tool on`
2. Run the drive preperation tool as below:
```
waimportExpot.exe prepimport /j:<journalFile> /id:<session ID> /logdir:<LogDir> /sk:<destinantion storage account key> /InitialDriveSet: <driveset> /DataSet:<DataSet>
```
3. Go to the destinantion Storage Account
    * Create an Import Job within Azure and select `Import into Azure`
    * Job Details - Choose the journal file created with WAImportExport Tool
    * Select Destinantio Storage
    * Specify the return address
### Install and Use Storage Explorer
* Storage Explorer supports:
    * Blob
    * Tables
    * Queue
    * Files
    * CosmosDB
    * Data Lake
* Storage explorers authentication mechanisms:
    * Add an Azure Account
    * Connection String - Can get this from `Access Keys`
    * Use a Shared Access Signature
    * A storage Account Name and Key - can get this from `Access keys`
    * Attach a local emulator
### AzCopy
* AzCopy is the cli version of storage explorer
* You need to authenticate before copying any data. You can do this by using the ` azcopy login ` command
* Supported logins are:
    * service principle
    * SAS token
    * access key
    * managed Identity
* AzCopy can also be used to copy between storage accounts
```
azcopy copy "https://examref.blob.core.windows.net/ srccontainer/[blob-path]?<sas token>" "https://examrefdest.blob.core.windows.net/destcontainer/[blob-path]?<sas token>"
```
* AzCopy v10 is multiplatform it supports windows, linux MacOS
* You can use AzCopy to sync between two containers
```
azcopy sync "https://examref.blob.core.windows.net/ srccontainer/[blob-path]?<sas token>" "https://examrefdest.blob.core.windows.net/destcontainer/[blob-path]?<sas token>"
```
### Storage Account Replication
* You can move a storage account between LRS, GRS and RA-GRS freely
* To move to and from ZRS, GZRS and RS-GZRS you are better to:
    * AzCopy 
    * Raise a support request to live migrate data through a request to MS Support
### Blob Object Replication
* You can configure blob replication between two storage accounts. A pre-requisite is to enable:
    * must be general-purpose v2 or premium block 
    * version control on both storage accounts
    * `blob change feed` on the `source`
* The replication is `async`
* you can place `prefix filters` on the replication rule to select certain folders and files
* You can filter to:
    * replicate everything
    * only new objects
    * custom - objects created from a certain time
* To see the blob replication status you can go to the source blob and go to properties
* The source account can have a `maxium of two destination accounts`
* The destination account is `Read-Only`
---
## Skill 2.3 Configure Azure Files and Blob Storage
* Azure Files is a service that supports SMB shares
* Max default share size is 5TiB
* Max default share size with large file support is 100 TiB
* Max premium share size is 100 TiB
* Azure files supports SMB 3.0
### Create and Configure File Sync
Azure File Sync is a service that allows you to cache several Azure file shares on an on-premises Windows Server or cloud VM
Some of the functions of Azure File Sync are:
* Multi-site - Ability to write files across windows and azure files
* Cloud Triering
* Azure Backup Intergration
* Fast DR - Restore file metadata and recall immediately
* zure file shares can be used in two ways: by directly mounting these serverless Azure file shares (SMB) or by caching Azure file shares on-premises using Azure File Sync
#### Azure Sync Management Components
* **Azure File Share**: provides a `cloud endpoint`
* **Server Endpoint**: The path on the Windows Server that is being synced to an Azure file share. This can be a specific folder on a volume or the root of the volume. Multiple server endpoints can exist on the same volume if their namespaces do not overlap
* **Sync Group**: The object that defines the sync relationship between a cloud endpoint, or Azure file share, and a server endpoint
##### Azure Sync Deployment Steps
* Pre-requisites for Azure Sync Deployment:
    * Azure Files created with storage account
    * Server with the Azure File Sync agent
1. Create a Azure File Sync Resource
    * Choose the name and resource group
2. Register a server
3. Create Azure Sync Group
    * This configures:
        * Sync Group Name
        * 1st cloud endpoint
        * storage account
        * Azure File Share
### Configure Blob Storage
* Blob Types:
    * Page Blobs:
        * Optimised for random IO and VHD files
    * Block Blobs
        * optimised for `efficient uploads and downloads`for video, images and general files
        * Append Blobs
            * like block blobs but are optimised for append operations. Good for logs
* Once you have set the type of blob you `cannot change it`. If you uploaded a .vhd as a block blob you have to delete it and uplad as a page blob
* when uploading a file to blob storage you can select:
    * blob type from block, page and append
    * block size 4MiB to 100MiB
    * Access Tier: Hot, cool, archive
    * Encryption Scope
    * Retention Policy: You need to enable `version imunity at the container level`
#### Soft Delete
* Soft Delete allows you to save and recover your data when `blobs` or `blob snapshots` are deleted
* feature has to enabled on the `storage account` and a `retention period must be set for how long the deleted data is retained`
* Maximum retention is `365 days`
#### Storage tiers
* three tiers are supported
     * Hot
     * cool
     * archive
* Data in the archive tier is stored offline and must be rehydrated to a hot or cool tier. This can take up to `15 hours`
* changing the acces tier can change at the `account`
### Monitoring /Log Analytics
* you can configure the storage account to send logs to:
    * log analytics (azure Monitor)
    * storage account
    * Azure event Hub
    * partner solution
### Lifecycle Manager
* move blobs between tiers using:
    * date accessed
    * created
* you can move blobs from hot to cool to archive or delete
# Chapter 3 - Deploy and Manage Compute Resources
## ARM Template Overview
* ARM Templates are JSON files.
The basic structure of the JSON Template is:
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "functions": [],
    "resources": [],
    "outputs": {}
}
```
* Schema
    * For resource group target deployments use `/deploymentTemplate.json#`
    * for subscription target deployments use `/subscriptionDeploymentTemplate.json#`
* ContentVersion
    * This is your version control
    * You can reference this version in other templates to ensure you are deploying from the correct version of template
* parameters
    * Parameters passd at runtime
    * you can also use the file `azuredeploy.parameters.json` to pass parameters
* Variables
    * defines values used in the template
    * variables can be hard-coded or dynamically generated
* functions
    * Can be used to create functions that are used in the template
* resources
    * contains the resources that are created or updated
    * you can define conditions on when to create a resource
    * dependsOn is used to determine hierarchy and flow during resource creation
* outputs
    * define data outputs that are returned as part of the deployment
### ARM Deployment Modes
* There is two deployment modes:
    * Incremental - This updtes any changes but does not delete any resorces that have been manually created
    * Complete - This deletes resources that exist in the resource group that are not in the template
* to select the deployment mode you use the `-deploymentmode` parameter in the new-azresourceGroupDeployment cmdlet
#### Deploy from Template
* There are three ways to deploy from template:
    * Portal
    * CLI
    * REST API
* Portal
    1. Create a resource
    2. Select `template deployment`
    * You can choose to deploy from a common template, samples or upload a template
* CLI
    * with powershell you would use the `new-azResourceGroupDeployment` cmdlet
    ```powershell
    new-azResourceGroupDeployment -TemplateParametersObject or -TemplateParametersFile or -TemplateParametersUri https://*.json
    ```
* You can export a configuration from existing resources. This can be used to redeploy is analyse how the resource was deployed

## VM Extensions
* To use the custom script extension your script has to be available through a URI. Be it a storage account or GitHub. 
* The URI has to allow anonymous access to pass a SAS key
* extension v1.10 and later supports `managed identities` for downloading friles from URLs. You cannot use a managed identity with a `storage account` or `storageAccountKey`
* The custom script runs under the `LocalSystem` account
### Troubleshoot VM Extension Failures
* You can find the logs in windows `c:\windowsAzure\Logs\Plugins\Microsoft.Compute`
* Logs for Linux are located `/var/libwaagent/Microsoft.OSTCExtensions.CustomScriptForLinux-<version>/download/1`
## Skill 3.2 Configure VMs for HA and Scalability
### Availability Zones
* There is a minimum of three AZ's per region (where applicable)
* using AZ's gives a `99.99` uptime SLA
* When you create VMs in three AZ's, those will automatically be distributed across `fault domains`
* Availability Zones are deivided into:
    * Zonal Services - A resource can be deployed to a specific (self selected) AZ to achieve more `strigent latency`. Resilience is `self-architected` by replicating apps and data to one or more zones in the region. Example: VM, App services
    * Zone-redundant - Resources are replicated to distributed `across zones automatically`. Example: Azure SQL, ZRS Storage Account
    * Always-Available Services - Always available across all Azure geographies and are resilient to zone-wide outages and region-wide outages. Example: Azure AD
### Availability Set
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

#### Create Availability Set
`It's recommended to use VM Scale Sets with flexible orchestration mode for high availability. Virtual machine scale sets allow VM instances to be centrally managed, configured, and updated, and will automatically increase or decrease the number of VM instances in response to demand or a defined schedule. Availability sets only offer high availability.`
* There are two ways to create an availability Set:
    1. While creating a VM select to create an availability set from the `basics` tab
    2. Create a resource called `availability sets`
### Deploy and configure VM Scale Sets VMSS
#### Scale Set Orchestration
* Scale set orchestration modes allo yu to have greater control over how VMs are managed by a scale set
* The orchestration mode is defined when you create the scale set and `cannot be changed or updated later`
* Uniform Orchestration (legacy)
    * Optimised for `large-scale stateless workloads` where the instances are identical
    * VM Instances are deployed from a `profile` or `template`
    * have to be the same type of VM, SKU, OS
    * Access to VM Instance API is limited to a subset of APIs mostly throught he scale set
    * Uniform provides `fault domain` HA guarentees when configured with `fewer than 100 instances`
    * Supports VMSS management, orchestration, monitoring, scaling and OS upgrades
    * supports spot instances
* Flexible (new)
    * Best used for `quorum based`, `open-source databases`, `stateful apps` workloads
    * During VMSS Flex creation you don't have to create the VMSS with a profile. You have to create the VM and then `add` the VM into the scale set manually.
    * You have full access to the per instance VM IaaS API instead of the restricted API. This means you can use Azure Backup, ASR for the instances
    * Most SKUs are supported. Speciality SKUs are not supported
    * Can only have `1 profile per VMSS Flex` instance
    * can mix different VMs, Spot, OS, SKUs into the VMSS Flex
    * Up to `1000` VMs in VMSS Flex
    * `scale-in` is `not` supported in flexible mode
    * Upgrade policy is not supoorted in flexible mode
#### VMSS Scaling Profile
* VMSS allows you to define scaling profile or template, which specifies the properties of the VM Instance which include.
    * VM Image
    * Admin credentials
    * Network Interface settings
    * Load Balancer Back-end Pool
    * OS and Data disk settings
* When you increase the instance count it will create VMs based on the `profile`
* Scale Sets with a profile are `uniform`
#### VMSS Scaling
* Initial Instance count
    * Scaling Policy
        * Manual
        * custom
            * Min & Max instance count
            * Scale Out
                * CPU Treshold
                * Duration
                * Increase instance count by number
            * Scale In
                * CPU Treshold
                * Duration
                * Increase instance count by number
        * Scale in Policy
            * Default - Balance across AZs and fault domains
            * Newest VM
            * Oldest VM
##### VMSS Management
* Upgrade Policy
      * Manual
      * Automatic - Will randomly be upgraded
      * Rolling - Upgrades roll out in batches with optional pause
        * set batch size
        * Set pause time betwen batches
        * enable cross zone upgrade
* Enable Overprovisioning
    * This is used to ensure a higher success rate when scaling out
* Guest OS updates
* Instance termination notification
    * Enable Instance termination notice when scaling in. Min `5 min` & max `15 min`
    * Notifications through Azure Metadata Service
##### VMSS Health
* Health probe
* Auto repair policy
    * This deletes unhealth instances if detected
##### VMSS Advanced
* Force strictly even balance across zones
    * This forces the scale out and in to make sure all VMs are evenly scales across zones. If it can't it will fail
* spreading algorithm
    *  Max spreading
        * the scale set spreads your VMs across as many fault domains as possible within each zone. This spreading could be across greater or fewer than five fault domains per zone
    * Static Fixed Spreading
        * With static fixed spreading, the scale set spreads your VMs across exactly five fault domains per zone. If the scale set cannot find five distinct fault domains per zone to satisfy the allocation request, the request fails
### Azure Compute Gallery - NEW

* Allows you to create VM images and then publish those into a gallery
* Structure of Gallery is:
    * Azure Compute Gallery > VM Image Definition > VM Version
    * You can have multiple versions within a VM Image Definition
* VM Image Version is replicated to one or move regions
* You need to you RBAC to control access to images and updating of images
* You can create a VM Image or Application Image
* To capture a VM Image you can use:
    * Capture from VM in portal
    * create an image from a snapshot
    * Upload a vhd
## Skill 3.3 Configure VMs
When creating a VM there are several options to choose from sych as:
* Basics
    * Subscription
    * Resource Group
    * VM Name
    * Region
    * Availability options:
        * VM Scale Set
        * Availability Zone
        * Availability Set
    * Security Type
        * Standard
        * Trusted launch VM (Virtual Trusted Execution - recommended)
        * Confidential VM (Hardware Trusted Execution)
* VM architecture
    * x64
    * arm64
* Run spot - used for jobs that can be restarted and low priority jobs
* Size
* username
* Password
* Public Inbound Ports
    * None
    * selected ports
* Disks
    * OS Disk
        * Disk Type
            * premium SSD LRS
            * Standard SSD LRS
            * Standard HDD LRS
            * Premium SSD ZRS
            * Standard SSD ZRS
    * Key Management
        * Platform Managed
        * Customer Managed
        * Platform and Customer Managed Keys
    * Ultra Disk Compatible - Used for very high IO
* Networking
    * vNet
    * Subnet
    * Public IP
    * NIC NSG
    * Accelarated networking - low latency on the NIC
    * Load Balancing
* Management
    * Enable systems assigned managed identity
    * Azure AD - Login with Azure AD
    * Auto Shutdown
    * Backup
    * Guest OS Updates:
        * hotpatch
        * Patch orchestration options:
            * azure orchestrated (managed patching across availability sets)
            * windows update
            * Manual
            * Image default
        * Reboot setting:
            * always
            * if required
            * never
* Monitoing
    * Alerts - enable common alerts for a VM
    * Boot diag
    * Enable OS Guest diagnostics - save metrics of VM to storage account
* Advanced
    * extension
    * application to install
    * custom data:
        * you can pass data into the VM to be used for installing
    * User Data
        * Pass a script, configuration file, or other data that will be accessible to your applications throughout the lifetime of the virtual machine
    * NVMe
        * higher IOPS and throughput
        * Gen v2, v3 and v4 supports SCSI only. Gen v5 supports SCSI and NVMe
        * The NVMe enabled `Ebsv5` series is designed to offer the highest Azure `managed disk` storage performance while the L series VMs are designed to offer higher IOPS and throughout on the local NVMe disks which are `ephemeral`
    * Dedicated Host
    * capacity Reservaions - reserve a VM SKU ahead of time to ensure you get the capacity
    * Proximity placement group - place VMs as lose together in the same `region`
* Tags
* Review
#### Enable Encryption on VM Diks
There are various ways to encrypt disks such as:
* Azure Disk Server-Side Encryption
    * Data at rest encrypted
    * It doesn't encrypt temp or disk caches
* Encryption at host
    * Enchances Azure Server Side Encryption and encypts temp and cache disks at rest
* Azure Disk Encrypton (within the OS)
    * You can enable encyption on either:
        * OS Disk
        * OS & Data Disks
    * To bring your own keys you need to deploy key vault `premium`, and configure it to allow `Azure Disk Encryption`
    * Disk encryption for Windows uses bitlocker and Linux it uses DM-Crypt
* Confidential Disk Encryption
    * Binds disk encryption keys to the virtual machines TPM and makes the protected disk content accessible only to the VM
#### VM Networking
* accelarated networking is SR-IOV. This bypasses the hyper-v vSwitch
* SR-IOV is only supported on select VM SKUs such as general D servies with more than 2 x vCPUs
* Only suported on 2012 R2 and up
* If there are more than one vNICs on a VM the first one is designated the `primary` NIC
* You can control which network interface you send outbound traffic to. However, a VM by default sends all outbound traffic to the IP address that's assigned to the primary IP configuration of the primary network interface
#### Redeploy VM
* When you redploy a VM this stops it and allocates it to anoter Hyper-v host
```powershell
set-azvm -redploy -ResourceGroupName "rg" -Name "testvm"
```
```azcli
az vm reploy --resource-group rg --name testvm
```
### Azure Bastion - NEW
* Bastion server has to be in the same region as the destination VMs
* The bastion service will create a VMSS and will want to created a dedicated vNet and subnet. The subnet `must` be named `AzureBasionSubnet` and it must be a `/26`
* You can peer the Bastion vNet with other subnets to access the VMs

# Skill 5.2 Monitor and Azure Backups
## Azure Monitor
* Three are three types of monitoring elements:
    * Metrics
    * Logs
* By default metrics sampled at 1 minutes intervals and are are kept for `93` days
* Retention is 14 days
* App Insights retention is 90 days
### Azure Monitoring Agent
* All legacy agents are getting consolidated into one age called the Azure Monitoring Agent AMA
* The agent being replaced are:
    * Log Analytics Agent OMS
    * Telegraf Agent - Linux 
    * Diagnostic Extension
        * Sends data to Monitor Metrics (windows only), Event Hubs and Azure Storage
* To install the AMA you can:
    * Deploy through extensions in portal. You need `VM Contributor`
    * Deploy through ARM `Log Analytics Contributor`
    * Deploy the agent manually
* The new AMA agent allows for more granular data collection using `data collection rules`
* Supports data uploads to multiple locations
### Log Analytics (Azure Log Monitor)
* By default Log Analytics will include 5GB of data storage per month
* You can increase the storage limit by purcasing per-GB pricing
* To onboard VMs to push their logs to Log Analytics you have to install the `azure log analytics agent (OMS)`
* The OMS agent must have 443 access to the below domains:
    * *.ods.opinsights.azure.com
    * *.oms.opinsights.azure.com
    * blob.core.windows.net
    * *.azure-automation.net
 ### Azure Activity Logs
 * Activity logs are good to see what actions occur within your environment against the ARM APIs. You will be able to see resource creations, updates, deployments and deletions, but not when traffic is blocked by a rule. This is what diagnostic logs are for. 
* events in the activity log are kept for `90 days`
### Resource Diagnostic Logs
* Diag logs are used to increase Logging to give more advanced analysis
* Diag logs have to m set manually and can be sent to:
    * storage account
    * event Hub
    * Log Analytics
    * partner solution
* Not all resource can have a retention set in diagnostic logs. A retention value of 0 days means infinite. Retention in days is `only applicable if you select storage account`
### Azure Monitor Alerts
* when creating an alert rule you have to:
    * Target the scope (This defines the signals to define the condition)
    * condition defines the signals
        * Metrics
        * Log Analytics KQL Query
        * Activity Log
        * Resource Health
    * Action Groups (what do you do)
        * Notifications
            * Email Azure Role
            * Email/SMS/Push/Voice
        * actions
            * runbooks
            * logic apps
            * ITSM
            * webhook (secure)
            * Function (Requires v2)
            * Event Hub
* Alerts can have one of three user states
    * New
    * Acknowledged
    * Closed
* Alerts can have 1 of 2 states
    * Fired
    * resolved
## Azure Backups

Azure Backup backups up:
* VMs
* SAP Hana Databases running in an Azure VM
* Azure File Shares
* Azure SQL
* SQL running in Azure VMs

To enable Azure Backups you need to create a recovery services vault. This vault must be in the `same region as the VMs you are backing up`.
* There are two key components to Azure Backup
    * Recovery Service Vault
    * Backup Policy
* backup policy defines the Backup:
    * schedule
    * Retention
        * Daily
        * Weekly
        * Monthly
        * Yearly
* With the recovery service Vault. There are a number of additonal options you can enable. such as:
    * Backup Config: Storage Replication
        * GRS (Default)
        * ZRS
        * LRS
    * Encryption
        * By Default data at rest is encrypted by Microsoft Managed Keys
        * Can use your own Keys (Integration with Key Vault)
        * Can enable encryption on the backup storage Infra
    * Multi-User Auhorisation
        * Used to ensure critical tasks on the recovery serice or backup vaults have an additional layer of protection
    *  Security Settings
        * Soft delete
        * Enable soft delete for on-prem systems
        * soft delete retention period (14 to 180)
        * Enable Always-on soft delete
    * Cross Subscription Restore
    * Security PIN
### Restore Azure Backups
* You can restore VMs or files
* when restoing a VM you can create new or replace existing
* If restoring a VM you have to create a dedicated storage account. This storage acoount must:
    * Non ZRS
    * Doesn't display premium storage accounts
    * Not attached to any affinity group
    * Network restricted filtered out
* File restore has some limitations
    * Must be less than 10GB of files
    * bandwidth is imited to 1 GB per hour
    * You have to map a drive to the snapshot
    * Can't run the script on:
        * server with dynamic disks
        * storage spaces
        * VM has large disks > 4TB
    * `It's recommended to have a separate VM only for file recovery (Azure VM D2v3 VMs)` and then shut it down when not required.
    * if you have encrypted disks you must you need to allow the backup service access to key vault











