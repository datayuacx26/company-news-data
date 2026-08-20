---
schema_version: "1.0.0"
document_id: "6a09eab45e90b3359af7a1c2e994a72f50be1259ae15e6b7fa5feb04bc8e0baf"
company_key: "yc-mesh-2"
company: "Mesh"
source_id: "yc-mesh-2-news-import-08422f80fc3e"
canonical_url: "https://usemesh.com/article-unbilled-accruals-guide"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-27T03:45:30.475054+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:42b4073c55eb8d242048ea41765d07a0782791c320792d21cc77a7e3ee23ef1e"
---

# The Complete Guide to Unbilled Accruals: Catching the Incurred-But-Not-Invoiced Problem Before It Hits Close

## Summary


Key takeaways


-


An unbilled accrual records an expense before the invoice exists


: the work was done or the good was received, but nothing has hit accounts payable yet.


-


Most accrual tools only read data that's already structured


: POs, receipts, invoices. Unbilled accruals live upstream of all three.


-


The signal is usually a confirmation, not a document


: a vendor email, a Slack message, a verbal sign-off that work is done.


-


A consistent estimation method matters more than precision


: contract terms, run-rate, or percentage of completion, applied the same way every period.


-


Mesh watches the same inboxes and channels a person would


, so these accruals get captured as they happen instead of being discovered during close week.


Every close has a handful of accruals that don't show up cleanly in any system: the contractor who wrapped up work last Thursday but hasn't sent an invoice, the vendor who confirmed delivery over email, the project a department head mentioned was "basically done" in a Slack thread. None of that creates a record in your ERP, and all of it represents a real liability that belongs on this period's books.


This is a practical guide to identifying, estimating, and eventually automating unbilled accruals, the part of the close that most accrual software quietly skips.


## What Is an Unbilled Accrual?


An unbilled accrual, sometimes called an accrued liability or an incurred-but-not-invoiced (IBNI) expense, is a cost your company has already incurred but hasn't yet been billed for. The matching principle in GAAP requires that expenses be recorded in the period they're incurred, not the period the invoice happens to arrive. If a contractor performed work in June but doesn't send an invoice until the second week of July, that expense still belongs on June's books.


This is different from a standard accounts payable balance, which only exists once an invoice has actually been received and entered. An unbilled accrual is, by definition, an estimate made ahead of the paperwork. It gets trued up once the real invoice shows up, usually the following period.


Common examples include:


- **Professional services** billed monthly in arrears, like legal, consulting, or contractor work where the invoice always lags the work by a few weeks.
- **Utilities and usage-based costs** where the billing cycle doesn't line up with the close calendar.
- **Freight and logistics** where goods have shipped or arrived but the carrier's invoice hasn't been processed.
- **Partially delivered services** under a longer contract, where only a portion of the total scope has been completed as of period-end.


## Why Unbilled Accruals Get Missed So Often


Most accrual software, and most controllers doing this manually, work from the assumption that the source of truth lives in a structured system: a purchase order, a receiving record, an invoice queue. That assumption breaks down exactly where unbilled accruals live, because the whole reason they're unbilled is that no document has been created yet.


The actual signal that an expense has been incurred is almost always informal. A vendor replies to a thread confirming "yes, that phase wrapped up on the 28th." A department head mentions in Slack that a project is done. A delivery confirmation lands in a shared AP inbox days before the actual invoice follows. None of that is structured data. All of it is real information that a careful controller would use to decide an accrual is owed.


Tools built to read ERP and procurement data are, by design, blind to this. They can tell you what's already been recorded. They can't tell you what hasn't been recorded yet but should have been. That gap is exactly where manual close time goes: someone has to go looking for these signals themselves, usually by scanning inboxes and pinging project owners in the days before close.


