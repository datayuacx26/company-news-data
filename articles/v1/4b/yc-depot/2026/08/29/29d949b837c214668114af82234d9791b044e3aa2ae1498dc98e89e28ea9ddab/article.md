---
schema_version: "1.0.0"
document_id: "29d949b837c214668114af82234d9791b044e3aa2ae1498dc98e89e28ea9ddab"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/delegating-work-not-judgment"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T14:43:00.617637+00:00"
fetched_at: "2026-08-11T14:43:03.418584+00:00"
content_hash: "sha256:52d2969220e179ce65a36dbf680452d0310af4cc907279d9af89f380ca636916"
---

# How I delegate the work, not my judgment

I'm doing so much more with agentic workflows. Agents do the tedious work and are way better at multitasking than my own brain. But how do I maintain my knowledge of the whole system?


If I'm using agents to write code, investigate issues, and fix weird bugs, it becomes much harder to commit that work to memory. I can read a final diff and understand what it does, but that's not always the same thing as understanding *why* the system works the way it does or having the intuition to know when a proposed change is off.


Even if an agent writes the change, I maintain and own the system (at least for now).


## The old way I built context


Before I relied much on agents, I built context through the whole process of solving a problem. I'd investigate an issue, read logs, and figure out what was happening. Then I would write a ticket capturing the problem and relevant context so others (and especially future me) would know what was going on and why we were doing this. When I started working on it, I would need to find where the behavior lived in the codebase, understand the existing implementation, and decide how to change it. Finally, I would test and validate the result.


That process was not always fast, but a useful side effect was that by the end, I had a mental model of that part of the system and pretty deep context about tradeoffs and why changes were made. I knew where that behavior lived, what had caused the issue, what alternatives I had considered, and what other pieces of the system could be affected. Even when I forgot the finer details, I still had higher-level intuition about generally how that part of the system worked.


## What agents changed


Agents can now move through that process incredibly quickly. But I have noticed a tradeoff. If the entire workflow happens through agents, I can end up mostly looking at the final answer. I know the bug was fixed. I can see the before and after repro. I can see the diff and the passing tests. But I may not have built the context that I used to get by working through each step myself.


That can slowly take away my intuition about the system, the architecture, and the codebase. The question I keep coming back to is "If I am mostly reading the final answer, when am I building the intuition to challenge it?"


## I'm responsible for judgment


If agents can also debug the next problem and implement the next change, why do I still need to understand the system? I do not need to retain every implementation detail the way I did in the before agent times, but I do need a strong enough mental model to direct the work, evaluate the result, and make decisions that shape the system over time.


I am happy to delegate time-consuming work. But delegation does not transfer ownership. I'm still responsible for how the system behaves in production, whether a proposed change fits its architecture, and what tradeoffs we make as it evolves.


That understanding is also what allows me to use agents effectively. Agents can produce answers that are plausible, technically valid, and still wrong for the system as a whole. Without enough context of my own, I cannot reliably challenge those answers or recognize when a local fix may create a larger problem. Choosing between several reasonable approaches also becomes harder if I first have to relearn the system or depend on an agent to surface every relevant constraint.


I want to delegate the work without delegating away my judgment. I still want to be able to explain why we made a change and remember that later when investigating issues or making further changes.


So far, I've made a few changes to my personal workflows that help me keep building that intuition.


## Stay involved in planning


Agents can create a reasonable plan quickly and it's tempting to accept it and move on. But planning is also where requirements, priorities, scope, and architectural tradeoffs get decided. Those decisions can disappear into an agent's implementation if I am not paying close enough attention, especially while multitasking agents across different tasks (more on this later).


My general rule of thumb is that if something needs a decision, I want it to reach me and not be an invisible choice made by the agent that I only notice after the code is written.


One tool I find useful is Compound Engineering's brainstorm skill. I start by giving it the context of what I want, and it follows up with questions that make me clarify the problem before we settle on a plan. That gets me past a hand-wavy feature outline and makes the decisions behind the work more explicit. For details I have not already specified, it makes me weigh tradeoffs I may not have considered yet and clarify the scope. Some of those boundaries might seem obvious to me, but making them explicit helps both me and the next agent working on the code.


Another tool I've been trying out is the[grill-with-docs](https://github.com/mattpocock/skills/tree/main/skills/engineering/grill-with-docs) skill. It starts by exploring the relevant code and existing domain documentation and then asks high-leverage questions one at a time about the plan or domain concept. It can challenge vague terminology, point out when my assumptions conflict with the codebase, and help surface edge cases I might otherwise overlook.


