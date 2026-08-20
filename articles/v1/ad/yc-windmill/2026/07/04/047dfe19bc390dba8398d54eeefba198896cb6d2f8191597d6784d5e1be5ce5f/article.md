---
schema_version: "1.0.0"
document_id: "047dfe19bc390dba8398d54eeefba198896cb6d2f8191597d6784d5e1be5ce5f"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/workspace-forks-on-cloud"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:e3509185781bc6d62d09bbe4768b3f92e28b5a7fba1de8cd67a249a530e9282d"
---

# Workspace forks on Windmill Cloud

### [Workspace forks on Windmill Cloud](https://www.windmill.dev/changelog/workspace-forks-on-cloud)


Workspace


Cloud


[v1.746.0](https://github.com/windmill-labs/windmill/releases/tag/v1.746.0)


[Docs](https://www.windmill.dev/docs/advanced/workspace_forks#forks-on-windmill-cloud)


Workspace forks are now available on app.windmill.dev for workspaces on a paid plan. A fork inherits its parent (root) workspace's premium status and usage limits, and its executions are metered into the parent's billing. Free-tier workspaces cannot fork, and the number of forks is capped at 5 per paid developer seat of the root workspace.


#### New features


- Fork workspaces on Windmill Cloud for paid (Team or Enterprise) workspaces.
- Forks inherit the parent (root) workspace plan and usage limits.
- Fork executions are metered into the parent workspace billing.
- Fork count capped at 5 per paid developer seat of the root workspace.
- Free-tier workspaces cannot create forks on cloud.
