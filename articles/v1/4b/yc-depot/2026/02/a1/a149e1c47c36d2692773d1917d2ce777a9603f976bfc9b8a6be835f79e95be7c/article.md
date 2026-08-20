---
schema_version: "1.0.0"
document_id: "a149e1c47c36d2692773d1917d2ce777a9603f976bfc9b8a6be835f79e95be7c"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-depot-skills"
published_at: "2026-02-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:57:40.692150+00:00"
content_hash: "sha256:7e87e984939af2988139bbe64f47429a503aefa93aecbda31dfb5b719c120d4e"
---

# Now available: Depot skills

We've been using AI coding agents heavily at Depot. Claude Code, Codex, Cursor — are tools widely used across the entire team. We've yet to settle on one and instead give folks the choice of whichever one suits their style best.


All of them are good. They've gotten exponentially better since the end of last year and that momentum hasn't let up at all as we start to head into Spring.


We've written a lot of internal tools, documentation, and skills that power a lot of our agent led workflows. Going through that process, we noticed a common pattern: the better context and instructions you can feed into an agent, as it needs it, the better the results are.


So we started writing skills to help agents use Depot. Today we're publishing[Depot skills](https://github.com/depot/skills) — a collection of skill files that teach AI coding agents how to leverage Depot for container builds, GitHub Actions runners, Depot CI (beta), and more.


## What skills are available?


We built four skills to cover the main things you'd want an agent to do with Depot:


-


**Container Builds** — Teaches agents how to use` depot build` and` depot bake` , handle multi-platform builds, manage caching, work with the Depot Registry, and migrate from plain old` docker build` or` docker buildx` commands.


-


**GitHub Actions Runners** — Shows agents how to leverage Depot GitHub Actions runners,` runs-on` labels available, how to install the GitHub application, how caching works, egress filtering, dependabot integration, ssh debugging, and how to connect runners to your own tailnet.


-


**Depot CI** — Covers Depot CI (currently in beta),` depot ci migrate` , secrets and variables, running workflows, and GitHub Actions compatibility.


-


**General** — CLI installation, authentication (tokens and OIDC), project setup, org management, the Depot API, and pricing.


## Getting started


Install Depot skills into your agent of choice with[skills.sh](https://skills.sh/) :


```text
npx skills add depot/skills
```


That's it. skills.sh handles the rest — detecting your agent and installing the right files in the right place. For more details and manual installation options, check out the[skills repo](https://github.com/depot/skills) .


## Why we built this


Honestly, because we were tired of watching agents confidently generate the wrong thing.


When you have Depot as part of your stack, you want agents that know how to use it. Not agents that fall back to` docker build` because they can't remember the right flag syntax, or that generate a GitHub Actions workflow with the wrong runner label. The fixes are quick, but they add up. Every correction is a little friction that shouldn't exist.


These skill files give agents the context they need to get it right the first time.


## What could be better?


There are a few things we know about and are working on improving:


-


**Coverage is still growing.** The four skills cover the most common Depot use cases, but there are corners we haven't documented yet. If something's missing that your agents keep getting wrong, open an issue or a PR on the[skills repo](https://github.com/depot/skills) .


-


**Agent support varies.** Skills work best with agents that explicitly support the SKILL.md convention — Claude Code, Codex, Cursor. If you're using something else, you can still reference the skill files manually, but the install path is less turnkey. Skills are also sometimes notorious for not being automatically used by agents. If you see that case, we find that adding **using the Depot skills** to your prompt is a good forcing function to get them to use the skills.


-


**Skills evolve with the product.** As we ship new Depot features, the skills need updating. We'll keep them current, but there may be a lag between a new feature landing and the skill reflecting it.


## What's next


We're going to keep improving these skills as we build more of Depot. And if you want to contribute — whether it's fixing a wrong flag or adding coverage for a use case we missed — pull requests are very welcome.


We can't wait to see what you build with Depot skills. If you have questions, feedback, or run into something your agent is still getting wrong, hop into our[Discord community](https://discord.gg/MMPqYSgDCg) and let us know!


## Related posts


- [The bottleneck has shifted from writing code to integrating it](https://depot.dev/blog/the-bottleneck-has-shifted)
- [Faster Claude Code agents in GitHub Actions](https://depot.dev/blog/claude-code-in-github-actions)
- [Using AI as my engineering copilot (not autopilot)](https://depot.dev/blog/using-ai-as-my-engineering-copilot-not-autopilot)
- [How we automated GitHub Actions runner updates with Claude](https://depot.dev/blog/how-we-automated-github-actions-runner-updates-with-claude)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
