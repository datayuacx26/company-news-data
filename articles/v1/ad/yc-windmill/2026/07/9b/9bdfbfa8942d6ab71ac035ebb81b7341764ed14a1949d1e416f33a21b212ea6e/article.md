---
schema_version: "1.0.0"
document_id: "9bdfbfa8942d6ab71ac035ebb81b7341764ed14a1949d1e416f33a21b212ea6e"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/fork-lineage-deploy-target"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-08-03T23:21:06.717996+00:00"
fetched_at: "2026-08-05T03:48:36.198781+00:00"
content_hash: "sha256:2d4b68fda0f6e4654c4fe8a384164b8450a9e3ca1d849f9af78171e9a0729d22"
---

# One deployment target, derived from the workspace lineage

### [One deployment target, derived from the workspace lineage](https://www.windmill.dev/changelog/fork-lineage-deploy-target)


Workspace


[Docs](https://www.windmill.dev/docs/core_concepts/staging_prod#upgrading-from-the-deploy-target-setting)


The workspace-level "deploy to" setting is gone. A workspace now deploys into its parent - the workspace it was forked from, or the prod workspace its dev workspace is paired with - so the deployment target has a single definition instead of two that could disagree. There is nothing to do on upgrade to Windmill 1.776.0, since a staging workspace that was the only one deploying into prod simply becomes prod's dev workspace. Only two shapes need a look, and both are named in the migration logs. Job tags became lineage-aware at the same time, so $workspace in a tag resolves to the nearest ancestor workers are actually provisioned for.


#### New features


- A workspace deploys into its parent: a fork into the workspace it was forked from, a dev workspace into the prod workspace it is paired with. There is no longer a setting linking two otherwise unrelated workspaces
- The Deployment UI settings tab is gone. Its deployable-item filters live under Workspace settings -> Dev workspace, alongside the now read-only target
- Nothing to do on upgrade for the usual setup: a staging workspace that was the only one deploying into prod becomes prod's dev workspace and keeps its own job tags and promotion mode
- Only two shapes need a look, both named individually in the migration logs: several workspaces pointing at one target (only one can be its dev workspace, the rest become plain forks), and a link the lineage cannot express at all (target missing or archived, self-reference, source already a fork of something else, cycle), which is dropped
- Either way the fix is a single attach of the staging workspace as the dev workspace of the prod workspace - there is no other setting to reproduce
- $workspace in a job tag, and per-workspace default tags, now resolve to the nearest ancestor an admin would provision workers for, instead of a generated fork id no worker serves
- For a one-off deploy into an unrelated workspace, pick an explicit target from Compare & Deploy instead
- The wmill settings.yaml no longer carries deploy_to; an older file that still has it is ignored rather than rejected
