---
schema_version: "1.0.0"
document_id: "91906971d4a8ee2204330c367466db962f80a3536b6ff937032b76199c8c059e"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/how-co-sell-works/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T08:10:57.347630+00:00"
fetched_at: "2026-08-11T08:11:00.150542+00:00"
content_hash: "sha256:7339476dd688c98c3dffc409c79a6ce6ee25303a4aae45c67edeaa74425e82d1"
---

# How Co-Sell Works on AWS, Azure, and Google Cloud

*Co-sell is the process of working an opportunity jointly with a cloud provider’s field sales team, coordinated through a shared record: an ACE opportunity on AWS, a co-sell opportunity in Microsoft Partner Center, or a partner engagement in the Google Cloud Partner Network. The relationship is the easy part. The record has rules.*


---


Most co-sell advice is about the motion — map accounts, meet the reps, build the joint story. That advice is correct and it is covered elsewhere, including in[Partnerships 101](https://www.suger.io/resources/blog/partnerships-101-build-a-winning-co-sell-motion-with-aws-azure-gcp/) and the[co-sell playbook](https://www.suger.io/resources/guides/co-sell/) .


This post is about the other half, which almost nobody writes down: what happens to the record after you submit it. Each cloud runs its own state machine, with its own validation gates, its own required fields, and its own definition of when a deal counts. Teams that lose co-sell deals rarely lose them on relationship. They lose them because a submission sat in a status nobody was watching, or missed a threshold nobody had read.


Below is the mechanical view of all three, from each provider’s own documentation.


---


## **What does co-sell actually mean?**


**Co-sell means a cloud provider’s seller has been given a stake in your deal** — visibility, credit, and usually a quota impact — in exchange for helping you win it. The vehicle is a record you create in the provider’s partner system, which their seller can see, accept, and work.


That record is doing three jobs at once, and they are worth separating because they fail differently:


1. **Notification** — telling the provider a deal exists.
2. **Attribution** — establishing who sourced it, which determines your program credit.
3. **Coordination** — carrying the state both sides work from.


A submission that succeeds at notification and fails at attribution is the common outcome. The deal closes, everyone is happy, and it doesn’t count toward anything.


---


## **AWS: a validation state machine**


AWS runs the tightest process of the three, and it is the only one whose statuses are fully documented as an API contract.


An opportunity in AWS Partner Central carries a` Lifecycle.ReviewStatus` that moves through a fixed sequence:


Status What it means


` Pending Submission` Created, not yet sent to AWS. Nothing is happening


` Submitted` Sent for validation. “No changes can be made to the opportunity until the review process is complete”


` In-Review` AWS is validating that “the opportunity details are accurate and complete”


` Action Required` AWS wants changes. Requirements arrive in` Lifecycle.ReviewComments`


` Approved` Validated, and “ready for co-selling activities”


` Disqualified` The terminal rejection


Three mechanical facts fall out of that, and each is a place teams lose time.


**Creating an opportunity does not submit it.**` CreateOpportunity` produces a record in` Pending Submission` . You must then associate at least one solution — “a Partner Solution, AWS Marketplace Solution, or AWS Marketplace Product,” between one and ten of them — and then call` StartEngagementFromOpportunityTask` . An opportunity that was created but never had engagement started is invisible to AWS and will sit indefinitely.


**` Action Required` opens only eleven fields.** When AWS returns an opportunity, you can edit the customer’s city, country, postal code, state or region, street address and website URL, the target close date, the expected customer spend amount and currency, the customer business problem, and your own partner opportunity identifier. Nothing else. If the problem is the solution you associated, you are not fixing it with an update.


**Status changes arrive as events, not emails.** AWS’s own guidance is to monitor the` Opportunity Updated` event through Amazon EventBridge and then call` GetOpportunity` to read the current` ReviewStatus` . A team polling a portal tab once a week is the team whose deals sit in` Action Required` for six days.


Separately from review status, the opportunity moves through AWS’s sales stages: **Prospect** , **Qualified** , **Technical validation** , **Business validation** , **Committed** , **Launched** , and **Closed lost** . AWS defines *Launched* precisely — “Billing for the solution has begun” — which is why marketplace transaction data and co-sell records have to agree.


AWS also assigns “a co-sell motion and an Opportunity Quality score that together determine how AWS engages on the deal.” That scoring, and what it means for whether an AWS seller actually works your opportunity, is covered in[how AWS decides whether you get a rep](https://www.suger.io/resources/blog/aws-decides-whether-you-get-a-rep/) . Field-level submission mechanics are in the[AWS ACE guide for ISV sellers](https://www.suger.io/resources/blog/aws-ace-guide-for-isv-sellers/) .


---


## **Microsoft: six deal types and a registration threshold**


Microsoft’s Partner Center models the *shape of the collaboration* rather than a validation queue, which makes the first decision a taxonomy question.


**You need a co-sell-ready solution before you can create anything.** Microsoft is explicit: “To create a new co-sell deal, you need at least one co-sell-ready solution.”


Then you pick a type:


- **Azure IP co-sell** — you have an Azure IP co-sell eligible solution in the deal.
- **Services co-sell** — for partners with a Solutions Partner designation.
- **Partner-to-partner (P2P)** — you invite another partner, and optionally Microsoft.
- **Solution assessments** — restricted to vetted assessment partners.
- **Partner-led** — Microsoft sellers can *see* the deal but are not asked to help.
- **Private** — Microsoft sees nothing until you change your mind.


The type is set by two questions in the form: “Identify the kind of help that you need from Microsoft?” and “Would you like Microsoft sellers to view this deal?” Answer no to both and you have created a private deal that no Microsoft seller will ever see.


**Customer account selection decides eligibility before you have entered a deal value.** Search results appear on three tabs — *Microsoft Managed* , *Microsoft Unmanaged* , and *Other* (from the Moody’s database). Only Microsoft-managed accounts are eligible for Azure IP co-sell deal registration. Microsoft’s own guidance is blunt about the cost of guessing: pick from the Other tab and “you won’t know whether deal registration is possible until later, and you won’t be able to change the account without involving support.”


**Required fields now include solution taxonomy.** Deal name, location, estimated value, and estimated close date have always been required. Solution Area and Solution Play are now required for both IP and Services co-sell deals created through the portal or bulk upload.


**Microsoft has 14 days to decide.** “Microsoft sales representatives have a 14-day window to decide whether they’ll participate in the deal.” Inbound opportunities you don’t respond to are archived as *expired* on the same clock.


Sales stages are numeric and standard, which matters if you sync from a CRM: Created and Accepted at 10%, Qualified 20%, Developed 40%, Proposed 60%, Negotiated 80%, Won 100%. Partner Center maps your stages onto these.


### The registration criteria worth memorising


Deal registration is where Microsoft co-sell converts into program credit, and it has hard gates. A deal is eligible only if **all** of the following hold:


- Status is *won*
- An Azure IP co-sell eligible solution is in the deal
- Deal type is *partner-led* or *co-sell*
- If co-sell, Microsoft accepted the invitation or marked it won
- Deal value is **at least USD 25,000**
- The customer account is Microsoft-managed


And one timing rule that catches fast-moving teams: “If the deal is eligible for deal registration, make sure that there’s a gap of 72 hours between creating the deal and marking the deal *won* . Closing a deal as won earlier than that might result in deal registrations being rejected.”


Note also that *expired* , *declined* , *won* , and *lost* are terminal — “Deals can’t be modified after they move into a terminal state.” Marking a deal won to tidy up the pipeline is irreversible.


If the customer has committed Azure spend, the marketplace side of the transaction matters too;[what a MACC is](https://www.suger.io/resources/blog/what-is-a-macc-azure-committed-spend-explained/) covers the drawdown.


---


## **Google Cloud: the offer is the record**


Google Cloud is the shortest section here, and honesty about why is more useful than filling it out.


The partner program is the **Google Cloud Partner Network** , and partner engagement runs through it. But Google’s published Marketplace partner documentation covers the Producer Portal, private offers, receiving payments, and reporting — it does not document a partner-facing opportunity lifecycle with named statuses equivalent to AWS’s` ReviewStatus` or Partner Center’s sales stages. There is no public state machine to write down.


What that means practically is that on Google Cloud the artefact carrying the deal is usually the **private offer** and the Marketplace transaction attached to it, rather than a separate co-sell record with its own review queue. Sequencing therefore differs: on AWS and Microsoft the co-sell record typically precedes the commercial paperwork; on Google Cloud the offer often *is* the coordination point.


Two practical consequences:


- **Get the offer right early** , because it is doing more work than its AWS or Microsoft equivalent.[Google Cloud private offers: setup and pitfalls](https://www.suger.io/resources/blog/google-cloud-private-offers-setup-and-pitfalls/) covers the prerequisites and the errors that block submission.
- **Track engagement yourself.** With no published status field to read, the record of who at Google is engaged and when lives in your CRM or nowhere.


If a reseller is involved, the mechanics diverge further —[bringing resellers into Google Cloud Marketplace](https://www.suger.io/resources/blog/resellers-on-google-cloud-marketplace/) covers what transfers from an AWS CPPO motion and what has to be rebuilt.


---


## **The cross-cloud comparison**


AWS Microsoft Google Cloud


System Partner Central (ACE) Partner Center Referrals Google Cloud Partner Network


Published status model Yes —` ReviewStatus` + sales stages Yes — deal types + numeric sales stages Not published for partner opportunities


Prerequisite to submit One associated solution or product At least one co-sell-ready solution Marketplace listing and offer capability


Provider response window Validation queue, no fixed SLA published 14 days for a seller to decide Not published


Hard value threshold None published for submission USD 25,000 for IP co-sell deal registration None published


Change control Locked in` Submitted` /` In-Review` ; 11 fields in` Action Required` Terminal states cannot be modified Offer amendment rules apply


Read that table as three different contracts rather than three flavours of one. A single internal “co-sell submitted” stage that maps onto all three will misreport at least two of them.


---


## **The operating rules that follow**


**Submit early, not at close.** On AWS, validation happens before the opportunity is usable. On Microsoft, registration eligibility requires a 72-hour gap. Both punish the team that batches submissions at quarter end.


**Watch the return path, not the outbox.**` Action Required` and inbound expiry are both silent failures if nobody owns the queue. Subscribe to the events; don’t rely on somebody opening a tab.


**Qualify the account before the deal.** Microsoft’s managed/unmanaged distinction decides registration eligibility at the first form field, and it cannot be changed later without support.


**Keep one field mapped across all three: your own opportunity ID.** AWS has` PartnerOpportunityIdentifier` , Microsoft has an optional CRM ID. Populate both. It is the only thing that lets you reconcile provider records against your pipeline, and it is the field most commonly left blank.


**Never let stage mapping be implicit.** AWS’s *Launched* means billing started. Microsoft’s *Won* is terminal and gates registration. If your CRM pushes a generic “Closed Won” to both, one of them is wrong.


---


## **Frequently asked questions**


**How does co-sell work with a cloud provider?** You create an opportunity record in the provider’s partner system, associate an eligible solution, and submit it. The provider validates it, decides whether a seller engages, and both sides then work from that shared record through to close.


**What is the difference between co-sell and a marketplace transaction?** Co-sell is the sales collaboration and the credit attached to it. A marketplace transaction is how the customer pays. A deal can be one, the other, or both — and on AWS the *Launched* stage explicitly means billing has begun.


**Why do AWS co-sell submissions get returned?** AWS sets the status to` Action Required` when details are inaccurate or incomplete, and explains why in` Lifecycle.ReviewComments` . Only eleven fields are editable in that state, mostly customer address, close date, expected spend, and the business problem.


**What is the minimum deal size for Microsoft co-sell?** There is no minimum to submit a co-sell opportunity. Azure IP co-sell *deal registration* requires a deal value of at least USD 25,000, along with a won status, an eligible solution, and a Microsoft-managed customer account.


**How long does Microsoft take to respond to a co-sell deal?** Microsoft sellers have a 14-day window to decide whether to participate. The same 14-day clock applies in reverse: inbound opportunities you do not respond to are archived as expired.


**Does Google Cloud have an equivalent to AWS ACE?** Google Cloud runs the Google Cloud Partner Network, but does not publish a partner opportunity status lifecycle equivalent to ACE. In practice the private offer and its Marketplace transaction carry the deal.


---


## **Takeaways**


- AWS runs a documented validation state machine. Creating an opportunity does not submit it — you must associate a solution and start the engagement.
- In` Action Required` , AWS opens only eleven fields. If the problem is the associated solution, an update will not fix it.
- Microsoft’s deal *type* is set by two questions in the form, and answering no to both creates a private deal no seller ever sees.
- Microsoft deal registration needs a won status, an eligible solution, a Microsoft-managed account, USD 25,000, and a 72-hour gap before you mark it won.
- Google Cloud publishes no equivalent opportunity lifecycle. The private offer carries the deal, so it has to be right earlier.
- Populate your own opportunity identifier on every provider record. It is the only reliable key back to your pipeline.


---


Three providers, three state machines, one pipeline that has to reconcile against all of them. See how[co-sell automation in Suger](https://www.suger.io/platform/cosell/) keeps ACE, Partner Center, and Google Cloud records in step with your CRM — so a status change reaches the deal owner instead of a portal tab.
