---
schema_version: "1.0.0"
document_id: "049dacd98a489623170a8bea8295e52ed8abfab06f928a877bfc647e82a2e0b2"
company_key: "yc-cubic"
company: "cubic"
source_id: "yc-cubic-news-import-612274238019"
canonical_url: "https://www.cubic.dev/blog/coding-agents-out-of-beta"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-23T06:53:38.364034+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:f2272f067748f1b3e52f4a407a505e061facd7535c97fa4779937dc25bee105b"
---

# cubic blog: Coding agents | Launch week 03, Day 3

We’ve been building a faster way for teams to act on code review feedback.


Starting today, cubic lets you fix review comments directly from GitHub using background coding agents. Instead of switching tools or breaking your flow, you can resolve issues as soon as they appear, whether you’re reviewing a PR or already back to building.


You open a PR, and a few minutes later cubic flags issues. If you trust the feedback, you should be able to fix it immediately.


### Fix issues directly from your browser


Under every cubic issue in GitHub, you’ll now see a “⁠Fix with cubic” button.


Click it, and cubic spins up a background coding agent to resolve the issue for you.


### Fix everything at once


You do not have to go issue by issue.


cubic also posts a summary of all issues in the PR, with a link to fix everything in one go.


### Works on all comments - not just bots


This is not limited to comments from cubic.


If a teammate leaves feedback on a pull request, you can ask cubic to handle that too. Just comment something like:


⁠` @cubic-dev-ai please fix this`


That will trigger a coding agent to work on the change.


### No broken builds


Before opening a fix, the background agent runs tests, linting, and type checks to make sure the change passes CI.


Learn more[in the docs](https://docs.cubic.dev/ai-review/claudecode-fix#using-the-fix-button-in-cubic) .