The value is not that these tools make design decisions for me. They make it harder for me to handwave a decision that needs to be made before implementation begins. Being more deliberate about this part of the design-to-shipping process has helped me retain context.


## Review agent work more pedantically


Agent output can easily land in a place where it technically works, the tests pass, and the overall approach seems reasonable. That does not automatically mean it's written the way it should be. So I treat review as more than a final correctness check; it's also the point where I make sure the code reflects the decisions, patterns, and level of care I want to maintain.


This means going deep on things that can look minor at first glance, such as where code lives, whether names make the intent obvious, whether an abstraction is at the right level and provides the intended value, and whether comments are adding useful context or just adding more text to read.


I also use review to make sure I understand why each meaningful change exists. The question is not only "does this work" but also "would I have made these same choices" and "is this how I want the codebase to evolve." Sometimes that means keeping the approach; sometimes it means changing it to something simpler or easier to read.


## Cap multitasking


One place where my workflow is wildly different from when I started using agents in my day-to-day is the amount of multitasking I do. Agents make it easy to kick off a lot of work at once, which is especially powerful when doing initial investigations. But investigation is often only the beginning.


My involvement usually increases as a task moves into planning, implementation, review, and validation. Not every task demands the same amount of attention, either. Fixing a small bug with clear expected behavior in a familiar part of the codebase is very different from building a new feature that requires decisions about scope, architecture, and how the system should behave. Some changes need very little handholding, while others need me closely involved.


My agents can do a lot of work, but I still have only one singular brain. I might be able to ask agents to pick up several unrelated problems at once, but I cannot deeply plan and design five large unrelated features and fixes at once. If I try, I end up with shallow context on all of them.


Because of that, my limit isn't a fixed number of tasks. I try to judge how much involvement each one will need and how that will change as the work progresses. I may have more tasks running while agents are gathering information, then narrow my focus once those tasks begin requiring decisions and careful review. The question is no longer "How much can I kick off at once?" It is "How much can I reasonably stay engaged with at once?" Being disciplined about that helps me benefit from the amount of parallel work agents can do without creating an information firehose that I cannot meaningfully process.


## Staying in the loop does not mean being involved in everything


I don't want to be personally involved in every little thing my agents do. A small wording change or light refactor does not need the same level of thought as building out a new feature or changing how a system behaves. The goal is to spend more time where the work establishes new patterns, changes existing behavior, or interacts with a complex system.


Agents are making it much faster to produce code. For me, that makes it more important to deliberately maintain context as someone who has to keep owning and maintaining the systems behind that code. I am still figuring out the right balance. But I want to make sure agents help me move through the codebase faster, not leave me behind.


## FAQ


How do I keep understanding my codebase when agents write most of the code?


Participate in the parts of the process where context gets built. For me that means being present for planning decisions, reviewing changes closely enough to know why each one exists, and limiting how many things I have in flight so my attention on each one is real. The diff tells you what changed, and the planning and review tell you why.


What should I look for when reviewing code an agent wrote?


Agent output can technically work, pass its tests, and still not be written the way I want this codebase to evolve. So I get pedantic about the things that look minor: where the code lives, whether the names make the intent obvious, whether an abstraction sits at the right level and does what it was meant to, and whether the comments add context or just more text to read. The question is not only "does this work" but also "would I have made these same choices." Sometimes I keep the approach, and sometimes I change it to something simpler.


How many coding agents can I run in parallel?


It's not really a fixed number. I can have more agents running while they are gathering information, then narrow my focus once those tasks start requiring decisions and careful review. My involvement goes up as work moves through planning, implementation, review, and validation, and a small bug in a familiar part of the codebase needs far less from me than a new feature. If I try to deeply plan five large unrelated things at once, I end up with shallow context on all of them. The question is how much I can reasonably stay engaged with, not how much I can kick off.


## Related posts


- [Staying in control of your codebase in the AI era](https://depot.dev/blog/staying-in-control-of-your-codebase-in-the-ai-era)
- [Using AI as my engineering copilot (not autopilot)](https://depot.dev/blog/using-ai-as-my-engineering-copilot-not-autopilot)
- [My intuition doesn't work anymore](https://depot.dev/blog/my-intuition-doesnt-work-anymore)


Iris Scholten


Staff Engineer at Depot
