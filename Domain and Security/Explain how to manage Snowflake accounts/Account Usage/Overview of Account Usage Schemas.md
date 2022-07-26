# Account Usage 
* In SF DB
    * ACCOUNT_USAGE & READER_ACCOUNT_USAGE schemas enable querying object metadata, historical usage data for all reader accounts associated with 'main' account.

<br>

# Overview of Account Usage Schemas 
## ACCOUNT_USAGE
* Views display <mark style="background-color: #89cff0">object metadata</mark> and <mark style="background-color: #89cff0">usage metrics</mark> for your **account**.
* Mirror information schema but with the following difference:
    * Records the dropped objects included in each view.
    * Longer retention time for historical usage data.
    * Data latency. 

<br>

## READER_ACCOUNT_USAGE
* Views display <mark style="background-color: #89cff0">object metadata</mark> of the ACCOUNT_USAGE views that apply to **reader accounts**.
* <mark style="background-color: #89cff0">RESOURCE_MONITOR</mark> view only available here.
* Small subset of ACCOUNT_USAGE view 
* Views empty if no reader accounts have been created by account. 

<br>

# Differences between Account Usage and Information Schema
* Utilizes identical structures and naming conventions.
* Key differences:

|DIFFERENCE|ACCOUNT USAGE|INFORMATION SCHEMA|
|----------|:-----------:|------------------|
|Includes dropped objects|Yes|No|
|Latency of data|From 45 minutes to 3 hours (varies by view)|None|
|Retention of historical data|1 Year|From 7 days to 6 months (varies by view/table function)|

<br>

## Dropped Object Records
* Account usage objects include all objects that have been dropped. 
* Make use of ID columns when similarly named object is dropped and recreated. 

<br>

## Data Latency 
* Natural latency since data needs to be extracted from SF internal metadata store. 
* Information schema does not have this latency.
* Can take significant amount of time to retrieve data (up to 2h)

<br>

## Historical Data Retention 
* Up to a year of historical data retention. 
* Significantly less for Information Schema. 

<br>

# ACCOUNT_USAGE Views
|VIEW|TYPE|LATENCY|NOTES|
|---|:---:|:---:|---|
|[ACCESS_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/access_history.html)|Historical|3 hours|Data retained for 1 year.|
|[AUTOMATIC_CLUSTERING_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/automatic_clustering_history.html)|Historical|3 hours|Data retained for 1 year.|
|[COLUMNS](https://docs.snowflake.com/en/sql-reference/account-usage/columns.html)|Object|90 minutes||
|[COMPLETE_TASK_GRAPHS](https://docs.snowflake.com/en/sql-reference/account-usage/complete_task_graphs.html)|Historical|45 minutes|Data retained for 1 year.|
|[COPY_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/copy_history.html)|Historical|2 hours [2]|Data retained for 1 year.|
|[DATABASES](https://docs.snowflake.com/en/sql-reference/account-usage/databases.html)|Object|3 hours||
|[DATABASE_STORAGE_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/database_storage_usage_history.html)|Historical|3 hours|Data retained for 1 year.|
|DATA_TRANSFER_HISTORY|Historical|2 hours|Data retained for 1 year.|
|FILE_FORMATS|Object|2 hours||
|FUNCTIONS|Object|2 hours||
|GRANTS_TO_ROLES|Object|2 hours||
|GRANTS_TO_USERS|Object|2 hours||
|LOAD_HISTORY|Historical|90 minutes [2]|Data retained for 1 year.||
|LOGIN_HISTORY|Historical|2 hours|Data retained for 1 year.|
|MASKING_POLICIES|Object|2 hours||
|MATERIALIZED_VIEW_REFRESH_HISTORY|Historical|3 hours|Data retained for 1 year.|
|METERING_DAILY_HISTORY|Historical|3 hours|Data retained for 1 year.|
|METERING_HISTORY|Historical|3 hours|Data retained for 1 year.|
|OBJECT_DEPENDENCIES|Historical|3 hours||
|PIPES|Object|2 hours||
|PIPE_USAGE_HISTORY|Historical|3 hours|Data retained for 1 year.||
|POLICY_REFERENCES|Object|2 hours||
|QUERY_ACCELERATION_ELIGIBLE|Historical|3 hours|Data retained for 1 year.|
|QUERY_ACCELERATION_HISTORY|Historical|3 hours|Available as part of the Query Acceleration Service preview. Data retained for 1 year.|
|QUERY_HISTORY|Historical|45 minutes|Data retained for 1 year.|
|REFERENTIAL_CONSTRAINTS|Object|2 hours||
|REPLICATION_USAGE_HISTORY|Historical|3 hours|Data retained for 1 year.|
|ROLES|Object|2 hours||
|ROW_ACCESS_POLICIES|Object|2 hours||
|SCHEMATA|Object|2 hours||
|SEARCH_OPTIMIZATION_HISTORY|Historical|3 hours|Data retained for 1 year.|
|SEQUENCES|Object|2 hours||
|SERVERLESS_TASK_HISTORY|Historical|3 hours|Data retained for 1 year.|
|SESSIONS|Historical|3 hours|Data retained for 1 year.|
|STAGES|Object|2 hours||
|STAGE_STORAGE_USAGE_HISTORY|Historical|2 hours|Data retained for 1 year.|
|STORAGE_USAGE|Historical|2 hours|Combined usage across all database tables and internal stages. Data retained for 1 year.|
|TABLES|Object|90 minutes||
|TABLE_CONSTRAINTS|Object|2 hours||
|TABLE_STORAGE_METRICS|Object|90 minutes|
|TAG_REFERENCES|Object|2 hours||
|TAGS|Object|2 hours||
|TASK_HISTORY|Historical|45 minutes||
|USERS|Object|2 hours||
|VIEWS|Object|90 minutes|
|WAREHOUSE_EVENTS_HISTORY|Historical|3 hours|Data retained for 1 year.|
|WAREHOUSE_LOAD_HISTORY|Historical|3 hours|Data retained for 1 year.|
|WAREHOUSE_METERING_HISTORY|Historical|3 hours|Data retained for 1 year.|

* Notes:
    * Latency is approximated.
    * For tables with 32 or fewer DML statements/100 or less rows - latency can be up to a day. 

<br>

## Account Usage Table Functions
* At current SF supports one ACCOUNT_USAGE table function.

|TABLE FUNCTION|DATA RETENTION|NOTES|
|--------------|:------------:|-----|
|TAG_REFERENCES_WITH_LINEAGE|N/A|Results are only returned for the role that has access to the specified object.|

<br>

# READER_ACCOUNT_USAGE
* READER_ACCOUNT_USAGE contains the following views:

|VIEW|TYPE|LATENCY|NOTES|
|---|:---:|---|---|
|LOGIN_HISTORY|Historical|2 hours|Data retained for 1 year.|
|QUERY_HISTORY|Historical|45 minutes|Data retained for 1 year.|
|RESOURCE_MONITORS|Object|2 hours|
|STORAGE_USAGE|Historical|2 hours|Combined usage across all database tables and internal stages. Data retained for 1 year.|
|WAREHOUSE_METERING_HISTORY|Historical|3 hours|Data retained for 1 year.|