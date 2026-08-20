---
schema_version: "1.0.0"
document_id: "4f9e2a015770f8ba60563c7c40674bef936f049d943fae881f73bea31ac6c624"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/api-key-management-now-in-graphos-studio"
published_at: "2026-06-02T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:e1e3bad7a7aa45835914d0e4f4732d3af1d284378c6679075698f47dd4f46b7f"
---

# API Key Management now in GraphOS Studio

Today, we’re launching a new API keys page in[GraphOS Studio](https://www.apollographql.com/graphos/studio) , a single place where you can view and manage your SCIM, subgraph, and operator API keys directly from the UI. With built-in tools to filter by key status, cleanly rotate expiring keys with configurable buffer periods, and revoke unused keys, it empowers platform teams to maintain strict security oversight over their graph infrastructure without disrupting active development workflows.


Figure 1: *API keys page in GraphOS Studio*


## The problem it solves


API keys are the backbone of securing access to your GraphQL infrastructure. But managing them has historically been fragmented. Subgraph keys, operator keys, and SCIM keys lived in different places, and were only available through[Rover CLI](https://www.apollographql.com/docs/rover) and the[Platform API](https://www.apollographql.com/docs/graphos/platform/platform-api) .


If a key was compromised or a token needed to be rotated out, your options were binary: delete it and lose the audit trail, or leave it active and accept the risk. Now, you can rotate or revoke a key so that it cannot be used, but you can continue to view it in your list of API keys.


## What we built


### View, search, and filter API keys at a glance


SCIM, subgraph, and operator keys are now visible in Apollo Studio. Search by name, filter by type or status, and quickly find the key you’re looking for.


Figure 2: *Filter API keys by type*


### Create, rename, and update expiration


Need a new API key for an upcoming deployment? As long as you have the appropriate permissions you can now spin one up without leaving the browser. You can also rename existing keys and adjust their expiration dates directly from the UI, keeping your key inventory clean and descriptive as your organization grows.


*Figure 3: Create a new API key*


### Rotate API keys without downtime


The new rotate action lets you cycle out a key while keeping it accessible for investigation or a transition period, so you can maintain continuity without sacrificing oversight. No more choosing between security and uptime.


Figure 4: *Rotate API key*


### Revoke vs. delete API key


Sometimes you need to immediately cut off access to a key, but you’re not ready to erase it entirely. Revoking a key stops it from being used right away while preserving the record, useful for incident response, audits, or investigations where visibility matters. Deleting a key remains available when you’re ready to permanently remove it.


Figure 5: *Revoke API key*


## Getting started


Access to the API keys page is based on your role in GraphOS Studio. Graph admins can manage subgraph keys for their associated graphs. Org Admins have full access to all key types across the organization. Want to learn more? Check out our[documentation](https://apollographql.com/docs/graphos/platform/access-management/api-keys/manage-api-keys-studio) for detailed setup instructions.


Rover CLI users, this doesn’t change your workflow. Rover CLI continues to support creating, listing, deleting, and renaming subgraph and operator API keys. The Studio UI is an additive surface, not a replacement.


## What’s next


The[API keys page](https://apollographql.com/docs/graphos/platform/access-management/api-keys/manage-api-keys-studio) is the foundation for a broader vision: a unified view of every key in your Apollo organization. Currently, graph and user API keys are available on separate key management pages, but in the future they will be available on the API keys page. You’ll be able to see and manage personal, graph-level, and org-level keys side by side. We’re also working on getting all of these new capabilities into Rover CLI.


Written by


Meryl Charleston


[Read more by Meryl Charleston](https://www.apollographql.com/blog/author/meryl-charlestonapollographql-com)
