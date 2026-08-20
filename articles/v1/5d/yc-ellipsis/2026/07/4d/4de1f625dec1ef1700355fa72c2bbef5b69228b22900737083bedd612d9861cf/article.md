---
schema_version: "1.0.0"
document_id: "4de1f625dec1ef1700355fa72c2bbef5b69228b22900737083bedd612d9861cf"
company_key: "yc-ellipsis"
company: "Ellipsis"
source_id: "yc-ellipsis-news-import-97656c4ed8a9"
canonical_url: "https://www.ellipsis.dev/blog/the-ellipsis-agent-cloud"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T00:19:13.171637+00:00"
fetched_at: "2026-07-29T00:19:14.391456+00:00"
content_hash: "sha256:7fdbe53ba948cdbcadcc7ed80e837b93b9181df7be61144f869e957316173c46"
---

# Introducing the Ellipsis Agent Cloud

Ellipsis launched in 2023 as one of the first AI Code Review bots. Our agents have reviewed millions of lines of code for hundreds of companies, eventually becoming the most popular AI Code Review agent among YC founders.


Historically developers wanted a product that "just worked". Developer installs, Ellipsis starts reviewing code. That is not the case anymore. Teams understand agents, and want to customize models, prompts, and environments.


This is why we are releasing the Ellipsis Agent Cloud, a cloud platform for deploying and managing coding agents.


## What teams can do with Ellipsis


- Build custom agents for Feature Development, Code Review, and Workflow Automations
- Define agent permissions before they run
- Enforce budget limits on coding agents
- Monitor session logs, including agent thinking and tool calls


## Agents as code


Every agent is a YAML file in your repo. It declares its model, its prompt, what it watches, what it is allowed to touch, and what it may spend. You change how it behaves with a pull request, so agent config is shared like any other code.


- **Version history for free.** Changing an agent is a pull request, with an author, a date, and a diff. If it makes things worse, you revert it.
- **Shared, not personal.** The config lives next to the code it operates on, so the whole team gets the same agent instead of a setup that lives on one laptop.
- **Two of anything.** A strict reviewer for migrations and a lenient one for docs is two files, not one settings page you have to compromise on.
- **Blast radius is explicit.** Permissions and budgets are fields in the file, so they get reviewed like any other code.


See[Agents as code](https://www.ellipsis.dev/docs/concepts/agents-as-code) for how sync and previews work, and the[agent config reference](https://www.ellipsis.dev/docs/reference/agent-config) for every field.


## Feature Development


Most teams already run coding agents on laptops. The agent works, but the machine it runs on is the problem. Moving the same agent to the cloud changes what your team can do with it.


- **Real parallelism.** Every session gets its own sandbox, so you can start five against the same repository without them fighting over the working tree.
- **It survives your laptop.** The work is not on your machine, so closing the lid, losing wifi, or rebooting does not kill the run.
- **The team can search the logs.** Every session is recorded end to end, so anyone can find which session touched a file or opened a pull request.
- **Governance by default.** Per-session and monthly spend caps are enforced before compute starts, so a bad prompt costs you a capped amount.
- **Bot permissions, not yours.** A local agent runs as you, with your full access. A cloud agent runs with a scoped token minted for that session, narrowed by permission and by repository, and revoked when the session ends.


That last point is the one teams underrate. On a laptop, the agent inherits whatever you can do. In the cloud, the ceiling is whatever the YAML granted, enforced at the token mint rather than by asking the agent nicely.


As part of this launch, we are releasing the` agent` CLI. It is a drop in replacement for your favorite coding agent harness, but runs your sessions in the cloud where your organization can properly manage them.


The session keeps running after you close the terminal, and you can send it a follow-up message mid-task instead of killing it and starting over. Sessions start from a Slack mention, a GitHub thread, a Sentry alert, a cron schedule, or your terminal. We also have a[REST API](https://www.ellipsis.dev/docs/reference/api) for spawning sessions. See the[CLI reference](https://www.ellipsis.dev/docs/reference/cli) to install it.


## Code Review


Review is where we started, and it is better as an agent you define than as the fixed product it used to be.


- **Your standards, not ours.** Your model, your instructions, your repositories, your authors. If your team has a review convention in a doc, the agent can apply that document.
- **Specialists are just more files.** A stricter reviewer that only wakes up for database migrations is a path filter on a second config.
- **Incremental by default.** The agent reviews the range since its own last posted review, so pushing a fixup does not re-litigate the whole pull request.
- **Read-only if you want it.** A reviewer does not need write access, so you can scope its token to read and leave it there.


Start with[reviewing every pull request as commits land](https://www.ellipsis.dev/docs/guides/how-to-review-every-pull-request-as-commits-land) , or[reviewing database migrations before they merge](https://www.ellipsis.dev/docs/guides/how-to-review-database-migrations-before-they-merge) for the specialist version.


## Workflow Automations


The third job is everything that should happen without a person noticing first. Same schema, same sandbox, same spend caps, same transcript. Only the trigger and the prompt change.


- **Production events get a first pass.** A Sentry trigger means an alert has a diagnosis waiting before anyone opens the dashboard.
- **Filed work gets started.** A Linear trigger turns a new issue into a first attempt at an implementation.
- **Recurring chores actually recur.** A cron trigger handles the daily and weekly work nobody wants to own.
- **Anyone can ask.** Mention the agent in GitHub, Slack, or Linear and it answers in the thread, with your codebase in scope.
- **Adoption is one more file.** A team that came for review already has the infrastructure for all of this.


Worked examples:[turning Sentry alerts into root-cause diagnoses](https://www.ellipsis.dev/docs/guides/how-to-turn-sentry-alerts-into-root-cause-diagnoses) ,[automating your daily standup](https://www.ellipsis.dev/docs/guides/how-to-automate-your-daily-standup) , and[answering codebase questions from GitHub, Slack, and Linear](https://www.ellipsis.dev/docs/guides/how-to-answer-codebase-questions-from-github-slack-and-linear) .


## Pricing


Platform pricing is usage based. You pay for the tokens and compute your agents use. No seats.


New users get $10 in credit to try it, and new organizations get $100 to build their first agent or run Claude Code in the cloud. No card required while the credit lasts. Spend caps are per session and per month, so the bill cannot run away from you either way. See[Budgets and spend](https://www.ellipsis.dev/docs/concepts/budgets) for how the limits cascade.


We build Ellipsis with Ellipsis. Our own agents open and merge PRs in this repo every day. The[quickstart](https://www.ellipsis.dev/docs/get-started/quickstart) gets you a working agent in a few minutes.
