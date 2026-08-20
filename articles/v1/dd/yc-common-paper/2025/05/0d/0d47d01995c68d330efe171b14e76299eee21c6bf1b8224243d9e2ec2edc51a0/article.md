---
schema_version: "1.0.0"
document_id: "0d47d01995c68d330efe171b14e76299eee21c6bf1b8224243d9e2ec2edc51a0"
company_key: "yc-common-paper"
company: "Common Paper"
source_id: "yc-common-paper-rss-3b76f5ee637c"
canonical_url: "https://commonpaper.com/blog/contract-playbook/"
published_at: "2025-05-23T14:55:45+00:00"
first_seen_at: "2026-07-20T23:20:15.555416+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:b6d5241882ffa055be6b063569b88943952cdac93910ce0decfa6621576d3ae7"
---

# What is a contract playbook? (And what it isn’t)

A customer sends back your contract with 15 redlines at 4pm on the last day of the month. Someone has to decide what to accept and what to push back on. At most growing software companies, you won’t yet have an in-house attorney, so that someone ends up being a founder, a RevOps lead, or whoever happens to be closest to the deal. They make the call from memory, the deal closes on the 6th day of the next month, and the same decisions come up again next month.


That is the problem a contract playbook solves.


## What is a contract playbook?


A contract playbook is the rule book for exceptions. It documents which redlines you accept, what you counter with when a customer pushes, and who makes the call when a request falls outside both. The playbook exists for everything that deviates from your standard agreements.


> I prefer having the playbook as a rule book for exceptions, because we already have our main document. I can extend our 30-day payment window to 60 days for anyone above a certain revenue threshold.
>
>
> Operations leader, 300-person services company


That’s a complete playbook entry in one sentence. A default (net 30), a fallback (net 60), and a condition for using it (deal size). Nobody needs to ask him. The rule answers the question before it gets asked.


## What a contract playbook isn’t


