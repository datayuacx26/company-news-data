---
schema_version: "1.0.0"
document_id: "cbe07187ba97cdef92497b1666205d5f0e9874fc949ba829a35adc5e8f654741"
company_key: "yc-jitsu"
company: "Jitsu"
source_id: "yc-jitsu-news-import-65f0e2b767a6"
canonical_url: "https://jitsu.com/blog/openapi-and-cli"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-22T00:57:29.784311+00:00"
fetched_at: "2026-07-28T21:45:24.644708+00:00"
content_hash: "sha256:2e3f82f428a268ad6b12b4bbf99249802f72d7023c8737953a87f30163261e4d"
---

# Jitsu, now programmable: Management API, CLI, and ready for AI agents

## TL;DR


Three things shipped today:


- A **public Management API** for every Jitsu instance, with a full OpenAPI 3.0 reference at[docs.jitsu.com/api](https://docs.jitsu.com/api) .
- **` jitsu-cli`** — a real command-line interface that does everything the UI does.
- An API surface designed so that **AI agents can run it on your behalf** — and a` llms.txt` manifest at[docs.jitsu.com/llms.txt](https://docs.jitsu.com/llms.txt) so they can find the docs.


If you've ever wanted to ask Claude or Cursor to "spin up a new destination, point our checkout events at it, and only forward` purchase` events" — that's a one-paragraph prompt now.


## Why this took us a while


Jitsu has always had an internal API — the Jitsu UI is just a client of it. But "internal" and "public" are not the same thing. A public API is a contract: stable URLs, typed inputs, real error messages, versioned schemas, an honest reference doc that doesn't drift from the code.


We rebuilt the inside of the API to make that contract real. Every endpoint is now declared with the same Zod schemas the server uses for validation. The OpenAPI reference is generated from those schemas at build time — there is no second source of truth, no hand-written docs to rot. When the code changes, the docs change.


## The CLI


` jitsu-cli` is what we always wanted to give you. Install it, log in once, and you can stop clicking:


```text
npm i -g jitsu-cli
jitsu-cli login
jitsu-cli set-default-workspace my-workspace


```


Now you can do real work from the terminal:


```text
# inventory
jitsu-cli config destinations list
jitsu-cli config streams list


# create a destination from a JSON file
jitsu-cli config destinations create --from-file ./destinations/snowflake-prod.json


# wire a stream to it
jitsu-cli config connections create \
--from snowflake-prod \
--to events-stream


# tear it down (with cascade if it's referenced)
jitsu-cli config destinations delete legacy-bq --cascade


```


The CLI calls the same Management API under the hood, so anything you can script with` curl` , you can do with` jitsu-cli` — and vice versa.


## Infrastructure as code, finally


Check your Jitsu config into git. Apply it from CI. Promote it between environments. The pattern that's been standard for the rest of your stack is now possible for your data pipeline.


```text
# .github/workflows/jitsu-apply.yml
- run: |
jitsu-cli login -h https://use.jitsu.com -k "${{ secrets.JITSU_KEY }}"
for f in destinations/*.json; do
jitsu-cli config destinations upsert --from-file "$f"
done


```


No more "who changed prod last Tuesday" —` git log` answers it.


## Built for agents


Here's the part we're most excited about.


Modern coding agents — Claude Code, Cursor, Codex, the home-grown ones your team is already writing — are very good at calling well-documented HTTP APIs. They are bad at clicking through admin panels.


We designed this release with that in mind:


- **One canonical reference.** Every endpoint lives at[docs.jitsu.com/api](https://docs.jitsu.com/api) , with request/response schemas, examples, and the same Bearer-token auth model across the surface. An agent can read it once and be productive.
- **` llms.txt` .** The docs site serves a[llms.txt](https://docs.jitsu.com/llms.txt) index and a full-text bundle. Drop them into your agent's context and it has the whole API surface in one request.
- **Predictable shapes.** Configuration objects (destinations, streams, services, connections, syncs, functions) all follow the same` list / get / create / update / delete` pattern. Once an agent has the shape of one, it has the shape of all of them.
- **Honest errors.** Validation errors come back as structured JSON with the field path. Agents can fix their own mistakes without you in the loop.


A prompt that works today:


> *"Create a new ClickHouse destination called` analytics-eu` . Use the credentials in` ~/.jitsu/clickhouse-eu.env` . Connect it to the` web-events` stream, but filter to only` purchase` and` signup` events. Use the existing` pii-redact` function on the way in."*


Drop that into Claude Code in a repo with` jitsu-cli` installed. It reads the docs, calls the right endpoints in the right order, and reports back. The same prompt would have taken twenty clicks across three pages in the UI.


This isn't a future plan. It works now. We use it ourselves.


## What's covered today


The first cut of the public surface covers the configuration objects most people touch every day: **workspaces, destinations, streams, services, connections, syncs, functions, and profile builders** . That's the 90% of what the UI does.


Endpoints for sources, reports, schema, and SQL are still internal — they work, they're just not yet in the public spec. We'll roll them in next.


## Get started


- Install:` npm i -g jitsu-cli`
- Log in:` jitsu-cli login`
- Reference:[docs.jitsu.com/api](https://docs.jitsu.com/api)
- CLI docs:[docs.jitsu.com/jitsu-cli](https://docs.jitsu.com/jitsu-cli)


Tell us what you build. We read every Slack message and GitHub issue.
