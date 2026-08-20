---
schema_version: "1.0.0"
document_id: "ea625d367e6d7f1d7f9e66c772c9a0fa225b24c0c02319f685e53ddaf8f91a68"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-b1ffd1321f3f"
canonical_url: "https://www.microsoft.com/en-us/microsoft-365/blog/2026/06/02/introducing-microsoft-scout-your-always-on-personal-agent/"
published_at: "2026-06-02T18:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.173713+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:b0b78d94b0fd3bfb5fc0e171c5db4c2861d4a643cf37f79ce95e962c3d7a0315"
---

# Introducing Microsoft Scout: Your always-on personal agent

Work is moving forward in new ways, with the rhythm shifting from single exchanges to something more continuous. Most systems still stop at answering the question. The real unlock is in the follow-through, where systems hold your priorities and act on them for you, under your control.


## A new category of agents


Today we are introducing a new category of agents called Autopilots. Autopilots are always-on agents that work autonomously, with their own identity, and act on your behalf.


Autopilots stay active in the background, understand how work gets done across your apps and systems, and take action without needing to be prompted each time. Because they operate with their own identity, they can carry out tasks within the permissions and policies you and your organization set. This creates a more durable way to keep work in motion so it continues even when your attention is elsewhere.


## Introducing Microsoft Scout


We are also introducing Microsoft Scout, our first Autopilot agent.


Microsoft Scout is integrated across the Microsoft 365 apps you use every day, keeping it grounded in your flow of work. It operates across cloud, desktop, and web, connecting to Teams, Outlook, OneDrive, and SharePoint, and to the data that powers your day, including chats, email, calendar, and contacts. You interact with it in Teams, and extend its reach through the desktop app to your browser, local resources, and model context protocol servers.


Microsoft Scout is built with enterprise-grade security and controls so it can be trusted in your organization from day one. It is powered by OpenClaw open-source technology, reflecting our commitment to building with the community while extending capabilities to meet enterprise needs.


Microsoft Scout reduces the coordination work that builds throughout the day. It can proactively schedule and coordinate meeting times across time zones, flag important meetings, and generate the materials you need to prepare while keeping you in the loop. It identifies upcoming deliverables, then automatically blocks time on your calendar to help you stay on track. It can also spot risks, like stalled decisions, so you can address them before they become blockers.


Over time, Microsoft Scout builds context powered by Work IQ, learning how you work, what you care about, and what needs to happen next. Work IQ carries work forward, becoming more useful, relevant, and aligned to your priorities.


## Contributing back to the open-source community


We are contributing policy conformance directly upstream to OpenClaw.


Organizations running OpenClaw will be able to validate whether their environment is configured within their security and compliance requirements, operating securely, and get a verifiable, audit-ready answer.


## Making Microsoft Scout ready for enterprises


Microsoft Scout adds the enterprise solution built on Microsoft 365: the identity, credential, and access controls that turn open-source capability into something you can safely run across your organization. Every agent operates under its own governed Entra identity, not a shared, anonymous service account, so the work it does is attributable to a known actor your directory already understands.


The credentials behind that identity are protected end to end: scoped to the task at hand, redacted from logs or diagnostics, and managed with the same rigor you expect from any first-party Microsoft service. When Microsoft Scout acts on your behalf, you know precisely whose authority it carried and that nothing sensitive leaked along the way.


Identity tells you who is acting; access control determines what they can do. With Microsoft Scout, agents can only reach the resources and destinations you’ve approved. Sensitive actions can require a human to sign off before they proceed, and data protection policies from Microsoft Purview, including sensitivity labels and loss prevention, are enforced in the moment, before anything is sent or written. Microsoft Scout doesn’t bypass these controls; it operates within them, adhering to the protections your organization has already configured.


## Getting started with Microsoft Scout


Microsoft employees have already been using an early Microsoft Scout desktop experience. We built this to learn how always-on agents show up in real work, and we are seeing it take on coordination, surface risks earlier, and keep work moving without constant prompting.


We are now extending that early experience to a select group of customers in private preview and to Frontier organizations. Microsoft Scout is available as an experimental release through Frontier, giving customers a chance to explore how Microsoft Scout can fit into their own workflows.


Access requires Frontier enrollment, Intune policy configuration, and an opt-in attestation. Users with a GitHub Copilot license can then download and install the experience. **[Full setup instructions are available](https://learn.microsoft.com/microsoft-scout)** .


---


Microsoft Build brings together innovations across our platforms, many of which are now showing up in Microsoft 365 Copilot:


- [Announcing the new Work IQ APIs](https://www.microsoft.com/en-us/microsoft-365/blog/2026/06/02/announcing-the-new-work-iq-apis/)


- [Work IQ: Production-ready intelligence for every agent](https://aka.ms/WorkIQAPI_GA)


- [Build collaborative agents where work happens](http://aka.ms/TeamsPlatform-Build)


- [Frontier Tuning: Teaching AI to work the way you do](https://aka.ms/frontiertuningblog)
