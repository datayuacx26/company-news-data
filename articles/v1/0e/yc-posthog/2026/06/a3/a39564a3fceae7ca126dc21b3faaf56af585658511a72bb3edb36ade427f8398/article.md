---
schema_version: "1.0.0"
document_id: "a39564a3fceae7ca126dc21b3faaf56af585658511a72bb3edb36ade427f8398"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/slack-app-beta"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:b1b2c6a320f0af65a2b68d3ee2275f420c019f6d7c77f4163b3bf4d21f9a29ad"
---

# We put PostHog in Slack and now everyone's an engineer

# We put PostHog in Slack and now everyone's an engineer


- [Cleo Lant](https://posthog.com/community/profiles/36864)


Jun 02, 2026


- [Product](https://posthog.com/blog/product)


,
- [AI](https://posthog.com/blog/ai)


#### Contents


-
-
-
-
-
-
-
-
-
-
-
-


Today, we're releasing the[PostHog Slack app](https://posthog.com/slack)


into beta.


We built it for those times when a colleague flags an annoying UI quirk, or a customer mentions a bug. The issues that normally end up on a backlog, untouched and ignored.


With the PostHog Slack app, you **@PostHog** to "fix this" or "build that". It spins up a sandbox, makes a plan, edits files, runs checks, opens a draft PR, and answers review comments in the thread.


The bot uses your product data as context and follows your repo's rules. It even reacts with[emojis](https://posthog.com/docs/slack#emoji-cues)


while it works, which makes it feel less like a coding tool and more like chatting with a clever teammate.


Try it yourself →[setup docs](https://posthog.com/docs/slack/setup)


##


@PostHog in the #papercuts channel


[Paul D'Ambra](https://posthog.com/community/profiles/30173)


was the first to fall in love with @PostHog. Among other important[blitzscale](https://posthog.com/teams/blitzscale)


duties, he owns the` #papercuts` Slack channel, where anyone can post the small bugs and nits they hit in the app. He'd been fixing them with PostHog Desktop like a good engineer. Now he mentions @PostHog in nearly every thread.


**The resulting PR**


[PostHog/posthog#58811](https://github.com/PostHog/posthog/pull/58811)


##


Real prompts that became actual PRs


It's awesome when a prolific engineer gets even more productive, but what makes @PostHog *really* magical is that it empowers every role. Sales, marketing, customer support – anyone can tag the bot with a bug, a papercut, or a feature idea.


Here's a few examples of @PostHog usage across the org chart:


###


The one where it built a new feature for the web app


[Will Wearing](https://posthog.com/community/profiles/41941)


(technical account manager) asked @PostHog to add support to copy and paste for markdown into PostHog[notebooks](https://posthog.com/docs/notebooks)


with proper rendering. The bot wrote the code, added 20 test cases, and auto-closed a related stale GitHub issue.


[PostHog/posthog](https://github.com/PostHog/posthog)


is a massive production repo that most people in a sales role would never feel empowered to touch. Will's PR got merged in less than 24 hours, and the only hiccup was a flaky test (nothing wrong with the code, just CI being CI).


Clearing CI is as much of a job as the code generation itself, and the bot sticks with a PR through red checks and reruns until it's mergeable.


**The resulting PR**


[PostHog/posthog#59142](https://github.com/PostHog/posthog/pull/59142)


###


The one where it prepared for a user interview


It's not just code generation. You can tag @PostHog with a data question and it runs the same agent loop as[PostHog AI](https://posthog.com/ai)


. The only difference is that answers turn up where you're already working (i.e. wasting time searching for the perfect reaction emoji).


[Cory Slater](https://posthog.com/community/profiles/34160)


(product manager) asked @PostHog to pull context on a[Session Replay](https://posthog.com/session-replay)


user he was interviewing that afternoon. The bot came back with a full brief: account value, product usage, how long she'd been a customer.


Then it went further. In addition to detecting zero[MCP](https://posthog.com/docs/model-context-protocol)


activity, it noticed she works across two PostHog projects with replicated feature flag configs. The bot flagged it as odd and suggested Cory ask about that in the interview. Clever robot.


###


The one where it updated the company handbook


[Lizzie](https://posthog.com/community/profiles/43387)


(product marketer), asked in #team-marketing what URL format to use when linking to the PostHog app in emails. She got an answer in the thread – then asked @PostHog to write it into the company[handbook](https://posthog.com/handbook)


. She didn't specify which repo, but the bot figured it out.


**The resulting PR**


[PostHog/posthog.com#16957](https://github.com/PostHog/posthog.com/pull/16957)


###


The one where it did the logs legwork


[Lucas Ricoy](https://posthog.com/community/profiles/33028)


(product engineer) asked @PostHog to check whether a recent[PR](https://github.com/PostHog/posthog/pull/58177)


, which aimed to tag WebStats queries by strategy for better performance visibility, actually worked. The logs API was returning stale data (which threw off the bot), but once Lucas confirmed the cutover had happened, the bot not only confirmed the new ones were appearing, but also that one query –` stats_table_path_bounce_query` – was showing up as a bottleneck.


**The resulting PR**


[PostHog/posthog#58177](https://github.com/PostHog/posthog/pull/58177)


###


The one where it added feet pics to the website


Then there's[Richard](https://posthog.com/community/profiles/40548)


(product engineer). He broke his foot at the recent company offsite, posted his x-ray, and asked @PostHog to add it to the[secret company feet pics folder](https://posthog.com/feet-pics)


. The bot grabbed the image link, labeled it` broken bone (real).jpg` , and passed 19 CI checks. The resulting PR was merged a lot quicker than Richard's anticipated 4-6 week recovery period.


**The resulting PR**


[PostHog/posthog.com#16876](https://github.com/PostHog/posthog.com/pull/16876)


##


What you can @PostHog to do


Over the past two weeks, we've merged 116[contributions](https://github.com/PostHog/posthog/graphs/contributors)


from @PostHog into production across AI Observability, Session Replay, Error Tracking, Feature Flags, Workflows, billing, MCP, and the Data Warehouse. No corner of the codebase is off-limits (except our secrets).


The work it's taken off our hands sorts into roughly these[categories](https://posthog.com/docs/slack/commands)


– and yours probably looks similar:


- **Content and docs** – Navigation changes, removing stale content, adding new pages, copy updates, fixing 404s. Admin chores[fit for a robot](https://posthog.com/blog/machine-copy-paste-mcp-intro)


.
- **Code maintenance** – Removing released feature flag guards, updating naming conventions, bumping versions, resolving merge conflicts, dead click tracking.
- **Bugs and CI fixes** –[Errors](https://posthog.com/blog/agents-closed-4063-errors)


, display issues, flaky tests, merge conflicts. Anything you tell an agent to "debug".
- **UI polish** – Layout tweaks, swapping icons, adding keyboard shortcuts, task renaming, in-app banners and notifications. The nice-to-haves you never seem to get to.
- **AI infrastructure** – Updating the MCP server, adding skills and prompts for AI observability, writing[evals](https://posthog.com/blog/stop-ai-slop)


, tool schema improvements, LLM gateway routing logic (e.g. Bedrock fallback).
- **Net new features** – UX additions, new screens and capabilities, setup scripts, scaffolding whole new products.


##


Why it doesn't feel like chaos


You'd expect engineers to hate this. A bot opening hundreds of PRs for them to review, non-technical people shipping AI-generated code to production – that sounds like[a mess](https://posthog.com/blog/stop-ai-slop)


!


It's not, and here's why: the PostHog Slack app understands your codebase and your product data. It doesn't merge its own work. It follows a rigorous review process, and runs a flurry of tests (which most engineers would rather not do).


If CI fails, it fixes the failure. If you add a review comment, it addresses the comment. It pokes you when the PR sits idle until the merge is clean or it runs out of ideas.


The code generation that lands from one sentence prompts are surprisingly good. So good that even[Cory Watilo](https://posthog.com/community/profiles/30200)


, our resident webmaster, is pleased to @PostHog.


**The resulting PR**


[PostHog/posthog#59432](https://github.com/PostHog/posthog/pull/59432)


##


For when the question needs an answer (not code)


Generating PRs with @PostHog in Slack is so easy it feels illegal, and it's a glimpse at what the[self-driving product](https://posthog.com/blog/self-driving-product)


means in practice.


Despite being a coding agent, it won't answer every @mention with a PR. Every @PostHog mention runs through a two-stage classifier before any work happens:


1. **Task classifier** – Does this request need repo access, or is it analytics/data/config?
2. **Repo router** – If it does require code generation, which GitHub repo does it go to?


Both classifiers use Claude Haiku (tiny, fast, cheap) and we track latency and cost with[AI Observability](https://posthog.com/docs/ai-observability)


.


This simple routing is why Paul's prompt " *@PostHog can you generate a team photo of team blitzscale as the Spice Girls* " – was correctly classified as non-actionable work.


Paul was disappointed, but the bot knew what it was doing.


##


Try it


The PostHog Slack app is in beta – we skipped alpha by[dogfooding](https://posthog.com/product-engineers/dogfooding)


it to the extreme.


It's free to install, and free to uninstall when you realize this means you can ship production code from your phone (which, frankly, might be too much power for anyone).


[Connect Slack](https://posthog.com/docs/slack/setup)


##


FAQ


Is it free to use?


The agent that runs behind @PostHog mentions consumes PostHog AI credits for the LLM work it does. This includes tokens used when planning a task, editing files, reasoning about your data. For credit mechanics, the free monthly tier, billing limits, and the live pricing calculator, see[PostHog AI pricing](https://posthog.com/docs/posthog-ai/pricing)


.


Why use this over Cursor, Claude Code, or Codex?


Other tools only write code. PostHog is connected to your product data, so you can start from a problem – tag @PostHog with a message like "conversion dropped on signup". It finds the cause in your analytics and replays, explores solutions, and opens a PR to propose a fix. No hopping between a dashboard, a replay tab, and your editor.


Is it a better coding model?


No. It runs the same frontier models everyone else does. The difference is context – an agent that can read your funnels, replays, and errors is working from evidence, not guessing at what matters.


Is it an analytics agent or coding agent?


Both, in one thread. Explore data and build in the same conversation.


Does it just open a PR and walk away?


No. It sticks with the PR through failing checks and reruns until it's mergeable – we call that babysitting. On a big repo, getting through CI is often more work than the code.


Will it touch our whole codebase?


It only touches repos you connect, and every change goes through a PR you review. Nothing merges without a human saying yes.


Do I need to be an engineer to use it?


Not at all! It's a Slack message. Our sales and marketing teams regularly use it make fixes to our main app. Describe the problem or idea, and the agent takes it from there.


Won't it try to code every message?


It classifies each @mention first – code task or data question, and which repo. Data questions get answered, not turned into PRs.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
