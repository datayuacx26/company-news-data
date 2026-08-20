---
schema_version: "1.0.0"
document_id: "6b87b39c919d9efa4e150988d69a716a6f7a13f4f14b63e58252e4e37eac013b"
company_key: "yc-aviator"
company: "Aviator"
source_id: "yc-aviator-rss-35144b27026a"
canonical_url: "https://www.aviator.co/blog/how-to-kill-the-code-review/"
published_at: "2026-07-22T13:33:24+00:00"
first_seen_at: "2026-07-22T13:39:01.772886+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:e200dcf287088f80cb97b70f148e740787d0b91e9870fed814320c106faae5fa"
---

# How to kill the code review

This is a follow-up to my viral post published on The Latent Space titled[How to kill the code review](https://t.co/YPwkvT9tiS) .
It argued that traditional code review is no longer a viable quality gate in an AI-accelerated SDLC. The natural next question is: if not code review, then what?


In this article, based on my talk at the[AI Engineer World’s Fair](https://www.ai.engineer/worldsfair/2026) conference, I’ll explain the two-layer system that replaces code review.


Code review was broken before AI coding tools showed up. But writing code was still slower than reading it, so we could pretend everything was fine.


The math broke with the agents that write thousands of lines of code in less time than it takes me to write this sentence.


In April this year, Faros shared in a[report](https://www.faros.ai/blog/ai-software-engineering) what they observed while instrumenting 22,000 developers:


- Code churn (deleted vs. added) is up 861%
- Incidents per PR are up 243%
- Time in review is up 441%.
- 31% more PRs are being merged without any review at all.


Engineers didn’t just decide to stop reviewing. They just can’t keep up! For a large and growing share of code, the review already isn’t happening. The question isn’t whether to kill the code review. It’s what replaces it.


## **Code review is not about reviewing code**


Code review was never one thing. Over fifteen years, it became the foundation of how teams collaborate—knowledge moves, juniors learn how seniors think, architecture gets questioned, ownership is shared. It also catches bugs and enforces conventions.


Code review really has two jobs.


The first job is alignment. This is where knowledge moves through the team, where junior engineers watch how senior engineers think, where architectural decisions are challenged, and where shared ownership takes shape. It is the place where the team agrees on what is being built and why.


The second job is standards-checking. Catching bugs, enforcing conventions, flagging security issues, and verifying that the output meets the team’s expectations for the code itself.


These two jobs look the same from the outside—both happen in the pull request, and both involve someone reading something and leaving a comment. But they are structurally different problems that need different solutions. Conflating them is why the conversation about AI code review keeps going in circles. People who want to preserve reviews are usually trying to protect alignment. People who hope to automate it away are usually solving for standards.


## **Job one: Alignment**


Alignment is the harder problem, and the one AI cannot solve. When a human writes code, intent travels with the author through the review process. They can explain what they considered, what they rejected, and the constraints they worked within. Even unspoken, that context is accessible.


When an agent writes code, intent lives in a prompt that was never saved, a ticket that does not capture the decision-making, or only in the engineer’s head; the implementation is preserved. The reasoning behind it is not.


The popular response to this is spec-driven development: write the intent down completely before generation begins, make the spec the source of truth. The instinct is right. However, the spec is written before the work teaches you anything, so it is wrong in ways you cannot yet see. Implementation surfaces a hundred micro-decisions the spec never anticipated, and they never make it back into the document.


Intent does not live in a spec. It lives in your JIRA ticket, your design doc, and most importantly, in your prompts. The back-and-forth with an agent is a live stream of real decisions. The spec is being generated in real time and discarded when the session ends.


[Intent-driven development](https://www.aviator.co/verify?utm_source=tns&utm_medium=content&utm_campaign=q3-2026-tns-verify&utm_term=net-new&utm_content=awareness) means capturing it where it is actually made, not in a document written before the work starts, but in the session where the decisions happen.


Capturing intent is easier than it sounds. A brief description of scope, a list of acceptance criteria, or a note on what is explicitly out of scope. This can often come directly from the prompts—the decisions the engineer made while talking to the agent are already there. The discipline is in capturing them before submitting the change rather than losing them.


The reviewer’s job changes. Instead of reading a 600-line diff and asking, “Does this look right?” they are reading eight lines of intent and asking, “Are we solving the right problem with the right constraints?” That is a fundamentally better use of a senior engineer’s time. It is also where the knowledge-sharing function of review survives. Reviewers reading acceptance criteria are reading the decisions behind the implementation, not the implementation itself.


## **Job two: The AI slop register**


Standards checking is the part everyone wants automated, and it is exactly where AI code review performs worst. Pointing an LLM at a diff with a checklist does not create a quality gate. When the same model writes and reviews the code, they share the same blind spots. Verification has to be structurally separate from code generation.


Every codebase has patterns that AI consistently gets wrong. These comments appear in code reviews repeatedly because there is no mechanism to make them permanent checks. Every recurring review comment is a guardrail that has not yet been written.


We call that[Invariants](https://docs.aviator.co/verify/concepts/invariants?utm_source=tns&utm_medium=content&utm_campaign=q3-2026-tns-verify&utm_term=net-new&utm_content=awareness) or your engineering team’s[AI slop register](https://thenewstack.io/engineering-ai-slop-registry/) . It’s the running list of patterns your team keeps correcting. When a senior engineer types the same comment for the fourth time, that comment belongs in the register. It then becomes an invariant: a standing check that automatically runs on every change. And the register builds itself: once a week, it reads the comments on the merged PRs and proposes new invariants.


When teams pull their last hundred PR review comments and sort them—deterministic, execution-testable, genuine judgment—the split tends to land around 45/30/25. Three-quarters of review feedback is codifiable. Once a check is written, it never needs a reviewer again.


For the remaining 25%, genuine judgment calls remain. Pattern recognition over context, architectural questions, cases where something looks off but a rule cannot capture why. This is where LLMs earn their place as a scoped fallback: a separate verifier agent with no shared context with the implementing agent, a constrained prompt, and structured output with file references and reasoning for each finding. Deterministic where you can. LLM where you must.


## **Build the register first. Do not kill the review on day one.**


The system that replaces code review runs on two layers.


The first layer captures intent: it watches the session, turns the decisions into acceptance criteria, and has a human agree. The second layer does semantics: it takes those criteria, adds the Invariants from your slop register, builds a test plan, and runs it deterministically. Both layers feed the new form of “code review”—a ”human engineer now reviews intent, decisions, and verdicts instead of a diff.


The two layers answer the question that traditional code review was trying to answer: Did this implementation do what it was supposed to do, and does it meet the team’s standards?


Have in mind that the transition will not happen overnight. Month one will most likely feel like the team is doing double work: reviewing code and[building invariants](https://docs.aviator.co/verify/concepts/invariants?utm_source=tns&utm_medium=content&utm_campaign=q3-2026-tns-verify&utm_term=net-new&utm_content=awareness) . Set clear expectations with the team: the register takes time to build. During month two, the register catches enough that reviews shrink. In month three, the reviewer reads intent, not code.


The review is not killed. It is transformed. What goes away is the part that was never working very well anyway: a human skimming a 500-line diff at 4 p.m., pattern-matching against their memory of the codebase, writing the same comment they wrote six months ago. What stays is the part that required human judgment all along.[Replace code review with verified intent.](https://www.aviator.co/verify)


*This article was originally published on The New Stack.*
