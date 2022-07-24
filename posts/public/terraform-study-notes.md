# 1. Terraform IaC Concepts
## 1.a What is IaC
* IaC is the practise of defining and provisioning infrastructure resources through a machine readable format and be provisoned in an automated fashion
* The format can be **declaritive** (defining the desired outcome) or **imperative** (defining the process of provisioning)
* Terraform is a declaritive language and is focused on the desired end state. It requires an interpretation engine to create and configure the actual resources.
* Imperative solutions such as bash scripts focus on how to actually provision resources
## 1.b Describe Advantages of IaC Patterns
The advantages of IaC can be broken down into a few categories:
* consistency
* repeatability
* efficiency
---
# 2. Terraforms purpose (vs Other IaC)
## 2.A Explain multi cloud a*nd provider agnostic benefits
* Terraform is agnostic of the propriety cloud IaC solutions.
* It leverages providers for each different cloud as well as on-prem solutions like VMware
* Terraform is able to provide a common tool, process and language (HCL) to be used across multiple clouds and services
## 2.B Explain the benefits of State
* The creation and manipulation of resources is tracked by the implementation of a state file.
* terraform state enables the mapping of real world instances to resources in a configuration, improved performance of the planning engine, and collaboration of teams through state file sharing
* Terraform is Idempotent. This is where you can run the same code over and over and get the same result.
### Dependencies
* Terraform created a **graph** that maintains a list of dependencies in the state file to that it can properly track and deal with depencency relationships
### Performance
* To improve performance terraform will refresh the state file when you run `terraform plan`
* terraform can be told to skip the refresh by using the command `terraform plan -refresh=false`
* terraform can also only refresh certain resources instead of the whole state file by using the command `terraform plan -target=`
*This is used in exceptional circumstances*
---
# 3. Understand Terraform Basics
## 3A Terraform and provider Installation and Versioning
* The installation process for terraform is to download it and add it to your environmental variables
## Providers
* The resources and data sources are all enabled by plug-ins in terraform called providers. 
* A prpvider is an executable that contains the code necessary to interact with the API of the service it was written for.
* providers can be explicitly defined in the code or implicitly by the resources that are provisioned or data sources that are used
* The recommended best practise is to use the `terraform_providers block`
``` javascript 
terraform {
    required_providers {
        azurerm = {
            source = "hashicorp/azurerm"
            version = "~>4.22"
        }
    }
}
```
* The `required_providers` block sets the version for all instances of the provider, including child modules
* There are multiple arguments for specifying the version number such as:
```
>= 4.22.0 is greater or equal to the version
<= 4.22.0 is less than or equal to the version
~> 4.22.0 any version on the 4.22.x range. Keeps you on the major range
>= 1.22, <= 4.22 is any version between 1.22 and 4.22 inclusive 
```
* terraform 0.14 introduces a file called `terraform.lock.hcl`. This records the versions of providers and can be checked into the VCS.
This allows a consistent provider version used across the team.
You can upgrade the provider version (within the version constraints defined in the `required_providers` block) by using the command `terraform init -upgrade`
## 3B Describe plugin based architecture
* terraform is provided as a single binary. This includes the core components to parse and deploy terraform configurations.
The interactions with providers is through plug-ins
* each plug-in is executed as a seperate process communicating with the terraform binary
## 3C Using Multiple Providers
### Using multiple different providers
* when using multipe providers you can explicitly define this is the `required_providers` block or implied by creating resources that use multiple providers

### Multiple instances of a provider
* If you want to create resource in multiple regions then you have to define multiple instances of the provider. To accomplish this you have to use the `alias` meta-argument. This enables you to reference that provider in the resource declerations
``` javascript 
terraform {
    provider "aws" {
        region = "us-east-1"
    }
    provider "aws" {
        region = us-west-2
        alias  = "west"
    }
resource aws_ec2_instance "ec2-east-1" {
    name = "instance2"
}
resource aws_ec2_instance "ec2-west-1" {
    name = "instance2"
    provider = aws.west
}
}
```
* To ensure resources are deployed into a certain region you can use the provider = in the resource block to specify the region
## 3D Describe how terraform finds and fetches providers
* As of terraform 0.13 terraform can automatically download providers from either a **public** or **private** repo
* 0.13 introduces three types of providers:
    * official
    * verified
    * community
