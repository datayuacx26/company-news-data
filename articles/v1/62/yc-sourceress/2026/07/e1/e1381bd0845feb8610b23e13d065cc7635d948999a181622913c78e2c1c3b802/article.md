---
schema_version: "1.0.0"
document_id: "e1381bd0845feb8610b23e13d065cc7635d948999a181622913c78e2c1c3b802"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/unused-claude-tokens-for-good"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:7e66162a4ec228fc856ff88540d9e569aaf618692c87fa35c231293a48eb2808"
---

# Use your unused Claude tokens for good

**Even though you pay every month for your Claude plan, your quota is actually expires sooner than that.** Quota is based 1) a 5-hour window that opens on your first prompt and resets five hours later, and 2) a weekly window.


**The daily and weekly budget *doesn't roll over!* You've already paid for it, but that capacity simply disappears** when the current session's 5 hour window is up.


This translates to:
- 40% of a typical day's 5-hour Claude quota expires.


- 12,000 lines of working code that idle capacity could write every week.


Imbue engineer Ev used their quota to find and fix bad tests in the mngr codebase. In a day, they were able to produce 9 merged PRs that[added integration tests](https://github.com/imbue-ai/mngr/pull/1980/changes/85dc9e9ed3debd89180c1cf8beec51042296db1a) , fixed[silently failing tests](https://github.com/imbue-ai/mngr/pull/1895) , and[cleaned up code](https://github.com/imbue-ai/mngr/pull/1973) .


## How to use it


Ev's workflow and all of their code is checked into our[open source mngr repo](https://github.com/imbue-ai/mngr) .


###
1. Install mngr and the usage plugins


The default[install of mngr](https://imbue.com/product/mngr) doesn't include the usage plugin that monitors token usage. Add the plugin:


```text
sh
```


###
2. Fork Ev's "fix bad tests" Claude skill, or write your own skill


[Ev's skill](https://github.com/imbue-ai/mngr/blob/c72d8c39055d1acce27d0a072a836d2853271272/.claude/skills/identify-bad-tests/SKILL.md?plain=1#L4) lives in our open source mngr repo. Then,[add this custom skill to Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude#h_a4222fa77b) .


### 3. Trigger mngr to check quota every 10 minutes, and run skill if there's spare capacity


Schedule[this script](https://github.com/imbue-ai/mngr/blob/c72d8c39055d1acce27d0a072a836d2853271272/libs/mngr_usage/docs/cron_recipes.md?plain=1#L165) every 10 minutes on cron or LaunchAgent on macOS. The script does two things:


1) Use` mngr usage` to check the remaining Claude quota


2) If there's quota, start the agent using` mngr create` .


This script defines "budget to spare" as when the 5h window has <80% used budget **and** weekly usage is below a line that starts ~30% under the plain` used% = how_much_the_cycle_has_elapsed%` pace early in the rolling 7-day cycle and tapers up to meet it by the cycle's end. But you’re free to fork the script and customize it yourself. Such is the power of open source software!


And that’s it! The skill will run magically in the background without any action from you.


##
What mngr does under the hood


mngr appends a JSON line per refresh to a standardized path (` events/<source>/usage/events.jsonl` ) — each line contains a small` cost_snapshot` with a` session_id` , the rate-limit windows, and cost.` mngr usage` is a pure reader that aggregates over those files.


This is cool because it shows the usefulness of mngr’s abstraction over agents! In the future, you could manage your unused tokens across Claude, OpenAI, etc, all from one command line tool.


what\`mngr usage\` is doing conceptually under the hood


##
Donate your idle quota to science


Here's the part we're most excited about! We’re working on out-of-the-box Claude skills in partnership with Dr. Sinnott-Armstrong from Fred Hutch Cancer Center, so mngr users can donate your about-to-expire capacity to genetics research with one command.


**We're looking to sponsor other science and informatics research projects.** If you're a researcher that could use the compute, reach out at jean+doinggood@imbue.com.
