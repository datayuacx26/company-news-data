---
schema_version: "1.0.0"
document_id: "62c05f15734126658ecf1044bd56384ef22169855f61a755d974f8902ed928f5"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/code-mode-data-assistant"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T02:51:36.625681+00:00"
fetched_at: "2026-08-14T02:51:38.375619+00:00"
content_hash: "sha256:eb839b136a0481c2ee57926eb57390d23cdd4f27f313692d6f6a36b01997822e"
---

# AI Can Build. Can It Know What Worked?

Every product runs the same loop: build it, ship it, measure what happened, then use what you learn to build the next thing.


The first half of that loop has become increasingly automated. CI/CD ships code. Feature flags control rollouts. AI can write, test, and iterate alongside developers. But turning what happens next into the right next move still depends heavily on human judgment.


That judgment is the next thing to automate.


Knowing why a metric moved, whether it matters, and what to do next requires more than access to data. It requires two things: the ability to reason through the context, and confidence that the underlying data is trustworthy. As more decisions shift from humans to agents, both become increasingly important. An agent that reasons well from bad context (or bad data) can still confidently make the wrong call.


In Anthropic's[June 2026 Economic Index report](https://www.anthropic.com/research/economic-index-june-2026-report) , researchers asked Claude users what they believed AI would never be able to do. The most common answers pointed to the same thing: judgment, contextual awareness, and situational reasoning—knowing what a situation calls for.


We see that as the next problem to solve: helping teams make better decisions with better context and more trustworthy signals.


Today, we're introducing two AI capabilities built for that problem: **Code Mode** and **Data Assistant Agent** .


Code Mode helps teams reason more deeply about what their data shows: what changed, why it matters, who's affected, and what to do next. Data Assistant Agent helps make the data behind that reasoning easier to trust by identifying and prioritizing data-quality issues.


##
Answer the questions behind your metrics


Most product teams can see when a metric moves. The harder questions come next: Why did it move? Which users drove the change? What behaviors predict what happens next? Did the thing we shipped actually make a difference?


Answering those questions often means exporting data, writing custom SQL or Python, or waiting for help from a data scientist. Code Mode brings that work directly into Amplitude.


It runs SQL and Python against the behavioral data already in Amplitude, letting teams investigate questions that go beyond a standard chart without leaving their workflow.


Teams are already using Code Mode to uncover hidden segments and behavioral patterns, compare journeys across cohorts, build custom visualizations, and catch discrepancies between systems that neither flags on its own.


That means teams can move beyond questions like "How many people used this feature?" and start asking:


- Which behaviors predict long-term adoption?
- Where do different user segments get stuck?
- Did this launch actually improve retention?
- Which users are most likely to benefit from an intervention?


And because every query and validation step is logged along the way, the analysis can be inspected, challenged, and rerun. Teams don't have to take an AI-generated conclusion on faith.


Available to all Free, Plus, and Growth customers, Code Mode brings deeper analysis into Amplitude so teams can move from signal to explanation to opportunity without exporting data or waiting on a separate analysis cycle.


## Build a trusted data foundation


Deeper reasoning only works if the data underneath it can be understood and trusted.


If event names are unclear, definitions are missing, similar behaviors are tracked in multiple ways, or important context lives only in someone's head, humans and AI can interpret the same signals differently. As more analysis shifts to agents, those ambiguities become more consequential: AI can only reason from the context your data provides.


And it isn't hypothetical. In Hex's[2026 State of Data Teams survey](http://hex.tech/state-of-data-teams) , data quality and lack of trust was the biggest barrier data leaders cited to scaling AI—named nearly twice as often as cost, skills, or tooling combined.


Data Assistant Agent expands[Data Assistant](https://amplitude.com/blog/Amplitude-AI-data-assistant-data-governance) with new intelligence, a broader set of prioritized data-quality recommendations, and an interactive experience for investigating and acting on those recommendations in chat.


It reviews taxonomy, metadata, usage patterns, and project context to identify unclear, duplicated, or undefined events and properties. Then it explains why those issues matter and recommends improvements that make Amplitude data more trustworthy for both people and agents.


Teams can:


- Run an AI-specific audit
- Investigate taxonomy issues
- Ask for prioritization
- Ask for explanations
- Draft metadata improvements


Now available to all Plus, Growth, and Enterprise customers with AI features enabled, Data Assistant Agent ensures teams adopting AI-powered analytics have reliable data for both humans and agents to make the right decisions about your product.


##
**Close the loop from measurement to action**


AI has accelerated how products get built and shipped. Now the rest of the product development loop needs to catch up.


Code Mode and Data Assistant Agent tackle the two things AI needs to make better decisions from product data: the ability to reason deeply about what the signals mean, and a data foundation reliable enough to reason from. One helps uncover what happened, why it happened, and what to do next. The other helps identify and improve the data-quality issues that could undermine those conclusions.


Together, they turn measurement from the end of the process into what it was always supposed to be: the beginning of what you build next.
