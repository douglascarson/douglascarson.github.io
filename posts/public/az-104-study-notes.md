# AZ-104 Study Notes
## Describe AzureAD

## Skill 1.1 Manage Azure identities and governance
* there are three types of objects in Azure AD
    * Users, Groups, Devices
* There are two types if User Objects
    * Cloud Only
    * Synchronised
* To create cloud only users within AzureAD you must have `Global Administator` or User `Administrator roles`
* AzureAD groups can contain Users, Groups, Devices and `Service Principles`
* When creating AzureAD groups there are two types:
    * Security
    * Office 365
        * An 0365 group is to give access to a shared mailbox, calendar or sharepoint site
* when using dynamic groups you can populate the group with users or devices but not both
* You can transition from a dynamic group to an `assigned group` and back