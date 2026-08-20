---
schema_version: "1.0.0"
document_id: "def27136dc8931d56132fb3b6084c739c928409f392dd8e86d9ca4587e0eeb6e"
company_key: "yc-dailybot"
company: "DailyBot"
source_id: "yc-dailybot-rss-59e03def7d75"
canonical_url: "https://www.dailybot.com/blog/how-we-run-long-horizon-agent-work/"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:17.151433+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:8c92817f1483cbe5cad2cfd1f11222fa2c14e26a55b5fe901f56286ef5b0c439"
---

# Deep Work Plan: Models matter. Context matters more. Give your coding agent a plan.

When we[moved our public surface](https://www.dailybot.com/blog/how-we-migrated-dailybot-to-astro/) and shipped Dailybot 3, the post ended on a question: a year from now, who will move the product and all the technical work behind it? Our answer was everyone on the team, because an agent sits between the people and the repository and does the translation. That answer raised a harder one, one we had to solve before the velocity was real: once everyone is shipping, and every shipper is part human and part agent, how do you keep the work from drifting?


The drift is specific, and if you have run a long agent task you have seen it. An agent opens strong. The first hour is sharp: it reads the right files, makes the right edits, runs the tests. Past a certain point the thread is longer than the model’s useful attention, the original goal has scrolled out of view, and the agent starts solving a slightly different problem than the one you asked for. Everything still works. It just is not what you wanted. By the time you notice, you are reviewing work that wandered, and there is no clean way to resume from the last good state because the state only ever lived in a chat window that is now too long to trust.


We ran into this working across so many projects that we stopped treating it as a prompting problem and started treating it as a structural one. The method we landed on is[Deep Work Plan](https://deepworkplan.com/) , a methodology we now practice across our repositories and have open-sourced under an[MIT license](https://github.com/DailybotHQ/deepworkplan-skill) . This post is how we use it and why it works for us. It is not a product we sell. It is a way of working we adopted, wrote down, and released.


We started on this before **spec-driven development** and harness engineering — the repository as the harness — became popular ideas. We did not arrive from theory but from the work: solving the same problem over and over until the pattern was obvious. We were not the only ones, and we do not claim to be; many teams reached the same conclusion from different starting points, and that convergence tells us this is not a fad but a real shift in how software gets built with agents. Plenty of teams have it solved their own way today. This is ours: we tested and verified it across our own repositories, and we left it as a standardized methodology any agent can pick up to work without drifting, against a structured plan.


## Two ideas that fixed the drift


The two ideas in one picture: a durable spec the work is charted against, and a repository that carries its own harness so any crew can sail it.


The first idea is to make the plan the source of truth instead of the chat. Before any agent touches code, we write a spec: the goal, the work broken into atomic tasks, and for each task an explicit set of acceptance criteria and a validation gate it has to pass. The agent does not get to decide it is done. The gate decides. A task is complete when its tests pass, its build is green, and its acceptance criteria are checked off, not when the model feels finished. This is spec-driven development, and the durable part is the word durable. The spec lives on disk as files in the repo, so it survives a context reset, a new session, or a handoff to a different agent tomorrow. When the chat gets too long to trust, the plan is still there, and so are the checkboxes that say which tasks already passed their gates.


The second idea took us longer to name. For a while we kept building scaffolding for agents: the context they need, the tools they can call, the control loop that keeps them on task, the guardrails, the way state persists so a run can stop and resume. Every tool we tried wanted us to rebuild that scaffolding inside its own framework. So we put it where it belonged. We put it in the repository. The context is the files. The tools are the scripts and the test suite the repo already has. The control loop and the guardrails are the plan and its gates, written as plain files any agent can read. The state is on disk. Said plainly, the repository itself becomes the harness, and because the harness is just files in the repo rather than one vendor’s framework, any agent can pick up the repo and run it. It is what other teams had already started naming, and what the industry now calls **harness engineering** .


Those two ideas are the whole method. A durable spec the work executes against, and a repository that carries its own harness so the spec is runnable by whatever agent shows up. The first stops the agent from drifting away from the goal. The second stops the method from being trapped inside a single tool.


## The proof is that we run it on ourselves


The strongest thing we can say about this is that we are not describing a thing we hope works. We have been running it internally since late 2025, across dozens of projects, and it is part of how our engineers ship now. This very page runs on the method: the site you are reading and the site that documents the methodology are both maintained this way, so the methodology documents itself from a repo that uses it. When we say a plan can run for hours across context resets and stay on track, it is because we watched it do that on the repository serving these words.


It is also installable today, which matters more than it sounds. A lot of good methodology never leaves the team that invented it because there is no clean way to hand it over. This one has a public site, a canonical adoption endpoint at[deepworkplan.com/init](https://deepworkplan.com/init) , and an installable skill at[DailybotHQ/deepworkplan-skill](https://github.com/DailybotHQ/deepworkplan-skill) . The path from reading about it to running it in your own repo is one step: install the skill, let it onboard your agent, then generate and execute a plan. We wanted the distance between interested and running to be short, because that distance is where most methodologies die.


The part we think actually matters in a year is that it is tool-agnostic. There are good tools in this space that bundle spec-driven workflows, and GitHub Spec Kit is a fair example of the category. The difference is where the method lives. Those tools hold the workflow inside the tool, so adopting the method means betting that the tool survives and that every agent you use speaks its format. Deep Work Plan lives in the repository as files any agent can read, so it is not a bet on one vendor at all. When the tooling landscape shifts again, and it will, the plans and the gates and the state are still sitting in the repo where the next agent can pick them up.


## Where it does not help


It is worth being clear about what the method does not solve: it does not make a bad goal good. If the plan is vague, the agent executes vagueness faithfully and the gates pass on work nobody wanted. When execution is this fast, ambiguity gets expensive: the discipline moved from watching the agent to writing the spec, and a thin spec is still the failure mode. We spend real time on the plan now, because a sharp plan is the part the whole run depends on. Writing one well means defining the real problem and not the symptoms, setting the constraints and the criteria it will be validated against, and anticipating where the agent will guess so you can close those gaps before it starts. That clarity is the part that stays ours, and the hardest part: the method does not replace it, it makes it indispensable. That is why it earns its keep on long work, not on a one-line change: the longer the run, the more it pays to have thought clearly before delegating.


## What actually changed


What changed is not that our agents got smarter. The models are the models, and they will keep improving on their own schedule. What changed is that the work no longer lives in a chat window that stops being trustworthy as it grows. It lives in the repository as a plan with gates, and that is the part that makes a long run verifiable when it finishes and resumable when it stops.


That shift is what let the velocity from our migration hold up once everyone was contributing. A team moving fast across the product and everything that holds it up needs more than capable agents. It needs a way to point agents at long work and trust that what comes back matches what was asked, run after run, agent after agent. The repository as the harness is how we get that, and the spec is what the agent cannot drift from. If your agents drift into a different problem than the one you set on long work, the fix is probably not a better prompt: it is a plan they execute against and a harness that can carry it. The models will keep getting better on their own; what puts that capability to work for you is the plan you set in front of it. Start yours at[deepworkplan.com](https://deepworkplan.com/) .
