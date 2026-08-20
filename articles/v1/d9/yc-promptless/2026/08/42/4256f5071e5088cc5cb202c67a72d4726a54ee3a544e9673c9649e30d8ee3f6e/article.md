---
schema_version: "1.0.0"
document_id: "4256f5071e5088cc5cb202c67a72d4726a54ee3a544e9673c9649e30d8ee3f6e"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/gitlab-merge-request-comment-trigger/"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-11T00:25:09.203102+00:00"
fetched_at: "2026-08-11T00:25:10.814368+00:00"
content_hash: "sha256:da13b98a80ad5a4d601cf6990030c813593fc9271489c0bccd866f6d4664a651"
---

# Comment @promptless on a GitLab Merge Request to Request Documentation

# Comment @promptless on a GitLab Merge Request to Request Documentation


[← Back to Blog](https://promptless.ai/blog)


Mention` @promptless` in a GitLab merge request comment and Promptless reviews the MR and opens a documentation suggestion. It works on any project your GitLab connection has read access to, whether or not that project is in your trigger configuration.


This brings GitLab comment-mention triggering to parity with the existing GitHub behavior.


## The problem


Section titled “The problem”


Documentation trigger rules cover the routine case. You configure them once: every MR in these projects, on open, or on merge. They run automatically without thinking about it.


The gaps show up when documentation needs arise outside the routine. A reviewer reading an MR notices the behavior it introduces isn’t documented. A tech writer participating in a review wants to request a suggestion while the context is fresh. An older MR gets referenced in a ticket and someone wonders whether it was ever documented.


Previously, GitLab teams had no way to ask Promptless for documentation from inside the MR. They’d have to open the Promptless dashboard and create a task manually, or add the project to their trigger config and wait for the next event. Either way, the request was disconnected from the conversation where it came up.


## What it does now


Section titled “What it does now”


Leave a comment on any GitLab merge request with` @promptless` in the text. Promptless reads the MR, identifies relevant sections of your docs, and opens a suggestion PR.


Specifics:


- **Matching is case-insensitive.**` @Promptless` ,` @PROMPTLESS` , and` @promptless` all work.
- **Commenting on a merge request Promptless opened revises that suggestion.** It doesn’t create a second one.
- **Commenting on any other MR starts a fresh documentation review.**
- **The project doesn’t have to be in your trigger configuration.** Promptless just needs your GitLab connection to have read access to the project. This matters for repos that are outside your standard trigger scope but occasionally produce changes worth documenting.
- **Use` @promptless-for-oss` for OSS projects.**
- **` /promptless` at the start of a comment line is a less-reliable alternative** if you’d rather not include the` @` mention in the comment text.


One difference from GitHub: a mention in an MR title or description doesn’t trigger Promptless. Only comments do.


*We regularly share actionable insights grounded in research, experiments, and real-world product learnings. Subscribe to get future posts in your inbox.*


## Who benefits most


Section titled “Who benefits most”


Teams using GitLab for code review who want to request documentation on specific MRs, not every MR that matches a trigger rule. In practice:


- Reviewers who spot an undocumented behavior mid-review and want to flag it without leaving GitLab
- Tech writers embedded in engineering teams who follow MR reviews and want to kick off documentation work at the right moment
- Teams with repos that fall outside their standard trigger scope but occasionally ship changes worth documenting


If your trigger rules already cover the projects and events you care about, you won’t use this for every MR. But when a specific MR needs a suggestion and your rules won’t catch it, a comment is the fastest path.


## How to use it


Section titled “How to use it”


No configuration needed. Add a comment to any GitLab merge request that includes` @promptless` . Your existing GitLab connection handles authentication — Promptless uses your connected GitLab group access token automatically.


When Promptless picks up the mention, it adds a 👀 reaction to your comment to acknowledge it and starts working.


## More from the blog


- [Fix skill slop before it makes your AI workforce worse](https://promptless.ai/blog/product-updates/introducing-promptless-for-agent-instructions) Product Updates


- [Grant Promptless Access to Files in Private and Shared Teams Channels](https://promptless.ai/blog/product-updates/teams-private-channel-file-access) Product Updates


- [Promptless Now Alerts You When an Integration Has a Problem](https://promptless.ai/blog/product-updates/integration-health-alerts) Product Updates


[← Back to Blog](https://promptless.ai/blog)
