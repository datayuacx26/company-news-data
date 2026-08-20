---
schema_version: "1.0.0"
document_id: "887e3bb862e2d37bb4d5a69d85bfcb51ad7facdaa86c2e86ea5bebc5eccd835b"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/introducing-agent-search"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-31T19:20:01.369509+00:00"
fetched_at: "2026-07-31T19:20:02.511618+00:00"
content_hash: "sha256:7affba41e7b5bc49256746d3869dd89135a5cab8dddcd7e282c1e25a6bb4d24e"
---

# Introducing Agent Search: email search built for agents, not people

TL;DR


- **What:** Agent Search is full-text relevance search across AgentMail inboxes and threads. It's built for a consumer that issues one query, reads one page of JSON, and acts on it.
- **Why it matters:** Human email search works because a person reformulates, skims, and tries again. An agent does none of that. If the right message isn't in the first page of results, the agent concludes it doesn't exist and proceeds confidently on that basis.
- **How it works:** Prefix matching on sender, recipient, and subject so a partial identifier matches on the first attempt. Quoted replies and signatures stripped before indexing. Per-field match attribution so an agent can triage without a model call. Stable pagination. Spam and trash excluded, with no opt-in.
- **Who it's for:** Any team building agents that read email, from support and sales agents to invoice and document workflows.
- **Get started:**` GET /v0/inboxes/{inbox_id}/messages/search` ,` GET /v0/threads/search` , or the` search_messages` and` search_threads` MCP tools. Live on all AgentMail inboxes.


## What is Agent Search?


Agent Search is AgentMail's relevance search over email. It runs against sender, recipient, subject, and message body, returns ranked threads or messages, and tells the caller which fields matched.


It takes one query string. There are no boolean operators, no field prefixes, and no query language to construct. Precision comes from typed filter parameters on the list endpoints, where a malformed value is a validation error instead of a silently wrong result set.


## Why we built it


Our goal with AgentMail is to be the Gmail for AI agents. That's a useful shorthand, and it hides the actual work: almost every part of email that feels solved for humans is unsolved for agents. Search is the clearest example.


Human email search is a conversation between a person and an interface. You type two words, skim the bolded fragments, notice the sender, spot the date, scroll, refine, try again. The interface does half the retrieval and the person does the other half. The ranking function only has to get the right answer somewhere into the visible top ten, because the person does the last mile, and when it fails they search again with better words.


An agent has no loop. It issues one query, gets a JSON page, and acts. No skim, no glance at the sender, no second attempt. If the right message isn't in that page, as far as the agent is concerned it does not exist, and it will report that to your user as fact. That's worse than failing.


An agent also can't cheaply look closer. A human scanning ten results spends a second. An agent reading ten results spends roughly ten thousand tokens and a model call. Precision isn't a quality metric here, it's a cost function.


So we rebuilt every search decision around what's left when the human is removed.


## What agent queries actually look like


Human queries are recalled approximations. You half-remember a message, type` stripe invoice` , and correct toward the target based on what comes back.


Agent queries are identifiers, usually values the agent lifted verbatim out of its own prior context: a field from an earlier tool call, a string the user pasted, an order number it generated three steps ago.


```text
INV-4471
stripe.com
noreply@github
acme


```


Two consequences follow, and neither applies to human search.


**The token is exact but the boundary is wrong.** The agent holds` stripe.com` because that's what was in its context. The mailbox has` billing@stripe.com` . A human bridges that gap instantly by trying something else. An agent submits the exact string it holds, and if that returns nothing, it's done.


**An empty result is treated as ground truth.** Not as "try differently," but as "no such message." That's the failure mode the whole system is designed to avoid.


So partial matching has to work on the first attempt, without becoming fuzzy enough to return garbage, because a wrong top result gets acted on.


## How deep this goes


**Prefix matching without slop.** Sender, recipient, and subject index as edge n-grams, every leading substring of each alphanumeric run from 2 to 24 characters, with` @` ,` .` ,` -` and` <` treated as break characters. So` stripe.com` finds` billing@stripe.com` , and` inv` finds` invoice` . Index time and query time use different analyzers, so documents expand into prefixes while queries stay whole, and every query token is required to match. Prefix matching without that last constraint is a slower way to return noise.


**Identifiers stay exact.** No stemming, no stopword removal, no synonyms. The only normalization is lowercasing and accent folding. Stemming trades precision for recall, which is the right trade for a human typing from memory and the wrong one for an agent holding an order number. Typo tolerance applies to message bodies only, never to identifier fields.


