---
schema_version: "1.0.0"
document_id: "e05d1bc698a5143cbba517f2c7b1cfb16084275e845addf546716102c70646f8"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/gitlab-duo-cli-generally-available/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:0b68ff76a8a847f90e0924b80d1d12cf0fa1640b47caf2df6aaa7a038d337298"
---

# Bring GitLab Duo Agent Platform to your terminal

Most of the work for software delivery doesn’t happen only in the editor. Pipelines fail. Tests break. Vulnerabilities show up. And a lot of that work starts and ends at the command line.


Agentic AI in the terminal that only understands code can’t help with any of that. A standalone assistant doesn’t know your agents across the entire software lifecycle, your permissions across many projects in your organization, or a specific project context you’ve already set up in GitLab.


That’s what changes in[GitLab 19.2](https://docs.gitlab.com/releases/19/gitlab-19-2-released/) .[GitLab Duo CLI](https://docs.gitlab.com/user/gitlab_duo_cli/) is generally available, and it brings[GitLab Duo Agentic Chat](https://docs.gitlab.com/user/gitlab_duo_chat/agentic_chat/) straight into your terminal. Unlike tools bolted on from the outside, it already knows your project, your pipelines, and your agent setup. You can use it interactively when you’re exploring and building, or headless when you want it running in a job or script.


As a result, developers stay in the shell where the failure showed up, and their work continues across the terminal, the UI, and your editor. Platform teams govern rollout like everything else on GitLab, and agentic help finally covers more of the delivery lifecycle than “write this function.”


## Why agentic AI in the terminal stopped at code


Agentic tools matured first where demos look best: editing files. The lifecycle after the commit is messier and more operational. When the failure is a pipeline, a dependency, or a CI config, context lives in GitLab — not in a coding agent’s training set or local context.


Teams that tried to bridge that gap with generic CLI assistants paid another cost: no shared admin control, no MCP/setup diagnostics aligned with the platform, and no single identity model with the rest of the agentic software lifecycle. Agentic work in the terminal stayed stuck on coding, instead of helping across the rest of delivery.


## What teams get when Duo CLI is generally available


- **Stay in the terminal for the work that already lives there.** Dig into how the codebase fits together, build and refactor, chase down pipeline failures, tidy up CI/CD, and knock out multi-step tasks — without jumping to the browser for every answer.
- **Pick up where you left off, on any surface.** Sessions are shared across the GitLab Duo CLI, the GitLab UI, and editor extensions. Start in the browser. Continue in your shell. Same conversation.
- **Plan first, then build.** Interactive mode works like Agentic Chat: Plan mode looks around without changing anything, build mode makes the changes. Need it unattended? Headless mode drops into CI jobs and scripts.
- **Turn it on when you’re ready.** Duo CLI works on GitLab.com, GitLab Self-Managed, and GitLab Dedicated. On Self-Managed and Dedicated, admins can switch instance access on or off. Anywhere you use it, developers can run` /doctor` to check their setup and` /mcp` to see their MCP configuration.


**See GitLab Duo CLI in action:**


## How Duo CLI works


The easiest path is through the[GitLab CLI](https://docs.gitlab.com/cli/duo/cli/) : Run` glab duo cli` and` glab` handles authentication for you. You can also install and run` duo` as a standalone tool with a personal access token. Both setups support the same modes and capabilities.


- **Interactive mode** — chat in the terminal, and approve tools before anything runs. Explore the codebase, plan a fix, then switch to build mode when you’re ready to make changes.
- **Headless mode** — non-interactive execution for runners, scripts, and automation. Use` glab duo cli run --goal` or` duo run --goal` .


For instance, when a pipeline fails, ask from the same shell:


```text
$   glab   duo   cli
>   The pipelines in MR 23 are failing. Please help me fix them.


```


Duo CLI looks at what’s going on, figures out what went wrong, and proposes changes you can review before applying. It follows your custom instructions (` chat-rules.md` ,` AGENTS.md` ,[SKILL.md](http://skill.md/) ) as you scale, and you can extend its interactive sessions with[custom slash commands](https://docs.gitlab.com/user/gitlab_duo_cli/#custom-slash-commands) .


## Start using Duo CLI today


Head over to the[GitLab Duo CLI documentation](https://docs.gitlab.com/user/gitlab_duo_cli/) to install and authenticate — and if you already use the GitLab CLI, start with[glab duo cli](https://docs.gitlab.com/cli/duo/cli/) .


New to GitLab?[Start a free trial of GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) . Already on Premium or Ultimate?[Turn on Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/turn_on_off/) and use the[GitLab Credits included with your subscription](https://docs.gitlab.com/subscriptions/gitlab_credits/#included-credits/) .


##


Is AI achieving its promise at scale?


[Get your AI maturity score](https://about.gitlab.com/assessments/ai-modernization-assessment/)


Quiz will take 5 minutes or less


## More to explore


[View all blog posts](https://about.gitlab.com/blog/)


[AI Claude Opus 5 on GitLab: Reasoning built for the hard tasks](https://about.gitlab.com/blog/claude-opus-5-on-gitlab-duo-agent-platform/)[AI Modernize Java with Cursor and GitLab](https://about.gitlab.com/blog/modernize-java-with-cursor-and-gitlab/)[AI GitLab Transcend Hackathon: What developers built on GitLab Orbit](https://about.gitlab.com/blog/gitlab-transcend-hackathon-orbit/)


## We want to hear from you


Enjoyed reading this blog post or have questions or feedback? Share your thoughts by creating a new topic in the GitLab community forum.


[Share your feedback](https://forum.gitlab.com/)
