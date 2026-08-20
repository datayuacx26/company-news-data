---
schema_version: "1.0.0"
document_id: "273af25a52dd195e076e41b8eac000920d6c8f59be0a79566b73d40d507bcf0d"
company_key: "yc-worklytics"
company: "Worklytics"
source_id: "yc-worklytics-news-import-9ab18239f248"
canonical_url: "https://www.worklytics.co/blog/tracking-codex-usage"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-24T07:25:13.507600+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:5f19535293e941838cbebc49bfb3e2064adde8f4e65407e8cc24355c56d959e0"
---

# Tracking Codex Usage Across Your Engineering Team

Codex has evolved from an autocomplete experiment into a full agentic coding platform - a CLI, an IDE extension, cloud tasks that run autonomously, and automated code review on pull requests. If your organization runs ChatGPT Enterprise with Codex enabled, your engineers are probably already using it. The questions leadership asks next are predictable: who is actually using it, what is it costing us, and is it making the team faster?


The good news is that OpenAI now ships a substantial analytics stack for Codex. The less good news is that, like every vendor-native dashboard, it only sees its own tool. This guide covers what you get out of the box, how to pull the data programmatically, and how to put Codex usage in context alongside the rest of your AI stack.


## Why Track Codex Usage at All?


**Credits are real money.** Codex usage on enterprise plans consumes credits, and agentic workloads - long-running cloud tasks, automated code review on every PR - consume them much faster than chat ever did. Without per-team visibility, the first signal you get is the invoice.


**Adoption is always uneven.** In most organizations a minority of developers generate the majority of AI-assisted output, while a long tail barely touches the tools they're paying for. Knowing which teams are ahead lets you spread what's working instead of guessing.


Codex adoption by department, led by Engineering, IT and Product


‍


**Agentic tools change the governance picture.** A tool that autonomously edits code, opens pull requests, and comments on reviews needs an audit trail. Security and compliance teams will ask for one — better to know where it lives before they do.


‍


## What OpenAI Gives You Natively


### The Codex Analytics Dashboard


Workspace administrators and analytics viewers in ChatGPT Enterprise get a self-serve Codex analytics dashboard. As of mid-2026 it covers:


- **Active users by product surface** - CLI, IDE extension, cloud, desktop, and Code Review, so you can see *how* engineers use Codex, not just whether they do
- **Credit and token consumption** broken down by surface and by model
- **Activity metrics** - threads and turns by client, with daily and weekly views
- **A user ranking table** sortable by credits, threads, turns, text tokens, and engagement streaks
- **Code Review throughput** - PRs reviewed, findings by priority, comments, replies, reactions, and feedback sentiment
- **CSV or JSON export** for workspace usage, per-user usage, and Code Review details


Two practical caveats: usage data can lag by up to 12 hours, and the dashboard is scoped to your ChatGPT workspace — usage through raw API keys is governed separately.


### The Codex Analytics API


