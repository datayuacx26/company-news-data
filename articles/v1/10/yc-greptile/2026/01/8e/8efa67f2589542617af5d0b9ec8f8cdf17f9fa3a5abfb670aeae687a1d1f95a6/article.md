---
schema_version: "1.0.0"
document_id: "8efa67f2589542617af5d0b9ec8f8cdf17f9fa3a5abfb670aeae687a1d1f95a6"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/ai-code-review-bubble"
published_at: "2026-01-24T00:00:00+00:00"
first_seen_at: "2026-07-21T22:07:23.426026+00:00"
fetched_at: "2026-07-28T22:23:12.089184+00:00"
content_hash: "sha256:2ccee712131742bb8f7b8a2712c5b6253dfbb6a21358e9a7d8d80e62e3e0c705"
---

# There is an AI Code Review Bubble

Today, we're in the[hard seltzer](https://www.youtube.com/watch?v=iBpgqRbEFVg) era of AI code review: everybody's doing them. OpenAI, Anthropic, Cursor, Augment, now Cognition, and even Linear. Of course, there's also the "White Claws" of code review: pure-play code review agents like Greptile (that's us!), CodeRabbit, Macroscope, and a litter of fledgling YC startups. Then there are the adjacent Budweisers of this world:


Amazingly, these two were announced practically within 24 hours of each other.


As the proprietors of an, er, AI code review tool suddenly beset by an avalanche of competition, we're asking ourselves: what makes us different?


**How does one differentiate?**


Based on our benchmarks, we are uniquely good at catching bugs. However, if all company blogs are to be trusted, this is something we have in common with every other AI code review product. One just has to try a few, and pick the one that feels the best.


Unfortunately, code review performance is ephemeral and subjective, and is ultimately not an interesting way to discern the agents before trying them. It's useless for me to try to convince you that we're the best. You should just try us and make up your own mind.


Instead of telling you how our *product* is differentiated, I am going to tell you how our *viewpoint* is differentiated - how we think code review will look in the long-term, and what we're doing today to prepare our customers for that future.


Our thesis can be distilled into three pillars: independence, autonomy, and feedback loops.


## Independence


We *strongly* believe that the review agent should be different from the coding agent. We are opinionated on the importance of *independent* code validation agents. In spite of multiple requests, we have never shipped codegen features. We don't write code; an auditor doesn't prepare the books, a fox doesn't guard the henhouse, and a student doesn't grade their own essays.


Today's agents are better than the median human code reviewer at catching issues and enforcing standards, and they're only getting better. It's clear that in the future a large percentage of code at companies will be auto-approved by the code review agent. In other words, there will be some instances where a human writes a ticket, an agent writes the PR, and another agent validates, approves, and merges it.


This might seem far-fetched but the counterfactual is Kafkaesque. A human rubber-stamping code being validated by a super intelligent machine is the equivalent of a human sitting silently in the driver's seat of a self-driving car, "supervising".


If agents are approving code, it would be quite absurd and perhaps non-compliant to have the agent that *wrote* the code also *approve* the code. Only once would you have X write a PR, then have X approve and merge it to realize the absurdity of what you just did.


## Autonomy


Something that Greptiles generally agree on is that everything that *can* be automated, *will* be automated.


Code validation - which to us is the combination of review, test, and QA, is an excellent candidate for full automation. It's work that humans don't want to do, and aren't particularly good at. It also requires little in the way of creative expression, unlike programming. In addition, success is generally pretty well-defined. Everyone wants correct, performant, bug-free, secure code.


While some other products have built out great UIs for humans to review code in an AI-assisted paradigm, we have chosen to build for what we consider to be an inevitable future - one where code validation requires vanishingly little human participation. We have no code review UI, and view ourselves as more of a background automation or "pipes" product. Human engineers should be focused only on two things - coming up with brilliant ideas for what should exist, and expressing their vision and taste to agents that do the cruft of turning it all into clean, performant code.


## Loops


Not long ago, we released our Claude Code plugin. It can do many things - but most notably, you can ask Claude Code to pull down and address Greptile's comments from the PR. You can ask it to keep going until there are no new comments, waiting a few minutes for a review after each push.


This is a step towards the future we're excited about: Human expresses intent, coding agent executes, validation/review agent finds issues and hands them back - kicking off a loop until it approves and merges. If there is ambiguity at any point, the agents Slack the human to clarify.


The question of how these things are different is important. Unlike picking IDEs and coding agents that ostensibly have low switching costs, code review products are harder to rip out, so your decision will very likely turn out to be a long-term one, especially if you're a large company.


We've been around for about as long as AI code review has been around. It has gone from a fringe interest of the world's most adventurous vibecoders to a mainstream product that our enterprise users (including two of the Mag7) often describe as a "no-brainer" purchase.


Yet, our guess on where this goes is about as good as anyone else's. Meanwhile, we'll keep doing what we've always done - trying to make things our users love.
