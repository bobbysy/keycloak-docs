---
layout: default
title: Setup Keycloak with Angular
parent: Angular
description: "Get started with Keycloak and Angular."
nav_order: 1
---

# Keycloak with Angular
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Before you start

Make sure you have Keycloak Angular installed.

```sh
npm i --save keycloak-angular
```

## About Keycloak Angular

This library helps you to use [keycloak-js](https://www.keycloak.org/docs/latest/securing_apps/index.html#_javascript_adapter) in Angular > v4.3 applications providing the following features:

- A **Keycloak Service** which wraps the keycloak-js methods to be used in Angular, giving extra
  functionalities to the original functions and adding new methods to make it easier to be consumed by
  Angular applications.
- Generic **AuthGuard implementation**, so you can customize your own AuthGuard logic inheriting the authentication logic and the roles load.
- A **HttpClient interceptor** that adds the authorization header to all HttpClient requests.
  It is also possible to disable this interceptor or exclude routes from having the authorization header.
- This documentation also assists you to configure the keycloak in your Angular applications and with
  the client setup in the admin console of your keycloak installation.

## Configure keycloak-angular using the APP_INITIALIZER

The KeycloakService can be initialized during the application loading, using the APP_INITIALIZER token.

### AppModule

```js
import { NgModule, APP_INITIALIZER } from "@angular/core";
import { KeycloakService, KeycloakAngularModule } from "keycloak-angular";
import { AppConfig } from "./config/app.config";

export function initializeApp(
  appConfig: AppConfig,
  keycloak: KeycloakService
): () => Promise<any> {
  return (): Promise<any> =>
    appConfig.load().then(async result => {
      // On appConfig load finished, used the variables to init the keycloak
      const KEYCLOAK_CONFIG = result?.keycloakConfig;
      try {
        await keycloak.init({
          config: {
            url: KEYCLOAK_CONFIG.url,
            realm: KEYCLOAK_CONFIG.realm,
            clientId: KEYCLOAK_CONFIG.clientId,
            credentials: KEYCLOAK_CONFIG.credentials
          },
          bearerExcludedUrls: ["/assets", "/home"]
        });
      } catch (error) {
        console.log(error);
      }
    });
}

@NgModule({
  imports: [KeycloakAngularModule],
  providers: [
    AppConfig,
    {
      provide: APP_INITIALIZER,
      useFactory: initializeApp,
      multi: true,
      deps: [AppConfig, KeycloakService]
    }
  ]
})
export class AppModule {}
```

## AuthGuard

A generic AuthGuard, `KeycloakAuthGuard`, was created to help you bootstrap your security configuration and avoid duplicate code. This class already checks if the user is logged in and get the list of roles from the authenticated user, provided by the keycloak instance. In your implementation you just need to implement the desired security logic.

Example:

```js
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';
import { KeycloakAuthGuard, KeycloakService } from 'keycloak-angular';

@Injectable({
  providedIn: 'root'
})
export class CanAuthenticationGuard extends KeycloakAuthGuard implements CanActivate {
  constructor(protected router: Router, protected keycloakAngular: KeycloakService) {
    super(router, keycloakAngular);
  }

  isAccessAllowed(
      route: ActivatedRouteSnapshot,
      state: RouterStateSnapshot
    ): Promise<boolean> {
      return new Promise(async (resolve, reject) => {
        if (!this.authenticated) {
          this.keycloakAngular.login().catch((e) => console.error(e));
          return reject(false);
        }
        console.log(
          'role restriction given at app-routing.module for this route',
          route.data.roles
        );
        console.log('User roles coming after login from keycloak :', this.roles);
        const requiredRoles: string[] = route.data.roles;
        if (!requiredRoles || requiredRoles.length === 0) {
          return resolve(true);
        } else {
          if (!this.roles || this.roles.length === 0) {
            resolve(false);
          }
          let granted: boolean = false;
          for (let i = 0; i < requiredRoles.length; i += 1) {
            if (this.roles.indexOf(requiredRoles[i]) > -1) {
              granted = true;
              break;
            }
          }
          resolve(granted);
        }
      });
    }
}
```
