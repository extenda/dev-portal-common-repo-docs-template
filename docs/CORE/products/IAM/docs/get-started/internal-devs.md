# `IAM Getting started guide` for Developers

The IAM system solves several authentication problems - Human-to-Machine auth, Machine-to-Machine auth.
It also provides flexible access management for every application that uses IAM

## Concepts

If you are not familiar with some terms, take a look at core concepts documentation:

- [IAM](./../../index.md)
- [OCMS]() <!-- TODO: link to other products? how to? -->

## Events

IAM system emits pubsub messages for some events. [details](./../events/index.md)

## Use cases

**IMPORTANT!!** Carefully read available use-cases for IAM and select a flow that suits your needs.

### 1. Human to machine authentication

Commonly used pattern, where Human signs in with Federated identity (Google, Azure, Okta, etc.)
or Standalone Identity (login + password) with a login UI (for example - our landing page `https://testrunner.hiiretail.com/`).

[How to set up this auth in your application](https://github.com/extenda/tf-infra-gcp/blob/master/docs/authn.md)

In your backend service API set up OPA to check for permissions in access token, needed for endpoint.

Example rule -

This rule will check for `iam.statements.create` permission in user token and tenant

```rego
allow = true {
  http_request.method == "POST"
  parsed_path = ["api", "v1", "tenants", tenant_id, "resource"]
  check_permissions("iam.resource.create")
  check_tenant_access(tenant_id)
}
```

Example token for `testrunner`-

```jwt
eyJhbGciOiJSUzI1NiIsImtpZCI6IjJjOGUyYjI5NmM2ZjMyODRlYzMwYjg4NjVkNzI5M2U2MjdmYTJiOGYiLCJ0eXAiOiJKV1QifQ.eyJuYW1lIjoiTWFrc3ltIERvdmhvcG9saXkiLCJwaWN0dXJlIjoiaHR0cHM6Ly9saDYuZ29vZ2xldXNlcmNvbnRlbnQuY29tLy1hcFpIT1VTcVk2QS9BQUFBQUFBQUFBSS9BQUFBQUFBQUFBQS9BTVp1dWNrUUQ1MDk4QmpHLVBqMS1JbWhrVkxMTmZUSm9BL3M5Ni1jL3Bob3RvLmpwZyIsImh0dHBzOi8vZXh0ZW5kYXJldGFpbC5jb20vdGVuYW50IjoiQ0lSN25Rd3RTMHJBNnQwUzZlamQiLCJodHRwczovL2V4dGVuZGFyZXRhaWwuY29tL2dyb3VwcyI6WyJ4M0ZqUld3MElvaks2MXlSaHJkWSIsImtXbzZZUzMzSTUxQjRUUXhtWWFvIl0sImlzcyI6Imh0dHBzOi8vc2VjdXJldG9rZW4uZ29vZ2xlLmNvbS9oaWlkZW50aXR5LXN0YWZmIiwiYXVkIjoiaGlpZGVudGl0eS1zdGFmZiIsImF1dGhfdGltZSI6MTYxNjc2NDg1MSwidXNlcl9pZCI6Im5MdlRQU3hyUHRiR3dkcnVoSFJtejdhT2VLRTMiLCJzdWIiOiJuTHZUUFN4clB0Ykd3ZHJ1aEhSbXo3YU9lS0UzIiwiaWF0IjoxNjE4OTA1NDQ1LCJleHAiOjE2MTg5MDkwNDUsImVtYWlsIjoibWFrc3ltLmRvdmhvcG9saXlAZXh0ZW5kYXJldGFpbC5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiZmlyZWJhc2UiOnsiaWRlbnRpdGllcyI6eyJvaWRjLndiY2h6IjpbIjExNTQ2MTI2ODA4MjAwNDEyMzM4MiJdLCJlbWFpbCI6WyJtYWtzeW0uZG92aG9wb2xpeUBleHRlbmRhcmV0YWlsLmNvbSJdfSwic2lnbl9pbl9wcm92aWRlciI6Im9pZGMud2JjaHoiLCJzaWduX2luX2F0dHJpYnV0ZXMiOnsibG9jYWxlIjoiZW4iLCJoZCI6ImV4dGVuZGFyZXRhaWwuY29tIiwianRpIjoiMjc5ZTc4NmRjNjczNDQ4ZDNlODE5MGNkM2VkMjRhMDNjZjRiZDg3YyJ9LCJ0ZW5hbnQiOiJ0ZXN0cnVubmVyLTJtZnVrIn19.U54DAjCjhtN245HQFwgnpuy-HuS5UwYqdXj9kEHLNyMjgd-MyB7zQeAaIh9t0BK8zA0lA9jnjg9imrEpda3-TBMrKvjFEghicrJJ622sRO_zHn8P1CK75MAXS1Qqq_lyiCfXUQ5Ae_bk0bTNouaYy4Qsc3TK0sLznjSsdn-gb1qZeOdq5Q7GrLCdGVwBnKHkyQaUbHaErDIZZXPvixEv2moZegpTILyl6EFoPRF0RwuTz-McT80RscIeGO98UYKtV437l-_ID9gTcywZVKkolXMp6YA6HCxIhM-g-2l8lhOMTlYjZTvAd56Z-Ig1PhaQeGeDqe2fpUQYtcfVJIoImg
```

Decoded

```JSON
{
  "name": "Maksym Dovhopoliy",
  "picture": "https://lh6.googleusercontent.com/-apZHOUSqY6A/AAAAAAAAAAI/AAAAAAAAAAA/AMZuuckQD5098BjG-Pj1-ImhkVLLNfTJoA/s96-c/photo.jpg",
  "https://extendaretail.com/tenant": "CIR7nQwtS0rA6t0S6ejd",
  "https://extendaretail.com/groups": [
    "x3FjRWw0IojK61yRhrdY",
    "kWo6YS33I51B4TQxmYao"
  ],
  "iss": "https://securetoken.google.com/hiidentity-staff",
  "aud": "hiidentity-staff",
  "auth_time": 1616764851,
  "user_id": "nLvTPSxrPtbGwdruhHRmz7aOeKE3",
  "sub": "nLvTPSxrPtbGwdruhHRmz7aOeKE3",
  "iat": 1618905445,
  "exp": 1618909045,
  "email": "maksym.dovhopoliy@extendaretail.com",
  "email_verified": true,
  "firebase": {
    "identities": {
      "oidc.wbchz": [
        "115461268082004123382"
      ],
      "email": [
        "maksym.dovhopoliy@extendaretail.com"
      ]
    },
    "sign_in_provider": "oidc.wbchz",
    "sign_in_attributes": {
      "locale": "en",
      "hd": "extendaretail.com",
      "jti": "279e786dc673448d3e8190cd3ed24a03cf4bd87c"
    },
    "tenant": "testrunner-2mfuk"
  }
}
```

[More detailed docs](./README.md)

### 2. Machine to machine authentication outside our cloud

If you need to request HiiRetail services, and your service must authenticate without
human interaction (for example - Transaction engine, Image recognition service, POS, etc.).

[OCMS](./../../ocms/internal/README.md) service should solve this issue. It implements [Dynamic Registration protocol](https://tools.ietf.org/html/rfc7591)

Also, we have a [example code](https://github.com/extenda/hiiretail-iam-demo) for OCMS -

The idea is -

As a developer

- create registration template (used by applications to register)

As a manager (or dev)

- (optional) create software statement (credential, that verifies application identity, used by applications to register)

As application

- register itself as oauth client in OCMS,

After registration application receives `client_id` and `client_secret` which can be used to fetch the access token

example in OPA

```rego
allow = true {
  http_request.method == "POST"
  parsed_path = ["v1", "tenants", tenant_id, "software-statements"]
  check_permissions("iam.statements.create")
}
```

[More detailed docs]() <!-- TODO: link to other products? how to? -->

### 3. Machine to machine authentication inside our cloud

[This doc](https://github.com/extenda/tf-infra-gcp/blob/master/docs/authn.md#service-to-service-communication-inside-gcp)
will explain how to solve this issue. Contact the Cloud Native team if you still have questions.
