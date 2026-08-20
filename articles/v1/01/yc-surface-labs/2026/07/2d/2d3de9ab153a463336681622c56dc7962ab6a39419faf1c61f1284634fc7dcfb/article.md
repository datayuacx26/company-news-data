---
schema_version: "1.0.0"
document_id: "2d3de9ab153a463336681622c56dc7962ab6a39419faf1c61f1284634fc7dcfb"
company_key: "yc-surface-labs"
company: "Surface Labs"
source_id: "yc-surface-labs-news-import-ffea4c1e1d4e"
canonical_url: "https://withsurface.com/blog/build-a-read-only-marketing-ai-agent-first"
published_at: "2026-07-23T22:56:03.588+00:00"
first_seen_at: "2026-07-24T03:22:19.340672+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:a08266a22a4b8681b92e5d881973eb070ab6c0bd34cd0adbcd503375ce111f00"
---

# Build a Read-Only Marketing AI Agent First

## July 22nd Marketing Best Practices


An easy marketing-agent demo ends with an action. The agent notices a weak campaign, moves budget, updates the CRM, publishes a page, and sends a tidy report. A useful first production workflow retrieves the evidence, explains the anomaly, prepares a recommendation, and waits for a person who understands the account.


That restraint can look timid during a sales presentation. In an operating environment, it gives the team time to learn where the data bends. A wrong summary costs attention. A wrong write can change spend, routing, attribution, customer communication, or the public website before anyone notices.


Google's current[Ads MCP server](https://developers.google.com/google-ads/api/docs/developer-toolkit/mcp-server) offers a helpful precedent. It supports account discovery and performance queries while remaining read-only. HubSpot's generally available server shows the next tier because it can write to parts of the CRM. The difference between those releases is a sensible maturity model for a marketing team.


## Begin with one recurring decision


Choose a question that appears often enough to justify automation and has an answer a specialist can evaluate. A paid media team might ask which campaigns changed materially after controlling for spend and weekday mix. A content team might ask which declining pages also lost query coverage after a release. RevOps might look for leads that meet the routing definition but lack an owner.


