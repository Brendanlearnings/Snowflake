# Topic quick links 
* [Account Usage](#account-usage)
* [Overview of Account Usage Schemas](#overview-of-account-usage-schemas)
    * [ACCOUNT_USAGE](#accountusage)
    * [READER_ACCOUNT_USAGE](#readeraccountusage)
* [Differences between Account Usage and Information Schema](#differences-between-account-usage-and-information-schema)
    * [Dropped Object Records](#dropped-object-records)
    * [Data Latency](#data-latency)
    * [Historical Data Retention](#historical-data-retention)
* [ACCOUNT_USAGE Views](#accountusage-views)
    * [Account Usage Table Functions](#account-usage-table-functions)
* [READER_ACCOUNT_USAGE Views](#readeraccountusage-views)
* [Enabling Snowflake DB Usage for Other Roles](#enabling-snowflake-db-usage-for-other-roles)
* [Querying the Account Usage Views](#querying-the-account-usage-views)
    * [User Login Metrics](#user-login-metrics)
    * [Warehouse Performance](#warehouse-performance)
    * [Warehouse Credit Usage](#warehouse-credit-usage)
    * [Data Storage Usage](#data-storage-usage)
    * [User Query Totals and Execution Times](#user-query-totals-and-execution-times)
    * [Obtain a Query Count for Every Login Event](#obtain-a-query-count-for-every-login-event)




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
|[DATA_TRANSFER_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/data_transfer_history.html)|Historical|2 hours|Data retained for 1 year.|
|[FILE_FORMATS](https://docs.snowflake.com/en/sql-reference/account-usage/file_formats.html)|Object|2 hours||
|[FUNCTIONS](https://docs.snowflake.com/en/sql-reference/account-usage/functions.html)|Object|2 hours||
|[GRANTS_TO_ROLES](https://docs.snowflake.com/en/sql-reference/account-usage/grants_to_roles.html)|Object|2 hours||
|[GRANTS_TO_USERS](https://docs.snowflake.com/en/sql-reference/account-usage/grants_to_users.html)|Object|2 hours||
|[LOAD_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/load_history.html)|Historical|90 minutes [2]|Data retained for 1 year.||
|[LOGIN_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/load_history.html)|Historical|2 hours|Data retained for 1 year.|
|[MASKING_POLICIES](https://docs.snowflake.com/en/sql-reference/account-usage/masking_policies.html)|Object|2 hours||
|[MATERIALIZED_VIEW_REFRESH_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/materialized_view_refresh_history.html)|Historical|3 hours|Data retained for 1 year.|
|[METERING_DAILY_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/metering_daily_history.html)|Historical|3 hours|Data retained for 1 year.|
|[METERING_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/metering_history.html)|Historical|3 hours|Data retained for 1 year.|
|[OBJECT_DEPENDENCIES](https://docs.snowflake.com/en/sql-reference/account-usage/object_dependencies.html)|Historical|3 hours||
|[PIPES](https://docs.snowflake.com/en/sql-reference/account-usage/pipes.html)|Object|2 hours||
|[PIPE_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/pipe_usage_history.html)|Historical|3 hours|Data retained for 1 year.||
|[POLICY_REFERENCES](https://docs.snowflake.com/en/sql-reference/account-usage/policy_references.html)|Object|2 hours||
|[QUERY_ACCELERATION_ELIGIBLE](https://docs.snowflake.com/en/sql-reference/account-usage/query_acceleration_eligible.html)|Historical|3 hours|Data retained for 1 year.|
|[QUERY_ACCELERATION_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/query_acceleration_history.html)|Historical|3 hours|Available as part of the Query Acceleration Service preview. Data retained for 1 year.|
|[QUERY_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/query_history.html)|Historical|45 minutes|Data retained for 1 year.|
|[REFERENTIAL_CONSTRAINTS](https://docs.snowflake.com/en/sql-reference/account-usage/referential_constraints.html)|Object|2 hours||
|[REPLICATION_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/replication_usage_history.html)|Historical|3 hours|Data retained for 1 year.|
|[ROLES](https://docs.snowflake.com/en/sql-reference/account-usage/roles.html)|Object|2 hours||
|[ROW_ACCESS_POLICIES](https://docs.snowflake.com/en/sql-reference/account-usage/row_access_policies.html)|Object|2 hours||
|[SCHEMATA](https://docs.snowflake.com/en/sql-reference/account-usage/schemata.html)|Object|2 hours||
|[SEARCH_OPTIMIZATION_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/search_optimization_history.html)|Historical|3 hours|Data retained for 1 year.|
|[SEQUENCES](https://docs.snowflake.com/en/sql-reference/account-usage/sequences.html)|Object|2 hours||
|[SERVERLESS_TASK_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/serverless_task_history.html)|Historical|3 hours|Data retained for 1 year.|
|[SESSIONS](https://docs.snowflake.com/en/sql-reference/account-usage/sessions.html)|Historical|3 hours|Data retained for 1 year.|
|[STAGES](https://docs.snowflake.com/en/sql-reference/account-usage/stages.html)|Object|2 hours||
|[STAGE_STORAGE_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/stage_storage_usage_history.html)|Historical|2 hours|Data retained for 1 year.|
|[STORAGE_USAGE](https://docs.snowflake.com/en/sql-reference/account-usage/storage_usage.html)|Historical|2 hours|Combined usage across all database tables and internal stages. Data retained for 1 year.|
|[TABLES](https://docs.snowflake.com/en/sql-reference/account-usage/tables.html)|Object|90 minutes||
|[TABLE_CONSTRAINTS](https://docs.snowflake.com/en/sql-reference/account-usage/table_constraints.html)|Object|2 hours||
|[TABLE_STORAGE_METRICS](https://docs.snowflake.com/en/sql-reference/account-usage/table_storage_metrics.html)|Object|90 minutes|
|[TAG_REFERENCES](https://docs.snowflake.com/en/sql-reference/account-usage/tag_references.html)|Object|2 hours||
|[TAGS](https://docs.snowflake.com/en/sql-reference/account-usage/tags.html)|Object|2 hours||
|[TASK_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/task_history.html)|Historical|45 minutes||
|[USERS](https://docs.snowflake.com/en/sql-reference/account-usage/users.html)|Object|2 hours||
|[VIEWS](https://docs.snowflake.com/en/sql-reference/account-usage/views.html)|Object|90 minutes|
|[WAREHOUSE_EVENTS_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/warehouse_events_history.html)|Historical|3 hours|Data retained for 1 year.|
|[WAREHOUSE_LOAD_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/warehouse_load_history.html)|Historical|3 hours|Data retained for 1 year.|
|[WAREHOUSE_METERING_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/warehouse_metering_history.html)|Historical|3 hours|Data retained for 1 year.|

* Notes:
    * Latency is approximated.
    * For tables with 32 or fewer DML statements/100 or less rows - latency can be up to a day. 

<br>

## Account Usage Table Functions
* At current SF supports one ACCOUNT_USAGE table function.

|TABLE FUNCTION|DATA RETENTION|NOTES|
|--------------|:------------:|-----|
|[TAG_REFERENCES_WITH_LINEAGE](https://docs.snowflake.com/en/sql-reference/functions/tag_references_with_lineage.html)|N/A|Results are only returned for the role that has access to the specified object.|

<br>

# READER_ACCOUNT_USAGE Views
* READER_ACCOUNT_USAGE contains the following views:

|VIEW|TYPE|LATENCY|NOTES|
|---|:---:|---|---|
|[LOGIN_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/login_history.html)|Historical|2 hours|Data retained for 1 year.|
|[QUERY_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/query_history.html)|Historical|45 minutes|Data retained for 1 year.|
|[RESOURCE_MONITORS](https://docs.snowflake.com/en/sql-reference/account-usage/resource_monitors.html)|Object|2 hours|
|[STORAGE_USAGE](https://docs.snowflake.com/en/sql-reference/account-usage/storage_usage.html)|Historical|2 hours|Combined usage across all database tables and internal stages. Data retained for 1 year.|
|[WAREHOUSE_METERING_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/warehouse_metering_history.html)|Historical|3 hours|Data retained for 1 year.|

<br>

# Enabling Snowflake DB Usage for Other Roles
* By default the SNOWFLAKE DB is available only to the AccountAdmin role. 
* Use AccountAdmin role to grant the following data sharing privs to the desired roles.
    * Makes use of <mark style="background-color: #89cff0">imported privileges</mark>.

```sql
use role accountadmin;

grant imported privileges on database snowflake to role sysadmin;
grant imported privileges on database snowflake to role customrole1;

use role customrole1;

select * from snowflake.account_usage.databases;
```

<br>

# Querying the Account Usage Views 
* Prerequisite schema set (previous examples grants apply)
```sql
use role accountadmin;

use schema snowflake.account_usage;
```
<br>

## User Login Metrics

* Eg - Average number of seconds between failed login attempts by user (month-to-date)
```sql
select user_name,
       count(*) as failed_logins,
       avg(seconds_between_login_attempts) as average_seconds_between_login_attempts
from (
      select user_name,
             timediff(seconds, event_timestamp, lead(event_timestamp)
                 over(partition by user_name order by event_timestamp)) as seconds_between_login_attempts
      from login_history
      where event_timestamp > date_trunc(month, current_date)
      and is_success = 'NO'
     )
group by 1
order by 3;
```
* Eg - Failed logins by user (month-to-date):
```sql
select user_name,
       sum(iff(is_success = 'NO', 1, 0)) as failed_logins,
       count(*) as logins,
       sum(iff(is_success = 'NO', 1, 0)) / nullif(count(*), 0) as login_failure_rate
from login_history
where event_timestamp > date_trunc(month, current_date)
group by 1
order by 4 desc;
```
<br>

## Warehouse Performance 
* This query calculates virtual warehouse performance metrics such as throughput and latency for 15-minute time intervals over the course of one day.
* Replace current_warehouse() with desired warehouse.
```sql
with
params as (
select
    current_warehouse() as warehouse_name,
    '2021-11-01' as time_from,
    '2021-11-02' as time_to
),

jobs as (
select
    query_id,
    time_slice(start_time::timestamp_ntz, 15, 'minute','start') as interval_start,
    qh.warehouse_name,
    database_name,
    query_type,
    total_elapsed_time,
    compilation_time as compilation_and_scheduling_time,
    (queued_provisioning_time + queued_repair_time + queued_overload_time) as queued_time,
    transaction_blocked_time,
    execution_time
from snowflake.account_usage.query_history qh, params
where
    qh.warehouse_name = params.warehouse_name
and start_time >= params.time_from
and start_time <= params.time_to
and execution_status = 'SUCCESS'
and query_type in ('SELECT','UPDATE','INSERT','MERGE','DELETE')
),

interval_stats as (
select
    query_type,
    interval_start,
    count(distinct query_id) as numjobs,
    median(total_elapsed_time)/1000 as p50_total_duration,
    (percentile_cont(0.95) within group (order by total_elapsed_time))/1000 as p95_total_duration,
    sum(total_elapsed_time)/1000 as sum_total_duration,
    sum(compilation_and_scheduling_time)/1000 as sum_compilation_and_scheduling_time,
    sum(queued_time)/1000 as sum_queued_time,
    sum(transaction_blocked_time)/1000 as sum_transaction_blocked_time,
    sum(execution_time)/1000 as sum_execution_time,
    round(sum_compilation_and_scheduling_time/sum_total_duration,2) as compilation_and_scheduling_ratio,
    round(sum_queued_time/sum_total_duration,2) as queued_ratio,
    round(sum_transaction_blocked_time/sum_total_duration,2) as blocked_ratio,
    round(sum_execution_time/sum_total_duration,2) as execution_ratio,
    round(sum_total_duration/numjobs,2) as total_duration_perjob,
    round(sum_compilation_and_scheduling_time/numjobs,2) as compilation_and_scheduling_perjob,
    round(sum_queued_time/numjobs,2) as queued_perjob,
    round(sum_transaction_blocked_time/numjobs,2) as blocked_perjob,
    round(sum_execution_time/numjobs,2) as execution_perjob
from jobs
group by 1,2
order by 1,2
)
select * from interval_stats;
```
<br>

## Warehouse Credit Usage
* Eg - Credits used by each warehouse in your account (month-to-date):
```sql
select warehouse_name,
       sum(credits_used) as total_credits_used
from warehouse_metering_history
where start_time >= date_trunc(month, current_date)
group by 1
order by 2 desc;
```

* Eg - Credits used over time by each warehouse in your account (month-to-date):
```sql
select start_time::date as usage_date,
       warehouse_name,
       sum(credits_used) as total_credits_used
from warehouse_metering_history
where start_time >= date_trunc(month, current_date)
group by 1,2
order by 2,1;
```
<br>

## Data Storage Usage 
* Eg - Billable terabytes stored in your account over time:
```sql
select date_trunc(month, usage_date) as usage_month,
       avg(storage_bytes + stage_bytes + failsafe_bytes) / power(1024, 4) as billable_tb
from storage_usage
group by 1
order by 1;
```
<br>

## User Query Totals and Execution Times
* Eg - Total jobs executed in your account (month-to-date):
```sql
select count(*) as number_of_jobs
from query_history
where start_time >= date_trunc(month, current_date);
```
* Total jobs executed by each warehouse in your account (month-to-date):
```sql
select warehouse_name,
       count(*) as number_of_jobs
from query_history
where start_time >= date_trunc(month, current_date)
group by 1
order by 2 desc;
```
* Average query execution time by user (month-to-date):
```sql
select user_name,
       avg(execution_time) as average_execution_time
from query_history
where start_time >= date_trunc(month, current_date)
group by 1
order by 2 desc;
```
* Average query execution time by query type and warehouse size (month-to-date):
```sql
select query_type,
       warehouse_size,
       avg(execution_time) as average_execution_time
from query_history
where start_time >= date_trunc(month, current_date)
group by 1,2
order by 3 desc;
```
<br>

## Obtain a Query Count for Every Login Event
* Eg - query count for each user login event.
```sql
select l.user_name,
       l.event_timestamp as login_time,
       l.client_ip,
       l.reported_client_type,
       l.first_authentication_factor,
       l.second_authentication_factor,
       count(q.query_id)
from snowflake.account_usage.login_history l
join snowflake.account_usage.sessions s on l.event_id = s.login_event_id
join snowflake.account_usage.query_history q on q.session_id = s.session_id
group by 1,2,3,4,5,6
order by l.user_name
;
```