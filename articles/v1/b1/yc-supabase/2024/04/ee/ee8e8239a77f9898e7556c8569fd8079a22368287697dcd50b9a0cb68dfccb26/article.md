---
schema_version: "1.0.0"
document_id: "ee8e8239a77f9898e7556c8569fd8079a22368287697dcd50b9a0cb68dfccb26"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/branching-publicly-available"
published_at: "2024-04-15T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:d81e5ff04f6a3c28ffa7b3d9351651835999b9d86160cf373a4058c45133e7ab"
---

# Branching now Publicly Available

tl;dr: Supabase Branching is now in open beta! You can enable it on any project that's Pro Plan or above.


## What is Branching?#


Branching is a seamless integration of Git with your development workflow, extending beyond your local environment to a remote database. Leveraging Git, particularly focusing on GitHub initially, each time a Pull Request is opened, a corresponding "Preview Environment" is spawned.


Preview Branches are essentially full Supabase instances. Every push triggers migrations from the` ./supabase/migrations` folder, ensuring team synchronization and a shared source of truth. When you merge a Pull Request, your migrations are applied to the Production database.


We[announced Branching](https://supabase.com/blog/supabase-branching) a few months ago in our previous Launch Week, with a deep dive on a few of the features like data seeding, integrations with Vercel, and seamless handling of environment variables. Since launching Branching for early-access we've worked with early users of all sizes. Today we're making Branching available to everyone.


## New Features#


Our open Beta introduces a number of requested features:


### Edge Function support#


Branching now deploys your Edge Functions along with your migrations. Any Functions added or changed in your` ./supabase/functions` will automatically be deployed without any extra configuration.


### Monorepo support#


You can now set a custom Supabase directory path which allows for monorepo support. You can also choose to only spin up new branches when there are changes inside your Supabase directory. See all the configuration settings in your projects[here](https://supabase.com/dashboard/project/_/settings/integrations) .


### Persistent branches#


We had quite a few users of branching request for long-running branches so we added the concept of persistent branches. In persistent mode, a branch will remain active even after the underlying PR merges or closes.


Please note that branches should still be treated as replaceable at any time. Persistent or ephemeral Branches should not be used for production data.


## Feedback#


A special thank you to all our early-access branching users who provided lots of actionable feedback. Our feature development was largely driven by the direct feedback from our users.


We still have many features to add to branching before 1.0, so please continue[sending us your feedback](https://github.com/orgs/supabase/discussions/18937) !


## Getting Started#


You can easily get started with Branching by following our[Getting Started Guide](https://supabase.com/docs/guides/platform/branching#how-to-use-supabase-branching) .
