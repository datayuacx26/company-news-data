---
schema_version: "1.0.0"
document_id: "98b00f683d6a32503569ef86cac13236fe3369b3b9b76026e05d436454a0edf1"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/dev-workspaces"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:589ff02fa65e04f42e0aacf1c2f73a62608bb5085651cacfb655a266f1129a08"
---

# Dev workspaces paired with a lockable prod workspace

### [Dev workspaces paired with a lockable prod workspace](https://www.windmill.dev/changelog/dev-workspaces)


Workspace


[Enterprise](https://www.windmill.dev/pricing)


[Docs](https://www.windmill.dev/docs/advanced/dev_workspaces)


Pair a persistent dev workspace with a prod workspace that can be locked so changes only reach it through promotion. Edit safely in dev, mark resources and variables workspace-specific to keep separate values per environment, and promote manually from Compare & Deploy.


#### New features


- Persistent dev workspace with a plain, git-branch-safe workspace ID (no wm-fork- prefix)
- Lock the paired prod workspace against direct deploys and forking (reuses protection rulesets)
- On a locked prod, Edit redirects to the dev workspace with an "Edit in <dev>" affordance
- Attach or detach an existing workspace as dev from the Dev workspace settings tab
- Mark resources and variables workspace-specific from Compare & Deploy to keep separate values per environment
- Strictly create-only "Create in <other>" seeds a missing copy (including secrets) without overwriting
- AI chat session picker badges and steers toward the dev workspace on a locked prod
