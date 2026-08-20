---
schema_version: "1.0.0"
document_id: "afc3a717e4c2ebd36e9e96f512b0292274b46b72d036704fa281f56949684a50"
company_key: "yc-notte"
company: "Notte"
source_id: "yc-notte-news-import-7238aba58520"
canonical_url: "https://www.notte.cc/blog/diana-browser-automation"
published_at: "2026-04-30T17:00:00+00:00"
first_seen_at: "2026-07-22T06:23:42.732156+00:00"
fetched_at: "2026-07-28T22:15:30.724879+00:00"
content_hash: "sha256:f993b85127ad400187ab33079d83ce884741b9af2b6efa2126fc19b3a7522df3"
---

# How Get Diana Uses Notte to Automate Tools That Don't Have APIs

Thousands of tools don't have a public API, and for platforms that need to act inside them on a user's behalf, that's a dead end. Unless you go through the browser.


Diana is a Slack-based AI assistant platform backed by Y Combinator and General Catalyst. Their product connects to thousands of tools. For tools without a public API, Diana logs in through a browser on behalf of the user.


For their browser infrastructure, they've partnered with Notte.


### **How it works**


A user asks Diana to perform an action in a tool that has no API. Diana spins up a Notte browser session, authenticates through the browser, and completes the task. The user can watch the session live through a secure, shareable viewer link.


> "Most of the tools our users need to automate weren't built with APIs in mind. The browser is the only universal interface that exists for all of them, so that's where we had to operate."
>
>
> — Upeka Bee, CEO,[Diana](https://w.getdiana.com/)


### **Secure Session Sharing**


Diana needed their end users to watch live sessions without exposing credentials. Notte's session viewer now uses JWT-scoped tokens: short-lived, scoped to a single session, with a 4-hour expiry. This means no API keys in the URL, and no cross-session access.


> "The live session viewer isn't just a technical feature but a transparency guarantee. The user stays in the loop and stays in control. This is a significant matter when you're asking someone to let an AI act on their behalf."
>
>
> — Upeka Bee, CEO,[Diana](https://www.getdiana.com/)


### **Result**


Diana launched browser logins today as part of their platform, giving their users access to tools that have never been automatable before.


Details on their launch at[getdiana.com](https://getdiana.com/) .
