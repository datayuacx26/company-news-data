---
schema_version: "1.0.0"
document_id: "7810b137bec89055fa574e7b15c97e3b4af361756e5c4ee263dc963a147ce2fd"
company_key: "yc-aviator"
company: "Aviator"
source_id: "yc-aviator-rss-35144b27026a"
canonical_url: "https://www.aviator.co/blog/code-review-dead/"
published_at: "2026-03-24T16:19:17+00:00"
first_seen_at: "2026-07-20T23:23:35.507164+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:6144b3c460b8f897c0e83821dff66d0d47347f66e99d8ba68ce87a473a973d63"
---

# I said code review was dead. Here’s what I got wrong – and right

I recently published an article saying that 2026 will be the year code review dies. I even offered instructions on[how to kill it](https://www.latent.space/p/reviews-dead) .


The article resonated with many people. Some were glad to see their everyday frustrations acknowledged, a new way of working identified, and were curious to see what a proposed solution might look like.


Some said they were not happy with code review being replaced by something else, but admitted that the reality and the economics all point in that direction. And some were not convinced and voiced their counterarguments.


Before I address them, let me just say two things:


- Do I believe that AI can ever write code without bugs? **No.**
- Do I believe that AI can ever write code better than most humans? **Soon.**


## **“Code reviews are not only about catching bugs”**


This is the objection I take most seriously, because I used to agree.


About a year ago[I spoke to Adrienne Tacke](https://www.aviator.co/podcast/code-reviews-looks-good-to-me) , the author of *Looks Good To Me: Constructive Code Reviews* , about code review as a team’s **knowledge-sharing and record-keeping function.** Code reviews are the best place to capture the whys and the whats about changes in the codebase and spread that knowledge across the team, she argues, because you have to do them anyway. I found that argument compelling then.


It feels like a different era now. The entire software development lifecycle as we know it is collapsing and being redefined. AI did not just make the SDLC faster; it’s completely reshaping it.


**David Poll** recently wrote[a piece](https://www.davidpoll.com/2026/02/code-review-is-not-about-catching-bugs/) that goes even further. His argument is that code review answers a fundamentally different question than “Does this code work?” It answers, ***Should this be part of my product?***


Tests tell you whether the code does what the author intended. Production observability tells you what the system is actually doing. Code review tells you whether the author’s intent was the right thing to build.


But if the goal of review is to exercise judgment about whether a change belongs in the product, then reviewing a 500-line AI-generated diff is not actually the right mechanism. You’re not getting the judgment; you’re getting the diff. **The judgment should have happened upstream, during the planning phase.** By the time the code arrives, you are reading an artifact of a decision, not the decision itself.


### **Where does the judgment go?**


I said the human checkpoint should move upstream: review plans, constraints, and acceptance criteria rather than 500-line diffs. Several commenters pushed back, saying that if the agent writes both the code and the tests, you’ve just moved the problem.


[One commenter noted](https://open.substack.com/pub/swyx/p/reviews-dead?utm_campaign=comment-list-share-cta&utm_medium=web&comments=true&commentId=222221562) that natural language specs are ambiguous and culturally loaded. **But the spec is not supposed to be a replacement for a programming language.** It doesn’t have to be more than a PRD or a JIRA ticket. And engineers don’t have to spend days dwelling over it; with guardrails in place, they can even use AI to help define scope and acceptance criteria.


I see standardized specification as the new unit of knowledge for the project. Not as a prompt, but as a structured artifact, something that product owners and engineers co-author, check into version control, and hold as the thing they are accountable for. Automated checks verify not just that tests pass, but that the code conforms to the spec.


##
**“LLMs generate security-vulnerable code 30% of the time”**


Humans do too. AI code trained on bad human practices reproduces those practices at scale. The reviewer who once caught those issues is now reviewing ten times as much code. The ratio of bugs slipping through goes up even if the individual rate stays flat.


That is a real risk, and the right answer to it is not “humans read more diffs.” It’s better guardrails: deterministic linters, contract verification, adversarial test agents, security scanning that doesn’t have opinions.


The future here is not: catch bugs in review. It is to define what “no bugs” means in formal terms[before the first line is generated](https://www.aviator.co/blog/what-if-code-review-happened-before-the-code-was-written/) , and let machines enforce it continuously.


Here’s an[experiment we ran recently](https://www.aviator.co/blog/what-if-code-review-happened-before-the-code-was-written/) to test how spec-driven verification would work: we implemented a full-stack feature with zero lines of manually written code.


Instead of AI writing code and engineers reviewing it, the team spent two days writing and reviewing a detailed spec: scope, acceptance criteria, and edge cases, before any implementation started. Then we handed the approved spec to an AI agent and let it build.


The result was about 6,000 lines of code. **A second agent then verified the output against the 65 acceptance criteria items in the spec.** It took six minutes. 60 passed, 4 failed, and 1 partial. A human doing the same verification would have taken hours.
The spec review caught issues that would have been a full rework post-implementation.


##
**“AI agents can’t take accountability”**


This is the comment that got the most support, and it deserves an answer.


Accountability is currently unresolved. Right now, if an agent ships something that breaks production, the human who prompted it is accountable. That is inappropriate. It is also unsustainable at scale.


I mentioned in my original article that it is completely understandable not to trust the agents. We’ve all caught them going off the rails more than once. But when we catch a junior engineer making mistakes, we don’t fire them. We teach them, we build guardrails, and we create feedback loops. The accountability model for agents should be structural, **not rely on humans carefully reviewing every line of code that AI spits out.**


The question is then, where does the human sit in the loop? Not in the diff, I’d argue. In the constraints, acceptance criteria, in the decision about what gets automatically shipped and what gets flagged.


## **“I am not reviewing code anymore. But I am user-testing my software 10x more.”**


That comment is closer to the right model than anything else in the thread. The shift is not from review to nothing. And I definitely don’t expect anyone to stop doing code reviews overnight.


A lot more people are shipping AI-authored code than will admit it. The pressure to justify AI investment with velocity metrics is real. The incentive to skip review is real. My argument is not that skipping review is fine; it is that skipping the old kind of review is fine if you have replaced it with[something better upstream](https://verify.aviator.co/) .


We are in transition. The tools aren’t ready. The organizational structures aren’t ready. Most teams are neither fully reviewing nor fully trusting. They are somewhere in between, making it up. That is normal. It is also temporary.
