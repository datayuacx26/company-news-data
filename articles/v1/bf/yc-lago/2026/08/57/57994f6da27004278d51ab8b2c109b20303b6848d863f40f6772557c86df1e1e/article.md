---
schema_version: "1.0.0"
document_id: "57994f6da27004278d51ab8b2c109b20303b6848d863f40f6772557c86df1e1e"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/generative-ai-in-finance"
published_at: "2026-08-14T08:46:14+00:00"
first_seen_at: "2026-08-14T15:23:45.920294+00:00"
fetched_at: "2026-08-14T15:23:48.528570+00:00"
content_hash: "sha256:ff8c7591c77059bb473dc91a4ecca0f223fe68f32abd6d95b39d9663e0624d12"
---

# Generative AI in Finance: Separating the Real Use Cases from the Hype

Disclosure up front, because this piece has an edge to it and you should know where it's coming from: Lago sells an AI assistant that sits on top of billing data. So take the "here's what's real" framing below with that in mind. I still think it holds up, and honestly, being the vendor is exactly why I've sat through enough vague pitches from other people's decks to know one when I see it.


A finance director I talked to a few months back described her team's first generative AI pilot in one sentence: "it wrote beautiful summaries of things we already knew." That's not nothing, but it's also not what the slide deck promised. Somewhere between "generative AI will transform finance" and "it wrote a nice paragraph," a lot of vendors quietly stopped being specific, and that gap is where most of the disappointment in this space actually lives.


## What generative AI means here, and where it gets confused with agents


Generative AI and AI agents get used almost interchangeably in finance marketing, and they shouldn't be. Generative AI produces content: text, summaries, drafted emails, synthesized explanations. An agent takes autonomous multi-step action toward a goal, generative AI is often the component that lets it write the output, but the autonomy is a separate capability. We've written more on that distinction in[AI Agents in Finance](https://getlago.com/blog/ai-agents-in-finance) if you want the fuller version. For this piece, the shorter version matters more: generative AI alone, without agentic structure around it, is a drafting and summarizing tool. That's a real, useful thing. It is not a system that goes and reconciles your books.


## Where it actually works


Three places, and they share a common trait: the output gets checked by a human before it does anything.


Drafting routine communications is the easiest win, and it's basically solved. Dunning emails, renewal notices, collections follow-ups. Generative AI drafts based on account context, a person approves, low stakes if the first draft isn't perfect.


Summarization is the second, and it's where that finance director's "beautiful summaries" line comes from. Turning a pile of transaction data or a messy vendor contract into something a human can scan in thirty seconds. Genuinely useful. Also genuinely limited to information that's already there. It's not analysis, it's compression.


Natural-language querying over clean, structured financial data is the third, and it's the one with the most upside precisely because billing and revenue data tends to be well-modeled: subscriptions, invoices, usage events, clear relationships between them. Ask "why did net revenue retention drop for this segment" and get an answer grounded in real numbers instead of a search through five dashboards.[Billing observability](https://getlago.com/blog/billing-observability) work has been building toward exactly this kind of visibility for a while, generative AI is the layer that makes it conversational.


## Where the pilots stall


Here's the part most of what's currently ranking for this topic skips entirely: naming what actually goes wrong.


The first failure mode is ungrounded number generation. A model asked "what was our churn rate last quarter" that doesn't have a hard connection to the actual billing system will sometimes produce a plausible-sounding number that's wrong, and it will say it with exactly the same confidence as when it's right. This is the single most dangerous failure mode in finance specifically, because a wrong paragraph in a marketing draft gets caught by a human skim. A wrong number in a board deck gets believed.


The second is pointing generative AI at judgment calls instead of retrieval. "Should we write off this receivable" is a decision that depends on context a model doesn't reliably have: relationship history, negotiating leverage, what happened in a phone call last week. Teams that expect a drafting tool to make a judgment call get a confident-sounding answer that's really just a guess wearing a suit.


The third, and the most boring to say out loud, is bad underlying data. If your billing events are inconsistent or your revenue categorization has drifted, a generative AI layer on top doesn't fix that. It just answers questions faster, including wrong ones, faster.


None of the OECD paper, the IBM overview, or the Deloitte transformation piece on this topic name a specific failure pattern. They gesture at "governance" and "responsible adoption" without saying what actually breaks. I think that's a tell. If a piece about generative AI in finance can't point to something concrete going wrong, it hasn't gotten close enough to real deployments to know.


## What to check before you adopt one


A short, practical list, not a governance framework. Ask the vendor whether every number the tool produces traces back to a source record you can check, or whether it's generating from a general model with your data as loose context. Ask what happens when it doesn't know the answer: does it say so, or does it guess. Ask whether there's a human-approval gate before anything customer-facing goes out, and whether that gate is enforced by the workflow or just a suggestion in the docs. And ask for one example of a query or task the tool refuses or flags as low-confidence. If a vendor can't produce one, they either haven't tested it against hard cases or aren't telling you what they found.


Even Ramp, who's clearly invested real effort into AI-in-finance positioning, doesn't have a page that ranks for this specific question. That's not a knock on them. It's evidence that almost nobody has written the honest version of this yet, which is a strange gap given how much money is riding on teams getting this decision right.


## Key takeaways


Generative AI in finance works today for drafting, summarization, and natural-language querying over clean structured data, and it does not reliably work for judgment calls or as a substitute for fixing bad underlying data. The failure mode that matters most is confident, ungrounded number generation, since a wrong number in finance gets trusted the same way a right one does. Before adopting a tool, ask specifically how it grounds its numbers, what it does when it's uncertain, and whether human approval is enforced or just assumed.
