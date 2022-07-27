# Snowflake Information Schema
* aka Data Dictionary consist of a set of system-defined views and table functions that provide extensive metadata information about objects in your account. 
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
* table

|TABLE FUNCTIONS|DATA RETENTION|NOTES|
|---|:---:|------|
|[AUTOMATIC_CLUSTERING_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/automatic_clustering_history.html)|14 days|Results depend on MONITOR USAGE privilege.|
|[AUTO_REFRESH_REGISTRATION_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/auto_refresh_registration_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[COMPLETE_TASK_GRAPHS](https://docs.snowflake.com/en/sql-reference/functions/complete_task_graphs.html)|60 minutes|Results returned only for the ACCOUNTADMIN role, the task owner (i.e. the role with the OWNERSHIP privilege on the task), or a role with the global MONITOR EXECUTION privilege.|
|[COPY_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/copy_history.html)|14 days|Results depend on the privileges assigned to the user’s current role.|
|[CURRENT_TASK_GRAPHS](https://docs.snowflake.com/en/sql-reference/functions/current_task_graphs.html)|N/A|Results returned only for the ACCOUNTADMIN role, the task owner (i.e. the role with the OWNERSHIP privilege on the task), or a role with the global MONITOR EXECUTION privilege.|
|[DATA_TRANSFER_HISTORY](https://docs.snowflake.com/en/sql-reference/functions/data_transfer_history.html)|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[DATABASE_REFRESH_HISTORY]()|14 days|Results depend on the privileges assigned to the user’s current role.|
|[DATABASE_REFRESH_PROGRESS]() , DATABASE_REFRESH_PROGRESS_BY_JOB|14 days|Results depend on the privileges assigned to the user’s current role.|
|[DATABASE_STORAGE_USAGE_HISTORY]()|6 months|Results depend on MONITOR USAGE privilege. [1]|
|[EXTERNAL_FUNCTIONS_HISTORY]()|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[EXTERNAL_TABLE_FILES]()|N/A|Results depend on the privileges assigned to the user’s current role.|
|[EXTERNAL_TABLE_FILE_REGISTRATION_HISTORY]()|30 days|Results depend on the privileges assigned to the user’s current role.|
|[LOGIN_HISTORY]() , LOGIN_HISTORY_BY_USER|7 days|Results depend on the privileges assigned to the user’s current role.|
|[MATERIALIZED_VIEW_REFRESH_HISTORY]()|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[PIPE_USAGE_HISTORY]()|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[POLICY_REFERENCES]()|N/A|Results returned only for the ACCOUNTADMIN role.|
|[QUERY_ACCELERATION_HISTORY]() [2]|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[QUERY_HISTORY]() , QUERY_HISTORY_BY_*|7 days|Results depend on the privileges assigned to the user’s current role.|
|[REPLICATION_USAGE_HISTORY]()|14 days|Results returned only for the ACCOUNTADMIN role.|
|[REST_EVENT_HISTORY]()|7 days|Results returned only for the ACCOUNTADMIN role.|
|[SEARCH_OPTIMIZATION_HISTORY]()|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[SERVERLESS_TASK_HISTORY]()|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[STAGE_DIRECTORY_FILE_REGISTRATION_HISTORY]()|14 days|Results depend on the privileges assigned to the user’s current role.|
|[STAGE_STORAGE_USAGE_HISTORY]()|6 months|Results depend on MONITOR USAGE privilege. [1]|
|[TAG_REFERENCES]()|N/A|Results are only returned for the role that has access to the specified object.|
|[TAG_REFERENCES_ALL_COLUMNS]()|N/A|Results are only returned for the role that has access to the specified object.|
|[TASK_DEPENDENTS]()|N/A|Results returned only for the ACCOUNTADMIN role or task owner (role with OWNERSHIP privilege on task).|
|[TASK_HISTORY]()|7 days|Results returned only for the ACCOUNTADMIN role, the task owner (i.e. the role with the OWNERSHIP privilege on the task), or a role with the global MONITOR EXECUTION privilege.|
|[VALIDATE_PIPE_LOAD]()|14 days|Results depend on the privileges assigned to the user’s current role.|
|[WAREHOUSE_LOAD_HISTORY]()|14 days|Results depend on MONITOR USAGE privilege. [1]|
|[WAREHOUSE_METERING_HISTORY]()|6 months|Results depend on MONITOR USAGE privilege. [1]|