---
schema_version: "1.0.0"
document_id: "aa2c9952b3ac90f7b3270385e2595b3c24d70491d6a0741d4ae523c9b449b24c"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-b1ffd1321f3f"
canonical_url: "https://www.microsoft.com/en-us/microsoft-365/blog/2026/06/16/copilot-cowork-is-now-generally-available/"
published_at: "2026-06-16T15:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.173713+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:1fb59f187738b787c0b5a5e4b0869269791767c20533686012db5054ed8a92ee"
---

# Copilot Cowork is now generally available

Today we’re announcing the general availability of[Copilot Cowork](https://www.microsoft.com/en-us/microsoft-365-copilot/cowork) worldwide.


After three months of preview in Frontier, more than half of the Fortune 500 is using Copilot Cowork, along with companies like Accenture, Avanade, Advance Local, Capital Group, Koch, LTM, Ooredoo Qatar, and Zurich Insurance.


We have been impressed by your creativity and what you have all built with it. One engineering team taught Cowork how to safely edit batch‑job spreadsheets and generate dependency flow charts after every change, automating work that previously required careful manual intervention. One team compared nearly four thousand files across two product versions, work that would have taken weeks. One sales lead pointed Cowork at a stalled pipeline and got back a ranked list of at-risk opportunities with the exact follow-up that had gone cold on each, collapsing a week of manual review into a single morning.


Cowork is the fastest growing feature in the history of our Frontier program, and Cowork has among the highest user satisfaction of any Copilot or agent experience we have shipped. We learned from what we saw, engaged with you along the way, and used everything we heard to improve quality and add new features, including model choice, extensibility through plugins, and new cost management controls. Read more below andwatch the full demo .


## **What makes Copilot Cowork different**


Copilot Cowork executes complex, long-running, multi-tool tasks. You define the work and Cowork runs it end-to-end and returns a completed result, not just a draft or a recommendation. Cowork is designed to be more accurate, more secure, and lower cost than other offerings. Five things make that true:


1. Cloud hosting means files are not stored locally, security is strongly enforced, and your tasks keep running even when your laptop is off.
2. Native Work IQ support grounds every task in the systems your business already runs on, so the work reflects real context.
3. Enterprise-grade security and compliance ensure Cowork operates within your Microsoft 365 trust boundary, with protections that align to your organization’s existing policies and controls.
4. Multi-model design lets you run the models a task needs, so capability scales with the work as more models become available.
5. Lower cost from a runtime that efficiently finds the right information and tools, model choice that matches the right model to each task, and billing that charges you only for what you use.


When comparing the cost per prompt between Copilot Cowork and Claude Cowork with their Microsoft 365 connector, testing showed that Copilot Cowork on average was 30-40% cheaper.1


Our newest model, Cowork 1, will be a secure, fine-tuned model releasing in the coming weeks, post-trained to handle tasks at a substantially lower cost. You are not locked into one model with Copilot—you can use the most efficient model or frontier models.


## **Pricing model**


Copilot Cowork requires the Microsoft 365 Copilot User Subscription License (USL). Users are then billed for Cowork on a usage-based basis, with charges determined by the tasks they run.


The Microsoft 365 Copilot USL includes a complete AI productivity experience: Copilot Chat; Copilot in Word, Excel, PowerPoint, Outlook, and Teams, the Work IQ context engine; a multi-model system offering frontier intelligence; pre-built agents like Researcher and Analyst; and custom agents built with Agent Builder. All of that for a predictable per user per month fee.


Copilot Cowork adds a whole new way of working: an agentic system designed for complex, long-running, multi-tool tasks. You are billed on a usage basis, denominated in Copilot Credits, and the price for each task is calculated from four inputs: model use, context retrieval, tool calls, and runtime.


In my customer meetings over the last few months, the most frequent question I’ve heard is how to budget for Cowork given its variable pricing model. From usage during the Frontier program, we observed three common task patterns: light, medium, and heavy tasks.


Light tasks use a small number of knowledge sources, apply limited reasoning, and produce one or fewer outputs. Medium tasks draw on multiple sources, apply structured reasoning, and generate two or more outputs. Heavy tasks aggregate broadly, apply deep reasoning, and produce many outputs.


We also identified four user personas with distinct usage patterns across these task types.


When you combine users by persona with their mix of light, medium, and heavy tasks—and apply a price per prompt—you get a flexible cost model that can help you create a cost estimate you can refine over time. At its core, the model multiplies the number of users in each segment by their expected prompt volume across light, medium, and heavy tasks, applies the cost per prompt type, and sums the total.


If you want to model this yourself, you can[download a simple spreadsheet here](https://aka.ms/CustomerCoworkEstimator) . These estimates assume Anthropic Opus 4.8.


At general availability, Copilot Cowork runs on Anthropic models, including Opus 4.8 and Sonnet 4.6. In Frontier, customers can use GPT 5.5, with Cowork 1 coming soon.


Cowork 1 will deliver optimal cost, quality, and enterprise-grade use, including removing model bias. It’s designed to handle everyday Copilot tasks at a substantially lower cost, making it a strong option for cost-sensitive workloads.


For more detail on the pricing model, refer to[this Microsoft Learn article](https://aka.ms/CopilotCredits/Learn) .


## **Cost management**


Usage-based billing makes it critical to track the value and ROI you get from AI. Long-running, agentic work can create a lot of value for your business but also requires significant compute resources. Three factors will continue to reduce cost over time: models will get cheaper, Cowork will get better at matching models to tasks, and both context retrieval and tool use will become more efficient.


In the meantime, cost management is one of the most important new capability areas in the general availability of Cowork, and we’re shipping features across three themes: control, visibility, and efficiency.


**Control:** Customers decide when Cowork turns on, who gets access, and how much can be spent.


- **Cowork is off by default.** Admins decide when to enable Cowork in their tenant and who gets access.


- **Spending limits at the tenant, group, and user levels.** Admins create scoped billing policies and define budgets, including user-level caps set inside group policies.


- **Customizable usage alerts.** Admins set the thresholds that matter for their organization and groups, and choose who gets notified when spend crosses them.


- **User-initiated credit requests.** When a user needs additional credits to complete a task, they can request them from inside Cowork.


**Visibility:** Customers see what’s being used, and what each task costs.


- **Usage reporting at the tenant, group, and user levels.** Admins see usage broken down by user, group, and feature, with clear accountability across the organization.


- **User-level pricing for each task, in credits.** Users see what each task costs as they run it.


**Efficiency:** Customers have options to manage cost.


- **Two payment options:** PayGo and P3. Customers choose pay-as-you-go for flexibility, or P3 to commit to a usage volume in advance in exchange for a discount. PayGo is priced at $0.01 per Copilot Credit.


- **Model choice in Frontier.** Where multiple models are available, customers can use the model picker to manage cost-per-task.


Billing for Copilot Cowork begins today. Before usage ramps, admins can set spending limits and allocate budgets using the cost management controls described above. Tenants that had at least one user in the Frontier program (March 30 to June 16) who used Cowork during that period get a grace period on Cowork usage and will not be billed until July 1, 2026, to support the transition.


For additional details on our cost management features, please see[this Microsoft Learn article](https://aka.ms/CopilotCredits/Learn) .


## **What’s new in Copilot Cowork**


The Microsoft 365 Copilot app now includes a toggle that takes you into Cowork’s full experience so you can move from chat to action faster than ever before.


- **Plugins.** **Nine** new partner plugins available now, **eight** plugins are coming soon and the Dynamics 365 suite. **Available now** : Enosix, Harvey, LSEG, Miro, monday.com, Moodys, Morningstar, S&P Global Energy, and TeamsMaestro. Coming soon: Adobe, Atlassian, Box, Canva, CB Insights, Databricks, MoneyForward, and Templafy. Fabric and Dynamics 365 Sales, Customer Service, and ERP apps are also now GA.


- **Browser use via Edge.** In Frontier, Cowork can browse the web through a local Edge browser following the enterprise policies in place for your users today.


- **New security and compliance capabilities.** Cowork prompts, responses, and generated artifacts flow through your existing Microsoft 365 controls and are governed, discoverable, and retained securely. Sensitivity labels are inherited and displayed end-to-end. The protected surface is here at GA: audit log, Data Security Posture Management (DSPM), eDiscovery, Insider Risk Management, Data Lifecycle Management (GA June 22), and Communication Compliance policies. Coming soon: Data Loss Prevention (DLP).


For the full feature list, see the[Copilot Cowork product blog](https://techcommunity.microsoft.com/blog/microsoft365copilotblog/cowork-in-progress/4511672) .


## **Get started with Copilot Cowork**


Copilot Cowork is generally available to Microsoft 365 Copilot customers worldwide today.


Get everything you need to successfully adopt Copilot Cowork on the[Microsoft Adoption site](https://aka.ms/CoworkAdoption) . For more information about Copilot Credits and cost management, see[this Microsoft Learn article](https://aka.ms/CopilotCredits/Learn) .


---


1This analysis was conducted by internal Microsoft teams. The analysis compared 125 test runs across a total of 12 light, medium, and heavy work data prompts in Copilot Cowork and Claude Cowork with their Microsoft 365 connector, both using model Opus 4.8. For Copilot Cowork, the costs were calculated using variable rates based on relevant models, context, tools, and runtime from our internal logs. For Claude Cowork, the costs were calculated using publicly available API rates based on token usage and Microsoft 365 connector usage. We used these costs to get a total price per run and compared the prices across light, medium, and heavy prompt sets between Copilot Cowork and Claude Cowork with their Microsoft 365 connector. Cost savings results based on tests conducted in June 2026. Actual costs and potential savings may vary depending on usage, configuration, time, and other factors.
