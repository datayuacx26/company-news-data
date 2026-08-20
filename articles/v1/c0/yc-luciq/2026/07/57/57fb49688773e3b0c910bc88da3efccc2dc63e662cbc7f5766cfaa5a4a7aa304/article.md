---
schema_version: "1.0.0"
document_id: "57fb49688773e3b0c910bc88da3efccc2dc63e662cbc7f5766cfaa5a4a7aa304"
company_key: "yc-luciq"
company: "Luciq (formerly Instabug)"
source_id: "yc-luciq-news-import-4fda431b7a4e"
canonical_url: "https://www.luciq.ai/blog/pm-builds-with-claude-code"
published_at: "2026-07-22T06:55:35.100+00:00"
first_seen_at: "2026-07-24T03:18:24.794445+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:ac8e429c5febdb7349ebb3f8120190f3f1cfa04646931046d2b312ea9d2d955c"
---

# The 80% Prototype: How PMs Using Claude Code Ship Faster than Your Whole Team

***TL;DR:*** *At*[Luciq](https://www.luciq.ai/) *,*[the first and leading Agentic Mobile Observability platform](https://www.luciq.ai/platform) *, our PM builds functional prototypes using*[Claude Code](https://claude.com/product/claude-code) *before a single engineer enters the sprint. By the time the handover happens, 80% of design decisions are already made and documented. The squad ships faster, engineers focus on infrastructure instead of spec interpretation, and the feature arrives right the first time.*


Most software teams are stuck in a cycle of translation loss where PMs and engineers can sometimes feel like they are at odds. It can look like this: the Product Manager writes a PRD. The engineer reads the PRD. The engineer builds a version of the PRD. The PM sees the version and says "not quite." You rinse and repeat until everyone is tired and the feature finally ships three weeks late.


At[Luciq](https://www.luciq.ai/) , we decided that the bottleneck is not the building. It is the queue, the prototyping, and the context switching. We have fundamentally changed our squad structure and our toolset to kill that bottleneck. We are not just using AI coding tools to write scripts. We are using them to change how humans on a team interact.


## 1. The Protected Capacity Model


The foundation is structural. We run a squad of five people. In a typical startup, every engineer has a 100% sprint load. They pick up tickets, they execute, and they have zero breathing room for "the weird stuff."


We did something different. We took one engineer and gave them a half-load on normal sprint work. Intentionally.


### Why Protected Time Changes Everything


That freed capacity is a "special projects unit" within the squad. This person has protected time for prototype hardening, exploratory research, and general improvement tasks that usually die in the backlog. They are not squeezing this in. It is their job to be available for high-upside exploration.


Most teams treat exploration as something that happens in the gaps between real work. We treat it as the work.


## 2. PMs Who Build (not Just Spec)


This is where the AI suite, specifically tools like[Claude Code](https://claude.com/product/claude-code) , becomes the unlock.


I am a PM, not a full-time engineer. But two years ago, the model of "PM builds the v1" was a fantasy. Today, I use AI coding tools to build actual, functional prototypes. Not Figma mocks or wireframes. Working code: backend services, frontend components, and test suites.


### The New Workflow


**The PM iterates alone.** I build versions 1 through roughly 15 on my own using Claude Code. I can iterate fifty times in a day. I try something, I hate it, I throw it away. I am not blocking anyone or waiting for a sprint cycle.


**The 80% handover.** By the time I bring in my partially dedicated engineer, I am not handing over ambiguity. I am handing over a working thing with eighty design decisions already made and documented. **‍**


**The last mile execution.** The engineer's job is to take that 80% and make it production ready. They handle the heavy lifting: auth, RBAC checks, feature flags, and scaling.


### What Changes at Handover


Traditional handover is a PRD and a set of assumptions the engineer has to decode. This handover is a running prototype the engineer can open, test, and extend. They do not spend days wondering what "intuitive UI" meant. They look at the prototype and they know exactly what the goal is.


That is not a subtle difference. It removes an entire class of rework from the sprint entirely.


## 3. Real-World Results: Luciq Lens And The MCP Dashboard


We are not theorising. We used this to ship Luciq Lens, our natural language dashboard interface.


I built the entire NL interface myself over three weeks: a FastAPI proxy, three UI surfaces, fuzzy matching for names, statuses and priorities, and the evaluation test suite. The squad did not have to "build a feature." They had to enhance the product. They focused on migrating the proxy to our AI service and wiring up proper security.


### The MCP Dashboard: Same Model, Same Result


We did the same for our MCP dashboard. I pushed a PR with seven new files and twenty-two unit tests to fix our onboarding flow. The engineer did not have to decipher a spec. They handled the targeted backend tasks: server-side events and gating.


Two builds. Same pattern. The methodology is not a theory. It is what already happened.


## 4. Why Engineers Actually Prefer This


You might think engineers would dislike a PM in the codebase. It is actually the opposite.


**Zero translation work.** They do not spend days wondering what I meant by "intuitive UI." They look at the prototype and they know exactly what the goal is.


**Focus on the hard stuff.** Nobody goes to school to wire up basic UI components for the thousandth time. My engineers get to focus on the interesting work: infrastructure, security, and complex logic.


**Engineers as co-creators.** Since we build developer tools, our engineers are the users. When I bring them a prototype, they provide immediate DX intuition. They are not ticket takers. They are shaping the experience from a working baseline.


### The Data Behind the Frustration


[72% of engineers say they struggle to find time for new features](https://thenewstack.io/survey-engineers-want-to-code-but-spend-all-day-on-tech-debt/) , spending the majority of their week on maintenance and technical debt. A meaningful portion of that is not complexity. It is the back-and-forth of underdefined requirements arriving late in the cycle. Remove the ambiguity at the source, and you remove the rework downstream.


## The Shift Is Cultural. Start Building (and Shipping).


Everyone is talking about how AI makes individuals more productive. That is a small story.


The bigger story is how tools like Claude Code change team dynamics. It blurs the lines between roles. It allows a small squad of five to move with the velocity of a team three times its size.


At Luciq, we have realised that when you give product people the ability to build and give engineers the protected space to co-create, you do not just ship faster. You ship better.


***See how Luciq's agentic platform works.***[Request a demo](https://www.luciq.ai/request-demo) ***.***
