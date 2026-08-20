---
schema_version: "1.0.0"
document_id: "f2421cfb68d4b8b6d92fa483d0dba0e16ee38777926c94e7c907c1c0fa25f0c3"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/long-running-agents-need-bash"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-22T23:30:37.706504+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:fd913fb2d4a7f691987dcdc9e7e7b5f2da6ae44760d26d9c2c5b2dec97749c92"
---

# Long-running agents don't need tools or hosted sandboxes; all they need is bash.

At Brex, we are building agents for corporate spend. One agent helps employees complete expenses, and another, which we call audit agent, reviews spend, detects abuse or fraud patterns, and escalates the highest-value actions to finance teams.


The audit agent has to inspect receipts, user history, prior cases, calendar context, web research, and policy text, and then produce financial actions: approve, reject, flag for review, request more information, or send a policy reminder. The goal is review-by-exception: audit more spend and surface only the decisions that need a human.


Some reviews are straightforward with one expense, one receipt, and one policy check. The harder reviews are open-ended. A dinner can look out of policy until the agent reads the trip context. A reimbursement can look harmless until it is joined with a corporate-card charge. A travel pattern can require receipt text, web research, prior cases, and policy math.


We cannot prescribe every investigation path upfront. The agent has to decide what to inspect, join, discard, and carry forward.


Our first implementation gave the model purpose-built tools for the obvious sources like expenses, receipts, users, cases, calendar, web search, and final review submission. We also used structured outputs and reran the job when the generated action did not meet the quality bar.


Simple reviews were fine. Harder investigations exposed the runtime problem: every native tool call returned into the conversation. Receipt pages, expense lists, partial joins, and hypotheses filled the context window, with no cheap place to discard irrelevant rows, keep scratch notes, or reduce a 1000-row result into the 12 rows that mattered.


Native tool loops push expense data, receipt text, prior cases, web results, and partial joins into the model context. A workbench keeps raw artifacts outside the transcript and returns a smaller decision packet.


The agent needed a workbench for the middle of the investigation: a place to keep raw artifacts, reduce them, and bring only the useful context back into the decision. Tools are how the agent reaches the world, and the workbench is where it turns raw access into useful context. That changes the tool-design problem. Expenses, trips, policies, merchants, web research, and cases sit on one axis, while fetching, filtering, grouping, sorting, and searching sit on the other. A specialized tool for every cell becomes x-by-y work. A workbench lets the agent compose the rows and columns itself.


## The coding-agent shape


Coding agents already found this pattern. When the work takes many steps, models use terminals and files well. They search, write scripts, inspect outputs, keep intermediate state outside the conversation, and bring back the result that matters.


We wanted the same shape for a product agent. We considered custom Python and JavaScript sandboxes, plus dedicated scratch tools, but neither felt as natural as the harnesses we were already using every day with coding agents.


When the Vercel team released **just-bash** , it made that shape available inside the product runtime: a TypeScript bash-like interpreter with a virtual filesystem, running in the same Node environment as the server, without starting a Docker container for every run. We wanted to try it immediately.


The first reaction after seeing just-bash: rebuild the audit agent with a bash runtime.


After security approved the library, we moved one Brex audit agent toward that runtime. P90 token usage fell from peaks near 3M tokens per execution to roughly 600k-700k, and P95 execution time dropped by about half.


Production token trace before and after moving one Brex audit agent toward just-bash. The tail flattened after the runtime change.


The production signal was strong, but it was not clean attribution. Prompts, workflow design, and runtime shape changed together. So we built a simplified audit benchmark to isolate the runtime question.


## The benchmark


Each benchmark run is a synthetic week of spend with the same ingredients we care about in production: company-paid expenses, reimbursements, receipt text, user context, prior cases, nearby spend, calendar evidence, web search, and a spend policy of about 1,500 words.


We ran it at three scales:


The benchmark runs the same spend-review task at 10, 100, and 1000 expenses to separate simple tool use from long-running detective work.


Each size ran across Haiku, Sonnet, and Opus, with 20 runs per size/model/runtime cell. Every runtime got the same product actions: **get_expenses, analyze_receipt, get_users, get_cases, analyze_calendar_events, web_search,** and **submit_review** . The full setup is in the repo.


The four runtime shapes were:


- Native tools
- Native tools with conversation compaction
- **just-bash**
- Full Docker container


The product actions stayed fixed. The agent reached them through different interfaces.


## What happened at scale


A spend-review run can finish and still be wrong. It can flag the wrong employee, miss a duplicate reimbursement, or create a case with weak evidence. We measured quality against the generated reference set:


