---
schema_version: "1.0.0"
document_id: "ba6c7e8ffd696f1c002a352d8f73cb8629db6ce0bee46ec721835fd64f9eda0f"
company_key: "yc-buildbuddy"
company: "BuildBuddy"
source_id: "yc-buildbuddy-rss-4f82164f35c8"
canonical_url: "https://www.buildbuddy.io/changelog/workflow-auto-cancel"
published_at: "2026-05-04T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:16.628240+00:00"
fetched_at: "2026-07-28T22:15:26.754194+00:00"
content_hash: "sha256:6f197cb849d348dfcd5d6ee610c3aafedcc1ca768bcb23c78f69e563d5ba1ef8"
---

# Auto-cancel duplicate CI runs on the same branch

Starting June 1, 2026, BuildBuddy will automatically cancel in-progress Workflow runs when a newer run is triggered for the same action on a non-default branch. This helps avoid wasting resources on outdated runs when, for example, several commits are pushed in quick succession to a pull request branch.


This behavior only applies to non-default branches. Runs on your repo's default branch are not affected.


If you'd like to disable this behavior and allow concurrent runs for an action, set` allow_concurrent_runs: true` in the action's configuration.


See the[Workflows configuration docs](https://www.buildbuddy.io/docs/workflows-config#concurrent-workflow-runs) for more details.
