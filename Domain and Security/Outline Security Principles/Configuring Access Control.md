# Topic quick links
* [Account Administration](#account-administration)
    * [Designating Additional Users as Account Administrators](#designating-additional-users-as-account-administrators)
    * [Enabling MFA for each Account Administrator](#enabling-mfa-for-each-account-administrator)
* [Creating Custom Roles](#creating-custom-roles)
    * [Create a role](#create-a-role)
    * [Grant Privileges to the Role](#grant-privileges-to-the-role)
    * [Grant Role to Users](#grant-role-to-users)
* [Creating Custom Read-Only Roles](#creating-custom-read-only-roles)
* [Creating a Role Hierarchy](#creating-a-role-hierarchy)
    * [Grant a role to Another Role](#grant-a-role-to-another-role)
* [Viewing Granted Privileges](#viewing-granted-privileges)
* [Assigning Future Grants on Objects](#assigning-future-grants-on-objects)
    * [Considerations](#considerations)
    * [Defining Future Grants on DB or Schema Objects](#defining-future-grants-on-db-or-schema-objects)
* [Defining Future Grants on Existing DB or Schema Objects](#defining-future-grants-on-existing-db-or-schema-objects)
    * [Revoking Future Grants on DB or Schema Objects](#revoking-future-grants-on-db-or-schema-objects)
    * [Managing Future Grants Using the Web Interface](#managing-future-grants-using-the-web-interface)
    * [Restrictions and limitations](#restrictions-and-limitations)
* [Enabling Non-Account Admin to Monitor Usage and Billing History in Classic Web Interface](#enabling-non-account-admin-to-monitor-usage-and-billing-history-in-classic-web-interface)

# Configuring Access Control 
How to configure access control for securable objects in your account

<br>

# Account Administration 
## Designating Additional Users as Account Administrators 
* **Strongly** recommended that more than one AccountAdmin assigned
    * Ensures account will always have at least one user to perform account-level tasks (to prevent lock out because of lost password)
* Make sure designated users has the following specified:
    * Grant the ACCOUNTADMIN role to the user(s), but <mark style="background-color: #89cff0">do not</mark> set this role as their default.
    * Use <mark style="background-color: #89cff0">custom role</mark> or lower level admin role - <mark style="background-color: #89cff0">SYSADMIN</mark>.
    * Ensure email address specified. 
* EG:
    ```
    grant role accountadmin, sysadmin to user user2;

    alter user user2 set email='user2@domain.com', default_role=sysadmin;
    ```
<br>

## Enabling MFA for each Account Administrator 
* **Strongly** recommend all users who can modify/view sensitive data to use MFA for login. 
* Especially <mark style="background-color: #89cff0">ACCOUNTADMIN</mark>, <mark style="background-color: #89cff0">SECURITYADMIN</mark>, <mark style="background-color: #89cff0">SYSADMIN</mark>

<br>

# Creating Custom Roles
* Follow principle of <mark style="background-color: #89cff0">least privileges</mark>.
* Align privs with business functions.
* General workflow:
    * Create custom role.
    * Grant a set of privs to role.
    * Grant role to users 
    * Grant role to another role to create role hierarchy - <mark style="background-color: #89cff0">highly recommended</mark>.

<br>


## Create a role 
* Only USERADMIN or higher (or another role with CREATE ROLE privs) can create roles.
* Eg:
```
create role r1
   comment = 'This role has all privileges on schema_1';
```

## Grant Privileges to the Role
* Only role with <mark style="background-color: #89cff0">global</mark> MANAGE GRANTS privs or higher role can execute command.
* By default only SECURITYADMIN role has MANAGE GRANTS privs
* Eg:
```
grant usage
  on warehouse w1
  to role r1;

grant usage
  on database d1
  to role r1;

grant usage
  on schema d1.s1
  to role r1;

grant select
  on table d1.s1.t1
  to role r1;
  ```

## Grant Role to Users
* Only role with MANAGE GRANTS priv or higher can execute command.
* By default only the SECURITYADMIN has privs.
* Eg:
```
grant role r1
   to user smith;
```

* Optionally you can set new custom role as the default role for the user. 
    * Next time the user logs in the default role will automatically activate in the session
* Eg:
```
alter user smith
   set default_role = r1;
```
<br>

# Creating Custom Read-Only Roles

* The following privs are needed for a read-only role:

|PRIVILEGES|OBJECT|NOTES|
|----------|:-----:|----|
|USAGE|Warehouse|To query an object (e.g. a table or view), a role must have the USAGE privilege on a warehouse. The warehouse provides the compute resources to execute the query.|
|SELECT|Table|To operate on any object in a schema, a role must have the USAGE privilege on the container database and schema.|
* The grant statements are:
```
grant usage
  on database d1
  to role read_only;

grant usage
  on schema d1.s1
  to role read_only;

grant select
  on all tables in schema d1.s1
  to role read_only;

grant usage
  on warehouse w1
  to role read_only;
```
<br>

* The ```GRANT SELECT ON ALL TABLES IN SCHEMA``` statement grant the select privs on all **existing** tables only. 
    * To grant select privs on **future tables**
    * ```grant select on future tables in schema d1.s1 to role read_only;```

<br>

# Creating a Role Hierarchy 
* Recommended approach.
* In general <mark style="background-color: #89cff0">SYSADMIN</mark> role works well as the role all other roles are assigned to in a hierarchy. 
* Create role hierarchy by granting a role to a second role.
    * NB! - role inheritance 
* Eg:
![Role hierarchy](https://docs.snowflake.com/en/_images/system-role-hierarchy-with-privileges.png)

<br>

## Grant a role to Another Role
* As easy as assigning the lower level role to a higher level one.
* Eg:
```
grant role r1
   to role sysadmin;
```

<br>

# Viewing Granted Privileges
* Executing command on object <mark style="background-color: #89cff0">requires same object privs</mark> as running SHOW command for the object type. 
    * Running SHOW GRANTS on a table requires the following privs on the table and container database and schema. 
    * ```
         Database
         USAGE

         Schema
         USAGE

         Table
         any privilege
        ```
* Eg of the SHOW GRANTS command:
```show grants on schema <database_name>.<schema_name>;```
* SHOW GRANTS command can also be used to view current set of privs granted to a role or the current set of roles granted to a user.
* ```
  show grants to role <role_name>;
  show grants to user <user_name>;
  ```
<br>

# Assigning Future Grants on Objects
* Allows initial set of privs to be granted to future objects of a certain <mark style="background-color: #89cff0">type</mark> in a db or schema.
* Only initial set of privs granted to new objects, i.e. other privs can be granted afterwards. 

<br>

## Considerations 
* Future grants defined for object at DB level apply to all objects of that type created in the future. 
* Must define future grant on each object type (schema, tables, views, streams, etc.) <mark style="background-color: #89cff0">individually</mark>.
* Privs defined by future grants automatically granted at object creation time.
* Schema level grants take <mark style="background-color: #89cff0">precedence</mark> over DB level grants, i.e. grants at schema level cause DB level grants to be ignored. 
* DB level grants apply to both regular and managed access schemas.

<br>

## Defining Future Grants on DB or Schema Objects
* Differentiation between schema and DB object levels for future grants 
* Eg - future grant at DB level
```
use role accountadmin;

-- Grant the USAGE privilege on all future schemas in a database to role r1
grant usage on future schemas in database d1 to role r1;
```
* Eg - future grant on schema level
```
use role accountadmin;

-- Grant the SELECT privilege on all future tables in a schema to role r1
grant select on future tables in schema d1.s1 to role r1;

-- Grants the SELECT and INSERT privileges on all future tables in a schema to r1
grant select,insert on future tables in schema d1.s1 to role r1;
```

<br>

# Defining Future Grants on Existing DB or Schema Objects
* Future grants only <mark style="background-color: #89cff0">pertain to new objects</mark>.
* <mark style="background-color: #89cff0">Explicitly</mark> grant the desired privs to a role on existing objects.
* Eg:
```
use role accountadmin;

-- Grant the USAGE privilege on all existing schemas in a database to role r1
grant usage on all schemas in database d1 to role r1;

-- Grant the SELECT privilege on all existing tables in a schema to role r1
grant select on all tables in schema d1.s1 to role r1
```

<br>

## Revoking Future Grants on DB or Schema Objects
* Future grants can be revoked through using the ```REVOKE <privileges> … FROM ROLE``` command with the future keyword. 
* Revoking future grants on DB objects only removes privs granted on the future objects of a specified type rather than existing objects. 
* Any privs granted on existing objects are retained. 
* Eg:
```
use role accountadmin;

-- Revoke the USAGE privilege on all existing schemas in a database from role r1
revoke usage on all schemas in database d1 from role r1;

-- Revoke the SELECT and INSERT privileges on tables in a schema from the role r1
revoke select,insert on future tables in schema d1.s1 from role r1;
```

<br>

## Managing Future Grants Using the Web Interface 
* Please refer to [link](https://docs.snowflake.com/en/user-guide/security-access-control-configure.html#managing-future-grants-using-the-web-interface) to see explanation. 

## Restrictions and limitations 
* For future grants on DB & Schema level.
    * <mark style="background-color: #89cff0">Not supported</mark> for:
        * Data Sharing
        * Data replication 
    * Future grants are supported on named stages with the following restrictions:
        * WRITE privs cannot be specified without the READ privs.
        * READ priv cannot be revoked if the WRITE priv is present.
        * Internal stages - only future grants with the READ and WRITE privs are materialized.
        * External stage - only future grants with the USAGE privs are materialized. 
    * Future grants are <mark style="background-color: #89cff0">not</mark> applied when renaming or swapping a table.
    * No more than one future grants of OWNERSHIP priv is allowed on each securable object type. 
    * DB level future grants of OWNERSHIP privs on the objects of managed access schemas in the db are not affected. 
    * When DB cloned - schemas in DB copy the future privs from the source schemas. 
        * Maintains consistency with regular object grants - grants of the source object (i.e. DB) are not copied to the clone, but grant on all children objects (i.e. schemas in db) are copied to the clone. 

<br>

# Enabling Non-Account Admin to Monitor Usage and Billing History in Classic Web Interface 
* Please refer to [link](https://docs.snowflake.com/en/user-guide/security-access-control-configure.html#enabling-non-account-administrators-to-monitor-usage-and-billing-history-in-the-classic-web-interface) for instructions on using Web interface. 