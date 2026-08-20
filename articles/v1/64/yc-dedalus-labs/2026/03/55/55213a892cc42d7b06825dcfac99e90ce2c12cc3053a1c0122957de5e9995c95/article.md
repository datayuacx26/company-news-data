---
schema_version: "1.0.0"
document_id: "55213a892cc42d7b06825dcfac99e90ce2c12cc3053a1c0122957de5e9995c95"
company_key: "yc-dedalus-labs"
company: "Dedalus Labs"
source_id: "yc-dedalus-labs-news-import-c6fd8a49c5f3"
canonical_url: "https://www.dedaluslabs.ai/blog/from-today-to-a2a"
published_at: "2026-03-25T09:00:00+00:00"
first_seen_at: "2026-07-24T04:01:24.304641+00:00"
fetched_at: "2026-07-28T22:17:51.657377+00:00"
content_hash: "sha256:ac626bed2127fa60c5a8e7fa4fab843671ffe8582901bdb0ece5fbed3e75739f"
---

# From Today to A2A: Crossing the Imagination Chasm

## The imagination chasm is real


**99% of the most useful agents, tools, and services have not been built yet.**


Not because foundation models are not good enough. Not because we do not have the necessary infrastructure. But because most industries that agents could fundamentally transform still struggle to define what an "agent" even is.


You cannot build what you cannot picture, and you cannot picture what you cannot define.


Even in SF, most people still cannot imagine what a real agent-native world looks like.


We are collectively experiencing what I call: **the imagination chasm** .


## Definitions matter. So what is an agent?


**To me, an agent is simple: agent := model(s) + tool(s)**


Not a workflow.


Not a GPT or Nano Banana wrapper.


Not a thin layer of prompt routing.


Not a skill by itself.


Definitions matter because they set the ceiling of our imagination.


Right now, too much of the landscape still thinks of "agents" as isolated copilots, fragile wrappers, or rigid workflows. **Those definitions are quietly keeping the imagination chasm alive.**


Even I still run into the limits of my own imagination. When people ask me where I see Dedalus in five years, I cannot give them a clean answer. Frankly, I do not think anyone working at the frontier really can.


## OpenClaw expanded the world's imagination, but not enough


**For me, OpenClaw was one of the first moments a general agent felt truly powerful.**


We are all tired of dressed-up workflows and polished demos being passed off as the future. **OpenClaw raised the collective bar for what agents can actually do in the real world.**


But not enough.


By my own definition, OpenClaw is still just a powerful general agent with access to a bunch of models and tools. That is a huge step forward, but it also makes the rest of the gap impossible to ignore.


More importantly, this is just the tip of the iceberg. If this is the first 1%, where do we go from here?


## Powerful does not mean safe


I'm a firm believer that agents with all the context should not automatically have all the access.


Knowing everything is not the same as being allowed to do everything. And it shouldn't be.


This is where I think people start to confuse powerful agents with safe agents.


A single agent can still be incredibly capable. It can use many tools, switch models, and handle complex tasks. But the moment you want agents operating in the real world, scope starts to matter.


Scope of context.


Scope of tool access.


Scope of authorization.


Scope of trust.


That is where multi-agent orchestration starts to become real. A powerful general agent should not do everything itself. It should be able to delegate work to smaller sub-agents with narrower roles, different tools and capabilities, and different trust boundaries.


And once you see the problem that way, it becomes obvious that an A2A future is not just about making agents more capable. It is about building the stack that lets them communicate, coordinate, authenticate, transact, and be evaluated safely in the real world.


## TL;DR: 6 steps to A2A


At a high level, I think the world still needs six things:


1. **Communication layer** , a standard way for models and agents to talk to each other
2. **Infrastructure** , vendor-agnostic, scalable infra to build, test, deploy, and host agents
3. **Trust** , secure, dynamic, scoped authentication and authorization
4. **Payments** , an agent-native layer for transacting in the real economy
5. **Marketplace** , a monetizable exchange where agents and humans find each other
6. **Evals** , benchmarks and definitions of what success actually looks like


**Together, these define the stack for a real A2A economy.**


## 1. An A2A-native communication layer


**Models need a standard way to communicate with tools, and agents need a standard way to communicate with each other.**


