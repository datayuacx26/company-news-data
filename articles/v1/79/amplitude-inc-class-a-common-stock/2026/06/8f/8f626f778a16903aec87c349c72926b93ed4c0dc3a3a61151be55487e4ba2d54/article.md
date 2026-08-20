---
schema_version: "1.0.0"
document_id: "8f626f778a16903aec87c349c72926b93ed4c0dc3a3a61151be55487e4ba2d54"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/wave"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-22T23:44:16.700638+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:5020f1f7e4e49a37a09880dfc1678f3a406b8e7a32c81329ba919095bb0d39b8"
---

# How We Built a Product That Tells Us What To Build Next: Inside Amplitude Wave

Coding agents like Claude, Codex, and Cursor have upended how product, engineering, and design teams work, and the models keep getting more capable. This week, Anthropic[shipped Fable 5](https://www.anthropic.com/news/claude-fable-5-mythos-5) , which ran a migration across a 50-million-line codebase within a day that would otherwise have taken a whole team over two months. As software development costs drop, we’re seeing the bottleneck shift from *how fast* you can build to deciding *what’s valuable* to build. Are users actually retaining, engaging, or churning? What are customers asking for? Is what you’re shipping working? Are you learning from experiments? This shift has profound implications for how teams build products.


AI-native teams are also[moving away from rigid roadmaps](https://steve-yegge.medium.com/the-anthropic-hive-mind-d01f768f3d7b) toward leaner, faster-iterating operating models: smaller teams using coding agents to prototype, explore, and ship, guided by taste and iterating based on customer feedback. This new approach to product development requires rethinking the infrastructure on which product and engineering teams operate today. It means combining context from your product data, business data, and codebase so agents can take advantage of the infrastructure and build reinforcement learning loops for your product.


That’s why we built[Amplitude Wave](https://amplitude.com/wave) , a proactive product agent that helps teams build self-improving products. Wave continuously analyzes your Amplitude data for signals, surfaces product improvement opportunities that you or your agents can approve and ship, then tracks the downstream experiments, outcomes, and learnings. With Wave, every product team can turn the signals buried in your data into the next move worth making, then measure whether it landed.


## How we got here


Humans triage Slack threads, watch replays, scan charts, read error logs, and squint at messy experiment results, then try to synthesize it all to answer one question: What should I work on next? We’ve spent a decade perfecting one side of this by ingesting behavioral signals and providing baseline analytics.


Our[first generation of agents](https://amplitude.com/ai-agents) pushed that further by interpreting those signals in context, like your conversion funnels or dashboards. They let you sense more deeply across your product, both actively through human input and passively through product monitoring.


Image credit:[James Clear](https://www.auroralive.com/insights/james-clear-unlock-the-power-of-tiny-gains)


Building on the shoulders of those agents, we are now entering a new era of product building with Wave. Wave completes the end-to-end loop, is agent-native, and aligns with your objectives over time. Tell us a few sentences about your product area, and Wave maps the user journeys, analyzes your key product flows with agent swarms, and synthesizes what it finds.


The output is concrete problems, product specs, and experiments to validate them, all while capturing the feedback signals that improve the next loop incrementally with compounding effects over time.


##### Our North Star is for product teams to get better exponentially with AI, not linearly. That’s a different kind of target than most tools are aiming for.


Our first iteration of Wave earned only about a 5% positive feedback rate internally on opportunities surfaced. Over time, we’ve used Wave’s self-improving system on itself, steadily iterating and increasing that positive feedback score to above 70%.


Notably, these aren’t just bug fixes. They’re larger behavioral patterns surfacing across event streams, replays, and feedback. More interesting is how this shifted the way our teams work, away from big-room planning and toward ambient signal analysis and constant product iteration.


Our[Design Agent](https://amplitude.com/blog/design-agent) is one example: The team now improves their agent through this same loop, letting Wave surface what’s failing and suggest what to change instead of triaging it by hand. That transformation in our processes and decision quality is the real win, and making the loop faster and more effective is where all of our development effort now goes.


## The mechanics behind Amplitude Wave


For this system to matter, we had to pick the right unit of work. Today’s software factory products are good at one half of the job: production and verification. Give coding agents a detailed specification, and they’ll execute it cleanly, open a pull request, and verify the acceptance criteria.


The trouble is feeding them the right context. Most signals they run on today, including bugbots and error logs, are easy to act on because a traceback is clean and specific, but specific isn’t the same as important. You get an overwhelming flood of well-formed work items with no sense of which ones actually improve the product, business, or the metrics you care about.


**Our goal was one layer up: the product itself. Not just the code, but what each part of the product is for, the metrics you’re trying to move, what customers are asking for, and how you stack up against comparable products.**


**Today’s software factories miss all of that. They execute specs without any sense of whether they were worth doing in the first place.**


So we built a network of agents that reads every signal a product gives off, maps the product, synthesizes and prioritizes problems worth solving, drafts specs, routes each to the right executor, then verifies and measures the result. Throughout the loop, we capture implicit and explicit feedback so the system learns each time. Let’s walk through each stage.


### Sense: Finding the problems worth solving


Amplitude processes over close to 80 billion events a day and over 2.5 trillion events a month, an enormous amount of behavioral context. Wave uses that context when the full platform is enabled, while staying extensible to external MCP connections. Our agents use that context to map your product: which areas exist, what each is meant to do, the corresponding events and pages, the objectives and success metrics, and the health of each node.


A note on privacy


We don’t train AI models on any customer data. Agents start from foundation-model best practices and system prompts we’ve developed from working with thousands of teams. Read the[FAQ](https://amplitude.com/ai-agents) .


Next, we dispatch swarms of scoped agents to hunt for opportunities in the weakest areas.


Take Amplitude’s chart creation flows. One agent digs into underperforming metrics and channels; another reads recent feedback from support and Slack; another analyzes agent traces from people editing charts with AI; another checks page load and query performance; another watches session replays to find friction; and another looks outward, at how comparable products solve the same flow. The system is proactive. Instead of waiting for a Datadog alert to fire, it asks where the holes are most likely to be and sends agents to investigate. Each agent returns candidate problem statements, all cited and tagged with the source that produced them.


We then integrate those candidates and surface the emerging opportunities. The themes include a mix of bugs, quick wins, features, growth experiments, large strategic bets, and our favorite: wildcards, or radical rethinks of the status quo.


### Decide: Prioritize opportunities by what impacts metrics


Here we dispatch another team of agents, one per opportunity seed, to define the problem and its scope. The output is a brief problem statement and the evidence behind it, including charts, replays, feedback, or other traces.


Using coding-agent connectors or direct GitHub integration, Wave scans the codebase to understand the current product state and recent changes, then plans the spec in detail. Feeding code context and git history into the problem-identification stage gave us one of our biggest quality leaps in Wave’s early iterations.


Each opportunity candidate is then scored by a quality judge: an LLM that uses a rubric to write qualitative feedback and generate features for an ML classifier to predict whether a human would thumbs-up or thumbs-down the opportunity. Low-confidence candidates are filtered or demoted before a human ever sees them. Survivors are ranked to surface the highest-ROI opportunities per objective.


An actual opportunity from Wave with a proposed wireframe and execution plan for adding sources to Amplitude’s AI Feedback product


For each surviving opportunity, Wave goes deeper. It combines the problem context, design mocks from session replay visuals, and agents to draft candidate solutions. The solution isn’t always code. Depending on the opportunity, the spec might describe a pull request or a new growth experiment to run


Our interactive agents allow planning and iteration much like an agent IDE, and could eventually grow into more feature-rich solution studios with collaboration built in. This spec becomes the contract between the loop and whatever executes the work. Everything downstream depends on it being accurate and worth doing.


### Act: Execute a plan and verify outcomes


Each spec carries a complexity tag that covers the execution method, autonomy level, sizing, and location. That tag determines where the work goes next. A self-contained code fix might route to a Cursor cloud agent. Work that needs richer context goes to a Claude Code session. An experiment can route to our experiment workflow. Anything large or sensitive that requires more judgment can be routed to a human. The agent loads the spec through MCP and starts from full context.


Autonomy is tunable. On Amplitude’s own dev surfaces, straightforward fixes are pre-drafted and ship with human approval once they clear our agentic code reviews. Everything else (net-new features, sensitive surfaces, and our beta customers’ work) is routed for human review or can be pushed to an issue tracker like Jira or Linear as a recommendation. The important thing is that humans decide how much the loop does on its own, and you can raise that threshold as you build trust with the system.


Before a pull request reaches a human, a separate review agent takes it. The agent pulls the original spec, checks the change against the acceptance criteria, runs the modified code in a sandbox, records a session replay of the change, and posts a structured review with verification GIFs for frontend features. Most agent failures aren’t “the code doesn’t compile,” since CI catches those. Instead, they reflect a reality where the agent confidently shipped the wrong thing, and that’s what the review agent catches.


### Learn: Track the effectiveness of the outcome


Once the PR merges, the loop tracks outcome metrics and guardrails named in the spec. An Amplitude PR review agent checks every change for missing instrumentation and offers to fill the gaps, so the events you need to judge success are actually there.


Our opportunity manager will automatically track deployments and move them from “Shipped” to “Measured” once sufficient statistical power has been achieved, keeping an eye on each feature and producing a status summary and notifications. The good, the bad, and the ugly will all be tracked on your behalf and used in future loops to double down on successes and move away from failures.


The signal that makes the loop compound is the context you build as you continue working with Wave. We capture it throughout: the product areas you set, the opportunities you approve or reject, the plans you override and how, and the freeform notes and thumbs you leave along the way. Automated reflection steps distill this into per-product-area memories, your team’s preferences, what works, and what doesn’t.


All of this context is captured and read back on the next run when Wave generates and specs opportunities. Each cycle ships impact and updates those memories, so every subsequent run is better-informed than the last.


## What using Wave every day actually looks like


Builders on our agents team interact with Wave in three ways:


- Most product managers start in the Amplitude UI, where the Opportunity feed shows a ranked queue (one per product area) that the team checks every morning.
- Others skip the UI and pull the same context through our MCP server. An engineer can ask, “What opportunities should I work on for improving Global Agent quality?” in their Cursor or Claude Code session and get the ranked list back with full evidence and specs attached.
- A few power users run automations on those MCP tools, watching for new high-confidence opportunities and kicking them off the moment they appear, with no human in the loop until review.


Once an opportunity surfaces, a PM, engineer, or agent picks it up and digs in, pulling on every signal that can confirm or discard it. Take this bug that Wave caught in our own product: “Clicking stop to halt a running agent response didn’t stop it cleanly. It deleted the whole session.”


It’s a good example of why no single signal would have been enough. Session replay caught the behavior first. The agent watched users click stop, lose their thread, and abandon the session. Customer feedback gave it a voice, with seven-plus mentions across Slack, Zendesk, and in-product surveys, all clustered around chat threads that vanished after being stopped. Analytics sized it: Roughly 30% of those sessions were abandoned within 60 seconds.


On their own, each source was suggestive, but together, they formed a conclusive story. The opportunity landed near the top of the feed titled, “Agent sessions failed after clicking the stop response button.” It carried all three citations, a one-line problem statement, a proposed spec, and a “in review” status.


From there, an agent drafted the spec and routed it to a Cursor cloud agent, loaded with the full context of Wave’s investigation. The agent opened PR #102090: 334 additions and 54 deletions across 4 files. Claude and Cursor reviewed it before our lead engineer reviewed, approved, and merged. Post-merge, the outcome metric tracked back to the originating opportunity.


Now picture that same path running across every product area at once. That’s dozens of opportunities investigated in parallel, specs drafted overnight, low-risk fixes clearing agentic review and shipping on their own, while bigger features and growth experiments wait in the feed or get pinged to the right person in Slack. The team’s job shifts from finding the work to deciding what to greenlight.


## Why this self-improving loop works on Amplitude data


The loop is only as good as what it can sense, which is why we built it on Amplitude rather than on code signals alone. Sentry and GitHub Issues can tell you what broke, but they say nothing about whether it impacted users. Closing the loop requires modeled, queryable product data, and that is the surface Amplitude already owns.


Our agent, Wave, reads across several modeled surfaces:


- **Product analytics** (funnels, retention curves, anomaly detection, KPI movements) are the “what happened?” layer.
- **Customer feedback** , pre-categorized and deduplicated, surfaces complaints, feature requests, and top themes before they reach the metrics.
- **Session replay** is the “Why?” layer: rage clicks, hesitation, exit patterns, and full user-journey analysis.
- **Error logs and web vitals** cover network failures, JS errors, click-handler regressions, and performance drift.
- **Agent traces** from[Agent Analytics](https://amplitude.com/blog/agent-analytics) expose tool error rates and failure patterns, which matter more and more for teams building agents.
- **Experiment results** tell the agent which past changes moved which metrics, which becomes the prior for estimating impact.


We also let the agent run web searches to understand the competitive landscape, and connect external MCPs as well as “bring your own” custom agents that we’ll be shipping soon. Every team’s own context wires straight into the system. Different surfaces matter at different stages: a zero-to-one product may lean on feedback and competitive research, while a high-volume product may lean on analytics and performance data, and the system dispatches agents in whichever direction fits.


The argument for modeled data is precision. Point an agent at six raw log streams with no schema, and it produces an order of magnitude more candidate problems at a fraction of the precision. A taxonomy and an event graph let it ask, “What’s broken in the checkout funnel?” rather than “Find anomalies in this firehose.” Modeled data is also what grounds the recommendations. Every opportunity includes source backlinks (chart IDs, replay IDs, feedback IDs), so the executing agent and the human reviewer can both check the work.


There is a deeper reason this matters. A single signal is a guess. Three corroborating signals are a decision. Without that corroboration, an automated system behaves like an ant: locally reactive, brittle, able to handle only simple, high-certainty tasks.


With Wave, the system behaves more like an octopus, integrating signals in parallel, reconciling the ones that disagree, and forming a higher-order judgment about what actually matters. That is the difference between reacting to noise and acting on understanding, and it is what modeled data gives you.


## What’s working (and what isn’t)


After a few months, three things are clearly working.


**1. The idea resonates, and so do the opportunities.** Almost everyone we show Wave to wants a self-improving product to exist, with an agent that can proactively sense and fix what’s wrong. Beyond the concept itself, Wave actually finds the right opportunities. Almost every builder who reviews the feed says the problem statement is right and the opportunity is something they’d work on. The customer previews we ran over the last few weeks produced the same reaction on both fronts.


**2. Opportunity identification is real.** The system surfaces genuine bugs and friction straight from raw product data, including problems no one had reported or gone looking for. For us, the stop-response bug came from feedback. Broken checkout click-handlers came from rage-click patterns in session replay. A Global Agent KPI multi-count bug came from agent traces. Different signals surface each time, but each one is a real defect that would likely have gone unnoticed without Wave watching.


**3. Agent performance on fresh repos is close to solved** . During AI Week, one of our engineers, Eric Carlson,[pointed the loop at a fresh, low-stakes app](https://amplitude.com/blog/ralph-loop) and walked away without intervening. It shipped 101 features in a week: slope-angle overlays on 3D terrain, a physics-based avalanche runout simulation, and even a mushroom-foraging prediction model that the agent decided to build on its own after doing competitive research. Each feature came with a browser-recorded GIF proving it worked end to end. On a greenfield codebase, the loop is far closer to solved than on a mature one.


Four things are still hindering the end-to-end ship rate, and this is where the system’s credibility lies.


**1. Analysis and citation quality.** Session replay analysis is noisy, and chart and feedback references go stale. Today, you still have to re-verify the citations before acting on them, which defeats part of the point. We are tightening this using a quality judge (80% F1 on thumbs up/down) that acts as a hard gate and an eight-mode override taxonomy (Evidence Not Sufficient, Inaccurate Citation, Product Area Unclear, Feature Already Shipped, Bad Strategy in Plan, Wrong Repos Selected, Non-Coding Agent Task, Not Worth It) that feeds back as labeled training data every time a human rejects something. The discriminator is improving quickly.


**2. Knowing what’s worth building, and how, is subjective.** Even when the evidence is accurate, deciding whether an opportunity is worth doing and what the right approach is comes down to judgment and taste. That call draws on things outside your product data: the existing backlog, what comparable products and competitors are doing, web research, hard-won best practices, and a specific team’s sense of what matters. We feed all of that in, but it is the most variable part of the system. Getting it consistently right across different teams and tastes is very much a work in progress, and it is where the “Bad Strategy in Plan” and “Not Worth It” rejections still cluster.


**3. Codebase and environment resolution is slow.** Finding the right repo, recreating a working dev environment, and confirming whether a bug is still live or already fixed are all major time sinks. “Feature Already Shipped” is one of the most common rejections today because the agent proposes a fix for something that already exists. We are addressing it with a “What’s currently live” context channel in every spec, plus per-product-area codebase indexing.


**4. The human-in-the-loop funnel is the hardest.** Even with a perfect problem and solution spec, team processes change slowly, and structures still need to shift toward small, autonomous teams with high agency over their product. Shipping a PR to a mature codebase requires heavy review, manual iteration, and enough domain knowledge to sign off on production code. Our end-to-end ship rate in our own codebase is still in the low single digits (around 11%) compared to a “would do” rate closer to 50%. It’s well short of what the fresh-repo experiments suggest is possible. Tunable, automated PR review agents have unblocked much of this without raising many production bugs, which raises an obvious question: Can we apply the same learning loop to tune the reviewers themselves?


We are publishing these numbers and learnings on purpose. A system that recommends what to build earns trust only by being honest about how often it is still wrong. This is only the first or second inning.


## What we’re learning from early customers


Every company has its own configuration of engineering systems, team processes, and ways of reasoning about its products. We went to our forward-looking customers with eyes open, knowing we had to meet them where they are, across a wide diversity of build environments. The goal was never to disrupt how customers build, but to show two things: (1) velocity and quality compound with the number of iterations and the use of data to drive decisions, and (2) if you align your system with the exponential progress of AI, each new generation gives you more for free.


So what are we learning from early customers?


- **Some teams are more ready for this than others** . We expected people to jump at the chance to point coding agents at their work, but many are rightly more conservative. Some are experimenting with small teams on lower-stakes internal software, while others want to run as autonomously as possible. That spectrum is wide, and it pushed us to build flexible configuration, steerability, and gating into Wave rather than a single fixed workflow. To learn alongside these teams, we have started embedding more deeply with a lighter-touch forward-deployed model to learn, transform, and ultimately accelerate our partners.
- **Two things shape how a builder works with Wave.** The first is how close you sit to the codebase. The closer you are, the likelier you are to take direct action. For instance, an engineer will take an opportunity straight into Claude or Cursor and run with it, while a PM is more inclined to triage the opportunity to Jira or Linear for someone else to pick up. The second is how AI-native you are. The more fluent you are with agentic systems, the more comfortable you are handing work to agents, experimenting with automations, and taking the kind of risks that compound—and the more impact you’ll pull out of a self-improving product system like this. We’re building for both axes and supporting everyone along them.
- **The compounding is real once started** . Teams that get through one or two loops get through the next dozen much faster. Getting through the first loop is very human. You have to confront a different kind of handoff, let go of some preconceived notions, and start thinking in terms of feedback and steering rather than direct control. But once the system aligns and starts spinning, the build rate is remarkable.


That last shift, from controlling the work to steering it, is the heart of what we’re co-developing, and it’s different for every team. If you want your product to get better faster than your team can manually make it, that’s what we’re building. We’re inviting a small group of design partners to our closed beta.[Join us here](https://amplitude.com/wave) .
