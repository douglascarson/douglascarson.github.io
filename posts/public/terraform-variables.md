## Terraform Variables

### Terraform Expressions
Vatiables allow you to hold data within the terraform code.
There are different vatiable types that can be used in terraform and they all have the syntax
```
    variable "NAME" {
        [CONFIG ...]
    }
```
The three __optional__ parameters that can be utilised in the variable are:
* description
* default
* Type

__Description__

This is used to provide comments about you code. It is also when running *plan* or *apply*

__default__

If no other variable is declared either through the commandline of through an environmental variable Terraform will fallback on the defauly value.

__Type__

The Type parameter allows you to enforce *type constraints*. This allows you to validate the type of data being passed into your code.

There are a number of types supported by Terraform such as:
* strings
* numbers
* object
* map
* boolean
* set
* tuple
* any

Set Type
The set type containts a single type of variable such as number, but 

