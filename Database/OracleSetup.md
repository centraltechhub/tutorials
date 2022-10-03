**Setting the init file**

1. Make a copy of init.ora to init<database-name>.ora.
  Modify the content as per the requirement. A sample file content is shown below:

  ```
db_name='ORCL'
memory_target=1G
processes = 150
audit_file_dest='<ORACLE_BASE>/admin/orcl/adump'
audit_trail ='db'
db_block_size=8192
db_domain=''
db_create_file_dest=/home/oracle/data/orcl/dbcreatefiledest
db_recovery_file_dest=/home/oracle/data/orcl/dbrecoveryfiledest
db_recovery_file_dest_size=2G
diagnostic_dest='<ORACLE_BASE>'
dispatchers='(PROTOCOL=TCP) (SERVICE=ORCLXDB)'
open_cursors=300
remote_login_passwordfile='EXCLUSIVE'
undo_tablespace='UNDOTBS1'
# You may want to ensure that control files are created on separate physical
# devices
control_files = (ora_control1, ora_control2)
compatible ='11.2.0'
```
  
**Set the following environment variables.**
  
```  
export ORACLE_BASE=/opt/database
export ORACLE_HOME=/opt/database/software/oracle
export ORACLE_SID=dev
```
alternately, the following command can be executed.
```
  . oraenv
```
  
**Logging into the SQL console.**
 
```  
  ./sqlplus / as sysdba
```
```
  create spfile from pfile;  
```    
```
  startup nomount;
  startup nomount pfile='/opt/database/software/oracle/dbs/initOMSNPR.ora'
```  
 
```sql
CREATE DATABASE OMSNPR USER SYS IDENTIFIED BY DevSysPass USER SYSTEM IDENTIFIED BY DevSystemPass MAXLOGFILES 5 MAXLOGHISTORY 10 MAXDATAFILES 50 CHARACTER SET US7ASCII NATIONAL CHARACTER SET AL16UTF16 DEFAULT TABLESPACE USERS DEFAULT TEMPORARY TABLESPACE TEMPTS UNDO TABLESPACE UNDOTBS;

grant all privileges to cn_stgchnmaster identified by password;
grant all privileges to cn_stgchntrans identified by password;
grant all privileges to cn_stgconfig identified by password;
grant all privileges to cn_stgmetadata identified by password;
grant all privileges to cn_stgstats identified by password;
grant all privileges to admin identified by password;
```
  
**Delete DB.**
```
shutdown immediate;
startup mount exclusive restrict;
drop database;  
```  
