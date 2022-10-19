# IAM Identity Providers

## Overview

Hii Retail IAM supports authentication by a federated identity provider.
It allows users to authenticate to Hii Retail using the credentials already familiar to them for systems like Microsoft Azure AD, Okta, or Ping Federate.  

## OIDC Provider

Hii Retail supports any [Certified OpenID 2.0 Identity Provider](https://openid.net/developers/certified/#OPServices). Creating and configuring an OIDC Identity Provider is a simple procedure performed in the Hii Retail IAM Management Console. Once complete, your Hii Retail Users will see a new log-on option on the Hii Retail landing page, which allows them to log in via the Identity Provider.
*To create and configure "OIDC Identity Provider" you will need the following permissions*

- `iam.provider.create`,
- `iam.provider.update`,
- `iam.provider.list`.

*For these permissions, you need to have one of these roles: `iam.admin`, `iam.provider-admin`.*
