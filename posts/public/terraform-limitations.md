# Terraform Limitations
## Count and for_each

* You cannot reference any resource outputs in count or for_each
* You cannot use count or for_each within a module configuration

### You cannot reference any resource outputs in count or for_each

Terraform requires it can compute the **count** and **for_each** phase during the plan *before* and resources are created or modified. This means that **count** and **for_each** can reference hardcoded values, variables, data sources and lists of resources (so long as the length of them can be determined *during* the plan phase

### You cannot use count or for_each within a module configuration
As of Terraform 0.13 count is moved out of the individual resource creation step and moved to module definition instead.

### Zero-Downtime Deployment has limitations
When using the zero-downtime deployment for an ASG there is no correlation between the ASG and the ASG policy. This means when a new ASG is created it will have no knowledge of the current ASG node count. It will provision the minimum set in the config.

The only work around is to write a script to figure out how many instances there was before the ASG deletion and set the minimum count to that value.

### Valid Plan can Fail
There are secenarios where a terraform plan can pass but the apply will fail. An example of this woild be is someone created a user and terraform went to create the same user.
The terraform plan would pass as the plan is **only looking at the state file** and not the deployed resources in the cloud provider.
In this scenario you would use the terraform import command to import the resoucre into the terraform state file. The terraform apply would pass as terraform would be aware that the resource has been created.






