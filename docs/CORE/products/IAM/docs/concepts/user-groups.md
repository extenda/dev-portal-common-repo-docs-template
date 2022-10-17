# IAM Users and Groups

## Users

A User represents a specific named entity that can act on a resource.
The User identity can represent a _human_ User like a store employee or back-office administrator.
The User identity can also represent a _machine_ or _service_ User such as a point of sales endpoint.
[For more information about managing access for machine or service-users see OCMS](./../../../ocms/public/readme.md).

### Tenant Admin User

When a Hii Retail Tenant is created for a new customer, a single User account exists.
This account is referred to as the Tenant Admin. This account can be thought of as a _root_ user,
as is granted access to perform _all_ actions on _all_ IAM resources (users, groups, etc.) within the Tenant.
Tenant admins get their permissions from OPA. These permissions are replicated as a static list in the UI. 
It is not possible to change these permissions from the UI.

This account is automatically created, and it must adhere to the naming convention of `hiiretail@<tenant-domain-name>`.
The initial password for this account is sent to the specified email, therefore the HiiRetail Tenant is expected to add support
for this email account and ensure it is routed to the individual or group of administrators who should have access
to the Tenant Admin account.

Extenda Retail _strongly_ recommends that this Tenant Admin account only be used for initial setup.
Once additional Users are created and granted access to the IAM system,
this Tenant Admin account should be disabled. Extenda Retail and its employees are never in
possession of the credentials for this account.

### Standalone (local) Users

A Standalone User is a local user account created and managed directly within the Hii Retail IAM console.
The username for a Standalone User must adhere to the
[email address format](https://tools.ietf.org/html/rfc5322#section-3.4.1).
The IAM Administrator for a tenant creates standalone users. The IAM admin can choose between two password options for the new user.

* Specify the password in the new user form.
* Leave the password field empty - a password reset link will automatically be sent to the new user's email.

### Federated Users

A "Federated User" is a user managed within a Tenant's own identity management system.
Microsoft Azure AD, Okta, or Ping Federate are examples of identity management systems. User federation is made possible by creating
a Hii Retail IAM Identity Provider. Federated Users validate their identity by successfully authenticating
themselves to the Tenant's Identity Provider. An identity token is then sent from the Identity Provider
to Hii Retail IAM. The identity token includes the user id, authentication information
and may also contain user roles and property attributes. These claims can be used by downstream
Hii Retail resources to enforce authorization policies.

### Anonymous Users

"Anonymous Users" can be used to access Hii Retail services that require authentication without
requiring users to sign in. A unique user session is created for an Anonymous User.
Anonymous Users require no setup or management. Anonymous users are not visible within
the Hii Retail IAM Management Console.

### Retail Employees vs. Retail Customers

Hii Retail IAM supports two user identity types - those belonging to retail _employees_,
and those belonging to retail _customers_.

#### Retail Employees

Applications used by retail employees support the following user authentication options:

* Federated
* Standalone

#### Retail Customers

Applications used by retail customers support the following user authentication options:

* Anonymous
* Federated

### Groups

A group is a named collection containing one or more User identities.
*You must have the `iam.admin` role to operate groups*
