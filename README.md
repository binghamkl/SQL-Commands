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

**Find all logins where password is same as login**
```
SELECT SERVERPROPERTY('machinename') AS 'Server Name', name AS 'Login With
Password Same As Name'
FROM master.sys.sql_logins
WHERE PWDCOMPARE(name,password_hash) = 1
ORDER by name
```

**Find all logins with blank password**
```
SELECT name FROM sys.sql_logins
WHERE PWDCOMPARE('', password_hash) = 1 ;
```

**Server Roles**
| role | permissions |
| --- | --- | 
| sysadmin | all permissions |
| bulkadmin | Run *BULK INSERT* and use *bcp.exe* |
| diskadmin | Manage disk files |
| process admin | Can kill processes on SQL server |
| public | - |
| security admin | Manage logins and properties |
| security admin | Change server-level configs and shut down SQL Server |
| setup admin | Can create linked servers |
| dbcreator | Can create alter drop and restore databases |

**Alter and Drop users from Roles**

`ALTER SERVER ROLE [<rolename>] ADD MEMBER [<login>];`

`ALTER SERVER ROLE [<rolename>] ADD MEMBER [<DOMAIN\name>];`

`ALTER SERVER ROLE [<rolename>] DROP MEMBER [<login>];`

**Login property Funciton**

[Login Function](https://docs.microsoft.com/en-us/sql/t-sql/functions/loginproperty-transact-sql?view=sql-server-ver15)
