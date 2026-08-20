---
schema_version: "1.0.0"
document_id: "00022216f9a3295effdf14b6cccf9ef9f503f8dd563c189e7199ebdc7f279e6b"
company_key: "yc-metorial"
company: "Metorial"
source_id: "yc-metorial-news-import-9c457a3d81d4"
canonical_url: "https://metorial.com/blog/share-ai-agent-skills"
published_at: "2026-08-11T09:00:00+00:00"
first_seen_at: "2026-08-12T08:59:22.495831+00:00"
fetched_at: "2026-08-12T08:59:23.098077+00:00"
content_hash: "sha256:1794b6d9a127dcf6e3ab38473dd40c840b1d3a102fa17355b142205c573df914"
---

# AI Skill Sharing and Versioning for Teams (2026)

TL;DR


A skill is a written workflow that teaches an agent how a team already does a task, and at most companies past a certain size, the same one gets rebuilt slightly differently by every team that needs it. This happens because there's no shared way to write, publish, or update one. Metorial's Skills product covers that gap end to end: anyone writes a skill in a collaborative document editor with no code, publishes it once to a company marketplace installable from a single link, and every copy stays in sync automatically, so nobody is left working from a stale version. Access to write, publish, or use a skill is governed the same way as everything else in Metorial: by identity and policy, not by who happens to have a GitHub account.


Somewhere in most companies, a skilled process, how to triage a refund request or qualify a lead, already exists in someone's head, and that person has probably already tried to teach an AI agent how to do it. The problem shows up once a second team needs the same thing. Without a shared place to write that workflow down and hand it to another team's agent, the second team writes its own version, usually a little different, and usually undocumented anywhere the third team could find it. Multiply that by five teams and a company has five slightly different versions of the same skill, none of them a source of truth, and no way to update all of them at once when the underlying process changes.


This isn't unique to AI work. Asana's Anatomy of Work Index, a global survey of more than 10,000 knowledge workers, found the average knowledge worker spends 209 hours a year on duplicative work, work that's already been done somewhere else in the company and gets redone because nobody could find it. Skills for AI agents inherit the same problem, just with a new name attached: the workflow needs an agent to follow it, and the fix has to work for people who've never opened a code editor.


## What is a skill, and how is it different from a prompt or an integration?


A prompt is a single instruction. An integration connects an agent to a system, like Salesforce or Zendesk. A skill is a reusable workflow that ties the two together: the steps for doing a task correctly, plus the specific tools an agent needs to carry them out. Metorial's own template for one reads close to how a person would explain a new job to a teammate: set the scene, list what has to be true before starting (a connected integration, a policy document to reference, etc.), then walk through the steps for each situation that comes up.


That framing also decides who's allowed to write one. A prompt is small enough that anyone types it into a chat box. An integration usually needs someone who can configure an API connection. A skill sits in between: it needs to be written by the person who knows the process, whether or not they can code.


## Who should be allowed to write one?


