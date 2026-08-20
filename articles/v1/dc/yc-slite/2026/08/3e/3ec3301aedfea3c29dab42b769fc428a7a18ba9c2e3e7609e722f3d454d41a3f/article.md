---
schema_version: "1.0.0"
document_id: "3ec3301aedfea3c29dab42b769fc428a7a18ba9c2e3e7609e722f3d454d41a3f"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-62ed1fb859d3"
canonical_url: "https://slite.com/blog/you-dont-need-a-context-graph"
published_at: null
first_seen_at: "2026-08-12T05:05:14.173039+00:00"
fetched_at: "2026-08-12T05:05:16.593292+00:00"
content_hash: "sha256:adcd93120e22c7e0ce608fb9df1b45feb2974dfa0b0f8087fb8df2c0d209b325"
---

# Your company doesn't need a context graph

In 1998, Yahoo was the internet's front door: a hand-curated directory where human editors filed the web into neat hierarchical categories.


Google launched the same year on the opposite bet, that you would never need to map the territory if you could search it well enough on demand.


We all know how that ended. Organization lost to search.


The enterprise AI world is now replaying the exact same sequence, and the new directory has a name. It is called the context graph.


## Key takeaways


- A context graph is a stored, pre-computed model of every entity and relationship in your business. It is[Glean](https://slite.com/learn/glean-ai-review) 's framing for giving enterprise AI structured context.
- Agentic retrieval is the alternative: it discovers those relationships at query time instead of storing them in advance.
- No one championing context graphs has shown a use case that a well-built agentic retrieval system cannot already handle.
- The same debate was settled in code. Claude Code dropped its[RAG](https://slite.com/learn/rag) index for agentic search, and said so publicly.
- Stored graphs rot. Every reorg, pivot, and role change leaves stale edges that need constant gardening.
- A stored graph also has to mirror every source system's permissions. Agentic retrieval inherits them for free.


## What is a context graph?


A context graph is a persistent model of your company's knowledge: a stored map of the relationships between people, documents, projects, workflows, and SOPs.


[Glean](https://slite.com/learn/glean-ai-review) has made the strongest version of the case, describing a knowledge canvas that captures how every entity in your business connects.


The promise is multi-hop reasoning.


With the relationships mapped in advance, AI can answer a question that spans several systems, like which high-value deals are at risk because of unresolved P1 bugs in features those customers depend on.


It is a genuinely strong argument.


The open questions are:


- What does it cost to build and keep true?
- Do you need the stored map at all?


## What is agentic retrieval?


Agentic retrieval is the opposite bet. Instead of pre-computing a graph, you give a capable model a set of search tools and let it assemble context in the moment.


Agentic retrieval:


- reads your CRM,
- follows a thread into Slack,
- checks a timeline in Linear,
- and builds the answer from live data.


The structure people want from a graph emerges from the search itself.


It combines every method at once:


- [semantic search](https://slite.com/learn/semantic-search) ,
- keyword,
- grep,
- listing,
- and SQL queries.


This is the same family as[RAG](https://slite.com/learn/rag) , made agentic, where the model decides what to look up next based on what it just found.


## The context graph thesis, tested


Glean's founder Arvind Jain points to the kind of questions a context graph is built to answer: how P1 incidents usually get resolved, the most common escalations about a product, what typically happens between "pilot created" and "deal closed."


We ran every one of them through an agentic retrieval system connected to all our data sources, orchestrating[search](https://slite.com/learn/enterprise-search) at query time with no pre-computed graph underneath. It answered them, and it could show how it inferred each answer.


Then we pushed harder.


List every pricing change we have ever made and what triggered each one. Surface every failed go-to-market hypothesis from the last five years.


Those queries span a hundred-plus sources with heavy time-decay.


The agentic system handled them by being fast and smart enough to discover the relationships in the moment, rather than knowing them in advance.


For companies under a thousand employees, and I suspect well above that line, context can be inferred from records at query time. You do not need a stored ontology.


You need agentic retrieval.


## The debate was already settled in code


This same question got answered in code months ago. Cursor originally built its coding assistant around RAG on the local codebase, indexing the whole repository into a vector database and treating semantic search as a core differentiator.


Claude Code tried the same approach early, then dropped it. Boris Cherny, who built Claude Code, put it plainly:


> Early versions of Claude Code used RAG + a local vector db, but we found pretty quickly that agentic search generally works better. It is also simpler and doesn't have the same issues around security, privacy, staleness, and reliability.


Instead of maintaining an index, Claude Code now uses tools like grep, file reading, and directory listing to explore a codebase on demand.


The simpler, less obvious solution won.


It took[models trained for agentic work](https://slite.com/learn/ai-agent) to make it possible, and once that unlocked, it became the obvious pattern.


Cursor later hired the team behind Claude Code's agentic search.


## Context graphs are expensive to keep accurate


Even if you accept the theory, there is a practical problem proponents understate: the cost of keeping the graph true.


Consider CRM data, arguably the most structured and most invested-in category of enterprise data in existence. Companies spend millions on CRM hygiene, with dedicated teams, enrichment tools, and executive mandates, and CRMs are still messy.


Now imagine maintaining a graph that encodes the informal, constantly shifting relationships between people, projects, decisions, and knowledge. It[rots](https://slite.com/learn/knowledge-drift) .


Every reorg invalidates parts of it.
Every pivot orphans nodes.
Every person who changes role leaves[stale edges](https://slite.com/learn/dangers-of-stale-documentation) behind.


The gardening requires understanding the semantics of what changed, which is exactly the judgment that is expensive to automate and easy to neglect.


"Agents will maintain it," the argument goes.


We are building exactly that kind of agent, and even so, human validation stays in the loop.


The graph does not maintain itself without oversight.


So ask what you would rather review:


- your canonical data, the docs and projects themselves,
- the relational data, every way those things interconnect.


The second is exponentially larger and far harder to check. Reviewing canonical knowledge is a much better use of a human's time.


## Then there's permissions


A stored graph inherits a second maintenance burden: access control. It has to mirror the permissions of every connected system at once, and update not only when data changes but when permissions change.


You take on all the imprecision of your source systems and add a synchronization layer on top.


Agentic retrieval sidesteps this.


When it searches Salesforce for a user, it returns what that user is allowed to see in Salesforce.


No separate permissions layer, no sync, and no gap between what the graph thinks you can see and what the source actually allows.


## What we saw when we shipped enterprise search


I have spent the last decade building tools for teams to share knowledge. I am the founder of Slite, and for most of that time the obvious answer to "how do I make sure my teammates can find this?" was to organize better.


Then we shipped[enterprise search](https://slite.com/ai-search) , and usage shifted fast.


People stopped browsing. They cared far less about folder structure. They just asked.


The instinct to organize is deeply human, and past a certain scale it is a losing investment.


The context graph thesis is the enterprise version of "if we just organized the wiki better, people would find things."


That has never been how it plays out.


Today Slite is a[self-maintaining knowledge base](https://slite.com/learn/self-maintaining-knowledge-base-guide) : a[company brain](https://slite.com/learn/claude-code-company-brain) kept in sync with reality, where an agent searches across your connected tools, proposes fixes when docs drift, and routes every change through human approval.


The retrieval is agentic. The map builds itself on the fly.


## The instinct has been wrong before


The urge to build a persistent model of everything you know is understandable. It has also been wrong before.


It was wrong for the web in 1998.
It was wrong for code in 2024.
It is wrong for enterprise search today.


The way to harness your company's data is agentic retrieval that combines your team's knowledge with every search tool available, building context the moment it is asked for.


A company brain does not need a context graph underneath it. It needs to be able to look things up.


See how Slite builds a company brain that stays in sync with reality, with no stored graph required:[http://slite.com/slite-agent](http://slite.com/slite-agent)
