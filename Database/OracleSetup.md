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
