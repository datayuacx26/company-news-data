---
schema_version: "1.0.0"
document_id: "1db9ba663ac517c6039bcc7878a149ba5406c70a291ea294886646386e420fbe"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/nyc-tech-week-2026-recap/"
published_at: null
first_seen_at: "2026-07-24T05:15:21.199363+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:ee1df8b656806d7c61d9566fba65c75bc4f70b267f64c81ab514917fc08a1726"
---

# NYC Tech Week 2026 Recap: How Category Leaders Are Navigating the Agentic SDLC

# NYC Tech Week 2026 Recap: How Category Leaders Are Navigating the Agentic SDLC


At NYC Tech Week 2026, technical leaders from Optiver, Lucanet, and ShipBob shared how their organizations are scaling agentic coding — the biggest wins, the bottlenecks that follow, and their advice for navigating the agentic SDLC.


Jul 9, 2026 — 11 min read


Adam


Co-founder & CEO


Share on XShare on LinkedInShare on Facebook


Of the many opportunities we’ve had to dive into the subject of agentic coding, NYC Tech Week 2026 was one for the books.


Since we’ve moved past the question of whether AI can write 100% of an engineer’s code, we wanted our panel to focus on how organizations can ship faster and more reliably using coding agents, with real testimony from technical leaders on what’s working and what isn’t.


We asked, and our guests delivered.


