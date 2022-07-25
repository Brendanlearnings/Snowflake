
# All Privileges  


|   PRIVILEDGE                                                                  | OBJECT TYPE                                                             | DESCRIPTION |
|-------------------------------------------------------------------------------|:-------------------------------------------------------------------------:|-------------|
| ALL [ PRIVILEGES ]                                                            | All                                                                     | Grants all the privileges for the specified object type.|
| APPLY MASKING POLICY                                                        | Global                                                                  | Grants the ability to set a Column-level Security masking policy on a table or view column and to set a masking policy on a tag. This global privilege also allows executing the DESCRIBE operation on tables and views. |
| APPLY ROW ACCESS POLICY | Global |Grants the ability to add and drop a row access policy on a table or view. This global privilege also allows executing the DESCRIBE operation on tables and views.|
| APPLY SESSION POLICY | Global | Grants the ability to set or unset a session policy on an account or user. |
| APPLY TAG | Global | Grants the ability to add or drop a tag on a Snowflake object. |
| ATTACH POLICY | Global | Grants ability to activate a network policy by associating it with your account.|
| CREATE object_type> | Global, Database, Schema | Grants ability to create an object of object_type> (e.g. CREATE TABLE grants the ability to create a table within a schema).|
| DELETE | Table | Grants ability to execute a DELETE command on the table. |
| EXECUTE MANAGED TASK | Global | Grants ability to create tasks that rely on Snowflake-managed compute resources (serverless compute model). Only required to create serverless tasks. The role that has the OWNERSHIP privilege on a task must have both the EXECUTE MANAGED TASK and the EXECUTE TASK privilege for the task to run.|
| EXECUTE TASK | Global | Grants ability to run tasks owned by the role. For serverless tasks to run, the role that has the OWNERSHIP privilege on the task must also have the global EXECUTE MANAGED TASK privilege.|
| IMPORT SHARE | Global | Applies to data consumers. Grants ability to view shares shared with your account. Also grants ability to create databases from the shares; requires the global CREATE DATABASE privilege.|
| OVERRIDE SHARE RESTRICTIONS | Global | Grants ability to set value for the SHARE_RESTRICTIONS parameter which enables a Business Critical provider account to add a consumer account (with Non-Business Critical edition) to a share. For more details, see Enabling Sharing from a Business Critical Account to a non-Business Critical Account.|
|IMPORTED PRIVILEGES| Database, Data Exchange | Grants ability to enable roles other than the owning role to access a shared database or manage a Snowflake Marketplace / Data Exchange.|
| INSERT | Table | Grants ability to execute an INSERT command on the table.|
| MANAGE GRANTS | Global | Grants ability to grant or revoke privileges on any object as if the invoking role were the owner of the object.|
| MODIFY | Resource Monitor, Warehouse, Data Exchange Listing, Database, Schema |Grants ability to change the settings or properties of an object (e.g. on a virtual warehouse, provides the ability to change the size of a virtual warehouse).|
| MONITOR | User, Resource Monitor, Warehouse, Database, Schema, Task | Grants ability to see details within an object (e.g. queries and usage within a warehouse).|
| MONITOR EXECUTION | Global | Grants ability to monitor pipes (Snowpipe) or tasks in the account. |
| MONITOR USAGE | Global | Grants ability to monitor account-level usage and historical information for databases and warehouses; for more details, see Enabling Non-Account Administrators to Monitor Usage and Billing History in the Classic Web Interface. Additionally grants ability to view managed accounts using SHOW MANAGED ACCOUNTS.|
| OPERATE | Warehouse, Task | Grants ability to start, stop, suspend, or resume a virtual warehouse. Grants ability to suspend or resume a task.|
|OWNERSHIP | All | Grants ability to delete, alter, and grant or revoke access to an object. Required to rename an object. OWNERSHIP is a special privilege on an object that is automatically granted to the role that created the object, but can also be transferred using the GRANT OWNERSHIP command to a different role by the owning role (or any role with the MANAGE GRANTS privilege).|
| READ | Stage (internal only) | Grants ability to perform any operations that require reading from an internal stage (GET, LIST, COPY INTO etc.) | 
| REFERENCES | Table, External table, View | Grants ability to view the structure of an object (but not the data). For tables, the privilege also grants the ability to reference the object as the unique/primary key table for a foreign key constraint.| 
| SELECT | Table, External table, View, Stream | Grants ability to execute a SELECT statement on the table/view.| 
| TRUNCATE | Table | Grants ability to execute a TRUNCATE TABLE command on the table.| 
| UPDATE | Table | Grants ability to execute an UPDATE command on the table.  |
| USAGE | Warehouse, Data Exchange Listing, Integration, Database, Schema, Stage (external only), File Format, Sequence, Stored Procedure, User-Defined Function, External Function | Grants ability to execute a USE object> command on the object. Also grants ability to execute a SHOW objects> command on the object.|
| WRITE | Stage (internal only) | Grants ability to perform any operations that require writing to an internal stage (PUT, REMOVE, COPY INTO location>, etc.).|
<br>

