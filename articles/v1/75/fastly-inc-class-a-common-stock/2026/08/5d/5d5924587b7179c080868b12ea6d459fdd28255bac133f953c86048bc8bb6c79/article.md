---
schema_version: "1.0.0"
document_id: "5d5924587b7179c080868b12ea6d459fdd28255bac133f953c86048bc8bb6c79"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/beyond-bot-block-fastly-experian-turning-ai-agent-traffic-into-revenue/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T15:10:49.393709+00:00"
fetched_at: "2026-08-07T15:10:50.553140+00:00"
content_hash: "sha256:f52cb25a7a339171608934c11dab4039ab780927d5e1dcf1e4f2c1730435d8d1"
---

# Beyond the Bot Block: How Fastly and Experian Are Turning AI Agent Traffic into Revenue

For years, security teams have operated on a simple rule: when non-human traffic spikes on your network, rate-limit it, challenge it, or block it.


That playbook made sense when automated traffic mostly consisted of aggressive scrapers, brute-force attacks, and credential stuffers. The problem is that **automated traffic isn't just scrapers anymore** .


Today, autonomous AI agents are browsing, comparing SKUs, and attempting to check out on behalf of real human buyers.[Automated traffic made up 53%](https://www.forbes.com/sites/renanaashkenazi/2026/06/19/more-than-half-of-web-traffic-is-bots-ads-cant-survive-it/) of all web activity last year with[agentic AI influencing $262 billion in 2025 holiday sales](https://www.salesforce.com/news/stories/2025-holiday-shopping-data/) . By 2030, McKinsey estimates[agentic commerce could drive up to $1 trillion](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-agentic-commerce-opportunity-how-ai-agents-are-ushering-in-a-new-era-for-consumers-and-merchants) in U.S. sales alone.


Agentic commerce isn’t on the horizon – it’s running on your network right now. But for many enterprises, autonomous agent traffic is still treated exclusively as a security liability rather than a revenue opportunity.


And that’s why we’re excited to announce that ****[Fastly has joined the Experian Agent Trust™ ecosystem](https://www.experianplc.com/newsroom/press-releases/2026/fastly-joins-experian-agent-trust--ecosystem-to-advance-trusted-) . Together, we’re helping organizations move from simply blocking automated traffic to verifying, authorizing, and monetizing legitimate AI agents in real-time at the network edge.


## Why Trust Decisions Must Live at the Edge


Validating an AI agent presents a unique architectural challenge. Your application needs to answer two questions instantly:


-


*Who is actually behind this agent?*


-


*Does it have the authority to make this purchase?*


If you route every agent validation request back to your origin servers to check centralized databases, you introduce massive latency, waste compute resources, and degrade the user experience. But relying on static, legacy rules at the perimeter means turning away legitimate buyers.


The edge solves both problems at once.


By extending[Experian Agent Trust](https://www.experian.com/blogs/news/2026/04/30/experian-agent-trust/) to Fastly’s globally distributed edge network, trust decisions happen in milliseconds – closest to the user and the agent, and long before a request ever touches your backend infrastructure.


## Executing Trust Logic at the Edge


Experian Agent Trust establishes identity and delegated authority through **Human to Agent Binding,** securely connecting verified individuals, their devices, and the AI agents acting on their behalf.


Fastly acts as the high-performance enforcement engine for those trust signals:


-


**Verification in milliseconds** : Using Fastly’s programmable platform, identity-checking frameworks are evaluated directly on our distributed edge network, including the[Skyfire](http://www.tryskyfire.com/) open protocol,[Know Your Agent (KYAPay)](https://www.fastly.com/blog/how-fastly-skyfire-enable-trusted-agentic-commerce-at-the-edge) , which is used by Experian Agent Trust.


-


**Programmable edge policy:** Rather than relying on binary allow-or-block rules, teams can write custom edge logic to apply identity-based rate limits, dynamic pricing, or custom checkout flows based on the agent’s verified trust score.


-


**Zero backend re-architecting:** Fastly plugs into your existing APIs, authentication workflows, security policies, and payment gateways. You don’t need to rebuild your backend or redesign your application stack to participate in agentic commerce.


Kathleen Peters, Chief Innovation Officer at Experian, said:


“The future of digital commerce isn’t just human to business. It’s human-authorized AI agents acting securely on consumers’ behalf. Experian Agent Trust™ provides the identity and delegated authority framework that makes this possible. Together with Fastly, we’re helping enterprises move beyond simply blocking bots to recognizing trusted AI agents in real time, enabling secure agentic commerce without compromising security or customer trust.”


Ultimately, that’s the real change here. By taking the friction out of agent verification at the perimeter, enterprises don’t have to choose between strict edge security and capture-ready commerce – you get both on the exact same network layer.


## Turning Network Traffic into Revenue


The shift to agentic commerce doesn’t mean lowering your security posture – it means making edge security smarter.


By pairing Experian’s trust ecosystem with Fastly’s programmable edge, enterprises can turn autonomous agent traffic from a blind security risk into an accountable, monetizable sales channel, keeping origin infrastructure protected while opening the door to new revenue.


Ready to prepare your edge for agentic commerce? Don't let your traditional bot policies block real revenue.[Talk to a Fastly expert](https://www.fastly.com/contact-sales) to see how our programmable edge platform helps you identify, authorize, and monetize trusted AI agent traffic – without re-architecting your backend.
