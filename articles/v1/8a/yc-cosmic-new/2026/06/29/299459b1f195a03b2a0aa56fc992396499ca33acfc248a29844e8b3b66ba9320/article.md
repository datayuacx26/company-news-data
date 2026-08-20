---
schema_version: "1.0.0"
document_id: "299459b1f195a03b2a0aa56fc992396499ca33acfc248a29844e8b3b66ba9320"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-cssquake-vpn-bans-atproto-architecture"
published_at: "2026-06-20T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:d833c8dd1c3f072b4b912ba6b35a65a767b374cf8e99e7d32fae02a291588ca6"
---

# Cosmic Rundown: CSSQuake, VPN Bans, and ATProto Architecture

## CSSQuake: A Technical Feat Worth Examining


[CSSQuake](https://cssquake.com/) is exactly what it sounds like: a recreation of the classic first-person shooter using only CSS. No JavaScript. No canvas. Just CSS.


The[Hacker News discussion](https://news.ycombinator.com/item?id=48608223) dives into the technical implementation. The project demonstrates CSS capabilities that most developers never touch: 3D transforms, complex animations, and state management through checkbox hacks. Whether this is practical is beside the point. It shows what the platform can do when pushed to its limits.


For content teams, the takeaway is simpler: CSS continues to evolve in ways that reduce JavaScript dependencies. That matters for performance, accessibility, and maintainability.


## UK VPN Restrictions: What Developers Should Watch


The UK government is[exploring VPN restrictions](https://www.birminghammail.co.uk/news/midlands-news/vpn-ban-update-uk-households-34141063) as part of broader age-verification efforts. The proposal ties into existing Online Safety Act requirements.


The[discussion](https://news.ycombinator.com/item?id=48609385) raises practical concerns: enforcement complexity, impact on remote workers, and the precedent for internet infrastructure regulation. For teams building products with UK users, this is worth monitoring. Age-gating requirements are expanding globally, and how the UK implements enforcement will influence other jurisdictions.


## ATProto: No Instances, Just Data


Dan Abramov published["There are no instances in ATProto"](https://overreacted.io/there-are-no-instances-in-atproto/) , explaining the architectural differences between Bluesky's protocol and ActivityPub (Mastodon's foundation).


The core insight: ATProto separates identity from hosting. Your data is portable by default. You can move between service providers without losing your identity, followers, or content history. The[Hacker News thread](https://news.ycombinator.com/item?id=48599515) includes detailed technical discussion on the tradeoffs.


For anyone building social features or thinking about content portability, this architecture is worth understanding. The patterns ATProto uses for decentralized identity and data ownership are applicable beyond social media.


## GLM-5.2 Outperforms GPT-5.5 on Hallucination Benchmarks


A[benchmark comparison](https://arrowtsx.dev/bigger-models/) shows the MIT-licensed GLM-5.2 hallucinates three times less frequently than GPT-5.5 on factual accuracy tests. The[discussion](https://news.ycombinator.com/item?id=48600167) debates methodology and what this means for open-weight model adoption.


The practical implication: open-source models continue closing the gap with proprietary alternatives. For teams evaluating AI integration, the licensing and deployment flexibility of open models becomes more attractive as capability differences narrow.


## Quick Hits


**Storing a website in a favicon** : Tim Wehrle[documented how](https://www.timwehrle.de/blog/i-stored-a-website-in-a-favicon/) he embedded an entire website into a favicon file. The[thread](https://news.ycombinator.com/item?id=48606619) explores the technique and its creative applications.


**LLMs are complicated now** : Ian Barber's[post](https://ianbarber.blog/2026/06/19/llms-are-complicated-now/) covers the growing complexity of LLM deployment, from context management to tool use to multi-modal inputs. The[discussion](https://news.ycombinator.com/item?id=48605355) adds perspective from practitioners.


**Cloudflare temporary accounts for AI agents** : Cloudflare[launched temporary accounts](https://blog.cloudflare.com/temporary-accounts/) designed specifically for AI agent workflows. The[conversation](https://news.ycombinator.com/item?id=48608394) explores use cases and security implications.


**John Jumper moves from DeepMind to Anthropic** : The AlphaFold co-lead[is joining Anthropic](https://www.reuters.com/technology/us-scientist-john-jumper-leave-google-deepmind-anthropic-2026-06-19/) , signaling continued talent movement in the AI research space.


**The European Social Stack** : A new initiative at[european.social](https://european.social/) aims to build European alternatives to major tech platforms. The[discussion](https://news.ycombinator.com/item?id=48609421) debates feasibility and scope.


## What This Means for Content Teams


The ATProto architecture discussion is the most relevant signal for content infrastructure planning. The separation of identity from hosting, and the emphasis on data portability, reflects where content management is heading. Users expect to own their content. Platforms that make content portable build more trust than those that lock it in.


Cosmic's approach aligns with this: your content lives in a structured API, accessible from any frontend, portable by design. Whether you are building for Bluesky, traditional web, or whatever comes next, your content layer stays consistent.


The GLM benchmark results matter too. If you are integrating AI into content workflows, open models with permissive licensing reduce vendor lock-in. The accuracy improvements make them viable for production use cases that previously required proprietary models.


---


Building content infrastructure that needs to stay portable and API-first?[Cosmic's headless CMS](https://www.cosmicjs.com/) gives you structured content with a fast REST API. Your content stays queryable and accessible regardless of which frontend frameworks or AI tools you adopt.


[Start free](https://app.cosmicjs.com/signup) or[talk to Tony](https://calendly.com/tonyspiro/cosmic-intro) about your architecture.
