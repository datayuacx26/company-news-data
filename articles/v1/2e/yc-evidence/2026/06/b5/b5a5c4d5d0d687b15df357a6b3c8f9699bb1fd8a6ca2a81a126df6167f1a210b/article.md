---
schema_version: "1.0.0"
document_id: "b5a5c4d5d0d687b15df357a6b3c8f9699bb1fd8a6ca2a81a126df6167f1a210b"
company_key: "yc-evidence"
company: "Evidence"
source_id: "yc-evidence-news-import-47bf0dc75044"
canonical_url: "https://evidence.dev/blog/deploy-previews"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-21T19:04:43.661863+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:182a7a30bdc9ef56185ca42bae5ab5805fa0e5e3827e0cd5aa8640856422216e"
---

# Deploy Previews

# Deploy Previews


[Adam McAskill June 23, 2026 · 2 min read](https://www.linkedin.com/in/adam-mcaskill-74515720/)


A comment on a pull request with a link and a timestamp.


Deploy previews are a simple feature but they reveal a lot about our world view. So I wanted to spend a few words on these tiny links.


## What is a deploy preview?


When you open a pull request on an Evidence project, Evidence Studio replies in Github with a comment like this:


Following that preview link shows you what your users *would* see if you merged the pull request.


These previews sit on their own dedicated url. Your users aren’t seeing them yet but you can get feedback and iterate before you publish. When you commit to the branch, the preview updates.


## How are they used?


Teams use previews to get feedback on things they’ve been working on *before* they publish them. Modifying a popular internal report or tweaking the footnote in a customer-facing analysis. Whatever the situation, these preview links are helpful when you need another set of eyes.


But, increasingly, these links are the first time any human will lay eyes on the proposed changes.


Coding agents are taking over more and more of the work on Evidence projects. When they’re ready to get feedback on their work, they do it in the form of a PR, and a preview link gets appended.


## They just… work


We use Linear as our issue tracker at Evidence. Linear surfaces Evidence deploy previews natively in their UI.


That’s not because we built a special integration with Linear. It’s because of how profoundly *normal* this pattern is in software development. A bot leaves a comment on a PR with a link that says “preview”. Linear already knows what that means.


And that’s where these tiny links get to our worldview.


The Evidence workflow isn’t only legible to Linear. It’s legible to every tool designed to speed up software development. Claude Code, Cursor, Greptile, and whatever comes next.


Markdown, in a repo, under version control. With a mix of people and agents delivering units of work as pull requests. Legible.


There’s an explosion of innovation occurring in coding agents. But from the perspective of these agents most BI tools aren’t legible at all… they’re mostly invisible.
