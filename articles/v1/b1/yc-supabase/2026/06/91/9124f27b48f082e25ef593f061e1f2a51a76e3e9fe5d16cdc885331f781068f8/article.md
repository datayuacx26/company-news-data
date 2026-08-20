---
schema_version: "1.0.0"
document_id: "9124f27b48f082e25ef593f061e1f2a51a76e3e9fe5d16cdc885331f781068f8"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/47197-log-connections-is-to-be-turned-off-by-default-for-new-projects-and-existing-free-pro-projects"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:dd7937dd61f6535332bf914dd1bf2e49d6891788486289f2ea50d757f35257f9"
---

# log_connections is to be turned off by default for new projects and existing Free/Pro projects

We are changing the default value of the Postgres` log_connections` setting from on to off for new projects, and will be migrating all Free and Pro projects to the new default configuration. This reduces log volume and noise, and aligns with industry-standard defaults (RDS, GCP Cloud SQL also default to off). We will be exposing this configuration to all users via dashboard and API for better log management, with` log_connnections` and` log_disconnections` now available via the[Update postgres config API route](https://supabase.com/docs/reference/api/v1-update-postgres-config) and will take effect from 9 July onwards.


### Who's affected#


- **New projects** (all tiers) created after 9 July will have` log_connections=off` by default.
- **Existing Teams + Enterprise customers (including HIPAA add-on customers)** - Configuration is unchanged
- **Existing Free / Pro customers** -` log_connections` will be turned off from 9 July. This configuration can be re-enabled below


### How to re-enable#


You can re-enable` log_connections` via the dashboard under Database > Settings. It can also be re-enabled programmatically via the Management API via the[Update postgres config API route](https://supabase.com/docs/reference/api/v1-update-postgres-config) . Changes will only take effect from 9 July onwards.


Learn more about PostgreSQL logging settings in our[logging documentation](https://supabase.com/docs/guides/platform/postgres-connection-logging) .
