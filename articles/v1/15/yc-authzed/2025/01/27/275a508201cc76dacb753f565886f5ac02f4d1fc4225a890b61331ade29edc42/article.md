---
schema_version: "1.0.0"
document_id: "275a508201cc76dacb753f565886f5ac02f4d1fc4225a890b61331ade29edc42"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/2024-soc2-reflection"
published_at: "2025-01-07T20:20:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:4558aee456c5567b3d9a5708c59aec959e1187cf37e7b9bf7ec303e6cda7c98f"
---

# Our SOC2 Renewal and Reflections on Compliance

I'm happy to announce that AuthZed recently renewed our SOC2 compliance and our SOC2 Type 2 and SOC3 reports are now available on[security.authzed.com](https://security.authzed.com/) .


Having just endured the audit process again, I figured it would be a good time to reflect on my personal feelings toward compliance and how my opinion has evolved.


## An unbiased description of SOC2


If you're reading this now and aren't familiar with SOC2 and SOC3, I'll give you an overview by someone that isn't trying to sell you a compliance tool (feel free to skip this section):


SOC (System and Organization Controls) is a suite of annual reports that result from conducting an audit of the internal controls that you use to guarantee security practices at your company. An example of an "internal control" is a company-wide policy that enforces that "all employees have an anti-virus installed on their devices". Controls vary greatly and can be automated by using software like password managers and MDM solutions, but some will always require human intervention, such as performing quarterly security reviews and annual employee performance reviews.


In the tech industry, SOC2 is the standard customers expect (or ISO27001 if you're in the EU, but they are similar enough that you often only need either one). As I wrote this, it came to my attention that I have no idea what SOC1 is, so I looked it up to discover that it is apparently a financial report which I've never heard of customers requesting in the tech industry. SOC3 is a summary of a SOC2 report that contains less detail and is designed to be more publicly sharable so that you don't necessarily need to sign an NDA to get some details. SOC2 comes in two variants "Type 1" and "Type 2". It's fairly confusing, but this is just shorthand for how long the audit period was. Type 1 means that the audit looked at the company at one point in time, while Type 2 means that the auditor actually monitored the company over a period of time usually 6 or 12 months.


## What engineers think about SOC2 Compliance


To engineering organizations, compliance is often seen as a nuisance or a distraction from shipping code that moves the needle for actual security issues. Software engineers are those deepest in the weeds, so they have the code that they're familiar with at the top of mind when you ask where security concerns lie. Because I knew where the bodies were buried when I first transitioned my career to product management from engineering, I always tried to push back and shield my team from having to deliver compliance features. The team celebrated this as a win for focus, but we never got to fully understand the externalities of this approach.


Fast forward a few years, I've now gotten *much* wider exposure to the rest of the business functions at a technology company. From the overarching view of an executive, the perspective of the software engineer seems quite amiss. If you asked an engineer what they're concerned about, it might be that they quickly used the defaults for bcrypt and didn't spend the time evaluating the ideal number of bcrypt rounds or alternative algorithms. This perspective is valuable, but can also be missing the forest for the trees; it's far easier to perform phishing attacks on a new hire than it is to reverse engineer the cryptography in their codebase. That simple fact makes it clear that if you haven't already addressed the foundational security processes at your business, it doesn't matter how secure the software you're building is.


## Compliance is ultimately about one thing: trust


All of that said, AuthZed's engineering-heavy team is not innocent from this line of thinking, especially since our core product is engineering security infrastructure. However, if we put our egos aside, there is one thing that reigns supreme regardless of the product you're building: the trust you build with your customers.


The compliance industry was never trying to hide that its end goal is purely trust in processes. SOC2 is defined by the American Institute of Certified Public *Accountants* and not a cybersecurity standards body; this is because compliance is about ensuring processes at your business and not finding remote code execution in your codebase. That doesn't mean that compliance cannot uncover deep code issues because SOC2 audits actually require you to perform an annual penetration test from an actual cybersecurity vendor. Coding vulnerabilities are only one aspect of the comprehensive approach that compliance is focused on.


Without compliance, our industry would be stuck having to blindly trust that vendors are following acceptable security practices. By conforming to the processes required for certifications like SOC2, we can build trust with our partners and customers as well as prove the maturity of our products and business. While it may feel like toil at times, it's a necessary evil to ensure consistency across our supply chains.


The final thought I'd like to leave you with is the idea that compliance isn't a checkbox to do business. It's a continuous process where you offer transparency to your customers to prove that they should trust you. I'm looking forward to seeing if my opinions change next renewal.


I'd like to thank the teams at SecureFrame and Modern Assurance who we've collaborated with during this last audit as well as all of the vendors and data subprocessors we rely on to operate our business everyday.


On this page


- An unbiased description of SOC2
- What engineers think about SOC2 Compliance
- Compliance is ultimately about one thing: trust


## Related


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)