* Official are owned and maintained by Hashicorp
* Verified are owned and maintained by other vendors but have gone through the partner provider process
* community and published and maintained by individuals in the community. They are not officially supported

The required providers block is defined in the terraform block
``` javascript 
// SYNTAX
terraform {
    required_providers {
        <LOCAL NAME> {
            <SOURCE> = <hostname/organisation/provider>
            #If hostname is omitted then terraform assumes public repo
            <VERSION>
        }
    }
}
// Example
terraform {
    required_providers {
        aws = {
            source  = "hashicorp/aws"
            version = "~> 4.22"
        }
        azure = {
            source  = "hashicorp/azurerm"
            version = "~3.0"
        }
    }
}

```
* By default terraform will download the plugins to the `.terraform` directory in the same directory as the local configuration
* To improve performance you can set a plugins cache directory. terraform will look in this directory first before downloading the binaries.
You can set it by one of two ways:
    * use the `plugin_cache_dir` cli argument
    * Set the `TF_PLUGIN_CACHE_DIR` environmental variable
## 3E when to use or not use provisioners and when to use or not use local-exec or remote-exec

* To allow terraform to run a script locally (on the same machine running terraform) or on a remote machine **provisioners** are used.
Hashicorp recommend using provisioners as a **last resort**
* The remote-exec provisioner allows you to connect to a remote machine using WinRM or SSH and run a script.
The recommended way to do this would be to use your cloud providers way to run a script on a VM. For AWS this would be `user_data` and Azure `custom_data`
* local-exec allows you to run a script on the machine running terraform
* A provisioner can only be triggerd with a resource is **created** or **destroyed**. It **does not work** for an altered resource.
* If a provisoner fails the resource is set as <u>tainted</u> as terraform has no idea how to resolve the issue
* If a resource has multiple provisioners defined it will run them in order
---
# 4 Use Terraform CLI (Outside of core Workflow)
## 4A Understand the help command
* terraform has a built in 
help system. You can get access to it by using `terrafrom` or `terraform <command> -help`
## 4B Choose when to use terraform fmt to format code
* the `terraform fmt` command is used to format the configuration files to match the canonical format and style outlined in Terraform language style conventions
* the `terraform fmt` command looks for .tf and .tfvars files
* You can use the `-recursive` command to traverse a directory structure looking for .tf and .tfvars files.
## 4C When to use terraform taint
* If you use `terraform taint` it will taint a resource to be recreated at the next apply
* terraform taint only marks the resource for recreation in the state file
```
SYNTAX
terraform taint <RESOURCE ID>
EXAMPLE
terraform taint aws_iam_user.name.joe
terraform taint 'module.iam.aws_iam_user.iam["neo"]'
                <module_name>.<resource_provider><resource_name><instance>
```
To remove a resource from being tained you can use the command `terraform untaint <path to resource>`
## 4D choose when to use terraform import
* terraform import is used to import resources that were created outside of terraform into the terraform state file
* terraform does not generate the configuration for an imported resource. It simply adds it to the state file. **You have to write the configuration that matches the existing resources configuration before importing.**
```
SYNTAX
terraform import <state file resource location><Instance> <cloud ID>
EXAMPLE
terraform import 'module.iam.aws_iam_user.iam["boo"]' boo
terraform import aws_iam_user.users boo
```
* you need to do this for all elements of a resource. So if you were importing a network then you would have to do the vNet and then subnets.
You basically have to build the data structure up manually
## 4E choose when to use terraform workspace to create workspaces
* terraform workspaces for terraform open-source is simply seperated state files and configuration
* etcd backend **does not support workspaces**
* local and amazon S3 supports workspaces
The support commands for `terraform workspace` are:
```
terraform workspace
* new
* list
* show
* select
* delete
```
* you cannot delete the workspace you are in unless you use the -force
* you cannot delete the default workspace
## 4F choose when to use terraform state
* the terraform state command has a few sub commands:
```
* list - List resources in the state file
* show - show the resources in the state file
* mv - move and item in the state file
* pull - pull current state and output to stdout
* push - update remote state from local state file
* rm - remove item from state
```
### terraform state mv
* the `terraform mv` command has a few options:
```
dry-run - like a what-if
state - This is the path to the source state file
state-out - This is the path to the destination file. 
```
* By using the state-out command you can move, modules, resources or single instance of a resource to a new state file
### terraform state pull
* The primary use for this command is to read the contents of a remote back-end state file. If it were local you could just cat the state file.
* terraform pull is used to manually download and output the state file from a remote back-end. (It's like having a raw read-only view of the state file)
### terraform state rm
* the terraform state rm command is used to remove a resouce from terraform control. It **will not** delete the resource.
## 4G when to enable verbose logging
* terraform can be set to generate detailed logs. This is set by the environmental variable `TF_LOG`
* There are 5 levels of logging:
    * TRACE
    * DEBUG
    * INFO
    * WARN
    * ERROR
* To set the log path use the environmental variable `TF_LOG_PATH` with the full path as it's value
* as of 0.15 you can set the logs for either terraform core or provier plugins using the enviromental variables `TF_LOG_CORE` or `TF_LOG_PROVIDER`
* if terraform core crashes it will create a crash.log file
---
 # 5 Interact with terraform modules
 * the main configuartion you are working on is the *root moule*
 * modules can call other modules
 * there is a hierarchial structure modules where the root module calls the child module which in turn can invoke another child module
 * the terminology is:
    * the module invoking the module code block is the *calling module*
    * the module being called is the *child module*
* modules are invoked by using the `module <name_label> {}` configuration
* To find the module terraform uses the source argument in the module block. The locations can be:
    * local path
    * terraform registry
    * Github
    * Bitbucket
    * Generic Git, Mercurial repos
    * HTTP URLs
    * S3 buckets
    * GCS buckets
* If the module source is remote terraform will copy it to the .terraform directory of where terraform init was ran
## 5B Interact wih modules Inputs and Outputs
* module inputs serve as parameters for the terraform module. 
* you can declare variables in the root module, using the -var cli switch or through environmental variables
* when you define input modules in the child module, the calling module **should pass the values from the child module**
* the input variables can be declared in the variables.tf file of the module.
* The variables have to be **unique** amoung all variables within the **same** module
* terraform 0.13 introduced the `for_each`,`count` and `depends_on` meta-arguments in a module 
## 5C Describe variable scope within modules/child modules
* a local value defined in a module, is only available in the context of that module
* any calling module or child modules invoked by the calling module does not have access to the module local value unless it is exposed as an output 
## 5D Discover Modules from the public module registry
* when consuming a module from the public registry it is best to specify a version
* it is also recommended to specify any child module versions to prevent any configuration issues
* to specify a version you can use the constraint expressions such as:
    * = - specific version
    * >= - greater or equal to
    * ~> - any version in the <major><minor>.x version
    * >= 1.0.0, <= 2.0.0 - any version in between the two versions
* version numbers are **only** applicable to public modules and not local
---
# 6 Terraform workflow

## 6A Terraform workflow
The terraform defines the core workflow in three parts:
    * write - author infrastructure as code
    * Plan - preview the changes
    * Apply - provision reproduciable infrastructure
## 6B Initialise a terraform working directory
* terraform is the first command you run on a terraform configuration
* the init command will prepare the directory holding the terraform configuration. It performs:
    * prepare the state storage: wether your using local or remote it prepares that backend for use (checks it is accessible)
    * retrieve modules: It will check for any modules being used, pull them from their source and place them in the .terraform/modules directory
    * retrieve plugins: terraform init will look for references direct and indirect to providers and provisioners and pulls them down
* you can run `terraform init` multiple times without issue. If nothing has changed nothing will happen. You do however need to run `terraform init` when:
    * you have updated a module, provisioner or provisioner
    * you have want to change the backend from local to remote
    * you have added a new module, provider or provisioner
* as of terraform 0.14 when you run `terraform init` it created a file called .terraform.lock.hcl. This contains a list of providers used, including the version constraints and hashes of the provider plugin
## 6C validate a terraform configuration
* terraform validate will validate the syntax of the terraform configuration, including providers and modules
* if you run terraform plan or a terraform apply wiout a saved plan it will automatically run a validate
* terraform validate will download the plugins for as it needs those plugins to validate the configuration
## 6D generate and review an execution plan
* the terraform plan command is used to create an execution plan
* terraform plan performs:
    * syntax validation
    * refresh of state based on the actual environment
    * perform a comparison of the state file against the configuration
* during the plan terraform might alter the state file if it finds out that one of the managed resources has changed since the previous refresh
* there are several arguments you can use when running terraform plan such as:
    * - out: specify a destination file output for a saved execution plan
    * - refresh: whether or not the state should be refreshed
    * - var: input variables you can pass the plan
    * - var-file: specify a file that contains key/value pairs of variable values
## 6E execute changes to infrastructure with terraform
* terraform apply will run a plan first by default
* if you don't want the plan to run and no prompts you can save an execution plan and run terraform apply with that
* there are several arguments you can apply when running terraform apply such as:
    * -auto-approve:
    * -input: determines whether or not to prompt for input.
    * -var: input variables
    * -var-file: specify a variable file input of inputs
    * -refresh: whether the state should be refreshed: default=true
## 6F terraform destroy
* terraform destroy will delete all the resources and dependencies
* there is two main arguments:
    * -auto-approve
    * -target: you can specify a terget and only the target and its dependecies will be deleted. You can have multiple instances of this flag
---
# 7 Implement and maintain state
## 7B State locking
* terraform will lock the state file when running certain commands. The majority of backends support locking but the below **don't**:
    * artifactory
    * etcd
    * http optionally support state locking
    * 
* terraform writes the backend into two plain text files:
    * .terraform/terraform.tfstate - contains the backend config for the current working directory
    * all plan files capture the information in .terraform/terraform.tfstate at the time the plan was created
## 7C Handle backend authentication methods
* when configuring a backend it is recommended not to store credentials within the backend code block
* you cannot define the backend credentials using Terraform variables, this is because the backend is evaluated during initialisation before the variables are loaded and evaluated.
* It is recommended to use a **partial configuration** in the root module and provide the rest of the information at run time but passing in variables using environmental variables.
## 7D Describe remote state storage mechanisms and supported standard backends
* hashicorp supports two classes of backends:
    * standard - includes state management and possibly locking
    * enhanced - includes remote operations on top of standard features
* The enhanced backends are either terraform cloud or terraform enterprise
* with remote backends the state data is never held on disk. It is loaded into memory and then flushed when no longer nedded
* remote backends allows for collaboration as multiple people can access the same state file
* any outputs from a remote state file can be accessed using a data source of the remote state file
## 7E Describe effect of terraform refresh on state
* `terraform refresh` will analyse the resource provisioned in the real world and compare them to the state file. If there are any differences on the real world it will update the state file to reflect the real world
* `terraform refresh` has been **deprected** as it will update you state file **without** allowing you to review the changes. It is recommended to use `terraform plan -refresh-only` or `terraform apply -refresh-only` which will give you the chance to review the changes
## 7F describe backend clock in configuration and best practises for partial configurations
* the backend configuration is defined in the terraform block of a root module
* To complete a partial backend and supply key elements are runtime you can supply this in 4 ways:
    * interactively at the command prompt when you run terraform init
    * environmental variables
    * through the argument `-backend-config` with a key/value pairs
    * through the argument `-backend-config` to a file containing a key/value pairs
* once you have supplied the details once and run `terraform init` the details will be saved in the .terraform\terraform.tfstate file. This directory should be omitted from the VCS using the .gitignore file
## 7G Understand secret management in state files
* state data includes all sensitive data like passwords, API keys and access creds in plain text. You can configure the backend to be encrypted.
* the sensitive value designation was introduced in terraform 0.13. This allows you to omit the secrets for resources, variables and data source attributes in the logging, terminal outputs or plan files. This doesn't stop the details being held in the state file in clear text.
* It is recommended to store you secrets in a secreats platform like Hashicorp Vault or Azure Key Vault and retreive the sensitive data when required
---
# 8 Read, Generate and modify configuration
## 8A Variables and Outputs
### Input Variables
* for input variables you can supply values at:
    * runtime using the -var-file or -var
    * set as a default
    * terraform.tfvars or terraform.tfvars.json file in the same directory as the configuration
    * placing a file ending with .auto.tfvards or auto.tfvards.json in the same directory as the configuration
    * environmental variables using TF_VAR_
    * interactively typing them in at the cli when running a terraform plan or apply

* the order or prescedence is:
1. environmental variables
2. terraform.tfvars or terraform.tfvars.json file in the same directory as the configuration
3. placing a file ending with .auto.tfvards or auto.tfvards.json in the same directory as the configuration
4. runtime using the -var-file
5. runtime using the -var
6. typing them at the commandline

6 is the highest priority and 1 being the lowest. 6 will override 1

### Output variables
* as of terraform 0.13 outputs can be complex data types
* when you invoke a child module the calling module can consime the outputs of the child module.
* The calling module can consome the outputs of the child module by using a data source
* the output block can have two addtional arguments in addtion to value and description which is:
    * sensitive = trure/false
    * depends_on - list of resources it depends on
* outputs are rendered when you run terraform apply
## 8B describe secure secret injection best practise
* don't store your secrets in .tfvars. terraform has no protections to encrypt or secure variable files. Also the .tfvars file will be in the VCS
* You can sumbit your variable values using -var argument but this will be in the command history in plain text
* one of the easiest ways is to use the environmental using using the TF_VARS_<name>. you can configure the environmental variables to pull the data and it will nto show up in the commandline history or terraform
* The best option is to use a key vault like hashicorp vault
## 8C understand the use of collectiona nd structural types
### Primative
* there are three primative data types in terraform:
    * string
    * boolean
    * number
* primative types have a **single** value
### Complex
* there is also **complex** data types which can be broken into:
    * collection
    * structural

* collection Types
    - list: a sequential list of values identified by their position on the index
    - map: a collection of values each identified by a unique key of type string
    - set: a collection of unique values with no identifies or ordering
* list and map are more common than **set**. all values in a **collection must be the same primitive type**
``` javascript 
variable "my_list" {
    type = list(string)
}
variable "my_map" {values can be multiple types
    type = map(number)
}
```
### structural
* structural types allow you to create a more complex object where the values can be multiple type values. 
    - object: similar to a map, it is a set of key/value pairs but each can be a different type
    - tuple: similar to a **list**, it is an indexed set of values where each value can be different
``` javascript 
Example: of Object
variable "network_info" {
    type = object (
        {
            network_name = string
            cidr_ranges  = list(string)
            sunbnet_mask = number
            nat_enabled  = boolean
        }
    )
}

Example of Tuple
variable "pi_tuple" {
    type = tuple (
        [string, number, boolean, list(number)]
    )
}
["Pi", "25.1", true, [3, 1, 5, 6] ]
```
## 8D Create and differentiate resource and data configuration
### Looping and multiple Instances
* For_each meta-argument takes either a **set** or **map** of **strings** as a value.
**You may need to convert tuples to sets to ensure the correct data type is submitted to a for_each**
``` javascript 
Example: for_each with set
resource "aws_bucket" "my_buckets" {
    for_each = to_set(["Red", "Green", "Blue"])
    name     = "this-is-my-${each.key}-bucket"
}
Example: for_each with map
resource "aws_bucket" "my_bucket" {
    for_each = {
        Red   = "Sand"
        Green = "Rocks"
        Blue  = "Dirt" 
    }

    name = "this-is-my-${each.key}-bucket"
    tags = {
        contents = each.value
    }
}
```
This will loop through the map and for each element (Red, Green, Blue) it will set the tag using the each.value
* for_each looping mechanism is preferred when the object instances being created have differences that are not easily expressed using an interger
## 8E Use resource addressing and resource parameters to connect resources together
* you can reference other resources or data sources by specifying the <resource_type.name or data.name>
* if you need to convert the value type you can use the for expression
## 8F Use terraform built-in functions to write configurations
* there are a number of functions that can be used to manipulate data
## 8G Configure Resource using a dynamic block
* Dynamic blocks are one or more nested blocks.
* they can be used inside resources, data sources, providers and provisioners
``` javascript 
Example
variable "subnet_data" {
    default = {
        subnet1 = {
            name           = "subnet1"
            address_prefix = "10.0.1.0/24"
        }
        subnet2 = {
            name           = "subnet2"
            address_prefix = "10.0.2.0/24"
        }
    }
}

resource "azurerm_virtual_network" "vnet" {
    name                = "prod-vnet"
    resource_group_name = "eastus"
    address_space       = "var.address_space"

    dynamic "subnet" {
        for_each = var.subnet_data
        content {
            name           = subnet.value["name"]
            address_prefix = subnet.value["address_prefix"] 
        }
    } 
}
```
* the data stored in the subnet_data variable needs to be a **map** or **set**
* The keyword dynamic estalishes this is a dynamic block expression
* the name label of subnet lets terraform know that this will be a set of nested block each of type subnet.
* the content section defines the actual content of each generated nested block
* within each content section we need to be able to refer to the information in the subnet_data variable
* terraform creates a temporary iterator with the same name as the name label of the dynamic block "subnet"
## 8H describe built-in dependency management
* when terraform is analysing a configuration it creates a resource graph that lists out all the resources in your configuration and their dependencies. This allows terraform to create the resources in the proper order.
* you can create an explicit dependency using the depends_on statement
---
# 9 Understand terraform enterprise capabilities
* terraform enterprise is an on-prem version of terraform cloud without resource limits, SAML integration and audit logging
## 9A describe the benefits of sentinel, registry and workspaces
### sentinel
* sentinel is a language and framework for policy enabling fine-grained logic based policy decisions
* sentinel is an enterprise only feature of hashicorp console, nomad, terraform and vault
* the sentinel intergation runs within the terraform cloud after the terraform plan and before the terraform apply
* the policies have access to the terraform plan, the state at the time of the plan, and the configuration at the time of the plan
* sentinel policies are rules enfoced on terraform runs to validate the plan and corresponding resources follow the defined policies
### Registry
* terraform cloud offers a private registry to share modules and proivders
* companies can go one tep further and host theor own terraform enterprise with their own registry
### Terraform Cloud Workspaces
* the terraform cloud workspace contains:
    - terraform configuration (usually pulled from VCS)
    - variables, values, secrets and credentials
    - logs from the last plans and applies
## 9C summirise features of terraform cloud
* workspaces
* VCS intergation
* Private module registry
* remote operations - terraform cloud uses managed, disposable VMs to run terraform OSS commands remotely and provide advanced features like sentinel policy enforcement and cost estimations
* Organisations: access control is managed through users, groups and organisations
* sentinel: used for policy definition and enforcement
* cost estimation: estimate the cost of resources being provisioned
* API: a subset of terraform clouds features are available through their API
* serviceNow: integration with serviceNow is available for terraform enterprise customers






