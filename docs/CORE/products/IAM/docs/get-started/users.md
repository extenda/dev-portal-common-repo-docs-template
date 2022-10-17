# `IAM Getting started guide` for Users (Managers)

HiiRetail IAM service solves authentication & role-based authorization.

The IAM UI is reachable at `https://[tenant-alias].hiiretail.com/iam` - ask your supervisor for this URL.
For example, the IAM UI for the `extenda` tenant - `https://extenda.hiiretail.com/iam`

## Concepts

If you are not familiar with some terms, take a look at core concepts documentation:

- [IAM Concepts](./../public/concepts/readme.md)
- [OCMS Concepts](./../../ocms/public/concepts/readme.md)

## First steps

After your tenant is created (done by Extenda employees), you will receive `tenant admin email`
with full access to the IAM system (create users, connect them to groups, etc.).

With this email, login to IAM UI (reset your password, if you don't have one), and create
all necessary users, and connect them to groups.
For example, you are going to need users within your IAM Admin group.

## Use-cases scenarios

### 1. When you have some user (customer or staff) logging in with their account

For this use case, you should use IAM UI. Create the necessary users for your employees. Then assign
them groups that reflect their respective functions. We recommend you to follow [Least access principle](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

<!-- TODO: where to get needed permissions for applications? (every service should provide docs for that perhaps) -->

### 2. When there is no user directly logging in (for example, external service)

This problem is solved by [OCMS](./../../ocms/internal/README.md). You should use OCMS UI - `https://[tenant-alias].hiiretail.com/ocms`.
Your developers should prepare registration templates and software statements.
More details for developers can be found [here](./GETTING_STARTED_DEVELOPER.md)

As a manager, you have access to see all machines that use this auth method in the OCMS UI.
Also, you can revoke compromised machine access with one click in UI.
