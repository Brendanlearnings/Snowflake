# Access Control Framework 
* Combines usage of the following models:
    * <mark style="background-color: #89cff0">DAC</mark> - Discretionary Access Control.
        * Each object has an owner, who can grant access to that object. 
    * <mark style="background-color: #89cff0">RBAC</mark> - Role Based Access.
        * Access privs are assigned to roles, which are assigned to users. 
* Key concepts for understanding access control in SF:
    * <mark style="background-color: #89cff0">Securable Object</mark> - Entity to which access can be granted.
    * <mark style="background-color: #89cff0">Role</mark> - Entity to which privs are granted. In turn assigned to users. (role - role assignment = role hierarchy).
    * <mark style="background-color: #89cff0">Privileges</mark> - Defined level of access to an object. Multiple distinct privs may be used to control granularity of access. 
    * <mark style="background-color: #89cff0">User</mark> - Identity recognized by SF (person/program).
* SF access model:
    * Access granted to securable objects via privs assigned to roles. 
    * Role to role assignment = hierarchy. User assigned to role. 
    * In addition - each object has an owner who can grant access to other roles. 
    * Designed for flexibility and control.

<br>

![SF Access Model](https://docs.snowflake.com/en/_images/access-control-relationships.png)

<br>

# Securable Object 
* Objects contained in logical containers in hierarchy of containers.
* Top most - customer organization. 
* Securable object - tables, views, functions, etc. contained in schema object - contained in DB. 
* DB - contained in account object. 

<br>

![Securable object hierarchy](https://docs.snowflake.com/en/_images/securable-objects-hierarchy.png)

<br>

* To <mark style="background-color: #89cff0">own</mark> an object means a <mark style="background-color: #89cff0">role</mark> has OWNERSHIP privs on the object. 
* Object owned by single role (default role that created object)
    * Role is assigned to users - effectively have shared control over object. 
* <mark style="background-color: #89cff0">Regular schema</mark> - owner role has all privs on object (ability to grant/revoke).
    * Ownership can be transferred from role to role.
* <mark style="background-color: #89cff0">Managed schema</mark> - object owner loses grant/revoke ability. 
    * Schema owner/role with MANAGE GRANTS privs can grant privs to objects in schema. 
* Ability to perform SQL actions on objects defined by privs granted to active role in <mark style="background-color: #89cff0">user session</mark>. 

<br>

# Roles
* Entities to which privs on securable objects can be granted/revoked.
* Assigned according to business functions.
* User may have multiple roles. 
* Small number of <mark style="background-color: #89cff0">system defined roles</mark>.
    * Cannot be dropped.
    * Privs granted to roles cannot be revoked.
* Role to role grant = role hierarchy.
* Privs inherited by 'above' role from 'below' role.
* NOTE - Role owner does <mark style="background-color: #89cff0">not</mark> inherit privs of the owned role. Role inheritance only possible within role hierarchy.

## System defined roles
* Additional privs can be granted to system defined roles but its <mark style="background-color: #89cff0">not recommended</mark>.
* Privs for these roles related to account management. 
* Best practice to <mark style="background-color: #89cff0">not mix account-management privs and entity managed privs</mark> in the same role. 
* System defined roles:
    * ORGADMIN
        * Role that manages operations at org level. 
            * Create accounts
            * View all account in the org (```SHOW ORGANIZATION ACCOUNTS```) & regions enabled for org. 
            * View usage information across org. 
    * ACCOUNTADMIN
        * Encapsulates SYSADMIN & SECURITYADMIN
        * Top level role in system - limited access & number of users recommended. 
    * SECURITYADMIN
        * Manages any object grants <mark style="background-color: #89cff0">globally</mark>.
        * Create, monitor and manage users and roles. 
            * Granted MANAGE GRANTS security priv to modify grants or revoke it. 
            * Inherits privs of USERADMIN role via hierarchy.
    * USERADMIN
        * User and role management.    
            * Granted ```CREATE USER``` & ```CREATE ROLE``` security privs. 
            * Manages users and roles that it <mark style="background-color: #89cff0">owns</mark>.
    * SYSADMIN
        * Create warehouses and db's. 
        * Within role hierarchy ultimately assign all custom roles to the SYSADMIN role. 
            * Ability to grant privs on warehouses, db's & other objects to other roles. 
    * PUBLIC 
        * Pseudo role - automatically granted to every user in account. 
        * Can own securable objects - by definition objects owned by every other user and role in account.
        * Used in cases where explicit access is <mark style="background-color: #89cff0">not needed</mark>. 

<br>

## Custom roles 
* Can be created by USERADMIN role or higher (```CREATE ROLE``` privs granted).
* Newly created role not assigned to any user, nor granted any roles. 
* RECOMMENDED to create a <mark style="background-color: #89cff0">role hierarchy</mark> to reinforce management of objects in account & restricting management of users and roles.
* Assign top most custom role to SYSADMIN. 
* To enable restriction of user and role - assign roles to USERADMIN. 

<br>

# Privileges
* For existing objects, privs need to be granted on individual objects. 
* Future grants - simplify grant management - define initial set of privs on objects in a schema. 
* Managed by using ``` GRANT <privileges> … TO ROLE and REVOKE <privileges> … FROM ROLE``` command.
    * Regular schema - use of command restricted to role that owns object/role with global ```MANAGE GRANTS``` role.
    * Managed schemas - object owners lose ability to make grant decisions. Only schema owner with ```MANAGE GRANTS``` role can grant privs & future privs on objects in schema. 
        * Centralizing priv management. 

<br>

# Role Hierarchy and Privilege Inheritance 
* Role to role assignment = hierarchy.
* Higher role inherits lower level roles.
![hierarchy](https://docs.snowflake.com/en/_images/system-role-hierarchy.png)

<br>

# Enforcement Model: Primary role and Secondary Roles 
* Every active user session has a "current role" - primary role. Current role determined when user session initiated and based on:
    * Role specified as <mark style="background-color: #89cff0">part of connection</mark> (JDBC/ODBC) & role is role that has been granted to connecting user - becomes current role.
    * No role specified and <mark style="background-color: #89cff0">default role has been set</mark> for connecting user - that role becomes current role.
    * No role specified and <mark style="background-color: #89cff0">default role has not been set</mark> for connecting user - system role PUBLIC is used. 

* Set of secondary roles can be activated in a user session. Aggregated priv grants can then be used (primary + secondary).
    * 2nd roles must be granted to user before activation of session. 
    * Many 2nd roles can be active in a session - exactly one primary role needs to be present.

![primary and secondary role hierarchy](https://docs.snowflake.com/en/_images/primary-secondary-roles-operations.png)