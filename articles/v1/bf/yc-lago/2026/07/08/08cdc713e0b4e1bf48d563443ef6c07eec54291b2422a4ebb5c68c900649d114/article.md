---
schema_version: "1.0.0"
document_id: "08cdc713e0b4e1bf48d563443ef6c07eec54291b2422a4ebb5c68c900649d114"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/removing-humans-from-the-loop-has-become-a-status-symbol"
published_at: "2026-07-20T23:11:34+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:d618e5af8c5382c7b0213b56d25507629334ca30f84b3b49400b96bdea9a424f"
---

# Removing humans from the loop has become a status symbol.

The pitch: ticket in, code out, revenue up, headcount flat.


Then someone, usually someone who manages billions, fresh from shipping a vibe-coded to-do list, asks why everyone cannot do the same.


Billing moves money and trust.


Healthcare, insurance, security, logistics and infrastructure also change states other people rely on. These companies are not less AI-native.


Their workflows are less forgiving. The right mental model is not how far an agent can go. It is where it should stop.


## **1. The judgment boundary**


On July 9, our coding agent opened a fix for a billable metric that kept a stale field after its aggregation type changed. The patch touched ten files. CI was red. The Reviewer left one comment: *“Held: CI is not green on this PR; not reviewed.”* Tests passed the next day, two engineers approved it and the change merged. The full exchange is[public](https://github.com/getlago/lago-api/pull/5895) . This is routine. The stop condition did its job.


Red CI is obvious. The harder stop is a ticket that quietly asks for a product decision. Changing a serializer can be implementation work. Deciding whether a fixed charge survives a plan migration is not. Both can fit in a five-line diff. Easy to say. Harder to spot in a five-line diff.


We call that line the judgment boundary. The agent can work when it can point to a tested precedent. Conflicting precedents are a stop. No precedent means the product decision comes first.


We do not assign autonomy to an agent. We assign it to a task. A documentation typo gets room to run. A tax change gets much less. Ambiguity, impact and reversibility set the limit. Model confidence does not.


OpenAI's[Goal mode](https://learn.chatgpt.com/docs/long-running-work) and Anthropic's[long-running Claude workflows](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) are useful for tests, bug reproduction and prototypes. More time lets an agent attempt more work. It does not grant more authority. Taste, the fashionable human moat, is not enough either. Taste helps choose among plausible answers. Infrastructure first asks whether the evidence supports any answer at all. A beautiful wrong invoice is still wrong.


## 2. The exits are the architecture


At Lago, a Linear ticket can travel as far as a draft pull request with green tests. Final review, QA and merge stay with a developer. We happen to use four agents. What matters is that every one of them has permission to refuse the work.


Triage is read-only. A real bug still gets a` SKIP` when the repository lacks evidence for a safe fix. The Fixer must name the merged, tested implementation it plans to follow before touching the code. The Reviewer ignores the pull request description, reconstructs the request from the ticket and waits for green CI. It can send the draft to a developer or hold it. Never approve. Never merge. The Reviser makes the smallest change that answers human feedback. If the comment opens a design question, the agent is done.


No agent can carry a ticket from report to production. The Fixer follows one rule: mirror, never invent. A proposed fix, a confident support answer or a plausible ticket comment is still a hypothesis. A merged pattern covered by tests is better evidence. Existing code can be wrong too, but this rule catches product decisions disguised as obvious bug fixes.


We hit this with a fixed charge whose code had diverged from the add-on it came from. The ticket had a clean reproduction and an obvious-looking fix. It also hid a question nobody had answered: what should happen after the two diverge? No precedent existed. The agent returned an impediment instead of a patch. We kept that case as a new stop condition.


## 3. The policy learns


We do not fine-tune the agents after every miss. We change the policy. A fix updated the REST read path but missed GraphQL and the write path. Now the agent must enumerate every place the value moves. Another run found two live implementations producing different results and confidently called one deprecated. Now disagreement is a stop. Missing eight locales created a locale check. Claiming a ticket after the pull request existed created a claim-first rule. This is the scar log.


A rule is not magic. It is a scar. The same failure now has to get past an explicit check. Over time, the instructions become a pile of encoded post-mortems. That is the asset.


The policy sits outside the model. We run the pipeline on the Claude API today. The driver is provider-agnostic, so moving to Bedrock or Vertex is a one-variable change. We test several models on the same work. Tool use, instruction adherence, context handling and patch quality all vary. Before a model change, we replay known tickets and compare what each model selects, where it stops, the patch it writes, the tests it runs and what review catches. It is a dependency upgrade, not a personality hire.


Our Engineering team had already spent months running and reviewing Claude-assisted changes across Lago before this pipeline went live. We made the automated workflow public from day one. The point is not volume. It is that every outcome is inspectable. Read[all of them](https://github.com/getlago/lago-api/pulls?q=is%3Apr+author%3Aapp%2Flago-claude-ai-agent+created%3A%3E%3D2026-07-01) : the[PR held on red CI](https://github.com/getlago/lago-api/pull/5895) , the[index change that closed](https://github.com/getlago/lago-api/pull/5896) , the[invoice-title fix that merged after review](https://github.com/getlago/lago-api/pull/5876) . A private repository can run the same pipeline, Open source lets anyone audit the claim.


We are expanding sideways: more repositories, SDKs, dependency bumps, flaky tests, small performance regressions and OpenAPI synchronization. Work with a clear definition of done. Not deeper into product judgment. Product defines the behavior. Engineering decides whether the codebase contains enough evidence. The agent works with what is there. A blocked ticket costs less than a confident guess in a financial machine.
