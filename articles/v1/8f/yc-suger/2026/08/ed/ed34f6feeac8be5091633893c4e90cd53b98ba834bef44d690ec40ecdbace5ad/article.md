---
schema_version: "1.0.0"
document_id: "ed34f6feeac8be5091633893c4e90cd53b98ba834bef44d690ec40ecdbace5ad"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/first-90-days-cloud-marketplace/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T08:10:57.347630+00:00"
fetched_at: "2026-08-11T08:11:00.150542+00:00"
content_hash: "sha256:ed4181a8d4be225c95033179bf3c7d3d38c7ab7f51bb29d6055d88889e8b5592"
---

# Your First 90 Days Selling on a Cloud Marketplace

*Cloud marketplace onboarding is the work between deciding to sell through a marketplace and running it as a repeatable motion: choosing what to list, wiring the technical integration, passing the marketplace’s review, transacting a first deal, and handing the operating process to whoever owns it afterwards.*


---


The most common way a marketplace program stalls is not technical. It is that someone inherits a half-finished listing with no dates, no owner list, and a vague instruction to “get us on AWS,” and spends six weeks discovering which decisions block which.


The dependencies are knowable. Most of the elapsed time in the first 90 days is spent waiting on two things — a marketplace review queue and an internal decision nobody has been asked to make — and both can be started on day one.


