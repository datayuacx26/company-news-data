---
schema_version: "1.0.0"
document_id: "258599c426fd2aa9154bc71ab68dff67ed3903ee1fba2ed6f85299aa5fc90acc"
company_key: "yc-common-paper"
company: "Common Paper"
source_id: "yc-common-paper-rss-3b76f5ee637c"
canonical_url: "https://commonpaper.com/blog/contract-playbook-template/"
published_at: "2026-06-24T15:19:45+00:00"
first_seen_at: "2026-07-20T23:20:15.555416+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:d5b3b3ea44124893da7b10d886d20825c021ef7bd57eee84b081b933c33fa89c"
---

# What goes into a SaaS contract playbook

I’m the CEO of Common Paper and this is the third startup I’ve co-founded. I’ve looked at a lot of SaaS contracts. In this article I’m going to walk you through how a founder and CEO of a SaaS company would approach building a contract playbook. If you’re a CEO, operations leader, or deal desk owner at a startup, then this is for you.


I’ve[written previously about what a contract playbook actually is](https://commonpaper.com/blog/contract-playbook/) . Here I’m going to cover some of the commonly used rules from our sample playbook for a B2B SaaS company selling to other businesses: eight clauses, each with a default position, a fallback, an escalation trigger, and context on why this might matter to our imaginary business. Copy the table, swap in your own deal-size thresholds and names, and you have a working first draft.


These are example positions, not legal advice about what’s right for your company. You and/or your lawyer should customize this to fit the unique needs of your business.


Of course, once you have your positions built out, the next step is to give them to[our AI contract review agent, Gerri](https://commonpaper.com/blog/how-to-set-up-your-first-contract-review-playbook-with-gerri/) . It then reviews redlines on your paper or the customer’s own paper against your established playbook.


## How a playbook entry is structured


Every useful playbook entry has the same anatomy:


- **Description:** What it is
- **Your positions:** Any context on how you have handled this in the past or what you will and won’t accept. Be clear about what is your default position that you accept without discussion as well as any pre-approved concessions based on the customer ask or the conditions.
- **Reviewer:** The person who owns the decision in case any new issues require escalation


## The SaaS contract playbook template


A contract playbook can cover as many terms as you want. This contract playbook template covers the big ones and shows how you might structure each of your positions. All data should be adjusted to fit your unique business and requirements.


Clause Description Your positions Reviewer


**Limitation of liability** Caps how much one party can owe the other under the contract. A default cap covers most claims, with carve-outs that sit above it or aren’t capped. Default: total liability capped at 1x annual fees. Concessions: 2x annual fees above your deal-size threshold, or an increased cap limited to data and confidentiality claims. Your exact playbook should include the increased cap you will tolerate. CEO. Escalate: any unlimited ask, or a fixed cap above your insurance coverage.


**Indemnification** A promise to cover the other side’s costs for a specified third-party claim. Each party covers the claims tied to what it controls. Default: third-party claims only; you cover IP infringement about your product, the customer covers their content and use. Concessions: more third-party claim types; never first-party. Outside counsel. Escalate: a rewritten clause, first-party indemnity, or indemnification for regulatory fines.


**IP and AI training data** Who owns what. You keep the service and improvements; the customer keeps their data. Also covers whether you may train AI on customer data. Default: each side keeps what it owns; no training on customer data without express permission. Concessions: a written no-training commitment, with a carve-out for de-identified usage analytics. CEO. Escalate: any request for ownership or assignment of your product or your improvements.


**Data processing (DPA)** Governs how you handle the customer’s personal data: subprocessors, audit rights, breach notice, and how DPA claims relate to the liability cap. Default: your standard DPA; general subprocessor authorization with public list and advance notice; SOC 2 satisfies audit rights; breach notice without undue delay and within 72 hours; DPA claims sit under the main cap. Concessions: 30-day subprocessor notice with a termination right (not a veto) if unresolved; one audit per year at customer expense; 48-hour notice for confirmed incidents affecting their data. Security lead or outside counsel. Escalate: per-subprocessor consent, unlimited on-site audits, 24-hour or “suspected incident” triggers, or uncapped data liability.


**Payment terms** How long the customer has to pay after you invoice. Default: net 30. Concessions: net 45; net 60 above your deal-size threshold. All of this should be customized to fit your tolerance. CFO. Escalate: net 90 or beyond, unusual payment mechanics, or portal invoicing that delays the clock.


**Governing law and venue** Which state’s law governs the contract and which courts hear a dispute. Default: Delaware law, Delaware courts. Concessions: any US state other than Louisiana, as long as law and courts move together. Outside counsel. Escalate: non-US law or courts, or an arbitration requirement.


**Auto-renewal** Whether the subscription renews on its own, and how much notice is needed to stop it. Default: auto-renew with 30-day non-renewal notice. Concessions: remove auto-renewal, or extend notice to 60 days. CFO. Escalate: 90+ day notice windows, or disputes over a locked-in renewal price increase.


**SLA** The uptime or performance you commit to, and the credits you owe if you miss. Default: your standard SLA, no per-customer modifications. Concessions: strike the unilateral right to modify the SLA for enterprise deals, but keep commitments uniform. CTO or engineering lead. Escalate: uptime above what you actually run, credits beyond your cap, or liability carve-outs tied to SLA breach.


## Clause-by-clause notes


### Limitation of liability


One of the most negotiated legal terms in SaaS. The cap is the default limit that applies to most claims under the contract, with a few carve-outs that sit above it or aren’t capped at all. Moving it changes how much money is actually at stake in the deal, so it’s a risk decision rather than a drafting one. That’s why it escalates to the CEO and not to whoever is running the deal. For the full breakdown of cap structures, see the[limitation of liability explainer](https://commonpaper.com/blog/understanding-the-limitation-of-liability-clause/) .


### Indemnification


The default has each side cover the third-party claims related to IP issues under that side’s control, rather than both sides covering the same list. Customers expanding the covered claim types is a common ask, and for risk-tolerant companies, can be easy to approve. My escalation line is structural change. As a sales leader at an AI startup put it when setting up routing rules: “Someone totally rewriting the indemnity clause might be something you just want to send straight to \[counsel\], because you don’t want to mess with that internally.” First-party indemnity, where the customer wants you to cover claims they initiate rather than third-party claims, can turn the clause into uncapped exposure for customer-vendor disputes and is worth the outside counsel hour every time.


### AI training data


Two years ago this row barely existed, today it’s a standard in contracts. Of course, you’ll likely decide your training position before the negotiation rather than during it; the[AI Addendum](https://commonpaper.com/standards/ai-addendum/) is an open-source starting point. The non-negotiable in this row is ownership. A customer who wants to own improvements is asking for a piece of your product, which is a CEO conversation.


### Data processing


A lot of a DPA is set by regulation before either side negotiates. GDPR Article 28 spells out the core of a processor’s obligations, which is why teams rarely spend negotiation energy on that boilerplate. The terms that actually get negotiated are the five in the row above: subprocessor mechanics, audit rights, breach timing, liability, and deletion. Many companies hold the line in two places. The first is report-based audits, since a SOC 2 answers what an on-site visit would address without hosting one per customer. The second is the liability tie-back, where an uncapped DPA quietly undoes the cap you negotiated in the main agreement. These are the default positions in the Common Paper standard[Data Processing Agreement](https://commonpaper.com/standards/data-processing-agreement/) .


### Payment terms


For most companies, the default is net 30. For larger deals, companies might be willing to stretch to 45, or 60 as a concession. Net 90 and beyond starts to affect cash flow, and is often the place where a playbook will escalate to the CEO or CFO.


### Governing law and venue


The lowest-stakes row, which is precisely why it belongs in the playbook: the response here can clearly and safely be automated. Delaware is acceptable to most counterparties because most of them are Delaware corporations. Many companies will accept any state but Louisiana due to the differences in how contracts are interpreted. Governing law and venue is a place where a company will often have its preferred venue, but a wide range of acceptable alternatives.


### Auto-renewal


Auto-renewal shows up in many signed agreements, but it’s a common concession to remove it entirely. Removing it costs you a renewal conversation, not revenue, and customers who ask usually have a procurement policy behind the request. What does deserve attention is the renewal fee increase. That’s a pricing decision, so it routes to the sales manager.


### SLA


[39% of signed CSAs include an SLA](https://commonpaper.com/resources/contract-benchmark-2024-q1/) . The risk here is in accepting too many variations of SLAs which means your engineering team then has to track across hundreds of accounts. In most cases, best practice is to keep one SLA, concede the unilateral modification right when an enterprise deal demands it, and escalate anything that commits you to reliability you don’t actually run.


## Making the playbook run


Two things turn this table from a document into a system.


First, start with defining your actual positions for the terms in the template contract above. Then add rows when an exception repeats.


Second, execution. A playbook in a doc still depends on someone reading it at the right moment. This is what[Gerri](https://commonpaper.com/blog/how-to-set-up-your-first-contract-review-playbook-with-gerri/) automates: it applies your playbook to every incoming redline, accepts what’s inside your defaults, counters with your fallback, and routes anything past your escalation triggers to the named owner with the context to decide.


## FAQ


### Is there a contract playbook example I can copy?


Yes, the contract template above is a good starting point for a B2B SaaS company. Replace the deal-size thresholds with your own numbers and name your escalation owners, then have a lawyer review the positions.


### How many clauses does a contract playbook need?


Fewer than you’d think. It’s not uncommon for a small set of recurring topics to drive the majority of redlines, and the eight clauses here cover that set for most SaaS sellers. Add entries when a new exception shows up twice, not before.


### What’s the difference between a contract playbook and a contract template?


The template is the agreement you send, like a[Cloud Service Agreement](https://commonpaper.com/standards/cloud-service-agreement/) . The playbook is your set of positions for when someone marks it up. You need both, and they do different jobs.