Below, we break down a[candid discussion](https://www.youtube.com/watch?v=exTACaE3y5s&t=1048s) we had with Matt Nassr, VP of AI & Global Data Engineering at Optiver; James Musson, VP of Engineering at Lucanet; and Jacob Radkiewicz, CTO at ShipBob, on how their organizations are navigating the agentic SDLC.


Keep reading to discover their biggest wins, practical lessons, unresolved blockers, and advice for other technical leaders looking to achieve AI transformation.


Meet the panel


[Matt Nassr](https://www.linkedin.com/in/matt-nassr-7b58684/)
VP of AI & Global Data Engineering,[Optiver](https://www.optiver.com/)[James Musson](https://www.linkedin.com/in/jamesmus/)
VP of Engineering,[Lucanet](https://www.lucanet.com/)[Jacob Radkiewicz](https://www.linkedin.com/in/jacobradkiewicz/)
CTO,[ShipBob](https://www.shipbob.com/)


## Biggest Outcomes Over the Past Year: Agentic Development Unlocks Benefits Beyond Faster Code


The panelists agreed that while AI coding agents have accelerated development velocity, more impactful outcomes are showing up in how work moves throughout their organizations:


- Roadmap and maintenance work are moving out of the backlog
- Research hypotheses turn into experiments faster
- Non-technical teams’ dependence on engineering is dwindling


### Helping Teams Work Across Unfamiliar Systems


Jacob kicked things off with ShipBob, where planning and implementation cycles are getting shorter, and engineering dependence is being reduced by the day. However, he claimed the larger shift is improved collaboration across hundreds of repositories and dozens of teams.


Rather than chasing down individual engineers for codebase knowledge, teams can use agentic workflows to surface relevant code, architecture, and dependencies in seconds. If they still need a domain expert, the conversation starts from a clearer place.


That reduced dependency has driven noteworthy gains in team efficiency, with ShipBob experiencing a[3x increase in output across 200 engineers](https://www.driver.ai/customer-stories/shipbob-customer-story) .


Jacob also pointed out the importance of context infrastructure in driving outcomes beyond development velocity. When ShipBob stood up a new technical support function, pairing Driver’s pre-compiled context with Claude and related support tools enabled that team to take on 80% of tickets that might otherwise have required engineering assistance.


### Turning More Ideas Into Experiments


At Optiver, Matt shared that the clearest gain was research throughput.


Historically, traders and quantitative researchers needed engineers to build an interface, feature, or research workflow before they could test an idea. Agentic workflows now let those domain experts do more of that early work themselves: prototype research workflows, investigate relevant code, and bring engineering in when the work requires deeper architectural or production expertise.


Matt described the ability to test more hypotheses as a “superpower” for Optiver’s quantitative researchers. Similar to ShipBob, Optiver has reported that investing in context infrastructure has driven outsized gains, with its team seeing a[5x increase in coding-agent effectiveness](https://www.driver.ai/customer-stories/optiver-customer-story) after partnering with[Driver](https://www.driver.ai/) .


### Reclaiming Engineering Capacity While Shipping More Products


James framed Lucanet’s progress around product-delivery capacity. Its engineers work across a varied estate of acquired codebases containing complex finance, tax, and regulatory logic, making unfamiliar systems difficult to navigate.


Agentic workflows are helping teams get oriented faster, investigate unfamiliar code, and review changes with greater confidence. The reclaimed time gives Lucanet more room to move product work forward across systems that previously required significant context-gathering before engineering could begin.


Taken together, the panelists were not celebrating more code for its own sake. They were describing fewer ideas left in the backlog, less work blocked by a single expert, and more people able to move useful work forward.


## Challenges Linked to Scaling Agentic Development: Bottlenecks Shift From Code to Everything Around It


As we moved from outcomes to the challenges that come with scale, the panelists shared that once agents accelerate implementation, the bottleneck shifts from code to everything human around it. That includes research, planning, review, release, operations, observability, and the cultural work of getting teams to think and ship differently.


“AI tooling places a magnifying glass on an organization’s existing processes,” James shared. Where the process is strong, work moves faster. Where it is weak, the obstruction becomes much harder to ignore.


### Quality Control and Adaptability Become Paramount


James pointed to PR reviews as one of the clearest pressure points. If teams produce significantly more code, they cannot review every change with the same level of scrutiny.


To solve for this, Lucanet is using context infrastructure to better understand PR risk and identify which changes need the closest attention. Some low-risk changes may be approved automatically, while higher-risk architectural, financial, or compliance changes receive deeper human review.


James was equally candid about how early this practice remains. No one has fully solved the problem of reviewing a much larger volume of agent-generated code. Matt added that Optiver is experimenting with assigning risk scores to PRs for the same reason.


### Operations Need to Keep Pace With Release Velocity


Matt then expanded the conversation beyond review. If agents help teams ship faster, operations have to move faster too.


Higher release velocity raises practical questions, such as:


- Can teams detect a bug quickly once a change reaches production?
- Can they understand which release introduced the issue?
- Can they roll back safely before the next wave of changes goes out?


For Matt, this points to the need for a more connected SDLC where development, testing, release, operations, and observability are not treated as separate stages. Production telemetry should feed back into the development cycle so teams can learn from what happens after code ships and use that information to guide future work.


### Roles Have to Become More Fluid


James pointed out that the traditional handoff, product manager to product designer to engineer to QA to SRE, no longer holds when agents are contributing across planning, implementation, review, testing, and operations. Teams are learning to organize less around job titles and more around whoever has the skills to move the work forward.


At Lucanet, that’s reshaping how product and engineering collaborate. When agents can tackle larger, longer-running tasks, teams have to ask clearer questions: Who defines the goal? Who sets the constraints? Who knows the customer problem deeply enough to shape the direction?


The answer often splits differently than before. Product managers focus on the commercial problem and customer needs, while engineers, who know the codebase deeply, shape the technical approach. But that’s just the starting point. Decisions around architecture, security, and compliance still demand human judgment. Repeatable work, including implementation, testing, and refinement, can flow faster through agent-assisted workflows.


For technical leaders, that means helping teams stay oriented as expertise, responsibilities, and handoffs keep shifting. The org chart matters less than clarity: who owns which decision, and when does that ownership change?


### Context Infrastructure Evolves From a Want to a Need


Each speaker brought a different version of the same[context blocker](https://www.driver.ai/blog/what-is-codebase-context) . ShipBob coordinates work across hundreds of repositories, 27 teams, and more than 200 engineers. Lucanet navigates[large acquired systems](https://www.driver.ai/blog/the-archaeology-problem-why-ai-coding-agents-fail-on-legacy-codebases) with deep financial and regulatory logic. Optiver has preserved four decades of trading knowledge embedded in code that changes as markets, exchanges, and internal systems evolve.


Matt highlighted the obstacle using Optiver’s example. The company trades on more than 100 exchanges globally, each with its own written specification. But after years of operating against those exchanges, Optiver’s code reflects exceptions and implementation details that the original spec may not capture.


The code becomes the source of truth. That means relevant knowledge may live in a change made years ago by someone who no longer remembers why, or who no longer works there. Surfacing that context enables more people to work safely in systems that previously relied on a single expert’s memory.


Matt had tried to build an internal knowledge store across Optiver. He could create the corpus, but he could not keep reconciling it while teams merged new changes. Lucanet ran into a similar issue with[static steering documents](https://www.driver.ai/blog/why-codebase-context-in-markdown-files) across acquired codebases.


That is where Driver entered the discussion. Matt and James had both[tried to maintain context internally](https://www.driver.ai/blog/diy-context-trap-codebase-context-infrastructure) , but static documents and one-time knowledge stores could not keep up with development velocity. Driver provided their teams with a pre-compiled context layer that accelerated development velocity, research, planning, implementation, and PR review.


## How Teams Are Responding to Agentic Tools: Lasting Adoption Requires Change Management


The conversation moved from systems to teams, and the panelists were clear that lasting adoption depends on whether people experience value in their own work and understand how agentic tools enhance their role.


### Real Experiences Are Solidifying Buy-In


When asked what helped change-resistant engineers move past their initial hesitation, Matt shared that nearly everyone has an “aha moment” when they complete a task in minutes that would have previously taken weeks.


That moment matters because it changes the conversation from abstract excitement to personal proof. Engineers stop evaluating agentic tools as conceptual and start seeing how they can change what they are able to take on.


James added that Lucanet initially tried to create that momentum through broad boot camps across its development centers, but they did not create the level of engagement the organization wanted. His takeaway was that leaders cannot give employees an “aha moment” by showing them a polished demonstration or telling them why they should be excited. They can only provide the tools, space, and real work that allow people to reach that conclusion themselves.


So Lucanet shifted its approach. The company brought engineering leads together to identify their own motivation for agentic development, so they could return to their teams with a personal reason for leaning in rather than a generic mandate.


ShipBob took a similarly practical approach. The company brought engineers together on a large Microsoft Teams call, asked every participant to bring a real task they wanted to automate, and gave them 45 minutes to try the workflow with Driver, Jira, and their agentic coding tools. Jacob said that as team members saw their workflows streamlined in real time, adoption accelerated.


### Engineers Are Reassessing Their Roles


Adoption has not been universally comfortable, and Jacob has observed that firsthand. He described engineers who fully embraced coding agents and others who lost some enthusiasm because hands-on implementation was central to the understanding of their role.


Conversely, James shared his own example of an experienced engineer who expected to dislike the transition after decades of writing code, but realized that what he valued most was the thinking. Less time typing and debugging meant more time to design and solve larger problems.


James also noted that the introduction of coding agents has shifted the podium of top performers across teams. Some previously high performers have adapted slowly, while others have emerged as leaders by leaning into the new workflows.


The panelists agreed that organizations looking to bridge the adoption gap should focus on proving that engineers are still solving hard problems, just at a different level. Instead of spending their energy on every implementation detail, engineers can focus on deciding what to build, how it should work, and whether the result is safe, scalable, and aligned with the product.


Jacob also cautioned against treating skepticism as resistance. The strongest engineers are often the ones best positioned to identify where agentic workflows break down, when junior engineers need more guardrails, and whether teams are moving faster than their quality standards can support.


For effective leaders, dissent becomes part of the adoption strategy: create momentum while keeping the people closest to the code involved in defining what safe adoption looks like. As more teams experience the value of agentic tools, a new question follows: who can now contribute directly to building software, and what engineering standards still need to be upheld?


## Removal of Technical Barriers to Entry: More People Can Build, But Quality Becomes Paramount


When asked how agentic tools were changing[who could contribute to software development](https://www.driver.ai/blog/codebase-context-isnt-just-for-developers) , the panelists had no shortage of examples. Product managers were submitting PRs, traders and researchers were testing ideas more directly, and support teams were resolving issues that once required help from engineers.


These examples showed how quickly the barrier to technical work is falling. However, they added that as participation rises, QC guardrails will play an increasingly crucial role.


### Non-Technical Teams Are Becoming Builders Overnight


Jacob shared one of the panel’s clearest examples: a ShipBob product manager with limited engineering experience who submitted approximately 20 PRs in a very short amount of time. Not every change made it through review, but a meaningful number did.


Matt brought a similar example from Optiver, where traders and quantitative researchers historically relied on engineers to turn ideas into test runs. With agentic workflows, those domain experts can now prototype more of the early work themselves, test hypotheses faster, and bring engineering in when deeper architectural or production expertise is required.


### Engineering Guidance Remains Crucial


As non-technical teams start building more directly, they need a clear path to turn useful prototypes into secure, reliable, production-ready systems. This broader participation changes the point at which engineering guidance is needed.


James illustrated that point with a familiar pattern: someone arrives with a working dashboard and asks how to put it into production. The excitement around the prototype is real, but production introduces authentication, single sign-on, hosting, data storage, monitoring, ownership, and maintenance.


A personal utility can spread quickly and become mission-critical, even though it was never designed for that responsibility. By then, the organization may depend on something that is still, in James’s words, “held together with Sellotape.”


Matt described two requirements for supporting broader participation: a production platform and stronger governance. People need an approved path to build, test, deploy, and operate what they create. They also need platform-level controls because informal policies become easier to bypass when software becomes easier to create.


Across the panelists’ examples, the pattern was clear: product, support, and domain teams can do more with agentic tools, but that broader participation requires ironclad permissions, review paths, and production controls that engineering teams also depend on.


## Advice for Listeners: Don’t Be Afraid to Move Quickly, But Preserve Human Judgment


As we moved into final reflections, the panelists emphasized that agentic development remains a moving target, and teams are still learning how to execute efficiently.


The wins were clear:


- ShipBob pulling roadmap work forward
- Optiver testing more research ideas
- Lucanet turning reclaimed engineering capacity into more product


The remaining question was how to make those gains repeatable without losing the quality, control, and engineering judgment that teams rely on to make good architectural decisions.


When we asked what blockers were still getting in the way, the panelists pointed to practical constraints across the SDLC:


- **Operational inertia** : Moving to an agentic SDLC means changing how work is planned, reviewed, shipped, measured, and supported, which creates friction with engineers who cling to established processes.
- **Fluid role handoffs** : Product managers, business analysts, engineers, QA teams, and SREs are still learning who owns which decisions.
- **Immature evaluations** : Matt called out measurement, evals, and benchmarks as areas that still need work, as it is hard to isolate what model, workflow, prompt, tool, or team habit caused which result.
- **Unpredictable token costs** : As usage grows, teams need to[manage spend without forcing engineers to calculate the cost of every request](https://www.driver.ai/blog/your-ai-coding-agent-is-burning-tokens-on-context-it-should-already-have) .
- **Review volume** : If teams produce more code, they cannot review every change with the same level of scrutiny.


### What Leaders Should Do Now


When asked what advice they would leave with the audience, the panelists agreed that technical leaders should not wait for a perfect playbook before getting started.


Matt elaborated on the urgency, noting that early movers are already pulling ahead and the gap is widening quickly. Agentic coding is not something you can ignore now and catch up on later. People need time to build the habit, learn how to ask better questions, and experience the value firsthand.


- **Start early with real work** : Let teams test agents on actual tickets, not polished demos. The skill compounds as people learn to scope tasks, direct agents, and refine the workflow over time.
- **Measure delivery, not activity** : Tool usage, generated code, and prompt volume can show motion, but leaders should look at end-to-end delivery, review speed, support escalations, and shipped value first.
- **Review by risk** : Put human attention on architecture, compliance, security, financial logic, and production-critical changes.
- **Match models to tasks** : Choose based on quality, latency, and cost instead of defaulting to the most powerful option every time.
- **Invite experienced skeptics** : Use senior engineers to identify failure modes and strengthen guardrails.
- **Keep humans accountable** : Architecture, business intent, and production risk still need human judgment.


The organizations that benefit most from the agentic SDLC will not simply generate code faster. They will realize a fundamental shift in the distribution of work between people and agents.
