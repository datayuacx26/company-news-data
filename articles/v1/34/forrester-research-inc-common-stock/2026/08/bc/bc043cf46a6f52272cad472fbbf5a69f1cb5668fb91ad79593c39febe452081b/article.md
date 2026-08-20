---
schema_version: "1.0.0"
document_id: "bc043cf46a6f52272cad472fbbf5a69f1cb5668fb91ad79593c39febe452081b"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/anthropics-pricing-shift-puts-ai-consumption-risk-back-on-customers/"
published_at: "2026-08-12T15:43:14+00:00"
first_seen_at: "2026-08-12T16:39:57.080475+00:00"
fetched_at: "2026-08-12T16:39:58.361453+00:00"
content_hash: "sha256:ca5075618c37f7ccc54d89d2708234b49a3b8e2c420dd829b396647bfd167ab1"
---

# Anthropic’s Pricing Shift Puts AI Consumption Risk Back On Customers

Back in May 2026, Anthropic announced


[changes to its pricing model](https://platform.claude.com/docs/en/about-claude/pricing) . Its original fixed-fee, per-seat subscription model was replaced with one that separates platform access from AI consumption. Customers still pay for access, but usage is now metered and billed separately based on token consumption.


Under the previous model, customers were split into two user tiers: Premium ($200 per user per month) and Standard ($40 per user per month), with API discounts typically ranging from 10% to 15%. Those economics only worked if most users consumed a small fraction of the value included in their subscriptions. Agents broke that assumption. SDKs, automation, GitHub Actions, and scripts can consume at a scale that humans simply can’t. Anthropic’s new pricing model reflects this shift. Users are now priced by role, with Claude.ai for nontechnical users at $10 per user per month and Claude Code for technical users at $20 per user per month and no API discounts.


The change also extends beyond chat. Services such as the Claude Agent SDK, Claude Code GitHub Actions, the claude -p command, and third-party applications authenticated through Anthropic consume separate monthly Agent SDK credits. Those credits expire each month and do not roll over. Once exhausted, usage either shifts to standard API pricing or stops until credits reset.


While Anthropic positioned the changes as a benefit through lower seat prices and added credits, the reality is costs are now driven by consumption. Token usage, model selection, Claude Code adoption, and commitment levels will have a far greater impact on spend than seat counts. So a small group of power users can drive a disproportionate share of costs, turning successful AI adoption into both a productivity win and a budgeting challenge.


Our clients have reacted negatively, arguing that tighter usage limits and consumption-based pricing massively reduce the value that made Claude Code attractive to developers. Many have also criticized Anthropic’s communication as unclear about what was changing and why. Some believe Anthropic framed the changes as a community benefit when the practical outcome for heavy users will be significantly higher costs.


So what does this mean for you? FinOps teams now have another cost layer to manage as consumption-based pricing introduces greater forecasting complexity and budget volatility.


If this sounds familiar, that’s because Anthropic is heading in the same direction as the rest of the enterprise AI market. I’m seeing a growing separation between platform access and AI consumption.


[OpenAI](https://help.openai.com/en/articles/20001106-codex-rate-card) combines subscriptions with credit- and token-based usage.


[Google](https://support.google.com/gemini/answer/16275805?hl=en) measures Gemini AI usage by the compute required, not by the request number.


[Cohere](https://cohere.com/pricing) has long favored consumption-based pricing tied to usage. Although these are new changes, the fixed-fee per user pricing structure was never meant to stay. Rather, it was the “the first one’s free” approach to entice tech users with a later switch to consumption pricing models. The market is converging on a model where customers pay for access and pay again for what they consume.


What customers need to remember is that the most important number in the agreement is no longer the seat price. Instead, it is the usage assumption and forecast used to calculate the commitment.


Remember these three things:


- **Question and validate the forecast** .


**** Before signing, validate the forecast. Challenge the commitment size by asking for the usage assumption to arrive at the calculated commitment level. If those assumptions do not reflect your expected deployment, challenge them early to avoid overcommitting.


- **Negotiate for flexibility.** Ask for annual commitment measurement or, at minimum, a quarterly consumption corridor that allows usage to fluctuate without affecting pricing or discount eligibility.


- **Buffer for overages.** Strong adoption can quickly increase consumption and spend, so negotiate predefined overage pricing, expansion discounts, or tiered economics that improve as usage grows.


Anthropic didn’t lower prices, but it also didn’t do anything out of market norms. It just pushed the onus of consumption management, forecasting, and governance back on to its customers.


Craving more insights on AI cost management or the specifics of Anthropic price changes? I’d love to connect with you via inquiry or guidance sessions.


Share


-
-
