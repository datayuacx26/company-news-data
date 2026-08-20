---
schema_version: "1.0.0"
document_id: "3a4eb0ba2bc657cbfb571166c8c22c7af3bacb2d5cd85eec7dd67cc7827fccd6"
company_key: "yc-weave-3"
company: "Weave"
source_id: "yc-weave-3-news-import-b8160165addf"
canonical_url: "https://weaveos.com/blog/the-top-10-models-our-router-actually-uses-and-what-each-one-is-for"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T05:02:28.015578+00:00"
fetched_at: "2026-08-19T05:02:30.372779+00:00"
content_hash: "sha256:ed1005f2c499e8a30cf443eaee24dd5f28617a4a21e2a088ec1b5ffa98a22e98"
---

# The top 10 models our router actually uses, and what each one is for

# The top 10 models our router actually uses, and what each one is for


We route every turn that comes through Weave to whichever model we think is best suited to it. That sentence hides a lot of machinery, so we went and looked at what the router really did over the last 30 days: about 235,000 requests across roughly 40 models on the roster.


The short version is that the router is not picking favorites. It is running a labor market. Cheap models do the errands, mid-tier models do the routine work, and the expensive models get pulled in for the turns where being right matters more than being fast. Here is what that looks like in production.


## How a turn gets routed


Every incoming turn goes through a classifier that assigns it a complexity cluster: fast, balanced, high, or maximum. Each cluster has a roster of eligible models, and an arm selector picks one from that roster based on what has worked before.


Not all traffic goes through the bandit. Some models are hard-pinned to jobs like title generation, classification, and health probes. Some turns are pinned by the user, who has picked a model and does not want us second-guessing them. The rest is usage bypass and a legacy strategy still serving a few pinned installations. Those exceptions matter for reading the numbers below, because a model can rack up volume without the router ever choosing it.


## The top 10


#


Model


Requests


Share


Tokens


What it does


1


deepseek-v4-flash


44.4k


18.9%


~5.8B


Fast cluster, subagent bootstraps, tool-result reroutes, title and classifier pins


2


claude-sonnet-5


38.6k


16.4%


~9.4B


High cluster workhorse, 65% of its traffic is real coding work


3


minimax-m3


24.5k


10.4%


~5.7B


Balanced cluster favorite, 77% balanced, cold-start default


4


gemini-3.1-flash-lite


21.5k


9.1%


~0.8B


100% hard pins, titles and classification only


5


claude-opus-5


21.3k


9.1%


~5.1B


Maximum cluster leader plus 29% user-pinned


6


claude-opus-4.8


20.9k


8.9%


~5.4B


Residual, retired mid-window


7


deepseek-v4-pro


13.5k


5.8%


~3.5B


Balanced cluster, 71% balanced, secondary role in high


8


gpt-5.6-terra


11.1k


4.7%


~2.6B


87% high cluster, the mid-priced arm for serious coding


9


gpt-5.5


10.6k


4.5%


~2.5B


55% user-pinned, the rest high cluster


10


claude-fable-5


8.2k


3.5%


~2.0B


Maximum cluster flagship, hardest turns, highest latency


gpt-5.6-sol just missed the cut at 5.4k requests, of which 93% were user-pinned. More on that one later, because it turns out to be the most interesting model on the list.


## The cheap tier is doing more work than you would guess


**deepseek-v4-flash** is the most-used model on the router and it almost never does anything a human would call hard. It picks up fast-cluster turns, bootstraps subagent sessions, and absorbs "communication" reroutes, which are the turns where a tool has just returned a result and somebody needs to say a sentence about it. None of that is glamorous and all of it is load-bearing. It is the model that keeps the expensive ones from being billed to say "got it, moving on."


**gemini-3.1-flash-lite** is not a bandit arm at all. It is 100% hard-pinned utility: 11,600 classification calls, 8,500 title generations, and 1,200 health probes. It never sees a coding turn and it has the fastest p50 latency on the roster at 0.9 seconds. It is the most boring model we run and we would notice immediately if it went away.


A pattern worth naming here: a meaningful slice of deepseek-v4-flash's traffic is labeled high or explore but is a deliberate downgrade. When a session is classified as hard, we still send the mechanical follow-ups to the cheap model. The complexity of a session and the complexity of the next turn in it are different questions.


## The middle is a two-model race


**minimax-m3** owns the balanced cluster. The selector picks it with probability around 0.97 when the features favor it, and it is also our cold-start default for turns we do not have enough information about yet. **deepseek-v4-pro** is the budget alternative in the same cluster and occasionally gets pulled up into high. Between them they cover most of the routine turns: small edits, straightforward questions, follow-ups that need real comprehension but not deep reasoning.


Balanced is also where our newest machinery runs. We are rolling out an XGBoost arm selector cluster by cluster, and balanced is the one it fully drives today. Fast, high, and maximum are still served by the pairwise bandit we started with. Worth holding in mind while reading the rest of this post: the balanced split reflects the new selector's learned preference, and the other clusters reflect the older one plus accumulated history.


## High is where the real work happens, and Sonnet is winning it


The high cluster has the deepest roster on the router: sonnet-5, opus-5, three OpenAI models, deepseek-v4-pro, glm-5.2, two Gemini variants, and kimi-k2.7-code. **claude-sonnet-5** wins it decisively. It is second overall on requests, first on token consumption at 9.4 billion, and 65% of its traffic is high-cluster coding work rather than pins or bypasses.


**gpt-5.6-terra** is the other clear high-cluster arm at 87%, and it has a distinction no other model has: it is the only model that receives more main-loop turns (6,800) than tool-result turns (4,200). The router trusts it to think and then hands the cleanup to someone cheaper.


## Maximum is small, expensive, and sticky


**claude-opus-5** , **claude-fable-5** , **gpt-5.5** , and **gpt-5.6-sol** make up the maximum cluster. Together they are a small share of requests and a large share of everything else.


Cost per request spans about 300x across the roster, from $0.003 for deepseek-v4-flash to $0.99 for claude-fable-5. The three cheapest models absorb roughly 38% of all requests for about 1% of the money. Meanwhile the four Claude models account for around 89% of total spend, roughly $40,200 of $45,300, on about 38% of requests. That gap is the entire argument for a complexity router in one sentence.


One reason we can afford to send Claude models as much traffic as we do: they run 69% to 80% cache-read tokens, against 45% to 48% for everyone else. Their effective cost sits well below list price.


The other thing the premium tier does is stay put.


Sessions currently pinned to deepseek-v4-flash average 2 turns, and not one of them has ever switched models. Sessions pinned to claude-opus-5 average 86 turns, fable-5 averages 95, and gpt-5.6-sol averages 577. The router does not just pick a premium model for a hard session, it keeps it there. This is also why gpt-5.6-sol looks small on request count and is not small at all in practice: it is a handful of users doing very long, very deliberate work.


## The roster is never settled


The last thing the data shows is how fast all of this moves. claude-opus-4.8 went from 24.4% share to zero in a single week when opus-5 replaced it and inherited its learned bandit weight. claude-fable-5 was at 20% share in mid-July and is under 1% now. deepseek-v4-flash spiked to 41% this week.


That churn is the point. We did not hand-write any of the assignments above, and we do not expect them to hold. A snapshot of a router is a snapshot of what it currently believes, and the useful property is not that it chose well in July, it is that it will choose differently in August without anyone editing a config file.


*All figures are production traffic over a 30-day window, aggregated across installations. 91% of it comes from Claude Code, with the remainder split across codex, cursor, loom, and opencode.*
