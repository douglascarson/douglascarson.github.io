---
tags:
    - Azure SQL
    - Security
---
## Securing Azure SQL
To secure Azure SQL from a Network Perspecive there are two levels:
1. Server level
2. Database level

The order of analysis is Database and then Server. It is advisable to secure at the database level from the application perspective and Server from an admin perspective.

The command to run to apply a database firewall IP filter are below. It is always advisable to allow azure access to the database.
``` shell
# Allow Azure access
EXECUTE sp_set_database_firewall_rule N'Allow Azure', '0.0.0.0', '0.0.0.0';
# Allow a single IP
execute sp_set_database_firewall_rule N'Allow VM1', '10.0.0.4', '10.0.0.4'
# Allow an IP address Range
execute sp_set_database_firewall_rule N'Allow VM1', '10.0.0.4', '10.0.0.6'

```
To list the current database IP firewall rules tou can run the command
``` shell
select * from sys.database_firewall_rules
```
To delete an IP firewall rule
```shell
execute sp_delete_database_firewall_rule N'Allow Azure'
```
