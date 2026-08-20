---
schema_version: "1.0.0"
document_id: "53e8036c49ce8c0596cd30d2fc979b8fe9bd0e27f1b16d633fc39618556046c1"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/replace-pentests-with-automated-evidence/"
published_at: "2026-06-19T13:22:19+00:00"
first_seen_at: "2026-07-20T23:23:55.281376+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:4f7812b29eb2d62ab5c00d395339ec087a0e9df2179ebb5cfe14af36ea0d9db9"
---

# How Escape helped Bridgetech Group replace pentests with automated evidence

Bridgetech Group is a UK-based B2B SaaS company, serving enterprise customers who routinely demand security evidence as part of their procurement process. The security function is led by Ben Dalby, Chief Delivery Officer, working alongside the CTO with an engineering team spread across three product squads.


## Problem


Bridgetech needed dependable, authenticated DAST coverage for both applications and APIs, but every option on the table failed on at least one of four dimensions: cost, authentication reliability, developer usability, or API support.


**Four concrete pain points before Escape:**


- **Cost ruled out several possible solutions.** With a limited budget per year, manual pentests, which customers kept asking for, ran into tens of thousands of pounds per engagement.
- **Authenticated scans often failed.** Ben had previously evaluated scanners that failed to log in, never logged the failure, and then returned a wall of findings against the public login page. This was not efficient for his security program and worse for compliance evidence.
- **A dashboard built for pentesters, not developers.** Bridgetech's incumbent setup ran another vendor scanner. Cheap, but the dashboard hid the granular detail. It wasn’t suitable for developers and couldn’t be easily reviewed and added into their ops workflows, so the security signal stalled at Ben's desk.
- API scanning was a gap. The previous-generation tools handled apps, but not modern API surfaces, and Bridgetech exposes APIs alongside every product.


Layered on top of all of this: ISO 27001 obligations, weekly-scan commitments to customers, and a small team that could not afford a heavy security review process.


## Solution


Bridgetech adopted Escape for combined application and API scanning with the freedom to mix and match between DAST and API scanning under a single allowance.


What made Escape the right fit:


- **Authenticated scanning that actually works.** OAuth2 logins, static IP addresses for firewall allowlisting, and clear scan logs that prove what was crawled
- **Customer-ready reporting in hours and without breaking the bank** . Branded, summary-and-detail format that customers accept as evidence of pentesting, without commissioning a manual engagement. And Ben describes the economics directly:


> *“* So it saves us money from a pentest point of view. The other thing is it's very quick. So if you need to do a manual pentest, then you have to. Identify a partner, a supplier, you need to give them passwords and accounts. They then have to go through the process. So it can take weeks and weeks and weeks. So as well as the financial side, you've also got the time sign. *”* — Ben Dalby, Chief Delivery Officer, Bridgetech Group


- **Granular detail surfaced in a developer-friendly UI** . Unlike the pentester dashboard Bridgetech was previously stuck with, Escape exposes the debug details developers actually need to act that are added to their corresponding tickets into Azure DevOps
- **Unified app + API coverage** . One platform, one budget envelope, and the flexibility to redistribute scans across products as the surface evolves.


Ben on the evaluation experience:


> *“* The authentication experience, the way it handles authentication, is markedly better than what I've experienced in other products. So that's something you're doing that's definitely making my life easier. My expectation is that trying to get a security scanner to log into an application is like going to a dentist. But with Escape, it was really easy. So that's a really good thing *.”* — Ben Dalby, Chief Delivery Officer, Bridgetech Group


## Impact


Months in, Escape has become a working part of Bridgetech's secure development process. The impact shows up across the following dimensions: customer evidence, security posture, and team workflow.


> *“At a very high level the value in Escape for my business is two things: So firstly, it makes our customer-facing platforms more secure, by identifying issues that we can within our power to fix and close, so basically, where those are to address weaknesses before potential hackers or whoever finds them. So that's one thing.*


> *The other thing is it allows us to demonstrate that we're diligent to existing and new customers. So, if customers will often ask for a penetration test against our platforms, and some customers will accept the output of tools like Escape. That is massively beneficial for us.”* — Ben Dalby, Chief Delivery Officer, Bridgetech Group


### 1. Pentests replaced by Escape reports at a fraction of the cost.


Bridgetech's enterprise customers routinely ask for pentest evidence. Many now accept Escape's automated reports in lieu of a manual engagement, Beyond raw cost: manual pen tests take weeks — finding a partner, provisioning accounts, scheduling the work — which can delay deals and delay revenue. Escape reports are available on demand, which compresses the security gate in customer onboarding from weeks to the same day.


### 2. Customer audits handled in minutes, not weeks.


When an enterprise customer requests evidence, Bridgetech can pull a current Escape report immediately rather than starting a procurement loop with a pentesting firm. Ben’s team keeps the open-issue backlog short on purpose. Customer disclosure means a long list of unresolved mediums and highs becomes a procurement blocker, so closing findings has direct revenue consequence. Escape gives the team the running list, the team works it down each month, and the discipline holds.


### 3. A secure development rhythm that developers actually go along with.


Each of Bridgetech's three development teams now holds a monthly secure development call. Ben walks the team through pkg, container, and DAST findings (with Escape providing the DAST layer) and tickets are created directly onto each team's Azure DevOps board. Developers are working on Escape findings inside their normal sprints.


> "Based on stuff that's in DAST, you can see one of my developers is working on this. And my developers putting effort into removing this vulnerability. You can see my team's using the platform every month. On a regular basis. So we don't just have the platform as a box-ticking exercise; we're actually using it to drive security into our platforms to make them more secure."
