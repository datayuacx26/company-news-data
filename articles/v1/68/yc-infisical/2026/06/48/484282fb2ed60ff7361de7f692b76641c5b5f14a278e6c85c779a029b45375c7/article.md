---
schema_version: "1.0.0"
document_id: "484282fb2ed60ff7361de7f692b76641c5b5f14a278e6c85c779a029b45375c7"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-news-import-3518eccf87d2"
canonical_url: "https://infisical.com/blog/idira-rebranded-cyberark"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-21T23:48:38.169985+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:8a6c2198c0c0f9ce4d03013cc6daf3299496d05cea8e993e7c0c9c50aa33bdfa"
---

# Idira (the Rebranded CyberArk): The Complete Guide

CyberArk is one of the oldest names in identity security. They're best-known for privileged access management (PAM), but their products encompass secrets management, certificate management, and more. After operating independently since 1999,[Palo Alto Networks (PANW) acquired CyberArk](https://www.prnewswire.com/news-releases/palo-alto-networks-completes-acquisition-of-cyberark-to-secure-the-ai-era-302685041.html) and rebranded its offerings to the security platform Idira.


The rebrand was a significant move that confused users because the CyberArk brand continues to exist alongside Idira. It left many teams wondering: what changes for anyone who relies on CyberArk? Will CyberArk continue to receive updates? Will I be forced to buy Idira instead? Will it get more expensive?


