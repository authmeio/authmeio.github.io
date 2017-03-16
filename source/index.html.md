---
title: AuthMe

language_tabs:
  - PHP
  - Java
  - Python
  - JS
  - Go


includes:
  - errors

search: false
---

# Introduction

Users keep same passwords everywhere. Despite you following security best practices, a data breach on an unrelated app can compromise your users.

AuthMe helps you do away with passwords. With AuthMe pattern lock, bring UX and security together.

# Getting Started

Before you start off, make sure that you have the API Key and Secret Key. If not, get them from [here.](https://account.authme.authme.host/generatekeys)

It can be found on the left panel of the dashboard.

API Key looks like this: `k-162b77f3-5937-4856-af1b-7950a001f732` and can be exposed in public clients (Android, iOS, Windows apps)

Secret Key looks like this: `s-001b77f3-5938-0056-af1b-7950a001f712` and should not be put in any public artifact.

Integration has 2 parts.

1. Client Integration (Android/iOS/Window)

2. Server Integration (PHP/Java/Go/Python/Js)

#Android Integration

## 1. Gradle import

Add following line to your app build.gradle file

```gradle
    # compile 'io.authme:patternlock:0.1.1'
```  

## Api Key

`Api Key`: a 38 character string beginning with `k-`, which can be exposed in public clients

`Api Secret`: a 38 character string beginngin with `s-`, which should not be put in any public artifact.

You can generate keys for your use from the [AuthMe Dashboard](https://account.authme.authme.host/generatekeys)

These keys also act as your ClientId/Secret for the OAuth2/OpenID APIs

## AuthMe Certificate

You can get the public keys for [AuthMe JWK Response tokens here](https://oauth.authme.authme.host/auth/realms/global/protocol/openid-connect/certs)

# Oauth2/OpenID

AuthMe uses [Keycloak](www.keycloak.org) to expose an OAuth2 Interface for authentication. Use the [OpenID Discovery EndPoint](https://oauth.authme.authme.host/auth/realms/global/.well-known/openid-configuration) to configure your OpenID clients
