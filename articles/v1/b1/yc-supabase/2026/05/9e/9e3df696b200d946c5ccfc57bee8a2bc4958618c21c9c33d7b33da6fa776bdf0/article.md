---
schema_version: "1.0.0"
document_id: "9e3df696b200d946c5ccfc57bee8a2bc4958618c21c9c33d7b33da6fa776bdf0"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/46084-self-hosted-supabase-making-analytics-and-vector-opt-in"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:1c4914bc8916589178393402379e87631c3a4dfe6cd98c56792ba27539e2ef3c"
---

# Self-hosted Supabase: making Analytics and Vector opt-in

## What's Changing?#


The week of June 1, 2026, the` analytics` (Logflare) and` vector` services will be removed from the default` docker-compose.yml` and moved into an opt-in overlay file:` docker-compose.logs.yml` (PR[#45327](https://github.com/supabase/supabase/pull/45327) ).


The default` docker compose up -d` will start a leaner stack without log aggregation. To keep using Logs Explorer in Studio, you'll need to include the overlay:


`
_10


docker compose -f docker-compose.yml -f docker-compose.logs.yml up -d


`


The base` docker-compose.yml` will also set` ENABLED_FEATURES_LOGS_ALL: false` on Studio by default, so the Logs menu item won't appear in the UI unless you bring up the overlay.


## Why?#


- Analytics (Logflare) is a heavy service in the default self-hosted Supabase stack. Making it an opt-in reduces CPU and memory footprint.
- Studio, Auth, Storage, PostgREST, and Realtime all work without` analytics` or` vector` .


## Am I Affected?#


You are affected if **all** of the following are true:


- You run self-hosted Supabase from the` ./docker` directory
- You pull updates from` master` without overriding the compose invocation
- You actively use Logs Explorer in Studio


You are **not** affected if you:


- Use the Supabase[platform](https://supabase.com/)
- Use the Supabase[CLI for local development](https://supabase.com/docs/guides/local-development) (` supabase start` ) - it's a different deployment option
- Don't use Logs Explorer (` analytics` and` vector` silently going away)
- Already maintain a customized compose configuration


## What Should I Do?#


### If you use Logs Explorer#


Add the overlay to your` docker compose` invocation:


`
_10


docker compose -f docker-compose.yml -f docker-compose.logs.yml up -d


`


You'll need to include both` -f` flags on every` docker compose` command (` up` ,` down` ,` logs` ,` ps` , etc.) for the overlay to apply consistently. If that's inconvenient, set` COMPOSE_FILE=docker-compose.yml:docker-compose.logs.yml` in your` .env` and the flags become implicit.


### If you don't use Logs Explorer#


No action needed. Pull the new compose files and` docker compose up -d` will just work, with a smaller resource footprint. Existing` analytics` and` vector` containers and their volumes will stop being managed - you can remove them once you've confirmed the stack is healthy:


`
_10


docker compose rm -f -s -v analytics vector


`


## Rollout#


Date Change


2026-05-18 This changelog published


2025-06-03 Default flips in the next self-hosted Supabase release
