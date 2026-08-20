---
schema_version: "1.0.0"
document_id: "1e79b90ea0492d13879351651c9a4f4b7c247973b1634d95a34c39be4720d424"
company_key: "yc-zep-ai"
company: "Zep AI"
source_id: "yc-zep-ai-rss-0db2565bfdf1"
canonical_url: "https://blog.getzep.com/observations-durable-patterns-from-the-context-graph/"
published_at: "2026-05-21T15:25:50+00:00"
first_seen_at: "2026-07-24T08:02:00.467869+00:00"
fetched_at: "2026-07-28T21:55:53.408599+00:00"
content_hash: "sha256:106d52832012b168d2623efbd12448237f9306caa5a83d21f6492252277149c4"
---

# Observations: Patterns and Insights from the Context Graph

## Key Takeaways


- Observations are a derived context type that captures patterns across many episodes (decisions, constraints, preferences, state transitions, recurring behaviors) that no single fact or entity holds.
- Zep discovers and synthesizes them automatically in the background, encoding each as one retrievable claim over a coherent neighborhood of the graph.
- They close an aggregation gap: on LongMemEval, an Observation that rolled a running count up at ingestion let the agent answer “15 videos” directly instead of counting across raw messages at query time.
- Retrieve them via the new observations search scope or as a Context Template variable (available on Flex Plus and Enterprise).


Today we're announcing[Observations](https://help.getzep.com/observations?ref=blog.getzep.com) , a new[context type](https://blog.getzep.com/zep-context-types/) that captures patterns and insights across your Context Graphs. They surface what no single fact or entity captures: behaviors, decisions, and preferences that emerge across many episodes. Zep discovers and creates Observations automatically. You can then retrieve Observations via the new` observations` graph search scope, or as a Context Template variable. Observations are available now on Flex Plus and Enterprise.


## What Observations are


An Observation is a derived node in the Context Graph that represents a pattern across a coherent neighborhood of entities, relationships, and episodes. The patterns and insights it captures include decisions and commitments, constraints and preferences, state transitions, recurring behaviors, and stable relationships.


Here's an Observation from a real-estate-agent graph. After Jane Doe spent three sessions narrowing down a home purchase, an Observation captured her decision (Maple Street over Oak Avenue, planned offer) as a single retrievable claim:


The Observation captures three sessions' worth of evidence (the viewings, the stated preference, the offer plan) as a single retrievable claim instead of scattered facts.


## Why Observations matter


Zep extracts[facts](https://help.getzep.com/facts?ref=blog.getzep.com) and[entities](https://help.getzep.com/entities?ref=blog.getzep.com) from narrow, local windows of[episodes](https://help.getzep.com/episodes?ref=blog.getzep.com) . Patterns that emerge across sessions, or episodes that are temporally distant, aren't easily captured. Observations surface these patterns and insights from across the Context Graph. The Context Graph is the ideal substrate for this: it encodes them as locally coherent neighborhoods, making them discoverable as a user's history accumulates.


The lift shows up on[LongMemEval](https://arxiv.org/abs/2410.10813?ref=blog.getzep.com) , a benchmark that probes the kinds of aggregation and reasoning questions agents typically struggle with. While evaluating Zep against it, we saw Observations doing exactly what we'd hoped on questions like *"How many Crash Course videos have I watched in the past few weeks?"* The user mentioned individual videos across many sessions but never stated a total. An Observation had rolled the count up to '15' at ingestion, so the agent retrieved the answer directly instead of counting across raw messages at query time.


## How to use Observations


Zep discovers and synthesizes them in the background — no setup required. You can retrieve them in two ways.


### Graph search


Use the new` observations` scope on graph search alongside the existing` edges` ,` nodes` , and` episodes` scopes:


```text
from zep_cloud import Zep


client = Zep(api_key="YOUR_API_KEY")


results = client.graph.search(
user_id="user_123",
query="What does this user typically do when traveling?",
scope="observations",
limit=5,
)
```


### Context Templates


You can also reference Observations in[Context Templates](https://help.getzep.com/context-templates?ref=blog.getzep.com) , so they appear in the output of` thread.get_user_context()` :


```text
client.context.create_context_template(
template_id="customer-support",
template="""# CUSTOMER PROFILE
%{user_summary}


# OBSERVATIONS
# Patterns and insights about this user.
%{observations limit=10}


# FACTS
%{edges limit=10}


# KEY ENTITIES
%{entities limit=5}""",
)
```


Then retrieve the output by passing the template ID:


```text
user_context = client.thread.get_user_context(
thread_id="thread_id",
template_id="customer-support",
)
context_block = user_context.context
```


## Getting started


Read the[Observations docs](https://help.getzep.com/observations?ref=blog.getzep.com) for the full reference. Observations are available now on[Flex Plus and Enterprise](https://www.getzep.com/pricing?ref=blog.getzep.com) .
