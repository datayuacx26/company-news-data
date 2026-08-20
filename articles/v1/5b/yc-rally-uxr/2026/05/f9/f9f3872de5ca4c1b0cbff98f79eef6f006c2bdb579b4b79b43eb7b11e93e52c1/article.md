---
schema_version: "1.0.0"
document_id: "f9f3872de5ca4c1b0cbff98f79eef6f006c2bdb579b4b79b43eb7b11e93e52c1"
company_key: "yc-rally-uxr"
company: "Rally UXR"
source_id: "yc-rally-uxr-news-import-94fad56766fc"
canonical_url: "https://www.rallyuxr.com/post/introducing-rally-mcp"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-25T20:31:53.624362+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:a215e8ff80e5b16bcbee92dacd63ab77337432ea540f72030b5ea28330924f04"
---

# Rally's MCP and the start of our agentic journey

Blog Posts


May 11, 2026


## Rally's MCP and the start of our agentic journey


Contributors:


Oren Friedman


CEO & Co-founder


CEO & Co-founder at Rally


Alec Robins


CTO & Co-founder


CTO & Co-founder at Rally


Today we're shipping the first version of Rally's MCP.


Your team can generate insights, search your audiences, and kick off new studies (coming soon) from Claude, Cursor, ChatGPT, or whichever AI tool they're already using.


This is the first step of a bigger vision we're building toward, and we’re excited to share a bit about how we're approaching it.


### **Why we're building this**


Over the past few months, we've interviewed dozens of our customers to learn how their product teams are adopting AI and using MCPs to run research.


Across the spectrum of AI maturity, one pattern came up consistently: teams want to look up past research, discover the right people to talk to, and kick off new studies, all from the AI tools they're already using.


This is part of a bigger shift. Product teams are being re-architected in real time. Roles between researchers, PMs, designers, and engineers are blending, and every company is figuring out how user research fits into AI-native product development.


In nearly every conversation, Rally's MCP was the missing piece. Teams were either building custom API bridges to get Rally data into agentic systems or doing it the hard way by jumping into Rally's UI every time they needed something.


The first step we're taking is closing that gap with our launch of the Rally MCP today.


### **How we're approaching it**


A lot of MCPs shipping right now are thin wrappers around an API: exposing every endpoint as a tool and letting the model do the heavy lifting. That works for simple, targeted lookups.


It breaks down the moment you want the agent to actually do something useful and complex, like kicking off a new study. MCPs with dozens of tools create disjointed and non-deterministic experiences, with every new conversation feeling like you're starting from scratch.


We're building Rally's MCP to be **agentic-first** .


That means in practice:


- **It carries context across conversations.** Wherever you're talking to the Rally Agent, whether that's in Claude, Slack, or Rally's UI, it knows what you asked yesterday, what study you're running, and what you've already explored. One consistent Rally experience across every surface, not a different one per client.
- **A more reliable agentic experience.** Because we're not handing the model a pile of tools and hoping it figures out the right sequence, we can control the quality of every response that comes back through the Rally MCP. Our team evaluates the agent continuously, which means it only gets sharper over time.


Lastly, for teams building their own agents on top of Rally who need full control, we're committed to exposing every action and request in the UI as an API. From what we've seen, leveraging Rally's API directly is more effective than stitching together individual MCP tool calls.


### **What's releasing today**


The first version of the Rally MCP includes:


- **Audience search.** Use natural language to narrow down the right people in your panel by behavior, properties, study history, or whatever signals matter to you.
- **Past research search.** Pull transcripts, surface screener responses, and find what your team has already learned without opening Rally.
- **Study building.** Kick off new studies from the chat surface you already use. Draft a study from a prompt, refine the screener, and configure the scheduler before anything goes live. Reach out to us to get early access to study building via Rally MCP.
- **Study management.** Track consent submissions, incentive statuses, and the other tasks that come up when you're managing an active recruit.
- **Permissions that mirror Rally.** Whatever you can access in the Rally UI is exactly what the MCP exposes. No need to worry about PII leaking or unauthorized access to a panel.


We've been building our MCP alongside AI-native product orgs like Ramp, Figma, and Perplexity, and have found so much value using it to run research at Rally. We can't wait for you to try it as well.


Enabling high-quality research, regardless of who's conducting it, has been the defining ambition of every research and research operations leader for years. It's finally becoming possible. The PMs, designers, and engineers who want closer access to your customers now have a way in, governed by the best practices, guardrails, and compliance your team designs.


### **What's next**


This MCP is step one toward what Rally has been building toward for the last year: **ending manual recruitment.**


The next release brings recruitment automation into the same surface: audience outreach, screening, scheduling, follow-ups, and incentives handled for you, with humans in the loop on anything that touches a customer.


If you're a Rally customer,[try the MCP for free today here.](https://help.rallyuxr.com/articles/5139669410-rally-mcp?lang=en)


Oren Friedman


Alec Robins


Supercharge your Research Ops with Rally


Rally’s Research Ops Platform enables you to do better research in less time. Find out how you can use Rally to empower your teams to talk to their users, without disjointed tooling and spreadsheets. Explore Rally now by[setting up a demo](https://www.rallyuxr.com/demo) .


[Back to all blogs](https://www.rallyuxr.com/blog)
