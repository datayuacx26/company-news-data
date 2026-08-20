---
schema_version: "1.0.0"
document_id: "c0f709a0d8f034b2559622330241c1dbc188440472ae7c63a15e94c527ea1b9d"
company_key: "yc-gumloop"
company: "Gumloop"
source_id: "yc-gumloop-news-import-488d75a2156b"
canonical_url: "https://www.gumloop.com/blog/sync-skills-between-github-and-gumloop"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-23T22:29:12.253632+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:343c53bb5de29f82ff996c23c43b5ffedb2adf4df7fcf9cfb45b6ce6a3cb1c24"
---

# Sync skills between GitHub and Gumloop

[Skills](https://www.gumloop.com/blog/announcing-skills-for-agents) teach agents how your company works, by giving them reusable knowledge that they can apply at scale, every time they run. You can create and manage skills in Gumloop, but since skills exist as a common markdown file format, they work across multiple platforms. In fact, your team might have already collected a number of skill files that live outside of Gumloop, that you use in other apps.


If that describes your team, good news: **you can now connect a GitHub repo as the source of truth for your Gumloop agents’ skills.** By connecting a GitHub repo, you can manage your skills with the pull requests, code review, and version history features of GitHub that your team already uses.


‍


When you edit a skill in the linked GitHub repo, open a PR, get it reviewed, and merge it, Gumloop will then pull the canonical skill definitions from the repo, version the changes, and automatically distribute the approved changes to agents across your entire organization. Skills still run in Gumloop exactly as before, but they’re sourced from GitHub so that changes to skills can move through your team’s existing code-review process.


## What this unlocks


Since you can already create and manage skills for your organization in Gumloop, you might be wondering: why would I want to connect a GitHub repo for skills?


- **Skills now follow your team’s existing review and approval flows** : if your skills are in GitHub, changes to skills can go through PRs the same way production code does. Changes to skills get reviewed and approved before reaching your agents.
- **Version control and a full audit trail in GitHub** : changes to skills are commits, so you can see who changed what, when, and why, and roll back changes with a simple \`git revert\`.
- **Meet your developers where they already work** : your engineers live in GitHub and their IDE. By syncing GitHub to Gumloop, your engineers can iterate on skills in their preferred editors, commit changes alongside the rest of your code, and let the changes propagate automatically into Gumloop. No platform-switching necessary!
- **Your skills stay yours** : when skill definitions live in your GitHub repo, you get full portability, and the ability to use the same skills across Gumloop *and* other agent platforms.


## How to set up the GitHub skills sync


To set up the sync, an org admin connects an authorized GitHub repository, Gumloop syncs the skill definitions from the repo, and the managed skills become available to agents across the organization. Changes merged in GitHub sync automatically, and version history is stored.


GitHub skills syncing is available now for Pro and Enterprise plans.[Read the docs to learn more.](https://docs.gumloop.com/core-concepts/organization_skills#connect-a-github-repository)
