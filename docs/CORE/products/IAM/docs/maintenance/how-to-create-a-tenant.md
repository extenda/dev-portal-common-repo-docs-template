# Steps for creating a tenant in Hii Retail

## Introduction

Tenants should manage as much as possible of their data and configurations as possible. This can either be done by the tenant themselves or with the help of consultants. However, there are some tasks related to the creation of tenants that must be performed by Extenda Retail employees.

It is strongly recommended that you read the [complete documentation](./README.md) before starting the tenant creation process.

## Defining the names

A tenant has a name and an alias. The name could be almost anything and should be used only for display. The alias is used in the URL for accessing the tenant's applications. Ie `[tenant-alias].hiiretail.com`. The customer will need to agree on the name and alias. Example:
    Name: Extenda Retail
    Alias: extenda

## Create an email account for the tenant admin

Tenants will perform this step, and the purpose is to create a mail account to be used for the [tenant admin](../public/concepts/tenants.md#tenant-admin). The email address MUST be in the format `hiiretail@<your-domain>`. The tenant admin has the right to create users and grant access in Hii Retail. It should be used to create the initial user(s) and grant administrative access. This is described later in this document.

## Configuring the tenant-specific URL

An Extenda Retail employee will perform this step.

1. In the [landing-page repository `cloud-run.yaml`](https://github.com/extenda/hiiretail-engineering-landing-page/edit/master/cloud-run.yaml), add `[tenant-alias].hiiretail.com` to the list of domain mappings. It might take some time for the changes to be applied.
2. In the [firebase console](https://console.firebase.google.com/project/hiidentity-staff/authentication/providers) add `[tenant-alias].hiiretail.com` to authorized domains

## Create the tenant in Hii Retail

An Extenda Retail employee will perform this step.

Based on the information obtained above, the tenant is created in [Hii Retail](https://extenda.hiiretail.com/iam/tenants), and a password reset email will be sent to the tenant admin's email account.

## Set the password

Follow the link in the reset password mail and set the password for the tenant admin user.

## Create Hii Retail as an application in the federated system

In the tenant's identity system such as Azure AD, Okta, Ping Identity, or similar, create Hii Retail as an application and obtain the issuer and client id values. How this is done varies between the different vendors and might require in-depth knowledge of the identity provider.

*IMPORTANT:* do not forget to whitelist redirect-URIs that you will get after Identity Provider creation

## Integration with external events service

For tenant to be able to receive events from Hii Retail you need to setup pubsub topic and subscription in [cloud-core-common](https://github.com/extenda/engineering-cloud-core-common) repository.

Get tenant id from [Tenants Page](https://extenda.hiiretail.com/iam/tenants).

Add folder to `/infra/prod/pubsub/exe.private.output.[TENANT_ID]-webhook-requests.v1` with the contents similar to [testrunner configuration](https://github.com/extenda/engineering-cloud-core-common/tree/master/infra/prod/pubsub/exe.private.output.CIR7nQwtS0rA6t0S6ejd-webhook-requests.v1) using the correct tenant id.

## Create the identity provider in Hii Retail

Log in as tenant admin to `[tenant-alias].hiiretail.com`. Select the menu "Identity Providers" and click the plus sign to create a new provider. Enter a name of your choice and the `client id` and `issuer` obtained in the previous step. Select "Staff" if this provider is intended to be used as login for staff users or "Shoppers" if this provider is intended to be used by shoppers. Only providers marked as "Staff" will be available for login on the landing page `[tenant-alias].hiiretail.com`.
[The difference between Staff and Shoppers user](https://extenda.hiiretail.com/iam/tenants)

## Steps recommended for Staff user

If you have created a provider for staff users the following steps should be taken:

* Create a group and assign it the role "IAM Admin"
* Create one or more users
* Add the users to the newly created group

The tenant admin should no longer be used to administer your tenant after the completion of these steps.