# Global Privileges


|   PRIVILEDGE                                                                  | USAGE                                                              | NOTES |
|-------------------------------------------------------------------------------|:-----------------------------------------------------------------------:|-------------|
| APPLY MASKING POLICY | Grants ability to set a Column-level Security masking policy on a table or view column and to set a masking policy on a tag. | This global privilege also allows executing the DESCRIBE operation on tables and views.| 
|APPLY ROW ACCESS POLICY| Grants the ability to add and drop a row access policy on a table or view. | This global privilege also allows executing the DESCRIBE operation on tables and views.|
| APPLY SESSION POLICY | Grants the ability to set or unset a session policy on an account or user. | | 
| ATTACH POLICY | Grants ability to activate a network policy by associating it with your account. | | 
| CREATE ACCOUNT | Enables a data provider to create a new managed account (i.e. reader account). For more details, see Managing Reader Accounts. | Must be granted by the ACCOUNTADMIN role. |
| CREATE ROLE | Enables creating a new role. | | 
| CREATE USER | Enables creating a new user. | | 
| MANAGE GRANTS | Enables granting or revoking privileges on objects for which the role is not the owner. | Must be granted by the SECURITYADMIN role (or higher).| 
| CREATE DATA EXCHANGE LISTING | Enables creating a new Data Exchange listing. | Must be granted by the ACCOUNTADMIN role. |
| CREATE INTEGRATION | Enables creating a new notification, security, or storage integration. | Must be granted by the ACCOUNTADMIN role.|
|CREATE NETWORK POLICY|Enables creating a new network policy.|
|
|CREATE SHARE|Enables a data provider to create a new share. For more details, see Enabling non-ACCOUNTADMIN Roles to Perform Data Sharing Tasks.|Must be granted by the ACCOUNTADMIN role.|
|CREATE WAREHOUSE|Enables creating a new virtual warehouse.|
|
|EXECUTE MANAGED TASK|Grants ability to create tasks that rely on Snowflake-managed compute resources (serverless compute model). Only required for serverless tasks. The role that has the OWNERSHIP privilege on a task must have both the EXECUTE MANAGED TASK and the EXECUTE TASK privilege for the task to run.|Must be granted by the ACCOUNTADMIN role.|
|EXECUTE TASK|Grants ability to run tasks owned by the role. For serverless tasks to run, the role that has the OWNERSHIP privilege on the task must also have the global EXECUTE MANAGED TASK privilege.|Must be granted by the ACCOUNTADMIN role.|
|IMPORT SHARE|Enables a data consumer to view shares shared with their account. Also grants the ability to create databases from shares; requires the global CREATE DATABASE privilege. For more details, see Enabling non-ACCOUNTADMIN Roles to Perform Data Sharing Tasks.|Must be granted by the ACCOUNTADMIN role.|
|MONITOR EXECUTION|Grants ability to monitor any pipes or tasks in the account.|Must be granted by the ACCOUNTADMIN role. The USAGE privilege is also required on each database and schema that stores these objects.|
|MONITOR USAGE|Grants ability to monitor account-level usage and historical information for databases and warehouses; for more details, see Enabling Non-Account Administrators to Monitor Usage and Billing History in the Classic Web Interface. Additionally grants ability to view managed accounts using SHOW MANAGED ACCOUNTS.|Must be granted by the ACCOUNTADMIN role.|
|OVERRIDE SHARE RESTRICTIONS|Grants ability to set value for the SHARE_RESTRICTIONS parameter which enables a Business Critical provider account to add a consumer account (with Non-Business Critical edition) to a share.|For more details, see Enabling Sharing from a Business Critical Account to a non-Business Critical Account.|
| ALL [ PRIVILEGES ] | Grants all global privileges.|
|

