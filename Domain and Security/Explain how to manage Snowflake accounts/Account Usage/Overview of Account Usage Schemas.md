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