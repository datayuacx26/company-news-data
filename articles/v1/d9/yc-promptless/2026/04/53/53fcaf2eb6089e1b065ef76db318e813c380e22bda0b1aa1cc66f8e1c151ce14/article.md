---
schema_version: "1.0.0"
document_id: "53fcaf2eb6089e1b065ef76db318e813c380e22bda0b1aa1cc66f8e1c151ce14"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/request-docs-via-pr-comments/"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-22T10:20:48.668154+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:d3ebee55513e09072237e062931aeaded7224bb1e8af8f661ffdb4466e1c731d"
---

# Comment @promptless on a PR to Request Documentation

# Comment @promptless on a PR to Request Documentation


[← Back to Blog](https://promptless.ai/blog)


You can now comment` @promptless` on a pull request to request documentation updates. It works in any repo you’ve connected to Promptless, whether the PR is open, draft, merged, or closed.


This is an addition, not a replacement. Your existing trigger rules keep running.` @promptless` comments give you an on-demand option for PRs your rules don’t cover, or PRs where you want to give Promptless specific instructions.


## The problem


Section titled “The problem”


Promptless triggers are rules. You configure when to run, and it runs on every PR that matches. Rules can fire on PR open, on first approval, on merge, or when a glob matches.


Rules don’t cover every case. Three common gaps:


- A PR merged months ago introduced behavior users keep asking about, but it predates your trigger setup.
- A draft PR has a feature worth documenting early, before it hits your usual trigger.
- You want to focus Promptless on one slice of a broad PR, not the whole thing.


Previously, the workaround was to edit configuration, re-run builds, or skip the PR.


## How it works


Section titled “How it works”


Leave a comment that includes` @promptless` on any pull request in a connected repo. Promptless picks up the comment, fetches the current state of the PR, and starts a doc run based on what’s in the PR right now.


A few things to know:


- Any PR state works, whether the PR is open, draft, merged, or closed. Promptless fetches live details at the time of the comment, so PRs that shipped weeks or months ago are handled correctly.
- The repo has to be connected to a Promptless project. Installing the Promptless GitHub App on an additional repo isn’t enough on its own. If you comment` @promptless` on a PR in a repo that isn’t configured, nothing happens.
- Only comments that include` @promptless` trigger a run, so conversational PR comments won’t accidentally start one.


You can add instructions in the comment. If you write` @promptless please focus on the new retries config` , that context is passed to the doc run. Useful when the PR is broad but only one slice matters for your docs.


## Who benefits most


Section titled “Who benefits most”


**Teams filling in docs gaps retroactively.** If you set up Promptless recently, most of your historical PRs were never processed. Pick the significant ones and document them one at a time.


**Teams that want explicit control over specific PRs.** Some PRs want the standard flow. Others are irregular: a big refactor with one user-visible slice, a config PR that changes defaults, a 40-file rewrite that needs three lines of docs.` @promptless` comments let you guide Promptless on the ones that need judgment.


**Launch-week triage.** Point at the exact PRs that shipped a release and queue docs for each in a comment, instead of waiting for automatic triggers.


*We regularly share actionable insights grounded in research, experiments, and real-world product learnings. Subscribe to get future posts in your inbox.*


## How to use it


Section titled “How to use it”


No new configuration for repos you’ve already connected. If you’re on GitHub, it’s live.


Leave a comment that includes` @promptless` on any pull request in a connected repo. GitHub won’t autocomplete` @promptless` because it isn’t a GitHub user. The keyword still works. Add instructions if you want to. Promptless picks it up, runs the doc flow, and comments back with a draft.


GitHub Enterprise isn’t supported yet. It’s on the list.


## What’s next


Section titled “What’s next”


We’re building thread-aware follow-ons so you can iterate on a draft inside the PR. We’re also working toward parity for GitHub Enterprise.


## More from the blog


- [Slack Notifications for Suggestion Outcomes](https://promptless.ai/blog/product-updates/suggestion-lifecycle-notifications) Product Updates


- [Connect Multiple GitHub Organizations to One Promptless Account](https://promptless.ai/blog/product-updates/multi-org-github-connect) Product Updates


- [Edit Doc Collection Settings Without Contacting Support](https://promptless.ai/blog/product-updates/self-serve-doc-collection-editing) Product Updates


[← Back to Blog](https://promptless.ai/blog)