<br>

# User Privileges

|   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|MONITOR|Grants ability to view the login history for the user.|
|OWNERSHIP|Grants full control over a user/role. Only a single role can hold this privilege on a specific object at a time.|
 ALL [ PRIVILEGES ]|Grants all privileges, except OWNERSHIP, on the user.|

 <br>

 # Role Privileges 

 |   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
| OWNERSHIP|Grants full control over a user/role. Only a single role can hold this privilege on a specific object at a time. Note that the owner role does not inherit any permissions granted to the owned role. To inherit permissions from a role, that role must be granted to another role, creating a parent-child relationship in a role hierarchy.|

 # Resource Monitor Privileges 


 |   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|MODIFY|Enables altering any properties of a resource monitor, such as changing the monthly credit quota.|MONITOR|Enables viewing a resource monitor.|
|ALL [ PRIVILEGES ]|Grants all privileges, except OWNERSHIP, on the resource monitor.|

<br>

# Virtual Warehouse Privileges 
|   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|MODIFY|Enables altering any properties of a warehouse, including changing its size. Required to assign a warehouse to a resource monitor. Note that only the ACCOUNTADMIN role can assign warehouses to resource monitors.|
|MONITOR|Enables viewing current and past queries executed on a warehouse as well as usage statistics on that warehouse.|OPERATE|Enables changing the state of a warehouse (stop, start, suspend, resume). In addition, enables viewing current and past queries executed on a warehouse and aborting any executing queries.|
|USAGE|Enables using a virtual warehouse and, as a result, executing queries on the warehouse. If the warehouse is configured to auto-resume when a SQL statement (e.g. query) is submitted to it, the warehouse resumes automatically and executes the statement.|
|OWNERSHIP|Grants full control over a warehouse. Only a single role can hold this privilege on a specific object at a time.|
|ALL [ PRIVILEGES ]|Grants all privileges, except OWNERSHIP, on the warehouse.|

<br>

# Connection Privileges 
None 

<br>

# Integration Privileges 
|   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|USAGE|Enables referencing the storage integration when creating a stage (using CREATE STAGE) or modifying a stage (using ALTER STAGE).|
USE_ANY_ROLE|Allows the External OAuth client or user to switch roles only if this privilege is granted to the client or user. Configure the External OAuth security integration to use the EXTERNAL_OAUTH_ANY_ROLE_MODE parameter using CREATE SECURITY INTEGRATION (External OAuth) or ALTER SECURITY INTEGRATION (External OAuth).|
|OWNERSHIP|Grants full control over an integration. Only a single role can hold this privilege on a specific object at a time.|
|ALL [ PRIVILEGES ]|Grants all privileges, except OWNERSHIP, on the integration.|

<br>

# Network Policy Privileges
|   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|OWNERSHIP|Grants full control over the network policy. Only a single role can hold this privilege on a specific object at a time.|

<br>

# Session Policy Privileges 
|   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|OWNERSHIP|Transfers ownership of a session policy, which grants full control over the session policy. Required to alter most properties of a session policy.|

<br>

# Data Exchange Privileges 
|   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|IMPORTED PRIVILEGES|Enables roles other than the owning role to manage a Snowflake Marketplace or Data Exchange.|

<br>