It isn’t a template library. Templates are your starting documents: a[Cloud Service Agreement](http://google.com/search?q=common+paper+cloud+service+agreement&oq=common+paper+cloud+serv&gs_lcrp=EgZjaHJvbWUqCQgAEEUYOxiABDIJCAAQRRg7GIAEMgYIARBFGDkyBwgCEAAY7wUyCggDEAAYgAQYogQyBwgEEAAY7wUyBwgFEAAY7wXSAQgzOTI1ajBqN6gCALACAA&sourceid=chrome&ie=UTF-8) , a[Mutual NDA](https://commonpaper.com/standards/mutual-nda/) , the order form your sales team sends. The playbook doesn’t store contracts. It stores decisions about what happens when someone marks them up.


It isn’t a clause-by-clause encyclopedia. You don’t need a documented position on all 40 pages of your agreement, because customers don’t negotiate all 40 pages. Across 1,000+ companies in the[2024 Contract Benchmark Report](https://commonpaper.com/resources/contract-benchmark-2024-q1/) , the most negotiated terms were fee structure, renewal notice, and invoice timing. A general counsel at a growth-stage company might see a handful of topics comprising 70% of the redlines she sees. These are the topics worth documenting first.


It isn’t necessarily a legal-department manual. Most B2B SaaS companies think about hiring in-house counsel around $10–$20m or 100 employees. The playbook helps make sure that contracts get handled competently before that hire, and will certainly make in-house counsel more productive once you have them on the team.


Here’s the reality, if you’ve negotiated more than a handful of contracts, you effectively already have a playbook. You know which redlines you wave through and which ones need a second opinion. The problem is that it lives in your head, where nobody else can see it. Writing it down is not creating something new. It’s making something that already exists usable by other people.


## Who owns the playbook?


Whoever triages contracts (the founder, the RevOps lead, the operations manager who became the contracts person by accident) is the natural owner of the contract playbook. They already know what the rules are. They’ve just been applying them from memory.


You don’t need a lawyer to start a playbook, but you may want your lawyer to validate, refine, and extend it. Outside counsel is far more useful in helping to define your documented positions than re-deriving and applying them on every deal. The playbook moves their time to judgment calls, which is what you actually want to pay them for. The playbook allows you to scale legal expertise.


## What goes into a contract playbook


Every entry has four parts:


- Description: What it is
- Your positions: Any context on how you have handled this in the past or what you will and won’t accept
- Reviewer: The person who owns the decision in case any new issues require escalation


Here is what a complete entry might look like for one of the most negotiated legal terms in SaaS, the limitation of liability. Our imaginary company is an analytics company:


### Limitation of liability


**Description:** The clause that caps how much one party owes the other in the event of a legal dispute over the contract. It sets a default cap that covers most claims, and may include a short list of carve-outs that sit above the cap (a supercap) or aren’t capped at all.


**Your positions:** Start with the worst thing that happens if our product fails. We don’t touch the customer’s funds or hold their regulated data, and we sit outside their live transaction path. If we have an outage or a bug, the realistic loss is the value they got from the tool, not a catastrophic event. A cap at the fees paid over the prior year already covers that. An unlimited cap, or a cap many times the contract value, is priced for a risk our product doesn’t create. We hold the cap at fees paid, carve out the few claims that genuinely warrant more, such as a confidentiality or security breach, and when a customer pushes, we walk them through what our product can and can’t put at risk.


**Reviewer:** General Counsel, or the founder at a company without in-house legal.


## When should you build a contract playbook?


A few reliable triggers:


- Volume. You’re seeing five or more redlined contracts a month.
- Bottleneck. Deals wait on one person and that person can’t delegate because the rules exist only in their head.
- Spend. Outside counsel is billing you to apply the same decision over and over.
- Turnover. The person who knows the rules is leaving, or you’re about to hire someone who needs to learn them.


## How to build your first playbook


Skip the committee. A useful first playbook is an afternoon of work.


1. Write the default and the fallback for each recurring topic. Payment terms, liability cap, renewal notice, governing law. Plain language is fine. “We accept net 45, we offer net 60 above $50K, we don’t do net 90” is a complete playbook rule.
2. Name the escalation owners. Payment terms to the CFO. Liability to the CEO. A rewritten indemnification clause to outside counsel. The owner is the person whose risk it is, which is usually not whoever is running the deal.
3. Run it against the next contract and revise. A playbook that covers 70% of what you see is working on day one. It’s a living document, and it gets better every time an exception teaches you something.


There’s a longer walkthrough in[how to set up your first contract review playbook using our AI contract review agent, Gerri](https://commonpaper.com/blog/how-to-set-up-your-first-contract-review-playbook-with-gerri/) .


## Where AI fits


With a written playbook, you have the documentation you need to finally delegate contract review.


Combine this with an AI agent, and you can start to automate execution. Gerri, Common Paper’s AI contract review agent, applies your playbook to every redline that comes in: accepts what’s inside your defaults, counters with your fallback when a customer pushes, and routes anything outside the rules to the named owner with the context they need to decide. The playbook stays a living document. When you make a new call, you can make it become a rule with one click.


You write the rules once and they run on every contract after that.


## FAQ


### What is a contract playbook?


A contract playbook documents your company’s positions on negotiated contract terms: the redlines you accept, the fallbacks you offer, and who decides when a request goes beyond both. It’s the rule book for exceptions to your standard agreement.


### Who should own the contract playbook?


If you have an in-house lawyer, they are usually the best owner. If you don’t yet have an in-house lawyer, then it should be whoever triages contracts today, usually an operations or RevOps leader, or a founder.


### Do you need a lawyer to create a contract playbook?


No. Start by writing down the calls you already make, then have outside counsel review the positions. An hour of review on a written playbook replaces re-deriving your positions deal by deal at hourly rates.


### What’s the difference between a contract playbook and a CLM?


A CLM stores and tracks contracts. A playbook governs how you negotiate them. You can run a playbook with no software at all, and a CLM full of contracts tells you nothing about which redlines to accept.


Dig in deeper with our former Head of Legal Ecosystem, Tiffany Bui LeTourneau:


##
