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

        