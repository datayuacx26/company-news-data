---
schema_version: "1.0.0"
document_id: "448fe8c0f8ca836a6c697a20905babde264b2c1e3ad57be66674ec12b6a017be"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/launching-nao-skills/"
published_at: "2026-04-29T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:de3778a6e01e1210603a1222d51d29293fb4dc6b26377f3ef50fa397f0d5738e"
---

# Launching nao Context Engineering skills

[Blog](https://getnao.io/blog/) /


product updates


# Launching nao Context Engineering skills


Five open-source skills that set up your nao project, write its rules, build its test suite, audit it, and add a semantic layer. Install with \`nao skills add getnao/nao\`.


29 April 2026


By Claire Gouze


Founder @ nao


Time to ship your analytics agents with... agents.


We just launched **nao skills** : a set of open-source skills to set up your nao project with the right context, rules, and evaluation. All from your favorite agent: Claude Code, Codex, Cursor.


Context engineering is what makes an analytics agent actually reliable, but it's still a new discipline. I packaged everything we learned into 5 skills:


- 🚀 Create your first nao project and context
- 🪄 Audit your context quality and suggest improvements
- 🗒️ Write structured rules for your agent
- 🧪 Write tests to evaluate the agent
- 🕸️ Add a semantic layer


All open source on[github.com/getnao/nao](https://github.com/getnao/nao) .


### The fundamentals these skills are built on


If you want the research behind every choice in these skills, start here:


- [How to do context engineering for analytics agents](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) : the foundational how-to.
- [What context has the most impact on analytics agent performance?](https://getnao.io/blog/context-impact-analytics-agent) : what to prioritize when you write` RULES.md` .
- [4 steps to improve your analytics agent reliability from 45% to 86%](https://getnao.io/blog/improve-analytics-agent-reliability-steps) : the test-driven iteration loop the skills automate.
- [What is the impact of a semantic layer on analytics agent performance?](https://getnao.io/blog/semantic-layer-impact-analytics-agent) : why` add-semantic-layer` is reactive, not proactive.


## Watch demo


Or[watch on YouTube](https://www.youtube.com/watch?v=2U5MJyQlL7k) .


## Install


Full usage docs:[docs.getnao.io/nao-agent/context-engineering/skills](https://docs.getnao.io/nao-agent/context-engineering/skills) . The published source of truth lives at[github.com/getnao/nao/tree/main/skills](https://github.com/getnao/nao/tree/main/skills) . Install all five into the current project's` .claude/skills/` with:


bash


nao skills


add


getnao/nao


` nao skills` is a thin wrapper around the[open-source skills CLI from Vercel Labs](https://github.com/vercel-labs/skills) , so the equivalent direct call also works:


bash


npx skills


add


getnao/nao


Run from the root of the project where` nao_config.yaml` lives. Re-run any time to pick up updates; pass through` --force` to overwrite local edits.


## The 5 skills


All five live under[skills/ in the nao repo](https://github.com/getnao/nao/tree/main/skills) : read them, fork them, send PRs.


### ` setup-context`


**When to use:** First-time setup. Takes you from` pip install nao-core` to a synced project with a starter` RULES.md` and the LLM key wired up.


**Steps:**


1. Ask once for: warehouse + auth, scope strategy, extra repos (dbt / ETL / BI), LLM provider.
2. Look up the warehouse-specific config from[the nao docs](https://docs.getnao.io/nao-agent/context-builder/databases) .
3. Write` nao_config.yaml` , run[nao init](https://docs.getnao.io/nao-agent/context-engineering/playbook) , print a summary, get user confirmation.
4. Run` nao sync` .
5. Hand off to` write-context-rules` for the first` RULES.md` .
6. Wire up the LLM key (env-var ref preferred; never paste in chat).


**Principles:**


- ≤100 tables in scope; 20 is the target. ([why](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) )
- One batch of questions, no ping-pong.
- SSH git URLs only for repos.
- Run` nao init` non-interactively with the yaml pre-written.


**Templates:** none. Hands off to` write-context-rules` , which owns` RULES.md` .


### ` write-context-rules`


**When to use:** Generating the initial` RULES.md` from synced files, or improving an existing one. This skill owns the` RULES.md` template.


` RULES.md` is loaded with **every** message to your nao agent: keep it lean. It's an orchestrator (point the agent to the right context fast) plus broad answer rules. Per-table detail and full metric semantics belong in referenced files:` databases/<table>.md` ,` semantics/<metric>.yaml` . See[the RULES.md docs](https://docs.getnao.io/nao-agent/context-builder/rules-context) . The six sections below come straight from[What context has the most impact on analytics agent performance?](https://getnao.io/blog/context-impact-analytics-agent) and[How to do context engineering for analytics agents](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) .


**Steps** , generated section by section so you see progress:


1. ` ## Business overview`
2. ` ## Data architecture`
3. ` ## Core data models` (Most Used Tables + Tables detail)
4. ` ## Key Metrics Reference` (grouped by category)
5. ` ## Date filtering` (3 example formulas)
6. ` ## Analysis Process`


Then validate metrics with the user, and write date-filtering rules together.


**Principles:**


- Section by section, not all-at-once.
- Don't bloat` RULES.md` . Per-table detail in` databases/` , not inline.
- Three date-filtering examples max: last X weeks, last X days, current month. The agent extrapolates.
- Two questions decide most date logic: does a week start Sunday or Monday, and does "last 8 weeks" include the current incomplete week.


**Templates:**[templates/RULES.md](https://github.com/getnao/nao/blob/main/skills/write-context-rules/templates/RULES.md) , the six-section scaffold this skill writes.


### ` create-context-tests`


**When to use:** Establishing or extending the reliability benchmark. Tests are the only honest answer to "is the context working?"


[nao test](https://docs.getnao.io/nao-agent/context-engineering/evaluation) runs each natural-language prompt through the agent, executes both the agent's SQL and the test's expected SQL against the warehouse, and **diffs the result data row by row** . A test passes only if the actual data matches. The iteration loop this skill encodes is the same one that took us[from 45% to 86% reliability](https://getnao.io/blog/improve-analytics-agent-reliability-steps) .


**Steps:**


1. Ask once: does the user have trusted source-of-truth queries (Looker, dashboards, prior benchmarks)? Transform those into tests; draft new ones for metrics without coverage.
2. Save flat under` tests/` .
3. User validates each test.
4. Run` nao test -m <model> -t 10` (requires` nao chat &` in the background; first run prompts for login).
5. Recap pass rate, token cost, time.
6. Diagnose failures from` tests/outputs/` .


**Principles:**


- One test per key metric is the floor; coverage tests come after.
- Rule 1: prompts read like real chat. No leaked table names.
- Rule 2: output column names encode format / unit, not source.` churn_rate_float_0_1` , not` churn_rate_from_fct_subscriptions` .


**Templates:**` templates/test.yaml` , the single-test format.


### ` audit-context`


**When to use:** Any stage. Right after` setup-context` , mid-build, before a release, or whenever the agent gets surprising.


**Steps:** six checks, in order:


1. **Synced context** : what's in` nao_config.yaml` , what's missing, has sync run.
2. **` RULES.md` vs target structure** : present / missing / thin per section.
3. **Per-table coverage** : every table in` databases/` documented somewhere.
4. **MECE** : mutually exclusive, no duplicated metrics or columns.
5. **Test coverage** : failure root-cause analysis from` tests/outputs/` .
6. **Token optimization** : file sizes,` RULES.md` bloat, unused tables.


Output is a short in-conversation report ending with a prioritized plan, easiest-win to biggest-work. No files written.


**Principles:**


- Diagnose only, never fix. Routes fixes to` write-context-rules` ,` add-semantic-layer` , or` create-context-tests` .
- Apply one change at a time; re-run tests between fixes.
- Tests are the source of truth for "is the context working".


**Templates:** none.


### ` add-semantic-layer`


**When to use:** **After** a first round of[nao test](https://docs.getnao.io/nao-agent/context-engineering/evaluation) shows the agent struggling with metric reliability. Not before.


A semantic layer increases reliability and stability (one definition per metric) but reduces the scope of answerable questions. We measured the trade-off in[What is the impact of a semantic layer on analytics agent performance?](https://getnao.io/blog/semantic-layer-impact-analytics-agent) . If your test failures are concentrated on schema gaps or date logic, fix` RULES.md` first.


**Steps:**


1. Pick the tool: dbt MetricFlow (dbt Cloud with Semantic Layer) / Snowflake views / nao YAML semantic files / Other.
2. Install the matching MCP (full JSON configs in the skill body).
3. Hand off to` write-context-rules` to route metrics through the layer.
4. Re-run` nao test` and compare to the pre-semantic-layer baseline.


**Principles:**


- Only after tests show metric failures. Cite them.
- One semantic layer at a time. Two competing layers create unpredictable answers.
- Credentials via` ${ENV_VAR}` refs in` .claude/mcp.json` , never literals.


**Templates:**` templates/semantic.yaml` , one file holding all dimensions and metrics for the in-house option.


## Related articles


[insights The Agentic Analytics Playbook is out Learn how to choose your harness, build your context layer, plan your rollout, measure success, and get examples from 7 real-life companies.](https://getnao.io/blog/agentic-analytics-playbook/)[product updates We're launching the first Open Source Analytics Agent Builder We're open sourcing nao — an analytics agent framework built on context engineering. Here's our vision for what comes after black-box BI.](https://getnao.io/blog/open-source-analytics-agent-launch/)[product updates nao has a new look We rebuilt the nao interface from the ground up. New home screen, a prompt queue, visible agent reasoning, redesigned charts and stories.](https://getnao.io/blog/nao-redesign/)


Claire


For nao team
