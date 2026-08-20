---
schema_version: "1.0.0"
document_id: "69fce7d742956610810b3143f81423b2f4e7f46413ff491f47c2e7c74d5a3a34"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/giving-pi-agent-project-memory-with-mem0-a-practical-schema-migration-demo"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-23T17:29:03.310285+00:00"
fetched_at: "2026-07-28T21:23:16.325685+00:00"
content_hash: "sha256:e726d34cd118cbf158109ce53060210bec7d85977763268c02e12fe52c0f61dd"
---

# Giving Pi Agent Project Memory with Mem0: A Practical Schema Migration Demo

## **Quick Takeaways**


-


[Mem0](https://mem0.ai/) is a memory layer for AI agents.[Pi](https://pi.dev/) is a coding agent. The` @mem0/pi-agent-plugin` connects them, giving Pi memory that survives across sessions, projects, and devices.


-


Fresh coding-agent sessions fail in expensive ways: they do not know your repo's style, so they generate plausible code that violates local conventions.


-


This demo wires the Pi coding agent to Mem0 via the latest` @mem0/pi-agent-plugin` , running Pi as separate subprocesses, so there is no hidden chat-session continuity.


-


The point is not that the agent remembered a sentence. The point is that it wrote code closer to the repo's rules.


[Mem0](https://mem0.ai/) is a memory layer for AI agents and LLM applications. A raw language model is stateless, i.e., it forgets everything the moment a session ends, so every new conversation starts from zero. Instead of replaying whole transcripts back into context, Mem0 extracts discrete memories, stores them with scope (per user, per session, per project), and returns only what is relevant to the current task. This keeps the retrieval small as history grows.


[Pi](https://pi.dev/) is a coding agent that runs in your terminal. You give it a task, it reads your repo, writes and edits code, runs commands, and iterates. It works interactively in a TUI and non-interactively through print mode (` pi -p` ) and a structured JSON event stream (` pi --mode json` ). Pi can read the files in front of it, but it does not carry forward the decisions you made yesterday. The` @mem0/pi-agent-plugin` is the package that closes that gap.


Mem0 recently launched a plugin for Pi Code:


The integration ships as a Pi package,` @mem0/pi-agent-plugin` , installable with a single command. It gives Pi automatic memory capture, semantic search, project-scoped memory keyed to your git repo, and slash commands for managing it all from the terminal.


Please enter a valid YouTube, Vimeo, or direct video URL


If you would rather read than watch, the rest of this post walks through exactly what the video shows, why each piece is built the way it is, and how to run it yourself.


## **How Mem0 and Pi Integrate**


Most memory demos stop at preference recall: tell the agent you use pnpm, watch it repeat that back. That proves storage works, but developers do not open an agent to ask what package manager they use. They ask it to change the code. The real test is whether memory survives into a fresh session and shapes the code that comes out.


> 💡 You'll need a free Mem0 API key to follow along.[Get one at app.mem0.ai](https://app.mem0.ai/) - free tier, no credit card.


Before the demo, here is the integration in plain terms. You install the plugin into Pi:


```text
pi   install   npm :  @ mem0  /pi-agent-plugin
```


```text
pi   install   npm :  @ mem0  /pi-agent-plugin
```


```text
pi   install   npm :  @ mem0  /pi-agent-plugin
```


Then you point it at your Mem0 account:


```text
export    MEM0_API_KEY  = "m0-your-key-here"
```


```text
export    MEM0_API_KEY  = "m0-your-key-here"
```


```text
export    MEM0_API_KEY  = "m0-your-key-here"
```


From that point on, the plugin does four things that matter for coding work:


-


**Automatic capture:** It learns from conversations, both your messages and Pi's, without you issuing explicit save commands.


-


**Project scope:** It scopes memories by the git root, using` git rev-parse --show-toplevel` . Every subdirectory in a repo shares one memory pool, and different repos stay isolated.


-


**Semantic retrieval:** Pi can search memories by meaning through an agent-accessible tool, so it pulls relevant context mid-task instead of being handed everything.


-


**Lifecycle commands:** Slash commands like` /mem0-remember` ,` /mem0-search` ,` /mem0-dream` , and` /mem0-status` let you manage memory from the terminal.


## **What You’ll Build**


**The task is intentionally practical:** add a refunds table to an existing Postgres schema with non-obvious migration conventions.


The demo uses Pi Agent along with Mem0 to show cross-session project memory on the above task:


1.


A sandbox repo is created with existing SQL migrations.


2.


Session 1 states the project's schema and migration conventions.


3.


Those conventions are stored in the Mem0 project memory.


4.


Session 2 runs twice as fresh Pi processes: once with Mem0 disabled, once with Mem0 enabled.


5.


Both runs generate a refund migration.


6.


A checker scores each output against the repo's conventions.


**Note** : The important part is that Pi does not continue the same chat. Each Pi invocation runs with` --no-session` . If the second process knows anything from the first, it came from Mem0.


## **The Practical: Schema Conventions**


The sandbox repo is an Acme Checkout service with existing Postgres migrations.


The repo already contains:


```text
migrations  /    0001_  create_orders  . sql      0002_  create_line_items  . sql
```


```text
```


```text
```


The existing schema encodes several conventions that are not safe to assume from a blank prompt:


-


Primary keys are named` <entity>_id` , never bare` id` .


-


Money is stored as integer cents in` BIGINT <thing>_cents` , never DECIMAL, FLOAT, price, or amount.


-


Every table includes` created_at TIMESTAMPTZ NOT NULL DEFAULT now()` .


-


Deletes are soft, via nullable` deleted_at TIMESTAMPTZ` .


-


Foreign keys use` <entity>_id` and` ON DELETE RESTRICT` .


-


Migrations must include a paired` - +migrate Down` section.


Here is the kind of existing table the agent needs to match:


```text
-- +migrate Up
CREATE    TABLE   orders  (
order_id     BIGINT GENERATED ALWAYS  AS    IDENTITY    PRIMARY    KEY  ,
customer_id  BIGINT  NOT    NULL  ,
total_cents  BIGINT  NOT    NULL  ,
currency     TEXT  NOT    NULL    DEFAULT    'usd'  ,
status       TEXT  NOT    NULL    DEFAULT    'pending'  ,
created_at   TIMESTAMPTZ  NOT    NULL    DEFAULT   now (  )  ,
deleted_at   TIMESTAMPTZ
)  ;


-- +migrate Down
DROP    TABLE


```


```text
-- +migrate Up
CREATE    TABLE   orders  (
order_id     BIGINT GENERATED ALWAYS  AS    IDENTITY    PRIMARY    KEY  ,
customer_id  BIGINT  NOT    NULL  ,
total_cents  BIGINT  NOT    NULL  ,
currency     TEXT  NOT    NULL    DEFAULT    'usd'  ,
status       TEXT  NOT    NULL    DEFAULT    'pending'  ,
created_at   TIMESTAMPTZ  NOT    NULL    DEFAULT   now (  )  ,
deleted_at   TIMESTAMPTZ
)  ;


-- +migrate Down
DROP    TABLE


```


```text
-- +migrate Up
CREATE    TABLE   orders  (
order_id     BIGINT GENERATED ALWAYS  AS    IDENTITY    PRIMARY    KEY  ,
customer_id  BIGINT  NOT    NULL  ,
total_cents  BIGINT  NOT    NULL  ,
currency     TEXT  NOT    NULL    DEFAULT    'usd'  ,
status       TEXT  NOT    NULL    DEFAULT    'pending'  ,
created_at   TIMESTAMPTZ  NOT    NULL    DEFAULT   now (  )  ,
deleted_at   TIMESTAMPTZ
)  ;


-- +migrate Down
DROP    TABLE


```


A fresh agent can infer some of this from nearby files, but not reliably. More importantly, production projects often have partial docs, old migrations, conflicting examples, or conventions that were agreed in chat and never written down. This is exactly where persistent memory becomes useful.


The baseline can inspect:


```text
README  . md
package  . json
migrations  / 0001_  create_orders  . sql
migrations  / 0002_  create_line_items  . sql
migrations  / README  . md
```


```text
README  . md
package  . json
migrations  / 0001_  create_orders  . sql
migrations  / 0002_  create_line_items  . sql
migrations  / README  . md
```


```text
README  . md
package  . json
migrations  / 0001_  create_orders  . sql
migrations  / 0002_  create_line_items  . sql
migrations  / README  . md
```


And still, a fresh process may choose sensible defaults that do not match the repo:


```text
CREATE    TABLE   refunds  (
id          SERIAL  PRIMARY    KEY  ,
order_id    INTEGER  REFERENCES   orders (  id )    ON    DELETE    CASCADE  ,
amount      DECIMAL (  10  ,    2  )    NOT    NULL  ,
reason      TEXT ,
created_at  TIMESTAMP  DEFAULT   now (  )
)


```


```text
CREATE    TABLE   refunds  (
id          SERIAL  PRIMARY    KEY  ,
order_id    INTEGER  REFERENCES   orders (  id )    ON    DELETE    CASCADE  ,
amount      DECIMAL (  10  ,    2  )    NOT    NULL  ,
reason      TEXT ,
created_at  TIMESTAMP  DEFAULT   now (  )
)


```


```text
CREATE    TABLE   refunds  (
id          SERIAL  PRIMARY    KEY  ,
order_id    INTEGER  REFERENCES   orders (  id )    ON    DELETE    CASCADE  ,
amount      DECIMAL (  10  ,    2  )    NOT    NULL  ,
reason      TEXT ,
created_at  TIMESTAMP  DEFAULT   now (  )
)


```


This is the kind of output that looks useful at first glance and fails only when someone who knows the repo reviews it.


The point is not that agents cannot read files. The point is that project behavior includes decisions, preferences, and constraints that are not always fully recoverable from the current file tree.


## **How The Demo Is Wired**


The repo has four core files:


```text
app  . py           Streamlit   UI   and   demo   orchestration
pi_bridge  . py     Runs   real   Pi   subprocesses   and   reads  / writes   Mem0
sandbox  . py       Creates   the   git  - backed   schema   sandbox
checker  . py       Scores   generated   SQL   against   convention   rules
```


```text
app  . py           Streamlit   UI   and   demo   orchestration
sandbox  . py       Creates   the   git  - backed   schema   sandbox
checker  . py       Scores   generated   SQL   against   convention   rules
```


```text
app  . py           Streamlit   UI   and   demo   orchestration
sandbox  . py       Creates   the   git  - backed   schema   sandbox
checker  . py       Scores   generated   SQL   against   convention   rules
```


> **💡 Checkout the complete code**[GitHub repository](https://github.com/aashidutt-mem0/Pi-x-Mem0-demo)


### **1. Sandbox**


The Mem0 plugin scopes project memories by git root. So the demo does not fake a project ID and creates a git-backed sandbox like this:


```text
SANDBOX_ROOT   =  Path  (  os  . environ  . get  (  "PI_DEMO_SANDBOX"  ,    "/tmp/pi-mem0-sandbox"  )  )
```


```text
```


```text
```


Then, it derives the app ID from that same git root, so the UI and Pi agree on where memory lives. If the UI writes memories to one app ID and Pi reads from another, the demo lies. Matching the plugin's scoping model keeps the memory path honest.


### **2. Session 1: Without Mem0**


The Streamlit app pre-fills the conventions as the developer would state them:


```text
Schema and migration conventions for this service, follow them exactly:


1. Primary keys are named  <  entity  >


```


```text
Schema and migration conventions for this service, follow them exactly:


1. Primary keys are named  <  entity  >


```


```text
Schema and migration conventions for this service, follow them exactly:


1. Primary keys are named  <  entity  >


```


Session 1 sends those conventions to Pi and asks it to remember them. For demo determinism, the app also mirrors the convention text into Mem0 directly:


```text
bridge  . add_memory  (  decision  ,    app_id  )
```


```text
bridge  . add_memory  (  decision  ,    app_id  )
```


```text
bridge  . add_memory  (  decision  ,    app_id  )
```


The Pi plugin supports auto-capture, but auto-capture can be asynchronous. A live demo should not depend on timing. In normal use, the plugin's capture pipeline handles this after conversations. In the demo, we make the write explicit so the before/after is stable.


### **3. No-Memory Baseline**


The no-memory run does not uninstall the plugin. It launches Pi with memory disabled for that child process:


```text
if   inject_no_memory :
env  . pop  (  "MEM0_API_KEY"  ,    None  )
env  . pop  (  "MEM0_USER_ID"  ,    None  )
```


```text
if   inject_no_memory :
env  . pop  (  "MEM0_API_KEY"  ,    None  )
env  . pop  (  "MEM0_USER_ID"  ,    None  )
```


```text
if   inject_no_memory :
env  . pop  (  "MEM0_API_KEY"  ,    None  )
env  . pop  (  "MEM0_USER_ID"  ,    None  )
```


That produces a clean baseline with same repo, same task, same model, same Pi binary, and no Mem0 key in the subprocess environment. The only difference is memory access.


### **4. Session 2: With-Mem0**


The with-memory run is also a fresh Pi process and not a continuation of Session 1. The prompt asks Pi to recall the project's conventions before writing:


```text
Before   writing   anything  ,    recall   this    project  's schema and migration conventions from memory. Then: Add a refunds table...
```


```text
```


```text
```


Pi uses the` @mem0/pi-agent-plugin` resources to retrieve relevant memories from the project scope, then generates the migration.


## **Convention Checker**


The repo also includes a small checker that scores the generated migration against six rules. The rules are simple on purpose:


```text
CONVENTION_RULES   =  [
{
"id"  :  "pk_naming"  ,
"label"  :  "PK named <entity>_id, not bare id"  ,
"must_have"  :  r"refund_id\b"  ,
"violation"  :  r"\bid\s+(?:BIGINT|SERIAL|INT|UUID)\b(?![_a-z])"  ,
}  ,
{
"id"  :  "money_cents"  ,
"label"  :  "Money as BIGINT <thing>_cents, never DECIMAL/float"  ,
"must_have"  :  r"_cents\b\s+BIGINT"  ,
"violation"  :  r"\b(?:DECIMAL|NUMERIC|FLOAT|REAL|MONEY)\b|\b(?:price|amount)\b"  ,
}  ,
# ... four more rules
]
```


```text
CONVENTION_RULES   =  [
{
"id"  :  "pk_naming"  ,
"label"  :  "PK named <entity>_id, not bare id"  ,
"must_have"  :  r"refund_id\b"  ,
"violation"  :  r"\bid\s+(?:BIGINT|SERIAL|INT|UUID)\b(?![_a-z])"  ,
}  ,
{
"id"  :  "money_cents"  ,
"label"  :  "Money as BIGINT <thing>_cents, never DECIMAL/float"  ,
"must_have"  :  r"_cents\b\s+BIGINT"  ,
}  ,
# ... four more rules
]
```


```text
CONVENTION_RULES   =  [
{
"id"  :  "pk_naming"  ,
"label"  :  "PK named <entity>_id, not bare id"  ,
"must_have"  :  r"refund_id\b"  ,
"violation"  :  r"\bid\s+(?:BIGINT|SERIAL|INT|UUID)\b(?![_a-z])"  ,
}  ,
{
"id"  :  "money_cents"  ,
"label"  :  "Money as BIGINT <thing>_cents, never DECIMAL/float"  ,
"must_have"  :  r"_cents\b\s+BIGINT"  ,
}  ,
# ... four more rules
]
```


The checker is not a SQL linter, but it answers one narrow question:


```text
Did   the   generated   migration   follow   the   conventions   Session   1    stored
```


```text
```


```text
```


## **What The Demo Looks Like**


In the working Streamlit demo, the page shows Pi readiness, Mem0 connection status, the installed` @mem0/pi-agent-plugin` the sandbox repo and app ID, current project memory, the existing schema (collapsible), session controls, the two generated migrations side by side, the convention checker, and a collapsible diff.


When the run completes, the key visual is the checker:


```text
No   memory :      fewer   conventions   honored
With   Mem0 :      more   conventions   honored  ,    often   6  / 6
```


```text
No   memory :      fewer   conventions   honored
With   Mem0 :      more   conventions   honored  ,    often   6  / 6
```


```text
No   memory :      fewer   conventions   honored
With   Mem0 :      more   conventions   honored  ,    often   6  / 6
```


***👉The key point here is:***


```text
The   fresh   process   generated   code   that   matched   project  - specific   conventions
```


```text
```


```text
```


However, there are a few caveats here:


-


**The checker uses regexes:** It is intentionally transparent and narrow. It does not prove the migration is production-ready. It proves that the generated SQL followed the specific conventions the demo is testing.


-


**Session 1 is still part of the run:** If the memory panel is empty before the run, the with-Mem0 result can still score well because Session 1 stores memory before Session 2 runs. The lifecycle becomes: empty memory, then state conventions, then store memory, then a fresh process recalls memory.


-


**Auto-capture is mirrored for determinism:** The plugin can capture memory automatically, but the demo also calls` bridge.add_memory()` after Session 1. This avoids waiting on asynchronous capture timing.


-


**Memory can become stale:** Persistent memory is powerful because it survives. This is also why it needs lifecycle tools and commands like` /mem0-forget` ,` /mem0-dream` , and` /mem0-pin` .


## **Running The Demo**


Install dependencies:


```text
bash   setup  . sh
```


```text
bash   setup  . sh
```


```text
bash   setup  . sh
```


Set Mem0:


```text
export    MEM0_API_KEY  = "m0-your-key-here"
```


```text
export    MEM0_API_KEY  = "m0-your-key-here"
```


```text
export    MEM0_API_KEY  = "m0-your-key-here"
```


Don't have a key yet?[Get your free Mem0 API key →](https://app.mem0.ai/)


Start the UI:


```text
streamlit   run   app  . py
```


```text
streamlit   run   app  . py
```


```text
streamlit   run   app  . py
```


Before a clean run, use` Reset project memory` , then click` Run the demo` .


## **Try It On A Real Repo**


The easiest way to adapt this pattern is to pick one task where a fresh agent often gets local conventions wrong, write down the conventions in Session 1, store them with Mem0 project scope, run the same task with memory disabled and enabled, and add a small checker that scores the output.


***Pi x Mem0:*** the agent does not merely remember your project; it uses that memory to produce code that fits your project.


Start with a free Mem0 account:[app.mem0.ai](https://app.mem0.ai/)


Fork the demo from the[GitHub repository.](https://app.notion.com/p/Giving-Pi-Agent-Project-Memory-with-Mem0-A-Practical-Schema-Migration-Demo-37ff22c70c9080e3b651c2bb4831094b?pvs=21)


## **Frequently Asked Questions**


### Q. Is the with-Mem0 run using the same chat session as Session 1?


No. Pi is invoked with` --no-session` , so Pi's own session state is not the carrier. The bridge starts a new process for each run, but anything the second process knows came from Mem0.


### Q. Why does the page show empty memory before the with-Mem0 output succeeds?


Initially, the memory is empty because the memory panel is rendered before Session 1 runs. During the run, Session 1 writes the conventions to Mem0, then Session 2 with Mem0 retrieves them.


### Q. Why not just put these conventions in a README?


You should document the stable ones. But migration gotchas, rollout preferences, team decisions made in chat, and "do not repeat this" lessons usually surface during work and rarely make it into docs. Mem0 captures that layer, so it survives without manual upkeep.


### Q. Is this limited to database migrations?


No. The same pattern applies to API response casing, auth token TTLs, feature flag rollout rules, infra region constraints, testing strategy, and SDK compatibility decisions. The migration demo is useful because the output is easy to check.


## Sources


-


Pi Mem0 package:[https://pi.dev/packages/@mem0/pi-agent-plugin?name=mem0](https://pi.dev/packages/@mem0/pi-agent-plugin?name=mem0)


-


Mem0 paper:[https://arxiv.org/abs/2504.19413](https://arxiv.org/abs/2504.19413)
