---
schema_version: "1.0.0"
document_id: "0d80b3ae14361c24eafe61f773f42113d06eb51449b142716728fe363eeab704"
company_key: "yc-whatnot"
company: "Whatnot"
source_id: "yc-whatnot-rss-30861744a6f8"
canonical_url: "https://medium.com/whatnot-engineering/your-sops-are-already-code-you-just-cant-run-them-yet-789e3e055455"
published_at: "2026-03-10T15:01:01+00:00"
first_seen_at: "2026-07-24T07:07:55.027426+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:19c378fc8e52d4092cc2b5e133d9ab739721d9c7d05017836c9510b4083cc3ac"
---

# Your SOPs Are Already Code — You Just Can’t Run Them Yet

Featured


# Your SOPs Are Already Code… You Just Can’t Run Them Yet


[Whatnot Engineering](https://medium.com/@whatnotengineering?source=post_page---byline--789e3e055455---------------------------------------)


7 min read


·


Mar 10, 2026


--


**Paul Gut | CX & Trust Engineering**


Every support org has them: thousands of pages of branching, conditional logic scattered across flowcharts, docs, and the heads of your most tenured agents. “If the label’s been generated, check the tracking status; if it shows delivered, verify the address on file…” The Standard Operating Procedures (SOPs) grow until they can no longer be managed. And when you’re the fastest-growing live commerce platform in history, it all catches up with you even faster.


At Whatnot, our agents handle everything from shipping questions on vintage sneaker orders to address changes on trading card shipments. Until recently, they were spending the majority of their time on work that required no human insight, like checking order prices, validating addresses, looking up tracking statuses — routine data checks interspersed with the actual decision-making. Even worse, our more experienced agents had memorized the SOPs and stopped referencing them altogether, which made propagating even the slightest changes a nightmare.


We needed SOPs to behave like code: automatable, deployable, version-controlled. And there’s no better way to make something behave like code than to simply *turn it into code* . So in mid-2025, we built Atlas: our new workflow execution and automation engine.


## Formalize first, automate later


The idea behind Atlas is simple: for every ticket, the engine traverses a directed graph of nodes, eventually reaching a resolution the agent can execute. The graph is represented in code (more on that decision later).


Press enter or click to view image in full size


For every ticket, the engine traverses a directed graph of nodes, eventually reaching a resolution the agent can execute.


We built a small Python DSL to represent these graphs. Each node could be a decision (“Has a label been generated for this order?”), an action (“Generate a shipping label”), or a resolution (“Send the label to the user”). We also made data points — reusable pieces of information like order IDs and tracking numbers — first-class citizens. Here’s a slightly simplified example of a decision node:


```text
BinaryDecisionDefinition(    title="Does the tracking show the item was delivered?",    node_key=NodeKey("decide_delivery_confirmed"),    data_points=[DataPointKey("tracking_status")],    yes_node=NodeKey("check_address_match"),    no_node=NodeKey("check_tracking_stuck"),    automation=None,  # …for now.  )
```


Being engineers, our first instinct was to jump straight to automating things, but we held off. Instead, every node simply started out as “manual” — it would display the instructions and context to the agent and ask them to make the decision or take the action. Only once the workflow was fully formalized and used by agents did we add the automation function.


That took some self-control, but it turned out that simply breaking the workflow up into tiny pieces made many of the nodes almost trivial to automate. Every node was a simple question or action, complete with the data points needed to resolve it — more than enough for our LLM coding assistants to automate a given node in most cases.


## Formalizing thousands of pages with an LLM


*Note: I won’t call the LLMs “agents” here to avoid confusion with our customer support agents, but that’s what we’re talking about.*


Then came the fun part: we had to formalize all these SOPs into Atlas workflows. We were a team of three, and between building the Atlas engine itself and writing automations, we didn’t exactly have the bandwidth to hand-translate thousands of pages of SOPs. So we reached for the most obvious tool for the job: LLMs.


It’s always good to try the simplest possible approach first. After all, these models are getting better and better — perhaps they could one-shot this task? So we tried giving the LLM a doc and asking it to formalize it. The results weren’t great. Some of these SOPs are close to a hundred pages long, so the model would quickly lose track of the branching logic and start making silly mistakes.


Press enter or click to view image in full size


We eventually landed on an agentic map-reduce style flow to import SOPs into our workflow engine.


We iterated on the approach and landed on an agentic map-reduce style flow that looked something like this:


1. **Sectioning:** The LLM takes the doc, finds natural places to split it up, and creates several smaller “sub-SOP” docs.
2. **Subgraph generation:** Separate LLMs work on the sub-SOPs, converting each into a subgraph representing a part of the workflow.
3. **Joining:** A “big picture” LLM, aware of the full workflow but not necessarily all the details, joins these subgraphs into a complete workflow.
4. **Validation and refinement:** At each stage, we run a set of checks — some programmatic, some LLM-driven — to catch structural and stylistic mistakes. More on what these checks are in a moment.
5. **Ops review:** The ops lead responsible for that SOP goes through the workflow manually to confirm correctness.


This ended up working very well. The sectioning kept each LLM focused on a manageable chunk, but the real star was the validation suite, which we called “the harness.”


## The harness


No matter how good an LLM is, it will still make mistakes — and we cannot afford mistakes in our workflows. That’s why we built the harness: three layers of checks that catch mistakes before they ever reach an agent.


The programmatic validator was the most straightforward piece. It let us define hard rules that, if broken, render a workflow “invalid”. Every workflow had to have exactly one root node; it could not contain any orphaned nodes; it could not reference nonexistent data points; etc. Since these rules were deterministic and fast to run, this was always our first line of defense. The LLM would run them after every single change, and they would also run in our CI.


Our principles validator was the piece we developed after realizing that some of our rules were more qualitative and, therefore, hard to encode. It takes the form of a simple MD file containing rules like:


- Break up multi-condition checks into separate binary decision nodes instead of cramming them into one node.
- Node keys should clearly indicate their purpose (e.g., “decide_delivery_confirmed”, not “check_status_1”)
- Only include data points that are actually needed for the current decision.


This piece worked extremely well. Every time we noticed the LLM falling into some bad pattern, we would add a rule around it to the principles file and watch the pattern disappear.


Press enter or click to view image in full size


Example of the principle validator that kept the LLM on rails.


The principles file turned out to be so useful that we started including it in the initial context (the “required reading” if you will) of every LLM in the pipeline. The initial context also included examples of some of our best-designed workflows and common pitfalls. We found that this resulted in much more cleanly structured workflows right out of the gate.


The final piece was the human check. The creator or maintainer of the SOP, usually an ops lead, would go through the workflow and point out any discrepancies with the SOP. Sometimes these were mistakes made by the LLM; other times they were slight contradictions in the original SOP that did not confuse human agents, but needed to be ironed out in the formal version of the SOP.


## Forget the database — just put it in the code.


One of the most common questions we get is why we chose to put these workflows in the codebase instead of the database. We recently announced our plans to *eventually* move them into the database, so now we also get asked whether putting them in the codebase was a mistake. On the contrary: **it was probably the single best decision we made when designing Atlas.**


When we started working on this project, we had just a few months to build this entire system from scratch in time for the holiday season. Our holiday season starts on Black Friday, and things don’t slow down until… Well, they never really slow down, because due to our growth, come spring, “Black Friday volume” usually becomes “regular Friday volume.”


Because of this, we needed to get clever about scoping this project out. But many requirements simply could not be cut: we needed version control for the workflows, we needed an approval process for all changes, we needed to ensure the workflows were always in a valid state, etc.


Building even a minimal version of this kind of change management system would likely have cost us weeks or even months. But we realized that we already used one such system every day — it was called GitHub.


By putting all the workflows in the code, we instantly hit all of our change management requirements. The version control was in Git. The approval process was handled by GitHub PRs. The CI made sure the workflows never became invalid. We used a GitHub Action to implement the changes requested by ops. This all ended up working quite well, and it’s still how Atlas works today.


The main downside of this approach is that any changes to the workflows require a deploy. That’s why we are planning to eventually move the workflows into the database. But we do not feel particularly rushed; here at Whatnot, we deploy many times a day, so this has not been a major bottleneck.


## Where things stand today


Formalizing our very first workflow took three days of engineering work. Today, the same task takes under an hour of LLM time, and *no engineering time at all* . Our ops team edits the workflows in a visual admin tool, an LLM refines the changes, and an engineer merges them in. Nobody is operating outside their comfort zone.


If you’re sitting on a mountain of operations docs full of branching and conditional steps, just remember: it’s already code — you just can’t run it yet.


Press enter or click to view image in full size


SOP steps are increasingly becoming automated via LLMs, simplifying the agent experience.


## Whatnot is hiring great engineers!


If developing trail-blazing AI use cases to deliver tangible improvements to business outcomes excites you, we’re hiring. Join us in building the future of social commerce at[whatnot.com/careers](http://whatnot.com/careers) .
