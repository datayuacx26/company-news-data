---
schema_version: "1.0.0"
document_id: "0932aa9b5f7f80ddcfd57ff75c6ca482cc6ad3ea46ef80f21919e1f55241734e"
company_key: "yc-warmly"
company: "Warmly,"
source_id: "yc-warmly-news-import-09260ba94cb0"
canonical_url: "https://www.warmly.ai/p/blog/agentic-gtm"
published_at: null
first_seen_at: "2026-07-22T19:22:02.099634+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:1bfa2df5d76e38763a5c4c40ab2e85ee64850d45a6b6440982ba77d11c3075f9"
---

# Agentic GTM: The Future of Sales, Marketing, and Revenue Agents

[Book a Demo](https://www.warmly.ai/p/book-a-demo)


## TLDR


- AI made execution effectively infinite. The bottleneck moved from human productivity to **context engineering** and **AI memory** .
- Five things compound at the same time: pre-training scales, post-training scales, test-time scales, agentic scales, and synthetic data scales. Together they explain why the curve does not bend.
- Sales and marketing collapse into one function. Marketing has always been "scaled-out sales." When AI makes 1:1 sales free, the line between the two erases.
- The traditional **AI SDR** was a volume play. It failed. Signal-based, marketing-owned outbound replaced it.
- The CMO seat is becoming the CRO seat. The team that runs the website, the agents, the signal layer, and the buying experience is marketing.
- AEO and GEO are the new market allocation layer. Buyers do not start at Google anymore. They start at an AI answer with a recommendation already inside it. AEO pulls the market toward truth in a way Google never could.
- Permissioned memory is the new moat. Not the model, not the data, not the workflow. Trust plus context plus the right to act.


---


## The Two Things That Just Happened


Meta laid off 20% of its workforce last quarter. Google did its own round. Microsoft, Salesforce, Amazon, every Big Tech, all trimming hard.


Everything we know about software, workflows, and functional roles is collapsing into a more natural state of flow thanks to AI.


For the last decade, most business software was built around rigid workflows.


A salesperson lived in Salesforce, Outreach, Gong, LinkedIn, and Slack.


A marketer lived in HubSpot, Canva, Webflow, analytics tools, and ad platforms.


A customer success manager lived in call recordings, product analytics, support tickets, spreadsheets, and CRM notes.


Each tool had a fixed shape. But the actual problems inside a company do not have fixed shapes.


A company does not wake up and say, "I need to send 200 more emails." It says, "I need more pipeline." "I need to retain this customer." "I need to take market share."


Similarly, the old GTM stack was built around departments, not outcomes.


Marketing had marketing automation. Sales had CRM and sales engagement. RevOps had routing, enrichment, attribution, and reporting. Customer success had support tickets, call notes, product usage, and renewal workflows.


But the customer does not experience your company in departments. The customer experiences one journey. They see an ad. They visit the website. They read content. They talk to ChatGPT and Claude to compare vendors. They talk to sales. They buy your product.


Organizations split that journey into departments because humans needed boundaries to manage the work.


But AI does not need the same boundaries.


Once agents can read signals, retrieve context, recommend actions, and execute workflows, the organizing principle stops being the department and starts being the outcome.


---


## What Is Agentic GTM?


Agentic GTM is what happens when AI agents handle most go-to-market execution and humans operate the strategy layer above them. Instead of teams of SDRs, demand gen ops, content writers, ad operators, and BDR managers each running a slice of the funnel, you get a small group of strategists pointing a network of agents at the right accounts, with the right messages, at the right time. The agents do the work. Humans set goals, hold trust gates, and steer.


The simplest definition: agentic GTM replaces the workflow layer of B2B revenue with autonomous decision systems. The customer does not see "sales" or "marketing." They see one continuous experience that knows them.


This is the point I keep coming back to when I think about how to position Warmly, a company that services GTM teams, myself as a leader, or just as a human preparing for the inevitable future.


The model I keep arriving at is this: software is moving from rigid tools into fluid problem-solving loops.


There are four AI scaling laws plus one data law that explain why this is happening:


Pre-training scaling. Post-training scaling. Test-time scaling, or long thinking. Agentic scaling, or AI multiplying itself. And synthetic data scaling, which feeds all four.


Each one opens up a new dimension where more compute, more data, or more system design creates more capability. Together, they explain why AI is moving from a text generator into a new operating layer for problem solving. And they explain why the curve does not bend.


---


## Pre-training scaling


Pre-training is the very expensive procedure of teaching a model general intelligence through historical next-token prediction. Input context and predict what comes next.


One mental model: pre-training is roughly 95% of broad knowledge and compute. Post-training is the smaller but high-leverage shaping phase.


This is the original scaling law: train on massive amounts of text, code, images, video, and structured knowledge, and the model develops broad general intelligence.


In pre-training, foundational labs choose domains that have strong verifiability, which means it is easy to confirm whether the answer is right or wrong given an input and output. Coding is a good example because you can see if code compiles or works to specification.


They also choose domains they want to do well in because they believe those domains will provide the most economic impact. They do not need AI to be good at everything. Training is expensive, and too many domains leads to a heavier, more expensive, higher-latency model.


This is what leads to the jaggedness of models. They are good at some things and not others.


If the domain you work in is operating in the circuits that are part of the foundational model's reinforcement learning loop, your domain flourishes with AI. If you are operating in a domain out of the data distribution, the model will not perform as well.


But we have no idea what the foundational labs are training the models on. They don't give us a manual. We know they care about certain domains like math, science, and coding. But for domains that have low verifiability, are less important to the foundational model labs, or are highly niche, this is where post-training comes in to round out the long tail.


---


## Post-training scaling


Post-training is a less expensive procedure and more fine-tuned to a specific task or how you want a job to be done.


Models get more useful after pre-training by learning from feedback, examples, preferences, synthetic data, tool traces, and real-world outcomes. This is how a raw model can create amazing coding agents, support agents, or a GTM agents.


At Warmly for example, we fine tune our own models for our AI autopilot agents to reason through the next best GTM actions, how to write good emails, how to handle objections, and how to have human-like and effective conversations in the context of GTM in your organization.


To see if your post-trained model is good, you can recreate the world GTM model at the time and see how many accounts would convert to the next stage given context around these accounts at the time.


The buyer got an email, saw an ad, the company was hiring, the account was ICP or not. Then you see whether the model can justify the right reasoning.


Both pre-training and post-training have to do with fine tuning the model itself by feeding in input, output, verified outcome, and feedback data, then adjusting the actual weights of the model.


This is different than updating the system prompt with "learnings" since we're affecting the "brain" or the model directly. But it also means we save on context window tokens at test-time.


The next scaling law happens while the model is working.


---


## Test-time scaling, or long thinking (and the rise of context engineering)


Test-time scaling is the idea that models get better by spending more compute while solving the problem, not just during training.


Pre-training and post-training happen before the model is deployed. The model weights are updated. The model becomes generally smarter or more useful.


Test-time scaling happens at runtime. The model weights do not change. Instead, the model is given more time, more context, more tools, more attempts, and more verification while it is working on the task.


For simple tasks, the model can answer quickly. If you ask it to rewrite a sentence or summarize a paragraph, it does not need much thinking.


But for high-value work, the model needs to reason, retrieve, plan, compare, verify, and sometimes try multiple paths before choosing an answer.


A shallow AI system might see: Account visited pricing page. Send email.


But a test-time scaled system thinks longer.


**What happened so far:** Who is the company? Are they in our ICP? Have they talked to us before? Which pages did they visit?


**What should we do:** Should we trigger chat, notify an AE, launch outbound, suppress the account, retarget them, or wait?


**How should we do it:** What message should be sent?


**Who should do it:** Should the AI execute automatically or ask for human approval?


This is where people misunderstand context windows.


A one-million-token context window sounds big. It's not.


Let me show you the math. A single week of GTM activity for a mid-market B2B company is something like:


- 50,000 website sessions × 200 tokens of behavioral data each = 10M tokens
- 10,000 emails (sends + opens + replies) × 500 tokens = 5M tokens
- 500 sales call transcripts × 5,000 tokens each = 2.5M tokens
- 2,000 CRM activity records × 200 tokens = 400K tokens
- 1,000 internal Slack threads × 1,000 tokens = 1M tokens
- Enrichment data, intent signals, product usage, support tickets = 5M+ tokens


Conservatively 25-30 million tokens of activity. Per week. The context window is 1 million.


So the agent that is supposed to make decisions about your business literally cannot hold your business in its head. It has 3-4% of the relevant context at any given moment.


If you dump every website visit, CRM note, email, call transcript, support ticket, and product event into the prompt, most of it will be irrelevant. The hard part is not having more context. The hard part is selecting the right context at the right moment.


But what the industry is starting to figure out is that the next context window recursively explores.


The agent sees a problem. It decides what it needs to know. It searches memory. It retrieves the relevant context. It calls tools. It writes code to inspect a dataset. It spawns sub-agents to analyze pieces of the problem. It compresses what matters. It updates memory. Then it continues.


**Recursive context:** The model is not storing all context inside itself. It is learning how to find context, write down what matters, preserve state outside the prompt, and call itself again with better information.


The context becomes a living system.


This discipline has a name now. Context engineering. It is the new prompt engineering and a much bigger surface area.[Anthropic is publishing on it.](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) LangChain has a guide. Weaviate is shipping infrastructure. The term went from invisible to ~3,000 monthly searches in a year, almost entirely driven by AI builders realizing that prompts do not scale but context does.


Context engineering is the work of deciding what your AI system remembers, how it stores those memories, how it links them, when it surfaces them, and how it forgets the irrelevant ones. Prompt engineering optimizes a single conversation. Context engineering optimizes the system's intelligence over months and years.


In GTM, this is the unlock. To make a high-quality decision, the AI needs an AI memory layer that is searchable, retrievable, and constantly updated by what is happening across the business. It needs to know which accounts matter, which signals are real, which actions worked, which objections came up, which messages converted, which workflows are safe, and which moments require a human.


This is what we call the[context graph](https://www.warmly.ai/p/blog/gtm-brain-own-decisions) . The state clock (who, what, where, how much) is the CRM. The event clock (why, when, with what reasoning) is the context graph.


In GTM, the expensive mistake is not that an AI writes a bad sentence. The expensive mistake is that it picks the wrong account, contacts the wrong person, uses the wrong context, misses the real buying signal, or automates a workflow that should have gone to a human.


Long thinking reduces these higher-order context mistakes. It lets the system retrieve relevant context instead of using all available context, reason through the account state, compare possible actions, use tools to fill missing information, check whether the recommendation is safe, verify that the action matches business rules, and decide whether to act automatically or route to a human.


This is also how the system compounds.


Every agent run creates a trace: what the agent saw, what context it retrieved, what tool it used, what action it took, and what happened afterward. Some traces are bad and should be discarded. Some become negative examples. Some are excellent. The best traces become memory.


The loop becomes: agent does work → work creates trace → trace becomes memory → memory improves future context → future agents perform better → more traces are created → the system compounds.


This is different from a human organization. In a human org, knowledge is fragmented across people. One SDR learns an objection. One AE learns a buying trigger. One CSM learns a churn pattern. One marketer learns a message that works. Then the company has to mobilize that knowledge through meetings, enablement docs, Slack threads, training sessions, managers, and repetition.


Dissemination becomes a bottleneck for the organization.


Agents change that. If the system has shared memory, shared governance, shared tools, and shared orchestration, every agent can benefit from the learning of every other agent.


But this only works if the memory is governed. You do not want bad learning to compound or wrong assumptions to propagate.


So the future is not just recursive agents. It is recursive agents with governed memory.


---


## Synthetic data and experience data


At first, people thought AI scaled mainly through pre-training: bigger model, more human data, more compute. Feed the model the internet, books, code, papers, videos, and structured knowledge, and it gets smarter by learning to predict what comes next.


Then the industry hit the obvious question. What happens when we run out of high-quality human data?


There was a panic around pre-training. If the model has already consumed most of the useful internet, then maybe the original scaling law starts to slow down. Maybe AI progress hits a wall.


That misunderstands what data is becoming.


The next wave of data is starting to come from synthetic data generated to fill the gaps in existing human data. The powerful version of synthetic data starts with some form of ground truth, then uses AI to expand it.


For example, you can start with a verified coding problem and solution. Then an AI can generate thousands of variations of that problem, different edge cases, different frameworks, different bugs, different constraints, and different explanations. Another system can run the code, check the tests, reject bad examples, and keep the good ones. Now you have created far more high-quality training data than humans could have manually written.


The deeper point. Most of what we call "human data" was already synthetic in any meaningful sense. We invented language. The substrate of human knowledge is something humans constructed. When AI generates more of it, it's just continuing the trend. There is no clean line between "natural" data and "synthetic" data.


The same pattern works across many domains. In a GTM system, this can be data produced by the revenue team, the world model at the time, and the decision traces from agents operating inside this dynamic environment. Actions and results are fed back into pre-training and post-training models to further refine future decision-making.


An agent can attempt a task, fail, retry, and preserve the successful path as training data.


Synthetic data is knowledge that has been compressed, structured, explained, and regenerated so another intelligence can learn from it. AI can now do this at massive scale.


But the key is verification. Bad synthetic data creates garbage. Verified synthetic data creates a flywheel. The system can generate examples, score them, filter them, and keep only what is useful. In code, the verifier is whether the tests pass. In math, it is whether the answer is correct. In GTM, it is whether the action led to a reply, a meeting, pipeline, retention, expansion, or revenue.


**Feedback loop:** Some of the data the model generates in production (post-training) gets used as input to the next pre-training run. So the system that just made a sales decision today is contributing to the model that makes sales decisions next year. Each cycle compounds.


AI is moving from a static model trained on historical data to a living problem-solving system that generates new data through its own work, whether in a simulated environment or in a production environment.


---


## Agentic scaling, or AI multiplying itself


Because of context limits, each agent instance can only hold so much context. But a single agent can spin up sub-agents to complete subtasks of research gathering or tool calling, each with their own context window, and then feed the results back to the main agent.


This essentially means AI can multiply itself.


Think about hiring. Hiring one human takes weeks. Hiring 30 humans takes months and millions of dollars. Hiring 1,000 humans is a multi-year org-design problem. Spawning 30 agents takes 5 seconds. Spawning 1,000 agents takes 30 seconds. The cost of growing the workforce went from a hard limit to effectively zero.


What used to require separate humans, teams, and handoffs can now be decomposed into agent loops.


In GTM, RevOps compiled the account list, research teams gathered context, SDRs wrote the outreach, and managers checked the work. Now a single AI system can kick off the job, spawn the right agents, coordinate the work, and route the high-context decisions back to a human.


This is why the future of work does not look like every person clicking through more apps. It looks like humans defining the problem, AI systems decomposing the work, agents executing the repeatable loops, and humans approving the moments where judgment, trust, or risk matter.


But the moment AI becomes a team of workers, the enterprise bottleneck becomes: will the company let the AI act inside real systems?


---


## If AI is so great, why is it not working?


Across enterprise AI deployments, the pattern is becoming obvious. Companies spend millions on AI software, token spend, and "AI-first" initiatives, but when you ask what has changed in the day-to-day, the answer is usually some version of nothing.


Reps are still not spending enough time selling. The CRM still has the same data decay. Most business workflows are not verifiable like coding.


**Most companies are still at the personal productivity layer.** Everyone wants to be AI-native, but AI-native is not binary. A company where employees use ChatGPT to summarize meetings is not the same as a company where agents can read systems of record, take bounded action, move workflows across teams, and improve future work. Both might call themselves AI-forward, but they are not operating at the same level.


The better question is not whether a company is AI-pilled. The better question is what AI can actually do inside the company. Can it see the work, or does the work still live in meetings, Slack threads, private docs, and people's heads? Can it act on systems of record, or can it only summarize what humans already wrote down? Did AI actually change how work gets done, or is the company still running the 2023 org chart with better autocomplete?


**The clearest case study is the AI SDR collapse.** The "AI replaces SDRs" pitch you saw on every billboard last year cratered.[TechCrunch broke the 11x.ai story in March 2025](https://techcrunch.com/2025/03/24/a16z-and-benchmark-backed-11x-has-been-claiming-customers-it-doesnt-have/) : $10M ARR claimed, $3M actual, 70-80% churn within months.[Lead Gen Economy's autopsy](https://www.leadgen-economy.com/blog/ai-sdr-cancellation-wave-failure-forensics/) : 50-70% of AI SDR contracts cancel within 90 days.


Volume AI SDRs failed because volume was never the actual job. An SDR is not a person who sends 200 emails a day. An SDR is a person who knows the right account to email, the right context to use, the right moment to reach out, and the right person to follow up with after a no-show. The volume is incidental. The judgment is the job.


Pure-AI SDR vendors automated the volume and called it done. The judgment layer was never solved. So inboxes filled up with AI-generated noise, deliverability tanked, brand reputations cratered, and customers churned.


Drift literally got shut down this quarter. Salesloft acquired it, repositioned the surviving pieces as a["Buyer Engagement Platform,"](https://www.salesloft.com/platform/drift) and let the rest die. The original conversational marketing tool, dead. The rules-based chatbot of 2018 cannot compete with an agent that has full account context. The category moved.


**Then comes agent sprawl.** Every employee with AI access becomes their own agent factory. One person builds a lead scoring agent. Another builds a follow-up agent. Another builds a Salesforce summary agent. At first this feels like leverage. Everyone is moving faster. But soon the company has dozens of disconnected workflows, each with its own prompts, data access, approval logic, logging, model config, and memory. There is no shared agent spine. No shared governance. No shared memory.


The content agent does not know what sales is hearing on calls. The outbound agent does not know what marketing just learned from ad conversion. None of it feeds the rep on the sales call.


A hundred brittle automations do not equal a compounding operating system. They are just another form of software debt.


**The fix has to be architectural from day one:** a shared orchestration layer on top of the existing stack with common infrastructure for ingestion, approvals, audit logs, model routing, memory, observability, and outcomes. Every new use case lands on the same platform. Every agent makes the whole system smarter. Every workflow feeds the same memory layer. Every action is tied to a measurable outcome.


The practical loop is: audit → decompose → orchestrate → route models → monitor → tune → retire → improve.


**But architecture alone is not the moat.** Once AI can write code, touch production systems, message customers, and trigger actions, the question stops being "can it act?" and becomes "should it?" Once AI can change the state of the business, trust becomes the control point.


Permission is what separates an AI that answers questions from an AI that operates inside the enterprise.


The more enterprises rely on the capability, the more they need governance. The more they rely on the governance, the harder the capability is to replace. AI makes this loop compound. AWS did not understand your company better every time you ran a workload. Microsoft's identity layer did not become a living model of how work happens inside your org. AI is different. The longer it operates inside a company, the more it learns how that company actually works. What worked. What failed. Who approved what. Which accounts matter. Which workflows are safe. Which decisions created outcomes.


That memory becomes more than data. It becomes organizational know-how. And the trusted system where that know-how compounds becomes very hard to replace.


**The next great moat in enterprise AI is not intelligence alone. It is trust plus context. Trust plus governance. Trust plus permission. Trust plus the memory of how work actually gets done.**


That is the future Warmly is building toward in GTM.


And we have already started building it ourselves. See[how we 3x'd our own pipeline in 30 days](https://www.warmly.ai/p/blog/how-we-3xd-b2b-pipeline-in-30-days) using this exact architecture: 2-person marketing team, under $30K in spend.


---


## When signal turns into action: marketing has always been scaled sales


In the old world, software mostly stored signals. A website visit, email open, ad click, form fill, CRM note, sales call, or product event all told you something. But a human still had to interpret the signal and decide what to do next.


That is why the GTM stack fragmented. One tool captured the website visit. Another enriched the account. Another scored the lead. Another routed it. Another sequenced it. Another booked the meeting. Another tracked the opportunity. Another reported attribution.


AI collapses that chain because the system can move from signal to decision to action.


That is the moment marketing automation becomes revenue orchestration.


Marketing exists because 1:1 sales is too expensive.


If I could afford to clone my best AE one million times and have each clone walk into a different prospect's office, sit down, build rapport, understand the use case, demo the product, handle objections, and close the deal, I would never run a marketing campaign again. I would never write a blog post. I would never buy a Meta ad. I would never produce a webinar. None of those things solve a problem better than 1:1 selling. They exist as compromises because cloning your best AE is impossible.


So we invented marketing. Marketing is a series of one-to-many compression schemes designed to deliver some fraction of what 1:1 selling does, but cheap enough to apply to the entire market. Brand is compressed trust. Content is compressed product education. Ads are compressed reach. Email sequences are compressed follow-up. Webinars are compressed demos. Every marketing channel is a workaround for the fact that human selling does not scale.


Andrew Chen wrote the cleanest version of this.[His bet](https://andrewchen.substack.com/p/ai-and-marketing-what-happens-next) : "With smarter AI-powered conversations, marketing will look more like sales over time, moving from 1:many broadcast to many 1:1 agents selling people over chat/phone/video. Marketing exists because 1:1 sales is too expensive, but AI is changing this by converting dollars to labor."


That is the whole thesis. AI just turned the cost of 1:1 sales into close to zero. The workaround we built (marketing) and the original (sales) are now the same thing.


The data backs this hard. 6sense ran the[most rigorous B2B buyer study of 2025](https://6sense.com/science-of-b2b/buyer-experience-report-2025/) . 95% of the time, the winning vendor is on the buyer's Day-One shortlist. Four out of five deals are won by the pre-contact favorite. First seller contact happens at 61% of the journey. Average buying group: 11 people. Bain found the same thing from a different angle.[80-90% of buyers](https://www.bain.com/insights/marketing-budgets-decisions-data-and-more-decisions-video/) have a Day-One vendor shortlist, and 90% buy from that list.


Read that again. The deal is decided before sales is on the call. Not in some deals. In 95% of deals.


[Forrester's 2025 prediction](https://www.forrester.com/press-newsroom/forrester-predictions-2025-b2b-marketing-sales/) : more than half of $1M+ B2B transactions will run through digital self-serve channels. Million-dollar deals. No salesperson at the keyboard.


In Q1 2026, Forrester spun up a brand new analyst category called[Revenue Marketing Platforms](https://www.forrester.com/blogs/converging-platforms-for-greater-efficiency-the-rise-of-revenue-marketing-platforms/) , explicitly merging marketing automation and ABM into "a single, comprehensive hub." Salesforce, Adobe, 6sense, and Demandbase were named Leaders. Forrester does not invent categories on a whim. They invent them when their clients are already buying it that way.


Sales got the leftover 5%. Marketing got the 95%. Then we kept putting most of the revenue tooling spend on the sales side of the org chart. That is the gap. That is what is closing.


### The "one brain" reframe


The AI that sidekicks the sales rep on the call is the same AI that sends the personalized email an hour later. The same AI that personalizes the website chat. The same AI that updates the CRM, builds the deal brief, drafts the contract, answers the support question, and schedules the renewal.


One brain. One memory. One context graph powering both the human-in-the-loop work and the autonomous work.


You cannot split that brain across two budgets and two leaders. The brain is one thing. The function it serves is one thing. The team it serves is one team. Sales and marketing are not merging because somebody decided to merge them. They are merging because the underlying intelligence layer is one brain, and you cannot run one brain through two org charts.


### Where this leaves the org chart


The old separation between sales and marketing was created by human bottlenecks. Sales is human persuasion. Marketing is scaling that persuasion through systems because we did not have enough humans. Marketing created demand at scale. Sales converted demand one conversation at a time. That made sense when every step required a human to read, research, write, route, follow up, personalize, qualify, demo, negotiate, and remember what worked.


AI changes the cost structure of action.


Agents can research accounts, write messaging, qualify inbound, route accounts, recommend next steps, trigger follow-up, personalize landing pages, give demos for lower-ACV products, send credit card links, monitor intent, and turn closed-won buyer journeys into training data.


So the revenue org starts to look less like a set of departments and more like a learning system.


As agent systems reach a new level of scale, marketing becomes even more leveraged because it owns the largest surface area of demand generation and demand capture. The future of marketing is not channels and campaigns. It is fleets of AI sales agents working off a single shared brain. Marketing gets full context from top of funnel to bottom of funnel: website visits, ad engagement, email engagement, intent data, account activity, sales conversations, pipeline, and closed-won revenue.


Marketing will govern the agent fleet that turns disparate data streams into pipeline. And because those loops can be tied to closed-won revenue, marketing becomes the function that teaches the system what actually converts people to buy across their unique buyer journey.


The leader who governs this function needs deep domain expertise: who we sell to, what they care about, what pain is becoming urgent, and what buying experience makes the sales conversation feel like a layup. They also need to be equipped to run the agent fleet, or have someone on their team who can.


[Spencer Stuart's 2025 CMO tenure study](https://www.spencerstuart.com/research-and-insight/cmo-tenure-study-2025-the-evolution-of-marketing-leadership) found that 65% of exiting CMOs got promoted internally or took lateral / step-up jobs, and 10% became CEOs.[Latané Conant went from CMO of 6sense to CRO.](https://6sense.com/newsroom/6sense-enhances-go-to-market-team-alignment-names-latane-conant-as-chief-revenue-officer/) She ran 100% YoY revenue growth five years in a row as CMO. Then she got the CRO seat. Same company. Same person. Bigger scope. Her trajectory used to be exotic. Now it is the predictable next move.


### Sales changes too


The best salespeople will look more like consultative FDEs for revenue outcomes. They will help customers deploy the system, build trust, navigate internal politics, connect the software to the customer's actual operating model, and make sure the customer achieves results.


In the old world, a salesperson could sell software and leave value realization to onboarding, services, or the customer. In the new world, that is not enough.


Future buyers will be moving toward AI-native companies themselves. Most GTM teams start at Level 1: using AI to pull reports, write copy, summarize calls, and automate individual tasks. Then they move to Level 3: agents handling work that was below the ROI threshold for humans, like mining negative keywords, checking broken links, cleaning CRM fields, watching closed-lost accounts return to pricing, and updating routing rules.


Level 4 is where it compounds. A campaign teaches outbound. A sales call teaches messaging. A closed-won deal teaches the next campaign.


The holy grail is Level 5: the system notices, decides, acts within authority, escalates when needed, and updates shared memory so future behavior improves.


They do not want more vendor lock-in. They need systems that generalize across their organization: their agents, their memory, their workflows, their governance, their compounding learning loop. That means vendors cannot sell vaporware into enterprise anymore. They have to deliver outcomes and build trust through relationships and deployments.


The CRO role changes too. The future revenue leader is deeply domain-specific, but also able to harness agents. Their job is not to manage sales and marketing as separate functions, but to operate a revenue learning system that hits revenue goals.


That system powers every person through the collective learning of every sales call, website visit, email reply, ad conversion, creative test, demo, objection, and closed-won deal.


It will be a while before agents replace sellers in enterprise sales because the environment is not fully observable or repeatable. The deal is political. The buyer is emotional. The timing is uncertain. The reinforcement loop is weak.


However, every seller will have their own Jarvis hooked into the GTM brain. The copilot will give them an edge on every deal. It is powered by the same revenue brain marketing uses to understand the market, generate pipeline, test messaging, learn from conversion, and build the buying experience.


With each deal, the system observes what happened. The best traces become better training data. And the next seller starts from a better version of the system.


This is the collapse. Sales and marketing do not disappear. They converge into an agentic revenue system where marketing owns the signal layer, agents execute the scalable work, sales handles the highest-trust moments, and the entire system learns from every outcome.


---


## AEO and GEO: the new market allocation layer


That collapse inside your GTM stack is the smaller story. The bigger story is what is collapsing outside it, in the buyer's discovery.


For twenty years, the market allocation layer was Google. You searched, you got ten blue links, you clicked, you compared, you decided. SEO was the discipline of being one of those ten links. Marketing teams optimized for it because that was where buyers started.


That layer is moving.


The buyer's first touch is no longer a search results page. It is an answer in ChatGPT, Claude, Perplexity, Gemini, or an AI Overview at the top of a Google search. The buyer asks a question and gets a synthesized answer with a recommendation already inside it. They do not click through ten blue links. They start from a position the model has already taken.


This is what AEO and GEO are about. Answer Engine Optimization is being the answer when the model picks one. Generative Engine Optimization is shaping how generative models talk about your category, your competitors, and you.


It is not SEO with a new name. SEO is about ranking. AEO and GEO are about being trusted enough that the model recommends you, accurately enough that the recommendation holds up, and structured enough that the model can use what you have written.


The mechanics are different. The model is not crawling for keyword density. It is reading your site, your G2 reviews, your customer case studies, your YouTube transcripts, your podcast appearances, your earned press, and your social posts. It is forming a representation of who you are, who your customers are, what problems you actually solve, and how you compare to alternatives. It then uses that representation to decide whether to mention you when a buyer asks.


The companies that win this layer are the ones that teach the model. Not by stuffing keywords. By publishing structured, specific, defensible evidence. Real customer outcomes with numbers. Real comparisons that include their own weak spots. Real positioning, not slogans. Real founders saying real things on real podcasts.


This is a strange shift for marketing teams that grew up on volume. The old playbook said publish more, rank for more keywords, capture more clicks. The new playbook is the opposite. Publish less, but make every piece dense with the truth a model can extract and trust.


The tradeoff is uncomfortable. Most marketing pages today were written to game a search engine. They are vague, hedged, full of soft claims. The model can see this. When a buyer asks Claude or Perplexity which vendor solves a specific problem in a specific industry at a specific stage, the model picks the one whose evidence is sharpest. Vague companies lose the recommendation.


There is one more dynamic that is easy to miss. AEO pulls the market toward truth in a way Google never did. You cannot keyword-stuff your way into a model's recommendation. You cannot buy your way to the top of an AI answer the way you buy AdWords. The model is allocating attention based on what looks true to it. Companies that have done the hard work of being good at what they do, and writing about it specifically, get pulled forward. Companies that built their growth on PPC and SEO arbitrage start to fall behind.


This is why I think AEO is the most important channel a CMO can be working on right now. Not because the volume is large yet. It is still small compared to organic search. But because it is the thing that determines whether the AI agent recommends you when the buyer asks. And the buyer is going to keep asking the AI agent more, not less.


Once AI allocates attention, agents execute. The answer engine recommends the vendor. The website agent qualifies. The outbound agent follows up. The market starts to operate less like a funnel and more like a routing system. The companies that win are the ones that make themselves easiest for that routing system to understand, trust, recommend, and activate.


---


## Memory, trust, and the new vendor moat


Klarna is not a perfect example, and it should not be treated as a clean story of "AI replaces everyone and everything gets better."


But it is one of the clearest early examples of what happens when a company aggressively uses AI to compress headcount, increase revenue per employee, and rethink how much work needs to be done by humans.


In 2024, Reuters reported that Klarna reduced active positions from about 5,000 to 3,800 over roughly 12 months, mostly through attrition. Klarna said its AI assistant was doing the work of 700 employees, cutting average customer service resolution time from 11 minutes to two minutes, while revenue per employee increased 73%.


Then in 2025, Klarna said headcount had dropped from 5,527 to 2,907 since 2022, technology was doing the work of 853 full-time staff, revenue had increased 108%, and operating costs stayed flat.


Again, not a perfect story. Klarna also learned the limits of automation in customer-facing work and had to bring back more human options when quality mattered.


AI does not eliminate humans everywhere. It compresses the work where the loop is structured, measurable, and repeatable. It exposes where humans still matter because the work requires trust, empathy, quality, judgment, or context the system cannot yet reliably handle.


So the lesson from Klarna is not "fire everyone." The lesson is that the revenue-per-employee frontier can move very quickly when AI is deployed against the right loops.


### The moat becomes organizational memory


As AI systems start doing real work, the reason you stay with a vendor starts to look more like the reason you stay with a great employee. They deliver the outcomes you need, you like the way they work, they understand your business, and over time they accumulate context you do not want to lose.


A trusted AI system living inside your organization can learn what worked, what failed, who approved what, which accounts matter, which workflows are safe, which actions require a human, and which decisions created outcomes. That becomes more than data. It becomes organizational know-how.


This is the new moat. Not just data, not just workflow, not just model quality, but permissioned memory. A trusted AI system that has been allowed to operate, observe, learn, and improve inside the enterprise becomes much harder to replace than a dashboard.


DeepSeek made the broader point bluntly. A Chinese hedge fund manager open-sourced a frontier model and momentarily wrecked the US market cap of every public AI company. The lesson: the model itself is not the moat. The model is a formula. The formula gets cheaper to copy every quarter. The moat is the data the model trains on, the context it accesses at inference time, and the customer relationships that put both into a feedback loop.


That is why the infrastructure layer matters so much. Memory only compounds if the company has the architecture to capture it, govern it, route it, audit it, and turn it into better decisions. No shared orchestration layer means no shared memory. No shared memory means no compounding intelligence. No compounding intelligence means no moat.


We've gone deeper on the architecture underneath all of this in our[GTM Brain post](https://www.warmly.ai/p/blog/gtm-brain-own-decisions) .


---


## Why the curve keeps accelerating


A small number of companies will grab everything because intelligence scales and generalizes so well, and it is only getting better.


Everyone in tech, including me, is incentivized to remove friction from AI consuming as much data as possible. So we build MCPs and APIs into our apps. Even Salesforce has announced it is going headless, which means they are building for agents to do work and are not optimizing for people clicking around in apps or UI.


The models keep generating smarter intelligence, so pre-training, post-training, test-time inference, and agentic scaling all see big lifts. And they are doing it for cheaper. The cost of compute is rapidly decreasing thanks to the cost of energy decreasing through advancements in AI itself, chip architecture, and data center design.


---


## How companies win


Pre-training makes intelligence broader and cheaper. Post-training makes it useful inside a specific domain. Test-time scaling lets the system spend more compute to retrieve context, reason, verify, and decide. Agentic scaling lets the system multiply itself into teams of workers. Synthetic data scaling and the post-training-to-pre-training feedback loop ensure the curve does not bend for lack of data.


All five curves are headed up and to the right at the same time, and they multiply against each other.


Put those together and the direction becomes obvious. Any workflow with enough data, repetition, and feedback will be pulled into an agentic loop.


The companies that win will not be the ones that spray AI everywhere equally. They will be the ones that find the highest-leverage loops fastest and focus their humans there.


The power laws are getting stronger. More things are possible, which means there are more paths to go down. But only a small number of those paths will create most of the outcome. That is where humans still matter most.


Deciding what to build, for who, when, and how to market to them is not a controlled environment. The world is changing too fast, and the leverage available is too large to waste time on the wrong bets.


So the human job moves up. Find the scarce leverage. Ask the better question. Choose the right market. Shape the story. Know the customer. Decide which loops are worth automating. Decide where agents should act and where humans should stay in control.


So when we hire in sales, CS, or marketing, we give a big advantage to people who are strong with AI. Not because AI replaces their function, but because it lets them elevate themselves and become the person who reinvents the function.


Intelligence is becoming cheaper and more abundant. That does not make humanity less valuable. It makes the human parts harder to fake: taste, judgment, trust, pain tolerance, creativity, generosity, and the ability to mobilize people around a shared vision.


The future is inevitable. Pre-training scales. Synthetic data scales. RL environments scale. Agentic multiplication scales. Compute keeps getting cheaper. The economics are too aligned, the scaling laws are too steep, and the feedback loops compound faster than any single company or country can resist them.


The gravitational pull is too strong. Anything that resists gets pulled in eventually. The only useful question is whether you contribute to the shape of the future or get rolled by it.


Build toward the inevitable, but be a steward of it. Decide what your company's relationship to AI looks like. Decide what data you feed in. Decide what guardrails you build. Decide which jobs you are still going to staff with humans and why. Decide what your product actually delivers as an outcome to your customer, instead of as a workflow.


Democratized intelligence is not something to fear. It is an incredible tool to make humanity more powerful.


---


## FAQ


### What is agentic GTM?


Agentic GTM is the application of autonomous AI agents across the go-to-market function (marketing, sales, customer success) so that execution is handled by agents and humans operate the strategy layer above them. It replaces traditional workflow tools with decision systems that ingest signals, make routing and messaging choices in real time, and learn from outcomes.[Apollo](https://www.apollo.io/magazine/redefining-how-companies-drive-revenue) , Highspot, Aviso, Evergrowth, Landbase, and Warmly are all building in this category as of 2026.


### What is context engineering, and how is it different from prompt engineering?


Prompt engineering is the practice of crafting good prompts for an LLM at inference time. It is tactical and ephemeral. Context engineering is the practice of designing what an AI system remembers, how it stores those memories, how it links them, and how it surfaces them at decision time. It is strategic and persistent. Prompt engineering optimizes a single conversation. Context engineering optimizes the system's intelligence over months and years. In GTM, context engineering is the discipline of building and governing the context graph (the memory layer) that every agent queries.


### What are the four AI scaling laws?


(1) Pre-training scaling: bigger models, more data, more compute produce broader general intelligence. (2) Post-training scaling: feedback, examples, preferences, and tool traces fine-tune a raw model into a useful assistant or agent. (3) Test-time scaling (long thinking): the model gets better answers by spending more compute at runtime to retrieve context, reason, verify, and decide. (4) Agentic scaling: a single agent can spawn sub-agents, each with their own context window, multiplying the system's effective workforce. A fifth law sits underneath all four: synthetic data scaling, where AI-generated data verified against ground truth feeds the next training cycle.


### What is an AI SDR, and why are they failing?


An AI SDR is an autonomous agent that handles sales development tasks (prospecting, cold outreach, follow-up) without a human in the loop. The vendors that pitched "AI SDR replaces humans" failed in 2025-2026 because they automated volume without solving judgment. AI-generated cold emails at scale ruined deliverability and brand reputation, leading to 50-70% contract churn within 90 days. The teams winning are using signal-based, context-aware agents that decide when not to reach out, and those agents live in marketing's P&L, not sales'.


### Will AI replace sales reps and marketing teams?


No, but both functions will get smaller and more strategic at the same time. The execution layer (SDR volume, ad operations, content production, list building, basic email marketing) is being compressed by AI. The strategy layer (brand, narrative, ICP definition, agent orchestration, enterprise relationships, deal closing) is being amplified. Expect headcount to shrink and individual contributor leverage to skyrocket. The CMO seat is becoming the CRO seat at the companies moving fastest.


### What is AI memory and why does it matter for sales and marketing?


AI memory is the persistent, structured store of information an AI system can access across conversations and decisions. It matters for sales and marketing because the context window of any single LLM call is too small to hold a complete picture of an account, a buying committee, a deal, or a customer relationship. Without memory, AI hallucinates. With memory, AI reasons. The companies winning agentic GTM are the ones building memory infrastructure (context graphs) underneath their agent layer.


### What is AEO and GEO, and how is it different from SEO?


Answer Engine Optimization (AEO) is the practice of being the answer when an AI model picks one. Generative Engine Optimization (GEO) is the practice of shaping how generative models talk about your category, your competitors, and you. Both replace SEO as the dominant marketing discipline because the buyer's first touch is no longer a Google search results page. It is an answer in ChatGPT, Claude, Perplexity, Gemini, or an AI Overview, with a recommendation already inside it. AEO and GEO are not SEO with a new name. SEO is about ranking. AEO and GEO are about being trusted enough that the model recommends you, accurate enough that the recommendation holds up, and structured enough that the model can actually use what you have written. The model is reading your G2 reviews, your case studies, your YouTube transcripts, your podcast appearances, your earned press, and your social posts. It is forming a representation of who you are and using that to decide whether to mention you when a buyer asks. The companies that win this layer are the ones that publish dense, specific, defensible evidence, not the ones that ranked highest on a keyword.


### Is the CMO becoming the CRO?


Increasingly yes. Spencer Stuart's 2025 CMO tenure study found 65% of exiting CMOs got promoted internally or took lateral / step-up jobs, and 10% became CEOs. Latané Conant went from CMO of 6sense to CRO at the same company.[Forrester's AI CMO report](https://www.forrester.com/press-newsroom/forrester-b2b-marketing-sales-product-2026-predictions/) says CMOs are now evaluated on their ability to design and orchestrate the conditions under which growth consistently occurs.


### Is agentic GTM hype or real?


Real. The data supports it. The companies that have moved fastest are running smaller teams, smaller demand gen budgets, and bigger pipeline numbers (Warmly is one of them: see[how we 3x'd pipeline in 30 days](https://www.warmly.ai/p/blog/how-we-3xd-b2b-pipeline-in-30-days) ). The category is being defined right now (Apollo's agentic platform launch in March 2026, Forrester's Revenue Marketing Platforms Wave in Q1 2026). The vendors that do not build agentic systems in the next 18 months will be acquisition targets, not category leaders.


---


## The bet Warmly is making


We are a B2B SaaS company. Our customers run sales and marketing teams. We sit at the website, identify visitors, capture signals, route the right people to the right reps, and run AI agents that handle inbound conversations, outbound sequences, and account orchestration.


The bet we are making is that the next decade in B2B is decided by context engineering. That every GTM motion eventually runs on a context graph that compounds learning across customers. That the team running the website, the website chat, the agent layer, and the signal infrastructure is not sales. That the team shaping what the AI says about your category when a buyer asks is also not sales. It is all marketing, with a CMO who is becoming a CRO.


We did not get here because we are smart. We got here because we started building the identity graph and the signal layer four years ago, before the market knew what it was. We have processed over 137 million sessions. Every one of them is a data point our system learned from. That compounds.


Will Warmly be the canonical agentic GTM platform? I do not know. The category is being decided. There are five other vendors with serious takes. But I know the architecture is right. I know the team is right. I know the customers are betting on us. So we keep building.


Whoever wins this category wins one of the biggest software markets of the 2030s. Hundreds of billions of dollars. The CMO seat becoming the CRO seat. The marketing function absorbing what used to be sales, support, and analytics. A single team running the entire revenue motion through agents.


That is where this is going. The question is not whether. The question is who. And whoever it is, the rest of us are going to live under the defaults they set.


If you are building toward this future, build well. Steer carefully. The next 50 years of B2B revenue depend on getting the architecture right and putting the right humans on top of it.


We are trying. Come build with us.


If you run sales or marketing at a B2B company and want to see what an agentic GTM stack looks like in production,[book a demo](https://www.warmly.ai/p/book-a-demo) . We will show you the context graph, the signal stack, and the agents we run on top of it.
