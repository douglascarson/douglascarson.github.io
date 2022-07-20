## Count
Count is good to loop over resources. It only works with a number being passed in as an argument.

##Syntax
```
resource "<PROVIDER>_<TYPE>" "<NAME>" {
    count = <NUMBER> or length(<VARIABLE consisting of a list>)
    <PROPERTY> = [count.index]
}

Example
resource "aws_iam_user" "example" {
  count = length(var.user_names)
  name = var.user_names[count.index]
}
OR
resource "aws_iam_user" "example" {
  count = length(var.user_names)
  name = "User-${count.index}" # interpolation using the count.index
}
```

There are 2 limitations:
1. You can't use count within a resource to loop over inline blocks. An inline block is an argument you set eithin a resource like tags for a VM.
2. With count, the resource you create becomes a list or an array of resources. If you delete an item from the middle of the list Terraform will rename the index you removed, delete and recreate all items in the last after the change.

### Count Object Type

When using count you are returned an array
```
aws_iam_user.example
[
  {
    "arn" = "arn:aws:iam::88848412487732:user/neo"
    "force_destroy" = false
    "id" = "neo"
    "name" = "neo"
    "path" = "/"
    "permissions_boundary" = tostring(null)
    "tags" = tomap(null) /* of string */
    "tags_all" = tomap({})
    "unique_id" = "AIDA45XONMI2I7OE3OBVP"
  },
  {
    "arn" = "arn:aws:iam::88848456487732:user/trinity"
    "force_destroy" = false
    "id" = "trinity"
    "name" = "trinity"
    "path" = "/"
    "permissions_boundary" = tostring(null)
    "tags" = tomap(null) /* of string */
    "tags_all" = tomap({})
    "unique_id" = "AIDA45XONMI2LSL7M6QXY"
  },
  {
    "arn" = "arn:aws:iam::88848674487732:user/morpheus"
    "force_destroy" = false
    "id" = "morpheus"
    "name" = "morpheus"
    "path" = "/"
    "permissions_boundary" = tostring(null)
    "tags" = tomap(null) /* of string */
    "tags_all" = tomap({})
    "unique_id" = "AIDA45XONMI2IGU6KOSN6"
  },
]
```

### Get instance within an array
```
aws_iam_user[0] or aws_iam_user[1] 
```

### Get an attribute of an instance in an array
```
aws_iam_user[0].arn
```

## For_each
The for_each expression is used to loop through lists, sets or maps to create either:

1. multiple copies of an entire resource

2. multiple copies of an inline block within a resource

The sytax for this is
```
resource "<PROVIDER>_<TYPE>" "<NAME>" {
    for_each = <COLLECTION>
        [CONFIG ...]
}
```
* COLLECTION is a set or map as lists are not supported when creating a resource.

* CONFIG is used to set what ever config you need to set for that resource.

* each.key is used to reference the individual key of each item in the COLLECTION 

* each.value is used to reference the individual item in the COLLECTION

### each.value
each.value is used to refer to the individual item in a set

### each.key
* each.key is typically used when using maps of key/value pairs

### Object Type

* Once you have created a resource with for_each it creates a map instead of a single resource or an array like count

```
{
  "morpheus" = {
    "arn" = "arn:aws:iam::89088484487732:user/morpheus"
    "force_destroy" = false
    "id" = "morpheus"
    "name" = "morpheus"
    "path" = "/"
    "permissions_boundary" = tostring(null)
    "tags" = tomap(null) /* of string */
    "tags_all" = tomap({})
  }
  "neo" = {
    "arn" = "arn:aws:iam::88845484487732:user/neo"
    "force_destroy" = false
    "id" = "neo"
    "name" = "neo"
    "path" = "/"
    "permissions_boundary" = tostring(null)
    "tags" = tomap(null) /* of string */
    "tags_all" = tomap({})
  }
  "trinity" = {
    "arn" = "arn:aws:iam::88848764487732:user/trinity"
    "force_destroy" = false
    "id" = "trinity"
    "name" = "trinity"
    "path" = "/"
    "permissions_boundary" = tostring(null)
    "tags" = tomap(null) /* of string */
    "tags_all" = tomap({})
  }
}
```

