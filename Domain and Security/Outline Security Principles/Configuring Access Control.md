# Configuring Access Control 
How to configure access control for securable objects in your account
<mark style="background-color: #89cff0">
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

