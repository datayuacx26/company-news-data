---
schema_version: "1.0.0"
document_id: "86b649131c20aaf60b499767c15f99d6f5e25e1fe6a00e2eeebcc669ecd9d9f3"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/agent-workspace"
published_at: null
first_seen_at: "2026-08-03T17:52:07.036009+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:dd35f8feb3cec151d8b54be94d287433630e5ac0b7e5df21442d56500f45ca4b"
---

# Introducing Deepnote Agent Workspace

[← Back to all posts](https://deepnote.com/blog)


Most of the data work ahead will be done by agents. But where will that work happen?


Everyone agrees agents can't do data work without understanding your business: your metrics, your schemas, and your org’s quirks — your unique context. Weirdly, we can’t seem to agree where this context should live. Warehouse companies say it should be the warehouse. The AI labs say it should be in the chat history. Context startups say it should be in their products. In its latest piece on the topic,[a16z says Deepnote is one of the likely candidates for this](https://a16z.com/your-data-agents-need-context/) .


**Today, we’re introducing the Deepnote Agent Workspace, a shared environment where data teams turn trusted analyses into reusable skills, agents, and apps.**


Before we dive into the announcement, it’s useful to recap the current state of the art.


## **What the frontier labs built**


In the first half of 2026,[OpenAI](https://openai.com/index/inside-our-in-house-data-agent/) ,[Meta](https://medium.com/@AnalyticsAtMeta/inside-metas-home-grown-ai-analytics-agent-4ea6779acfb3) , and[Anthropic](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) each published a detailed write-up of the data agent they run internally. Different stacks, different vocabularies, but underneath, the same four architectural patterns:


- A set of verified sources, so the agent starts from a curated subset instead of the whole warehouse.


- Semantic knowledge written down: what the columns mean, what the business calls things, and the tribal context that never made it into a schema ([OpenAI counts six layers of this](https://openai.com/index/inside-our-in-house-data-agent/) ).


- Procedures that encode how a given task should be done, kept deliberately separate from the knowledge ([Meta calls this Ingredients and Recipes](https://medium.com/@AnalyticsAtMeta/inside-metas-home-grown-ai-analytics-agent-4ea6779acfb3) ).


- An interface where people review and steer agents, wrapped in existing access controls and a full record of what the agent did.


Every lab built that for itself, with a platform team most companies don't have.


## Companies are attempting to replicate this, too


Function-specific teams (e.g., GTM, product, etc.) are now building narrower versions for their own workflows. These systems can be fast to prototype, but they are often tied to personal credentials, limited to one department, and difficult to govern or extend across the company.


Miro's GTM team recently[described](https://www.linkedin.com/posts/cfischetti_we-launched-our-gtm-engine-in-may-today-share-7486147522747387904-F7Z6/?utm_source=social_share_send&utm_medium=android_app&rcm=ACoAABl1KnEB6C4PLrKg2GTi5WNskmo9Vpgui04&utm_campaign=share_via) the engine they run for sales and customer success.


Semantic views in Snowflake as the source of truth across Gong, Salesforce, Gainsight, and product usage. Capabilities, the atomic reads everything else calls. Knowledge references, which they describe as the context layer of their GTM org. Workflows, which are the skills their team actually opens. It runs out of Claude Cowork, and about 56% of the team uses it weekly. Sources, knowledge, procedures, an interface: the same four things, arrived at independently.


Similarly,[Apollo.io](http://apollo.io/) CEO[described building](https://www.apollo.io/magazine/a-ceos-guide-to-gtm) a sequence analytics interface he could deploy company-wide in about 30 minutes, for about $30 in tokens. Work that used to mean a RevOps analyst, a data engineer, an ETL layer, and a BI license.


So a company gets its context layer in one of two ways:


1. A functional team builds it for a specific use case. It works quickly, but it is narrow in scope, often tied to personal credentials, and usually only reflects one part of the business (like GTM). The next adjacent use case requires rebuilding the same system again, often in another 30 minutes and another $30 in tokens.


2. The data team builds it as a shared layer. In this case, it can become authoritative across the business—but only if they have a proper environment to build and maintain it, rather than assembling it from scratch as a one-off platform project each time.


That second option is what we're making possible today: **Deepnote Agent Workspace** , a shared place where people and agents work on the same data. It's the architecture everyone keeps rebuilding, as a product.


## **What is Deepnote Agent Workspace?**


In a nutshell, it comprises three building blocks, with 100+ integrations as the connective tissue.


1. **Skills** are knowledge an agent can use, with rules about who may use it. A module called active_workspaces settles what counts as active in the first place, whether a scheduled run counts when nobody has opened anything, which internal and trial accounts are excluded, and how orgs running four workspaces are deduped. Agents read it like markdown, but it also carries the permissions and the sources it's allowed to touch, so the same skill answers a question for a finance lead and refuses one it shouldn't.


2. **Agents** are a runtime plus a set of instructions, which is a thing that has had a name for a while: *a notebook* . Everything in it is context, including your skills, SQL, integrations, and tables. Run one while you shape the analysis, schedule it once the work recurs, or trigger it from the API. Sequoia calls this shape a[Goldilocks agent](https://sequoiacap.com/article/goldilocks-agents/) : adaptive within the boundaries of explicit state, tools, and permissions.


3. **Apps** are how the work reaches people who won't open a notebook. Build one in Codex or Cursor or the terminal, host it on Deepnote, and it inherits the workspace: integrations instead of your local keys, the team's permissions, a schedule if it needs one.


Integrations ground all three. 100+ data sources natively, and other connections like Salesforce or Linear through pre-built MCPs, plus any custom MCP you add. Connect a source once, and every skill, agent, and app inherits it along with the permissions you are entitled to.


## **Work with agents anywhere**


The workspace is available wherever the work begins: in Deepnote, from Codex, Claude Code, or any IDE via MCP, in Slack, from the terminal, or headlessly through API.


## **The foundation: executable markdown**


The foundation is the notebook - something Deepnote is great at. Turns out, this is the format agents needed all along: an executable markdown. Narrative, code, permissions, and environments live in one file.


In Deepnote, writing the work down is the work: every notebook automatically becomes context that agents read from and build on, so each question starts from everything the last one learned. You don't point a separate context layer at your stack. Your Deepnote workspace becomes one.


## Data teams are the natural owners of this layer


Building an agent stopped being an implementation problem. It's a[specification and evaluation problem](https://motherduck.com/blog/vibe-coding-dangerous-agentic-engineering-wes-mckinney/) , which is a different job with a different owner. Domain teams hold the operating knowledge: what counts as fraud, which accounts matter, when an exception is fine.[Data teams are the ones](https://hamel.dev/blog/posts/revenge/index.html) who can turn that into governed skills, agents, and apps, then own the harness around an AI application as data science work in its own right: reading traces, building evals, studying failures. **Nobody else in the company has this know-how.**


If your team has an analysis that ought to be an ongoing responsibility, something currently spread across a notebook, a script, a scheduler, a dashboard, and Slack, we'd like to build it with you. Book a call with our forward-deployed engineers to pilot Deepnote Agent Workspace as a design partner.


### Further reading


- OpenAI,[Inside our in-house data agent](https://openai.com/index/inside-our-in-house-data-agent/)


- Analytics at Meta,[Inside Meta's home-grown AI analytics agent](https://medium.com/@AnalyticsAtMeta/inside-metas-home-grown-ai-analytics-agent-4ea6779acfb3)


- Anthropic,[How Anthropic enables self-service data analytics with Claude](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude)


- Matt Curl,[A CEO's guide to GTM: what used to cost millions now costs $30](https://www.apollo.io/magazine/a-ceos-guide-to-gtm)


- a16z,[Your data agents need context](https://a16z.com/your-data-agents-need-context/)


- Google Cloud,[How the Open Knowledge Format can improve data sharing](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing)


- Sequoia,[Goldilocks agents](https://sequoiacap.com/article/goldilocks-agents/)


- Tomasz Tunguz,[Agent gravity](https://tomtunguz.com/agent-gravity)


- Hamel Husain,[The revenge of the data scientist](https://hamel.dev/blog/posts/revenge/index.html)


- Wes McKinney and Simon Späti,[Vibe coding is dangerous, agentic engineering isn't](https://motherduck.com/blog/vibe-coding-dangerous-agentic-engineering-wes-mckinney/)


- Foundation Capital,[Context graphs, one month in](https://foundationcapital.com/context-graphs-one-month-in/)


Jakub Jurovych


CEO @ Deepnote


Follow Jakub on[LinkedIn](https://www.linkedin.com/in/jakubjurovych/)
