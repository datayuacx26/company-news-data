---
schema_version: "1.0.0"
document_id: "da77c0cbc3dfb0f42afc0da8abcc55d2008f465c99f979b13cf701c43c82d405"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/chatgpt-enterprise-pricing"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-22T21:28:59.137691+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:4d12d61204d7639523153acb5d2389d045c8cfda14d3ae66c35e0cf2887586ce"
---

# ChatGPT Enterprise Pricing Guide (2026)

[← All posts](https://www.beam.cloud/blog) Engineering


# ChatGPT Enterprise Pricing Guide (2026)


Nathanael Chiang


June 25, 2026


7 min read


OpenAI's plan lineup got more crowded in 2026, and the pricing that matters most, ChatGPT Enterprise, is the one number OpenAI never publishes. This guide lays out the full ladder from Free to Enterprise, separates the figures OpenAI states officially from the ones only reported by buyers and analysts, and clears up two things people routinely get wrong: what ChatGPT Go actually is, and what OpenAI Frontier actually is.


## The short version


- The current flagship model is **GPT-5.5** , released April 23, 2026. GPT-5.5 Instant has been the default model for all users since May 5, 2026.
- **ChatGPT Enterprise pricing is not published.** Every contract is sales-negotiated. Reported 2026 figures cluster around **$60/user/month** , a **150-seat minimum** , and an **annual prepaid commitment** , which works out to a roughly **$108,000/year floor** . Treat all of these as reported, not official.
- The full ladder runs **Free → Go ($8/mo) → Plus ($20/mo) → Pro ($100 or $200/mo) → Business ($20 annual / $25 monthly) → Enterprise (custom)** .
- **ChatGPT Go is a consumer plan for individuals (~$8/month)** , not a mid-market business tier.
- **OpenAI Frontier is a separate agentic-AI platform** , sold alongside Enterprise, not a higher tier of the chat product.
- Nonprofits get **up to 75% off** , with eligibility validated through **Goodstack** .


## The full list of ChatGPT SKUs


Plan Price Who it's for Notes


Free $0 (ad-supported in US) Individuals GPT-5.5-era access, limited message volume


Go ~$8/month Individuals Ad-supported in US; ~10x Free's limits


Plus $20/month Individuals Monthly only; full feature set


Pro $100/mo or $200/mo Power users Same models, different usage volume


Business $20/seat annual ($25 monthly) Teams (2+ seats) Renamed from "Team"; admin controls


Enterprise Custom (~$60/seat reported) Large orgs ~150-seat min, annual prepaid


Sold separately: **OpenAI Frontier** (an agentic-AI platform, custom pricing) and the pay-per-token **API** .


### Free and Go are both individual plans


OpenAI's pricing FAQ is explicit: "Go, Free, and Plus plans are designed to be used by individuals. Business and Enterprise are for businesses." ChatGPT Go launched in India in 2025 at roughly $4–5 and rolled out globally in January 2026 at **$8/month** . It runs on GPT-5.5-era models with about 10x the message limits of Free, is ad-supported in the US, and omits enterprise features. It is not a 10–149 seat business tier, that plan does not exist.


### Plus — $20/month


ChatGPT Plus has held at **$20/month** since February 2023, monthly only, with no annual option. It includes GPT-5.5 access, Deep Research, Sora, Codex, and Agent Mode.


### Pro — now $100 and $200


There are two Pro tiers as of 2026. The **$200/month** tier is the original: roughly 20x Plus usage limits, a ~1M-token context, and 250 Deep Research runs per month. A **$100/month** tier launched April 9, 2026, with about 5x Plus limits, positioned against Anthropic's $100 Claude Max. Both share the same model access, including GPT-5.5 Pro; the difference is usage volume, not capability.


### Business — $20 annual / $25 monthly


ChatGPT Team was **renamed to ChatGPT Business on August 29, 2025** . On April 2, 2026, OpenAI cut the standard seat by $5, so Business now runs **$20/user/month on an annual plan or $25/user/month monthly** , with a **2-seat minimum** . Self-serve Business tops out around 1,000 members; OpenAI suggests Enterprise above roughly 250. One limitation worth knowing: **Business does not include a HIPAA BAA** , that requires Enterprise.


## ChatGPT Enterprise: what it costs (reported)


OpenAI publishes no Enterprise list price. The official pricing page only says "Contact our sales team." Everything below comes from procurement disclosures and analyst sources, which agree closely but are not authoritative.


- Procurement sources put most contracts around **$60/user/month** , within a **$45–$75** range, on a **150-seat minimum** and a **mandatory annual commitment** , which sets a floor near **$108,000/year** (150 × $60 × 12).
- Advisory sources add scale nuance: negotiated 2026 deals land between **$50 and $60/user/month** at 150-plus seats, fall toward **$40 at 5,000-plus seats** , and rise **above $60** for short terms or small seat counts.


Read these as reported, estimated, and negotiated figures. None are OpenAI list prices, and your number will depend on seat count, term length, and what you negotiate.


On usage, Enterprise gives "virtually unlimited" GPT-5.5 Instant messages subject to abuse guardrails, with advanced features funded from a shared credit pool purchased at the contract level rather than per-seat caps. As of April 2, 2026, OpenAI added usage-based pricing for advanced features and a usage-based Codex seat type for Business and Enterprise.


## ChatGPT Enterprise: security and compliance


These are confirmed on OpenAI's official business-data and enterprise-privacy pages:


- **No training on your data by default** for Business, Enterprise, Edu, Healthcare, and the API.
- **SOC 2 Type 2** , plus **ISO/IEC 27001, 27017, 27018, 27701** , and **CSA STAR** certifications.
- **HIPAA BAA** available for ChatGPT Enterprise, ChatGPT for Healthcare, and the API (not for Business).
- **SSO/SAML, SCIM, RBAC, MFA, domain verification, and IP allowlisting.**
- **Audit logs and a Compliance API** , now part of OpenAI's Compliance Logs Platform (admin audit, user authentication, and Codex usage logs).
- **Data residency** for content at rest in 10 regions: US, Europe, UK, Japan, Canada, South Korea, Singapore, Australia, India, and the UAE, with optional in-region GPU inference in the US or Europe.
- **Enterprise Key Management (EKM)** for customer-controlled encryption keys.
- Encryption is AES-256 at rest and TLS 1.2+ in transit. Enterprise context windows are 128K for GPT-5.5 Instant and 196K for GPT-5.5 Thinking.


## OpenAI Frontier is not a ChatGPT tier


This is the most common mix-up. **OpenAI Frontier** , launched February 5, 2026, is a separate platform for building, deploying, and managing AI agents ("AI coworkers") with shared business context, agent execution, identity and permissions, and governance. It is built on open standards and can manage agents from OpenAI and third parties. It is not a higher tier of the ChatGPT chat product.


OpenAI named HP, Intuit, Oracle, State Farm, Thermo Fisher, and Uber among the first adopters, and said dozens of existing customers, including BBVA, Cisco, and T-Mobile, had piloted the approach. Pricing is custom and sales-led; OpenAI declined to discuss it at launch. A follow-on "Frontier Alliances" program with McKinsey, BCG, Accenture, and Capgemini was announced February 23, 2026. If you see Frontier described as "ChatGPT Enterprise plus," that's wrong, it's a distinct agentic-AI product.


## The model lineup behind the plans


As of late June 2026, the family is the **GPT-5.5 era** :


- **GPT-5.5 Instant** is the default for all users (since May 5, 2026; exposed in the API as` chat-latest` ).
- **GPT-5.5 Thinking** is available on Plus, Pro, Business, and Enterprise.
- **GPT-5.5 Pro** is available on Pro, Business, Enterprise, and Edu.


GPT-4o was retired from ChatGPT on February 13, 2026 (Custom GPT access for Business, Enterprise, and Edu ran until April 3, 2026) and remains available through the API. The retirement cadence has been brisk: GPT-5 retired February 13, GPT-5.1 retired March 11, and GPT-5.2 retired June 12, 2026.


On the API, **gpt-5.5 costs $5 per million input tokens and $30 per million output** ; **gpt-5.5-pro is $30 / $180** .


## Nonprofit discounts


OpenAI offers nonprofits **up to 75% off** ChatGPT Business or Enterprise, confirmed on its pricing pages and the February 6, 2026 "OpenAI for Nonprofits" update. Eligibility is validated through **Goodstack** (OpenAI's stated partner), not TechSoup, despite some third-party guides claiming otherwise.


The "up to 75%" figure is the negotiated maximum, applying mainly to larger, sales-led, and Enterprise deals. For self-serve Business, verified nonprofits pay about **$8/user/month annual or $10/user/month monthly** (roughly 20% off). OpenAI does not officially state whether the 150-seat Enterprise minimum is waived for nonprofits, so treat any "nonprofit Enterprise minimum" figure as an estimate.


## How it compares: Microsoft, Google, Anthropic


Product Price Structure


Microsoft 365 Copilot $30/user/month add-on On top of a qualifying M365 base license


M365 Copilot Business (SMB) $21/user/month ($18 promo through Dec 31, 2026) Orgs under 300 users


Google Gemini for Workspace Bundled (no add-on) Included in Workspace plans (~$7–22/seat)


Anthropic Claude Team $25/seat monthly ($20 annual) 5-seat minimum


Claude Team Premium $125/seat monthly ($100 annual) Higher usage


Claude Enterprise Custom ~$20/seat base plus usage-based billing in 2026


A few details that trip people up. Microsoft's $30 Copilot figure is an **add-on** , so true cost is the base M365 license plus $30; enterprise volume discounts on that add-on expire June 30, 2026. Google **discontinued the separate Gemini add-on in 2025** and now bundles Gemini into Workspace at no extra per-seat charge, with Business tiers running roughly $7–22/seat. Anthropic's Claude Team Standard is **$25/month** now (the older $30 figure is dated), and its Enterprise tier shifted to a ~$20/seat base plus usage-based token billing in 2026.


## FAQ


**How much does ChatGPT Enterprise cost?** OpenAI doesn't publish a price. Reported 2026 contracts average around $60/user/month within a $45–75 range, on a 150-seat minimum and annual prepaid term, putting the floor near $108,000/year. Large deals (5,000+ seats) can fall toward $40/seat; small or short-term deals run higher.


**What's the difference between ChatGPT Business and Enterprise?** Business is self-serve at $20/seat annual ($25 monthly) with a 2-seat minimum and admin controls. Enterprise is sales-negotiated, adds a HIPAA BAA, EKM, data residency, the Compliance API, and higher usage, and carries a ~150-seat minimum.


**Is ChatGPT Go a business plan?** No. Go is a ~$8/month individual consumer plan, ad-supported in the US, with about 10x Free's limits. Business and Enterprise are the team plans.


**What is OpenAI Frontier?** A separate platform (launched February 5, 2026) for building, deploying, and governing AI agents. It's sold alongside Enterprise with custom pricing, not as a higher chat tier.


**What's the current ChatGPT model?** GPT-5.5, released April 23, 2026. GPT-5.5 Instant is the default for everyone; Thinking and Pro variants are available on paid tiers.


**Do nonprofits really get 75% off?** Up to 75% is the negotiated maximum, mostly for larger and Enterprise deals. Self-serve Business for verified nonprofits is about $8/seat annual ($10 monthly). Eligibility runs through Goodstack.


## Before you buy


OpenAI's model names and prices have changed roughly monthly through 2026, and Enterprise pricing in particular is only knowable by talking to sales. Use the figures here to set expectations, then verify against[openai.com/chatgpt/pricing](https://openai.com/chatgpt/pricing) and a live sales quote before you commit. If OpenAI ever publishes an official Enterprise list price, the "reported" hedges throughout this guide can come off, until then, treat per-seat Enterprise numbers as informed estimates.


Nathanael Chiang


Published June 25, 2026


Keep Reading


## More from the Beam blog


[Engineering Tinker Model Pricing: What Fine-Tuning Costs in 2026 See what fine-tuning costs on Tinker, including worked cost examples and where renting GPUs gets cheaper. Tim Huynh](https://www.beam.cloud/blog/tinker-model-pricing)[Engineering What Is a Container, Really? Five Years of GPU Infrastructure Five years of GPU infrastructure at Beam — from ECS and Knative cold starts to a custom container runtime, FUSE lazy-loading, and a trustless binary. Luke Lombardi](https://www.beam.cloud/blog/what-is-a-container-really)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
