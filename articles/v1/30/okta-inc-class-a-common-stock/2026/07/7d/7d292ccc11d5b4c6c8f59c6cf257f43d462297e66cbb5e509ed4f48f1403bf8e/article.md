---
schema_version: "1.0.0"
document_id: "7d292ccc11d5b4c6c8f59c6cf257f43d462297e66cbb5e509ed4f48f1403bf8e"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/company-and-culture/founders-in-focus-itay-keren-of-bloom-security/"
published_at: "2026-07-30T07:00:00+00:00"
first_seen_at: "2026-07-31T19:31:06.584573+00:00"
fetched_at: "2026-07-31T19:31:07.432136+00:00"
content_hash: "sha256:aad49dd09f8fa99b85c7bd360deb666f6ebe5369430c43023d308436264cfba2"
---

# Founders in Focus: Itay Keren of Bloom Security

Each month, we’ll highlight one of the founders of[Okta Ventures](https://www.okta.com/company/okta-ventures/) ’ portfolio companies. You’ll learn more about them and how they work with Okta. This month, we’re speaking with Itay Keren of Bloom Security.


**TL;DR:** Bloom Security provides a unified security platform for AI-native endpoints, addressing risks from AI agents, MCP servers, and IDE plugins. By integrating Okta’s identity context into Bloom’s risk engine, security teams gain real-time visibility, contextual risk scoring, and automated enforcement fleet-wide.


## What is Bloom Security, and what is your mission?


Bloom Security is the unified security platform for the AI-native endpoint.


Bloom addresses what the team sees as the defining endpoint security challenge of the AI era. Endpoints have evolved into ecosystems of AI tools, agentic workflows, browser extensions, and integrated development environments (IDE) plugins that traditional endpoint detection and response (EDR) solutions simply weren't built to see. This creates an unmonitored attack surface that's already being exploited.


Bloom gives security teams end-to-end control over this new software layer, from the moment a tool attempts to enter the environment to the moment it runs on an endpoint. Bloom provides full visibility, risk analysis in context, automatic remediation, and a supply-chain firewall that blocks risky software before it lands. Security teams get real control without slowing down their builders.


## What were you doing prior to Bloom Security that led you to this moment?


Before entering cybersecurity, I served as a Naval Officer. Leading teams where the margin for error is zero teaches you things about risk and responsibility that stay with you forever.


More than a decade in cybersecurity followed. I wasn't just building products—I was learning how to build great companies. I went through two acquisitions, at Demisto and Dig Security, and spent years at Palo Alto Networks seeing enterprise security at its most mature. These experiences gave me a clear sense of what it actually takes to build something that lasts.


But I always knew I wanted to build something of my own—something that solves a real problem.


At Dig Security, I found the people I wanted to build it with. My co-founders, Itay Frishman and Ofir Balassiano, came with the same drive and depth. Balassiano led security research at Dig and later the Cortex Cloud Posture Security group at Palo Alto Networks, and is now Bloom’s Chief Product Officer. Frishman, our Chief Technology Officer, built core AI security posture management (AISPM) and data security posture management (DSPM) solutions across both companies.


We had worked through hard problems together and understood enterprise security from the inside. We saw the same gap forming on the endpoint that nobody was addressing. That was the beginning of Bloom.


## What is Bloom’s solution? What challenge does it solve?


The core challenge is simple: The endpoint became an ecosystem, and security tools haven't kept up. Employees assemble agents, Model Context Protocol (MCP) servers, browser extensions, and code packages every day. Yet most security teams can't answer these basic questions: What's running across the fleet? How is it configured? What can it access, and what does it do with that access?


Bloom enables security teams to get ahead of that.


We close the loop and provide complete coverage, from discovery to enforcement. It starts with visibility, providing a live, fleet-wide inventory of every extension, plugin, package, and AI tool on every endpoint.


From there, the platform analyzes risk in context, and that's where things get interesting. The same tool can be completely acceptable on one endpoint and a serious risk on another, depending on who's running it, what data they can access, and what else is installed alongside it.


When Bloom detects a risk, security teams can act immediately by removing tools, revoking permissions, and fixing misconfigurations, all from a single view. For AI agents specifically, Bloom enables teams to define what they can access, execute, and transmit, and automatically scales back overpermissioned configurations to safe defaults.


On the prevention side, a supply-chain firewall intercepts marketplace requests and blocks risky software before it ever reaches an endpoint. Security teams set policies by tool, user, role, or team, and Bloom enforces them fleet-wide, continuously. For AI agents, Bloom enforces runtime guardrails that define and limit what each agent can access, execute, and transmit in real time across every endpoint.


Dozens of large enterprises across the US and Europe already use Bloom Security. Teams consistently tell us they finally feel like they know what's on their endpoints and can actually do something about it.


## Why did Bloom want to work with Okta?


Context is everything in endpoint security. The same tool and the same configuration can be completely acceptable on one endpoint and a real risk on another. What makes the difference is understanding the user: their role, their access, and what systems they reach.


That's exactly what Okta provides. As the enterprise identity layer, Okta holds the context Bloom needs to make precise endpoint-risk decisions. When Bloom knows not just what's running, but who is running it and what they're authorized to do, the risk picture sharpens significantly.


But the reason it's Okta specifically comes down to where it sits in the enterprise. Okta is the identity standard for the security-conscious organizations we work with. It's independent, deeply embedded, and trusted by the largest enterprises in the world.


And as both companies move deeper into AI agent security, the alignment becomes natural. Okta governs agent identity and access, while Bloom adds the endpoint layer beneath it.


## How is Bloom Security working with Okta? What support do you look for in a corporate partner?


On the technical side, we're working on integrating Okta's identity context directly into Bloom's risk engine. The idea is straightforward: When Bloom evaluates a tool on an endpoint, it pulls the user's role, group membership, and access profile from Okta to sharpen the risk score. The same extension looks very different on a finance executive than on a junior developer. Okta makes that distinction possible at scale.


Beyond the product work, Okta has opened doors into the enterprise security community in a way that would take years to build independently. That kind of introduction matters early.


What we look for in a corporate partner is three things. First, they have genuine domain expertise we can learn from. Second, a shared view of where the market is heading. Third, a willingness to build something new together rather than just co-market what already exists.


Okta brings all three. They understand identity in depth, making our conversations genuinely useful, and they think seriously about the same AI security challenges we do. That's a rare combination in a corporate partner.


## What trends do you expect to see in the endpoint industry?


Endpoint security is at an inflection point similar to what cloud security went through a decade ago. We didn’t secure the cloud by making firewalls smarter. Cloud security required an entirely new category designed specifically for that environment—one that sees everything, understands risk, and enforces what belongs there. That shift produced some of the most important security companies of the last decade.


The same thing is happening on the endpoint now. EDR solved a real problem and will keep solving it, but the modern endpoint has outgrown it. AI agents, extensions, MCP servers, and code packages are not malware, and they don't behave like malware. They need a different kind of security: one built around visibility, context, and governance.


The future we are preparing for is one where every desktop runs dozens of small, specialized AI agents alongside local language models—each handling a specific task, with its own permissions, and connecting to different services—across every role in the organization. All of this happens simultaneously on the same device. The security challenge this creates is an order of magnitude larger than what existing tools were ever designed to handle.


The right solution will need to be broad. It has to cover the full endpoint, every type of AI tool, and every stage from discovery to enforcement. And it has to integrate deeply with the rest of the security stack: identity, EDR, and security information and event management (SIEM). No single tool operates in isolation. The winners in this category will fit into how security teams already work, not ask them to start over.


A new category is forming. That's where the endpoint industry is heading, and it's where Bloom is focused.


## Partner with Okta Ventures


Are you building the future of enterprise security?[Get in touch](https://www.okta.com/cdn-cgi/l/email-protection#24455157504d4a0a4556414a5746415643020712101f4b4f50450a474b49) to learn how you can grow your startup alongside the world’s identity company.


About the Author


[Austin Arensberg VP, Okta Ventures Austin leads Okta Ventures, overseeing startup investments in privacy, identity, and security. Austin started his career in Asia where he spent over 10 years investing around the world in both private equity and corporate development. The past 6 years Austin has invested in over 50 early stage technology startups based out of San Francisco. Austin is passionate about international exchange and outreach and is Board Member Emeritus of the Princeton in Asia Fellowship program, and was also a Marshall Memorial Fellow.](https://www.okta.com/blog/author/austin-arensberg/)
