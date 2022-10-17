# Tokens

In Hiiretail, different kinds of tokens are used for different scenarios

## IAM User tokens

IAM User tokens are used on a frontend, to access backend resources
as a User (actual human, through UI) or for testing (CI/CD Pipelines can fetch a token)

Permissions for IAM Users are managed in IAM UI
(can be found in Hiiretail Console at `https://[tenant-alias].hiiretail.com>`)

If you modify permissions for user, token MUST be re-fetched for changes to take effect

### Structure (with examples)

This documentation presents a real token example, but with garbled signature (security reasons)

The token
```
eyJhbGciOiJSUzI1NiIsImtpZCI6IjJkYzBlNmRmOTgyN2EwMjA2MWU4MmY0NWI0ODQwMGQwZDViMjgyYzAiLCJ0eXAiOiJKV1QifQ.eyJuYW1lIjoiTWFrc3ltIERvdmhvcG9saXkiLCJwaWN0dXJlIjoiaHR0cHM6Ly9saDYuZ29vZ2xldXNlcmNvbnRlbnQuY29tLy1hcFpIT1VTcVk2QS9BQUFBQUFBQUFBSS9BQUFBQUFBQUFBQS9BTVp1dWNrUUQ1MDk4QmpHLVBqMS1JbWhrVkxMTmZUSm9BL3M5Ni1jL3Bob3RvLmpwZyIsImh0dHBzOi8vZXh0ZW5kYXJldGFpbC5jb20vdGVuYW50IjoiQ0lSN25Rd3RTMHJBNnQwUzZlamQiLCJodHRwczovL2V4dGVuZGFyZXRhaWwuY29tL2dyb3VwcyI6WyJrV282WVMzM0k1MUI0VFF4bVlhbyIsImhqTWRrRUM0RHI2a2Y4VjNBdk53IiwieDNGalJXdzBJb2pLNjF5UmhyZFkiLCJkVnFuREdFWjlmV2xoVDhwUUpNMSIsInFRdjN0NDgzNkQzWmdDTDNKelRJIl0sImlzcyI6Imh0dHBzOi8vc2VjdXJldG9rZW4uZ29vZ2xlLmNvbS9oaWlkZW50aXR5LXN0YWZmIiwiYXVkIjoiaGlpZGVudGl0eS1zdGFmZiIsImF1dGhfdGltZSI6MTY0NjY1NzA3NSwidXNlcl9pZCI6Im5MdlRQU3hyUHRiR3dkcnVoSFJtejdhT2VLRTMiLCJzdWIiOiJuTHZUUFN4clB0Ykd3ZHJ1aEhSbXo3YU9lS0UzIiwiaWF0IjoxNjQ2ODM5MDQ3LCJleHAiOjE2NDY4NDI2NDcsImVtYWlsIjoibWFrc3ltLmRvdmhvcG9saXlAZXh0ZW5kYXJldGFpbC5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiZmlyZWJhc2UiOnsiaWRlbnRpdGllcyI6eyJvaWRjLndiY2h6IjpbIjExNTQ2MTI2ODA4MjAwNDEyMzM4MiJdLCJlbWFpbCI6WyJtYWtzeW0uZG92aG9wb2xpeUBleHRlbmRhcmV0YWlsLmNvbSJdfSwic2lnbl9pbl9wcm92aWRlciI6Im9pZGMud2JjaHoiLCJzaWduX2luX2F0dHJpYnV0ZXMiOnsibG9jYWxlIjoiZW4iLCJoZCI6ImV4dGVuZGFyZXRhaWwuY29tIiwianRpIjoiODRlMWMxYjIxZjA4ZjcxZmEyNDA2ZDkwMGJkMWVjYWQzMzEwOTdjZSJ9LCJ0ZW5hbnQiOiJ0ZXN0cnVubmVyLTJtZnVrIn19.bGzz3OapX6T6jEeZ1WT9WDQJiR6ZXwg3iQ9Hff5ct-rAix7cwA2JfJeXKGqe5u2dM9TLqzkwCDC7-pYHhFd1r70F9JbiYuS0WNJqHeoEe5bB_a0ALiOD6YTfx00XfT4h-Ay-QvIdl-for-docs-JpJFGKl486-VoCxIqD16EnNAV2entzagLk5x-l6vo5DkR9MiZ1SA5N4HzgJ-WirCYk7snPD3PAbWfkhTm86SmKRFik9uz3H3SGWHK8IKcgOOo1h6jMh5rE_CWpS5dJIp9dfu_tqb2FVrzeakloKFCflXGS033qjA8aoNTqZqMeUAUJWZ_psl2yesnsZA
```

