---
schema_version: "1.0.0"
document_id: "2f2b37765bf3d5a791c2365c1c3ffa203815071de291abb482fb60c0203b83e3"
company_key: "yc-toku"
company: "Toku"
source_id: "yc-toku-news-import-57695167b900"
canonical_url: "https://www.toku.com/resources/how-toku-runs-fully-private-stablecoin-payroll-on-aleo-and-usad"
published_at: "2026-07-24T12:00:00+00:00"
first_seen_at: "2026-07-26T02:46:59.685075+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:f6a8af7414c6d1587e5b102a3be1b12fe78a98f3faebdc590d7e05f1c2b781fb"
---

# How Toku Runs Fully Private Stablecoin Payroll on Aleo and USAD

Aleo's USAD case study details how Toku runs fully private stablecoin payroll on USAD, the US dollar stablecoin issued by Paxos Labs. Most stablecoin payroll runs on public blockchains, where every salary and wallet is visible to anyone. Toku's does not, and Toku and Aleo proved it on their own teams first.


**TL;DR**


- Toku has launched what it describes as the first fully private stablecoin payroll solution, built on Aleo's zero-knowledge blockchain and settled in[USAD](https://aleo.org/post/usad-case-study/) , a US dollar stablecoin issued by Paxos Labs.
- The problem it solves: on a public blockchain, every payment amount and wallet is visible, so standard[stablecoin payroll](https://www.toku.com/resources/what-is-stablecoin-payroll) exposes individual salaries and total headcount spend to anyone with a block explorer.
- Aleo and Toku paid their own globally distributed teams through the system before offering it to enterprise clients.
- Toku has processed more than one billion dollars in token payroll volume. The private launch adds confidentiality to payroll infrastructure that was already in production.
- Salaries stay private on-chain while the employer keeps a complete, exportable record for tax and audit. Privacy on the ledger and disclosure to your auditor are two separate controls.


In January 2026, Toku launched a fully private stablecoin payroll system built on the Aleo blockchain and settled in USAD, a dollar stablecoin from Paxos Labs. It uses zero-knowledge cryptography to keep salary amounts and identities off the public ledger, while the employer keeps full records for tax and audit.


What gets recorded Public-chain stablecoin payroll Toku's private payroll


Individual salary amounts Visible to anyone Shielded on-chain


Recipient wallet and identity Visible to anyone Shielded on-chain


Employer identity and payout wallet Visible to anyone Shielded on-chain


Total payroll spend and headcount signal Derivable by anyone Not exposed


Tax, withholding, and net-pay records Kept off-chain Kept off-chain, fully retained


Auditor and tax-authority access Through employer records Through employer records


## What did Toku launch?


Toku launched a stablecoin payroll system that keeps compensation private on-chain. It runs on the Aleo blockchain, a layer-1 network built around zero-knowledge proofs, and settles in USAD, a US dollar stablecoin issued by Paxos Labs. Salary amounts and recipient identities are shielded on the public ledger. The payment still settles same-day, and it still produces a record the employer can prove.


Toku is a global payroll platform that has processed more than one billion dollars in token payroll volume. The compliance and payroll mechanics were already running in production, so this launch adds a confidentiality layer to infrastructure that already works, rather than starting from scratch. Toku describes it as the first fully private stablecoin payroll solution.


The launch was announced alongside Aleo and Paxos Labs as part of the USAD rollout. The short version: dollar-denominated pay, same-day settlement, and none of it posted for the public to read.


## Why does private payroll matter?


A public blockchain is a shared ledger. Every transaction records the sender, the recipient, and the exact amount, and it stays there permanently for anyone to read. That transparency is a feature for an open payment network. It is a liability for payroll.


Run salaries through a public chain and the exposure compounds. Anyone who maps one employee wallet to one person can see that person's pay, every cycle, forever. Add up the outgoing payments from the company wallet and you have total payroll spend, headcount, and the raw material to estimate runway. None of that requires a breach. It is simply how the ledger works.


Ken O'Friel, Toku's CEO, puts the buyer reaction plainly: "Every public company CFO we talk to gets excited about stablecoins until they realize their payroll would be public."


That sentence is the whole blocker in one line. The settlement speed and the cost savings are real, but they do not matter if adopting them means broadcasting compensation data. Privacy, not speed or cost, is what kept enterprise finance teams on the sidelines. It is[a big part of why adoption stalled](https://www.toku.com/resources/why-enterprise-companies-have-been-slow-to-adopt-stablecoin-payroll) .


## How does confidential stablecoin payroll work?


The flow looks like standard payroll with a private settlement layer underneath. It fits alongside the payroll workflows a finance team already runs, rather than replacing the system of record.


1. Step 1. The company funds payroll from its treasury in a US dollar stablecoin. No fiat prefunding sits idle for days waiting on wire cutoffs.
2. Step 2. Payments settle to each recipient in USAD on Aleo. The amounts and identities are shielded on-chain by zero-knowledge proofs.[Same-day settlement](https://www.toku.com/resources/instant-settlement-payroll) , with no public trail of who earned what.
3. Step 3. The employer keeps the authoritative record. Gross pay, withholding, deductions, and net pay are logged for every recipient, exportable for tax filing and reconciliation. The confidentiality applies to the public ledger. Your books stay complete.
4. Step 4. Recipients receive dollar-denominated pay they can hold or spend, without their salary posted to a ledger the whole internet can read.


Approvals, reporting, and your source of truth stay where they are. What changes is the settlement rail underneath, and the fact that it keeps compensation private by design.


## Does private payroll stay compliant and auditable?


Yes, and this is the question every finance and tax lead asks next. Privacy on a public ledger and accountability to a regulator are separate controls, and a serious payroll setup keeps both.


Confidential does not mean opaque to the people who are supposed to see it. The employer holds a complete, timestamped payroll record: who was paid, how much, what was withheld, and when. That record is what a tax authority, an auditor, or a finance team works from. Zero-knowledge privacy shields the data from the public block explorer. It does not shield it from your own books or from a lawful request.


[Payroll data privacy](https://www.toku.com/resources/payroll-data-privacy) is increasingly treated as a core payroll compliance obligation in its own right. Programs are expected to prevent compensation data from leaking through exports, vendor sprawl, or on-chain visibility, and to prove the controls worked. A confidential rail addresses the on-chain exposure at the settlement layer, instead of patching it after the fact.


Treat this as informational only. It is not legal or tax advice. Classification, withholding, and reporting requirements vary by jurisdiction, so confirm your specific obligations with counsel.


## Aleo and Toku ran it on their own payroll first


Aleo and Toku ran their own globally distributed teams on the system before offering it to enterprise clients, paying their own people in private stablecoin payroll first. Dogfooding a payroll product is a specific kind of proof: the teams building it accepted their own salaries on the rail before asking anyone else to.


The broader market has been waiting for this. Stablecoin transaction volume ran into the tens of trillions of dollars in 2025, yet a very small share of businesses use stablecoins for payroll, and the multi-trillion-dollar global payroll market still runs almost entirely off-chain. The gap was never speed or cost. It was privacy and compliance, and this launch closes both at once.


## Frequently Asked Questions


### What did Toku launch, and when?


In January 2026, Toku launched a fully private stablecoin payroll system built on the Aleo blockchain and settled in USAD, a US dollar stablecoin issued by Paxos Labs. It lets companies pay salaries in dollar-denominated stablecoins without exposing amounts or identities on a public ledger, while keeping complete records for tax and audit.


### Can salaries stay private on a public blockchain?


Not on a standard public chain, where amounts and wallet addresses are visible to anyone. They can stay private on a chain built for confidentiality using zero-knowledge proofs, which verify that a payment is valid without publishing the amount or the identities. That is the difference between a transparent ledger and a confidential one.


### Is it legal to pay employees in stablecoins?


In most jurisdictions, paying in stablecoins is permitted as long as the underlying payroll obligations are met: correct classification, withholding where required, statutory contributions, and reporting. The stablecoin is the settlement method. It does not exempt anyone from payroll rules. Requirements vary by country, so confirm your obligations with tax and legal counsel.


### What crypto is used for payroll?


Payroll uses fiat-pegged stablecoins rather than volatile cryptocurrencies, because pay needs to hold its value between the pay run and the moment it is spent. US dollar stablecoins are the common choice. Toku's private payroll settles in USAD, a US dollar stablecoin issued by Paxos Labs on the Aleo network.


### Is private payroll still auditable for tax and compliance?


Yes. Confidentiality applies to the public ledger, not to the employer's records. The company keeps a complete, exportable payroll record covering who was paid, how much, and what was withheld, which is what auditors and tax authorities work from. Privacy on-chain and disclosure to the people entitled to see the data are separate.
