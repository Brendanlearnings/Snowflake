# Topic quick links 
* [Snowflake Information Schema](#snowflake-information-schema)
* [What is INFORMATION SCHEMA?](#what-is-information-schema)
* [INFORMATION SCHEMA Views and Table Functions](#information-schema-views-and-table-functions)
    * [List of views](#list-of-views)
    * [List of Table Functions](#list-of-table-functions)
* [General Usage Notes](#general-usage-notes)
* [Considerations for Replacing SHOW Commands with Information Schema Views](#considerations-for-replacing-show-commands-with-information-schema-views)
* [Qualifying the Names of Information Schema Views and Table Functions in Queries](#qualifying-the-names-of-information-schema-views-and-table-functions-in-queries)

<br>

# Snowflake Information Schema
* aka <mark style="background-color: #89cff0">Data Dictionary</mark> consist of a set of system-defined views and table functions that provide extensive metadata information about objects in your account. 
* Implemented as a schema named INFORMATION_SCHEMA that SF automatically created in every database in an account. 
<br>

# What is INFORMATION SCHEMA?
* Each DB in your account has a built-in, read only schema named INFORMATION_SCHEMA which contains:
    * Views of all objects contained in DB, views for account level objects (eg. roles, warehouses and databases)
    * Table functions for historical and usage data across account. 
<br>

# INFORMATION SCHEMA Views and Table Functions 
## List of views 
* ANSI-standard views for the database and account-level objects that are relevant to Snowflake.
* Snowflake-specific views for the non-standard objects that Snowflake supports (stages, file formats, etc.).

|VIEW|TYPE|SNOWFLAKE SPECIFIC|NOTES
|---|:---:|:---:|---|
|[APPLICABLE_ROLES](https://docs.snowflake.com/en/sql-reference/info-schema/applicable_roles.html)|Account|||
|[COLUMNS](https://docs.snowflake.com/en/sql-reference/info-schema/columns.html)|Database|||
|[DATABASES](https://docs.snowflake.com/en/sql-reference/info-schema/databases.html)|Account|✔||
|[ENABLED_ROLES](https://docs.snowflake.com/en/sql-reference/info-schema/enabled_roles.html)|Account|||
|[EXTERNAL_TABLES](https://docs.snowflake.com/en/sql-reference/info-schema/external_tables.html)|Database|✔||
|[FILE FORMATS](https://docs.snowflake.com/en/sql-reference/info-schema/file_formats.html)|Database|✔||
|[FUNCTIONS](https://docs.snowflake.com/en/sql-reference/info-schema/functions.html)|Database|||
|[INFORMATION_SCHEMA_CATALOG_NAME](https://docs.snowflake.com/en/sql-reference/info-schema/information_schema_catalog_name.html)|Account|||
|[LOAD_HISTORY](https://docs.snowflake.com/en/sql-reference/info-schema/load_history.html)|Account|✔|Data retained for 14 days.|
|[OBJECT_PRIVILEGES](https://docs.snowflake.com/en/sql-reference/info-schema/object_privileges.html)|Account|||
|[PACKAGES](https://docs.snowflake.com/en/sql-reference/info-schema/packages.html)|Database|✔||
|[PIPES](https://docs.snowflake.com/en/sql-reference/info-schema/pipes.html)|Database|✔||
|[PROCEDURES](https://docs.snowflake.com/en/sql-reference/info-schema/procedures.html)|Database|✔||
|[REFERENTIAL_CONSTRAINTS](https://docs.snowflake.com/en/sql-reference/info-schema/referential_constraints.html)|Database|||
|[REPLICATION_DATABASES](https://docs.snowflake.com/en/sql-reference/info-schema/replication_databases.html)|Account|✔||
|[SCHEMATA](https://docs.snowflake.com/en/sql-reference/info-schema/schemata.html)|Database|||
|[SEQUENCES](https://docs.snowflake.com/en/sql-reference/info-schema/sequences.html)|Database|||
|[STAGES](https://docs.snowflake.com/en/sql-reference/info-schema/stages.html)|Database|✔||
|[TABLE_CONSTRAINTS](https://docs.snowflake.com/en/sql-reference/info-schema/table_constraints.html)|Database|||
|[TABLE_PRIVILEGES](https://docs.snowflake.com/en/sql-reference/info-schema/table_privileges.html)|Database|||
|[TABLE_STORAGE_METRICS](https://docs.snowflake.com/en/sql-reference/info-schema/table_storage_metrics.html)|Database|✔||
|[TABLES](https://docs.snowflake.com/en/sql-reference/info-schema/tables.html)|Database||Displays tables and views.|
|[USAGE_PRIVILEGES](https://docs.snowflake.com/en/sql-reference/info-schema/usage_privileges.html)|Database||Displays privileges on sequences only; to view privileges on other types of objects, use OBJECT_PRIVILEGES.|
|[VIEWS](https://docs.snowflake.com/en/sql-reference/info-schema/views.html)|Database|||
<br>

## List of Table Functions 

|TABLE FUNCTIONS|DATA RETENTION|NOTES|
|---|:---:|------|
|[AUTOMATIC_CLUSTERING_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/automatic_clustering_history.html)|14 days|Results depend on MONITOR USAGE privilege.|
|[AUTO_REFRESH_REGISTRATION_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/auto_refresh_registration_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[COMPLETE_TASK_GRAPHS](https://docs.snowflake.com/en/sql-reference/functions/complete_task_graphs.html)|60 minutes|Results returned only for the ACCOUNTADMIN role, the task owner (i.e. the role with the OWNERSHIP privilege on the task), or a role with the global MONITOR EXECUTION privilege.|
|[COPY_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/copy_history.html)|14 days|Results depend on the privileges assigned to the user’s current role.|
|[CURRENT_TASK_GRAPHS](https://docs.snowflake.com/en/sql-reference/functions/current_task_graphs.html)|N/A|Results returned only for the ACCOUNTADMIN role, the task owner (i.e. the role with the OWNERSHIP privilege on the task), or a role with the global MONITOR EXECUTION privilege.|
|[DATA_TRANSFER_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/data_transfer_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[DATABASE_REFRESH_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/database_refresh_history.html)|14 days|Results depend on the privileges assigned to the user’s current role.|
|[DATABASE_REFRESH_PROGRESS, DATABASE_REFRESH_PROGRESS_BY_JOB](https://docs.snowflake.com/en/sql-reference/functions/database_refresh_progress.html)|14 days|Results depend on the privileges assigned to the user’s current role.|
|[DATABASE_STORAGE_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/database_storage_usage_history.html)|6 months|Results depend on MONITOR USAGE privilege. [1]|
|[EXTERNAL_FUNCTIONS_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/external_functions_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[EXTERNAL_TABLE_FILES](https://docs.snowflake.com/en/sql-reference/functions/external_table_files.html)|N/A|Results depend on the privileges assigned to the user’s current role.|
|[EXTERNAL_TABLE_FILE_REGISTRATION_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/external_table_registration_history.html)|30 days|Results depend on the privileges assigned to the user’s current role.|
|[LOGIN_HISTORY, LOGIN_HISTORY_BY_USER](https://docs.snowflake.com/en/sql-reference/functions/login_history.html)|7 days|Results depend on the privileges assigned to the user’s current role.|
|[MATERIALIZED_VIEW_REFRESH_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/materialized_view_refresh_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[PIPE_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/pipe_usage_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[POLICY_REFERENCES](https://docs.snowflake.com/en/sql-reference/functions/policy_references.html)|N/A|Results returned only for the ACCOUNTADMIN role.|
|[QUERY_ACCELERATION_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/query_acceleration_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[QUERY_HISTORY, QUERY_HISTORY_BY_*](https://docs.snowflake.com/en/sql-reference/functions/query_history.html)|7 days|Results depend on the privileges assigned to the user’s current role.|
|[REPLICATION_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/replication_usage_history.html)|14 days|Results returned only for the ACCOUNTADMIN role.|
|[REST_EVENT_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/rest_event_history.html)|7 days|Results returned only for the ACCOUNTADMIN role.|
|[SEARCH_OPTIMIZATION_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/search_optimization_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[SERVERLESS_TASK_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/serverless_task_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[STAGE_DIRECTORY_FILE_REGISTRATION_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/stage_directory_file_registration_history.html)|14 days|Results depend on the privileges assigned to the user’s current role.|
|[STAGE_STORAGE_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/stage_storage_usage_history.html)|6 months|Results depend on MONITOR USAGE privilege. [1]|
|[TAG_REFERENCES](https://docs.snowflake.com/en/sql-reference/functions/tag_references.html)|N/A|Results are only returned for the role that has access to the specified object.|
|[TAG_REFERENCES_ALL_COLUMNS](https://docs.snowflake.com/en/sql-reference/functions/tag_references_all_columns.html)|N/A|Results are only returned for the role that has access to the specified object.|
|[TASK_DEPENDENTS](https://docs.snowflake.com/en/sql-reference/functions/task_dependents.html)|N/A|Results returned only for the ACCOUNTADMIN role or task owner (role with OWNERSHIP privilege on task).|
|[TASK_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/task_history.html)|7 days|Results returned only for the ACCOUNTADMIN role, the task owner (i.e. the role with the OWNERSHIP privilege on the task), or a role with the global MONITOR EXECUTION privilege.|
|[VALIDATE_PIPE_LOAD](https://docs.snowflake.com/en/sql-reference/functions/validate_pipe_load.html)|14 days|Results depend on the privileges assigned to the user’s current role.|
|[WAREHOUSE_LOAD_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/warehouse_load_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[WAREHOUSE_METERING_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/warehouse_metering_history.html)|6 months|Results depend on MONITOR USAGE privilege. [1]|

<br>

# General Usage Notes
* INFORMATION SCHEMA - read only, i.e. objects cannot be dropped or modified. 
* <mark style="background-color: #89cff0">No guarantee</mark> on consistency with respects to concurrent DDL on INFORMATION SCHEMA. 
* Output of view depends on user's role and associated privs.
* Performance issue error code that can occur ```Information schema query returned too much data. Please repeat query with more selective predicates.```.
<br>

* TIP - INFORMATION SCHEMA optimized for small subsets of objects from dictionary. 

<br>

# Considerations for Replacing SHOW Commands with Information Schema Views
* Same SQL interface provided by SHOW commands. 
* Key differences:

|CONSIDERATIONS|SHOW COMMAND|INFORMATION SCHEMA VIEWS|
|----|----|----|
|Warehouses|Not required to execute.|Warehouse must be running and currently in use to query the views.|
|Pattern matching/filtering|Case-insensitive (when filtering using LIKE).|Standard (case-sensitive) SQL semantics. Snowflake automatically converts unquoted, case-insensitive identifiers to uppercase internally, so unquoted object names must be queried in uppercase in the Information Schema views.|
|Query results|Most SHOW commands limit results to the current schema by default.|Views display all objects in the current/specified database. To query against a particular schema, you must use a filter predicate (e.g. ... WHERE table_schema = CURRENT_SCHEMA()...). Note that Information Schema queries lacking sufficiently selective filters return an error and do not execute (see General Usage Notes in this topic).|

<br>

# Qualifying the Names of Information Schema Views and Table Functions in Queries
* When querying INFORMATION SCHEMA - use qualified name for object or set session parameters. 
* Eg - To query using the fully-qualified names of the view and table function, in the form of database.information_schema.name:
```sql
select * from testdb.information_schema.tables where table_schema = 'PUBLIC' ... ;

select * from table(testdb.information_schema.login_history( ... ));
```
* Eg - To query using the qualified names of the view and table function, in the form of information_schema.name:
```sql
use database testdb;

select * from information_schema.tables where table_schema = 'PUBLIC' ... ;

select * from table(information_schema.login_history( ... ));
```
* Eg - To query with the INFORMATION_SCHEMA schema in use for the session:
```sql
use schema testdb.information_schema;

select * from tables where table_schema = 'PUBLIC' ... ;

select * from table(login_history( ... ));
```