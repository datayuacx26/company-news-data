---
schema_version: "1.0.0"
document_id: "3c60e332fe0017f2f48d8d3ef97e1bf5e9ea813a13d790daabb0ea5691b60b23"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/merge-into-any-workspace"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T17:41:07.360869+00:00"
fetched_at: "2026-07-31T17:41:08.668363+00:00"
content_hash: "sha256:e714b2cb8d8703c79ec828bdbc096ef8c210038b1a2d622678416f923879893c"
---

# Compare & Deploy into any workspace

### [Compare & Deploy into any workspace](https://www.windmill.dev/changelog/merge-into-any-workspace)


Workspace


[Docs](https://www.windmill.dev/docs/advanced/workspace_forks#merging-into-a-workspace-outside-the-lineage)


Compare & Deploy now accepts an arbitrary target workspace, not just the parent. Pick it from the destination badge in the merge header, compute a full diff over both workspaces, and deploy the items you select one way into the target. Requires being an admin of both workspaces. The Deployment UI settings tab is now part of Dev workspace.


#### New features


- Pick any workspace you administer as the deployment target from the destination badge, or with /forks/compare?target=<workspace id>
- A pair outside the fork lineage has no continuously tracked diff: compute one explicitly, then Recompute to refresh it - the card shows when it was last computed
- Comparisons outside the lineage are one-way (current workspace into the target), so the update direction and conflicts are hidden
- Items that exist only in the target are flagged "Removes in target" and must be selected one by one
- A workspace with no parent lands directly on the target picker
- The Deployment UI settings tab, with its deployable-item filters, moved under Dev workspace
