---
schema_version: "1.0.0"
document_id: "70acc8ada56a846697d724866a7ce453c262a1f598ef4ce0bbbf0f3d04d110e3"
company_key: "yc-reprompt"
company: "Reprompt"
source_id: "yc-reprompt-news-import-f6b2fbe9777c"
canonical_url: "https://repromptai.com/blog/deep-research-places-benchmark"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-23T23:37:40.595312+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:b3b90576b060f4a115210d17241dc092dfd685f2d4af7aeffc7b1a6a4612f06c"
---

# Reprompt outperforms OpenAI, Claude, Gemini on hard places research tasks

Reprompt builds Deep Research Agents for business and places data. A common task is to check for the basic details of a place: is it still open, what are its hours, what's the phone number, what's the website.


Our Agent harness is hand-built for the task and has over 40 tools at its disposal. But does it actually outperform frontier models from the AI labs, which also have tools and web access? Yes.


To tune our agent, we constantly benchmark it against an updated dataset of hand-labeled places across the world. To benchmark against the AI labs, we sampled 158 real places across Southeast Asia, Europe, and North America. Five systems, same inputs, same JSON schema, same automatic scorer, human-verified answer key:


- **OpenAI GPT-5.5** with native web search
- **OpenAI Deep Research**
- **Gemini 3.5 Flash** with Google Search
- **Claude Opus 4.8** with web search
- **Reprompt** (our agent with its harness and tools)


We scored against the fields that places teams care about most: permanent closures and opening hours.


To make it a comparable benchmark, the labs were given our definition and schema for opening hours and closure. The only thing that changed was the agent harness and underlying LLM models.


## Permanent closures


Reporting precision on whether a place is open or closed is a diluted metric- most places are open so most models will score above 95%. Instead we look at recall on closures: How many places were correctly identified as closed? From a dataset of over 800, we sampled 39 places that were human-confirmed to be permanently closed.


Reprompt caught 36 of 39 closures, a 92% recall.


System Closures caught Recall


**Reprompt** **36 / 39** **92%**


OpenAI Deep Research (o3) 25 / 39 64%


Gemini 3.5 Flash 25 / 39 64%


OpenAI GPT-5.5 23 / 39 59%


Claude Opus 4.8 17 / 39 44%


o3-deep-research is an interesting case. It spent a median of five minutes and ~75k tokens per place, and it still called 14 of the 39 closed businesses open. Not "unsure." Open.


GPT 5.5 missed 41% of closures, Gemini 3.5 Flash missed 43%, and Claude Opus 4.8 missed 56%.


For closures, the most common failure point is stopping early. The deep research models will find a plausible looking web result for a place and call it open instead of continuing to research. We know from experience that it only takes one credible closure source to render a place` permanently closed`


For example, Razon's by Glenn in Ayala Malls closed, but it's still in a mall directory. That case fooled o3, GPT, Gemini, and Claude. GPT-5.5 said Cerería Ortega, a 131-year-old candle shop in Madrid, was open based on historical records; it closed in December 2024 and the closure was covered in the Madrid press.


The reverse failure matters just as much: flagging live businesses as dead. Across the 137 truly open places our agent produced exactly one false "closed" — a McDonald's in Tacloban that was missing from the chain's JavaScript-rendered store locator. The only way to truly know a place is open is to call or visit, but deep web research is the next best thing.


## Opening hours


Hours show a different issue: Fill rate is high across every system. For 80–92% of places, they return hours. The question is how often those hours are right.


We scored 90 places with human-verified published hours. A schedule counts as correct if it matches the verified one on every day, within 15 minutes.


System Answer rate Error rate when answered


**Reprompt** 88% **16%**


OpenAI Deep Research (o3) 92% 29%


OpenAI GPT-5.5 87% 21%


Gemini 3.5 Flash 88% 28%


Claude Opus 4.8 80% 32%


o3 answers the most and verifies the least: wrong on 29% of the schedules it returns. Our agent is wrong on 16% which is the lowest, but still not a number we're satisfied with, because hours are genuinely hard: split shifts, seasonal schedules, a website that disagrees with the social profile it links to.


### A note on cost


While cost and speed weren't a focus of this test, they're hard to ignore at global places scale (200M+ places). Reprompt is 5-10x cheaper than AI labs both due to fewer tokens spent, and cheaper search and page calls. Web search in particular is relatively expensive when used with the AI labs' models. Gemini Search Grounding costs $35/1000 requests and GPT's web search costs $10/1000 searches. ChatGPT Deep Research averaged over $2 per query and was overkill for this task.


## How we beat the AI lab models


Given the same prompt and similar-class LLM model, we find that the Reprompt harness and toolset outperform the AI labs on difficult places tasks like opening hours and permanent closures. Reprompt especially shines in Southeast Asia and Europe where key information is more likely to be shared on Facebook, Instagram, or WhatsApp, and where aggregators are particularly outdated. And from the strict guardrails we've set, Reprompt is far less likely to mistake one entire place for another.


### Top error categories show how the general agents think:


**Wrong entity** The closures that fooled the most systems were all outlets of healthy chains. The brand is alive and generating fresh which seems to trick the general agents into thinking the branch is too.


**Closure sources** When a closure had press coverage, the general agents caught most of them. But when closure was cited in a single source such as a branch's listing webpage, the general agents were far more likely to mark it "open." We know from experience that it only takes one credible closure source to render a place` permanently closed.`


**Not citing enough sources** In our source audit of 78 hours answers, GPT-5.5 cited one source or none for **71%** of the schedules it returned; Reprompt answered from a single source on fewer than 8%, and typically cross-referenced three or more (official site, social profiles, aggregators as tie-breakers).


**Hallucinations** The benchmark includes seven open places that verifiably publish no hours anywhere; the correct answer is null. Scored strictly that would drop GPT-5.5's hours precision to 70%.


We find that generic agents, especially OpenAI's Deep Research are conditioned to produce an expansive, well-cited report. Intuitively, the labs want them to show something for all the tokens they've spent- "place not found" is an incredibly unsatisfying result. However, they often over-research to the point of pulling information about the wrong place or even hallucinating hours.


### Outlook


We built Reprompt specifically for places tasks, and we've seen over the past 3 years where general agents fall short. We're pleased but not surprised that our harness and toolset outperform the AI labs.


By design the Reprompt agent goes deeper into the web to validate information, ranks sources more effectively, and is more conservative and more accurate in its final answers. In short, we built Reprompt to be an exceptional places researcher and we're seeing that pay off in this test.
