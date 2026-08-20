---
schema_version: "1.0.0"
document_id: "f3750601f502685aedb998989d1f58feb522a501256c82c496e23612d3eeeaae"
company_key: "yc-infracost"
company: "Infracost"
source_id: "yc-infracost-news-import-5e947f59e679"
canonical_url: "https://www.infracost.io/resources/blog/shifting-costs-left-of-left-now-announcing-support-for-all-major-ides-and-coding-agents"
published_at: "2026-05-18T21:18:00+00:00"
first_seen_at: "2026-07-23T11:56:51.837513+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:a644c39946bf22330395f956b7daa950811404007c9ff3388b8189008ba88ae6"
---

# Shifting Costs Left of Left: Now Announcing Support for All Major IDEs and Coding Agents

### Build cost-aware infrastructure before you even review it with Infracost Dev 🤯


For years, our[VS Code extension](https://www.infracost.io/resources/blog/jul-2022-update) was the answer to "How do I see cloud cost estimations while I write Terraform?" But now that engineers use agents to write infrastructure as code, it was time to think both broader and bigger with our CLI and local tooling.


Engineers should be able to use coding agents to write infrastructure as code that is optimized for cost, *even before they have look at it.* It’s now possible with[Infracost Dev](https://cost.dev/) — a local agent that runs across any IDE AND your coding agent. It does job as the old VS Code extension (catch cost and policy issues before they hit PR review), but treats coding agents like a first-class citizen.


## Why install Infracost Dev


The IDE integrations and the coding-agent skills are wrappers that call the CLI. That matters because:


-


Like our existing CI/CD offering, **Infracost Dev works across Terraform, AWS CloudFormation, and CDK** . It fetches price points from over a thousand services across AWS, Azure, and GCP.


-


**It runs locally.** Your code never leaves your machine, you get keystroke-fast feedback, and you're not burning API calls per character when you want to fetch prices.


-


**The CLI does the deterministic work** — fetching price points, scanning the code, validating fixes. The coding agent does the natural-language part. You don't have to trust the LLM to remember the rules, and can verify it called the right CLI command.


-


**It provides consistent rule layer across every tool you use.** Get cost estimates in your IDE AND your coding agent with a single install. Infracost Dev supports VS Code, Cursor, Windsurf, JetBrains, Antigravity, VSCodium, Claude Code, GitHub Copilot, OpenAI Codex, Cursor, and Gemini CLI.


With the actual price points in front of you, your can focus less on price calculator lookups and engineering prompts, and more on the appropriate cost tradeoffs for all the infrastructure you ship.


## Why FinOps context matters when you have a coding agent


When you’re dealing with something as tangible and potentially expensive as cloud costs, precision matters. Plain prompting a coding agent with "make this Terraform cost-optimized" is expensive and lossy. You burn tokens loading policy context into every conversation. Your agent could make up a price or even a service, and you wouldn’t know because it’s difficult to verify.


You also stop chasing hallucinations. The CLI runs static analysis on the real code, pulls real-time price points from cloud vendors, and passes that context to the coding agent. So the agent narrates; the CLI is the truth. That combination — agents plus deterministic logic — ensures results are accurate.


##
Infracost Cloud + Dev: FinOps Policies and Context, Delivered to your Coding Agent 🤯


Nobody likes stopping what they’re doing to change instance types or rewrite tags for infrastructure they deployed four months ago. Every Dev account comes with a 14-day trial of[Infracost Cloud](https://www.infracost.io/product) . Set up a basic tagging policy in Infracost Cloud, and your coding agent can then use it across your configurations as long as the trial is running. Infracost Cloud also delivers FinOps best practices, custom price books, cost guardrails, and more.


When paired with Infracost Cloud, the skills in Dev turn your coding agent into a FinOps agent by default. You don't prompt it to optimize — the agent checks every IaC change against your live policies as it writes. You can also use the` scan` command to scan existing resources for cost and FinOps violations too.


##
Try it free


Dev is included in our free tier. The CLI ships a setup wizard that detects your coding tool and IDE, handles auth, and installs the right skill files for whatever coding agent you use. One command, then go back to writing infrastructure as code.


**→**[Install Infracost Dev in minutes](https://www.infracost.io/docs/) ****
