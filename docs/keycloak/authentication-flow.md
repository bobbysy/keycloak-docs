---
layout: default
title: Keycloak Authentication Flow
parent: Keycloak
description: "Authentication flow for SSO."
nav_order: 3
---

# Keycloak Authentication Flow
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Authentication Flows

## SSO Protocols

## Client Configuration

The client represents either a web application or web service which uses Keycloak to authenticate and authorize users.
To create a new client: 
1. Click `Clients` (left-hand menu)
   * Click `Create` (top-right corner of table)
1. Fill in the form with the following values:
   * Client ID: `spring-boot-app`
   * Client Protocol: `openid-connect`
   * Root URL: `http://localhost:8085`
1. Click `Save`
1. Verify the settings:
   * Enabled: `ON`
   * Access Type: `public`
   * Standard Flow Enabled: `ON`


