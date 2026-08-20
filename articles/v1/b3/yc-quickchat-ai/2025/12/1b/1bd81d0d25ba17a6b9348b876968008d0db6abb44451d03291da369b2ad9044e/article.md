---
schema_version: "1.0.0"
document_id: "1bd81d0d25ba17a6b9348b876968008d0db6abb44451d03291da369b2ad9044e"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/ilya-sutskever-interview"
published_at: "2025-12-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:3fdec1040d2ab72fded511742a2d2d5d311ad46c59803e6c0b630bd714fe55c5"
---

# The Ilya Sutskever interview - my key takeaways

I finally got round to listening to[the Ilya Sutskever interview](https://www.youtube.com/watch?v=aR20FWCCjAs) . A few points caught my attention. I’m listing them below starting from the least controversial.


## #1 RL value functions


In today’s Reinforcement Learning, agents get **a single scalar reward** at the very end of the task they were given. In RLHF which we use for fine-tuning LLMs it’s[arguably](https://x.com/karpathy/status/1821277264996352246) even worse than that.


Ilya, like many others, see that as a clear sign that RL could work so much better if we figure out how to make good use of value functions.


What is a value function? It’s being able to tell **how well I’m doing on a task as I’m still doing it** .


Imagine an LLM being fine-tuned for a long-ranging agentic task of refactoring a large codebase. If the full task takes 100 steps, it’s very non-trivial to be able to tell 20 steps into the task how well the agent is doing. And for that technique to make sense to be weaved into RL setups it would need to be extremely fast, cheap and efficient.


Ilya also mentions the *“human built-in value function”* which he loosely ties to emotions that evolution embedded into our brains. Amazingly, he says, emotions that evolution optimised for the world of 10,000 - 100,000 years ago still serve us well today (except for some notable failure cases like addiction).


## #2 Taste in research


Ilya, when asked about where he gets his taste in research (intuition about what ideas to pursue), answers that ultimately all deep learning is **inspired by the human brain** . If something works in the human brain it should work in computers too.


He describes the frustrating moments every researcher faces when they’re not sure if the results are bad because it’s not the right approach or they should keep on debugging. To him the human brain was the fundamental inspiration and proof that if it works *in vivo* it should also work *in silico* .


## #3 Scaling versus research


The age of scaling has ended. Now is the age of research. Not much more is to be gained from just more data and more compute. New ideas are needed. Or as Ilya put it *“there are more companies than ideas”* .


To me the visible shift to more conceptual and theoretical thinking in AI is exciting! Clearly, current approaches will not take us all the way to AGI and many more qualitative breakthroughs are needed.


Although next week, just like any other week this year, we will most likely see yet another wave of similar LLMs pushing the same benchmarks even further. In the interview Ilya did mention researchers **leaking benchmarks into models** as part of the reason why we don’t see the progress in benchmark manifesting itself in the real world.


## #4 Continual learning


Next level superintelligent agents will not come with as much built-in pretraining knowledge as today’s LLMs. Instead, pretraining will be less about gaining knowledge and more about gaining the skill to learn new things. Ilya’s agents will be less capable from day 1 but **learn quickly on the job** .


## #5 Forbidden ideas


Ilya mentioned there are some ideas in the field of Artificial Intelligence that **“should not be talked about”** . As to what those are he only briefly mentioned that brain neurons might be *doing more than we think* .


That brings echoes of Roger Penrose’s theory that thinking is tied to consciousness which arises through[quantum processes we don’t fully understand yet](https://en.wikipedia.org/wiki/Orchestrated_objective_reduction) .
