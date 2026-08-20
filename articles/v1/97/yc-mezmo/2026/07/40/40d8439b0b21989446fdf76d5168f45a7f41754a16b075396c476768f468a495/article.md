---
schema_version: "1.0.0"
document_id: "40d8439b0b21989446fdf76d5168f45a7f41754a16b075396c476768f468a495"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/builder-in-the-loop-what-production-agents-were-missing-before-aura"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:e9e363b6857127f7b7efbd87203c5fb249482bf1b531c81618746db6b07cc4ad"
---

# Builder in the loop: what production agents were missing before AURA

Builder in the loop is a Mezmo interview series with the engineers, product leaders, and operators shaping AURA. Each installment looks past the product layer to explore the decisions, tradeoffs, and lessons involved in building agents for real production work.


This installment features Mike Shearer, the engineer who built AURA and, until recently, its only developer.


AI agents are easy to believe in when the task is small.


Ask one to summarize an incident, inspect a few logs, or suggest a next step, and the answer can look convincing. The problem starts when the work gets bigger. More tools. More evidence. More services. More opportunities for the agent to miss something while still sounding certain.


That is the problem Mike set out to solve with AURA.


[AURA](https://www.mezmo.com/) is an open-source agent harness for production operations. It runs server-side, connects to the systems a team already uses, retains useful context, and divides complex work into focused tasks that can be inspected and evaluated.


Mike’s experience with AI began before the current generative AI wave, supporting teams that built and tuned their own models. That work taught him that the hard part is rarely the model alone. It is everything around it: what the model sees, which tools it can use, how the work is divided, and how a team knows whether the answer can be trusted.


> “My job isn’t hype. It’s to solve friction.”


AURA came from that belief. As the system developed, Mike and the team turned real misses into benchmarks and built a more rigorous process for testing AURA against complex production work.


## **Three ideas behind AURA**


Three principles shaped AURA from the beginning.


First, the difficult part of building a useful AI system is often context engineering, not application code.


Teams need to decide what information the model receives, which tools it can reach, and how a workflow should fit together. Mike wanted AURA to be low-code in a specific sense: changing a model, tool, prompt, or workflow should not require writing a new application just to test the idea.


> “Iterating over the code part of things usually isn’t what you’re tuning and where the friction goes.”


Second, production agents need to operate somewhere other than a chat window.


There were already assistants that worked well when a person sat in front of them and typed a question. Mike needed something different: an agent that could run remotely inside a team’s environment, retain useful state, respond without constant supervision, and connect to the systems the team already operated.


Third, one agent cannot absorb unlimited context without losing quality.


As an investigation grows, so does the amount of telemetry, service history, tool output, and operational context involved. Orchestration helps by dividing that work into focused tasks and bringing the results back together.


Those ideas are more familiar now. When Mike began building AURA, he could not find an existing tool that combined them in the way his team needed.


> “Especially at that time, nothing really existed. It was a really obvious necessity for us to build something.”


Mike says he would have been happy to adopt something off the shelf. The team looked, but the available tools were too far from the server-side, headless system he had in mind.


Building[AURA](https://github.com/mezmo/aura) stopped being a preference and became the practical path forward.


## **Coffee, ginger chews, and parallel work**


We started with a few icebreakers about Mike’s working habits and the tools he relies on most.


Mike’s debugging habit starts with coffee, although he drinks it too consistently to consider it much of a ritual.


His more distinctive habit came from his father, a machinist who chewed Altoids while working through difficult problems. Mike’s version is strong ginger candy. When a benchmark stalls or the work stops moving, the jolt helps him stay focused.


The tool he cannot live without is TMUX.


Running coding agents and AURA sessions in parallel creates a coordination challenge, so Mike rebuilt his setup to keep concurrent work visible in one place.


That preference for visible, well-scoped parallel work also shows up in how Mike approached AURA’s orchestration.


## **Why one agent is not enough**


Mike was drawn to orchestration early.


The premise was straightforward: break a large problem into focused context windows and give each agent only the evidence and tools needed for its part of the job.


One worker might investigate telemetry. Another might inspect deployment history. Another might validate the emerging explanation. A coordinator then brings the findings together.


Mike describes this as a saturation problem.


As more instructions, logs, metrics, incident history, and tool output accumulate in one context window, the response may continue to sound polished even as its quality declines. The agent can overlook evidence, misunderstand a dependency, or settle on a conclusion before it has gathered enough information.


It is similar to a person working across too many open tabs. The work is technically still visible, but important details start getting missed.


That becomes especially dangerous in production, where the largest incidents naturally involve the most context and the highest stakes.


The failure Mike fears most is not a visible crash.


> “It’s an answer that is wrong or missing something that is very confident.”


He would rather see an agent fail loudly than produce an explanation that looks complete enough to accept.


> “I much prefer the fail hard than the answer that looks right enough that you accept it or you get kind of comfortable with it.”


Orchestration is one way to reduce that risk. Each agent gets a narrower job, a smaller set of evidence, and a clearer definition of what it is expected to solve.


That does not make the output automatically correct. It makes the work easier to inspect, evaluate, and improve.


## **Building orchestration against real failure modes**


As[AURA’s orchestration](https://www.mezmo.com/blog/why-sre-agents-need-orchestration-not-just-more-tools) layer developed, Mike and the team tested it against increasingly complex incidents.


One of the[most useful early findings came from Tony Rogers](https://www.mezmo.com/blog/builder-in-the-loop-tony-rogers-on-stress-testing-aura-before-production) , an SRE who had worked closely with the project from the beginning. In one investigation, AURA found a great deal of relevant evidence but did not capture the full scope of what was failing.


The result reinforced the problem Mike was already designing around: an agent can produce a strong, confident explanation while still missing part of the picture.


Rather than treating the miss as a one-off, Mike turned it into a repeatable test.


That case became part of AURA’s internal benchmark harness, along with other examples where too much context, unclear task boundaries, or incomplete validation reduced the quality of the result.


> “We just took all examples of them and I built them into tests.”


The benchmark harness gave the team a more disciplined way to improve orchestration. Instead of changing a prompt or workflow and judging the output by feel, they could run the same difficult scenarios repeatedly and measure whether the system had improved.


It also helped reveal weaknesses that simpler tests did not expose.


> “I feel like I’m less stabbing around in the dark now, because it kind of forced me to get organized.”


As AURA improves against the current test cases, the team makes them harder by adding more context, more ambiguity, and more competing explanations.


The goal is not to build an agent that claims it will never fail. It is to build an orchestration layer that can divide complex work, surface the evidence behind its conclusions, and improve against the failure modes the team sees in real use.


## **A shared agent harness for production teams**


Mike sees the clearest fit for AURA among SRE, platform engineering, and development teams responsible for production.


The main differentiator is that AURA runs server-side.


A team can deploy one shared AURA server rather than depending on agents running on individual laptops. People with different permissions can use the same deployment while requests are routed through the controls the team defines.


That creates a shared, auditable operational workflow. Teams can inspect what the agent attempted, which tools it used, why it called them that way, what evidence it found, and where an investigation went off course.


Mike describes AURA as requiring the model to “divulge its own intent” during orchestration. In practice, that means recording the plan and actions clearly enough that an operator can understand what the agent was trying to do.


> “If it ever goes a little bit off the rails, or as you’re testing it, you’re like, ‘Why did it do that?’ Well, you can at least tell what it thought it was doing.”


[AURA](https://github.com/mezmo/aura) is also deliberately configurable.


Teams can define their own models, tools, prompts, workflows, and approval rules rather than adapting operations to a fixed vendor process. Through MCP, they can connect AURA to the telemetry, infrastructure, source control, and knowledge systems they already use.


For smaller teams without a dedicated SRE function, the same server-side model provides a shared place to investigate failed deployments, service degradation, and infrastructure problems without depending on whose laptop is available.


> “You don’t have to worry about whether it works on my machine, or his machine, or whatever. You can deploy this server-side and just keep hitting it.”


The goal is not to replace the people responsible for production. It is to give them an agent environment they can configure, inspect, and operate by their own rules.


## **The complaints are the signal**


Mike spends a surprising amount of time reading complaints about AI tools.


He looks at Reddit and, more selectively, X for posts from developers and companies that invested heavily in AI and found that it did not work the way the case studies promised.


> “What are people actually complaining about is a heck of a lot more interesting to me than what a blog post is excited about.”


The interest is not cynicism for its own sake.


Mike’s job is to remove friction, and friction often appears more clearly in negative feedback than in polished success stories. People describe unreliable answers, forced workflows, missing context, and AI tools that create more work than they remove.


Those complaints give him a map of where the product needs to improve.


> “We aren’t going to experience friction the same way that everybody else is.”


AURA is being built for developers, SREs, and platform engineers who may already be skeptical of AI that has been imposed on their work without making it better.


Mike sees that skepticism as useful.


When someone is frustrated by an agent that cannot explain what it did, repeatedly forgets context, or produces a confident answer that does not hold up, that is not resistance to overcome. It is a product problem to solve.


## **The AURA workflow Mike runs every night**


One of Mike’s most useful AURA workflows is not an incident investigation.


It is a knowledge system for his own coding agents.


He built an LLM-assisted wiki for his coding agents. Each session leaves behind useful context for the next one. Overnight, an AURA job reviews that information, consolidates overlapping notes, removes unnecessary detail, and rewrites the indexes agents use to find the right material.


The next session begins with curated context instead of a blank slate.


> “Every new session, it’s curated and gardened in a way that it’s very efficient to consume. I can fire a handoff, go get my coffee, and the next session I open, I don’t have to re-explain everything.”


The workflow saves Mike from repeatedly deciding which details might matter later. It also reduces the number of tokens each agent needs to spend reconstructing the history of the project.


> “I basically don’t have to think about what information is going to be important later. It’s all there. It’s just curated so that it’s very easy to find.”


The organization happens while he is away from the keyboard, so maintaining the system does not consume more time than it saves.


It also became a practical way to dogfood AURA while using the harness to accelerate its own development.


## **What success looks like for teams using AURA**


For Mike, success is not measured by how autonomous AURA appears.


It is measured by whether the team responsible for production can move faster without giving up control.


A useful[AURA](https://www.mezmo.com/aura) deployment should reduce the time engineers spend gathering context by hand. It should help them investigate incidents with the telemetry, service history, and tools they already use. And it should leave enough of the work visible that an engineer can inspect the evidence, challenge the conclusion, and redirect the agent when needed.


For a larger SRE or platform team, that can mean a shared operational workflow that does not depend on one person’s laptop or one vendor’s fixed process.


For a smaller engineering team, it can mean getting useful help with production problems even when there is no dedicated SRE watching the system full time.


The first experience should also be simple.


Mike wants a team to be able to deploy AURA in Docker, start with a strong out-of-the-box configuration, connect it to something real, and find it helpful before completing a more extensive setup.


> “I really just want somebody to be able to take it and say, ‘Hey, I’m just going to deploy that in Docker and check it out, run a few queries,’ and be like, ‘Oh wow, that actually was helpful. I haven’t even set it up all the way yet.’”


That is the practical bar.


AURA should remove friction before it adds more. It should take on the context gathering and investigation work that slows teams down while keeping the final judgment with the people responsible for production.


Run it against a problem your team already understands. Inspect the tools it used, the evidence it found, and how it reached the result.


The point is not to trust an answer because it came from an AI agent.


It is to give the agent enough context, structure, and oversight that your team can check the work and decide what happens next.


AURA is open source under Apache 2.0 and runs in Docker.


Explore the repository:[github.com/mezmo/aura](http://github.com/mezmo/aura)


‍


## **About Mike Shearer**


[Mike Shearer](https://www.linkedin.com/in/michael-shearer-90711175/) is the engineer who built AURA and served as its sole developer until the engineering team expanded in 2026.


His experience with AI began before the current generative AI wave, supporting teams building and tuning specialized language models in-house. That background shaped AURA’s central premise: the reliability of an AI system depends not only on the model, but on the context, tools, orchestration, and evaluation surrounding it.


Mike is currently focused on strengthening AURA’s benchmark system, advancing its orchestration layer, and making it easier for the open-source community to extend.


‍


‍
