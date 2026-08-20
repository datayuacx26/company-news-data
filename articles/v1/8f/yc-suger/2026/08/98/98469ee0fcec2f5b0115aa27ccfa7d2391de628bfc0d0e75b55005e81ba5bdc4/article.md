---
schema_version: "1.0.0"
document_id: "98469ee0fcec2f5b0115aa27ccfa7d2391de628bfc0d0e75b55005e81ba5bdc4"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/the-monthly-marketplace-close-checklist/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-20T00:55:49.760493+00:00"
fetched_at: "2026-08-20T00:55:51.412607+00:00"
content_hash: "sha256:915b5bd2643e822151c924436e1ac9a2475a908a2f62721c3351a0f330792e04"
---

# The Monthly Marketplace Close Checklist

*The monthly marketplace close is the recurring reconciliation a billing-ops owner runs to tie every marketplace transaction to the money that arrived, in dependency order — disbursements first, then metering, tax, revenue recognition, and variance. Run the steps out of order and each one starves the next of the data it needs.*


---


The month-end close on a cloud marketplace is not a report you generate. It is a sequence you run, and the sequence has a shape: each step produces the reconciled figure the next step depends on. Skip disbursement matching and your revenue recognition has nothing solid to book against. True up metering after you have already closed and the correction lands in the wrong period.


Most billing teams learn this order the hard way, one painful close at a time — usually the first month they sell on a second marketplace and discover that the two consoles disagree, pay out on different triggers, and name the same dollar three different things. This post is the annotated checklist: the five steps of the monthly marketplace close, why they run in this order, and what “done” looks like for each before you move on.


