# Oracle

## Scenario 1: Create a user

1. Create a Temporary Tablespace if you have not created it yet.
```sql
CREATE TEMPORARY TABLESPACE DB_TEMP01
TEMPFILE 'Your-Oracle-Directory\oradata\Your-Service-Name\DB_TEMP01.DBF'
SIZE 32M
AUTOEXTEND ON
NEXT 32M MAXSIZE UNLIMITED
EXTENT MANAGEMENT LOCAL;
```
2. Create a Tablespace if you have not created it yet.
```sql
CREATE TABLESPACE DB_DATA01
LOGGING
DATAFILE 'Your-Oracle-Directory\oradata\Your-Service-Name\DB_DATA01.DBF'
SIZE 32M
AUTOEXTEND ON
NEXT 32M MAXSIZE UNLIMITED
EXTENT MANAGEMENT LOCAL;
```
3. Create a User. Replace `your-username` and `your-password` with your own ones.
```sql
alter session set "_ORACLE_SCRIPT" = true;  

CREATE USER your-username IDENTIFIED BY your-password
ACCOUNT UNLOCK
DEFAULT TABLESPACE DB_DATA01
TEMPORARY TABLESPACE DB_TEMP01;
```
4. Grant roles to this User.
```sql
GRANT CONNECT,RESOURCE,DBA TO your-username;
```

## Scenario 2: Drop all the tables of an admin user

The following command will not drop this user but all the tables it owns.

``````sql
alter session set "_oracle_script" = true;
drop user your-username CASCADE;
``````