Decoded
```json5
{
  // user name, optional
  "name": "Maksym Dovhopoliy",
  // user photo, optional
  "picture": "https://lh6.googleusercontent.com/-apZHOUSqY6A/AAAAAAAAAAI/AAAAAAAAAAA/AMZuuckQD5098BjG-Pj1-ImhkVLLNfTJoA/s96-c/photo.jpg",
  // user tenant id, required
  "https://extendaretail.com/tenant": "CIR7nQwtS0rA6t0S6ejd",
  // user groups array, optional
  "https://extendaretail.com/groups": [
    "kWo6YS33I51B4TQxmYao",
    "hjMdkEC4Dr6kf8V3AvNw",
    "x3FjRWw0IojK61yRhrdY",
    "dVqnDGEZ9fWlhT8pQJM1",
    "qQv3t4836D3ZgCL3JzTI"
  ],
  // user issuer, required, can be either
  // https://securetoken.google.com/hiidentity-staff (for stuff users)
  // https://securetoken.google.com/hiidentity-shoppers (for shoppers users)
  "iss": "https://securetoken.google.com/hiidentity-staff",
  // audience, required, one of:
  // hiidentity-staff (for staff users)
  // hiidentity-shoppers (for shoppers users)
  "aud": "hiidentity-staff",
  "auth_time": 1646657075,
  // user id in Google Identity Platform (usually it is the same as in IAM)
  "user_id": "nLvTPSxrPtbGwdruhHRmz7aOeKE3",
  // user id in Google Identity Platform (usually it is the same as in IAM)
  "sub": "nLvTPSxrPtbGwdruhHRmz7aOeKE3",
  "iat": 1646839047, // Standard JWT claim
  "exp": 1646842647, // Standard JWT claim
  // user email, optional
  "email": "maksym.dovhopoliy@extendaretail.com",
  "email_verified": true,
  // firebase claim, holds additional claims
  // from Google Identity Platform (Firebase)
  // and from original token, if user authenticated with ID Provider
  "firebase": {
    "identities": {
      "oidc.wbchz": [
        "115461268082004123382"
      ],
      "email": [
        "maksym.dovhopoliy@extendaretail.com"
      ]
    },
    // id of provider, used during login process, optional
    "sign_in_provider": "oidc.wbchz",
    // data from original ID token from ID Platform, optional
    "sign_in_attributes": {
      "locale": "en",
      "hd": "extendaretail.com",
      "jti": "84e1c1b21f08f71fa2406d900bd1ecad331097ce"
    },
    // Google Identity Platform tenant id, required
    "tenant": "testrunner-2mfuk"
  }
}
```

### External Identity Platform ID Token features

You can controll some of the IAM features directly from you ID Platform,
without a need to go in to Hiiretail Console. 

The example of the ID Token, and some special claims that let you controll Hiiretail IAM
```json5
{
  "exp": 1661152174,
  "iat": 1661151274,
  "auth_time": 1661151274,
  "jti": "4be30a2a-5306-46ac-af52-58126dddb9c7",
  "iss": "[REDUCTED]",
  "aud": "[REDUCTED]",
  "sub": "9204a071-7252-4ca9-b9b2-d4b8fce4cd05",
  "typ": "ID",
  "azp": "[REDUCTED]",
  "nonce": "89a43580129f146f74c500279255577c2616edaf858e9eb8123f312f3d08e19a",
  "session_state": "3c436537-d221-4b39-ae3f-90cf76b7d4c4",
  "acr": "1",
  "s_hash": "yTX90GB-yGPa6TRL7cIglg",
  "sid": "3c436537-d221-4b39-ae3f-90cf76b7d4c4",
  "email_verified": true,
  "operatorIds": [ # array of {storeId}-{operatorId} strings, some of the apps use that to identifu cashier
    "001-505",
    "002-506"
  ],
  "roles": [ # roles array, lets you specify permissions for user (ignores strings, that are not valid role ids)
    "SeniorCashier",
    "Authorizer",
    "rec.manager",
    "StoreManager"
  ],
  "name": "[REDUCTED]",
  "groups": [ # groups array, lets you specify permissions for user (ignores strings, that are not valid group ids)
    "kWo6YS33I51B4TQxmYao"
  ],
  "preferred_username": "[REDUCTED]",
  "locale": "en-GB",
  "given_name": "[REDUCTED]",
  "family_name": "[REDUCTED]",
  "email": "[REDUCTED]",
  "x_preview_features": [
    "test"
  ]
}
```

### How to get IAM user token (as an external tenant app)

In IAM There are 2 types of users:
- Standalone (password users, managed by our IAM)
- Federated (users, managed by external IDP)

#### **Authorization for token exchange endpoints**

For these endpoints you need to use a special key which you can get from [external apps](./external-apps.md) 
You need to put that key in your `Authorization` header with `Bearer` prefix, 
example: `Authorization: Bearer hii-ext-app:365e6e85-6c3d-4434-bf0b-b2cb9d4dfec1` 

#### **Exchange federated user token for IAM token**

First of all, you need to obtain an ID token from you ID provider. It MUST be registered as ID provider
in IAM UI.

With proper token, you can use [this endpoint](https://developer.hiiretail.com/api/iam-api#operation/generateTestFederatedToken) to exchange federated token for IAM token.

#### **Exchange standalone user email and password for IAM token**

For standalone users you can use [this endpoint](https://developer.hiiretail.com/api/iam-api#operation/generateTestStandaloneToken)

### Verification

Before letting request in, developers should verify token.

Every IAM User token can be verified by Google public certs, located here (map of keys, by ids) -
```
https://www.googleapis.com/robot/v1/metadata/x509/securetoken@system.gserviceaccount.com
```

Generally, a User token is valid when:
- token can be verified via certs
- `iss` is valid (see structure)
- `exp` is valid
- `https://extendaretail.com/tenant` is present


## OCMS Tokens

These tokens are used by external integrations or apps, that do not have direct access to our cloud.

More docs & examples, in OCMS Docs:

- [OCMS Docs](https://developer.hiiretail.com/docs/ocms/public/readme)
- [OCMS Tokens](https://developer.hiiretail.com/docs/ocms/public/concepts/oauth2-authentication)

### Verification

Every OCMS token is signed by OCMS Auth server, that issues JWT tokens.
```
https://auth.retailsvc.com
```

JWKs can be found at __well known__ path

```
https://auth.retailsvc.com/.well-known/jwks.json
```

Generally, User token is valid when:
- token can be verified via certs
- `iss` is valid (`https://auth.retailsvc.com/`)
- `exp` is valid
