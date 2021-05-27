# SQL-Commands

## Securing SQL

**Finding all logins without policy and expiration checks**
``` 
SELECT serverproperty('machinename') as 'Server Name', [name],
[is_policy_checked], [is_expiration_checked] FROM master.sys.sql_logins
WHERE ( [is_policy_checked] = 0 OR [is_expiration_checked] = 0 ) and name
not like '##MS_%'
```

**All Logins that have not changed password in last 6 months**
```
SELECT name,loginproperty([name], 'PasswordLastSetTime')
FROM sys.sql_logins
WHERE loginproperty([name], 'PasswordLastSetTime') <
DATEADD(month,-6,GETDATE())
```

**Login property Funciton**

[Login Function](https://docs.microsoft.com/en-us/sql/t-sql/functions/loginproperty-transact-sql?view=sql-server-ver15)