For anything beyond eyeballing charts, OpenAI exposes the same data via an Analytics API at` api.chatgpt.com/v1/analytics/codex` , authenticated with an API key scoped to` codex.enterprise.analytics.read` . It returns daily or weekly UTC buckets with up to 90 days of lookback, across three endpoint groups: workspace usage (threads, turns, credits, per-client token breakdowns), code reviews (PRs reviewed and comment counts by priority), and code review responses (how engineers react and reply to Codex's comments).


This is the right layer if you want Codex metrics in your own BI stack - joining usage against your HR system's team structure, or against delivery data from your issue tracker. If you're already exporting LLM telemetry, this slots in alongside the[token and cost tracking](https://www.worklytics.co/blog/how-to-track-llm-token-usage-and-cost) pipelines you may have built for API workloads.


### The Compliance API


For audit and governance, the Compliance API exports activity logs - prompt text, Codex responses, user and workspace identifiers, timestamps, token usage - designed to feed eDiscovery, DLP, and SIEM systems. Logs are retained for up to 30 days, and coverage is limited to ChatGPT-authenticated usage. If your security team needs to answer "who did what, when, with which model," this is where that answer comes from.


## What Native Codex Analytics Won't Tell You


The dashboard answers "how much Codex?" thoroughly. It can't answer three questions engineering leaders actually get asked:


**How does Codex compare to the other tools we pay for?** Most engineering orgs now run two or more coding assistants - Codex alongside[Claude Code](https://www.worklytics.co/blog/tracking-claude-code-usage) ,[Cursor](https://www.worklytics.co/blog/tracking-how-employees-utilize-cursor-ai) , or[GitHub Copilot](https://www.worklytics.co/blog/tracking-copilot-utilization-in-your-organization) . Each ships its own dashboard, each with different metrics and definitions, and none of them will show you the others. Deciding where to consolidate spend requires a view none of the vendors provide.


**Is usage translating into outcomes?** Threads and turns measure activity, not impact. Whether high-Codex teams actually ship faster, review quicker, or collaborate differently requires joining usage data with delivery and collaboration signals - which lives outside any single vendor's scope.


Codex credit spend trend by surface — CLI and IDE, cloud tasks, and code review


**What about everyone outside engineering?** Codex is one surface of your AI investment. The same leadership deck that asks about Codex will ask about[ChatGPT usage](https://www.worklytics.co/blog/track-if-employees-use-chatgpt) in sales and marketing, Copilot in the Microsoft stack, and Gemini in Workspace.


## Seeing Codex in Context


This is the gap Worklytics fills. Worklytics aggregates usage signals across your AI stack - Codex, Claude Code, Cursor, Copilot, ChatGPT, Gemini - into one privacy-first dashboard, broken down by team and department, and correlates adoption with the collaboration and productivity signals already flowing through your organization. Metadata only: who used what and how often at the team level, never the content of prompts or code.


A practical starter set of metrics, whether you build it yourself from the Analytics API or use a platform:


1. **Weekly active Codex users as a share of engineering** - the basic adoption number
2. **Active users by surface** - CLI/IDE users behave differently from Code Review consumers; track them separately
3. **Credits per active user, by team** - your unit-economics number, and the early warning for runaway agentic spend
4. **Code Review engagement rate** - findings that get replies and reactions indicate the reviews are being read, not ignored
5. **Cross-tool split** - share of AI-assisted activity by tool, so consolidation decisions rest on data


## Frequently Asked Questions


**Does OpenAI provide a built-in way to track Codex usage?**
Yes. ChatGPT Enterprise workspaces with Codex enabled get an analytics dashboard showing active users by surface (CLI, IDE, cloud, desktop, Code Review), credit and token consumption by model, threads and turns, user rankings, and Code Review activity. Data can lag by up to 12 hours, and admins can export it as CSV or JSON.


**Can I see Codex usage per developer?**
Yes. The dashboard includes a per-user ranking table sortable by credits, threads, turns, and tokens, and the Analytics API returns per-user usage in daily or weekly buckets. Per-user exports include email addresses where workspace settings permit.


**How do I track what Codex is costing us?**
The analytics dashboard breaks credit and token consumption down by product surface and model, and the Analytics API exposes the same data for up to 90 days of lookback. For team-level cost accountability, join per-user credits against your org structure - natively the dashboard ranks individuals, not teams.


**Can our compliance team audit Codex activity?**
Yes, through the Compliance API, which exports prompts, responses, identifiers, timestamps, and token usage for integration with eDiscovery, DLP, and SIEM tooling. Note the limits: logs are retained for up to 30 days and only ChatGPT-authenticated usage is covered.


**How do I compare Codex usage against Claude Code or Cursor?**
There's no native way - each vendor's dashboard only sees its own tool, with different metric definitions. Cross-tool comparison requires either building a pipeline from each vendor's export or API, or using a platform like Worklytics that aggregates usage across all your AI tools into one consistent, privacy-first view.


‍


‍
