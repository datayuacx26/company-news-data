---
schema_version: "1.0.0"
document_id: "09c045c602693949f74a7b2b4211895d077770014a5617b09773a2e725e56448"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/ai-changed-the-qa-job-the-skepticism-stayed"
published_at: "2026-05-07T15:20:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:5a86387434443371e4630aa42aa078de8d4e993532e70ac1f33403f7e9c69ca9"
---

# AI changed the QA job, the skepticism stayed.

In a typical week, our team ships four or five features. Sometimes more. Testing all of them at that pace, with humans, isn't really realistic.


So at some point we had to bring AI into our QA workflows. Honestly, we were pushed into it. Development moved first, the way it usually does, and QA had to catch up.


By nature, tho, QA should be skeptical, and that didn't change when AI showed up. If anything, we became more careful. Whenever we use a Playwright agent, or one of the agents we've built ourselves with Claude, we always check after them. We’ve come to put more trust in them, but it's something you build per use case: you learn what kinds of bugs each agent catches well, and where it tends to miss things. That takes time, and I don't think there's a way to shortcut it.


There's a lot of writing right now that frames AI as either the end of QA or as a magic productivity boost. From where I sit, neither feels quite right. The data I've seen lines up more with what we're experiencing day to day.


- Faros AI's[2026 telemetry across 22,000 developers](https://www.faros.ai/research/ai-acceleration-whiplash) found incidents per PR up 242% year over year.
- GitClear's[research on AI-assisted code](https://www.gitclear.com/ai_assistant_code_quality_2025_research) shows that code churn (lines reverted or rewritten within two weeks of being merged) has roughly doubled since 2021.
- And, Stack Overflow's[2025 developer survey](https://survey.stackoverflow.co/2025/ai) found that 84% of developers are using AI tools, but only 29% trust the output.


That last gap is, in a way, the part that interests me most. Engineers feel genuinely empowered by AI, and they should, they’re using[fleets of agents to ship way faster](https://rootly.com/blog/a-council-a-sword-and-a-fleet-of-agents-how-i-ship-code-now) . But quality has a harder time scaling at that speed.


Something has to sit between the code that gets generated and what reaches users, and it makes sense to me that QA is the function that absorbs that responsibility.


Most of the conversation I see is about whether AI will replace QA. I think the more useful question is what QA has to become in order to keep up. From inside a team that ships at this pace, the honest answer is that the role has already shifted. We're not just testing anymore. We're designing and maintaining the agents that test for us, deciding which kinds of bugs are safe to delegate and which still need a human eye, and increasingly sitting inside the PR pipeline alongside the coding agents themselves.


That shift is what I want to talk about in the rest of this piece.


## The role of QA has changed


The way I'd describe what's happened to my job over the last year is that I spend less time testing and more time designing the systems that test.


When most of your week was writing test cases, executing them, and triaging the results, you developed a particular kind of expertise. You knew the product surface, you knew the regression patterns, you knew where bugs liked to hide. That expertise still matters, but it's no longer where most of the value comes from.


Most of the value now comes from being able to look at a system and know what's safe to delegate to an agent, what isn't, and how to wire the pieces together so that the agents actually catch what they're supposed to catch. That requires a different kind of understanding. You have to know the codebase well enough to design agents that work *with* it, not against it. You have to understand how the application is built, not just at the surface, but architecturally, because you can't write a good agent for a system you don't really understand.


I think this is why the Capgemini[World Quality Report 2025](https://www.capgemini.com/insights/research-library/world-quality-report-2025-26/) found that GenAI is now the #1 ranked skill quality engineering leaders are looking for, ahead of core QE skills. It's not that the core skills don't matter, they still do, especially when something goes wrong and a person needs to figure out what the agent missed. It's that on top of those, you now need to be able to think architecturally about a fleet of agents that are doing work on your behalf.


I'd be lying if I said this transition has been clean. It hasn't being for me or anyone on my team. The instincts that made you good at the old version of the job don't always transfer. And there's a real risk, I think, of QA people being told to "use AI" without being given the time or the support to actually develop the deeper understanding that makes the agents useful. If your organization wants the productivity gains from AI in QA, that has to come with an investment in QA people learning to think this way. It doesn't happen on its own.


## In-house QA is more powerful than it used to be


The other thing that's changed, and this is maybe the one I feel most strongly about, is that for a lot of QA teams, building your own tooling is now a real option. The default was to buy. There's a whole market of QA platforms, and they exist for good reasons: they solved problems that most teams couldn't solve themselves.


But the underlying capability has become accessible. With Playwright, especially[Microsoft's MCP server](https://github.com/microsoft/playwright-mcp) , and a model like Claude, a small team can build automation that would have required a vendor a year ago. We've done it. It's not magic. You still have to maintain it, you still have to decide what to test, you still have to design the agents carefully. But the barrier to entry has come down a lot.


[Mordor Intelligence](https://www.mordorintelligence.com/industry-reports/software-testing-market) reports that 73% of teams now use at least one open-source testing framework in production, and I think that number understates where things are heading. The combination of Playwright, MCP, and a coding agent like Claude or Cursor is letting teams build things in-house that they would have outsourced before.


That doesn't mean vendors are going away. A lot of orgs, especially larger ones with complex compliance requirements, will find vendors are still the right call. But the choice is more open than it was.


What I'd say to other QA leads thinking about this is: the build-vs-buy decision is no longer a question of capability. Building can now make you faster than before.


## What I'd want other QA leads to take from this


If you're a QA lead reading this, I don't think the question is whether AI is going to change your work: it already has, whether you've adopted it formally or not.


The features your engineering team is shipping are being written, at least in part, by agents. The PRs you're reviewing are coming faster than they used to. The bugs that are reaching production are showing up in patterns that didn't exist two years ago.


The question is whether you're going to be the one shaping how AI gets integrated into quality on your team, or whether it's going to happen to you. Those are very different positions to be in. I'd rather be in the first one, and I think most QA people would too. But it requires being willing to do the work of learning a new way of working, and pushing back when leadership treats AI in QA as a productivity multiplier rather than a fundamental shift in what the job is.


By nature, QA should be skeptical. That's still true. It's just that what we're skeptical of has expanded. We're no longer just skeptical of the code: we're skeptical of the agents we use to check the code, and skeptical of the assumption that any of this works without people who deeply understand what's being built. That skepticism is what makes QA valuable in this era. I don't think we should give it up.


‍
