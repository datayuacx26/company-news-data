---
schema_version: "1.0.0"
document_id: "11619a75a3b9ae56c64eeb1617d81033937e4b3bf26ab8f518589caafe490a1f"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/similarity-scores-are-not-enough-for-matching"
published_at: "2026-01-23T02:14:21.231+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:7e445b4a8f0b69aced29595825ef3751a2a6c9a93be197290fff1d90862aeb3f"
---

# Similarity Scores Are Not Enough For Matching

> The sculpture is already complete within the marble block, before I start my work. It is already there, I just have to chisel away the superfluous material.
>
>
> - Michelangelo


Here’s a quick thought experiment. Let’s say you were given a SQL query
` SELECT * from orders WHERE id = 999`


...and you needed to find a matching query from a list of queries below:


- ` SELECT * FROM users WHERE id = 999`
- ` SELECT * FROM teams WHERE id = 456`
- ` SELECT * FROM orders WHERE id = 789`


Most engineers, particularly those coming from an ML background, see mock matching and come up with the approach of collecting all possible candidates, scoring them against the original query by cosine similarity, and picking the query with the highest score.


Similarity scoring might give you these results:


- ` users` query: 0.92 similarity
- ` teams` query: 0.89 similarity
- ` orders` query: 0.94 similarity


By this logic, you would pick the` orders` query with 0.94 similarity.


But to throw a spanner in the works, what if the` orders` query is “used up” and unavailable to be matched? Or what if it's for a POST when you need a GET? Similarity scoring doesn't know about any of this context.


Over the course of building[Tusk Drift](https://github.com/Use-Tusk/tusk-drift-cli) , our CLI tool that records/replays production traffic for testing, the core technical challenge we faced was - when your service makes an outbound call during replay, which recorded mock should we return to serve as test data?


You might see this and think, "Just use Levenshtein or embedding search." We thought that too initially, but it turned out to be sub-optimal. You solve this problem by expanding constraints, not similarity scoring.
‍


## **Good Ol’ Process of Elimination**


Tusk Drift’s core value proposition is that it removes the need to manually mock API responses or set up complicated dependencies for integration tests. Therefore, a lot was riding on Tusk’s ability to select the correct recorded trace to replay against the developer’s changes when its tests run.


Tusk has to do this while encountering traces containing noisy inputs, repeated calls, and multiple protocols. At the same time, it needs to make sure matching happens fast enough to run inside every test (there can be thousands of tests in the suite).


After dogfooding Tusk Drift, we found that pure similarity scoring would fail obviously at times due to semantic constraints (GET vs POST), lifecycle constraints (pre-app-start vs. runtime), and temporal constraints (”used” vs “unused”).


We realized we needed to instead **work backwards from exclusions** . In other words, could we look at all the ways obviously-wrong matches could happen and create guardrails around them?


Some examples of guardrails to enforce:


- Shouldn’t reuse a mock that's already been consumed by another test (for determinism)
- Shouldn’t match GET to POST
- Shouldn’t match` /users` to` /teams`
- Shouldn’t match pre-app-start to runtime requests
- Shouldn’t prefer fuzzy matches from the same trace over exact matches from other traces


While working backwards to a solution isn’t always scalable, we are fortunate that the universe of ways a match could be obviously wrong is limited. So with another constraints in place for a match, you will very quickly get to an answer that is precise.
‍


## **Cascade of Constraints**


Priority Constraint Scope


1 Exact input value hash match Current trace


2 Exact input value hash match (used) Current trace


3 Reduced input value hash match Current trace


4 Reduced input value hash match (used) Current trace


5 Exact input value hash match All suite spans


6 Reduced input value hash match All suite spans


7 Input schema hash match + similarity Current trace


8 Input schema hash match + similarity (used) Current trace


9 Reduced input schema hash match + similarity Current trace


10 Reduced input schema hash match + similarity (used) Current trace


11 Exact input value hash match (duplicate of 5) All suite spans


12 Reduced input value hash match (duplicate of 6) All suite spans


13 Input schema hash match + similarity (pre-app-start only) All suite spans


14 Reduced input schema hash match + similarity (pre-app-start only) All suite spans


This then begs the question of the relative importance of these constraints. There are going to be stricter constraints that, if a match meets, should be considered a clear signal that the match is good. Then there’ll be looser constraints that are meant to help as a catch-all.


To get the full priority cascade (P1-14) above, we further split our matching algorithm into two orthogonal dimensions. In each case, the cascade of constraints prioritizes the most granular to the least granular.


The first axis is **scope** (where to search), where Tusk goes from Trace → Suite, while the second axis is **criteria** (what to match on), where it goes from Exact value → Reduced value → Schema. These compose independently. You can have "exact value at suite scope" or "schema match at trace scope."


Getting this right was important not just for running idempotent tests, but also for determining if Tusk Drift had uncovered an API regression or simply an intended schema change.


The match level (based on the above priority cascade) between the current and recorded request provides valuable signal as to whether a deviation is a regression. Consider these three scenarios:


Deviation Signal Strength


Higher priority constraints → stronger regression signal


Likely Regression


P1


Exact input value hash match


*Same input, different output.*
Something probably broke.


Possibly Intended


P7


Schema match + 0.6 similarity


*Request meaningfully different.*
Deviation may be expected.


Might Be Noise


P11


Suite-wide match


*Common infrastructure call.*
Cross-test pollution likely.


The constraint metadata in each case tells us why this mock was chosen, which allows Tusk to make better judgement on how much the resulting deviation matters.
‍


## **Similarity Score as Tiebreaker**


We’ve been throwing a lot of shade at similarity scoring, but we haven’t entirely forgone it. Where we find it most helpful is in playing the role of a tiebreaker.


At the end of the day, Tusk still computes Levenshtein distance to make an intelligent choice in the following scenarios where there are multiple matches for the constraint.


- **P7-8:** Input schema hash matched for a current trace
- **P9-10** : Reduced input schema hash matched for a current trace
- **P13** : Input schema hash matched across all suite spans
- **P14** : Reduced input schema hash matched across all suite spans


With that said, the reality is that in production, **89.6% of Tusk Drift requests never reach similarity scoring** . They hit P1-P6 (exact/reduced input value hash matching) in the priority cascade before the best mock is chosen for replay testing.
‍


## **Takeaways**


Similarity scoring doesn't understand semantics. Constraints, on the other hand, encode this semantic knowledge. They're the boring part of your matching engine; you might not get excited about lifecycle filters or HTTP shape validation but they’ve earned their place.


**I** f you're building any kind of complex matching system, whether its mocks, search, or recommendations, start first from figuring out **what constraints prevent catastrophically wrong matches** .


Build those constraints in first. Make them fail fast if possible. And layer them in a priority cascade so you can progressively relax them to find the best match. Similarity can then serve as the tiebreaker when multiple candidates pass the same constraints.


In our case, 9 out of 10 requests never reach similarity scoring. They hit exact or reduced input value hash matches at O(1). The algorithm degrades gracefully for the remaining 10%, but only after constraints have filtered the search space down from thousands of candidates to a handful.


Chisel away at the dataset, my fellow devs. The match is already in there.


‍