When that signal lives in a procurement platform, the request and purchase data there is often the earliest place it surfaces. We cover the specifics for[accrual automation on Zip](https://www.usemesh.com/accrual-automation-zip) and[accrual automation on Coupa](https://www.usemesh.com/accrual-automation-coupa) .


The accruals that slip aren't usually the big, obvious ones. They're the small, easy-to-forget ones that depended on someone remembering to check a specific inbox at the right time.


## How to Estimate an Unbilled Accrual


Without an invoice to work from, you need a defensible way to estimate the amount. The right method depends on the type of expense, but most fall into one of four approaches.


### Contract terms


If you have a signed contract or statement of work with a fixed monthly or milestone-based fee, that figure is usually your best estimate. This is the simplest case: a $15,000/month retainer that hasn't been invoiced yet gets accrued at $15,000, full stop.


### Historical run-rate


For recurring costs without a fixed contract, like usage-based SaaS or variable utility bills, the prior three to six months' average is a reasonable proxy. It won't be exact, but it's consistent, and consistency is what your auditors actually care about.


### Percentage of completion


For a longer engagement where only part of the scope is done by period-end, calculate the accrual as the contract value multiplied by the percentage of work actually completed. This requires an honest check-in with the project owner, which is usually where the informal Slack or email confirmation comes in.


### Vendor confirmation


Sometimes the most reliable number is the one the vendor gives you directly, even informally. A vendor confirming "we delivered $8,200 worth of goods this month, invoice to follow" is a legitimate basis for an accrual, provided you document where that confirmation came from.


Whichever method you use, the important part is applying it consistently period over period. Auditors are far less concerned with whether your estimate was exactly right than with whether your methodology was documented and applied the same way every time.


## Booking, Reversing, and Truing Up


An unbilled accrual is a temporary entry by design. The standard lifecycle looks like this:


1. **Book the estimate** in the period the expense was incurred, debiting the relevant expense account and crediting accrued liabilities.
2. **Reverse it automatically** at the start of the following period, so it doesn't sit on the books indefinitely.
3. **Record the actual invoice** when it finally arrives, which nets against the reversal.
4. **True up the difference** if the estimate and the actual invoice don't match, which is normal and expected, not a sign something went wrong.


Teams that skip the reversal step, or forget to true up consistently, are usually the ones whose accrual balances become impossible to reconcile after a few quarters. The estimate is supposed to be temporary. Treating it as permanent is where accrual accounts turn into a mess nobody wants to untangle.


## How the Common Tools Handle This


Most accrual and close automation platforms are built around structured data that already exists somewhere in your systems. That's genuinely useful for the accruals with a clean paper trail. It's much less useful for the ones that don't have one yet.


Approach Reads Structured Data Reads Email / Slack Signals Handles Unbilled Accruals


Manual Spreadsheets Manual entry only


No


Only if someone remembers to check


ERP-Native Modules ERP records only


No


Not until an invoice or PO exists


Gappify / BlackLine Verity ERP and procurement data


Sends outbound requests, waits for reply


Partial; depends on vendor response time


Mesh ERP, procurement, AP inbox


Yes; monitors inboxes and Slack in real time


Captured as the signal arrives, not after the fact


## Automating the Signal, Not Just the Entry


The hard part of unbilled accruals was never the journal entry itself, debiting an expense and crediting a liability is not complicated accounting. The hard part is knowing an accrual is owed in the first place, before anyone has told you directly through a formal document.


Mesh is built around that gap. It monitors your email, Slack, and AP inbox continuously, so when a vendor confirms scope completion or a delivery notice lands, that signal is picked up immediately, matched against your accrual logic, and staged as a journal entry for review. Nobody has to remember to go looking for it during close week, because it was already captured when it happened.


**The entries that used to require a person combing through inboxes are the ones Mesh is built to catch automatically.** That's the difference between an accrual tool that reads what's already documented and one that catches what hasn't been documented yet.


Because every accrual Mesh stages ties back to the specific signal that triggered it, whether that's an email thread, a Slack message, or an AP inbox record, the audit trail holds up even for the estimates that started as an informal confirmation rather than a formal document.


## Frequently Asked Questions


What is an unbilled accrual?


An unbilled accrual is a liability for a good or service that has already been received or performed, but for which no invoice has been received or entered into the accounting system yet. It's recorded so expenses match the period in which they were actually incurred, per the matching principle.


How is an unbilled accrual different from an accounts payable balance?


Accounts payable is created once an invoice has been received and entered into the system. An unbilled accrual exists precisely because that invoice hasn't arrived yet, even though the expense has already been incurred. It's an estimate booked ahead of the paperwork, then trued up once the invoice arrives.


Why do unbilled accruals get missed so often?


Because the signal that an expense was incurred usually lives outside the ERP entirely, in a confirmation email, a Slack message, or a verbal sign-off, rather than in a PO or invoice record. Tools that only read structured ERP or procurement data never see it, so the responsibility for catching it falls entirely on whoever remembers to check.


How do you estimate an unbilled accrual without an invoice?


Most teams use contract terms, historical run-rate, percentage of completion, or a vendor's own confirmation of work performed to arrive at a reasonable estimate, then true it up against the actual invoice once it arrives the following period. Consistency in the method matters more than precision in any single estimate.


Can unbilled accruals be automated?


Yes, but only by tools that monitor the same inbound signals a human would: email, AP inboxes, and messaging tools, not just ERP and procurement records. Mesh is built specifically to catch these signals and apply your accrual logic to them automatically, so the accrual is staged the moment the confirmation arrives rather than during a scramble at close.


### Catch unbilled accruals before they hit close


Mesh monitors your email, Slack, and AP inbox so incurred-but-not-invoiced accruals are captured the moment they happen, not discovered during close week.


Get a demo
