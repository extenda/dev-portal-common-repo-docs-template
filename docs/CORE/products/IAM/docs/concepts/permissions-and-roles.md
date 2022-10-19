# IAM Permissions and Roles

## Overview

Permissions and Roles are used to provide authorization controls for Hii Retail resources. Authorization is the process of granting
a User or Group of Users access to one or more resources.

## Permissions

Permissions determine what operations are allowed to be performed on a resource.
Permissions are represented in the form of `<service>.<resource>.<action>`.
For example, the permission `transaction.receipt.read` would allow
a Transaction (service) Receipt (resource) to be Read (action).

Every Hii Retail service has an associated set of permissions for each REST API method
that it exposes. The caller (e.g., User identity) of that method requires permission
to request that method in order for the request to succeed.
For example, if you are using `POST /api/v1/tenants/{tenantId}/groups` to add
a new Hii Retail Business Unit Group, your User must be granted the `bum.group.add` permission.

NOTE: You do not grant permissions to Users directly.
Instead, you use Roles that contain the appropriate Permissions, and then _bind_ those Roles
to a Group to which the User belongs.

## Roles

A Role is a named collection, which contains one or more Permissions
When a User attempts to access a Hii Retail API resource,
the Identity and Access Management service checks that User has the appropriate
Permission to perform the requested action. You can grant Permissions through
the use of Role bindings, where one or more Roles are bound to a Group which contains Users.

### Predefined Roles

Roles that are created and managed by Extenda Retail are known as _pre-defined_ roles.
These roles represent an opinionated view of the permissions commonly associated with
a retail function -- e.g., Cashier, Store Manager, Pricing Specialist, etc.
The Roles contain the Permissions which are typically required to fulfill the requirements
and responsibilities of the individuals who perform these functions.
Pre-defined Roles are accessible to _all_ Hii Retail Tenants.

### Custom Roles

When pre-defined Roles are not sufficient for granting the required permissions for performing
the responsibilities of a particular function, a Custom Role can be created.
This custom role is available only to the Hii Retail Tenant wherein it was created.
