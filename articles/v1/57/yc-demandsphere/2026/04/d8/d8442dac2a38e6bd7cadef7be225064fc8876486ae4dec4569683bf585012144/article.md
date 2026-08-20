---
schema_version: "1.0.0"
document_id: "d8442dac2a38e6bd7cadef7be225064fc8876486ae4dec4569683bf585012144"
company_key: "yc-demandsphere"
company: "DemandSphere"
source_id: "yc-demandsphere-rss-b433e6d35ba4"
canonical_url: "https://www.demandsphere.com/blog/prompt-volume-and-prompt-research/"
published_at: "2026-04-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:28.972599+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:69510562c72444f858e351aed7c99eab3dcd1ed6eb4e34d20bea571ed01f7fb4"
---

# Prompt Volume and Prompt Research: AI search intelligence for every prompt

Day 3 of Release Week. Today we are announcing two new tools:[Prompt Research](https://www.demandsphere.com/platform/demandmetrics-genai/prompt-research/) and[Prompt Volume](https://www.demandsphere.com/platform/demandmetrics-genai/prompt-volume/) .


We take what may be a unique approach to the question of Prompt Volume. We call it “let’s be honest with people.” There is a lot of snake oil around this topic, but I’ll come back to that below.


Let’s talk about Prompt Research first.


## Prompt Research: the hard problem of prompt discovery


Prompt research, from a workflow perspective, is not simple. It is not a matter of searching on a topic and getting generic suggestions.


We live in a world of PAAs, query fan-outs, and other forms of synthetic expansion. You have to find a grounded way to tie it back to some idea of actual intent and customer demand.


It is also quite a recursive process. Even a simple search can uncover hundreds or thousands of prompts to consider and you need a way to filter for relevance and semantic relationships.


And then you need to handle inputs from domains, single topics, and bulk import from external sources - CSV files, Google Search Console, and even streams from other platforms such as Zendesk, user forums, Reddit, and more.


This is the workflow system we have built for[Prompt Research](https://www.demandsphere.com/platform/demandmetrics-genai/prompt-research/) .


Here is what it looks like for “used car for sale”:


1.


**People Also Ask extraction** - real questions from Google’s PAA boxes


2.


**Synthetic generation** - AI-generated natural language prompts spanning different intents


3.


**Qforia fan-out** - systematically varied prompts across seven Google-patent query types (comparative, entity expanded, implicit, personalized, reformulation, related, step by step)


4.


**Query expansion** - related keywords and long-tail variations


Every generated prompt is then enriched with Google search volume, AI search volume across nine engines, confidence scoring, semantic clustering, entity extraction, and content format recommendations.


We are releasing Prompt Research in Preview mode today. It will go into Alpha in May, with General Availability coming over the summer. If you are at SEO Week, you can come check it out at our booth.


Research:


used car for sale


PROMPTS


2,104


UNIQUE INTENTS


419


TOTAL SV


66.1M


TOTAL AI SV


38.7M


Prompt


Source


SV


AI SV


Intent


carfax near me


Expansion


1,000,000


673,100


Navigational


used cars


Seed


673,000


393,796


Commercial


car dealerships near me


Expansion


673,000


293,196


Local


car lots near me


Expansion


135,000


83,016


Local


auto dealers near me


Expansion


73,000


43,516


Local


### Fan-out types and AI volume trends


Filter by fan-out type to see how different query variations perform. The AI Search Volume Trend chart breaks down monthly volume by engine so you can see which platforms are growing:


Fan-out:


Comparative


Entity Expanded


Implicit


Personalized


Recent


Reformulation


Related


Step By Step


TOTAL SV


44,584,490


TOTAL AI SV


26,082,802


AI SEARCH VOLUME TREND


Apr


May


Jun


Jul


Aug


Sep


Oct


Nov


Dec


Jan


Feb


Mar


Copilot


ChatGPT


Gemini


Perplexity


Meta


DeepSeek


Grok


### Analytics


The Analytics tab gives you four views: fan-out type distribution, recommended content formats, entity frequency, and entity co-occurrence networks.


FAN-OUT TYPE


Entity Expanded


1,859


88%


Personalized


202


10%


Comparative


28


1%


Reformulation


8


0%


RECOMMENDED FORMAT


Product Page


1,501


71%


List


432


21%


Table


140


7%


Step By Step


21


1%


TOP ENTITIES


used cars


cars sale


sale near


auto sales


cars under


For “used car for sale,” 71% of prompts map to product pages and the top entities are “used cars,” “cars sale,” and “sale near.” Entity co-occurrence networks show how these concepts relate to each other across the prompt landscape.


### Semantic Clusters


Interactive 3D semantic clusters built with Voyage AI embeddings. Each point is a prompt, positioned by meaning - prompts that are semantically similar appear close together. Filter by source, cluster, or confidence to explore the topic space:


Semantic Clusters


All


Expansion


Seed


Synthetic


Cluster


2,104 points


## Prompt Volume: let’s be honest about search volume


Now let’s return to the topic of Prompt Volume, which is the other tool we are announcing today.


The Prompt Research workflow tools above are built on top of our Prompt Volume engine. And I want to be transparent about how it works, because there is a lot of misleading data in this space.


### The problem with clickstream-based “prompt volume”


When we see what is currently being passed off for Prompt Volume, it has many issues. Despite the sexy labels, it is just clickstream data.


Clickstream data is great and useful for lots of use cases but not, in our opinion, for trying to estimate prompt volume. The panel sizes are too small - maybe 10-20M people, max. The data is noisy. A lot of it is actually just garbage generated by other LLMs.


You do not want to be making decisions off of this data.


### Our approach: grounded in Google Search Volume


Google has more data than anybody on the planet about what people are looking for. You are not going to find a more grounded source for anything related to topic volume than Google Search Volume.


What we have built with[Prompt Volume](https://www.demandsphere.com/platform/demandmetrics-genai/prompt-volume/) is a resolution process from any string of text back to the core topic, and we run search volume for that. Then we track the overall traffic percentages from the various AI platforms and assign a weighting so you have some idea of what the impact could be.


Here is what that looks like in practice. Take a conversational prompt like “used car for sale near me with no accidents and one owner.” Prompt Volume resolves it to the canonical keyword “used cars for sale near me” and returns volume across every engine:


PROMPT


used car for sale near me with no accidents and one owner


Synthetic


CANONICAL KEYWORD


used cars for sale near me


SV


135,000


AI SV


78,976


Engine SV


Keyword Map


Google


135,000


ChatGPT


28,350


Copilot


18,225


Gemini


16,200


Perplexity


5,063


Meta


4,050


DeepSeek


3,038


Claude


2,430


Grok


1,620


Total AI SV: **78,976**


Recommended format


list


Fan-out


personalized


This data is directional. Take the time to explain how this data works to your stakeholders!


Both tools are in Preview now and will go into Alpha in May, with General Availability coming over the summer. Contact your account manager for access.


[Learn more about Prompt Volume](https://www.demandsphere.com/platform/demandmetrics-genai/prompt-volume/)[Learn more about Prompt Research](https://www.demandsphere.com/platform/demandmetrics-genai/prompt-research/)
