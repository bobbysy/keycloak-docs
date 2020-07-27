---
layout: default
title: Setup Keycloak on Docker
parent: Keycloak
description: "Get started with Keycloak on Docker."
nav_order: 1
---

# Keycloak on Docker
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Before you start

Make sure you have Docker installed.

## Start Keycloak

From a terminal start Keycloak with the following command:

```sh
docker run -p 8080:8080 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin quay.io/keycloak/keycloak:9.0.3
```

This will start Keycloak exposed on the local port 8080. 
It will also create an initial admin user with username `admin` and password `admin`.

## Login to the admin console

Go to the [Keycloak Admin Console][keycloak-url] and login with the username and password you created earlier.

[keycloak-url]: https://192.168.1.57:8443/auth
