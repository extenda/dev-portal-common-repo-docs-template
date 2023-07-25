# Tokens for internal devs

This topic explains different types of tokens in Hiiretail
and provides instructions on how to validate them in Hiiretail apps.

## IAM Tokens

Examples and descriptions can be found [here](https://developer.hiiretail.com/docs/identity-and-access-management/public/concepts/tokens/)

### How to verify IAM Tokens in apps

#### In OPA

Basic example:
```rego
allow {
  http_request.method == "GET"
  parsed_path == ["api", "v1", "permissions"]
  check_permission("iam.permission.list")
}
```

[More about requests validation via OPA](https://github.com/extenda/tf-infra-gcp/blob/master/docs/authz.md)

#### Manually

Every user token can be verified by Google public certs, located here (map of keys, by ids) -
```
https://www.googleapis.com/robot/v1/metadata/x509/securetoken@system.gserviceaccount.com
```

Generally, User token is valid when:
- token can be verified via certs
- `iss` is valid (see structure)
- `exp` is valid
- `https://extendaretail.com/tenant` is present

## OCMS Tokens

Examples and descriptions can be found [here](https://developer.hiiretail.com/docs/ocms/public/concepts/oauth2-authentication)

### How to verify OCMS Tokens in apps

#### In OPA

Basic example:
```rego
allow {
  http_request.method == "GET"
  parsed_path == ["api", "v1", "movies"]
  check_permission("bhq.movies.list")
}
```

Validation of OCMS token is not different from IAM token,
but OCMS token also allows some additional things that you can validate.

#### Manually

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

## Workload Identity (Service Account) ID Tokens

This type of tokens is used for internal service to service
communications inside our Google Cloud.

There are some utility libs that can help you with fetching this token:
- [JS/TS](https://www.npmjs.com/package/google-sa-id-token)

Also token can be fetched manually, by http request:
```http request
GET /computeMetadata/v1/instance/service-accounts/default/identity?audience=bhq.api
Host: http://metadata.google.internal
Metadata-Flavor: Google
```

Audience - name of requester system.

Example of such token (signature is garbled)

Token
```
eyJhbGciOiJSUzI1NiIsImtpZCI6IjU4YjQyOTY2MmRiMDc4NmYyZWZlZmUxM2MxZWIxMmEyOGRjNDQyZDAiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJpYW0udXNlci1wcm9maWxlcy1hcGkiLCJhenAiOiIxMDY5MjQzNzk0NjA4NzE0MDg4MzciLCJlbWFpbCI6ImlhbS1zeW5jLXdvcmtlckBpYW0tcHJvZC00YWFkLmlhbS5nc2VydmljZWFjY291bnQuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImV4cCI6MTY0ODQxNTY0MCwiaWF0IjoxNjQ4NDEyMDQwLCJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJzdWIiOiIxMDY5MjQzNzk0NjA4NzE0MDg4MzcifQ.m485pxY6_dupAr7RoSLHqbChS4_10jjyw0_JTM6NzD9nmM35o2_gHTTFvz92ePKa9O6REj90_3C0rMtRsl1G_Xjfx7aWTrfM6sBexuRapn53Rltre58X8sIr-HJlS8ySy5qieO--Hp69sk6K89jYx93OH3vPV5neCSIjK1qzpKE_gbwB5SKbAl2L29wk6V5kHLSrpoPKFzQCxdb_4TpoCyfor-demo6KasBi0uYXCNTozZbpvnE5pon8mpuW2VOGEbunONSgYiNLM2w0PZ1S-4bZxTo-mmfo7NjBZhTJDJbSiqtDg
```

Decoded
```json5
{
  "aud": "iam.user-profiles-api",
  "azp": "106924379460871408837",
  "email": "iam-sync-worker@iam-prod-4aad.iam.gserviceaccount.com",
  "email_verified": true,
  "exp": 1648415640,
  "iat": 1648412040,
  "iss": "https://accounts.google.com",
  "sub": "106924379460871408837"
}
```

### How to verify Workload Identity (Service Account) ID Tokens in apps

#### In OPA

Most secure way

```rego
allow {
  http_request.method == "GET"
  parsed_path == ["api", "v1", "movies"]
  googletoken.check("bhq.api")
}
```

Also this way requires requester to whitelist itself in receiver
clan common repo iam `allowed-consumers` list. [Example for IAM Clan](https://github.com/extenda/engineering-iam-common/blob/b8a6bd4bd4d681e3cd90f0a4191cb831209635fd/iam/iam.yaml#L8)

Less secure way, does not require any additional steps, as it
will not check is service account whitelisted

```rego
allow {
  http_request.method == "GET"
  parsed_path == ["api", "v1", "movies"]
  googletoken.check_token("bhq.api")
}
```

#### Manually

In this example i'll use sample WI token.

```
eyJhbGciOiJSUzI1NiIsImtpZCI6ImU4NDdkOTk0OGU4NTQ1OTQ4ZmE4MTU3YjczZTkxNWM1NjczMDJkNGUiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJpYW0udXNlci1wcm9maWxlcy1hcGkiLCJhenAiOiIxMDY5MjQzNzk0NjA4NzE0MDg4MzciLCJlbWFpbCI6ImlhbS1zeW5jLXdvcmtlckBpYW0tcHJvZC00YWFkLmlhbS5nc2VydmljZWFjY291bnQuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsImV4cCI6MTY2MjY0NTU4NSwiaWF0IjoxNjYyNjQxOTg1LCJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJzdWIiOiIxMDY5MjQzNzk0NjA4NzE0MDg4MzcifQ.OUT6aZ0uIF-CWYFNT5KjE3_0cZWqDexwY-HSCnqpTT4OrCAV85xRDzcUNlgblhZunVS_JNL4ahd7ebEX6yJQpDV4QaTdcVFzaWIdR9T9mQf4_6gIMNj1_L_jT60tOw6BeqjKgBlVt6kNj1eYI_66C8kj5KUcdRxNVx6OI4cganSnNfcdn2wKs5C8e7ZSMSbcKzJs3HkZO6Mb9BL2RcXl8XGcZKMvnh-gvaCWwPDpZ5P8VGSizXgB5ai1woyWEHkz-W0Bp_5zOCSGvbORbW4nWDQqk-YHz6Axuymepw0uG-Z2SSqecwhxW0vITmKYmZgX
```

### 1. Decode the token

Use any jwt decode lib you can find for your language

decoded token should have `header` & `payload`

For example token:
- header
```js
{
    alg: 'RS256',
    kid: 'e847d9948e8545948fa8157b73e915c567302d4e',
    typ: 'JWT'
}
```
- payload
```js
{
    aud: 'iam.user-profiles-api',
    azp: '106924379460871408837',
    email: 'iam-sync-worker@iam-prod-4aad.iam.gserviceaccount.com',
    email_verified: true,
    exp: 1662645585,
    iat: 1662641985,
    iss: 'https://accounts.google.com',
    sub: '106924379460871408837'
}
```

### 2. Find out the what cert you need to use to validate

2.1 Get the key id (`kid` claim in header)
2.2 get list of certs from here - https://www.googleapis.com/oauth2/v1/certs
It will look something like this:

```json
{
  "caabf6908191616a908a1389220a975b3c0fbca1": "-----BEGIN CERTIFICATE-----\nMIIDJzCCAg+gAwIBAgIJAMs/Jq4FTae1MA0GCSqGSIb3DQEBBQUAMDYxNDAyBgNV\nBAMMK2ZlZGVyYXRlZC1zaWdub24uc3lzdGVtLmdzZXJ2aWNlYWNjb3VudC5jb20w\nHhcNMjIwOTA0MTUyMjEwWhcNMjIwOTIxMDMzNzEwWjA2MTQwMgYDVQQDDCtmZWRl\ncmF0ZWQtc2lnbm9uLnN5c3RlbS5nc2VydmljZWFjY291bnQuY29tMIIBIjANBgkq\nhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAs0WZ/ZkzbW3vUuUiWy3u2D1RNLWDM02V\neuizCFj16xX2Swd2WyS4+m0kLeOBgxU6zhenpzrU4aQypv4YFJMaB2QOvPXrLtcF\n4re3quSbjxjWqDKc4fJkOYMVV6X6GpaUV6FYdiYNDMiIBctPMoWVSpYhdHulKCXz\n366BmrAYFqUO1sUYEvkA8RKpnMyiOq85EZFXhTsoBc7OUjXrRGJQh/pUOF49/fZp\nCCW/y3BA9xDmxfw4AUn9ehwhmZ0J6ZicVNY10Axt7mTpilPtP++rNMYCDRXGODtt\nMSmEJO8bCb5h7hvCM4y9cpsYBR5oq953Ik5Hm24Mub99MwsJGzyp5QIDAQABozgw\nNjAMBgNVHRMBAf8EAjAAMA4GA1UdDwEB/wQEAwIHgDAWBgNVHSUBAf8EDDAKBggr\nBgEFBQcDAjANBgkqhkiG9w0BAQUFAAOCAQEAacg08jGF0Ecad/k7tGQArLHNcBKX\nwDiRVoKfDhipdCUojYCQq0hP9UFwMOTU9A3bjjs5IoNJcG8V/dmkQV1qnYXcYPVu\nzX3xpR2f6LGr+Oas1dKA1mgkI/hWaXKMWKh39BRdn73Qa6pQ0OlUq+IceRgPy/2F\nkPnUyLEMA4fvmpgfavbco+70PlJAt278HAf32drzsp9Mf7E2474aBbbLTJCQw4AS\n4KTGkUOn4xFgKH7ANIvhyQUMr1OhUZ3C/JT1dK2tJTHnKMBRqPivCXvUDlt6RvvM\nrCm8Mq45TvVaKYExNQ2zU8yXK8M3CTisWphem8zrjTrTj+Ss6xE499TyoQ==\n-----END CERTIFICATE-----\n",
  "e847d9948e8545948fa8157b73e915c567302d4e": "-----BEGIN CERTIFICATE-----\nMIIDJzCCAg+gAwIBAgIJAOiiP+CnwEYwMA0GCSqGSIb3DQEBBQUAMDYxNDAyBgNV\nBAMMK2ZlZGVyYXRlZC1zaWdub24uc3lzdGVtLmdzZXJ2aWNlYWNjb3VudC5jb20w\nHhcNMjIwODI3MTUyMjA5WhcNMjIwOTEzMDMzNzA5WjA2MTQwMgYDVQQDDCtmZWRl\ncmF0ZWQtc2lnbm9uLnN5c3RlbS5nc2VydmljZWFjY291bnQuY29tMIIBIjANBgkq\nhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA21mw5OBuQDYONRNRyek+5Mwe2anpgn+1\nNy/RGKU9eNO6/wWg+emzTpwKt4c7dDXgfyJEJ63L0zD/CS+FSyzksHKoGGySsDVX\n+6nD6n36MGxVCz5Z60wgM5FaSKpf7G3iOJi0IiutLcoYv5jl72g6k6nqrRTe5BSm\n7JfNedjpRzOeBm3IPQChW9OSW/fufV8q7Ty09ZbS0fU6KRnsMyCi80EYYg0ondJD\nd56iVUKR4f/OivS+EAZSUzjcu4uWYDzc9lOw8sCbb9oJE4HWLE1bgbQ05jxIqzD+\n6oztB1Mi+0fT5A8BV26MXnSLVPiTCgbSmQSiTq+I//uqxAfsg2v6OQIDAQABozgw\nNjAMBgNVHRMBAf8EAjAAMA4GA1UdDwEB/wQEAwIHgDAWBgNVHSUBAf8EDDAKBggr\nBgEFBQcDAjANBgkqhkiG9w0BAQUFAAOCAQEAg5PdBsjaj2+wcY5gKEFk8taZhXei\nR+hz1V+CWHMoj2U2uH824qr+DqtGrLviuTaY4zh6slaMc/H76swOwUhUIu0zG7n1\nFUl7A82zvX1LJYLRhzPQiegV8NusBJGDo6s10Pc238aX7/d5yovjzV9cAH3VUY2U\nWqBiy8c+0m7W3h+6eKkKLzoqnN3qGZxS9ahyNxJs6s30Ps3O7agWtFoV+9F9/fx+\nK6yoRcSu+gk1NxdlBs+OljuSQW07eW7gpDt7KnfmmAxqNZwqgwTJM7Mv80gbvAaP\ngQIWsCIrFhfAG8JD82gbqjoS+sGkfsSYDlaXule/bz+SPOnm2baB8CfMkA==\n-----END CERTIFICATE-----\n"
}
```
2.3 Get cert by your kid

### 3. Validate jwt token using cert from previous step.

For this step you would need to use any jwt validation lib, there are millions of the on the internet.

Pceudo code for validation:

```
isValid = validateJwt(token, { aud: 'expected audience', email: 'email of the requester', cert: cert });
```

### Code Samples

- Javascript
```js
// google-auth-library@8.5.1
import {Compute} from 'google-auth-library';

const client = new Compute();

async function validateToken(jwtToken) {
    try {
        const result = await client.verifyIdToken({
            idToken: jwtToken,
            audience: 'iam.user-profiles-api', // to validate audience, specify audience 🙃
        });
        return result.payload.email === 'iam-sync-worker@iam-prod-4aad.iam.gserviceaccount.com';
    } catch (err) {
        console.log(err);
        return false;
    }
}
```
