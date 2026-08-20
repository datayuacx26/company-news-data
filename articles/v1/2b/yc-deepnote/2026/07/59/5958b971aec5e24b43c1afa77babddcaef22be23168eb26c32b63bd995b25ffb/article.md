---
schema_version: "1.0.0"
document_id: "5958b971aec5e24b43c1afa77babddcaef22be23168eb26c32b63bd995b25ffb"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/how-we-used-claude-to-fix-failing-tests"
published_at: null
first_seen_at: "2026-07-26T08:11:39.657869+00:00"
fetched_at: "2026-07-28T21:16:48.751829+00:00"
content_hash: "sha256:eba2fc293358996f2145346a793540ec54d22bf69e9b1270cc67afdff5bf49c6"
---

# How we used Claude Opus 4.6 to fix failing E2E tests

[← Back to all posts](https://deepnote.com/blog)


We engineers hate writing tests. Fixing broken ones? Even less so. But for any enterprise-grade product, this is a must. So the true enjoyment of using AI in coding comes from using natural language and thinking models to address this issue. It all started with a regression after a release.


## AI as a part of the engineering workflow


We engineers hate writing tests. Fixing broken ones? Even less so. But for any enterprise-grade product, this is a must. So the true enjoyment of using AI in coding comes from using natural language and thinking models to address this issue.


It all started with a regression after a release.


The issue was easy to describe, but annoying to address in practice. Deepnote’s hierarchy is workspace → project → notebook → block. A project is the main container for collaboration, and a public project is one that can be viewed via a shared link. In this case, if a logged-out user clicked **View access** on a public project, they should have been prompted to log in. Instead, the system displayed a wrong state (see below).


## Broken E2E test after a release


So, not only did we have a product issue, but the end-to-end test covering that behavior was also failing. Exactly the kind of issue that one dislikes debugging.


Fixing broken E2E tests is hard because the context required to understand the task is huge. Deepnote is a complex product, split between an open-source and a closed-source internal repository, with 270k+ lines of code (approx. ~1.1M tokens in our case).


A unit test usually fails inside a relatively isolated boundary. The possible causes are narrow. An integration test widens the scope a bit, but it is still constrained.


An E2E test is a different beast. When it fails, almost everything can be in play.


The problem might be in the frontend state. It might be in the browser behavior, backend responses, or a recent UI change that broke some dependent behavior elsewhere. All of this would have to be checked manually when debugging.


That is exactly why I was curious if Claude could handle this task. If an agent can handle this level of complexity, it would save a lot of time. **Spoiler alert** , it did. But the solution wasn’t one-shotted.


## The setup


I did not drag and drop our repos (a roughly 1M context window would’ve been able to handle it) and ask Claude something vague like “please fix the broken test.” Instead, I gave it a structured debugging environment.


The agent got:


1. CI logs


2. a running local app


3. the failing test itself as the context


4. running containers and dependencies


5. the rules for how our E2E tests are run


6. a likely candidate commit that may have introduced the regression


7. a pointer to a similar interaction in the product that already behaved correctly


And importantly, all of this happened inside Cursor, where the agent could move between the code, the test, the logs, and the browser-driven verification flow in one place. I specifically prompted it not to stop at a speculative patch. It also needed to verify the behavior in the browser (this is a good pattern, in general).


A lot of what people call “prompting” in engineering is really context packaging. The better the model can see the system it is working in, the more useful it becomes. This is especially true for E2E debugging, where there are simply too many moving parts to reason from code alone.


A human debugging this by hand would likely need to jump between the test file, the UI, browser devtools, backend logs, the commit diff, and the relevant frontend code. None of that is individually hard. What is expensive is the switching cost and the time all of this consumes.


An agent that can start the environment, inspect the browser, read the logs, and move through the workflow on its own covers that surface area much faster.


That was one of the biggest practical takeaways from this example.


The value was not just in the model, eventually arriving at the right fix. It was that it could inspect the system directly, rather than forcing a human to manually check every layer one by one.


## How Claude worked the problem


Once it had the context, Claude started moving through the issue like an engineer would. In practice, our engineers use Claude Opus 4.6 most of the time for this kind of work, because Anthropic models have been the most reliable for high-context debugging and verification-heavy tasks.


It located the failing test and traced the relevant flow in the codebase. I had not pointed it to the exact frontend file or component. It found the relevant area on its own, then looked at the recent commit that may have introduced the regression. It used the browser to reproduce the live behavior and confirm that the UI was, in fact, wrong: instead of surfacing the login path, the wrong dropdown appeared.


From there, it traced it to a variable being used incorrectly when determining whether the user was anonymous.


That is the kind of bug that often happens in real systems. It is not dramatic, but a small flag gets passed down incorrectly, and the UI takes the wrong path in the user flow, and the tests start failing.


Claude proposed a fix, applied it, and then, most importantly, it went back to verify.


The most useful AI workflows in engineering are not built around one-shot answers. They are built around loops: inspect, hypothesize, change, verify. In the near future, we’ll also be exploring the[Ralph loops](https://github.com/snarktank/ralph) approach to this, but for now, throwing more test-time compute at this ‘smaller task’ did the trick.


## Verification is the point


After shipping the fix, Claude reopened the browser, navigated through the flow again, and checked whether the expected login behavior now appeared (our non-technical colleagues love this flow in Devin, too). It confirmed that the modal appeared in the browser, not just in the test output. It checked for the relevant test ID. Then it reran the E2E test itself to make sure the failure was actually gone.


After that, I manually verified the flow again locally. Even though the agent check went successfully, humans should be the final guardrail against[occasional LLM mishaps](https://www.businessinsider.com/amazon-tightens-code-controls-after-outages-including-one-ai-2026-3) .


Beyond local verification and E2E reruns, our GitHub review process includes automated review tools such as CodeRabbit. A good review layer should scale with the PR, catch obvious issues in parallel, filter out weak findings, and leave the engineer with something actionable: one clear overview and inline comments where they matter.


That’s the current engineering process at Deepnote. The agent may ship a fix, a second agent can test and review, but the engineer still owns the final call (and gets a review from two other engineers before merging).


## **Shorter loops, better fixes**


The practical lesson here is straightforward: E2E debugging is expensive, not because any single step is hard, but because of the context-switching cost across browser, logs, backend, and test layers. An agent can now manage the entire surface area, verify its own code, and implement review comments. This simplifies multi-hour debugging into a task that a junior engineer can handle and review.


AI is most useful when it stops being a separate tool and becomes part of the environment where human work already happens. We will explore how we set up our engineering systems to be most productive with AI in a follow-on blog post - let us know if you’re interested in something else. If you want to work with us on cool stuff like this - check out our open positions!


Tomas Kislan


Full Stack Engineer @ Deepnote


Follow Tomas on[LinkedIn](https://www.linkedin.com/in/tom%C3%A1%C5%A1-kislan-ab45b653/) and[GitHub](https://github.com/tkislan)
