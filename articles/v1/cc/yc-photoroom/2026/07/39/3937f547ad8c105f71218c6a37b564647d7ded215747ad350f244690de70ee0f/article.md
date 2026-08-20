---
schema_version: "1.0.0"
document_id: "3937f547ad8c105f71218c6a37b564647d7ded215747ad350f244690de70ee0f"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/ai-qualitative-analysis"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:9f016b382d7517e3b18e53253febea466907400b78ab3c67256c481b516ca661"
---

# Working smarter with AI-led qualitative analysis at Photoroom

At Photoroom, our small and mighty user research team of 2 is focused on empowering stakeholders to do their own research, while also conducting more in-depth strategic user research projects in parallel.


At a high level, our goal is to help every product decision at Photoroom start (and end) with real user understanding. Our team and our stakeholders work together to investigate new opportunities, pressure-test early concepts, understand user needs and behaviors, and evaluate shipped features. We then turn that learning into reliable, shareable knowledge.


As our product footprint grew and the volume of qualitative data like user interviews ballooned, we found ourselves wrestling with two practical problems: *scale* (hundreds of hours of qualitative data) and *speed* (stakeholders needing answers yesterday).


Generative AI offered a partial solution, but only if we could weave it into the craft without losing nuance. What follows is the story of how we did that—what worked, what didn’t, and why the human element still matters most.


## Building an AI-assisted practice


### Finding the right mix of tools


We began by aligning on a single evidence store: every interview recording and transcript lives in Dovetail. On top of that, we layered two home-grown helpers.


-


**Mining User Interviews (MUI).** MUI is a lightweight interface that lets anyone—from PMs to engineers—query our entire transcript library in plain language. Ask, “Where do Etsy sellers stumble during onboarding?” and it returns time-stamped quotes plus links to the raw video. It’s quick context without waiting for a slide deck.


-


**Interview-Guide Generator.** Colleagues often run their own discovery calls, so we built a script that drafts a worksheet of open-ended, non-leading questions. The user enters the audience and learning goal; the generator returns a structured guide, complete with follow-up probes and topic transitions. The result is more consistent interviews and fewer leading prompts.


Any stakeholder at Photoroom can use MUI to get quick insights about our users, the users of competitors, or target audience in general.


On the user research team, we use MUI to do qualitative analysis faster. Specifically, we use research questions as the basis for prompts that help us locate data points in a set of interviews or usability sessions. Then, there is the human part: we use Miro to affinity diagram the data points that we locate with MUI and group data that becomes insights.


Here’s a step-by-step look at how it works:


Step Manual work AI assistance Benefit


Collect data Upload interviews to Dovetail, our qualitative analysis tool — Central source of truth are the actual transcripts


Locate quotes Highlight and tag Ask Dust (our internal GPT wrapper) to surface quotes aligned to each research question Rapid first pass


Affinity mapping Cluster evidence in Miro — Human pattern-finding


Draft insights Write and refine Have AI play devil’s advocate to check for gaps or bias Adds perspective


Share findings Create slides or documents Generate concise summaries for stakeholders Quicker hand-off


AI is just as good, if not better, at locating quotes about a specific topic. It’s not only an efficiency add, but because it thinks differently and broader than a human, it often locates examples that we may not have thought of on our own if we were tagging data manually.


Because of our institutional knowledge, sentience, and ability to understand our users on a human level, we’re better at taking these data points and using our process to understand our target audience. However, asking AI to play devil’s advocate helps us keep our various blind spots and biases in check.


Human-centered design is a value at Photoroom, and by using AI to locate relevant data points and serve as a thought partner, while using our brains to group and form insights, we save a ton of time while also utilizing our knowledge and empathy to build knowledge about users.


### Working with prompts - lessons learned


Good prompt craft turned out to be the missing piece when it came to making use of AI for finding relevant data points. We moved from broad requests (“Summarize these five interviews”) to precise, testable queries:


- Ask **one** clear question at a time.


- Rephrase the same question twice and compare the outputs.


- Don’t assume that it’ll be exhaustive. Ask it to find more examples until you’ve truly exhausted them.


- Always request citations so we can trace every claim back to a speaker and timestamp.


- Provide only the context that truly matters; over-stuffed prompts make the model latch onto irrelevant details.


## What we’ve learned (and what comes next)


### A shift in perspective


Two discoveries reshaped the way we think about research. First, AI can shoulder the repetitive labeling and retrieval tasks, but synthesis—the leap from “what they said” to “what it means”—remains a fundamentally human act. This is because truly excellent work in this realm requires empathy, institutional knowledge, and a true will to innovate.


Second, faster turnaround changes perception. Shipping a robust study faster, over time, replaces the familiar notion that “research is slow” with the notion that “research is an enabler.”


### Looking ahead


As language models grow more capable, I expect researchers to spend less time cleaning data and more time co-creating solutions with designers and product managers. Stakeholders will self-serve simple factual queries (“How many users mention price confusion?”) and call us in for deeper why-based work. In short, we’ll have the privilege of both doing research but also having more time to spend in the solution space with our colleagues.


(This article was adapted from[my conversation with Hannah Clark](https://open.spotify.com/show/6jacN1tZEAoaobBmweOUXg?si=59ae86e18dbb432b&nd=1&dlsi=98b29b70c7e3451b) on The Product Manager podcast.)
