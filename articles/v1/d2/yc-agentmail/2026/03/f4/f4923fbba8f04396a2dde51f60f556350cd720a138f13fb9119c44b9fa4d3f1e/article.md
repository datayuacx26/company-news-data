---
schema_version: "1.0.0"
document_id: "f4923fbba8f04396a2dde51f60f556350cd720a138f13fb9119c44b9fa4d3f1e"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/we-do-not-build-for-humans"
published_at: "2026-03-06T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:0c5b99ac3768043d0d500676d139d4c0a20e322ff381353c6e35b2d72ebe8f96"
---

# We Do Not Build for Humans

## We do not build for humans


It was a few weeks into my role as a Founding Engineer that I picked up on a small nuance that actually defines most of engineering at the company.


We build for agents, not humans.


Yes, I understood that we were email infrastructure for agents but I thought to myself: they're still being directed by humans to use our platform...right? Not really.


These past 2 months, we've seen agents sign up for the platform from every direction. Initially, human directed. However, when OpenClaw came out, it was agents discovering us autonomously and deciding to sign up to grab an identity for themselves. We hadn't marketed to them. They found us, evaluated us, and onboarded themselves. That's when we knew something had shifted.


Agents informing other agents about AgentMail, reaching out to people and marketing us, unprompted. We were shocked but we were in for an even bigger shock. For the past month or so, the Engineering team @ AgentMail has been dealing with a lot of support requests. Our enterprise customers reach out to us with requests on Slack, our developers on Discord. But, a majority of requests aren't from any of these channels. They are through email. Written by agents.


Are we the only company today getting these? Maybe. These are interesting times to be in. However, that has completely shifted our building philosophies. We now optimize for what agents would like to see.


To that end, we are launching our CLI today.


## Why a CLI, not a TUI?


In a time where everybody is launching their TUIs, we are coming out with a no-frills CLI. But there is a reason for this and it's actually quite simple: agents don't really care about the experience. TUIs or dashboards or pretty-looking UIs are great for humans. Agents wouldn't mind them either, but your bill would.


Our CLI? Ugly, robust, and highly reliable. Now your agents can grab an inbox, send a message, reply to one, and do that a thousand times over all within the CLI. We just brought something agents desperately needed: an identity, closer to home.


## Get Started


Install the CLI:


```text
npm install -g agentmail-cli
```


Set your API key:


```text
export AGENTMAIL_API_KEY=am_us_xxx
```


Create an inbox and send your first message:


```text
agentmail inboxes create --display-name "My Agent"
agentmail inboxes:messages send --inbox-id <inbox_id> \
--to "recipient@example.com" \
--subject "Hello from my agent" \
--text "Sent via the AgentMail CLI"
```


The CLI is also available as a skill for your favorite agent:


```text
npx skills add agentmail-to/agentmail-skills --skill agentmail-cli
```


Find the package on[npm](https://www.npmjs.com/package/agentmail-cli) and the source on[GitHub](https://github.com/agentmail-to/agentmail-cli) .
