---
layout: default
title: Basic Keycloak Configuration
parent: Keycloak
description: "Managing authentication and authorization for web application and services."
nav_order: 2
---

# Basic Keycloak Configuration (Authentication and Authorization)
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Create a realm

A realm in Keycloak is the equivalent of a tenant. 
It allows creating isolated groups of applications and users. 
By default, there is a single realm in Keycloak called `master`. 
This is dedicated to manage Keycloak and should not be used for your own applications.

Let’s create our first realm.
1. Open the [Keycloak Admin Console][keycloak-url]
1. Hover the mouse over the dropdown in the top-left corner where it says `Master`,   
then click on `Add realm`
1. Fill in the form with the following values:
   * Name: `public-library`
1. Click `Create`

![Add Realm](/assets/keycloak/add-realm.png)

## Create User Federation

Initially there are no user federation in a new realm, so let’s create one:

1. Open the [Keycloak Admin Console][keycloak-url]
1. Click `User Federation` (left-hand menu)
   * Select `ldap` (drop-down list)
1. Fill in the form with the following values:
   * Enabled: `ON`
   * Console Display Name: `ldap`
   * Import Users: `ON`
   * Edit Mode: `WRITABLE`
   * Sync Registrations: `ON`
   * Vendor: `Active Directory`
   * Username LDAP attribute: `sAMAccountName`
   * RDN LDAP attribute: `cn` (default value)
   * UUID LDAP attribute: `objectGUID` (default value)
   * User Object Classes: `person, organizationalPerson, user` (default value)
   * Connection URL: `ldap://MyDomain.com:389` (port 389, port 636 for secure)
   * Users DN: `OU=Users,OU=Organizational,DC=MyDomain,DC=com` (usually CN=Users,DC=MyDomain,DC=com)
   * Bind Type: `simple`
   * Bind DN: `username@MyDomain.com` (usually CN=keycloak admin,OU=Users,OU=Organizational,DC=ad,DC=test,DC=com) (needs to have the necessary read/write access)
   * Bind Credential: `<bind dn password>`
   * Validate Password Policy: `ON`
1. Click `Save`

The ldap group attribute will need to be included. To do this:

1. Click `User Federation` (left-hand menu)
   * Click `Edit` (top-right button under actions)
   * Click `Mapper` (top of the page)
1. Click `Create` (top-right corner of table)
1. Fill in the form with the following values:
   * Name: `MemberOf`
   * LDAP Groups DN: `OU=Security Groups,OU=Organizational,DC=MyDomain,DC=com` (usually OU=groups,DC=MyDomain,DC=com)
1. Click `Save`

## Create Roles

Define a role with different security policies for resource accessibility: a `Member` role with normal privileges 
and a `Librarian` role with administrative privileges, so let's create it:
1. Click `Roles` (left-hand menu)
   * Click `Add Role` (top-right corner of table)
1. Fill in the form with the following values:
   * Role Name: `Member`
   * Description: `normal privileges`
1. Click `Save`

1. Click `Roles` (left-hand menu)
   * Click `Add Role` (top-right corner of table)
1. Fill in the form with the following values:
   * Role Name: `Librarian`
   * Description: `administrative privileges`
1. Click `Save`

![Add Realm](/assets/keycloak/add-role.png)

To have the `Librarian` role automatically include the `Member` role when assigned, the `Composite Roles` option needs to be enabled:
1. Click `Roles` (left-hand menu)
   * Click `Edit` (top-right button under actions)
1. Toggle `Composite Roles`
1. Choose `Member` role from the `Available Roles`
1. Click `Add selected`

![Add Realm](/assets/keycloak/add-composite-roles.png)

The default role will need to be setup to have new users assigned the `Member` role automatically. To do this:
1. Click `Roles` (left-hand menu)
   * Click `Default Roles` (top of the page)
1. Choose `Member` role from the `Available Roles`
1. Click `Add selected`

## Create a user

Initially there are no users in a new realm, so let’s create one:

1. Open the [Keycloak Admin Console][keycloak-url]
1. Click `Users` (left-hand menu)
   * Click `Add user` (top-right corner of table)
1. Fill in the form with the following values:
   * Username: `foobar`
   * First Name: Your first name
   * Last Name: Your last name
1. Click `Save`
1. Click `Role Mappings` (top of the page)
1. Choose `Librarian` role from the `Available Roles`
1. Click `Add selected` 

1. Click `Users` (left-hand menu)
   * Click `Add user` (top-right corner of table)
1. Fill in the form with the following values:
   * Username: `foobaz`
   * First Name: Your first name
   * Last Name: Your last name
1. Click `Save`
1. Click `Role Mappings` (top of the page)
1. Choose `Member` role from the `Available Roles`
1. Click `Add selected`

![Add Realm](/assets/keycloak/add-user.png)

## Create Group

Groups can be assigned to manage a collection of users and the roles, so let's create one:

1. Click `Groups` (left-hand menu)
    * Click `Add user` (top-right corner of table)
1. Fill in the form with the following values:
   * Name: `ROLE_MEMBER`
1. Click `Save`
1. Click `Role Mappings` (top of the page)
1. Choose `Member` role from the `Available Roles`
1. Click `Add selected`

1. Click `Groups` (left-hand menu)
    * Click `Add user` (top-right corner of table)
1. Fill in the form with the following values:
   * Name: `ROLE_LIBRARIAN`
1. Click `Save`
1. Click `Role Mappings` (top of the page)
1. Choose `Librarian` role from the `Available Roles`
1. Click `Add selected`

The default group will need to be setup to have new users assigned the `ROLE_MEMBER` group automatically. To do this:

1. Click `Groups` (left-hand menu)
    * Click `Default Groups` (top of the page)
1. Choose `ROLE_MEMBER` group from the `Available Groups`
1. Click `Add`

[keycloak-url]: https://192.168.1.57:8443/auth
