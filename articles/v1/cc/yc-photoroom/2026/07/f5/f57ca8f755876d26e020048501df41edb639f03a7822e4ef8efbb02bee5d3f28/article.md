---
schema_version: "1.0.0"
document_id: "f57ca8f755876d26e020048501df41edb639f03a7822e4ef8efbb02bee5d3f28"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/why-ai-for-bi-fails"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:3af8fa45025add8d970716eb2193c5978d578ade3e13b8c3571017f63cf11a63"
---

# Why AI-Powered Business Intelligence Fails (And How to Fix It)

***Why AI for BI fails:** AI analytics tools fail not because they write bad code, but because business data is semantically ambiguous. A single question like "What was our revenue last month?" can have 20+ valid interpretations depending on how metrics are defined. Without semantic discipline in the data layer, LLMs are forced to guess, and they often guess wrong.*


## The promise vs the reality


Everyone wants AI-powered analytics. The promise: ask a question in natural language, get an answer in seconds. The proof of concept works beautifully. Give any LLM a CSV, ask a question, and it answers. Magic.


Then reality catches up: the results aren't reliable. And the problem isn't the AI model. LLMs write perfect SQL. The problem is the precision and modeling of the data.


## The AI Context Gap


Questions are precise, at least in the head of the person asking. But data rarely is.
Let’s take an example, and ask the question: “What was our revenue last month?”


The question seems simple, straightforward, and calls for an unambiguous answer.
However, the data that holds the answer contains a lot of subtleties, and ambiguities.


For instance, in order to correctly interpret the question, the AI needs to know: do we want gross or net revenue? Booked or invoiced? Should we include or exclude refunds? And when we say last month, is it last complete month, or last 30 days… ?


Every ambiguity is a branch in a decision tree.
The more ambiguity you have, the more guesses the AI will have to make.
And the bigger the tree, the less likely the LLM is to pick the right path.


In our example, we have 2*2*2*2 = 16 possibilities for a basic question.
Real life is obviously much more complicated.
Every ambiguity divides the likelihood for the LLM to give the right answer.


The challenge is to reduce ambiguity, by providing clear data, with a good modeling and a good documentation. If we can’t provide this to the AI, it will have to guess.


## Why this matters now


AI in BI is moving from "nice to have" to "expected". Everybody has tried playing with LLMs, sometimes plugged MCPs and asked questions. Everybody was amazed by the speed, precision, and interpretation capability of the LLM. AI in BI is a must.


The problem is that most BI environments are semantically ambiguous (like in our example above). And ambiguity is where AI fails.


## The real problem


Humans infer meaning from context. They know the decision history. They carry tribal knowledge that's never been documented.


Machines don’t have all of this. BI + AI breaks when semantics are implicit.
Anytime the data isn’t perfectly matching a user’s question, there is a risk that the AI will have to guess what the user means, and that it will guess wrong.


## Our thesis: AI reliability is downstream of semantic discipline


Semantic discipline is the non-negotiable foundation for reliable AI. It has 3 components:


1.


**Design** : Intentional semantic architecture →[Article 2](https://www.photoroom.com/inside-photoroom/ai-for-bi-write-definitions-as-code)


2.


**Protection** : Guardrails before production → Article 3 (coming soon)


3.


**Validation** : Continuous meaning checks → Article 4 (coming soon)


At Photoroom, we treat semantic clarity as an engineering problem. Reliable AI analytics cannot be solved by prompts or fine-tuning. It is downstream of data modeling quality.


Structure scales. Intuition doesn't.


## Our results


Today, everybody at Photoroom can ask a BI question in natural language. We use Omni as our BI tool, so people can ask questions to the Omni AI assistant. But they can also ask directly in Claude using an MCP, or in Slack.


We are happy with the results.


In the next 3 articles, I will show what we have set up in the data codebase to make sure that we have reliable results, starting with[what "machine-readable meaning" actually looks like, and how to implement it](https://www.photoroom.com/inside-photoroom/ai-for-bi-write-definitions-as-code) .