The person who handles refunds, qualifies leads, or reviews contracts is also the person who should be writing the skill that teaches an agent to do the same thing, and at most companies, that person doesn't have a GitHub account and has never opened a terminal. Metorial's[skills](https://metorial.com/skills) are written in a collaborative document editor, the same way someone would write a guide for a new hire, with no repository and no command line involved. Multiple people can work on the same skill together, the way they'd co-edit a shared document.


## How does a skill reach an agent once someone's written it?


Writing a skill is only half the problem. The other half is getting it in front of the people and agents that need it, without someone copying files around by hand. Once a skill is published, it goes into a company skill marketplace, a centralized catalog installable from a single link or file. An admin adds that one link, and the skills someone is allowed to use show up automatically inside the agent tools they already have open: Claude Code, Claude Cowork, Codex, Cursor, Copilot, or Antigravity, delivered as a skill plugin native to each tool's own plugin system. There's nothing to download separately and nothing to wire up by hand.


A skill also brings its own tools with it. If a skill needs a Zendesk integration to look up a ticket, that connection travels with the skill through Metorial's[Magic MCP](https://metorial.com/magic-mcp) link, governed by the same access rules as everything else, so installing a skill doesn't hand an agent more access than the person installing it already has.


## What happens when a skill changes after people are already using it?


A workflow that changes and doesn't propagate is worse than one that was never written down, because now some people are following the old version without knowing it's stale. Metorial's skills are edited in place in a collaborative editor, and publishing an update rolls the new version out to everyone automatically. Nobody reinstalls anything, and nobody's stuck running an outdated copy because they missed an email about the change. A skill has one current version, shared everywhere it's used, rather than a dozen team-specific copies quietly drifting apart from each other.


## Does a skill stay locked inside Metorial, or can it leave?


A workflow a team builds shouldn't be trapped in one vendor's product just because that's where it was written.[Skill export](https://metorial.com/features/skill-export) publishes everything in a company's skill catalog as a marketplace on the source control platform it already uses, GitHub, GitLab, Bitbucket, or Azure DevOps, installable from a single git URL. The skill itself doesn't have to be rebuilt to get there: it's exported as written in the collaborative editor, so the same non-technical person who wrote it can also be the one who publishes it, without an engineer doing the export on their behalf.


## Who decides who can write, publish, or use a skill?


Making it easy for anyone to create a skill doesn't mean everyone should have access to all the skills in the company. Metorial applies the same access policies to skills that it applies to every integration and tool: per-user and per-group rules decide who can author a new skill, who can publish one to the company marketplace, and who can install and run one that already exists. Those policies are tied to a company's existing identity provider through[SSO and SAML](https://metorial.com/access-control) , so a skill's access rules follow the same accounts and the same process as everything else a person can reach.


## Can an agent following a skill do more than the person running it?


No. Every connection a skill makes runs on the identity of the person who installed it, not a shared credential the skill carries around. An agent following a skill only ever reaches what that person is already allowed to reach, so publishing a skill widely doesn't quietly widen anyone's access. The tools a skill packages are scoped exactly the same way a manually connected integration would be.


## What does this change for a team without engineers to spare?


The teams that lose the most to duplicated work are usually the ones without a developer to spare for the problem: support, sales, and operations, not engineering. A workflow that has to be exported to a repository, reviewed through a pull request, and merged before it reaches anyone is a workflow that only gets built by people already comfortable doing that. A workflow that's written in a document editor, published to a marketplace, and installed with a link is a workflow the person who does the job can build and maintain themselves. That's the difference between a company where skill sharing depends on engineering time being available, and one where it doesn't.


## FAQ


**Do I need to know how to code to build a skill?**


No. Skills are written in a collaborative document editor, the same way you'd write a guide for a new teammate. The person who knows the work can build and maintain it without an engineer.


**How is a skill different from a prompt?**


A prompt is a single instruction. A skill is a reusable workflow: the steps for a task, plus the specific tools an agent needs to complete it, so an agent follows the same process every time instead of being re-prompted from scratch.


**What happens to everyone using a skill when it's updated?**


Publishing an update rolls the new version out automatically. Nobody reinstalls anything, and there's no way to end up running an old copy by accident.


**Can a skill leave Metorial if a team wants to use it elsewhere?**


Yes. Skill export publishes a company's skills as a marketplace on GitHub, GitLab, Bitbucket, or Azure DevOps, installable from a single git URL, exported exactly as built in the collaborative editor.


**Which AI tools can use a shared skill?**


Skills are delivered as plugins into the agent tools teams already use: Claude Code, Claude Cowork, Codex, Cursor, Copilot, and Antigravity, kept in sync automatically as the skill changes.


**Can a skill let an agent do more than the person running it is allowed to do?**


No. A skill's tool access runs on the identity of the person who installed it, so it can never exceed what that person is already allowed to reach.


## Sources


1. [Asana Anatomy of Work Index, "How work about work gets in the way of real work"](https://asana.com/resources/why-work-about-work-is-bad)
2. [Metorial Skills](https://metorial.com/skills)
3. [Metorial Skill Export](https://metorial.com/features/skill-export)
4. [Metorial Skill Plugins](https://metorial.com/features/skill-plugins)
5. [Metorial Access Control](https://metorial.com/access-control)
