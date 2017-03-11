---
title: AuthMe Swipe Authentication

language_tabs:
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='https://account.authme.authme.host'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the AuthMe! You can use our AuthMe to authenticate the users on your application or website and get a profile of the user. We aim to remove Password from all user flows, provide high level of trust in the user and make the web secure for both the users and businesses.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

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