# Data Exchange Listing Privileges
|   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|MODIFY|Enables roles other than the owning role to modify a Snowflake Marketplace or Data Exchange listing.|
|USAGE|Enables viewing a Snowflake Marketplace or Data Exchange listing.|
|OWNERSHIP|Grants full control over a Snowflake Marketplace or Data Exchange listing. Only a single role can hold this privilege on a specific object at a time.|
|ALL [ PRIVILEGES ]|Grants all privileges, except OWNERSHIP, on a Snowflake Marketplace or Data Exchange listing.|

<br>

# Database Privileges 
|   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|MODIFY|Enables altering any settings of a database.|
|MONITOR|Enables performing the DESCRIBE command on the database.|
|USAGE|Enables using a database, including returning the database details in the SHOW DATABASES command output. Additional privileges are required to view or take actions on objects in a database.|
|CREATE SCHEMA|Enables creating a new schema in a database, including cloning a schema.|IMPORTED PRIVILEGES|Enables roles other than the owning role to access a shared database; applies only to shared databases.|OWNERSHIP|Grants full control over the database. Only a single role can hold this privilege on a specific object at a time.|
|ALL [ PRIVILEGES ]|Grants all privileges, except OWNERSHIP, on a database.|

<br>

### Notes:
* Changing the properties of a database, including comments, requires the OWNERSHIP privilege for the database.

* If **any** database privilege is granted to a role, that role can take SQL actions on objects in a schema using fully-qualified names. The role must have the USAGE privilege on the schema as well as the required privilege or privileges on the object. To make a database the active database in a user session, the USAGE privilege on the database is required.

<br>

# Schema Privileges
|   PRIVILEDGE                                                                  | USAGE                                                             |
|-------------------------------------------------------------------------------|-------------|
|MODIFY|Enables altering any settings of a schema.|
|MONITOR|Enables performing the DESCRIBE command on the schema.|
|USAGE|Enables using a schema, including returning the schema details in the SHOW SCHEMAS command output. To execute SHOW objects> commands for objects (tables, views, stages, file formats, sequences, pipes, or functions) in the schema, a role must have at least one privilege granted on the object.|
|CREATE TABLE|Enables creating a new table in a schema, including cloning a table. Note that this privilege is not required to create temporary tables, which are scoped to the current user session and are automatically deleted when the session ends.|
|CREATE EXTERNAL TABLE|Enables creating a new external table in a schema.|
|CREATE VIEW|Enables creating a new view in a schema.|
|CREATE MATERIALIZED VIEW|Enables creating a new materialized view in a schema.|
|CREATE MASKING POLICY|Enables creating a new Column-level Security masking policy in a schema.|
|CREATE ROW ACCESS POLICY|Enables creating a new row access policy in a schema.|
|CREATE SESSION POLICY|Enables creating a new session policy in a schema.|
|CREATE STAGE|Enables creating a new stage in a schema, including cloning a stage.|
|CREATE FILE FORMAT|Enables creating a new file format in a schema, including cloning a file format.|
|CREATE SEQUENCE|Enables creating a new sequence in a schema, including cloning a sequence.|
|CREATE FUNCTION|Enables creating a new UDF or external function in a schema.|
|CREATE PIPE|Enables creating a new pipe in a schema.|
|CREATE STREAM|Enables creating a new stream in a schema, including cloning a stream.|
|CREATE TAG|Enables creating a new tag key in a schema.|
|CREATE TASK|Enables creating a new task in a schema, including cloning a task.|
|CREATE PROCEDURE|Enables creating a new stored procedure in a schema.|
|ADD SEARCH OPTIMIZATION|Enables adding search optimization to a table in a schema.|
|OWNERSHIP|Grants full control over the schema. Only a single role can hold this privilege on a specific object at a time.|
|ALL [ PRIVILEGES ]|Grants all privileges, except OWNERSHIP, on a schema.|

<br>

### Notes:

* Changing the properties of a schema, including comments, requires the OWNERSHIP privilege for the database.

* Operating on a schema also requires the USAGE privilege on the parent database.