# IAM Tenants

## Overview

Hii Retail is a multi-tenant retail platform.
A Hii Retail Tenant represents an isolated resource boundary on the platform.
A Tenant can manage their resources but cannot access another Tenant's resources in any way.
A single Tenant is typically created for each Hii Retail customer. However, a customer
can request additional Tenants to be created, for example, one for each brand or chain belonging to the customer.
Only Extenda Retail administrative personnel are authorized to create a Tenant. Hii Retail customers are unable
to create a Tenant themselves. There is no concept of a sub-Tenant within Hii Retail.
Also tenant can have their own [test environment](./test-tenants.md)

### Tenant Admin

When a Hii Retail Tenant is created for a new customer, a single User account exists.
This account is referred to as the Tenant Admin. This account can be thought of as a _root_ user,
as is granted access to perform _all_ actions on _all_ IAM resources within the Tenant.
More information regarding the Tenant Admin User, including important usage warnings,
is available [here.](./user-groups.md#tenant-admin-user)
