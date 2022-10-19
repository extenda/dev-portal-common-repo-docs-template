# Resources

## Overview

`Resources` are representation of an entity in the IAM Service.
`Resources` are needed to be able to make authorization decisions based
on `Resource` - some user has permission `X` for some `Resource`, but
no permissions on other `Resources`.

IAM does not restrict what a `Resource` can be. It can be any entity,
so authorization decision can be made, based on it.

Each resource id consists of - `{prefix}:{original-entity-id}`. Prefix are free text.

Entities, that are automatically synced as `Resources` into the IAM.
- Business Units (managed by BUM API). - format - `bu:{bu-id}` (example - `bu:92ounqxBmnVhub6U5H1r`).

## How to use

### Manage Resources

IAM API offers [Resource API](https://developer.hiiretail.com/api/iam-api#tag/Resources).
It lets you maintain resources (basic CRUD) list. (In most cases you won't need
to manage resources yourself, Extenda will manage them for you).

### Limit users to have permission only for some resource.

Navigate to IAM UI -> Groups (IAM UI can be found on Landing page).
Create a new group (or use existing). When add role, that contains needed permission.
Then on each role, you will be able to press *Add Business Units* button, that will
let you 'Bind' a resource (Business Unit in this case) to a role. Binding of resource to
a role, will restrict user access only to specified resource.

*IMPORTANT:* Users will have a combined resource bindings from all roles with the required permissions.
If there is a role with required permission and no bindings - user  will be granted access to all resources.

### Make Authorization decisions based on resources

There are 2 endpoints in IAM API, that support resources.

1. Get All permissions, based on User token for specified resource.

[The endpoint](https://developer.hiiretail.com/api/iam-api#operation/getOwnPermissions) can receive
optional parameter `resourceId`. If passed, endpoint will return only roles, that can be applied to
passed resource.

2. Get All resources, based on passed permission and User Token.

[The endpoint](https://developer.hiiretail.com/api/iam-api#operation/getResources) can receive
optional parameter `permission`. If passed, endpoint will only return resources, where user has
specified permission.

*IMPORTANT:* If you pass `permission` query parameter, endpoint will require `IAM User Token`.

## For internal developers (Developers from Extenda Retail)

Refer to internal [doc](https://github.com/extenda/engineering-iam-common/blob/master/docs/identity-and-access-management/internal/resourses.md)
