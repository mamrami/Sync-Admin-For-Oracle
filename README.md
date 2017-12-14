# Sync Admin For Oracle 
Sync your Search engine with your data from Oracle database. 

# Getting started

Sync Admin allows you to sync data from an Oracle database with Outbound Rest supported platforms. 
This is usefull for sync data with Elasticsearch / Solar / Kafka and more. use External Indexes and Text Services. 

version 0.1 support, by Technology:
ElasticSearch:
 - Index Mapping
 - Dynamin Index Name
 - Complete refresh
 - Schedule sync 
 - User/Password

 
# Installation
1. Download the Util packages
2. Connect as DBA 
```
% cd /sync_scripts
% sqlplus sys as sysdba
Enter password: password
```
3. Create Tablespace 
```
create tablespace SYNC_TS...
```
4. Create SYNC_ADMIN user
```
create user SYNC_ADMIN identified by <password> default tablespace SYNC_TS;
```
5. Grant Privileges
```
@grants.sql
```
6. Connect to SYNC_ADMIN
```
SQLPLUS> conn SYNC_ADMIN
```
7. Create SYNC_ADMIN tables
```
@create_schema.sql
```
# Create Data Link
```
SQL> exec DBMS_SYNC_ADMIN.create_data_link(link_name ,provider_type ,connection_type ,host_name ,user_name ,password ,port)
```

On the creation we will
- LINK_NAME - uniqu name of the data link 
- PROVIDER_TYPE - data link provider
- CONNECTION_TYPE - Can be 'RO' for read only ,'RW' for read and write
- HOST_NAME - Provider host name
- USER_NAME - User name
- PASSWORD  - User Password
- PORT - Port of the Provider
 
1. Sync Admin can create different types of data link providers
   you can find the Providers at providers table:

```
SQL> select * from providers;

        ID NAME
---------- --------------------------------------------------
         1 Solar 1.0
         2 Elastic 1.4
         3 Elastic 5.0
         4 Cassandra
         4 Gremlin Server
```
3. Example:
Create Data link for Elasticsearch
```
exec DBMS_SYNC_ADMIN.create_data_link('MY_LINK',1,'RW','192.168.1.110','','',9200);
```
# Create Outbound Synchronization
```
SQL> exec DBMS_SYNC_ADMIN.CREATE_SYNC(owner  ,sync_name ,sql_cmd ,data_link ,object_name ,schedule ,REFRESH_METHOD)
```
- OWNER - syncs owner
- SYNC_NAME - sync unique name
- SQL_CMD - SQL Select Command 
- DATA_LINK -Data link Name
- OBJECT_NAME - Target Name of the Provider. es: Index_name
- SCHEDULE - Scheduler Parameters 
- REFRESH_METHOD - Refresh Method. can be: F- Fast refresh , C - Complete refresh , TC - Complete with Drop Object if Exist.

SQL> exec sync_admin.DBMS_SYNC_ADMIN.CREATE_SYNC('SYS','ALERT_LOG','select ORIGINATING_TIMESTAMP,MESSAGE_TEXT from sys.v_alert_log where MESSAGE_TEXT like '%ORA-%'','1','alert_{DDMM}',null,'C');


# Create Inbound Synchronization

# Create Extranl Index Service

# USE CASES
### Applicaiton analysis
### Oracle Performance Monitoring
### External Indexing and Text analysis



