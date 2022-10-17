# Steps for creating a test tenant in Hii Retail

## Before you read this

Make sure, you are familiar with [tenant creation process](./STEPS_FOR_CREATING_A_TENANT.md)

## Introduction

Test tenants were introduced to give customer separate environment for testing purposes.
Test env is created on customer's demand. Under the hood, test tenants, are just regular tenants.

## Defining the names

Test tenant names:
    - alias will use following template `{prod-tenant-alias}-test`.
    - name will use following template `{prod-tenant-name} Test`

## Tenant admin email

Tenant admin email will be the same

## Configuring the tenant-specific URL

An Extenda Retail employee will perform this step.

1. In the landing-page repository, add `[tenant-alias].hiiretail.com` to the list of domain mappings. It might take some time for the changes to be applied.
2. In the [firebase console](https://console.firebase.google.com/project/hiidentity-staff/authentication/settings) add `[tenant-alias].hiiretail.com` to authorized domains

## Create the tenant in Hii Retail

An Extenda Retail employee will perform this step.

Based on production tenant id, you can use special API, that will cretate test tenant (not UI support yet).

The API - https://developer.hiiretail.com/api/iam-api#operation/createTestTenant.

## Set the password

Follow the link in the reset password mail and set the password for the tenant admin user.

## Create Hii Retail as an application in the federated system

In the tenant's identity system such as Azure AD, Okta, Ping Identity, or similar, create Hii Retail as an application and obtain the issuer and client id values. How this is done varies between the different vendors and might require in-depth knowledge of the identity provider.

*IMPORTANT:* do not forget to whitelist redirect-URIs that you will get after Identity Provider creation

## Integration with external events service (optional)

For tenant to be able to receive events from Hii Retail you need to setup pubsub topic and subscription in [cloud-core-common](https://github.com/extenda/engineering-cloud-core-common) repository.

Get tenant id from [Tenants Page](https://extenda.hiiretail.com/iam/tenants).

Add folder to `/infra/prod/pubsub/exe.private.output.[TENANT_ID]-webhook-requests.v1` with the contents similar to [testrunner configuration](https://github.com/extenda/engineering-cloud-core-common/tree/master/infra/prod/pubsub/exe.private.output.CIR7nQwtS0rA6t0S6ejd-webhook-requests.v1) using the correct tenant id.

## Create the identity provider in Hii Retail

Log in as tenant admin to `[tenant-alias].hiiretail.com`. Select the menu "Identity Providers" and click the plus sign to create a new provider. Enter a name of your choice and the `client id` and `issuer` obtained in the previous step. Select "Staff" if this provider is intended to be used as login for staff users or "Shoppers" if this provider is intended to be used by shoppers. Only providers marked as "Staff" will be available for login on the landing page `[tenant-alias].hiiretail.com`.
[The difference between Staff and Shoppers user](https://extenda.hiiretail.com/iam/tenants)

## Steps recommended for Staff user

If you have created a provider for staff users the follwing steps should be taken:

* Create a group and assign it the role "IAM Admin"
* Create one or more users
* Add the users to the newly created group

The tenant admin should no longer be used to administer your tenant after the completion of these steps.
