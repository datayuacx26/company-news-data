---
schema_version: "1.0.0"
document_id: "7c910d12ea6f8fa2f0c42b34b2575cd8408ee61cb60af8873e36fd74d4319f0f"
company_key: "yc-metorial"
company: "Metorial"
source_id: "yc-metorial-news-import-9c457a3d81d4"
canonical_url: "https://metorial.com/blog/ai-infrastructure-accumulates"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-24T04:14:45.918392+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:d2d4aa6871299645e40e3bd715144e6fda123181cb2335ac7ab3a23a3bbb6284"
---

# AI infrastructure accumulates

# AI infrastructure accumulates


There is no meeting where someone stands up and says "this quarter, we are going to build an internal AI integration platform." No roadmap line item. No design doc titled *Agent Infrastructure v1* . No budget.


And yet, eighteen months later, you have one. It just has no name, no owner, and no documentation. It lives in four repos, three of which are called something like` ai-utils` . It is held together by a person named Dani who is the only one who understands the OAuth refresh logic, and Dani is on vacation.


This is how almost every AI platform actually gets built. Not by decision. By accumulation.


## It always starts reasonable


Here is the part that makes this so hard to catch. Every single step is correct. Nobody does anything dumb. The platform that nobody chose to build is assembled entirely out of good decisions.


Watch how it goes.


**Week one. Ship one agent.** Someone on the team wires up a model to do something genuinely useful. Summarize support tickets, draft replies, whatever. It works in the demo. Everyone is happy. Lines of integration code so far: roughly zero. The model did the work.


**Week three. It needs to reach a real system.** The demo used a CSV. Production uses Salesforce. So you write the Salesforce integration. OAuth flow, token storage, refresh handling, rate limit backoff, the pagination quirk where the API returns 200 with an error body. Maybe two thousand lines, and honestly you are a little proud of it. It is clean.


**Week six. It needs a second system.** Now it has to read from Jira too. You already solved this pattern for Salesforce, so you copy it. Except Jira does auth differently, paginates differently, and rate limits differently, so "copy it" turns into "rewrite seventy percent of it." Call it six thousand lines across the two. Still fine. Still readable, if you squint.


**Month four. Security finds out.** Someone in security notices that an autonomous process has standing write access to the company CRM and asks a completely fair question: what is it allowed to do, and how would we know what it did? You do not have a good answer, so you build an audit log. Eighteen thousand lines now, and the new code is the least loved code in the building because nobody wanted to write it and nobody wants to maintain it.


**Month seven. The deadline.** A bigger customer needs per-team scoping so their agent cannot read another team's data. This is real access-control work and it deserves real design time, but it ships the week of a launch, so it gets done fast instead of done well. Thirty-five thousand lines. There is now a file called` permissions.ts` that everyone is slightly afraid of.


Add it up. None of these were mistakes. Each one was the obviously correct next move. And the total is a platform that nobody chose to build, nobody owns, and security still cannot fully audit.


## The model was never the hard part


Here is the thing that surprises people the first time they sit with it. Across that entire eighteen months, almost none of the difficulty came from the AI.


The model worked in week one. The model kept working. Every hard week after that was not about reasoning or prompts or fine-tuning. It was about plumbing. Auth. Tokens. Scopes. Pagination. Retries. Rate limits. Audit trails. The unglamorous connective tissue between an intelligent thing and the systems it needs to touch.


We say this constantly because it keeps being true: the model is the easy part. The integration layer is where the actual production work lives. That is not a knock on models. It is just where the engineering hours go, and the hours do not lie.


The cruel part is that the integration layer is also the part teams systematically underestimate, because in the demo it does not exist yet. The demo is the model. Production is the plumbing.


## Why glue code is the worst code you own


Glue code has a specific personality, and it is a bad one.


It is load-bearing, so you cannot delete it. It is boring, so nobody wants to maintain it. It is bespoke, so it does not benefit from anyone else's fixes. And it is invisible to leadership, so it never gets staffed properly until it breaks in a way that is visible to a customer.


It is also where your security exposure quietly concentrates. Every hand-rolled OAuth flow is a place to leak a token. Every "we will add scoping later" is a standing grant that touches more than it should. Every integration without an audit trail is an incident you will not be able to reconstruct. The glue is exactly the layer a regulator or a customer's security review will ask about, and it is exactly the layer you built in a hurry the week of a launch.


So you end up with the worst possible ownership structure: the most sensitive code in your AI stack is also the least owned.


## The fix is not "write better glue." It is "stop owning the glue."


Once you see the pattern, the question changes. It stops being "how do we write our Jira integration more cleanly" and becomes "why are we writing our Jira integration at all."


This is the bet behind MCP, the Model Context Protocol, and behind what we are building at Metorial. The connective tissue between agents and real systems should be standard infrastructure, not something every team rebuilds from scratch and then babysits forever. Auth, scoping, audit, rate limiting, the boring retryable middle. That should be a platform you adopt, not a platform that accretes inside your codebase while you are looking the other way.


We think of it the way teams eventually thought about hosting. At some point you stopped racking your own servers not because you could not, but because it was never the thing that made your product good. The integration layer for agents is heading the same way. It is real infrastructure. It deserves to be treated like infrastructure, owned by people whose job is that infrastructure, instead of being the thing your best engineer is quietly maintaining on the side.


## One honest test


Here is a question worth asking your own team this week.


If your most sensitive agent integration broke tomorrow, who gets paged, and can they tell you exactly what that agent is allowed to do and exactly what it has done?


If the answer is a specific person and a specific dashboard, great, you have real infrastructure. If the answer is "Dani, probably, when she is back," then you did not decide to build an AI platform. You accumulated one. And the first step to owning it is admitting it exists.
