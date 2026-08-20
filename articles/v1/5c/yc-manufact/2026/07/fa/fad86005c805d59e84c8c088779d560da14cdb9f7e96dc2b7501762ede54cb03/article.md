---
schema_version: "1.0.0"
document_id: "fad86005c805d59e84c8c088779d560da14cdb9f7e96dc2b7501762ede54cb03"
company_key: "yc-manufact"
company: "Manufact (formerly mcp-use)"
source_id: "yc-manufact-news-import-914a697bc101"
canonical_url: "https://manufact.com/blog/agentmail-mcp-server-case-study"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-24T03:21:12.468725+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:9559bf7b554ddeba864969c4f3f549a8f418ae221c12871374bb998a94a74f50"
---

# How AgentMail handles 100k+ agent tool calls with Manufact

[AgentMail](https://agentmail.to/) builds email infrastructure for AI agents. Where a normal email API assumes a human is reading the inbox, AgentMail assumes the reader is software: an agent that receives mail, decides what matters, and replies on its own. Inboxes as a primitive for agents, kind of like the way Twilio made phone numbers a primitive for apps.


There was just one catch: users trigger the agents from Claude, ChatGPT, and Cursor directly. For AgentMail, an MCP server was never a checkbox integration; it was fundamental. That left an infrastructure company staring at an infrastructure decision: build the MCP hosting stack themselves, or ship on someone else's.


It is the same decision every API company hits the moment agents become their users. AgentMail just hit it earlier than most, and at higher stakes, because for them agent traffic was pivotal.


> "We tell our customers every day not to build email infra in-house, so we weren't about to make the same mistake with MCP. The weeks we didn't spend on transport, auth, and session handling went straight into shipping our enterprise integrations instead."
>
>
> *[Adi Singh](https://www.linkedin.com/in/adivirsingh13) , cofounder*


## Going Live and Scaling in 3 Weeks


AgentMail's timeline on Manufact is short because there was nothing slow about it. A week after becoming a paying customer, their MCP server was in production. No transport layer to build, no TLS or domains to manage, no auth to wire up. Session handling, scaling, and the monitoring stack were all handled by Manufact!


Then, production numbers scaled rapidly:


- **Day 1:** live in production
- **Week 3:** production traffic crosses six figures of tool calls a week


Was there an alternative to all this? Yes, and it wasn't pretty:


> "Building it ourselves meant standing up hosting, auth, sessions, scaling, and monitoring. That's probably a quarter of eng time for a ten-person team, minimum. None of that makes our core product better."
>
>
> *Adi Singh*


## An agent that reads, and speaks when it matters


The workload is a production email agent that triages at scale. It reads threads continuously, classifies them, and sends when it decides a reply is warranted.


This is the pipeline behind AgentMail's own inbound: every email hitting the company's inboxes gets read, classified, and routed by the agent, with replies drafted and sent when the agent judges a response is warranted. It's AgentMail running its own product on its most important traffic!


## Seeing what agents actually do


Every number and insight here comes from[Manufact's built-in analytics](https://manufact.com/platform/analytics) . AgentMail's team opens a dashboard and watches which tools agents call, how long each operation takes, and where failures cluster, per tool, per client, over time.


A raw API integration gives you server logs. It cannot tell you how agents behave on the other side of the wire: which capabilities they reach for, where they retry, what they never touch. Seeing how agents actually used the server, which tools they reached for and how often, shaped what the team prioritized next.


## What's next?


This month,[AgentMail shipped a native integration inside Clay](https://www.linkedin.com/posts/agentmailto_agentmail-is-now-natively-integrated-within-activity-7480317597129875456-P8Qp) , putting agent-managed inboxes into one of the largest GTM automation platforms. GTM teams now have an agent that does not just enrich records but can fully read and write email. For AgentMail, every new platform where agents can reach them means more traffic and distribution, while the infrastructure question stays answered.


> "Agents are becoming the primary users of every API, and email is where they show up first. Our job is to ensure every agent on the internet can have an inbox. Manufact makes sure they can all reach us."
>
>
> *Adi Singh*


Congrats and thanks to the AgentMail team for building with us!


Thinking of shipping your own MCP server? Deploy your first server in under 60 seconds at[manufact.com](https://manufact.com/) , or[book a demo](https://manufact.com/book-call) and we'll help you map the fastest path to production agent traffic!
