# Test Tenants

Before reading this topic, it is required to read [Tenant documentation](./tenants.md)

## Overview

Test tenants - are just usual production tenants, but with
a couple special things. Test tenants are based on relevant prod Tenants.
Each customer (tenant) can have its own test tenant (read - test environment).
Test tenant is the same as usual tenant, so all the SLAs will be the same.
Test tenant is usually used to test something out.

## What is different about test tenant

- Test Tenant ID ALWAYS has this format - `{tenantId}test`,
  so for tenant with id `qwerty123456789` tenant id will be `qwerty123456789test`
- If you are using direct calls to Hiiretail APIs
  (for example IAM API - `https://iam-api.retailsvc.com`), you will have to use
  different domain - `retailsvc-test.com` (for example IAM API for test tenants - `https://iam-api.retailsvc-test.com`)
- Auth Tokens from Tenant do not work with Test Tenants and vice versa.

Note: domain check can be disabled by passing `"Ignore-Domain-Check": "true"` header.

## Who manages Test Tenant

Test Tenant is managed by tenant (as usual tenants), and they created by IAM Team.

