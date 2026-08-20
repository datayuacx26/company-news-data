---
schema_version: "1.0.0"
document_id: "e0cfd8f0437927e6e963ffc6300b10f167c781a593395706a8beca9b33b5639d"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/agents-can-use-the-computer"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:19bc96e86b61c7b7264b9bf1874b116add2afc95092c04cacfd7482ed3bc8521"
---

# Agents Can Use the Computer

Agents can now use the computer. Turn on the **Computer Use** capability and an Agent can drive a real browser the way you would: open pages, log in, navigate, fill out forms, click through multi-step flows, download and upload files, and bring data back into the conversation. It works from a normal chat or from a scheduled run, so the same Agent that answers your questions can also go do the task.


For sites that require a login, attach **Browser Authentication** : saved login sessions you manage in your account settings. The Agent reuses them by reference only, so it can act on protected sites without your credentials ever being exposed to the model. Computer Use used to be a separate, single-purpose agent type. Now it's a capability any of your Agents can have, right alongside everything else they do.


## What's New


- **Computer Use is a capability, not a separate agent.** In an Agent's **Capabilities** , under **External Services** , turn on **Computer Use** and it gains a real browser it can operate, next to its other tools and channels. (A separate read-only **Read web pages** capability is also there for when an Agent only needs to fetch and analyze page content.)
- **Browser Authentication for sites behind a login.** Attach saved login sessions (managed in your account settings) so the Agent can work on authenticated sites. Sessions are referenced by id only, and auth state is never exposed to the client or the model.
- **Works in chat and on a schedule.** Ask an Agent to do something in conversation, or have a scheduled run drive the browser hands-off.
- **Visible in run history.** Browser runs show up in the Agent's runs with status and detail, so you can monitor progress and review exactly what happened.


## Why This Matters


Plenty of real work lives behind a browser: a vendor portal with no API, a dashboard you log into, a form you fill out every week. Until now an Agent could reason about those tasks but not actually do them. Computer Use closes that gap, and because it's just a capability, there's no separate kind of agent to manage to get it.


## How It Works


1. Open or create an Agent and go to its **Capabilities** . Under **External Services** , turn on **Computer Use** .
2. Once it's enabled, a **Browser Authentication** picker appears just below it. Optionally attach saved login sessions (managed in your account settings) for sites that need a login.
3. Ask the Agent to do something in chat, or schedule it. Watch progress and results in the Agent's run history.


Notes:


- Computer Use is available on paid plans, and runs on models that support it.
- Saved authentications are managed per user in account settings and referenced by the Agent. Credential and auth state are never sent to the browser client or the model.
