---
schema_version: "1.0.0"
document_id: "87b432b362a7428556ccf3ea45c1ffe9e7146df9ab719a3968626394042ad75b"
company_key: "yc-doola"
company: "doola"
source_id: "yc-doola-rss-a49d57f768e2"
canonical_url: "https://www.doola.com/blog/introducing-doola-formation-api-for-b2b/"
published_at: "2026-06-24T15:00:00+00:00"
first_seen_at: "2026-07-20T23:23:47.560138+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:0c538009b8a842eb189bceeb7397965709aa0d02a6121750f913eaf8851d97b5"
---

# Introducing doola Formation API for B2B

### Add Company Formation to Your Platform *in Days.*


[Become a Partner](https://www.doola.com/formation-api)


[Technology](https://www.doola.com/blog/category/technology/)


# Introducing doola Formation API for B2B


By Karishma Borkakoty


Published on 24 Jun 2026 Updated on 15 Jul 2026


11 min read Updated on 15 Jul 2026


The internet has a payments layer, a compute layer, and a delivery layer. But it has never offered the infrastructure layer for the most fundamental need of all, the company itself. Every other tool assumes founders already have one.


doola Formation API is that layer.


Through a single API or a live MCP,[you can now embed US company formation](https://www.doola.com/formation-api/) , EIN support, registered agents, and full lifecycle compliance directly inside your product. This infrastructure requires no SSN and no US residency, operating seamlessly across all 50 states.


Consequently, for platforms serving global founders, this completely bridges the divide. Users move from intent to formation natively, without ever leaving your interface or getting handed off to a third-party provider.


While most standard APIs stop at the initial filing and quietly assume a US resident is behind the keyboard, doola was built for the global founder from day one. In fact, our compliance infrastructure already serves founders across more than 175+ countries.


Today, that operational backend is accessible in four ways: a revenue-share affiliate program, an embedded iFrame, MCP support across the most popular AI environments, or a white-label API.


Ultimately, you choose the depth of your integration, while doola runs the formation, compliance, and complex operational layer underneath.


doola gives platforms the infrastructure to offer US company formation to users anywhere in the world through a single business formation API.


It also offers formation MCP support for AI-agent workflows. Live across Replit, Claude, ChatGPT, Lovable, Perplexity, and Vercel. →


[Know more](https://www.doola.com/blog/doola-mcp-form-your-llc-inside-ai-chat/)


No US residency required. No SSN required.


## What You’re Building On


Company formation is not a form submission. It is a legal and financial workflow that moves through state filings, IRS processing, document generation, and registered-agent assignment, with status changing asynchronously at each step and with no room for error at any step.


doola exposes that workflow as infrastructure your platform calls. You hit an endpoint; doola handles the filings, the IRS processing, and the document generation, and surfaces every state change back to your product programmatically. Your users never leave your interface to chase a document or check a status, because the status already lives in your product.


### How the doola Formation API Works


Capability What It Enables for Your Platform


Create / Get / List companies Submit and track a full US formation, LLC, or C-Corp programmatically, from inside your own product with no manual filing operations.


Live MCP
(separate agent interface) An agent can execute formation, not just recommend it. The only live formation MCP in the market.


Webhooks, signature-verified
(“do not poll”) Real-time status and document delivery, including the EIN letter, pushed to your endpoint. The SS-4/EIN passback fintechs need to unblock banking.


Documents API
(list + pre-signed URLs) Pull the EIN letter, Articles, and formation docs straight into your UI. The user never hunts through email.


Customers API
(one customer, many companies) Model your users natively and support repeat formations per user.


Reference data
(countries, states, NAICS, fees) Build a native formation UI with correct, current values. Pass state filing fees through transparently.


Idempotency keys on create Safe retries with no duplicate filings, which matters when every request is a real legal event.


Sandbox
(api.test.doola.com, dk_test_ keys) + OpenAPI spec Self-serve evaluation and build before a contract is signed. In production in as little as 48 hours.


Fully white-label doola stays invisible. Your platform keeps the brand, the funnel, and the customer relationship.


Downstream lifecycle via OAuth
(ITIN, Bookkeeping, Taxes, Annual Reports, Form 5472) Recurring revenue and retention after the one-time formation event. These are doola services delivered post-formation, via OAuth.


In production in as little as 48 hours. No contract required to start building.


The API simplifies this complex process into a straightforward, **two-step flow** :


1. **Create a customer**
2. **Submit a company formation**


From there, you don’t poll. Asynchronous progress is pushed to your endpoint via signature-verified webhooks, so your platform stays up to date without you building a status-check layer or running a polling loop against our API. Verify the signature, handle the event, update your UI.


### Seamless Document Delivery


When filings finalize, documents are available through the Documents API as pre-signed download URLs:


- EIN Letters


- Articles of Organization


- All Core Formation Documents


Pull them straight into your own UI, and your users see the finished documents exactly where they expect them, instead of digging through email for a third party’s attachments.


## doola: The Company Layer of the Internet


The following core pillars define exactly what kind of infrastructure partner doola is.


> “Founders come to doola because they need a real US business structure before they can bank, sell, raise, or operate globally. With the doola Formation API and MCP support, we’re bringing that infrastructure directly into the platforms where they’re already building. But formation is only the starting point. What matters just as much is helping founders stay compliant and supported as their business grows.”
>
>
> — Arjun Mahadevan, Founder and CEO, doola


### 1. The Full Compliance Lifecycle


Most formation tools just hand over the initial paperwork and walk away. doola handles the entire stack from end to end.


- **Comprehensive Coverage:** Formation, EIN, registered agent, ITIN, annual reports, and business taxes, all managed from the first API call through final IRS processing.


- **Built In-House:** If a step isn’t a clean API call yet, doola handles it directly. We never route your users to a chaotic third-party accountant network or a disconnected ITIN service.


- **Zero Management Overhead:** Your team doesn’t have to manage compliance on behalf of referred users. doola does it all for you.


### 2. Fully White-Labeled Infrastructure


doola is completely invisible to your end-users, ensuring your platform retains total ownership.


- **Your Brand, Your Funnel, Your Customer:** You own the entire customer relationship. Users form their companies seamlessly inside your product while doola’s infrastructure runs quietly underneath.


- **Captured Revenue:** Because the experience stays native, the revenue from the initial formation, and from every recurring compliance event that follows, flows directly through your product, not away from it.


### 3. Agentic Economy Ready (MCP Support)


The exact same formation engine that powers your standard product can also power AI agents via the exact same API.


- **Action over Recommendations:** Our infrastructure is callable via the Model Context Protocol (MCP). This means an AI agent operating on platforms like Replit, Claude, ChatGPT, Lovable, Perplexity, or Vercel can actually execute a real company formation, rather than just suggesting it.


- **The Only Live Option:** doola is the only[live formation MCP](https://docs.doola.com/) on the market today. If you are building on AI surfaces, this gives you instant agentic reach. For everyone else, our robust REST API is the rock-solid foundation you build on.


### 4. Built for Non-US Founders, Natively


doola specializes in forming and operating US entities for international founders, the exact case most US-SMB formation APIs skip.


- **No SSN, no US residency:** Filings run across all 50 states for founders anywhere in the world.


- **ITIN and Form 5472 in-house:** ITIN procurement and Form 5472 filing for foreign-owned entities are handled internally, not handed to a disconnected third party, so you can serve a non-resident founder end-to-end.


- **The whole non-US market, addressable:** This is what turns international founders into a segment your product can actually convert, rather than one it has to turn away.


### 5. A Proven Engine, Now Infrastructure


This is not new software in new packaging.


- **Operating history:** doola has run this formation and compliance back-office for years across 175+ countries.


- **Backed by:** Y Combinator, Nexus Venture Partners, and HubSpot Ventures.


- **Diligence-ready:** For a platform betting its user experience on an external vendor, that operating history is a requirement, not a footnote.


## What doola Solves


Affiliate iFrame White-label API & MCP


Setup effort Share a link Embed a snippet API integration


Tech required None Basic embed Developer resources


UX control Low Medium Full


Best for Creators, communities Platforms & marketplaces High-volume / infra


Volume Any ~50+/mo 100+/mo


doola brings US company formation into your platform as an API you call, not a flow you hand off. Rather than routing users to a third-party provider and losing them mid-funnel, you integrate formation at the depth that matches your stage and volume:


> “We don’t force partners into a starting point. Your entry depends on your audience, product, and volume. Most partners start simple and scale as volume grows; there’s no penalty for starting at affiliate, and no ceiling on where the partnership can go.”
>
>
> — Arjun Mahadevan, Founder and CEO, doola


### Affiliate


The affiliate model is the easiest way to start. You share a trackable referral link and earn revenue share when users form through doola. There is no technical setup and no minimum-volume requirement.


**Best for:** creators, communities, SaaS platforms testing demand, and partners who want to validate formation interest before integrating.


### Embedded iFrame


The iFrame option lets you embed doola’s signup and onboarding flow inside your product with minimal engineering work. Users can start formation without fully leaving your product experience, while doola handles the filing, formation, and compliance operations in the background.


**Best for:** platforms with steady formation demand, marketplaces, partner products that want more control over the user journey, and teams not ready for full API integration.


### White-label API


The white-label API option gives your platform direct access to doola’s formation infrastructure. Your product owns the branded experience while doola powers the backend formation and compliance workflows.


**Best For:** high-volume platforms, fintechs and neobanks, AI agent platforms, commerce and creator platforms, and infrastructure teams that want a native product experience.


Underneath all three, doola runs the operational layer your platform would otherwise have to build and staff: entity formation, EIN, registered agent, ITIN, annual reports, bookkeeping, and federal tax filings.


You call the endpoints; doola handles state filings, IRS processing, and document generation, and pushes every status change and completed document back to you programmatically.


The result is that your user stays in your product and your brand owns the relationship, while doola powers the formation and compliance infrastructure underneath.


## Who This Is Built For


doola Formation API solves a major user drop-off problem across three distinct industries. Here is exactly how it impacts your specific platform:


### 1. Fintechs and Neobanks: The Entity Is the Prerequisite


Your entire account product, the cards, the USD rails, and the core banking features sit behind a rigid legal prerequisite that you currently cannot fulfill.


A user absolutely needs a legal entity and an EIN before they can activate a single feature on your platform.


Unfortunately,[most embedded formation tools](https://doola.com/wp-content/uploads/2023/11/doola-Getting-Started-Guide.pdf) either require a US SSN to proceed or they completely break when handling non-residents. This means your activation flow snaps at the worst possible moment: right when user intent is highest.


#### The doola Solution


We completely remove that blocker.[With a single EIN](https://www.doola.com/blog/simplifying-ein-for-non-us-residents-how-to-get-an-ein-as-a-non-us-resident/) API call, the entity is filed, and the EIN is processed. The EIN is then returned to your endpoint via signature-verified webhooks, no manual polling required. Your account product unblocks in real time.


Furthermore, all ITIN processing is handled in-house, so no user is ever handed off to a messy third-party workflow mid-onboarding. Every non-US user who previously churned at the activation step now has a seamless path to convert.


### 2. AI Builders and Agent Platforms: The Agent That Acts


Filing legal paperwork isn’t something an AI agent can natively execute today. The agent is trapped in an advisory role, forcing the user to leave and do the manual labor themselves. That is a massive gap between what your product promises and what it actually delivers.


#### The doola Solution


Our Model Context Protocol (MCP) completely solves this bottleneck. Formation becomes agent-callable, exactly like web search or code execution.


A user simply describes their business, the agent calls doola, and a real US company is established before the chat session even ends. There is no handoff and no external to-do list passed back to the user.


### 3. Commerce and Creator Platforms: Keep Sellers at the Threshold


A successful seller on your platform hits a verification threshold on Stripe or PayPal and suddenly needs to formalize their business to keep selling.


Today, hitting that threshold means the seller has to leave your platform, figure out how to set up a[US LLC as a non-resident on their own](https://www.doola.com/blog/how-to-set-up-an-e-commerce-llc-in-the-us-as-a-non-resident/) , and hopefully find their way back to you.


Most don’t. High-growth platforms see this exact demand constantly. High-intent, scaling sellers are lost at the formalization step simply because the next logical feature doesn’t exist inside your product UI.


#### The doola Solution


We keep that critical moment entirely inside your product, under your own brand. The seller formalizes without ever leaving your interface.


Post-formation, doola’s OAuth integration surfaces the ongoing compliance lifecycle, such as annual reports, bookkeeping, taxes, and[Form 5472 filings](https://www.irs.gov/forms-pubs/about-form-5472) directly inside your dashboard. Your team doesn’t manage any of it; it just runs smoothly in the background.


In short, **** the very formalization moment that used to push the user out becomes a powerful retention event. You instantly capture the revenue, boost customer lifetime value (LTV), and own the relationship through every compliance deadline that follows.


## How doola Is Different


Most formation APIs are built strictly for US residents with SSNs and stop at the initial filing. If your users are non-US residents or AI agents, those APIs only solve half the problem. They file the LLC, but then leave your user to navigate ITINs, Form 5472 tax filings, and annual reports on their own.


doola handles the entire lifecycle completely in-house.


### Core Infrastructure Features


- **No SSN or US Residency Required:** Unlike standard tools that rely on a US Social Security Number, this system handles filings across all 50 states for users anywhere in the world.


- **In-House ITIN Processing:** Instead of referring international founders to outside vendors or third-party services to get their Individual Taxpayer Identification Number, the process is handled natively.


- **Integrated Non-Resident Taxes:** It manages complex international tax requirements, like[Form 5472 and foreign-owned LLC federal filings](https://www.doola.com/blog/filing-requirements-for-foreign-owned-single-member-llcs-forms-5472-and-1120/) under one roof, bypassing external accountant networks.


- **Business Banking Setup:** Instead of pointing newly formed companies to external banks they then have to qualify with separately, US business banking is built into the formation flow and opened alongside the entity itself.


- **Native AI Agent Support:** Beyond standard REST APIs, it includes a live Model Context Protocol (MCP). This allows AI agents on platforms like Claude or Replit to actually[execute a company formation](https://www.doola.com/blog/doola-mcp-form-your-llc-inside-ai-chat/) rather than just advising on it.


- **Downstream Compliance via OAuth:** Instead of a one-time transaction, it embeds ongoing requirements like bookkeeping, annual reports, and federal tax tracking directly into your platform’s dashboard.


- **Backed by Y Combinator, Nexus Venture Partners, and HubSpot Ventures:** This setup utilizes the same back-office system that has processed filings for 22,000+ companies across 175+ countries over the past five years.


## Formation Is the Start. Compliance Is What Keeps Them.


While setting up a legal entity is a great initial user milestone, the downstream compliance cycle is where long-term retention is won or lost.


Unfortunately, this infrastructure loop is incredibly easy to overlook, right up until it directly impacts your user retention and customer lifetime value.


Twelve months after a company forms inside your product, the US business system expects compliance. To maintain a valid corporate status, they are legally required to file:


- **State Annual Reports:** Mandatory filings required to avoid automatic administrative dissolution by the state of incorporation.


- **Form 5472 + Pro Forma Form 1120:** A strict IRS requirement for foreign-owned single-member LLCs that carries a severe **$25,000 minimum penalty** for late or inaccurate submissions.


- **Federal Income Tax Returns:** Year-end corporate tax compliance driven by structured, ongoing bookkeeping data.


If your interface lacks a clear compliance path, users have no choice but to drop out of your ecosystem. They will inevitably open Google, find an external accounting network, and connect their business bank accounts to a competitor’s dashboard.


### Bridging the Gap via OAuth


doola’s compliance stack closes that loop. Instead of passing users off to disconnected, third-party CPAs, the entire post-formation workflow remains embedded directly inside your own software.


This architectural shift changes your product dynamics in two concrete ways:


- **Programmatic Reminders:** Compliance deadlines trigger automated, contextual events inside your product UI, prompted entirely by doola’s operational backend.


- **Native Financial Data Pipelines:** Transaction and bookkeeping data feeds directly into tax filing engines, allowing users to fulfill IRS demands without ever leaving your interface.


Ultimately, keeping this transaction under your own brand converts what is typically a major churn risk into a predictable, recurring revenue stream. When you look at the mechanics, the initial company formation is merely a one-time acquisition event.


The ongoing compliance lifecycle is the real engine for sustainable growth.


## Start Building Today


[Check out our API docs today.](https://docs.doola.com/api/introduction) You can curl endpoints immediately with dk_test_ keys and download the OpenAPI spec, no signup walls, no intro calls, and no contracts required to push your first payload. A developer can evaluate and build before anyone from doola talks to them.


- **AI Agent Stack:** If you are building for AI environments, the formation MCP is already live in production across Replit, Claude, ChatGPT, Lovable, Perplexity, and Vercel.


- **Time to Deploy:** Because the endpoints are ready out of the box, most platforms[map the API and deploy to production in as little as 48 hours](https://docs.doola.com/api/introduction) .


When you’re ready to move past the sandbox and discuss a production partnership,[reach out to us →](https://doola.com/partners)


Every platform runs on infrastructure it didn’t build. This one is yours!


Add Company Formation to Your Platform *in Days.*


[Become a Partner](https://www.doola.com/formation-api)


#### Share this article


## Table of Contents


What You’re Building Ondoola: The Company Layer of the InternetWhat doola SolvesWho This Is Built ForHow doola Is DifferentFormation Is the Start. Compliance Is What Keeps Them.Start Building Today


### How Businesses Stay Compliant.


[Get Started for Free](https://www.doola.com/start-business/)
