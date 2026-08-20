---
schema_version: "1.0.0"
document_id: "e5b0639f816a946d34916b0e6c7ba6870afcaf385b794459d3d8f96780b10112"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/46346-feature-preview-temporary-token-based-database-access"
published_at: "2026-05-25T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:f619174aba2c65e0e3fa42dbc8728874fb9dc9400d79b841265e5a0ecfbe88a7"
---

# Feature Preview: Temporary token-based database access

### Connect to your database using a temporary access token#


We are enabling an experimental feature that allows direct, temporary database access using user tokens, such as Personal Access Tokens (PATs).


This feature allows giving developers direct database access, as a specific role, without ever disclosing the database password.


Project administrators specify the database role a project user is allowed to access the database as, and the time period for which that access is valid. Because the credential (token) is tied directly to a specific user, it will be possible to see who accessed the database, and with which role. Revoking project access from a developer, immediately revokes their ability to log into the database.


This setting is only available to project **owners** and **administrators** .


## Enabling the feature preview#


Database users must be enabled through the feature preview:[https://supabase.com/dashboard/org/_/?featurePreviewModal=supabase-ui-jit-db-access](https://supabase.com/dashboard/org/_/?featurePreviewModal=supabase-ui-jit-db-access)


## Configure access#


By default, Temporary token-based access is disabled and must be enabled on a per project basis. In your project’s database settings, enable Temporary token-based access:


[https://supabase.com/dashboard/project/_/database/settings](https://supabase.com/dashboard/project/_/database/settings)


Once enabled, you can grant other team members access. This is managed through the` Add rule` button. Access control is fine grained per database role. A user can be granted access to one or more database roles. Expiry is tied directly to the role and can be scoped down to minutes or a maximum of 90 days.


### Branches#


Temporary access is fully supported in branch projects. When enabling temporary access on the main project, temporary access will also be enabled for all existing and future branches.


Users access can be scoped to **branches only** .


Temporary access is particularly helpful when dealing with branches, as you don’t need to know or modify any passwords, your Personal Access Token is sufficient for gaining access.


## Disabling#


Temporary token-based access can be disabled for all users at once through the settings screen. Users will regain their previous access if you re-enable Temporary token-based access. Individual user’s access can be controlled by modifying the rule for that specific user.


## Connecting to the database with an Temporary token#


Once granted access, users can access the database using their Personal Access Token (PAT). Access works through both the Shared Pooler and directly to the database. Users will only be able to access database roles for which they have been granted access.


Shared pooler:


`
_10


psql "postgres://{role}.{database_ref}@aws-1-{region}.pooler.supabase.com:6543/postgres?sslmode=require&options=-c%20jit%3dtrue"


`


Direct access:


`
_10


psql "postgres://{role}@db.{database_ref}.supabase.co:5432/postgres?sslmode=require"


`


### Current limitations#


This feature is **only** available to projects on Postgres 17+. Older Postgres versions are not, and will not, be supported.


Temporary token-based access requires a user to be a valid member of the project. At present it is not possible to give access to users that are not part of your project. We are working on adding support for non-project members to be invited. This will allow granting database access to an external contractor, for example, while not making them part of your Supabase project or organization.


Temporary token-based access is not available through the dedicated pooler (port` 6543` on the database host).
