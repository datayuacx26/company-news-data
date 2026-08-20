---
schema_version: "1.0.0"
document_id: "e658a4ea831e0e8298d7487774349f4e695ad18e26ea5a84cb0f4adbd5cb9195"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/launch-week-git-sync"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:07.070377+00:00"
fetched_at: "2026-07-28T22:16:03.530620+00:00"
content_hash: "sha256:89206779bab7db6de9be87d5495645fb6e4819cdbab0d5e6956371e7d55e91a4"
---

# Git sync & workspace forks: your entire workspace, version controlled

**Day 4 of[Windmill launch week](https://www.windmill.dev/launch-week-march-2026) .** We have reworked Git sync and introduced workspace forks, giving you a full staging-to-production workflow inside Windmill.


## The problem​


Windmill workspaces are live environments. You deploy a script, and it runs in production immediately. That works for small teams iterating fast, but as your team grows you need review, staging, and rollback.


Teams had to build custom CI/CD pipelines around Windmill's CLI sync, managing branches manually, and writing their own merge logic. The deploy-to-prod path was functional but required too much glue.


## Git sync: automatic, bidirectional​


Every time you deploy an item in Windmill, it automatically commits and pushes to your configured Git repository. Pull from Git to update your workspace. Supports GitHub, GitLab, Bitbucket, and Azure DevOps.


Configure Git sync from workspace settings. Path filters let you sync only what matters. Type filters control which resource types are included.


## Workspace forks: branch your workspace​


Workspace forks let you create an independent copy of a workspace for feature development. Changes in the fork do not affect the parent until you merge them back.


When Git sync is enabled, creating a fork automatically creates a corresponding Git branch (` wm-fork/<parent-branch>/<fork-name>` ). You get parallel development with full version control.


### Merge workflow​


Three ways to bring changes back:


1. **Deploy UI** : deploy individual items from the fork to the parent workspace directly from the Windmill UI.


1. **Merge UI** : merge all changes at once with conflict detection, no Git sync required.


1. **Git merge** : use your preferred Git workflow (PRs, code review) to merge the fork branch.


### Coming soon: data table forking​


Workspace forks will soon support[data tables](https://www.windmill.dev/docs/core_concepts/persistent_storage/data_tables) as well. When forking a workspace, you will be able to clone a data table's schema only or its schema and data, letting you develop against a separate database without affecting upstream workspaces like production.


On merge, Windmill queries the full data table schema, detects differences, and generates SQL migrations automatically.


## Why we built it this way​


Three design choices drove the architecture:


**Workspace-level branching.** Forks operate at the workspace level, not the file level. When you fork, you get a complete copy of all scripts, flows, apps, resources, and variables. This means you can test changes end-to-end in an isolated environment before merging.


**Git as the source of truth.** If you need to roll back, reset the branch. If you need to audit, read the commit history. Windmill does not replace your Git workflow; it plugs into it.


**Multiple deployment paths.** Not every team needs the same workflow. Small teams can use the deploy UI. Growing teams can use workspace forks. Enterprise teams can use the full Git promotion workflow with separate dev and prod workspaces, CI/CD, and cross-instance deployment. See[Canonical deployment setups](https://www.windmill.dev/docs/advanced/canonical_deployment_setups) for step-by-step guides on each approach.


## Deployment options​


Workflow Setup Best for


**Draft and deploy** Single workspace Small teams, fast iteration


**Workspace forks** Fork + merge Teams that need staging


**Git promotion** Git sync + CI/CD + PRs Enterprise, cross-instance


**Deploy to prod UI** Multi-workspace Cloud/EE, quick deployments


## Getting started​


**Git sync:**


1. Create a Git repository.
2. Go to workspace settings, then Git sync.
3. Configure authentication (GitHub App, PAT, or GitHub Enterprise App).
4. Deploy an item and check your repo for the commit.


**Workspace forks:**


1. Enable Git sync (recommended but optional).
2. Create a fork from the workspace settings.
3. Make changes in the fork.
4. Merge back using the deploy UI, merge UI, or Git.


[Git sync Sync your workspace to a Git repository.](https://www.windmill.dev/docs/advanced/git_sync)[Workspace forks Fork workspaces for feature development.](https://www.windmill.dev/docs/advanced/workspace_forks)


## What's next​


Tomorrow is Day 5: **Workflow-as-code** . Define complex workflows entirely in code with the next generation of our SDK.[Follow along](https://www.windmill.dev/launch-week-march-2026) .


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
