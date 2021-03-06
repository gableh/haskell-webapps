# What are we building

A typical shopping-cart (ecommerce) webapp. But it isn't the entire app with all bells and whistles. Just a few features - enough to uncover a "Minimum Viable Architecture" (that leverages the Haskell type-system to good use).

# Things to cover in the spec

| Requirement / Case / Scenaro | How is it covered in the spec? |
| --- | --- |
| Authentication | Peristen login cookie which needs to be periodically refershed for a session token (which expires) |
| Type-safe authorization | `roles` table, which has a list of permissions. Permissions are pre-defined by the app, but the tenant can group any permission to define a new role |
| Siged-out (not logged-in) operations | End-customer visiting the storefront |
| Signed-in operations for users with different priveleges | Editing various fields in a product can require different set of permissions |
| Domain-level operations that require DB transactions | (a) Creating & editing a product with variants, (b) changing any record in the DB along with audit logs |
| Complex web-form with validations and error messages | (a) Product creation/editing (b) Image uploads |
| Searching the app's core data based on user-input | (a) Admin-side product listing page (b) End-customer facing product listing page |
| Sending out plain text & HTML emails with attachments | Tenant activation email. We can attach a logo inline for use by the HTML part of the email |
| ENUM support | `tenant_status`, `user_status`, `product_type`, `weight_unit` |
| JSONB support | `audit_logs.changes` and `products.propertes` |
| Array support | `roles.permissions` |
| 1:1 association | Tenant:Account-owner |
| 1:many associations | (a) Product:variant, (b) Product:Photo, (c) Variant:Photo |
| many:many associations | `users_roles` join-through table |
| Fields in response JSON should depend in incoming request | `/products/:id?fields=` [discussion](https://github.com/vacationlabs/haskell-webapps/issues/10) |
| Redis caching at object-level | Individual product JSONs should be cached in Redis **TODO: Discussion** |
| Redis caching at page-level | Final HTML of individual product pages should be cached in Redis **TODO: Discussion** |
| Audit logs | `audit_logs` table |
| How to deal with runtime errors in production | [Integrate with existing error management tools](https://github.com/vacationlabs/haskell-webapps/issues/13)
| Unit tests | **TODO** |
| Controller tests | **TODO** |
| Integration tests | **TODO** |
| Static assets during development | **TODO** |
| Deployment | **TODO** |
  
# Domain-Level API to be implemented in Phase 1

* Tenant creation
* User creation
* Tenant activation along with assigning owner
* Simple email+password based authentication
* Get list of products, with search, sort, and pagination
* Get single product details (fixed/pre-decided JSON response)
* Get single product details with fields specified in the request (variable JSON response, depending upon the request)
  * Only privileged users can get access to the following fields: inventory count, cost price
* Create a new product
  * Requires "product admin" privileges
  * Upload & auto-crop images to various geometries
* Modify an existing product
  * Tags
  * Variants
  * Images
  * Only 

