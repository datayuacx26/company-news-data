---
schema_version: "1.0.0"
document_id: "a519d452d1fbf8284bb4f65ab7849e6531b03eee15ca15221e2ec012731c9c6c"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/receive-webhooks-in-openclaw-and-hermes/"
published_at: "2026-07-28T12:00:00+00:00"
first_seen_at: "2026-07-30T02:08:22.185307+00:00"
fetched_at: "2026-07-30T02:08:23.755635+00:00"
content_hash: "sha256:0cfcc0bf283358e59fb76e09db03b6d93ccb34823ce08155e52b2e1973ddd830"
---

# Receive webhooks in OpenClaw and Hermes

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes for both[sending](https://www.svix.com/#sending) and[receiving them](https://www.svix.com/ingest/) . Want webhooks in your product without building the hard parts yourself?[Give it a try!](https://www.svix.com/)


Trying to get your agents to consume webhooks is a huge pain. You have to run an HTTP endpoint, make sure to have a dynamic DNS, open ports in the router, and watch out for webhooks failing every time you close your laptop or have internet issues.


I knew there must be a better way so I built a plugin that solves all of that using Svix and makes receiving webhooks a breeze!


## Receiving webhooks in your agent sucks


Traditionally when someone wants to inform you of an event, they make an HTTP request to a URL you gave it. That works great when 'you' are a server with a stable public address.


AI agents usually aren't on a stable address because often they run on a laptop, a dev environment behind a VPN, inside a locked corporate network, or in some kind of sandbox that can't accept inbound traffic. To take webhooks the normal way, that agent has to expose either a public URL, an open port, or a tunnel. That's annoying to set up and in a lot of environments it's a no-go for security reasons.


## How Svix enables agents to consume webhooks


The trick is to flip the direction of the connection: instead of the webhook provider pushing events to your agent, your agent pulls them from Svix.


[Svix Ingest](https://www.svix.com/ingest/) is the first step: you paste a Svix Ingest URL into whatever platform you want events from, and Svix takes it from there. It verifies the signature and buffers every message until your agent comes to collect them, without any additional infrastructure on your side.


The second step is creating an[AutoConfig](https://www.svix.com/blog/webhooks-autoconfig) polling token and adding it to your agent plugin config. AutoConfig is a single token that lets the plugin provision its own polling endpoint and start pulling messages, no dashboard setup needed. The plugin polls over a plain outbound HTTPS connection (every 5 seconds by default) and commits its position as it goes, so nothing on your side ever needs to be reachable from the internet.


This is also what solves the closed-laptop problem from earlier: if your agent is offline, messages simply queue up in Svix, and the next poll picks up exactly where the last one left off. Nothing gets dropped.


## OpenClaw


The[OpenClaw plugin](https://github.com/svix/ai/tree/main/plugins/svix-openclaw) turns incoming webhooks into actions on your agent: a webhook arrives at Svix, and on the next poll your agent creates a flow, runs a task, or wakes up. Under the hood it maps polled messages onto OpenClaw's existing webhook surface, either as TaskFlow actions (` create_flow` ,` run_task` , …) against a bound session, or as gateway hooks` POST` ed to OpenClaw's documented[/hooks/wake](https://docs.openclaw.ai/automation/cron-jobs#webhooks) /` /hooks/agent` endpoints. Three steps:


1. Clone the repo and link the plugin into your OpenClaw install:


```text
git   clone https://github.com/svix/ai.git
openclaw plugins  install    --link   ./ai/plugins/svix-openclaw


```


1. Add a route under` plugins.entries.svix-openclaw.config.routes` . Each route gets a single AutoConfig token (no URL to set) and the session its actions apply to:


```text
{
"routes": {
"ops": {
"token": { "source": "env", "provider": "env", "id": "SVIX_TASKFLOW_AUTOCONFIG_TOKEN" },
"sessionKey": "agent:main",
},
},
}


```


1. Run the gateway (` openclaw gateway` ). Anything sent to that route's Ingest URL gets applied as an action on the next poll, e.g.:


```text
{ "action": "create_flow", "goal": "Investigate alert" }


```


Point your alerting system at the route's Ingest URL and that payload is all it takes: the alert fires, and a few seconds later your agent is already investigating it.


## Hermes


The[Hermes plugin](https://github.com/svix/ai/tree/main/plugins/svix-hermes) turns incoming webhooks into prompts for your agent, and delivers the agent's response wherever you want it. It follows the same shape as Hermes' built-in` webhook` adapter, so each event flows through the familiar route → prompt → delivery pipeline. Three steps:


1. Clone the repo and install the plugin and the Svix SDK:


```text
git   clone https://github.com/svix/ai.git
cp    -r   ai/plugins/svix-hermes ~/.hermes/plugins/svix-hermes
pip  install    'svix>=1.96.0'


```


1. Add a route under` platforms.svix.extra.routes` in your gateway` config.yaml` . Give it an AutoConfig token, a prompt template, and a delivery target:


```text
platforms  :
svix  :
enabled  :    true
extra  :
routes  :
github_events  :
token_env  :   SVIX_GITHUB_AUTOCONFIG_TOKEN
prompt  :    "GitHub issue opened: {issue.title}\n\n{__raw__}"
deliver  :   telegram
deliver_extra  :
chat_id  :    '123456789'    # your Telegram chat id


```


1. Start the gateway. Each route provisions its sink on the first poll, then drains it from there. No other setup needed.


With the route above, someone opens an issue on your repo and a few seconds later your agent messages you on Telegram about it. No server, no tunnel, no open ports.


## Try it out


Both plugins live in[svix/ai](https://github.com/svix/ai) alongside our[agent skills](https://github.com/svix/ai/tree/main/skills) . Each plugin's README has the full setup guide.


Got feedback, or a framework you'd like to see next? Open an issue or PR over on[svix/ai](https://github.com/svix/ai)


---


For more content like this, make sure to follow us on[Twitter](https://twitter.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for[Svix](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
