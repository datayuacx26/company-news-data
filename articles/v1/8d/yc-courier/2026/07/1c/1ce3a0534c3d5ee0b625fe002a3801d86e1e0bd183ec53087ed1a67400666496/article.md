---
schema_version: "1.0.0"
document_id: "1ce3a0534c3d5ee0b625fe002a3801d86e1e0bd183ec53087ed1a67400666496"
company_key: "yc-courier"
company: "Courier"
source_id: "yc-courier-news-import-df9818472bef"
canonical_url: "https://www.courier.com/blog/courier-skills"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T10:01:06.714121+00:00"
fetched_at: "2026-07-30T10:01:08.550502+00:00"
content_hash: "sha256:5bd2da722ef7f24e9fb9a1c0b5b0d07e857d0fdab2fdab6c790ab0ca9ab640e4"
---

# Courier Skills: teach your AI agent to build with Courier

*One command installs verified Courier knowledge into Claude Code, Cursor, Codex, or whatever your team runs.*


We rebuilt[Courier Skills](https://github.com/trycourier/courier-skills) and it's available now. It's an agent skill that teaches your AI coding assistant how to use Courier: the right primitive for each use case, the exact payload shapes, and the mistakes that produce unhelpful errors.


```text
npx skills   add   trycourier/courier-skills
```


No API key, no config file, no signup. A few seconds.


## How Courier Skills makes building with AI faster


Your agent can write a week of integration code in an afternoon. What slows it down isn't the writing, it's the details it can't see: which payload shape an endpoint accepts, which field is required, what a vague error actually means. Each of those becomes a guess, and every guess costs a round trip.


We'd rather you moved at the speed your agent is capable of. That's the entire reason to package this knowledge where it can reach it.


Here's what the alternative looks like. One customer last week, mid-build:


> Hey mate. I'm now building the journeys and preparing them for handover to my engineering team. I've been trying to build my journeys with old mate Claude via API, but we're hitting some errors. Could you please give me a steer here?


The steer they needed was a JSON shape. One node type kept rejecting their payload, and the error never changed:


> We've tried roughly 15 different shapes and every single one comes back with that exact same generic "Invalid input" message, with no indication of what's actually expected.


Fifteen attempts, one error string. So they asked for help:


> We have three journeys ready to go the moment we know the right shape. Happy to share full request/response logs or hop on a call if that's faster.


We wrote back suggesting they install the skill and start there, and said we'd take a deeper look either way.


> Legend, I'll give that a go and let you know.


Then, minutes later:


> Fixed, no need to follow up with your team. Thanks mate!


So what changed? Their agent's own account of it is the clearest explanation of what a skill is for.


The answer was in the skill's[Journeys](https://www.courier.com/docs/platform/journeys/journeys-overview) guide, in a Common Mistakes list at the top of the file, naming that exact bug verbatim:


> Wrapping a single condition in an extra array (` \[\[...\]\]` ) or using non-string values


It found that with one` grep` , before making another API call. No guessing, no sixteenth attempt.


The reason it worked is ordering, not content. That warning sits in the first thirty lines of the file, under a heading an agent searches when something fails. You can't look up a concept you don't have a name for yet, so the fix has to be findable using the words you already have: the error you got, or the thing you tried. It was.


A support reply would have resolved one error. The guide resolved the category.


Keep talking to us anyway. Some problems need a human, and that customer's escalation told us two things we needed to know. But when the answer is a shape someone already worked out, waiting on a person is the wrong shape of solution.


## What's in the rebuilt Courier Skills


35 reference files covering channels, Journeys, templates, preferences, routing, and delivery debugging. Your agent reads one or two per task, not the whole tree. Four things define the rebuild.


**Failure modes come first.** Every guide opens with Rules and Common Mistakes before any explanation or example. A stuck agent searches before it reads, so what unblocks it has to be in the first thirty lines, not paragraph forty.


**Errors are indexed.** Guides map the literal error strings the API returns back to their causes, so your agent can search with the string it already has instead of guessing what to call the problem.


**It routes instead of dumping.**` SKILL.md` is a table: one row per reference, keyed by what you're doing. This is why the new version is *smaller* than the old one. Attention is the scarce resource, not storage, so a sprawling reference makes your agent read more and find less.


**It says when not to trust it.** Caches go stale, so the skill ranks its own sources, starting with the installed SDK's type definitions because they're local and true for the version you actually have. If a live source contradicts the skill, the live source wins and the agent is told to say so rather than quietly pick one.


It also carries the rules you can't get wrong, as constraints rather than suggestions. Never batch or delay an OTP. Mask contact details in security alerts. Use idempotency keys where a duplicate would hurt.


## Works with Claude Code, Cursor, Codex, and more


Agent skills are a shared open format, so this isn't a Claude Code feature others imitate. The same command covers Claude Code, Cursor, Codex, GitHub Copilot, Gemini CLI, Windsurf, Amp, Zed, Cline, and OpenCode. The installer detects what you have and writes to the right place, or takes` -a` to target directly:


```text
npx skills   add   trycourier/courier-skills -a claude-code -a cursor
```


A dozen of them now share` .agents/skills/` as their project directory, so one committed install increasingly covers a whole team regardless of what each person runs.


Claude Code can install it as a plugin instead, which self-updates and ships the Courier docs MCP alongside it:


```text
/plugin marketplace   add   trycourier/courier-skills    /plugin   install   courier@courier-skills
```


## Courier Skills vs the Courier MCP server


They do different jobs and you want both.


**An MCP server gives your agent capability:** which operations exist and the ability to run them against your workspace. **A skill gives your agent knowledge:** what the payload looks like and which shapes fail.


The customer above had our MCP server connected the whole time. It offers a` replace_journey` tool, and that tool's` nodes` parameter is typed as an object with nothing declared and anything allowed. It could execute the call. It could never describe the payload, because a tool schema is a call signature, not a payload grammar. As their agent put it: MCP for doing, skills for knowing the shape.


## Best practices for working with agent skills


**Install globally for yourself, per-project for your team.** Add` -g` for your user directory. Leave it off and it installs into the project, where you can commit it so teammates and CI agents get the same knowledge.


**Let it route. Don't paste it.** The skill is built to be read selectively. Dumping all 35 files into context buries the part that matters.


**Give it the error verbatim, then name the move.** The guides are indexed on those strings, so the raw response beats your paraphrase. "Check the Common Mistakes list before you try again" works well, because agents default to retrying variations, which is the wrong move against a generic error.


## Install Courier Skills


1. Install it:` npx skills add trycourier/courier-skills`
2. Ask your agent to build something. "Add a welcome email with Courier" is a fine first test.
3. When it gets stuck, point it at the Common Mistakes list before it starts guessing.


Or read the[Courier Skills docs](https://www.courier.com/docs/tools/courier-skills) to see what's covered. Already have an older copy? Reinstall with the command above, or run` /plugin update courier@courier-skills` . The layout changed in this release, so a fresh install beats a merge.


Found a gap?[PRs are welcome.](https://github.com/trycourier/courier-skills) The note you leave saves the next person an afternoon.


## Frequently asked questions


### What is Courier Skills?


Courier Skills is an agent skill for the Courier notification platform: a set of markdown reference files that teach an AI coding agent how to send notifications, build Journeys, manage preferences, and debug delivery with Courier. It's free, MIT licensed, and open source.


### Which coding agents does Courier Skills work with?


Claude Code, Cursor, Codex, GitHub Copilot, Gemini CLI, Windsurf, Amp, Zed, Cline, OpenCode, Continue, Goose, Roo Code, Kiro, Junie, Antigravity, Warp, Droid, Replit, and Augment, among others. Agent skills are a shared open format, so` npx skills add trycourier/courier-skills` is the same command everywhere and the installer writes to whichever directory your agent expects.


### How do I install Courier Skills?


Run` npx skills add trycourier/courier-skills` . Add` -g` to install into your user directory for every project, or leave it off to install into the current project so you can commit it for your team. Claude Code users can install it as a plugin instead with` /plugin marketplace add trycourier/courier-skills` followed by` /plugin install courier@courier-skills` .


### Do I need a Courier API key to use it?


No. The skill is a set of markdown files your agent reads locally. You'll need an API key to actually send notifications, but not to install or use the skill.


### What is the difference between an agent skill and an MCP server?


An MCP server gives your agent capability: which operations exist and the ability to run them against your workspace. An agent skill gives your agent knowledge: what the payload looks like and which shapes fail. A tool schema describes a call signature, not a payload grammar, so the two are complements rather than alternatives. The Claude Code plugin ships the Courier docs MCP alongside the skill.


### What does the skill cover?


Email, SMS, push, in-app inbox, Slack, Microsoft Teams, and WhatsApp, plus Journeys, templates and Elemental, preferences, routing strategies, brands, audiences, tenants, and delivery debugging through the CLI. 35 reference files in total.


### How do I update it?


Re-run the install command, or` /plugin update courier@courier-skills` if you installed the plugin.
