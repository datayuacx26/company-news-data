---
schema_version: "1.0.0"
document_id: "cd6ae63747dc964804b63bd5b11d456583896af79519f44f5db62fcdc8982f71"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/automatic-git-to-windmill-sync"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:e452aa9f9fb707fbbf49b9f2a8416c629e06d40e01687762140a9fbc21844f0c"
---

# Automatic git-to-Windmill sync

### [Automatic git-to-Windmill sync](https://www.windmill.dev/changelog/automatic-git-to-windmill-sync)


Git sync


[Enterprise](https://www.windmill.dev/pricing)


[v1.761.0](https://github.com/windmill-labs/windmill/releases/tag/v1.761.0)


[Docs](https://www.windmill.dev/docs/advanced/git_sync#automatic-sync-from-git)


The Git to Windmill direction of git sync now works fully in-app, making the documented GitHub Actions optional. Enable 'Automatically deploy changes from Git' on a repository and new commits to the tracked branch deploy into the workspace, instantly via webhooks for GitHub App repositories (with polling as a safety net) or by polling about every minute for token repositories. Windmill can also open pull requests for promotion and fork deploy branches, post a 'Windmill diff' check and managed comment on pull requests showing what merging would deploy, post a deploy status check on synced commits, and automatically deploy fork branch changes into fork workspaces from a single parent-level toggle.


#### New features


- Per-repository 'Automatically deploy changes from Git' toggle: new commits on the tracked branch deploy into the workspace, no CI/CD pipeline needed.
- GitHub App repositories (managed and GHES self-managed) sync instantly via an HMAC-verified webhook that Windmill registers itself; polling stays on as a safety net.
- Token-based repositories sync via polling, checking the tracked branch about every minute; commits made by Windmill (\[WM\] prefix) are skipped to prevent loops.
- In-app pull request creation: 'Open a pull request for each deploy branch' on promotion repositories and 'Open a pull request when an item is deployed in a fork' on the parent sync repository, replacing the gh pr create GitHub Actions.
- Pull requests targeting the tracked branch get a 'Windmill diff' check run and a managed comment showing what merging would deploy, updated on each push.
- Tracked-branch deploys post a 'Windmill' check run on the commit that flips from 'Deploying…' to 'Deployed N changes'.
- Parent-level 'Automatically sync forks with git branches' toggle deploys changes on each fork's wm-fork/** branch into the matching fork workspace, replacing the push-on-merge-to-forks action.