**Quoted text is stripped before indexing.** An email body is mostly not the email. It's a quoted reply chain, a signature block, a disclaimer, an unsubscribe footer, and a pile of HTML layout. Every mail client hides that at render time, which is why most people forget it's in the payload. An index gets no such help, and indexed naively, a ten-message thread becomes ten near-identical documents and ranking collapses into noise. So quoted replies and signatures are removed, HTML is flattened to text, and bodies are capped at 20KB. What gets indexed is what the sender actually wrote.


**Recency promotes, never suppresses.** The score is relevance with a bounded multiplier, floored so a fresh message can outrank a more relevant old one by a limited factor and no further. A contract signed eight months ago is more relevant to "contract terms" than yesterday's newsletter, and an agent asked to find that contract will accept whatever is ranked first. The weight is expressed as a ratio, which makes it scale-free, so it holds on a 10k-message inbox and a 10M-message one without retuning.


**Pagination is stable.** The recency origin is floored to the top of the hour and shared across every page in a session. Otherwise the page-1 to page-2 boundary shifts between requests, and results duplicate or vanish. A human notices a duplicate. An agent processes the same message twice or never sees the one that slipped through the gap, silently.


**Results carry match attribution.** Every result includes matched fragments per field, wrapped in` **` rather than HTML, because results land in a model's context window where markdown is native and tags cost tokens. A field only appears if the query matched that field, so the keys themselves tell the caller what caused the hit. A result matching only` from` is a "mail from this sender" hit. One matching` subject` and` text` is probably the conversation the agent wanted. That's a triage decision made without reading the message, which is the entire point, because reading the message costs a model call.


**Two modes, because agents search two ways.** Relevance search (` ?q=` ) is scored and ranked, for finding a specific thing. Filtered lists (` ?from=stripe.com&subject=invoice` ) are not scored at all: chronological, cursor-paginated, exactly like an unfiltered list. Agents page through mail programmatically to drain queues and reconcile batches, and relevance ordering is actively harmful there. Adding a filter changes what you get, never the order you get it in.


**Failures degrade rather than error.** If reranking times out, you get candidate order. If fuzzy fill fails, you get exact results only. An agent's response to an error is an immediate retry, which turns a slow query into a load amplifier. But partially-successful queries, where some shards answered and some didn't, are treated as errors rather than results, because a silently truncated result set is indistinguishable from "no such message." Degrade the ranking, never the completeness.


## One string, five parameters


The relevance endpoint takes` q` ,` limit` ,` page_token` ,` before` , and` after` . Page size goes up to 100, not the 20 a human interface would use, because there's no screen.


If your agent talks to AgentMail over MCP,` search_messages` and` search_threads` are available with no additional setup.


## Search results are untrusted input


Search is a retrieval surface into content written by strangers, which makes it a direct path for attacker-controlled text to reach a model's context window.


Restricted labels (spam, blocked, unauthenticated, trash) are excluded from relevance search unconditionally, with no opt-in parameter. Tenancy is filtered on every query before any scope narrowing. And the MCP tool descriptions state plainly that content originates from external senders and should not be treated as instructions.


## Who this is for


Anyone with an AgentMail inbox. Agent Search is on by default, there's nothing to enable, and it doesn't cost extra.


It matters most wherever a wrong or empty result gets acted on rather than double-checked, which is most agent workflows. If your agent looks up a conversation before replying, resolves an identifier, checks whether something was sent, or pages through mail programmatically, this is the retrieval layer underneath all of it.


## What this unlocks for you


You can hand an agent a fragment of an address or a bare order number and have it match on the first attempt, instead of building a query-rewriting layer to guess at boundaries.


You can branch on which field matched instead of reading messages to find out, which removes a model call from your triage path.


You can page through results programmatically without deduplicating, because the ordering doesn't shift underneath you.


And an empty result set means the mailbox doesn't contain it, rather than meaning the query was shaped slightly wrong.


## Summary


Very little of Agent Search is novel information retrieval. It's standard relevance scoring, prefix indexing, and field collapsing, all decades-old machinery. What's different is that every tradeoff is resolved in the opposite direction from a human search product: precision over recall, structured attribution over visual highlighting, one opaque string over rich operators, degraded ranking over errors, and a partial result treated as a failure rather than a page.


A human is a retrieval system with a search box attached. An agent is not. Agent Search is what email search looks like once you stop assuming otherwise.


**Agent Search is live on all AgentMail inboxes.**


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