The keys are the username and values are the attributes of that user

### get an instances value

```
<RESOURCE>.<REFERENCE_NAME>["name"]

Example:

aws_iam_user.example["neo"]
```
### Get all instances of a property in the map
To extract the values from the keys of the map you need to use the values built in function

```
values(aws_iam_user.example)[*].arn
```
## For_each inline
To create dynamic inline blocks you can use a for_each loop

```
dynamic "<VAR_NAME>" {
  for_each = <COLLECTION>
  content {
    [CONFIG ...]
  }
}
```
* VAR_NAME is the variable name that will store the value of each iteration (instead of each)
* COLLECTION is a list or map to iterate over
* CONTENT block is what you generate from each iteration

## Loops with For Expressions
With Terrafom you can loop over a list, a set, a tuple, a map, or an object and generate a single value.

### For Construct
```
[for <ITEM> in <LIST>: <OUTPUT>]

Example
variable "names"{
  type = list(string)
  default = ["neo", "trinity", "morpheus"]
}
output "upper_names" {
  value = [for name in var.names: upper(name)]
}
```
* upper_names = iutput name
* name is the item being workied on in the loop
* var.names is the list decalred in the variable var.names
* upper(name) is the function to place all text in name to uppercase 

### For conditions
You can filter the list as part of the for loop
```
output "upper_names" {
  value = [for name in var.names: upper(name) if length(name) < 5]
}
```
### For loop with a map construct
```
[for <KEY>, <VALUE> in <MAP>: <OUTPUT>]

Example

variable "hero_thousand_faces" {
  type = map(string) 
  default = {
    neo = "hero"
    trinity = "love interest"
    morpheus = "mentor"
  }
}

output "bios" {
  value = [for name, role in var.hero_thousand_faces: "${name} is the ${role}"]
}
```
### Use for loop to output a map from a list

```
{for <ITEM> in <LIST>: <OUTPUT_KEY => <OUTPUT_VALUE>}

Example
variable "names"{
  type = list(string)
  default = ["neo", "trinity", "morpheus"]
}

output "upper_names" {
  value = {for n, v in var.names : n => upper(v)}
}

Output
{
  "0" = "NEO"
  "1" = "TRINITY"
  "2" = "MORPHEUS"
}

```
* As we are using curly braces on the for loop it will output a map
* n is the idex number in the list
* v is the value

### Use for loop to output a map from a map

```
Example
variable "hero_thousand_faces" {
  type = map(string) 
  default = {
    neo = "hero"
    trinity = "love interest"
    morpheus = "mentor"
  }
}
{for k, v in var.map : v => upper(k)}

Output
{
  "hero" = "NEO"
  "love interest" = "TRINITY"
  "mentor" = "MORPHEUS"
}

```
* k is the key fro the map
* v is the value from the key
* The outoup was swapped around to show the new map

## String Directives
Sting Directives allow you to use for loops and if statements within strings.
Terraform supports two types of string directives:
* for loops
* conditionals
You use a percent sign and curly braces %{}


### for-loop string directive Construct
```
%{for <ITEM> in <COLLECTION> }<BODY>%{ endfor }
```
* <ITEM> is the local variable name to assign to each item in COLLECTION
* <COLLECTION> is the list or map to loop over
* BODY is what to ender each iteration

### for-loop string directive Example
```

```

### Conditional string directive Construct
```
"%{ if <VARIABLE> condition}${<VARIABLE>}&{ else }statement%{ endif }"

Example
variable "test" {
  default = ""
}

"%{ if var.test != "" }${var.test}%{ else }var.test is empty%{ endif }"

Output
"var.test is empty!"
```
### Breakdown
```
"%{ if var.test != "" }${var.test}%{ else }var.test is empty%{ endif }"
```
If the variable test is not equal to "" place the data into the ```${var.test}```
else output the string ```var.test is empty``` 