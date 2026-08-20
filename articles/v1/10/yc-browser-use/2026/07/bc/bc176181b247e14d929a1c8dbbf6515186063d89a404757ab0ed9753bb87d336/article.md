---
schema_version: "1.0.0"
document_id: "bc176181b247e14d929a1c8dbbf6515186063d89a404757ab0ed9753bb87d336"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-545dadaa110d"
canonical_url: "https://browser-use.com/changelog/1-7-2026"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:30.502118+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:a6e807dbe32e80c86d65403e5a6b9bcdc3c8d8e73642b211cd9ce1fd2d8b39c4"
---

# Browser Use CLI 3.0

## Browser Use CLI 3.0


Browser Use CLI 3.0 lets agents work online with our highest accuracy yet. It is powered by[Browser Harness](https://github.com/browser-use/browser-harness) , providing agents with a small, editable Python harness instead of a fixed menu of browser actions.


This release applies what we learned in[The Bitter Lesson of Agent Harnesses](https://browser-use.com/posts/bitter-lesson-agent-harnesses) and[The Bitter Lesson of Agent Frameworks](https://browser-use.com/posts/bitter-lesson-agent-frameworks) : the latest models do best when you give them freedom, rather than abstracting away complexity.


## Install the CLI


```text
uv   tool   install   browser-use
browser-use
```


## Set Up Your Agent


Paste this prompt into Claude Code, Codex, or another coding agent:


```text
Install or upgrade browser-use to the latest stable version with uv using Python 3.12, register the skill from `browser-use skill`, and connect it to my browser. Follow https://github.com/browser-use/browser-use if setup or connection fails.
```


Then, ask your agent to use Browser Use for work that needs the live web or a browser.


See the[CLI docs](https://docs.browser-use.com/open-source/browser-use-cli) for more information.


## What Changed


The old CLI provided agents with a set list of actions to execute in the browser (ex: click, type, etc.). CLI 3.0 allows agents to execute Python freely within the browser, so agents can do more actions and can inspect, adapt, and recover using the same coding loop they already use and were trained to be good at.
