---
schema_version: "1.0.0"
document_id: "a2e38d711a58d3d4e7ed8149be14fc8c336ce3e4d9731b3b1f9aeb4263c6b56a"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/zemail/"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:46fbee74e6e45d1886abe2a2c68406a8e4af58c2273d939c09ed47bbbcd875a1"
---

# Zemail: Semantic Gmail Search on Claude Code & Cowork

TL;DR


- **Zemail** is a free, open-source[Claude Code](https://docs.claude.com/en/docs/claude-code) plugin that gives Claude semantic search over your Gmail inbox.
- Gmail search is keyword-matched. Claude’s default Gmail connector is keyword-matched, paginated, and usually gives up before finding what you meant.
- Zemail[embeds](https://www.zeroentropy.dev/concepts/embedding/) your inbox locally with[zembed-1](https://www.zeroentropy.dev/articles/introducing-zembed-1-the-worlds-best-multilingual-text-embedding-model) and[reranks](https://www.zeroentropy.dev/concepts/reranker/) with[zerank-2](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker) . The index lives on your machine, not ours.
- Install:` /plugin marketplace add zeroentropy-ai/claude-plugins` →` /plugin install zemail@zeroentropy`
- Source:[github.com/zeroentropy-ai/zemail](https://github.com/zeroentropy-ai/zemail)


# Zemail


## Semantic Gmail Search for Claude Code


Gmail’s search box is older than many of its users, and that is reflected in the efficacy of its search. It matches keywords. If the email you’re thinking of doesn’t contain the exact word you typed, then as far as Gmail is concerned, it simply does not exist.


Claude’s built-in Gmail connector inherits this. Ask Claude to find an email, and it issues a keyword query, reads the first page of results, shrugs, and tells you it couldn’t find anything. The email *is* there; it just doesn’t share any vocabulary with the question you asked.


**Your inbox is a retrieval problem, and Gmail’s keyword search is a retrieval system from 2004.**


**Zemail** is a Claude Code plugin that solves this problem. Embed every email,[cosine-search](https://www.zeroentropy.dev/concepts/cosine-similarity/) by meaning, rerank the top candidates, hand the ten best to Claude. The same two-stage pipeline every serious[RAG](https://www.zeroentropy.dev/concepts/rag/) system converges on, pointed at your own mailbox.


## The Emails Gmail Can’t Find


Real Queries, Real Failures


- “the refund email from the airline about the cancelled flight last spring” — the actual subject line is *Your booking PQR-48X2 has been updated* . The word *refund* appears nowhere.
- “the Stripe receipt for the domain I bought” — Stripe calls it a *payment* , the registrar calls it an *order* , the word *domain* is in the body of neither.
- “who was the dentist my dad recommended” — your dad’s email said *“you should see Dr. Chen”* . It did not say *dentist* .
- “that newsletter with the sourdough recipe” — the issue is titled *Issue #47: Spring Breads* . Sourdough is one ingredient among six.
- Any query against an inbox that mixes languages. Gmail’s tokenizer does not know that *facture* and *invoice* are the same word.


None of these examples are exotic. They’re the median thing you actually want to find in your own email, and Gmail loses every one of them. A keyword index cannot match concepts; the model on the other side of a keyword index cannot find what the index didn’t return.


## How It Works


Zemail is a local[MCP](https://www.zeroentropy.dev/concepts/mcp/) server that Claude Code runs as a plugin. When Claude needs to search your email, it calls Zemail instead of Gmail’s keyword search.


#### Sync


Zemail pulls messages through the Gmail API (read-only, incremental), sends each subject + body to[zembed-1](https://www.zeroentropy.dev/articles/introducing-zembed-1-the-worlds-best-multilingual-text-embedding-model) , and writes the returned vectors to a float32 file on your disk. First sync takes a few minutes; subsequent syncs only process new mail.


#### Query


Claude asks a natural-language question. Zemail embeds the query, runs brute-force cosine similarity over the in-memory matrix, and takes the top 50 candidates. Even a few hundred thousand 2560-dim[vectors](https://www.zeroentropy.dev/concepts/vector/) is a couple hundred milliseconds on a laptop.


#### Rerank


The top 50 go to[zerank-2](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker) , which actually reads the query against each document and returns a[calibrated relevance score](https://www.zeroentropy.dev/concepts/score-calibration/) . The top 10 come back to Claude, which can call` get_email` on any ID to pull the full body.


The tools Zemail hands Claude:


Tool Purpose


` sync_emails` Fetch from Gmail, embed, write the local index.


` search_emails_tool` Embed the query, cosine top-50, rerank with zerank-2, return top 10.


` get_email` Fetch the full body of a message by ID.


` index_status` Report index size and last sync time.


` authorize_gmail` Paste-in OAuth flow for SSH / headless setups.


Claude decides when to call each; you just ask questions.


## Scope and Data


Zemail requests a single Gmail scope,[gmail.readonly](https://developers.google.com/gmail/api/auth/scopes) , which is the minimum required to read message bodies for indexing. Claude away with ease of mind: Zemail can’t send, edit, or delete any of your emails.


Your OAuth token, message metadata, and embedding index are stored locally in` ~/.zemail/` . Email text is sent to the ZeroEntropy embedding and rerank APIs only as part of each request, and is not retained. All your search happens locally and securely. Source:[github.com/zeroentropy-ai/zemail](https://github.com/zeroentropy-ai/zemail) .


## Install


#### Add the marketplace


In Claude Code:


```text
/plugin marketplace add zeroentropy-ai/claude-plugins
/plugin install zemail@zeroentropy
```


Claude will prompt for a ZeroEntropy API key at install. Paste a key from[dashboard.zeroentropy.dev/api-keys](https://dashboard.zeroentropy.dev/api-keys) — free, no credit card, keys start with` ze-` .


#### Authorize Gmail


The first time Claude uses Zemail, it’ll print a Google OAuth URL. Open it, pick your account, approve the read-only scope, paste the code back. The refresh token is saved locally so you only do this once.


#### Sync and ask


Tell Claude to run` sync_emails` . Once the index is built, ask: *“find the airline refund from last spring”* , *“who did my dad recommend for a dentist”* , *“pull the Stripe receipt for that domain”* . Claude calls` search_emails_tool` , reads the top few hits, and answers.


## Why We Built This


We train the[best](https://www.zeroentropy.dev/articles/mteb-evals) embedding and reranker models available, and the hardest part of trying them on your own data is usually the plumbing. Zemail is a way to skip the plumbing. Install a plugin, authorize a scope, watch a two-stage retrieval pipeline find the email you gave up on finding a year ago.


If it works on your inbox, it will work on your company’s documents.


### Get Zemail


Free, open-source, and runs locally. The ZeroEntropy API key is free too.


[→ GitHub Source code and installation docs](https://github.com/zeroentropy-ai/zemail)[→ Plugin Marketplace ZeroEntropy’s Claude Code plugins](https://github.com/zeroentropy-ai/claude-plugins)[→ Free API Key Sign up, grab a ze- key](https://dashboard.zeroentropy.dev/api-keys)[→ Discord Tell us what it surfaced in your inbox](https://discord.gg/5mkQCTnmY9)


Install in Claude Code:


```text
/plugin marketplace add zeroentropy-ai/claude-plugins
/plugin install zemail@zeroentropy
```


**Models** :[zembed-1](https://www.zeroentropy.dev/articles/introducing-zembed-1-the-worlds-best-multilingual-text-embedding-model) ·[zerank-2](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker) ·[evals](https://www.zeroentropy.dev/evals/)
