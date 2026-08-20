---
schema_version: "1.0.0"
document_id: "96855d85969331dd22959fc42d6a014a779cfe81fa1e37ca07020d279357e4d6"
company_key: "banco-santander-s-a-sponsored-adr-spain"
company: "Banco Santander S.A. Sponsored ADR (Spain)"
source_id: "banco-santander-s-a-sponsored-adr-spain-rss-14b90327b079"
canonical_url: "https://www.santander.com/en/stories/engineering-the-loop-how-santander-makes-ai-agents-reliable"
published_at: "2026-07-27T15:30:00+00:00"
first_seen_at: "2026-07-27T15:57:35.879479+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:a7e8c52bc52c7b5544d41ee101989377e21c070d05c92cd7f5fab488ee1eff87"
---

# Engineering the loop: how Santander makes AI agents reliable

[Communications Santander](https://www.santander.com/en/press-room/topics-of-interest)


CEST


[Linkedin](https://www.linkedin.com/company/banco-santander)


[Instagram](https://www.instagram.com/santander.global/)


[X](https://x.com/bancosantander)


As Santander expands the use of AI across customer service, software development, operations and internal productivity, there is an increasing need for building systems that do more than answer questions. They retrieve information, use tools, make decisions, complete tasks and interact with other systems. That shift from chatbot to agent is one of the most important developments in AI.


Here, an agent means an AI system that can decide what to do next, use tools, assess outcomes and continue working towards a goal. When a system is expected to take action rather than simply produce text, the engineering challenges change too. The focus is no longer only on the model itself, but on the software that surrounds it and enables it to work safely and effectively over many steps. One of the techniques used to build these systems is something that the technical team calls 'Loop Engineering'.


##


Harness to control


A single call to a language model produces an answer and stops. Real work rarely behaves that way. Most business processes unfold as a sequence of decisions: gather information, decide what to do next, perform an action, check the outcome and repeat until the objective is reached. An AI agent operates in much the same way.


That means the real unit of engineering is not the model call itself but the loop surrounding it. In practice, that loop is orchestrated by what is often called a harness: the components that manage state, invoke tools, evaluate outcomes, enforce boundaries and decide whether the agents should continue, retry, escalate to a human or stop. In Santander, we have adopted the term 'Harness Engineering' because it captures this shift well. As AI systems become more agentic, much of the engineering effort moves from the model itself to the harness that coordinates it.


In simple terms, the loop and harness follow a cycle: observe, decide, act, check and repeat. And stop when it should.


state = observe()
while not done(state):
plan = model.decide(state) # choose the next action
result = act(plan) # a bounded, logged tool call
state = observe(result) # ground on real state, not just chat
if outside_limits(state):
halt() # stop and hand over to a human


The code is simple. The engineering is not. The behaviour of an AI agent depends less on the model sitting in the middle of the loop than on the harness surrounding it. Reliability, safety, governance and performance all emerge from that design.


**Several design choices matter.**


The first is clear: **stopping conditions** . Every loop needs explicit rules for when it should stop, escalate or hand over to a human. It also needs limits on iterations, preventing unnecessary costs or unintended actions from growing indefinitely.


The second is **grounding** . Rather than reasoning only from conversation history, an agent should repeatedly reconnect with verified sources of truth. This reduces the risk that small mistakes accumulate into larger ones over time.


The third is **evaluation** . At every stage, the system should be able to assess whether it has completed the task, whether more work is needed or whether the situation requires human intervention. This evaluation step is often what separates a useful agent from one that simply continues operating without making progress.


Finally, there are **boundaries** . In a regulated environment, not every action should be automated. Certain decisions require escalation, approval or review. A well-designed harness also defines what each tool is allowed to be used for, distinguishing between actions that are read-only, reversible or require explicit human approval. These limits are not an external control layer added afterwards. They are part of the design of the loop itself.


Underneath all of this sits **observability** . Every relevant step is recorded with the appropriate level of detail so that outcomes can be reconstructed, analysed and audited when necessary, while respecting governance requirements.


From this perspective, the most robust loop is often the simplest bounded one. Trying a task, checking the result independently, retrying with fresh context if needed and stopping when progress stalls will often outperform a single, increasingly complex prompt.


##


'Loop Engineering' in practice


One example of this thinking is[Autoguardrails](https://github.com/SantanderAI/autoguardrails) , one of the AI projects Santander has released as open source.


Rather than relying only on manual policy tuning, Autoguardrails wraps guardrail improvement into an evaluation loop. A candidate policy is tested against a fixed set of both ordinary and adversarial inputs. It is kept only if it improves protection without materially reducing legitimate responses. Otherwise, it is rejected.


In other words, the loop becomes the optimisation mechanism: propose a change, evaluate it, measure the outcome, keep what works and discard what does not.


The same idea can be applied far beyond guardrails. Whether the objective is improving safety, orchestrating tools or completing a business process, the intelligence and reliability of an agent come from the loop and the harness that surrounds the model, rather than from any single prompt.


##


Why it matters


As AI systems become more capable, there is a natural tendency to focus on the model itself. Better models matter, but experience has shown that capability alone is not enough. In practice, an agent is only as trustworthy as the loop and the harness it operates within.


The work of designing stopping conditions, evaluation steps, escalation paths, boundaries and observability may not be the most visible part of AI. Yet it is often what determines whether a system can be relied upon in production. For users, all of this should be invisible. They should not have to think about loops, tools or evaluation. They should simply experience an AI system that is useful, careful and able to ask for help when it should.