Avoid a use case that starts with "optimize." Optimization hides several decisions inside one word. Define the input systems, comparison period, business rule, expected output, and reviewer. Teams improving[inbound lead management](https://withsurface.com/blog/inbound-lead-management) could start with a read-only exception report: list qualified inbound records that have no owner after fifteen minutes, show the timestamps, and group the likely causes. A person can then fix the routing or records through the normal system.


The question should have a reversible learning loop. If the agent misses a record, a reviewer can diagnose why. If the business definition changes, the team can update the test. If the workflow fails for a week, the existing process still works.


## Write the expected answer before the prompt


Most pilots begin with prompt tuning because the prompt is visible. Start with an answer contract instead. Specify the source systems, authorized accounts, required fields, exclusions, date logic, definitions, and output format. Require a link or stable identifier for every material finding.


An answer contract for campaign pacing might state that the agent can read two named ad accounts and the CRM; use the finance-approved revenue field; exclude tests, house campaigns, and the current partial day; compare against both plan and the previous four matching weekdays; and return no more than ten exceptions. Each exception must include the campaign ID, evidence window, observed change, likely explanations, confidence label, and suggested reviewer.


This contract creates an evaluation target. It also exposes the human work that automation cannot skip. Someone must decide which revenue field is authoritative and whether a campaign belongs in the test set. The discipline resembles[lead journey tracking](https://withsurface.com/blog/why-lead-ops-is-becoming-its-own-category) , where useful analysis depends on preserving identity and time across systems rather than producing another aggregate chart.


## Test history before live operations


Build a small evaluation set from ten to twenty historical cases. Include obvious wins, ordinary weeks, known tracking failures, partial data, naming changes, and one case where a specialist's initial conclusion was wrong. Run the workflow without telling the agent the answer.


Score five things. Did it retrieve the correct records? Did it apply the business definition? Did it distinguish fact from explanation? Did it expose missing data? Would the reviewer make a better or faster decision with the output? Record failures by type rather than collapsing them into one accuracy percentage.


The[Conversios case study](https://about.ads.microsoft.com/en/resources/discover/case-studies/conversios-customer-success-story) offers a useful pattern: the company says it used an MCP-based audit dashboard to review Performance Max signals across 121 accounts. The published source comes from Microsoft and Conversios, so its speed and staffing claims need independent validation. The workflow shape is still sound. Apply consistent checks across a portfolio, surface exceptions, and send the result to specialists.


## A 30-day read-only pilot


**Days 1 to 5: define and constrain.** Select one decision, one owner, no more than three sources, and a weekly review. Document credentials, scopes, retention, business definitions, and the manual fallback. Security or IT should approve the connection even when the workflow cannot write because read access can still expose customer, budget, or performance data.


**Days 6 to 12: build the evidence path.** Connect a sandbox or limited account where possible. Make every output include source records, query windows, definitions, and explicit missing-data notes. If the agent cannot cite the relevant evidence, the workflow is not ready for evaluation.


**Days 13 to 20: replay history.** Run the historical cases and log retrieval errors, reasoning errors, stale data, permission failures, and specialist disagreements. Revise the data contract before revising the prose. A smoother explanation will not repair a wrong join.


**Days 21 to 27: shadow production.** Let the agent produce the report alongside the existing process. Reviewers should rate usefulness before seeing the old answer when practical. Measure time to reviewed decision, evidence completeness, accepted recommendations, and exceptions that required manual investigation.


**Days 28 to 30: decide the next permission.** Continue, narrow, stop, or allow a draft action. A draft action might prepare a CRM correction, budget change, or content brief without applying it. Direct writes should wait until the team has a rollback path, monitoring, and a named incident owner.


## Human review needs a job description


"Human in the loop" often means that someone receives a notification. Define what the reviewer actually checks. The channel owner validates the marketing interpretation. The data owner checks definitions and joins. Marketing operations monitors repeatability. Security owns credentials and revocation. The accountable business leader decides which error rate and blast radius are acceptable.


OWASP's[MCP security guidance](https://cheatsheetseries.owasp.org/cheatsheets/MCP_Security_Cheat_Sheet.html) emphasizes least privilege, explicit authorization, validation, and logging. Those are engineering controls with direct marketing consequences. An agency role should not inherit every client account. A content analyst should not be able to alter lifecycle stages. A service credential should not survive the person or vendor that justified it.


The[connections among CRM, scheduling, and analytics tools](https://withsurface.com/integrations) can improve the handoff from research to action. The operating rule is simple: grant the smallest permission that can answer the current question, then require evidence before expanding it.


## Measure decisions, not agent activity


Do not lead the pilot review with reports generated, prompts submitted, or theoretical hours saved. Track the share of outputs with complete evidence, the rate specialists accept recommendations, the time from question to reviewed decision, the number and severity of exceptions, and the business result when a recommendation is implemented.


Keep causal language disciplined. A faster workflow may coincide with better performance without causing it. The same distinction appears when teams learn that[attribution is not incrementality](https://withsurface.com/blog/attribution-is-not-incrementality-how-to-measure-ai-creative) . The agent can improve decision preparation while the team still needs experiments or other causal methods to prove business lift.


A successful read-only pilot earns the right to become slightly more useful. It might create a draft, open a ticket, or route an approval. That progression feels slower than autonomous execution. It is much faster than repairing a system the team never learned to inspect.


#### Practical steps


- Pick one recurring, reviewable decision and document the manual fallback.
- Create an answer contract before writing prompts or connecting production data.
- Test ten to twenty historical cases, including tracking failures and ambiguous outcomes.
- Require source records, business definitions, and missing-data labels in every material output.
- Advance from read to draft, approval, and write only when each prior tier has passed a documented review.
