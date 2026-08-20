---
schema_version: "1.0.0"
document_id: "3786f7b8e767a2b4ab37fa04dda42a84e41d4f79f13c7b2cf9290198690fd8b6"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/agentmail-outposts-byoc"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T01:29:01.305300+00:00"
fetched_at: "2026-07-30T01:29:02.568995+00:00"
content_hash: "sha256:269f993aa5b702db520e830311c49c1c55cec34cb4c8127a5010332c75a166ff"
---

# Introducing AgentMail Outposts: Bring Your Own Cloud for agent email

TL;DR


- **What:** AgentMail Outposts is our Bring Your Own Cloud (BYOC) offering. It runs the part of AgentMail that touches email (sending, receiving, storage, and search) inside *your* AWS account instead of ours. AWS is supported today, with more clouds coming soon.
- **Why it matters:** For a lot of enterprises, encrypting email in a vendor's cloud isn't enough. The data can't enter the vendor's cloud in the first place. Outposts make that possible without you having to self-host and operate AgentMail yourself.
- **How it works:** Email content (bodies, subjects, attachments, drafts, raw messages, search indexes) stays in your account. AgentMail Cloud still runs the dashboard, SDKs, auth, billing, and upgrades. Same API, one different` baseUrl` .


## What is BYOC?


Bring Your Own Cloud (BYOC) means running a vendor's software inside infrastructure *you* own, so your data stays in your environment instead of theirs. You get the product, but the sensitive data never leaves your account.


AgentMail Outposts is our BYOC offering. It runs the parts of AgentMail that handle email (sending, receiving, storage, and search) inside your own AWS account. Your email content stays with you, and you keep the same AgentMail API, SDKs, and dashboard on top of it.


## Why we built it


Email is becoming a core interface for AI agents. It's also one of the most sensitive datasets an agent ever touches. An inbox holds customer conversations, contracts, financial documents, authentication links, and private attachments, the material a security team loses sleep over.


Most infrastructure asks you to trust that a vendor encrypts that data well and handles it responsibly in the vendor's own cloud. For many teams that's a fine trade. For a growing set of enterprises, it isn't. Their requirement isn't "encrypt our email in your cloud." It's "our email never enters your cloud at all."


We kept meeting teams who wanted AgentMail and couldn't use it, not because of the product, but because of where the data would live. Serious companies with real security review processes came to us ready to build, and hit the same wall: sensitive email data cannot leave infrastructure they control.


The usual answer to that wall is "self-host it." Ship the customer a copy of the app and let their team run it. That solves data ownership and creates a new problem: now someone has to operate, patch, monitor, and upgrade a complex email system forever. Most teams don't want a fork of your product. They want your product, with the data in their account.


Outposts is our answer to both at once: the ownership and security posture of BYOC, with the operational simplicity of a managed service.


## What is an Outpost?


An **Outpost** is a customer-hosted AgentMail service that sends, receives, stores, and searches email inside your AWS account.


It implements the same AgentMail API as our hosted service. The sensitive parts of the AgentMail dashboard and SDK talk directly to your Outpost, while AgentMail Cloud continues to manage accounts, organizations, permissions, billing, and the overall product experience.


That split creates a clean boundary:


- **Your Outpost handles email content.** Message bodies, subjects, attachments, drafts, raw messages, and search indexes stay in your AWS account and never touch ours.
- **AgentMail Cloud runs the product.** Your team keeps the dashboard, SDKs, APIs, authentication, and managed lifecycle it already knows.


The result is customer-owned email data without a separate, second-class edition of AgentMail to maintain.


## The same API, in your cloud


Adopting an Outpost doesn't change how you build. No second SDK, no new programming model, no "enterprise fork" with different semantics. You point the AgentMail client at your Outpost endpoint and keep using the same API you already wrote against.


```text
const agentmail = new AgentMailClient({
apiKey: process.env.AGENTMAIL_API_KEY,
baseUrl: "https://<outpost-id>.byoc.agentmail.to",
})


const thread = await agentmail.threads.get("thread_123")
```


The dashboard behaves the same way. When a user opens a message or searches an inbox, the sensitive content is fetched directly from your Outpost rather than routed through AgentMail's hosted infrastructure. Same experience on the surface, a completely different data path underneath.


## How deep this goes


BYOC is easy to say and hard to do well. The engineering that makes it work is where the real depth is.


**Guided deployment.** An administrator picks an AWS region in the AgentMail dashboard and launches a CloudFormation stack. That's the setup, with no infrastructure to assemble by hand.


**Least-privilege by construction.** The stack creates the dedicated resources and IAM roles the Outpost needs, scoped to the Outpost itself, not to unrelated infrastructure in your account. It gets exactly what it needs and nothing more.


**Your account, your controls.** The resources live in your AWS account and inherit the controls you already run: IAM, audit logging, retention policies, regional requirements, security monitoring, and cost controls. Nothing about data residency or compliance is something you have to take on faith. It's enforced by your own cloud.


**We still maintain the software.** New versions, security fixes, and product improvements are built, tested, and rolled out automatically. You don't maintain a fork, schedule upgrades, or operate the full AgentMail application. **You own the infrastructure and the data; AgentMail operates the product.**


## Who this is for


Outposts are an[Enterprise-tier](https://www.agentmail.to/enterprise) offering for teams that need email data to stay in infrastructure they own. We support AWS today, with more clouds coming soon. If any of these describe you, this is built for you:


- You have data-residency requirements, whether regulatory, contractual, or regional, that a[vendor cloud](https://www.agentmail.to/legal/subprocessors) can't satisfy.
- Your[security review](https://www.agentmail.to/legal/soc2) won't approve sensitive customer data leaving your environment.
- You operate under security or regulatory requirements where "the vendor encrypts it" isn't an acceptable answer.
- You want AgentMail's agent-email capabilities but need them behind your own security perimeter.


If you're an individual developer or an early-stage team, our hosted service is still the fastest way to give an agent an inbox. Outposts is for when data governance becomes the constraint.


## What this unlocks for you


With an Outpost, you don't have to choose between owning your data and running a managed product. You get both. Sensitive email stays in your account under your controls, and you keep the same API, dashboard, and automatic upgrades you'd get from the hosted service.


Concretely, that means:


- Sensitive email data that never leaves your AWS account, so it clears data-residency and security review instead of stalling in it.
- No operational burden. No fork to run, no upgrades to schedule, no email system for your team to babysit.
- No rebuild. The same code, SDK, and API you'd use on our hosted service, with one different` baseUrl` .


We took on the hard part: deploying a real email system into someone else's cloud and keeping it patched, secure, and current without your team lifting a finger.


## Summary


AgentMail Outposts brings the ownership and security of Bring Your Own Cloud together with the simplicity of a managed service. The part of AgentMail that touches email runs inside your AWS account, so sensitive email content never enters ours, while your team keeps the same dashboard, SDKs, API, and automatic upgrades. You own the infrastructure and the data; we operate the product.


For enterprises that couldn't previously let email data leave their environment, that's the difference between "we'd love to, but we can't" and "let's build."


**AgentMail Outposts are available to Enterprise-tier customers on AWS today, with support for more clouds coming soon.[Book a call to get started](https://cal.com/haakam/30min) .**
