## 📌 Phase IV: Database Creation & Configuration

This phase focuses on setting up the complete Oracle database environment required for the Smart Car Inventory System. In this phase, a dedicated pluggable database (PDB) is created, custom tablespaces for data, indexes, and temporary operations are configured with autoextend settings, and memory parameters such as SGA and PGA are documented. The archive log status is checked and recorded, and finally, a project-specific user is created and granted the necessary privileges to own and manage all future database objects.


## Pluggable Database Creation & Creating the Admin User and Granting Privileges
### Pluggable Database Creation
A dedicated pluggable database was created to isolate the project environment and support secure development and testing.

```sql
CREATE PLUGGABLE DATABASE TUE_27969_SABIN_SMARTCARINVENTORY_DB
ADMIN USER sabin1 IDENTIFIED BY sabin
FILE_NAME_CONVERT = (
  'C:\APP\HP\PRODUCT\21C\ORADATA\XE\PDBSEED',
  'C:\APP\HP\PRODUCT\21C\ORADATA\XE\TUE_27969_SABIN_SMARTCARINVENTORY_DB'
);

ALTER SESSION SET CONTAINER = TUE_27969_SABIN_SMARTCARINVENTORY_DB;
ALTER PLUGGABLE DATABASE TUE_27969_SABIN_SMARTCARINVENTORY_DB OPEN;
ALTER PLUGGABLE DATABASE TUE_27969_SABIN_SMARTCARINVENTORY_DB SAVE STATE;
```
### Creating the Admin User and Granting Privileges

```sql
CREATE USER izere IDENTIFIED BY sabin;
GRANT CONNECT, RESOURCE, DBA, SYSDBA TO izere;
```
<img width="1224" height="1000" alt="creating pdb,user and granting previleges" src="https://github.com/user-attachments/assets/2a036d0c-8b44-456a-bcb3-4cd3ea285a4f" />

### Creating Tablespaces (DATA, INDEX, TEMP)

```sql
-- DATA tablespace
CREATE TABLESPACE smartcar_data  
DATAFILE 'C:\APP\HP\PRODUCT\21C\ORADATA\XE\TUE_27969_SABIN_SMARTCARINVENTORY_DB\smartcar_data01.dbf'  
SIZE 100M  
AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED;

-- INDEX tablespace
CREATE TABLESPACE smartcar_index  
DATAFILE 'C:\APP\HP\PRODUCT\21C\ORADATA\XE\TUE_27969_SABIN_SMARTCARINVENTORY_DB\smartcar_index01.dbf'  
SIZE 50M  
AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED;

-- TEMP tablespace
CREATE TEMPORARY TABLESPACE smartcar_temp  
TEMPFILE 'C:\APP\HP\PRODUCT\21C\ORADATA\XE\TUE_27969_SABIN_SMARTCARINVENTORY_DB\smartcar_temp01.dbf'  
SIZE 50M  
AUTOEXTEND ON NEXT 5M MAXSIZE UNLIMITED;
```
<img width="1019" height="412" alt="tablespace creation" src="https://github.com/user-attachments/assets/3f0b5d52-a613-4907-b036-2b88a56a760b" />

### TABLESPACES IN OEM
<img width="1905" height="908" alt="OEM TABLESPACE" src="https://github.com/user-attachments/assets/796d81f5-0019-428f-890c-d669591514f8" />

### Enabling Archive Logging

```sql
SHUTDOWN IMMEDIATE;
STARTUP MOUNT;
ALTER DATABASE ARCHIVELOG;
ALTER DATABASE OPEN;
```
<img width="756" height="824" alt="archive logging enabled" src="https://github.com/user-attachments/assets/d20d3cd5-40df-4a1d-a4db-46b0f53fcdb0" />

### Memory Parameters
```sql
SHOW PARAMETER sga_target;
SHOW PARAMETER pga_aggregate_target;
SHOW PARAMETER memory_target;
```
<img width="1158" height="348" alt="Memory parameters (SGA, PGA)" src="https://github.com/user-attachments/assets/33a867f4-07f3-4180-ba7b-21c43578c778" />

### Phase IV Completion Summary
At the end of this phase, a fully functional Oracle Database Pluggable Database has been created and configured with dedicated tablespaces, archive logging enabled, documented memory parameters, and an administrative user ready to support all subsequent phases of the Smart Car Inventory System.
