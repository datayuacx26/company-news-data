---
schema_version: "1.0.0"
document_id: "f2ed2b0c007cb19715db7d18937a616158d879daacfcccd58da01968ac0ecefc"
company_key: "yc-cargo"
company: "Cargo"
source_id: "yc-cargo-news-import-f4a8864e899b"
canonical_url: "https://www.getcargo.ai/blog/gtm-infrastructure-for-ai-agents"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-28T06:48:16.360513+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:11c98b248e40739630e2d2d78b1ebc86cd86aaaea73f69d299feb5618cba7766"
---

# GTM software was built for humans. Agents are the new users.

Every GTM tool of the last decade was built for the same user: a person, in a browser, clicking a UI.


The enrichment platforms, the sales engagement tools, the CRMs, the intent dashboards. Different logos, same assumption. A human logs in, looks at a table, pushes a button. The tools competed on how pleasant that experience was.


That assumption just broke. The user doing go-to-market work is increasingly not a person. It is an agent.


And an agent doesn’t need a prettier table. It needs infrastructure.


## Agents don’t click#


Watch an agent try to do real GTM work through tools built for humans and you see the problem immediately. It’s puppeteering a UI designed to slow a human down in useful ways: wizards, confirmation modals, settings pages. Every screen is friction. Every credential is a copy-paste. Every workflow lives in a proprietary canvas the agent can’t read, version, or reason about.


Software ate every department by giving builders primitives. Compute, storage, queues, deploys. GTM never got its primitives. It got point tools, and the glue between them became a full-time job with a spreadsheet.


This is a now problem, not a someday one. Two things changed in the last eighteen months: agents crossed the reliability threshold where you can hand them multi-step work unsupervised, and MCP standardized how they connect to everything else. The client side is solved. Any agent can reach out. What’s missing is the server side: something on the GTM end actually worth connecting to.


So when we say Cargo is GTM infrastructure for AI agents, we mean something specific. Not “a GTM tool with AI in it.” The layer underneath: the primitives an agent, or a person building with agents, actually runs on.


## The primitives#


There are five that matter.


**Data models** : tables of accounts, contacts, and signals, sourced from your CRM, your warehouse, your enrichment providers, refreshed on a schedule. **Tools** : typed, callable units of work like enrich, verify, score. **Plays** : automation bound to data, firing as rows change. **Agents** : workers with a prompt, a step budget, an evaluator, and a set of things they’re allowed to use. **Context** : your ICP, your messaging, your objection handling, as versioned files an agent can read instead of tribal knowledge in someone’s head.


The test of a primitive is whether it composes. Here is a real agent definition:


```text
export   const   sdr   =   defineAgent  (  "sdr"  , {
systemPrompt:
"You qualify inbound leads, enrich missing contact info, and route hot leads to Slack."  ,
maxSteps:   12  ,
uses: [
{ ref: contacts, readOnly:   true   },   // a data model
enrich,   // a tool
{ ref: enricher, waitUntilFinished:   true   },   // a sub-agent
hunter.actions.findEmail,   // a connector action
],
triggers: [{ type:   "cron"  , cron:   "0 9 * * *"  , text:   "Daily qualification"   }],
evaluator: { rubric:   "Did it correctly qualify the lead?"  , threshold:   0.8   },
});


```


Read the` uses` array. A data model, a tool, another agent, a raw connector action, all passed the same way. That uniformity is the whole point. When everything is a primitive, everything composes with everything else. Your CRM is still there, doing what CRMs do. It’s a data source now. The revenue engine lives a layer up.


## Infrastructure means code#


If your revenue engine is going to be load-bearing, it can’t live in a canvas you screenshot for documentation. It has to behave like the rest of your software: in a repo, reviewed, diffed, deployed.


That’s how the whole workspace works. Connectors, models, plays, agents are resources you define in TypeScript. You plan to see what would change, deploy to reconcile, pull to turn a live workspace back into code. Engineers have managed infrastructure this way for fifteen years. GTM is simply the last department to get it.


The rule of thumb we use internally: if you’d commit it to git, define it in code. If you’d type it once, use the CLI. If you’d rather describe it than remember the flags, hand it to an agent.


Which brings us to the part that makes this agent infrastructure and not just GTM-as-code.


## The agent is a first-class client#


Everything above is reachable by an agent, natively. The workspace exposes itself as an MCP server, so any agent runtime can mount it: the tools, the data models, the actions, plus skills that teach the agent the entire surface.


The result is a different unit of work. “Trigger the scoring play on every lead added this week” stops being a feature request to a vendor or a ticket for ops. It’s a sentence. An agent reads it, finds the play, and runs the batch. The person who used to click through four tools to make that happen now reviews the output.


## Who this is for#


Not everyone. Cargo is built for the technical GTM builder: the engineer, or the ops lead with real engineering instincts, who says “give me the primitives and I’ll build it.” One builder ships the revenue engine in code. The rest of the org runs on what they built, without ever thinking about the layer underneath. That’s how infrastructure has always spread: sold to the builder, consumed by everyone.


If you want a finished workflow out of the box, the point tools are good and getting better. We’re the layer they’ll sit on.


The last decade of GTM software optimized the click. The next decade removes it. GTM is becoming an engineering discipline, agents are becoming the workforce, and both need something underneath them that was actually built for how they work.


That layer is being laid down right now. We’re building it.
