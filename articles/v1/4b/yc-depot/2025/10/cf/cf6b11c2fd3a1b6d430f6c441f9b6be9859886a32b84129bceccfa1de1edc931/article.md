---
schema_version: "1.0.0"
document_id: "cf6b11c2fd3a1b6d430f6c441f9b6be9859886a32b84129bceccfa1de1edc931"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/node-20-deprecation-psa-for-depot-users"
published_at: "2025-10-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:f5396f42191a5cc6511b4e1a1b0f046218358233486156554214cd7d8cfffb44"
---

# Node 20 deprecation: PSA for Depot users

GitHub[recently announced](https://github.blog/changelog/2025-09-19-deprecation-of-node-20-on-github-actions-runners/) they're deprecating Node.js 20 support for GitHub Actions. If you're using Actions, you'll want to know what this means for your workflows and when you need to act.


The tl;dr: Node 20 Actions will stop working in summer of 2026. Depot will follow suit to maintain compatibility. If you don’t maintain any custom Actions, then nothing will noticeably change for you. Actions maintainers will need to update their Actions to use Node 24.


## What exactly is being deprecated?


GitHub is deprecating Node 20 as a **target** for authoring GitHub Actions. This means that if you've written custom Actions that specify` runs.using: 'node20'` in their` action.yml` , those Actions won't start anymore after the full deprecation hits next summer.


The recommended version going forward will be` node24` . If you're just using Actions (not authoring them), most of this change will be transparent to you; Action maintainers will handle the migration.


Here's what's **not** changing right away:


- Node 20 is still the default preinstalled version on most GitHub runner images.
- Ubuntu runners still default to v20.x.x, though some newer platforms like Windows 2025 and macOS 15 have already moved to Node 22 as the default.


GitHub is gradually updating the system-installed versions across all platforms, though they haven't announced exactly when Ubuntu will follow suit.


## What you need to do


For most developers: probably nothing immediately. If you're using popular Actions from the marketplace, their maintainers will handle the migration to Node 24. Some Actions may require a version bump, however. That shouldn’t be the case for any of Depot’s GitHub Actions.


If you maintain your own Actions:


1. Update your` action.yml` files to use` runs.using: 'node24'` .
2. Test thoroughly. While the Node APIs are mostly stable, there can be subtle differences.


## At Depot


Since we build our GitHub Actions runner images from GitHub's base images, we'll be explicitly following their deprecation timeline. When GitHub updates their system Node version to v24 by default, we'll do the same. We're also updating our images, such as our[setup action](https://github.com/depot/setup-action) , to make sure we maintain compatibility.


## FAQ


If an Action I depend on hasn't migrated to Node 24 by summer 2026, what happens?


The Action will fail to start, and your workflow will break at that step. Your best bet is to either open an issue with the Action maintainer well before the deadline, fork the Action and update it yourself, or find an alternative Action that's been updated. You could temporarily set ACTIONS_ALLOW_USE_UNSECURE_NODE_VERSION=true to buy time, but GitHub will remove Node 20 from the runner entirely later in summer 2026.


Can I test my Actions with Node 24 before the switch?


Yes. Set FORCE_JAVASCRIPT_ACTIONS_TO_NODE24=true as an environment variable in your workflow or on your runner machine. This forces all Actions to run on Node 24 instead of the current Node 20 default, letting you catch any compatibility issues early without waiting for the automatic migration.


## Related posts


- [How we automated GitHub Actions Runner updates with Claude](https://depot.dev/blog/how-we-automated-github-actions-runner-updates-with-claude)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Comparing GitHub Actions and Depot runners for 2x faster builds](https://depot.dev/blog/comparing-github-actions-and-depot-runners-for-2x-faster-builds)


Billy Batista


Software Engineer at Depot