This is why protocols like[MCP](https://modelcontextprotocol.io/specification/2025-11-25) and[A2A](https://a2a-protocol.org/latest/specification/) matter. MCP gives models a standard way to connect to tools, resources, and prompts. A2A gives agents a standard way to delegate work to other agents. In other words, protocols replace custom glue code with shared interfaces.


I know the discourse has moved around a lot. MCP was big in 2024. Then, people moved to[skills](https://code.claude.com/docs/en/slash-commands) . Now we are on the[CLI](https://code.claude.com/docs/en/cli-reference) hill. **But skills and CLI are not protocols.** Skills give models specific instructions. CLI is an interface pattern for scripting, piping, and automating interactions. Neither provides the interoperability of a protocol.


That distinction matters because skills and CLI are mostly **one-to-one** . A model loads a skill. A user or agent invokes a CLI.


Protocols unlock a **one-to-many** layer. One agent can discover, route across, and securely interact with many tools, resources, services, or other agents through a shared standard. If you want to build agents and standalone tools that are discoverable and monetizable, you need a standard an ecosystem can form around.


That is why MCP still matters. It is not just a way to call tools. It also separates different kinds of interaction: **resources** provide context, **tools** take action, and **prompts** shape behavior. That separation is useful because an agent should not get all of its context and all of its authority from the same primitive.


*That said, MCP is still early, and A2A is even earlier. More on MCP vs skills vs CLI in my next article. I think that debate is really about something deeper: whether we want agent interaction to stay ad hoc, or whether we want a real shared substrate for the agent economy.*


## 2. Vendor-agnostic, scalable infra


**Protocol ≠ infrastructure, and confusing the two is slowing the market down.**


A protocol can standardize interaction. It cannot, by itself, make agents easy to build, test, deploy, host, or scale. A lot of today's frustration with MCP and agents is not really a protocol problem. It is an infrastructure problem.


For starters, the barrier to entry to building a production-grade agent is still far too high. Developers should not need hundreds of lines of code just to stitch together models, tools, routing, observability, cloud infra, and deployment for a single agent. They should be searching for PMF and testing the edges of their own imagination.


If building a *single* production-grade agent is painful, building *multi-agent architectures* will never happen.


What people actually want is simple: an easy end-to-end way to build, test, deploy, and host agents at scale.


And that infrastructure has to be vendor-agnostic. In a future where most models converge on baseline capability, the small differences in vertical accuracy, personality, tool use, reliability, or even milliseconds of latency are what matter. The edge, then, shifts to whoever can compare, swap, and deploy the right combinations of models and tools fastest for each use case.


Only when it becomes trivial to build, evaluate, deploy, and host a single production-grade agent can real multi-agent orchestration happen at scale.


In the long run, the infra layer that lowers this barrier the most will win.


## 3. Secure, dynamic authentication


**Agents do not hit a capability wall first. They hit a trust wall.**


Nobody really trusts their agents yet, and for good reason. The security surface is still rough. MCP's own guidance already has to account for confused deputy attacks, token passthrough, SSRF, session hijacking, local server compromise, and over-broad scope grants. That isn't hypothetical. In the OpenClaw ecosystem alone, ZeroLeaks reported a[91% prompt injection success rate in an early 13-attempt assessment](https://www.apistronghold.com/blog/openclaw-2026-security-crisis-credential-leaks-prompt-injection) , and Koi Security found[341 malicious skills](https://www.koi.ai/blog/clawhavoc-341-malicious-clawedbot-skills-found-by-the-bot-they-were-targeting) on ClawHub.


To put it bluntly: OAuth didn't solve auth. The hard part in agent systems isn't just standard authentication, it's *delegation under trust boundaries* . OAuth already[supports machine-to-machine access](https://datatracker.ietf.org/doc/html/rfc9200?utm_source=chatgpt.com) and more[fine-grained authorization](https://datatracker.ietf.org/doc/html/rfc9396?utm_source=chatgpt.com) . But that still does not fully answer the agent-native question: how should access move between agents, tools, and services in a dynamic, non-human, context-aware way?


In an A2A world, the real question isn't whether an agent can authenticate. It is whether it should be allowed to do something on someone's behalf, under what scope, for how long, with what downstream permissions, and with what audit trail. A2A is built for[enterprise auth](https://a2a-protocol.org/latest/specification/?utm_source=chatgpt.com) , while MCP pushes[tighter token boundaries and least-privilege handling](https://modelcontextprotocol.io/specification/draft/basic/authorization?utm_source=chatgpt.com) .


That is why I think the market still undershoots the problem. Too many systems still rely on broad bearer tokens, weak audience separation, local secret handling, and runtime token exposure. And most benchmarks still do a poor job reflecting realistic trust conditions. They miss too much of the messy runtime reality: malicious tools, insecure skills, prompt injection, delegated actions, and secrets exposure under real-world pressure. More on that in section 6.


Agents need secure, dynamic, scoped authorization. In the real world, trust is not binary. It is conditional.


## 4. An agent-native payments layer


**Agents need a way to natively pay each other, pay humans, and buy services.**


This part of the stack is moving fast. We now have early protocols like[x402](https://docs.stripe.com/payments/machine/x402) , which turns HTTP 402 into a machine-native payment flow for APIs and agents,[AP2](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol) , which is designed for secure agent-led payments and commerce, and recently,[MPP](https://stripe.com/blog/machine-payments-protocol) or Machine Payments Protocol, which Stripe and Tempo just launched as an open standard for programmatic machine payments.


I do not know what is going to win yet. Maybe no single protocol does. But the underlying requirement is obvious.


If agents are going to become real economic actors, they need native ways to transact without constantly waiting for a human in the loop. And because moving money incorrectly is one of the fastest ways trust breaks in an agent system, those payments have to come with audit trails, permissions, and proofs. AP2 explicitly emphasizes secure, auditable payments, and MPP returns a receipt as part of the payment flow.


Without that layer, agents can reason and act, but they can't participate in the real economy.


## 5. A monetizable marketplace


Great. Agents can now talk to each other, use each other with the right auth, and pay each other. But one question still remains: how do agents and humans actually find each other?


**The right exchange platform makes agents discoverable, usable, and worth building. That is what a marketplace should be.**


A real economy is not built on top of a library of tools. It is built on top of the right platform for exchange: a place where useful agents and agent tools can be discovered, trusted, used, and paid for. It also has to handle the rails around the exchange: pay-ins, pay-outs, and KYC/KYB, so builders can focus on shipping agents, not rebuilding financial infrastructure.


This matters because right now, people are still bounded by three things: *imagination, trust, and incentives* .


Most people still cannot picture what they would build. Most people still do not trust agents enough to let them act. And even when someone clears both of those hurdles, there is often still not enough incentive to build something real, safe, and premium for everyone else.


**If we really dig into human nature, we are missing the right incentives to build.**


Right now, only the biggest names can afford to ship truly safe, production-grade tools with the right OAuth, hosting, compliance, and financial infrastructure behind them. But what about everyone else?


**We need the entire world's effort to cross the imagination chasm.** We need open-source builders, small teams, and individual creators building from their own pain points, because that is where many of the best products come from. If you built something that solves your own problem, chances are it solves other people's too.


Only in a monetizable environment do people build, use, and pay for premium products. Without the App Store, you do not get a world of individual creators and small teams making premium apps. The same thing will be true for agents.


When builders can actually earn, quality goes up. Iteration gets faster. Niche products become viable. Specialists emerge.


**A safe A2A economy starts with the right incentives.**


## 6. A definition of success: benchmarks and evals


Congratulations! We've almost made it to A2A.


But before we get there, I have to ask you: what does a *good* agent actually look like for a given use case?


Don't have an answer?


Most people wouldn't. And that's part of the problem.


Right now, the agent world still feels like the wild west. Everyone is shipping demos. Everyone is calling things "agentic." But it is still unclear what success actually means across tasks, industries, and environments.


That's why benchmarks and evals are so important.


Most benchmarking still isn't realistic enough. It doesn't capture the messy runtime reality: trust boundaries, prompt injection, insecure tools, delegated actions, secrets exposure, changing contexts, real-world latency, and the tradeoffs people actually care about in production. And because benchmarks are bounded by our imagination, we still don't fully know what we should be measuring yet.


Earlier, I defined an agent very simply:


**agent := model(s) + tool(s)**


That definition matters here too. Because once you start from that, the space of what you can test becomes much larger.


The best benchmarks don't just measure progress. They expand the frontier. They teach us what kinds of systems are worth building next. You can test which combinations of models, tools, and agents perform best for a given task. Which setup has the highest success rate, is fastest, cheapest, safest, or works best for a given vertical.


Over time, the best traces become post-training data. The best evals become clearer targets. And the entire ecosystem gets stronger.


We need far more people working on this. If you are thinking deeply about benchmarking, evals, or post-training for agents, reach out. I would love to continue the conversation or collaborate.


*More on this soon.*


## Crossing the imagination chasm


These six steps are not separate. They are the conditions that make each other real.


Protocols without infra are dead specs. Infra without trust will not be used. Payments without a marketplace are just rails. A marketplace without evals becomes noise.


And none of it matters if we still cannot imagine what we are building toward.


That is the real cost of the imagination chasm.


It does not just limit what we can picture. It limits what we build and what we are willing to bet on.


The future of agents will not be unlocked by one better model, one better benchmark, or one powerful general agent. It will be unlocked when more people can actually build, trust, discover, monetize, and measure useful agents for the real world.


**That is how we cross the imagination chasm.**


**And once we do, I think the number of useful agents in the world will explode.**
