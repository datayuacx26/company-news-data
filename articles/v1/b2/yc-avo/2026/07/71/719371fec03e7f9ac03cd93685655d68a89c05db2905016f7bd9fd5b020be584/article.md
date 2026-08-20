---
schema_version: "1.0.0"
document_id: "719371fec03e7f9ac03cd93685655d68a89c05db2905016f7bd9fd5b020be584"
company_key: "yc-avo"
company: "Avo"
source_id: "yc-avo-news-import-5b3bdf315861"
canonical_url: "https://www.avo.app/blog/govern-your-data-your-agents-depend-on-it"
published_at: null
first_seen_at: "2026-07-21T08:54:15.034482+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:8c9bea84b5cd38c8d3d4417b52c2a1cf707b9ae03fa1db2a057eb4399918db35"
---

# Govern your data. Your agents depend on it.

Every team I talk to is racing to put agents on top of their data. Almost none of them trust what comes back. That gap is the problem Avo exists to close.


The code half is basically solved. Developers are shipping with AI everywhere\[[1](https://www.fastcompany.com/91531519/google-ceo-says-75-of-the-companys-code-is-ai-generated) ,[2](https://www.anthropic.com/institute/recursive-self-improvement) ,[3](https://survey.stackoverflow.co/2025/ai) \], and the tools are good enough that most teams don't debate it anymore — even as trust in AI output sits at 29%\[[3](https://survey.stackoverflow.co/2025/ai) \]. Code at least has a safety net: it compiles or it doesn't, the tests pass or they fail. Data has none. A wrong number looks exactly like a right one, and an agent will hand it to you with total confidence.


So when an agent can build almost any query in seconds, the hard question isn't whether it can build the thing. It's whether you can trust it. Three beliefs come out of that, and they guide everything we build.


Anthropic published how their own data team made Claude trustworthy on analytics\[[5](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) \] while this was in draft. Their post confirmed every belief in it.


### **Belief 1: Agents will be confidently wrong about your data. Well structured data and context management is what makes the difference.**


Without context, an analytics agent is a junior analyst making junior mistakes at machine speed. With it, a senior one. The difference is the structured data and definitions it can reach for first. Anthropic proved this on itself: without structured context, Claude answered their analytics questions correctly less than 21% of the time; with it, above 95%. Same model. They even gave it thousands of past queries and confirmed it read them, and accuracy barely moved\[[5](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) \]. The agent didn't need more to look at. It needed to know what things mean.


We see it from both sides. Let an agent write into a tracking plan on its own and it invents events that never existed. Give it Avo's guardrails and the quality jumps: the audit rules enforce your conventions, and the tracking-plan context tells it what already exists. Now customers run it in reverse, defining meaning in Avo and feeding it down into the warehouse so the agent answering questions there reaches for governed definitions instead of guessing from raw events. Avo is where teams (of agents and humans) design, govern, and define what their data means, so an agent looking for an answer finds the truth instead of a plausible guess.


### **Belief 2: The analytics stack is collapsing. Because it can. And it can only collapse onto well structured data.**


The stack existed because it had to. Fifteen years ago, rendering charts on millions of events was a hard engineering problem; vendors built their own databases to solve it. That constraint is gone. Your events already land in BigQuery, Snowflake, or Databricks, so why lock a second copy inside Mixpanel or Amplitude when an agent can answer the question right where the data lives? And I say that with love. I'm genuinely excited about where PostHog and Amplitude are headed next: the product iteration machine, a product that improves itself from its own data. It's long been Avo's ultimate vision for the world too. But that's the tell: their next act doesn't need the proprietary analytics store either. Even the companies that built this layer are betting on something beyond it. It's collapsing because it can.


So what does the stack collapse onto? Two legs are obvious: the warehouse holds the data, and the agent does the work. But a warehouse stores whatever you send it, and an agent answers from whatever it finds. Let two teams log the same action under different names and the agent quietly picks one and reports a number far lower than reality, with no sign anything is wrong. The collapse only works with a third leg: a context layer that knows what the data means. Even Anthropic's instructions to its own analytics agent put the governed semantic layer first\[[5](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) \]. And their advice for teams starting from zero? A handful of canonical datasets. "Everything else in this post is what we added once those were built."\[[5](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) \] The accuracy is downstream of the quality of the dataset. Always.


Here's the part their post leaves implicit. Everything they built stands on quality data that already existed; they know the third leg has to start sooner. So I'll say it explicitly:


> A semantic layer alone is a **deferred source of truth** : it describes your data only after it exists, blessing whatever mistakes already shipped.


Deferred is the polite word for too late. Meaning has to start where the data is born, in how an event is designed and implemented, then flow down to everyone, human or agent, who reaches for it. The warehouse holds the data. The agent does the work. Avo holds what it all means. **‍**


### **Belief 3: Data governance needs to be correct every time. Correct most of the time doesn't suffice.**


Anyone can ship an AI product that works most of the time; the hard part is shipping one a governance team will let into production. Get one decision wrong on identifiable data and the cost is regulatory and legal, not a bug. That isn't hypothetical: a major analytics vendor faced privacy claims over how it handled user data on a customer's behalf\[[4](https://news.bloomberglaw.com/privacy-and-data-security/doordash-consumers-must-arbitrate-amplitude-data-privacy-claims) \].


A team with Claude can ship a working agent in a weekend. They can't ship a trusted one, and at agent speed the gap between working and trusted only grows. Most people get this backwards: trust isn't the brake on AI, it's the accelerator. Even Anthropic admits the failure their safeguards miss is the silent one, an answer that is wrong, looks right, and gets used anyway\[[5](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) \]. The companies that move fastest next year will be the ones whose agents earned enough trust to run without a human checking every output. Everyone else stays stuck in review.


That trusted layer is what we're building Avo to be: vendor-neutral, governed, where every property declares whether it carries PII before it ships.


### **Where to start**


Three moves, in order:


1. **Maintain a source of truth schema.** Anthropic's canonical datasets are themselves downstream; they're built from the events you track. So start one step earlier: pick the events behind your most important questions, give each a reviewed definition in your schema, and let everything else build on them.
2. **Make your schema your agents' first stop.** Anthropic's own skill file opens with it: "the governed semantic layer is the mandatory default path for every data question"\[[5](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) \]. Borrow the line, swap the noun: your schema becomes that default path, the required first step before a single query runs, so the agent knows what your data means and how it's used. **‍**
3. **Verify what ships against your schema.** Trust has two halves, and design is only the first. Compare the events your apps actually send against the schema, so when an implementation drifts, you find out from a system, not from a wrong number three weeks later.


You can build all of this yourself: a schema repo with a review process, a lookup your agents can call, ingestion-time validation. Or you can just use Avo, because this is the core of what Avo is. The tracking plan is that source of truth schema. Agents read it over the Avo MCP as their first stop. The Inspector compares what your apps actually send against what you designed.


Your source of truth shouldn't be deferred. It should be designed. Govern your data. Your agents depend on it.


#### **References**


- \[1\] Sundar Pichai, Google Cloud Next 2026: 75% of new code at Google is AI-generated — https://www.fastcompany.com/91531519/google-ceo-says-75-of-the-companys-code-is-ai-generated
- \[2\] Anthropic, "When AI builds itself": more than 80% of production code merged at Anthropic is authored by Claude — https://www.anthropic.com/institute/recursive-self-improvement
- \[3\] Stack Overflow, 2025 Developer Survey (AI section): 84% adoption; trust in AI output down to 29% — https://survey.stackoverflow.co/2025/ai
- \[4\] DoorDash consumers must arbitrate Amplitude data privacy claims (Bloomberg Law) — https://news.bloomberglaw.com/privacy-and-data-security/doordash-consumers-must-arbitrate-amplitude-data-privacy-claims
- \[5\] Anthropic, How Anthropic enables self-service data analytics with Claude — https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude
