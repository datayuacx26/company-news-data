---
schema_version: "1.0.0"
document_id: "0f0c2c39fd510cfac03a3999fc412b7dea0e4c15ec4f56707080d1027d1f5266"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-rss-dd7d0cc56e2d"
canonical_url: "https://medium.com/riskified-technology/from-hallucinations-to-pull-requests-building-a-reliable-shifter-agent-in-48-hours-d3c8eef6421a"
published_at: "2026-04-29T11:42:08+00:00"
first_seen_at: "2026-07-20T23:18:31.853064+00:00"
fetched_at: "2026-08-20T02:06:38.087890+00:00"
content_hash: "sha256:f622b486b0336016b4bba3e7866623395601031ae98c39d94388ce1ffe7cc329"
---

# From Hallucinations to Pull Requests: Building a Reliable “Shifter” Agent in 48 Hours

#### Designing a constrained LLM pipeline that turns vague tickets into safe, reviewable pull requests


Hackathons compress everything: ambition, uncertainty, and deadlines. You get two days to build something that looks like the future — while knowing it might break five minutes before the demo.


Ours almost didn’t make it to the “looks like the future” part.


By 18:00 on Day 1, other teams were already packing up. Their prototypes worked. They’d spend Day 2 polishing slides and adding tiny UI flourishes.


We were still staring at an agent that was confidently generating random code in random places. The kind of output that makes you laugh… right before you realize you have to demo in less than 24 hours.


So we pivoted hard: **Stop solving the ticket. Start with: *where does this live?***


That single constraint turned a demo risk into a workflow we could actually trust. It’s what we demoed on Day 2 — and it ended up winning our company hackathon. This is the story of how our first approach failed, what clicked overnight, and the pipeline behind our “Shifter Agent.”


#### A quick note on the team


This wasn’t a solo effort — I worked on this hackathon project together with Tamara Bernshtein, Yair Ofek and Or Sagiv. The reason the pivot worked is that we could iterate in parallel: while one of us was pressure-testing the agent’s behavior, another was tightening the workflow constraints, and another was shaping the demo path and narrative. That collaborative loop is what let us go from “wild hallucinations” to something we could trust in under a day.


### The Problem: “Shift” Work is Mostly Context-Switching


If you’ve ever been “on-shift” for your team — especially on a data platform team where you both build internal tooling and support its users — it usually will look like this:


- The ticket arrives with minimal info.
- In an environment shaped by years of GitOps and platform growth, “the codebase” isn’t one place — it’s dozens of repos and automation layers. So the first 20 minutes can be just figuring out *where* the change even belongs.
- Only then do you write code… or realize it’s actually a documentation / “how do I” question.


Our hackathon goal was simple to describe and hard to execute: build an agent that takes a ticket assigned to our team and either:


- opens a PR, in the correct repository, with the correct code change, or
- drafts a high-quality reply with next steps.


In other words, the goal is for the shifter to focus on review and approval, not on searching, guessing, or chasing context.


### Day 1: The “Everything Context” Trap


Our first instinct was the classic one: **“ *Let’s give the agent all the history.”***


So we scraped completed tickets, fetched linked PRs, collected diffs, and stitched it all into a huge CSV (ticket content + metadata + diff details). Then we gave an LLM that CSV plus a “playbook” prompt: find similar tickets, use them as context, and generate a PR or reply.


By 14:00–15:00, we had a huge file full of *“everything the agent could possibly need.”*


Then we used a powerful LLM in Cursor and gave it:


1. The full CSV as context.
2. A markdown “playbook” telling it to:


- Receive a new ticket
- Find similar historical ticketsUsee those as context
- Create PRs or draft a reply via GitHub/Jira integrations.


What happened next was instantly clear. It was hilarious. And completely unusable.


#### Why it failed (in one sentence)


When we drowned the model in raw history and gave it too much freedom, it didn’t “search better” — it hallucinated harder.


The agent didn’t behave like a careful engineer. It behaved like a confident improviser. It invented solutions, invented justifications, and sometimes invented file paths.


### The Pivot: Stop Solving the Ticket. Start with “Where Does This Live?”


At the end of Day 1, we asked a different question:


**What is the smallest sub-problem that must be correct for everything else to work?**


Answer: **repository selection** .


If the agent can’t reliably identify where a change should happen, nothing else matters. So we re-framed the task:


1. Don’t solve the ticket yet.
2. First, determine which repo (and ideally which area of it) is relevant.


#### Building a “map” that the agent could actually use


Instead of forcing the model to reason over thousands of files, we gave it a navigable index.


We created a local workspace containing the team’s relevant repos and had the LLM generate a short markdown summary for each:


- What the repo is for
- High-level directory structure
- Where common changes usually happen


We kept these intentionally brief (~20 lines). The goal wasn’t completeness; it was orientation.


#### Using ticket labels as filters


Ticket labels correlated strongly with tech stack and scope, so we used the incoming ticket’s labels to pull a candidate set of completed tickets. From those, we fetched the linked PRs and looked at what actually changed.


Combining:


- repo summaries (“the map”)
- changed folders from similar tickets (“where did the solution live last time?”)


…gave us surprisingly consistent repo selection. And that unlocked Day 2.


### Day 2: Turning the Breakthrough Into a Pipeline


Once repo selection worked, the rest stopped feeling like magic and started feeling like engineering. The question became:


**How do we give the agent enough context to generate correct changes — without sliding back into the Everything Context trap?**


Our answer was to build a structured workflow where each step constrained the next.


#### The final workflow (high-level)


Here’s the pipeline we ended up with:


Worfklow of the Agent


The most important design choice is hidden in step 7:


**Once repositories were selected, the agent was allowed to use *only those repos* as code context.**


That one constraint prevented a lot of “creative wandering.”


#### A concrete example: from a vague ticket to a focused PR


Here’s a sanitized version of a real kind of request we built for.


**Ticket**


- Title: “Grant read access to the Analytics team on <internal_app> schema”
- Description: “Grant permissions on <internal_app> schema in the production data catalog”
- Labels: data_platform_permissions


This is a classic shift ticket: low information, but usually low risk — if you change the right place.


#### What the agent produces


**1) Where it lives (repo + file path)** Based on label filtering + similarity + “where did similar fixes land?”, the agent recommends:


- Repo: <infra-repo> (permissions-as-code for the data catalog)
- Path: environments/production/…/schemas/<schema_name>.tf


**2) The smallest safe change** In that file, the change is one line: add the relevant group to a read_only list:


Agent actions on the PR


**3) A PR that’s easy to review** Instead of a wall of text, the agent generates a PR description that’s short and audit-friendly. Here is an example:


- **Summary:** Grant read permissions to Analytics on <prod_catalog>.<schema_name>
- **Changes Made:** Added <analytics-readonly-group> to the schema’s read_only grants
- **Why this approach:** Matches the established pattern used in other schemas (consistency > creativity)
- **Impact:** Low risk (read-only; no existing permissions modified)
- **After merge:** The standard deployment pipeline applies the updated permissions automatically


That’s what “where does this live?” means in practice: the agent stops guessing and starts following proven paths.


### Why this worked


#### With this pipeline, the agent didn’t need to invent a solution from scratch in the abstract. It could learn:


- the “language” of the repo (conventions, patterns, structure)
- the likely location of changes (based on diffs from similar PRs and from understanding the structure)


And suddenly we saw something we hadn’t seen all Day 1: code changes landing in the correct repo, in the correct files, across different stacks.


### The Reliability Problem We Didn’t Solve (Yet): Input Quality


There was one uncomfortable truth: output quality depends heavily on ticket quality.


If a ticket is vague or missing critical details, the agent has to infer too much — and inference is where reliability goes to die. If we continue this project, the next step isn’t “make the model smarter.”


It would be: **make the inputs better.**


Because this was a 48-hour build, we optimized for a workflow we could trust in a demo, not a production-grade agent. If we had a few more days to invest, we’d focus on turning it into something proactive, safer, and more useful in real shift work:


**Make it proactive inside Jira (not just reactive).** Build a small platform/service that continuously tracks new shift tickets and runs the agent automatically in the background. For each new ticket, the agent would:


- propose the most likely repo/location,
- draft an initial response,
- and comment on the ticket with clarifying questions when context is missing (instead of guessing).


**Add a “needs-more-context” loop.** If the agent can’t confidently pick a repo or path, it should explicitly say so and request the missing details.


**Ground answers in documentation (Confluence).** Connect the agent to Confluence so it can answer “how do I…” tickets with citations and the latest internal process context.


### **Closing Thought: Hackathon Urgency Forces the Right Question**


On Day 1, our question was:


**“How do we make an agent solve tickets?”**


Under pressure, we learned to ask a better one:


“ **What’s the smallest thing it must do correctly so the rest can follow?”**


For us, that was repo selection — and once we got that right, the rest stopped being luck. When you only have 48 hours, you don’t get to build everything. You get to find the one constraint that turns chaos into a demo.


---


[From Hallucinations to Pull Requests: Building a Reliable “Shifter” Agent in 48 Hours](https://medium.com/riskified-technology/from-hallucinations-to-pull-requests-building-a-reliable-shifter-agent-in-48-hours-d3c8eef6421a) was originally published in[Riskified Tech](https://medium.com/riskified-technology) on Medium, where people are continuing the conversation by highlighting and responding to this story.
