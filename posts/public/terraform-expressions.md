## Terraform Part 1

### Terraform Expressions
An expression is anything that returns a value.
#### Literals
There are a couple of literals like string and numbers
#### Reference
This allows you to access values from other parts of your code.
To access this you need to use a resource attribute reference like:
```
 <PROVIDER>_<TYPE>.<NAME>.<ATTRIBUTE> 
 an example of this might be aws_security_group.instance.id
 ```