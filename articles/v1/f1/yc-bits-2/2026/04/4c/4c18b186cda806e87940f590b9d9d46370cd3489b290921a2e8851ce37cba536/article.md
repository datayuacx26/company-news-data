---
schema_version: "1.0.0"
document_id: "4c18b186cda806e87940f590b9d9d46370cd3489b290921a2e8851ce37cba536"
company_key: "yc-bits-2"
company: "Bits"
source_id: "yc-bits-2-news-import-8ae56e6aae1c"
canonical_url: "https://klausai.com/blog/how-to-write-custom-openclaw-skills-with-real-examples/"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-24T21:07:46.493176+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:2dcc2711e02d48ff56044517f4ea28938e8d218def17201d9731696f8b075d9f"
---

# How to Write Custom OpenClaw Skills (With Real Examples)

# How to Write Custom OpenClaw Skills (With Real Examples)


I write skills for Klaus. Not install them from ClawHub - write them. The skills I build for Klaus customers often get replicated into OpenClaw core shortly after, because the patterns solve real problems that other users hit too.


[ClawHub](https://clawhub.ai/) has over 53,000 skills now. The good ones are written by people who run the automation in production and iterate when things break. A SKILL.md file is just markdown, but the difference between a skill that works in testing and one that holds up in production is in the details nobody tells you about.


This is how I actually write skills. What patterns hold up, what breaks, and the mistakes I’ve made along the way.


## What a Skill Actually Is


A skill is a folder with a` SKILL.md` file. That’s it. No SDK, no API, no compilation step. You’re writing instructions in markdown that your agent reads and follows at runtime. If you’ve read our[best skills for business](https://klausai.com/blog/best-openclaw-skills-for-business) guide, you know what skills can do. This article is about how to build your own.


The` SKILL.md` needs YAML frontmatter with at least a` name` and` description` :


```text
---
name  :   my-skill
description  :   One sentence explaining what this skill does.
---


Instructions go here in markdown.
```


Here are the frontmatter fields you’ll actually use:


Field Required What it does


` name` Yes Skill identifier. Lowercase, hyphens, under 64 characters.


` description` Yes One-line explanation. This is what the agent reads to decide relevance.


` user-invocable` No Controls slash-command visibility (default: true).


` disable-model-invocation` No Prevents automatic triggering. Set to true for manual-only skills.


` metadata` No Single-line JSON (or JSON5) for gating and display. See below.


The` metadata` field deserves a closer look. It wraps everything under` metadata.openclaw` :


Key What it does


` emoji` Single emoji shown next to the skill name (e.g.` "💰"` ,` "📄"` ).


` requires.bins` Array of binaries that must be on PATH.


` requires.anyBins` Array of alternative binaries — any one must exist.


` requires.env` Array of environment variables that must be set.


` requires.config` Array of required config keys.


` os` Array of supported operating systems (e.g.` \["darwin"\]` ,` \["linux"\]` ).


` primaryEnv` Primary env var (e.g.` "NOTION_API_KEY"` ) — used for setup prompts.


` install` Array of install specs (` kind` :` "brew"` ,` "node"` ,` "go"` ,` "uv"` ,` "download"` ).


If any` requires` condition fails, the skill is invisible — it won’t load into the agent’s context at all.


Skills live in one of three places, in order of[precedence](https://docs.openclaw.ai/tools/skills) :


1. **Workspace skills** (` <project>/skills/` ) - highest priority, project-scoped
2. **Managed skills** (` ~/.openclaw/skills/` ) - shared across all sessions
3. **Bundled skills** - shipped with the OpenClaw install


If you put a skill with the same name in a higher-priority location, it overrides the lower one. Useful for patching a bundled skill without changing the original.


One thing to keep in mind: each skill adds roughly[24 tokens of overhead](https://docs.openclaw.ai/tools/skills) to every conversation. That cost is constant - the skill’s description loads into the system prompt whether or not the agent uses it. Ten focused skills is better than fifty untested ones.


## The Minimum Viable Skill


The simplest useful skill I’ve written is one that grants free API credits to a user. It’s a single wrapper around a shell script, but it saves me from remembering the exact incantation every time.


Here’s the complete SKILL.md:


```text
---
name  :   grant-credits
description  :   "Grant free credits to a user's OpenClaw instance"
metadata  : {  "openclaw"  :{  "emoji":"💰"  ,  "requires"  :{  "env"  :[  "OPENROUTER_PROVISIONING_KEY"  ]}}}
---


# Grant Free Credits


Grant free OpenRouter credits to a user. Updates both the database and syncs to OpenRouter.


## Usage


```  bash
bin/grant-credits [--no-email] <email-or-orgId> <amount-usd> <reason>
```


The reason is **required** — it’s shown to the user in the notification email.


Use` --no-email` to skip sending the notification email.


## Examples


```text
bin/grant-credits   sanjo@example.com   10   "Apology for checkout bug"
bin/grant-credits   --no-email   cmldqom4c00yo01l75ajmilnf   10   "Beta tester bonus"
```


## What it does


1. Looks up the org and its credit account in prod DB
2. Increments` openrouterCreditLimitUsd` on` org_credit_account`
3. Creates a` credit_transaction` record with type` "manual_adjustment"`
4. Calls OpenRouter API to sync the new limit
5. Sends a notification email to the user via Resend


## Rules


- Never grant more than $50 without explicit confirmation.
- If the org or email is not found, report the error. Do not guess org IDs.
- Always include a reason — it goes in the customer email.


```text


Walk through the pieces:


**The description** is specific. "Grant free credits to a user's OpenClaw instance" won't accidentally trigger when someone asks about billing in general or wants to check a balance. Vague descriptions like "manage credits" would cause misfiring.


**The `requires.env` gate** ensures the skill only loads when the provisioning key is available. On a machine without it, the skill is invisible — no confusing error at runtime.


**The workflow** is a single command with exact flags. The agent doesn't need to figure out the right incantation — it reads the usage pattern and fills in the arguments from context.


**The rules section** is where most people cut corners. Without explicit guardrails, the agent fills gaps with its own judgment. "Never grant more than $50 without confirmation" is there because an earlier version of this skill didn't say that, and the agent happily granted $200 in credits based on a customer's vague complaint about "not having enough."


## Patterns That Hold Up


After writing and iterating on skills that run daily in production, these are the patterns I keep coming back to.


### Be Specific About Triggers and Stop Conditions


The single most common problem with custom skills: they trigger when you don't want them to.


The description field in your frontmatter is what the agent reads to decide whether a skill is relevant. "Help with operations" will trigger on anything vaguely operational. "Generate weekly ops report from logs in /var/log/openclaw" won't.


Negative triggers matter too. If you have a skill that summarizes logs and another that monitors logs for errors, the agent might pick the wrong one. Make the descriptions mutually exclusive:


- Good: "Summarize weekly activity from OpenClaw session logs"
- Good: "Alert on error patterns in OpenClaw gateway logs"
- Bad: "Work with OpenClaw logs"


Once the skill starts running, it also needs to know when to stop. "Read the last 7 days of logs" is a boundary. "Read all the logs" is an invitation to process months of data and burn through your token budget.


### Read-Only First, Actions Later


Every skill I write starts as a reporting tool. It reads data, formats it, and shows you the result. No writes, no sends, no deletes.


Once the logic is solid, I add actions. "Summarize API usage" becomes "Summarize API usage and flag users who exceeded their allocation." Then eventually: "Summarize API usage, flag over-allocation, and draft a notification to the affected users."


The progression matters because a skill that formats a report wrong is an inconvenience. A skill that sends an incorrect notification to a customer is a support ticket.


### Put Constants in Config, Not the Skill


Hardcoded paths work on your machine. They break on everyone else's.


OpenClaw skills support `requires.env` for gating on environment variables and `{baseDir}` for referencing files relative to the skill folder ([OpenClaw docs](https://docs.openclaw.ai/tools/skills)). If your skill needs an API endpoint, a directory path, or a threshold value, it belongs in an environment variable or the skill's config entry in `openclaw.json`:


```json
{
"skills": {
"entries": {
"my-skill": {
"enabled": true,
"env": {
"REPORT_DIR": "/path/to/reports",
"ALERT_THRESHOLD": "85"
}
}
}
}
}
```


Someone else can then use your skill without editing the SKILL.md. The values inject into the environment at runtime and don’t leak into the shell afterward ([DataCamp](https://www.datacamp.com/tutorial/building-open-claw-skills) ).


### The Guardrails Section Is Not Optional


Without a Rules section, the agent makes its own judgment calls. Sometimes those calls are fine. Sometimes the agent decides that the best way to fix a slow instance is to kill processes it thinks are unnecessary.


Patterns that belong in every skill’s guardrails:


- **Never fabricate data.** “If data is missing, say it’s missing. Do not estimate.”
- **Scope boundaries.** “Only process files from the last 7 days.” or “Only read from the specified directory.”
- **Destructive action blocks.** “Never delete files. Never restart services. Never modify configs.”
- **Error handling.** “If a command fails, include the command and error text in the output. Do not retry silently.”


These read like obvious rules. They are obvious rules. The agent still needs them written down because it doesn’t share your assumptions about what’s obvious.


## A Skill That Wraps a Script


The second pattern that comes up constantly: skills where the SKILL.md delegates to a bundled script. The grant-credits skill above is already an example (it calls` bin/grant-credits` ), but let’s look at the folder structure for a skill that bundles its own script.


Here’s what that looks like:


```text
~/.openclaw/skills/usage-report/
+-- SKILL.md
+-- scripts/fetch_usage.sh
```


The key pattern: the SKILL.md references its script with` {baseDir}` :


```text
bash   {baseDir}/scripts/fetch_usage.sh   --days   7   --endpoint   "  $MONITOR_ENDPOINT  "
```


A few things to notice:


**` {baseDir}` resolves to the skill folder at runtime.** The SKILL.md and the script stay together as a unit. Move the skill folder anywhere and it still works.


**The` requires` field gates the skill.** It only loads if the specified binaries and environment variables are available ([OpenClaw docs](https://docs.openclaw.ai/tools/skills) ). If gating fails, the skill is invisible — it won’t appear in the agent’s available skills and won’t burn token overhead. Better than loading and failing at runtime.


**Credentials go in` openclaw.json` , not the skill.** Environment variables get injected at runtime:


```text
{
"skills"  : {
"entries"  : {
"usage-report"  : {
"enabled"  :   true  ,
"env"  : {
"MONITOR_API_KEY"  :   "your-key-here"  ,
"MONITOR_ENDPOINT"  :   "https://api.example.com/usage"
}
}
}
}
}
```


Keys are scoped to the agent’s runtime and cleaned up afterward. They don’t persist in your shell environment or show up in logs.


## What Breaks (and Why)


Every tutorial shows you the happy path. Here’s what goes wrong.


### Description Overlap


You write ten skills. Three of them mention “reports” in their descriptions. The agent starts randomly picking the wrong reporting skill depending on how the user phrases their request.


This is the most common issue once your skill library grows past 10-15 skills. The fix: make descriptions specific and mutually exclusive. Test by asking your agent ambiguous questions and seeing which skill it selects. If it picks the wrong one, tighten the descriptions until they don’t overlap.


` disable-model-invocation: true` in the frontmatter is another option for skills that should only run when explicitly called via slash command. This removes the skill from the model’s automatic selection entirely and saves the token overhead of its description being in context ([OpenClaw docs](https://docs.openclaw.ai/tools/skills) ).


### Metadata Must Be Single-Line JSON


This is a subtle one. The metadata field in YAML frontmatter must be a single-line JSON object:


```text
# Correct
metadata  : {  "openclaw"  :{  "requires"  :{  "bins"  :[  "curl"  ]}}}


# Wrong - multiline will silently fail
metadata  :
openclaw  :
requires  :
bins  :
-   curl
```


The multiline version won’t necessarily break — OpenClaw stringifies it internally — but the parsing path goes through` JSON5.parse()` , and nested YAML that doesn’t round-trip cleanly through JSON can produce silent gating failures. Stick with single-line JSON. Everyone who’s used YAML elsewhere expects nested syntax to work, but this is one place where it bites you ([LumaDock](https://lumadock.com/tutorials/build-custom-openclaw-skills) ).


### Permissions vs. Skills


Skills don’t grant tool permissions. If your OpenClaw tool policy blocks` exec` , a skill that relies on shell commands will load - it’ll show up in the skill list, the agent will try to use it - but every command execution will fail at runtime.


This is confusing because the skill looks installed and the agent confidently announces it’s running your workflow. Then nothing happens. Check your tool permissions before debugging the skill itself.


### The 80% Trap


A skill works in every test you throw at it. You deploy it. Within a week it’s broken.


The problem is almost always edge cases in real data that synthetic test data doesn’t cover. Dates in unexpected formats. Empty responses from an API that has always returned data before. A user asking for “last month’s report” on the first of the month when “last month” is ambiguous.


Test with your real inputs. Real meeting notes, real email threads, real API responses. The messy reality of actual data is where skills break. If you’re building a skill for your own workflow, you’re the best test case. Live with it for a week before sharing it.


## Testing and Sharing Your Skill


### Testing Locally


OpenClaw snapshots skills at session start but watches for new SKILL.md files with a ~250ms debounce ([OpenClaw docs](https://docs.openclaw.ai/tools/skills) ). In practice, if you save a change to your SKILL.md, it should appear on the next conversation turn. If it doesn’t, restart the session.


Debug commands:


```text
openclaw   skills   list                # See all loaded skills
openclaw   skills   list   --eligible     # Only skills that passed gating
openclaw   skills   check               # Verify skill health
openclaw   skills   info   my-skill       # Details on a specific skill
```


The most useful is` --eligible` . If your skill is installed but not eligible, it’s almost always a missing binary or unset environment variable.


If you’re using[scheduled tasks or heartbeat automation](https://klausai.com/blog/openclaw-automation-heartbeat-scheduled-tasks-what-runs-without-you) , test your skill in that context too. A skill that works interactively can behave differently when triggered on a schedule.


### Sharing With Your Team


For internal use, the simplest distribution is Git. Clone the skill into` ~/.openclaw/skills/` on each team member’s machine. For project-scoped skills, put them in` <project>/skills/` and they’re included in version control automatically.


### Publishing to ClawHub


If your skill solves a general problem, publish it:


```text
npm   i   -g   clawhub
clawhub   login
clawhub   skill   publish   ~/.openclaw/skills/my-skill   \
--slug   my-skill   \
--name   "My Skill"   \
--version   1.0.0   \
--tags   latest
```


Treat publishing like open source. No secrets in the SKILL.md. Clear requirements in the metadata. Explicit guardrails. A skill without guardrails is a skill that will surprise someone.


[ClawHub](https://clawhub.ai/) scans every published skill through[VirusTotal](https://openclaw.ai/blog/virustotal-partnership) , but code scanning won’t catch a skill that uses natural-language instructions to make the agent do something harmful. Review what you’re publishing.


For version updates:


```text
clawhub   skill   publish   ~/.openclaw/skills/my-skill   \
--slug   my-skill   \
--version   1.1.0   \
--changelog   "Added error handling for empty API responses"
```


## Frequently Asked Questions


### How long does it take to write a skill?


A simple reporting skill: 15-30 minutes. An API-connected skill with a bundled script: 1-2 hours. The initial write is fast. The iteration cycle - test with real data, observe failures, tighten the instructions, retest - is where the real time goes. Budget a week of daily use before considering a skill “done.”


### Can I use the built-in skill-creator to bootstrap?


Yes. OpenClaw ships with a[skill-creator](https://github.com/openclaw/openclaw/blob/main/skills/skill-creator/SKILL.md) that generates SKILL.md scaffolding from a description. It follows a six-step process (understand, plan, initialize, edit, package, iterate) and handles the folder structure and naming conventions. Treat the output as a starting point. The generated skill will be structurally correct but won’t have the specificity or guardrails that come from running it in production.


### How many skills should I run at once?


Each skill adds ~24 tokens of overhead to every conversation ([OpenClaw docs](https://docs.openclaw.ai/tools/skills) ). At 15 skills that’s around 360 tokens - negligible. At 50 skills you’re burning 1,200 tokens before the agent reads your first message. More importantly, description overlap at that scale means the agent starts picking wrong skills. 10-15 active skills is a practical ceiling for most business use.


### Do skills work with any AI model?


Yes. Skills are model-agnostic. They work with Claude, GPT, DeepSeek, Gemini, and local models through Ollama ([OpenClaw docs](https://docs.openclaw.ai/tools/skills) ). The skill is an instruction file, not model-specific code. The agent reads the markdown and follows the steps regardless of which model is interpreting them.


### What’s the difference between a skill and an MCP server?


Skills are instruction files. They tell the agent how to use existing tools. MCP servers are running processes that provide new tools. If your automation can be expressed as a sequence of existing commands and API calls, a skill is simpler. If you need a persistent connection to a data source or a custom protocol, an MCP server is the better fit.


## Key Takeaways


- A skill is a markdown file in a folder. The barrier to building one is zero. The barrier to building a good one is running it in production and fixing what breaks.
- Write the SKILL.md like a runbook for someone who will follow the instructions literally. Numbered steps, exact commands, explicit stop conditions.
- Start read-only. Every skill begins as a reporter. Add actions after you trust the logic.
- The guardrails section is where skills succeed or fail. Without explicit rules, the agent makes its own judgment calls. Sometimes those calls involve restarting production services at 2 AM.
- Test with real data, not synthetic inputs. Live with your skill for a week. The failures that matter only show up in actual use.
- If you build something useful,[publish it to ClawHub](https://clawhub.ai/) . The ecosystem has over 53,000 skills, and it’s only as good as the ones people contribute.


---


On Klaus, we pre-configure skills and write custom ones for customers who’d rather skip the trial-and-error. If that sounds useful,[sign up at klausai.com](https://klausai.com/) .


## Sources


- OpenClaw. “Skills.” Official Documentation.[https://docs.openclaw.ai/tools/skills](https://docs.openclaw.ai/tools/skills)
- ClawHub. Official OpenClaw Skill Registry.[https://clawhub.ai/](https://clawhub.ai/)
- OpenClaw. “Skill Creator.” GitHub.[https://github.com/openclaw/openclaw/blob/main/skills/skill-creator/SKILL.md](https://github.com/openclaw/openclaw/blob/main/skills/skill-creator/SKILL.md)
- DataCamp. “Building Custom OpenClaw Skills: A Hands-On Tutorial.” 2026.[https://www.datacamp.com/tutorial/building-open-claw-skills](https://www.datacamp.com/tutorial/building-open-claw-skills)
- LumaDock. “How to Build Custom OpenClaw Skills for Your Own Automations.” 2026.[https://lumadock.com/tutorials/build-custom-openclaw-skills](https://lumadock.com/tutorials/build-custom-openclaw-skills)
- BoilerplateHub. “OpenClaw Skills: How to Find, Install, and Build Your Own.” 2026.[https://boilerplatehub.com/blog/openclaw-skills](https://boilerplatehub.com/blog/openclaw-skills)
- VoltAgent. “Awesome OpenClaw Skills.” GitHub.[https://github.com/VoltAgent/awesome-openclaw-skills](https://github.com/VoltAgent/awesome-openclaw-skills)
- OpenClaw. “OpenClaw Partners with VirusTotal for Skill Security.” February 2026.[https://openclaw.ai/blog/virustotal-partnership](https://openclaw.ai/blog/virustotal-partnership)