- **Precision** : when the agent created a case, was it pointing at the right expenses?
- **Recall** : did the agent find the expenses it was supposed to find?
- **F1** : one combined score for precision and recall.


P70 means 70% of scored runs were at or below that number. P95 means 95% were at or below it. At 1,000 expenses, the table splits completed runs from valid reviews because those started to diverge for some runtimes.


At 10 expenses, quality looked like this:


At small scale, every runtime works. The runtime problem is easy to miss.


At 100 expenses, quality looked like this:


At medium scale, the result is mixed. All runtimes still complete, and with 20 runs per cell this is too small to claim a significant interface difference. The useful read is that the runtime problem has started to show up, but model and run-to-run variance still dominate.


At 1,000 expenses, quality looked like this:


At 1,000 expenses, recall is the result to watch. **just-bash** and the full Docker container consistently found more expected spend. Native-tool runs were often more selective, but they missed more of the batch.


In financial review, low precision creates cleanup work. Low recall loses the case.


Precision can be improved with follow-up steps that merge duplicates and suppress weak candidates. Recall is harder to recover. If the first pass never finds the duplicate reimbursement or direct policy breach, the later precision step never sees it.


A workbench-shaped runtime helps the first pass inspect more of the batch and keep the evidence it needs. Once the right candidates exist, the system can refine them.


## The cost picture


Runtime cost at 10 expenses:


For small tasks, native tools stay lean. The workbench has overhead before there's enough investigation work to amortize it.


Runtime cost at 100 expenses:


At medium scale, the cost story is mixed. Native tools still use fewer tokens in many cells, while the workbench variants start buying lower wall-clock time on some models.


At 1,000 expenses, cost became a reliability and tail problem:


The cost result is not "bash always uses fewer tokens." At 10 and 100 expenses, native tools are often cheaper because there is not much intermediate work to amortize.


At 1,000, the native-tool shape becomes less stable. Haiku and Sonnet raw tools still had context-overflow failures. Compaction reduced some token tails, but it still did not close the quality gap. In a production harness, that gap usually comes back as retries.


A single completed tool run can look cheaper than a workbench run. The end-to-end system can still cost more if the first pass misses the quality bar and has to run again.


The workbench variants paid for local state and memory, but they completed the large batch and found far more expected spend. Docker also finished, but the useful part was the same shell-shaped workspace. For this workload, **just-bash** got that shape inside Node with millisecond startup instead of a container lifecycle.


## Bash is the glue


Bash works because it gives the agent one small language for composing tools, files, filters, scripts, and submission validation.


The bash versions still used the same product capabilities **: get_expenses, analyze_receipt, get_users, get_cases, analyze_calendar_events, web_search,** and **submit_review** . They were exposed as commands inside a workbench.


The product team gets a better design loop. Instead of building a specialized tool for every possible scenario, you expose a small set of domain commands and let the agent compose them. This is the same reason Unix tools still work: small programs, text streams, pipes, and files.


The agent often needs a small view of a broad result:


The pipe prevents the full expense payload from getting dumped into the model context. The same thing shows up with files:


The agent can fetch once, save the result, and run multiple independent queries without paying to re-read the full payload in the conversation.


It also works for policy:


No policy tool call. No giant policy pasted into the system prompt. The agent can inspect the file system the way a person would.


Code becomes a context filter. Files become memory. Commands become the tool interface.


The product still owns the boundaries: the command manifest, help text, JSON contracts, policy, validator, and workflow.


## Read the traces


The work does not stop when the first commands exist. The next loop is trace-driven.


When the agent writes the same script repeatedly, that script probably wants to become a command. When it keeps loading the same guidance, that guidance probably belongs in the prompt or seeded file system. When it tries a flag that does not exist, that failed command is a product interface signal.


Use Docker when the workflow needs arbitrary native binaries, package installation, browser automation, or a real machine boundary. Most product-agent work is orchestration over known systems. For that, the useful primitive is smaller: a bounded workbench where the agent can shape evidence before asking the model to decide.


The stack that covers a large class of long-running product agents is:


- AI SDK for the production agent loop
- **just-bash** for the workbench
- Product CLIs for safe access to company data
- A submission harness that validates the final artifact


## How to try it out


The benchmark repo has the methodology, published results, and rerun instructions:


[https://github.com/herculesggimenes/bash-sandbox-benchmarks](https://github.com/herculesggimenes/bash-sandbox-benchmarks)


When the work becomes long-running, give the agent a place to work. Give it bash.
