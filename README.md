# SQL-Commands

## Securing SQL

**Finding all logins without policy and expiration checks**
``` SELECT serverproperty('machinename') as 'Server Name', [name],
[is_policy_checked], [is_expiration_checked] FROM master.sys.sql_logins
WHERE ( [is_policy_checked] = 0 OR [is_expiration_checked] = 0 ) and name
not like '##MS_%'
```