This is the *monthly* cadence. The heavier annual work — audit-ready statements, the full-year revenue reconciliation, the numbers your finance team hands an auditor — is a different job that consumes these monthly closes as its inputs; it is out of scope here. For the weekly and daily tasks that surround this monthly cycle, this checklist sits inside[the cloud marketplace operations runbook](https://www.suger.io/resources/blog/the-cloud-marketplace-operations-runbook/) .


---


## What is the monthly marketplace close?


The monthly marketplace close is the periodic reconciliation that ties each marketplace transaction — a subscription, a metered charge, a private offer — to the disbursement that eventually settled it, and produces a set of numbers finance can book and explain. It is a *close* in the accounting sense: at the end of it, the period is locked, the marketplace revenue is recognized, and any difference between what the console showed and what the bank received has an owner and a reason.


It exists as its own discipline because a marketplace dollar is slow and multi-state. One transaction passes through usage recorded, amount calculated, invoice issued, invoice paid, fees withheld, and payout sent — and the clouds hit those moments on different clocks, withholding their fees and triggering payouts at different points. A figure pulled from any single console on any single day is a snapshot of a process, not a total. The close is where you turn those snapshots into one reconciled month. Why the raw numbers never match in the first place is the subject of[why your marketplaces report different numbers](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) ; this post assumes that gap and gives you the routine that resolves it every month.


---


## The five-step close, in dependency order


The close runs in five steps, and the order is not a preference — it is a dependency chain. Each step consumes the output of the one before it, so running them out of sequence means reconciling against figures that are not yet firm.


# Step What it produces Why it must come before the next


1 **Disbursement reconciliation** Every payout matched to the transactions inside it Revenue you recognize has to book against money you can actually trace to a deal.


2 **Metering true-up** Corrected usage records for the period A payout you reconciled against wrong usage is a payout you will re-open.


3 **Tax and withholding** The gross-to-net bridge for the period You cannot recognize net revenue until you know what was deducted before payout.


4 **Revenue recognition** Booked revenue for the closed period Nothing downstream — variance, forecasting — is trustworthy until the period is booked.


5 **Variance review** An explanation for every difference over threshold The close is not done until each gap has an owner, a cause, and a note.


Read the table top to bottom and the logic is plain: money you can trace (1), against usage you have corrected (2), net of what was withheld (3), gives you revenue you can book (4), and anything that still does not tie out gets explained (5). The rest of this post is what “done” means for each step.


---


## Step 1 — Reconcile disbursements first


**Disbursement reconciliation is matching each payout that hit your bank to the specific marketplace transactions it settled, and it comes first because every later number depends on it.** A marketplace does not wire you a labeled, itemized total. It sends a netted amount — one deposit that bundles many agreements, minus fees, minus withholding, sometimes plus or minus adjustments from earlier periods. Your job at step one is to take that deposit apart.


“Done” for this step means: for the month’s disbursements, every payout line is matched to the transactions it represents, and the sum of the traced transactions plus fees plus adjustments equals the amount received. The failure mode to watch for is the *unexplained delta* — a deposit that is close to what you expected but not exact. That gap is almost always a fee taken at a moment you did not model, a credit or refund netted from an earlier period, or a payout that fired before the customer paid. Do not smooth it over; name it, because it is the seed of a variance you will otherwise chase at year end.


Because the deposit is netted, the single hardest artifact to produce here is the link from a payout back to the deals inside it. A billing platform earns its keep at exactly this step: Suger’s[marketplace billing and metering](https://www.suger.io/platform/billing-metering/) layer keeps each agreement, its invoices, and the resulting disbursement on one record, and its Cash & Disbursements and Revenue Records views are built to make a netted payout traceable to the transactions it settled ([doc.suger.io](https://doc.suger.io/) ). That trace is what lets step four book revenue against money, not against a hopeful estimate.


---


## Step 2 — True up the metering


**Metering true-up is reconciling the usage you reported to the marketplace against the usage your product actually generated, and correcting the difference before it hardens into the closed period.** For any usage-based or metered pricing model, the number the marketplace billed is only as right as the usage records you sent it. If a batch of records failed, retried late, or double-counted, the invoice is wrong, the disbursement you just reconciled is wrong, and the revenue you are about to book is wrong.


“Done” here means: for every metered dimension, the total usage the marketplace recorded for the period matches your system of record, and any discrepancy has either been corrected with an adjusting usage record or logged with a reason it cannot be. This is why the step sits *after* disbursement reconciliation but *before* revenue recognition — you want to discover a metering gap while you can still correct the period, not after it is booked. The mechanics of why usage records drop and how retries land are covered in[when usage records fail](https://www.suger.io/resources/blog/when-usage-records-fail/) ; the close-time discipline is simply to run the comparison every month rather than trusting that the pipeline was clean.


A practical guardrail: reconcile metering against your own event log, not against the marketplace’s own report, because the marketplace’s number and your number diverging is precisely the thing you are trying to catch. Suger’s Usage Metering interface reports metered usage across the marketplaces from one place, which makes the period-over-period comparison a single view rather than a per-console hunt ([doc.suger.io](https://doc.suger.io/) ).


---


## Step 3 — Account for tax and withholding


**The tax-and-withholding step is building the bridge from gross transaction value to the net amount that reached your bank, and it must precede revenue recognition because you recognize net.** Between the price on the customer’s order and the deposit in your account sit a marketplace service fee, tax on that fee, transaction tax that may or may not be your responsibility, and — in some countries — withholding deducted before payout. If you model marketplace revenue as list price minus a flat percentage, this is the step where that model breaks.


“Done” means: for the period, the gross-to-net difference on every disbursement is fully accounted for — fee, tax, and withholding each identified — so that the net figure you carry into revenue recognition is explained rather than assumed. Who actually remits transaction tax is decided by the marketplace and varies by country, so this step is where you confirm the current split rather than carry last quarter’s assumption; the structure to hold in your head is laid out in[tax and withholding on marketplace revenue](https://www.suger.io/resources/blog/tax-and-withholding-on-marketplace-revenue/) . Because the rules are the provider’s and they change, treat the specific percentages and country categories as something to re-read at the vendor’s own documentation each period, not a constant to hard-code into the close.


---


## Step 4 — Recognize the revenue


**Revenue recognition at close is booking the period’s marketplace revenue against the reconciled, corrected, net figures the first three steps produced — and it depends on all of them.** With disbursements traced, metering trued up, and the gross-to-net bridge built, you now have a set of numbers solid enough to recognize: revenue tied to real transactions, in the right period, at the right net amount.


“Done” here means the period is booked and locked: recognized revenue for the month ties to the reconciled transactions, deferred balances are carried correctly for multi-month agreements, and the entries are ones you could defend line by line. The reason this is step four and not step one is the whole argument of the checklist — recognize too early, before disbursements are traced or metering is corrected, and you book a number you will restate. Suger’s Revenue Records and Revenue Analytics and Reports are the surfaces that hold the recognized figures and the transaction-level detail behind them ([doc.suger.io](https://doc.suger.io/) ); the point of doing steps one through three first is that this booking rests on data that already reconciles.


---


## Step 5 — Review the variance


**Variance review is the final gate: every difference over your materiality threshold — console versus bank, expected versus recognized, this month versus last — gets an owner, a cause, and a written note before the period is called done.** A close with no variance step is a close that quietly absorbs its errors, and absorbed errors compound into the year-end reconciliation as differences nobody can reconstruct.


“Done” means: each material variance is documented — a timing difference, a fee taken at an unmodeled moment, a refund netted from a prior period, a metering correction that shifted the total — so that the number is not just reconciled but *explained* . Small, consistent variances are usually structural (a payout trigger that fires before collection, a fee withheld earlier than modeled) and belong in a running note so they stop surprising you. Large or new variances are a signal to re-open an earlier step. Export the closed period and its variance notes to your warehouse or finance system so the record survives the month; Suger’s Table Export writes table data to external storage on a schedule for exactly this ([doc.suger.io](https://doc.suger.io/) ). The variance log you keep monthly is what turns the annual close from an archaeology project into a rollup.


---


## Frequently asked questions


**What is the monthly marketplace close?** The monthly marketplace close is the recurring reconciliation that ties each marketplace transaction to the disbursement that settled it and produces numbers finance can book. It runs in five steps: reconcile disbursements, true up metering, account for tax and withholding, recognize revenue, and review variance.


**Why does the order of the close steps matter?** Because each step depends on the one before it. Revenue recognition must book against traced disbursements and corrected metering, and net revenue requires the tax-and-withholding bridge first. Run the steps out of order and you recognize revenue against figures that are not yet firm, then restate them.


**How is the monthly close different from year-end reporting?** The monthly close reconciles one period and locks it. Year-end reporting is the heavier annual job — audit-ready statements and the full-year reconciliation — that consumes twelve monthly closes as its inputs. Doing the monthly variance review well is what keeps the annual close from becoming a reconstruction effort.


**Why don’t the marketplace console numbers match my bank deposits?** Because a marketplace pays a netted amount — many agreements bundled together, minus fees and withholding, sometimes adjusted for earlier periods — not an itemized total. The console reports one state of the money and the bank reflects another. Disbursement reconciliation is the step that takes the deposit apart and matches it back.


**What does “metering true-up” mean at close?** Metering true-up is comparing the usage you reported to the marketplace against the usage your product actually generated, and correcting the difference before the period locks. It runs before revenue recognition so a metering error is fixed while the period is still open, not after it is booked.


---


## Takeaways


- The monthly close is a dependency chain, not a checklist you can shuffle: disbursements, then metering, then tax, then revenue recognition, then variance. Each step feeds the next.
- Reconcile disbursements first, because a marketplace sends a netted deposit, not an itemized total — and everything downstream books against the transactions you trace inside it.
- True up metering *before* you recognize revenue, so a usage error is corrected while the period is still open rather than restated after it is booked.
- Recognize revenue net, which means the tax-and-withholding gross-to-net bridge has to be built first.
- Keep a running variance log every month. It is the single thing that keeps the annual year-end reconciliation from becoming a reconstruction of numbers nobody remembers.


---


The monthly close is only as fast as the reconciliation underneath it. See how[marketplace billing and metering in Suger](https://www.suger.io/platform/billing-metering/) keeps disbursements, usage, and recognized revenue on one record, and where this monthly cadence fits the daily and weekly tasks in[the cloud marketplace operations runbook](https://www.suger.io/resources/blog/the-cloud-marketplace-operations-runbook/) .
