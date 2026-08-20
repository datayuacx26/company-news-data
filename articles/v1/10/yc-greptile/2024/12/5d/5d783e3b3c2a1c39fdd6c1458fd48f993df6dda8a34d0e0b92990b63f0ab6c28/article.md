---
schema_version: "1.0.0"
document_id: "5d783e3b3c2a1c39fdd6c1458fd48f993df6dda8a34d0e0b92990b63f0ab6c28"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/make-llms-shut-up"
published_at: "2024-12-18T00:00:00+00:00"
first_seen_at: "2026-07-21T22:07:23.426026+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:b22d5ff5fac49f125449713fcb3cac217f943295599fdbabad61a5de263e6689"
---

# How to Make LLMs Shut Up

*This post is adapted from a talk I gave at the Sourcegraph Dev Tools meetup at the Cloudflare office in San Francisco on December 16th, 2024.*


I am Daksh, co-founder of Greptile - AI that understands large codebases. Our most popular product is our[AI code review tool](https://www.greptile.com/) . It does a first pass review of PRs with full context of the wider codebase, surfacing bugs, anti-patterns, repeated code, etc.


When we first launched this product, the biggest complaint by far was that the bot left too many comments. In a PR with 20 changes, it would leave as many as 10 comments, at which point the PR author would simply start ignoring all of them.


We needed to:


- figure out how to reduce the number of comments that Greptile was generating,
- which meant figuring out which comments should be eliminated,
- which meant figuring out a way to evaluate the quality of each comment.


There were two ideas here:


1. GitHub lets developers react to comments with 👍/👎. We could use this as the quality indicator.
2. We could check which comments the author actually addressed in the code by scanning the diffs of the subsequent commits.


We picked the latter, which also gave us our performance metric - percentage of generated comments that the author actually addresses.


We analyzed existing Greptile comments and found that ~19% were good, 2% were flat-out incorrect, and 79% were nits - comments that were technically true but not something the dev cared about.


This is an example of a nit:


Essentially we needed to teach LLMs (which are paid by the token) to only generate a small number of high quality comments.


## Attempt 1: Prompting


Our first instinct was to “prompt engineer”.


Sadly, even with all kinds of prompting tricks, we simply could not get the LLM to produce fewer nits without also producing fewer critical comments.


Since LLMs are “few-shot learners”, we also tried to give Greptile several examples of good and bad comments in the prompt - hoping it would be able to generalize those patterns.


This also did not work. If anything, this made the bot even worse because it didn’t actually find a useful pattern across the available samples (some might argue LLMs are architecturally incapable of that), and instead inferred superficial characteristics.


## Attempt 2: LLM-as-a-judge


Since we couldn’t get the LLM to stop *producing* nit comments, we figured we would simply add a filtering step where the LLM could rate the severity of a comment+diff pair on a 1-10 scale, and simply eliminate any comments rated less than 7.


Sadly, this also failed. The LLMs judgment of its own output was nearly random. This also made the bot extremely slow because there was now a whole new inference call in the workflow.


## Out of Ideas


At this point we were running out of ideas. We had basically learned three things:


1. Prompting doesn’t work for this
2. LLMs are bad evaluators of severity
3. Nits are subjective - definitions and standards vary from team to team


The 3rd learning was pointing us in the general direction of *learning* . The bot would somehow need to infer where the bar for “nittiness” was for the team, and then filter comments accordingly.


We considered fine-tuning, but the cost, speed, and lack of portability (Greptile would no longer be model-agnostic) ruled it out.


## Final Attempt: Clustering


In a final attempt, we started generating embeddings vectors of past comments on a per-team level that were addressed/upvoted or downvoted by developers and storing them in a vector database. The idea was to filter out comments that were very similar to some minimum number of downvoted comments.


When Greptile generated a comment, we generated its vector embedding and ran it through a simple filter:


- If the comment had a cosine similarity exceeding some threshold with at least 3 unique downvoted comments, it would get blocked.
- The same situation but with three *upvoted* comments, it would pass.
- If neither or both, it would pass.


## Results


Remarkably - this works! It turns out that most nits can be placed into a small number of clusters. Users downvote nit comments, and when enough comments of the same type are downvoted, the bot can filter out any new comments of that type.


Within two weeks of rolling out this feature, existing users saw address rate (percentage of Greptile’s comments that devs address before merging) go from 19% to 55+%. While this is far from perfect, this has been far and away the most impactful technique in reducing the noise produced by the LLM.


This is an ongoing problem for us, and I will likely write a part II when we are fortunate enough to see another inflection in address rate!
