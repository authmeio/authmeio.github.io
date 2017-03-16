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

You can quickly get started with the Oauth2 flow to integrate with your website or application, or go through our core APIs for a deeper integration.


# Getting an API key

All the interaction between your website and AuthMe will be based on 2 pair of keys

## Api Key

`Api Key`: a 38 character string beginning with `k-`, which can be exposed in public clients

`Api Secret`: a 38 character string beginngin with `s-`, which should not be put in any public artifact.

You can generate keys for your use from the [AuthMe Dashboard](https://account.authme.authme.host/generatekeys)

These keys also act as your ClientId/Secret for the OAuth2/OpenID APIs

## AuthMe Certificate

You can get the public keys for [AuthMe JWK Response tokens here](https://oauth.authme.authme.host/auth/realms/global/protocol/openid-connect/certs)

# Oauth2/OpenID

AuthMe uses [Keycloak](www.keycloak.org) to expose an OAuth2 Interface for authentication. Use the [OpenID Discovery EndPoint](https://oauth.authme.authme.host/auth/realms/global/.well-known/openid-configuration) to configure your OpenID clients
