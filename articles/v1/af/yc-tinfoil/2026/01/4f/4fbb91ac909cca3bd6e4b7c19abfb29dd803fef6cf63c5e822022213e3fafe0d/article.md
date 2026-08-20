---
schema_version: "1.0.0"
document_id: "4fbb91ac909cca3bd6e4b7c19abfb29dd803fef6cf63c5e822022213e3fafe0d"
company_key: "yc-tinfoil"
company: "Tinfoil"
source_id: "yc-tinfoil-news-import-a84300979e46"
canonical_url: "https://tinfoil.sh/blog/2026-01-22-private-ai-web-search"
published_at: "2026-01-22T12:00:00+00:00"
first_seen_at: "2026-07-22T16:45:23.137007+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:90c22a627c99ae113e36b9fcf783812dab8a33172cb7299f7ea3b5daf3b48540"
---

# How We Implemented Private AI Web Search

[← Back to Posts](https://tinfoil.sh/blog)


# How We Implemented Private AI Web Search


Jan 22, 2026


•


11 min read


Tinfoil Team


A useful feature of AI chatbots and agents is the ability to automatically search the web. This grounds answers in verifiable references, reduces hallucination, and keeps the model informed of current events.


On Tinfoil Chat, web search is an optional feature that users must explicitly enable. Unlike pure model inference where the privacy of conversations is enforced through secure hardware enclaves, web search is more complex. The model needs to make external requests that contain search terms derived from private conversations. The search provider can be legally compelled to expose search queries even if Tinfoil itself cannot see them. The defenses against this threat that we built into our system are twofold:


1. We use Exa as our search provider with[Zero Data Retention](https://exa.ai/blog/zdr-search-engine) enabled. This means queries are never written to persistent storage or sent to external subprocessors, providing a legal safeguard around query privacy.
2. The model we use to generate search queries,[gpt-oss-120b](https://tinfoil.sh/models/gpt-oss-120b) , has been[safety trained](https://cdn.openai.com/pdf/419b6906-9da6-406c-a19d-1bb078ac7637/oai_gpt-oss_model_card.pdf) to exclude sensitive information such as personally identifiable information (PII) from responses. As an additional safeguard, we run a guardrail model,[gpt-oss-safeguard-120b](https://tinfoil.sh/models/gpt-oss-safeguard-120b) , that blocks any search query where PII is detected, before the query even leaves the secure enclave.


Unfortunately, we cannot promise protections beyond legal frameworks and careful prompting to minimize risk of PII exposure to third parties. This stands in contrast to our verifiable privacy guarantees around pure AI inference backed by confidential computing. Therefore, we recommend that users who are fully tin foil about their privacy not include PII in conversations when web search is enabled.


In this post, we dive into the system design, threat model, and evaluation of our approach to private web search in Tinfoil. We architected everything to minimize trust in Tinfoil and the search provider, making sure that no one learns who searches for what.


## System Overview


The web search system consists of a client, an AI model running inside a secure hardware enclave, and a search provider. The client encrypts and sends the conversation directly to the AI model running inside the secure enclave. The model can then issue a search query to the search provider and receive results.


**Figure 1:** Data flow in the lifecycle of a web search query.


We run the[search agent](https://github.com/tinfoilsh/confidential-websearch) within a secure enclave. This search agent performs three operations:


1. Generates search queries from the conversation context.
2. Establishes a TLS connection directly to the search provider, authenticated via Tinfoil's API key.
3. Returns search results to the model without exposing queries to Tinfoil.


### Design Goals


Web search is a complex feature for which we had several stringent design goals we wanted to hit around privacy and usability.


1. **Query Privacy** : Tinfoil must not learn search query contents.
2. **User Anonymity from Search Provider** : The search provider must not be able to identify the originating user or link a specific Tinfoil user to a search query.
3. **Minimal Information Disclosure** : Queries should contain only the information necessary to perform the search, nothing more. In particular, queries should not contain PII from the conversation.
4. **Server-side search** : Searches are executed server-side, with no client-side requests or round trips. This ensures low latency and ability to perform long-running agentic tasks.


### Security Limitations


We want to be upfront about what we cannot protect against and under what assumptions. Ensuring unlinkability between users and search queries requires Tinfoil and the search provider to be independent and non-colluding. If Tinfoil and our search provider were both compelled under law, a timing correlation attack could link specific users to specific queries (the conversation itself would still remain private as it is protected by the secure hardware). Our current user base is small enough that a timing correlation attack is viable. If this risk is unacceptable, note that web search is entirely opt-in, and can be disabled at any time, even on a per-message basis.


## 1. Query Privacy


Query privacy comes from running the search agent using Tinfoil infrastructure (inside a secure enclave). For privacy against the search provider, we have to rely on legal contracts such as Zero Data Retention agreements and reputation.


### Query Privacy from Tinfoil


Our goal is to ensure that Tinfoil cannot see the content of search queries derived from user conversations. Since conversations are already private by virtue of running inside secure enclaves, the challenge is ensuring that Tinfoil also never sees the queries sent to the search provider.


The enclave ensures Tinfoil cannot observe queries through two mechanisms:


- **Isolation** : Existing enclave properties already prevent Tinfoil from inspecting conversation data. Search queries are derived within the same protected environment as the conversations.
- **Encrypted Channels** : The enclave initiates TLS connections directly to the search provider. Tinfoil only sees encrypted traffic between the enclave and the external search API.


### Query Privacy from the Search Provider


Because we are introducing a dependency on a third-party search API, we need to choose a provider that offers both quality and contractual privacy.


There are several search APIs on the market such as Perplexity Sonar, Brave Search API, Google, Tavily, and Exa. Some existed pre-AI (Bing, Google, Brave) while others are purpose-built for AI (Exa, Tavily, Perplexity).


In addition to search quality, our key requirement was **Zero Data Retention** . Most search APIs rely on Google or Bing's indexes behind the scenes, and neither offers ZDR. Only providers with their own independent indexes can guarantee true ZDR. Brave, Exa, and Tavily all maintain their own indexes and offer ZDR agreements.


We chose Exa for its combination of ZDR and high quality search results.


## 2. User Anonymity from Search Provider


Our goal is to ensure the search provider cannot determine the IP address or identity of the user whose conversation initiated a query. The provider should learn only that *some* Tinfoil user triggered a search. The provider receives:


- The search query (functionally required).
- A single API key shared across all Tinfoil users (no per-user identifier).
- The enclave's IP address, which provides no information about the user's IP.


**Figure 2:** All search queries from all Tinfoil users across all models flow through the same search enclave.


We may run a small number of replicas for redundancy, but keep this minimal to maximize the size of the anonymity sets.


### Comparison to OHTTP


[OHTTP](https://en.wikipedia.org/wiki/Oblivious_HTTP) anonymizes requests by routing them through a non-colluding relay, hiding the requester's IP from the destination. Our architecture uses the enclave as a functional relay. Since the enclave and search provider are distinct parties, and we use a shared API key across all users, we achieve equivalent IP anonymity while additionally preventing user identification via API keys. Adding a separate OHTTP relay would increase latency without improving anonymity properties since a non-collusion requirement would still be necessary. In particular, even with OHTTP, timing attacks would be possible.


### The need for non-collusion


As we mentioned before, if we were colluding with our search provider, a timing correlation attack could link specific users to specific queries. Our current user base is small enough that such an attack is viable, even if using additional proxy mechanisms like Tor or OHTTP. While our ZDR agreement with Exa ensures queries aren't logged, it does not protect against a warrant compelling both parties to execute timing attacks.


## 3. Minimal Information Disclosure


It's already the case that "good" web search queries tend to be generic (since specific queries like a full conversation between the user and AI assistant are not going to return any useful information). The model we use as our search agent has furthermore undergone significant[safety training](https://cdn.openai.com/pdf/419b6906-9da6-406c-a19d-1bb078ac7637/oai_gpt-oss_model_card.pdf) to not reveal personal information during tool use. Despite these measures, it is still possible that queries inadvertently contain sensitive information given that they are derived from conversation context.


As an additional safeguard, we run a PII classifier that blocks any search query containing sensitive information. If a query is blocked, we notify the user and explain why.


### Understanding PII in search queries


To get a better understanding of information disclosure through web search, we empirically measured how often generated search queries included personal information from user conversation datasets.


### Methodology


We used the[AIxBlock dataset](https://huggingface.co/datasets/AIxBlock/stimulated-chatbot-conversations-PII-detection-7languages) of customer support conversations (8,976 user turns). For each turn, we prompted our search agent (` gpt-oss-120b` ) to generate a search query, then ran our PII safeguard model (` gpt-oss-safeguard-120b` ) to flag queries containing personal information. We verified the results using Claude Opus 4.5 as a judge, chosen for its[robust prompt injection and safety training](https://www.anthropic.com/research/prompt-injection-defenses) .


Our evaluation code and results are[open source](https://github.com/tinfoilsh/confidential-websearch/tree/main/evals) .


### Results


We found that 22% of user turns triggered web search queries (1,955 queries total). Of these, 3% (55 queries) were flagged by the safeguard as containing PII. Flagged results included vehicle registration numbers, bank account numbers, IBANs, voter IDs, and passport identifiers. Upon evaluating these decisions against the policy, we found 99.7% accuracy. The safeguard model made four false-positive identifications and missed two instances of PII out of the 1,955 queries. Most leaks occurred when users explicitly requested verification of specific data (bank accounts, government IDs, financial account numbers).


We also evaluated our safeguard classifier on datasets containing PII. We took 1,000 samples containing PII from the[AI4Privacy PII Masking dataset](https://huggingface.co/datasets/ai4privacy/pii-masking-400k) , and 1,000 samples from a generic web search dataset ([MS MARCO web search queries](https://microsoft.github.io/msmarco/) ) that has few instances of PII. The classifier achieved 97.4% accuracy. It had perfect precision (legitimate queries are never incorrectly blocked) and a recall of 95%, indicating that the classifier caught the vast majority of PII. The occasional misses were account numbers in unusual formats (e.g., long unformatted numeric sequences) and phone numbers with non-standard delimiters.


## 4. Server-Side Search


One obvious question is why not have clients execute searches directly? In such a setup, the AI model would request the client to execute searches and send results back. While this seems privacy-preserving, it actually *harms* privacy in three critical ways. First, clients connecting directly to search providers reveal their network IP address, enabling identity correlation across searches. Second, search providers require API keys for programmatic access, which uniquely identify users even when using VPNs. In short, client-side search tells the provider *who* is searching, directly correlating user identity with query content. Lastly, ZDR agreements are typically only available on enterprise plans and can be difficult and expensive for individuals to obtain for personal use.


Server-side search solves all three problems. By executing searches within our secure enclave infrastructure, we separate user identity from query content. Specifically, the search provider sees only encrypted traffic from our enclave's IP with a shared API key, creating an anonymity set across all Tinfoil users as we explained above.


Why not execute searches server-side but require client approval for each query? This would preserve user anonymity from the search provider while giving users control over exactly what queries are sent. But it creates other problems. A single prompt can trigger multiple searches, leading to approval fatigue and a bad user experience. Each approval adds a round trip, increasing latency, and it breaks long-running agentic tasks that need to search autonomously.


Fully autonomous server-side search allows users to get the privacy benefits of a shared API key with ZDR, without affecting usability and performance.


## Defense against Prompt Injections


When the model ingests search results, it processes untrusted content from the open web into its context. An attacker could craft a webpage that, when included in search results, contains hidden instructions designed to manipulate the model into leaking sensitive conversation data in subsequent search queries. This is known as a[prompt injection](https://simonwillison.net/series/prompt-injection/) attack.


However, because Exa already returns reputable sources, we don't view prompt injections as a significant risk in our web search system. Nonetheless, mitigating prompt injections continues to be an[active area](https://www.anthropic.com/research/prompt-injection-defenses) of work, and we will continue to evaluate the risk and implement appropriate measures to defend against such attack vectors in the future.


## Conclusion


As AI models become more agentic, they inevitably need to interact with the outside world—we can't keep them locked inside a secure enclave. Web search is the first of many capabilities that will require us to carefully navigate this boundary. Our architecture provides strong guarantees where possible. But as always, we welcome feedback from users who have thought carefully about threat models we may have missed.


### Subscribe for Updates


[RSS Feed](https://tinfoil.sh/feed.xml)


Stay up to date with our latest blog posts and announcements.


[Previous Post](https://tinfoil.sh/blog/2025-12-18-browser-native-verification)[Next Post](https://tinfoil.sh/blog/2026-01-26-private-url-sharing)