The short version is that Idira is the new brand for CyberArk's technology, extended with Palo Alto Networks' push into machine and AI agent identity. If you are evaluating it as a[secrets management](https://infisical.com/blog/secrets-management-complete-guide) or[privileged access management](https://infisical.com/blog/best-privileged-access-management-solutions) solution, the rebrand matters less than the underlying architecture, pricing, and operational model. Those (as far as public information goes) seem to carry over to the new brand.


Security infrastructure is expensive to adopt and painful to migrate, especially for tools with the operational complexity of Idira/CyberArk, which frequently require full-time teams to operate. When evaluating security infrastructure, it's worth understanding this. The rebrand raises practical questions:


- What is Idira actually?
- What does the CyberArk rebrand mean for customers?
- What does the platform offer?
- Where does Idira/CyberArk work well, and where does it struggle?


With products that bundle many services into one, it's worth understanding the fundamentals first.


## What is Idira?


Idira is Palo Alto Networks' identity security platform. It is built to discover, control, and govern identities across an organization. This includes:


- Human users
- Machine workloads
- AI agents.


Palo Alto Networks completed its acquisition of CyberArk, a[deal widely reported at around $25 billion](https://www.tradingview.com/news/reuters.com,2026:newsml_TUA5XWZBT:0-palo-alto-networks-completes-acquisition-of-cyberark-to-secure-the-ai-era/) , and in May 2026 folded the CyberArk portfolio into a single brand called Idira. The platform is positioned around three identity domains:


- **Human identity security:** PAM, identity and access management (IAM), and identity governance and administration (IGA). This is the classic CyberArk territory.
- **Machine identity security:** secrets management, certificate lifecycle, and governance for the non-human workloads that now vastly outnumber human users.
- **AI agent security:** discovery, control, and governance for autonomous agents, which is the newest and least proven part of the suite.


The documentation lists many additional services that bridge the gap between these or are variations of them. Besides the AI agent piece, most of these mirror pre-acquisition CyberArk products.


The acquisition and subsequent rebrand are confusing, especially since Palo Alto Networks preserves the CyberArk brand.


## How has CyberArk itself changed after the acquisition?


After the acquisition, CyberArk's offerings have changed meaningfully:


- CyberArk's open-source version of Conjur (its secrets management solution) has stopped receiving regular, meaningful updates.
- Conjur's hosted version has been renamed to "Secrets manager SaaS", while the self-hosted one is now called "Secrets Manager Self-Hosted"


Palo Alto Networks itself has stated that it will continue support and development of existing CyberArk products. How that support and development will look is currently unknown. PANW has announced plans for deep integrations with its Strata and Cortex products and hints at a converging roadmap with Idira.


In the short- or medium-term, existing CyberArk customers won't lose the products they've been paying for, and CyberArk's products will remain available for new customers. The product roadmap, however, is uncertain, especially if the product moves progressively more under the Palo Alto Networks umbrella.


## CyberArk vs. Idira: what actually changed and which to choose


Palo Alto Networks reassures existing CyberArk customers that the underlying technology and operational model stay the same. Their products have long been known as capable, but complex, often requiring specialized training and dedicated teams.


This complexity arose because CyberArk's product palette is itself a result of 10 acquisitions, including its certificate management/PKI tool (the erstwhile Venafi) and its secrets manager Conjur, which CyberArk acquired in 2017. Subsuming CyberArk entirely into Idira may further increase that complexity.


Since each CyberArk product has an Idira equivalent, the new platform likely operates similarly to CyberArk, although there's little public information about the exact parallels.


The branding itself did shift. The products inherit their CyberArk naming conventions, with Secrets Hub rebranding to Idira Secrets Hub, Privilege Cloud rebranding to Idira Privilege Cloud, and so on.


For now, the biggest changes seem to be cosmetic:


- **One brand, one control plane.** Palo Alto Networks consolidated CyberArk products under the Idira umbrella with a centralized control plane.
- **An explicit machine identity and AI agent push.** Idira explicitly mentions AI agents and machine identities in its new positioning.


If you were already running CyberArk, the rebrand is mostly cosmetic in the near term. If you are evaluating the platform fresh, treat Idira as CyberArk's mature PAM and secrets technology with a less proven machine and AI layer.


It's difficult to advise on which solution to choose. CyberArk's accepts new customers, but its future seems uncertain. After IBM's acquisition of HashiCorp (and the subsequent shutdown of HCP Vault Secrets), worrying about the future of one's security infrastructure is justified.


## How Idira is structured: the all-in-one promise


Idira's pitch is breadth. One vendor, one platform, every identity type, which roughly mirrors that of CyberArk, minus the agentic AI angle. CyberArk's technology (which powers Idira) is almost 30 years old, which has led it mature, but also means it has accumulated complexity, which is the biggest complaint.


A few components are worth understanding because they are where most evaluations actually land:


- **Idira Secrets Manager** is the secrets manager, built on CyberArk's Conjur lineage and available self-hosted or as a managed SaaS offering.
- **Idira Secrets Hub** is a governance layer that connects to cloud-native vaults you already run.
- **The PAM suite** handles human privileged access: session isolation, credential vaulting, just-in-time elevation, and the audit trails that compliance frameworks demand.


Palo Alto Networks separately acquired the AI gateway provider[Portkey](https://finance.yahoo.com/sectors/technology/articles/palo-alto-networks-panw-completes-112329938.html) , whose technology presumably contributes the AI agent security features like agent discovery, privilege control, task-scoped access, and more.


Idira includes a variety of other products, including solutions for[certificate management](https://infisical.com/blog/certificate-management) , password management, among others.


This breadth unites the security stack under one roof. This is convenient for governance and reporting, but means that each domain brings its own deployment, configuration, and operational surface.


This complexity may not be worth it if you only need a secrets manager ([see how Conjur compares to Infisical](https://infisical.com/compare/infisical-vs-cyberark-conjur) ), but works for anyone setting up a large security stack from scratch.


## Where Idira works well


Idira's ideal customer is a large enterprise where hiring a few extra infrastructure engineers makes no difference, and/or where CyberArk is the entrenched solution. These strengths are the reason CyberArk survived for almost 30 years, and continues to sell:


- **Mature, deep PAM.** For human privileged access in large enterprises, CyberArk built the category, and that depth carried into Idira. Session recording, credential vaulting, and just-in-time elevation are well-developed and widely deployed.
- **Compliance and audit pedigree.** If you operate under tight regulation, Idira's audit trails and governance reporting are built for auditors and recognized by them.
- **Genuine breadth.** For an organization that wants one vendor relationship with one governance view, the breadth of tools matters.


Idira is a credible solution in every category where it offers a product, which creates the complexity and clunkiness that's its weakness.


## Where Idira falls short


The same characteristics that make Idira strong for large enterprises make it a poor fit for many other teams.


- **Deployment and adoption are heavy.** The self-hosted secrets manager in particular is known for a steep setup curve, and forcing adoption is hard when the tooling makes a developer's day-to-day life more cumbersome. Tools that add friction get worked around, and workarounds are where[secret sprawl](https://infisical.com/blog/what-is-secret-sprawl) comes from.
- **It is closed-source.** Idira offers no open-source core of the current product to inspect, self-host transparently, or try before a sales conversation. You can self-host Idira's solutions, but the products are fundamentally closed-source.
- **Innovation pace after acquisition.** The identity security market is full of products built decades ago and now owned by large, slow-moving acquirers. Acquirers frequently prioritize integrations with their own products and slower development over innovative bets.
- **The AI agent layer is early.** Treat the agentic identity capabilities as a forward-looking bet rather than a mature product line. Idira includes an AI agent security product, but[agent security paradigms](https://infisical.com/blog/credential-brokering-for-ai-agents) are still evolving.


One point of contention with CyberArk has always been pricing. Idira seems to continue that tradition.


## Idira pricing and total cost of ownership


Idira inherits CyberArk's enterprise model: no public pricing, licenses are (probably) charged per identity, and many advanced deployments likely require professional services.


This means the total cost of ownership is high and opaque. Pricing is hidden, but CyberArk's rates have always been at the top end of the market. A mid-sized deployment can run into the hundreds of thousands annually, especially when you factor in operational costs and dedicated engineers.


Professional services are frequently required for onboarding, migration, or custom development, which are an additional (if one-time) cost on top of that. We broke down[CyberArk Conjur pricing](https://infisical.com/blog/cyberark-conjur-pricing) on this blog. While Conjur is only a secrets manager, the fundamentals should carry over decently well.


## Who Idira is a good fit for, and who it isn't


It is a good fit when:


- You are a large, regulated enterprise with a dedicated identity security or PAM team.
- You can stomach long migrations, onboarding periods, and potentially hiring new engineers with specific knowledge.
- Cost is not a concern.
- Human privileged access and compliance audits are your central problems.
- You already run CyberArk and/or have the institutional knowledge to do so.
- There's little urgency and the migration or adoption period can span months to years if it needs to.
- You're fully on board with Idira's product direction and/or already use Palo Alto Networks' products.


It is a poor fit when:


- Developer experience is an important concern and you want to simplify the operational side of security.
- You're a small or mid-sized team or an enterprise that wants to move quickly.
- You want to avoid custom development or hiring new engineers to operate your identity security stack.
- Open source and transparency are requirements you can't live without.
- Your primary need is modern secrets management, certificate management, or almost anything other than PAM.
- You want to adopt security tooling quickly.
- You're not sure about the direction of CyberArk's technology under PANW's umbrella.


## Idira alternatives: where Infisical fits


Many teams choose Infisical over Idira for its developer experience, open-source nature, and simplicity.


Infisical is an open-source identity security platform that unifies Secrets Management,[Certificate Management](https://infisical.com/blog/certificate-management) ,[key management](https://infisical.com/docs/documentation/platform/kms/overview) (KMS), and Privileged Access Management in one platform.


It shares Idira's all-in-one philosophy and contains many of its important features (e.g. granular audit logs, access policies, and controls), but inverts other Idira/CyberArk design choices:


- **Open source and flexible to deploy.** Inspect the code, self-host across any cloud or on-premises, or run it as a managed service. Especially larger organizations often opt for Infisical for the open-source flexibility.
- **Built on PostgreSQL, not proprietary infrastructure.** Deployment and scaling lean on well-known technology, while CyberArk's Digital Vault runs on proprietary technology. This means operating Infisical can often be done by infrastructure or DevOps teams rather than requiring dedicated staff with specialized knowledge.
- **Developer-first by default.** Workflows, a CLI for local dev, a usable dashboard, approval flows, secret rotations, and dynamic short-lived secrets work out of the box, so teams get value on day one rather than after a months-long rollout.


Infisical is a modern alternative to legacy vendors like Idira and CyberArk, which focuses on the developer experience to make security simple, avoid brittle workarounds and obviate expensive custom development.


If you are weighing your options,[our rundown of the best secrets management tools](https://infisical.com/blog/best-secret-management-tools) puts the landscape side by side.


[Try Infisical today for free](https://app.infisical.com/signup) or[book a demo](https://infisical.com/talk-to-us) to see it in action.