What follows is the calendar. The companion piece to this one is the dependency graph:[how long a marketplace integration takes](https://www.suger.io/resources/blog/how-long-marketplace-integration-takes/) covers which steps block which, and is the reference an engineering lead needs. This post is the schedule.


---


## **What does cloud marketplace onboarding involve?**


**Five things, in rough order: a commercial decision, a listing, a technical integration, a marketplace review, and a first transaction.** Everything else in the first 90 days hangs off those.


The trap is treating them as sequential. Two of the five have long external waits attached, and the way to finish in 90 days rather than 150 is to start those waits while the internal decisions are still being argued about.


---


## **Days 1–14: decide, then start the clocks**


The output of the first fortnight is a set of decisions, not artefacts.


**Decide what you are selling and how it is priced.** Marketplaces treat the pricing model as close to permanent — Microsoft states the pricing model can’t be changed after an offer is published, and AWS states it can’t be changed once a listing is published to limited. This is the single most consequential decision of the 90 days and the one most often made in a hurry.[Cloud marketplace pricing models](https://www.suger.io/resources/blog/cloud-marketplace-pricing-models/) works through the choice.


**Decide who owns the revenue.** Not who does the work — who carries the number. Alliances, sales, and RevOps each end up owning marketplace revenue at different companies, and each produces a different program.[Who should own cloud marketplace revenue](https://www.suger.io/resources/blog/who-owns-cloud-marketplace-revenue/) covers the trade-offs. Leaving this open is what turns week nine into a territory argument.


**Start the accounts.** Seller registration, tax and banking details, and the cloud’s partner program enrolment all involve other people at your company and sometimes a bank. None of them are hard; all of them wait on someone else’s inbox. Start them on day one.


**Pick one marketplace.** Not three. The second is materially cheaper once the first is done, and doing them in parallel triples the number of unfamiliar consoles your team is learning at once.


Owner for the fortnight: whoever will carry the revenue number. If that is still undecided, that is the fortnight’s only real task.


---


## **Days 15–30: the listing and the integration**


Now the artefacts.


**Listing content.** Product description, categories, support terms, and the assets. Suger’s published integration timeline puts creating the listing details at “about an hour” of hands-on work — the elapsed time is almost entirely waiting for marketing and legal to return copy, so request both in week two.


**The technical integration.** Connecting your product to the marketplace’s entitlement and metering interfaces is a smaller job than teams expect: Suger’s timeline puts integrating with AWS Marketplace, Azure Partner Center, or the GCP Producer Portal at “about 20 minutes” each once the account exists. The engineering work that surrounds it — handling the entitlement handshake, provisioning, and metering in your own product — is the real scope, and it is the part worth reading[offer vs entitlement](https://www.suger.io/resources/blog/offer-vs-entitlement/) before you design.


**Submit for review, then stop waiting on it.** The marketplace review queue is the longest external wait in the program and it is entirely outside your control. Submit as early as a complete submission allows, and use the review period for the work in the next section rather than watching the queue.


**Contract path.** Decide now whether you will use the marketplace’s standard contract or your own paper. It determines how fast your first deal can move, and it needs legal, which needs notice.


Owners: product marketing for content, engineering for integration, legal for the contract path.


---


## **Days 31–60: the first real deal**


The listing existing is not the milestone. A transaction is.


**Pick the first deal deliberately.** The best candidate is an existing customer with committed cloud spend and a renewal in the window. They already want to buy, and drawing down a commitment gives them a reason to prefer the marketplace — the mechanics are in[AWS EDP for sellers](https://www.suger.io/resources/blog/aws-edp-what-sellers-need-to-know/) .


**Run one private offer end to end.** Build it, send it, watch what the buyer sees, and time each step. The first one is a rehearsal and should be treated as one.[What buyers see when you send a private offer](https://www.suger.io/resources/blog/what-buyers-see-when-you-send-a-private-offer/) covers the half of that flow you cannot observe from your own console.


**Watch the money, not just the acceptance.** Trace one transaction from acceptance through invoicing to disbursement and confirm every system agrees on the amount. This is the exercise that surfaces the fee treatment nobody had modelled and the currency handling nobody had checked.


**Write down what broke.** The first deal always exposes two or three process gaps. Capturing them while they are fresh is what makes days 61–90 possible.


Owners: an AE with a real deal, deal desk for the offer, finance for the money trace.


---


## **Days 61–90: make it repeatable**


The goal of the last month is that the second deal does not need you.


**Document the offer process** — who requests, who approves, who sends, what the discount authority is. **Wire the CRM** so an offer and an entitlement are visible on the opportunity, and the seller does not need marketplace console access. **Set the co-sell motion up** if you want cloud seller engagement, because the submission mechanics have their own gates and lead times.[How co-sell works](https://www.suger.io/resources/blog/how-co-sell-works/) covers them. **Agree the reporting.** One number, one definition, one owner, reviewed on a fixed cadence. **Then start the second marketplace** , using everything above as a template.


---


## **The 90-day milestone table**


Window Milestone Owner Blocks


Days 1–5 Seller account, tax and banking submitted Finance Everything downstream


Days 1–10 Pricing model chosen and signed off Revenue owner Listing creation


Days 1–14 Revenue ownership decided Executive sponsor Comp, territory, reporting


Days 10–20 Listing content complete Product marketing Submission


Days 15–25 Product integrated with entitlement and metering Engineering First transaction


Days 20–30 Submitted for marketplace review Alliances Public availability


Days 25–40 Contract path decided Legal First private offer


Days 40–60 First private offer sent and accepted Deal desk + AE Everything about the process


Days 50–65 One transaction traced to disbursement Finance Revenue reporting


Days 60–80 CRM shows offers and entitlements RevOps Seller self-sufficiency


Days 70–90 Offer process documented; second marketplace started Revenue owner Scale


Two rows on that table are the ones that actually slip: the pricing model decision, because it feels reversible and is not, and the revenue ownership decision, because nobody wants to make it.


---


## **What usually goes wrong**


**Treating review submission as the finish line.** A listing nobody has transacted against is not a program. Teams that celebrate at submission often have no deal until month five.


**Sequencing the contract decision after the first deal appears.** Legal review of a marketplace contract path takes as long as it takes, and starting it when a real deal is on the table adds that time to the deal.


**One person holding the whole process.** If the offer process exists only in the head of whoever ran the first deal, the program’s capacity is one person’s calendar.


**Doing all of them at once.** Every additional marketplace in the first 90 days is a new console, a new vocabulary, a new review queue, and a new set of edge cases — before you have learned any of them once.


**No baseline.** Record the elapsed time of that first offer: request to send, send to acceptance, acceptance to entitlement. Without it, “faster” is an opinion.


---


## **Frequently asked questions**


**How long does cloud marketplace onboarding take?** Plan for a quarter to reach a repeatable motion. The hands-on work is small; most of the elapsed time is the marketplace review queue plus internal decisions on pricing, contracts, and ownership.


**What should I do first?** Submit seller registration, tax, and banking details, and decide the pricing model. The first waits on other people, and the second is close to irreversible once an offer is published.


**Should I start with one marketplace or all three?** One. The second is much cheaper once the first is done, because the process, the contract path, and the reporting are already built. Running three unfamiliar consoles in parallel is the most common cause of a stalled first quarter.


**Who should own the marketplace program?** Whoever carries the revenue number. Alliances, sales, and RevOps all work as owners, but leaving it undecided produces a program with no comp plan and no reporting line.


**What is the real first milestone?** A completed transaction, not an approved listing. Until a private offer has been sent, accepted, and traced through to disbursement, none of the process gaps have surfaced.


**When should co-sell start?** After the first transaction. Co-sell submissions have their own validation gates and response windows, and they are easier to work once you can already transact.


---


## **Takeaways**


- Start the long external waits — seller registration, tax and banking, marketplace review — on day one, while internal decisions are still being argued.
- The pricing model is the most consequential decision in the 90 days. AWS and Microsoft both freeze it at publication.
- Decide who carries the revenue number in the first fortnight, or comp and reporting will be unresolved at day 90.
- The first milestone that matters is a completed transaction traced through to disbursement, not an approved listing.
- Treat the first private offer as a rehearsal: time every step and write down what broke.
- Do one marketplace properly, then use it as the template for the second.


---


Ninety days is enough — if the process exists somewhere other than one person’s head. See how[getting started with Suger](https://www.suger.io/solutions/getting-started/) compresses listing, offers, entitlements, and reporting into one system, so the second marketplace costs a fraction of the first.
