---
schema_version: "1.0.0"
document_id: "4cd35b1bef13fbf28f37bf5c7653b984458b6eb7bd545405e1e3b420112c870a"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/real-time-payments-vs-ach"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T02:40:54.166685+00:00"
fetched_at: "2026-08-19T02:40:55.455714+00:00"
content_hash: "sha256:1aa60bd82af5417df3f780e8d3b3f849a820ee464352bc50ca6a4530cd67d3a1"
---

# Real-Time Payments vs ACH: Speed, Cost, and When to Use Each

ACH and RTP (Real-time payments) have fundamentally different settlement semantics, and your ledger must model each rail's finality separately or accumulate reconciliation drift.


An ACH transaction that posted to a user's wallet may still be disputed as unauthorized weeks later, subject to applicable return and consumer reporting timeframes. An RTP credit is final the moment the network confirms it. If your ledger records both through identical double-entry posting logic, one of those records is misrepresenting the state of the money.


Picking a rail on cost or speed alone, without deciding how each one lands in the ledger, pushes the problem to reconciliation. The payments team makes the routing call, but the finance team finds out at month-end, when a reversed ACH debit and a real-time credit for the same amount hit on the same day and nobody can tell which is which.


Formance provides a programmable financial infrastructure platform built around an open-source[programmable core ledger](https://www.formance.com/modules/ledger) . Our position is that irrevocable RTP credits and reversible ACH debits cannot share the same posting logic in one ledger without accumulating drift. The two rails need structurally distinct account paths and posting state machines from the first transaction.


### ACH mechanics: batch windows and return codes


ACH gives your ledger two problems batch systems don't: cutoff-driven timing that decides when funds actually move, and a return window that stays open for up to 60 calendar days after posting. Both have to be encoded as ledger state before the first ACH transaction.


#### How Nacha batch processing and Same-Day ACH work


ACH processes payments in batches through fixed daily windows, with standard next-day settlement and three intraday cycles under Same-Day ACH. Roughly[80% of ACH volume](https://www.nacha.org/news/significant-majority-ach-payments-settle-one-business-day-or-less) settles in one banking day or less. The standard next-day window uses a 2:15 a.m. ET deadline. Settlement occurs at 8:30 a.m. ET the next business day.


Same-Day ACH compresses this to within-day settlement across three[FedACH processing windows](https://www.frbservices.org/resources/resource-centers/same-day-ach/fedach-processing-schedule.html) . ODFI deadlines are 10:30 a.m., 2:45 p.m., and 4:45 p.m. ET, with settlement at 1:00 p.m., 5:00 p.m., and 6:00 p.m. respectively. Batches released after the 4:45 p.m. ET cutoff get next-day settlement even if the entries otherwise qualified.


Nacha's Operating Rules split liability between the two institutional roles. The originating depository financial institution (ODFI) warrants that every entry it originates is authorized and must accept returns of unauthorized items within Nacha's two-banking-day and 60-calendar-day return windows. The receiving depository financial institution (RDFI) holds the return rights. The liability split means the originating business, through its ODFI, carries the return-window exposure on every debit it pulls.


#### Return codes and their impact on ledger fund state


An ACH debit used to fund a wallet credit can be returned after the user-visible credit posts, so your ledger cannot treat wallet posting time as network finality. The return window depends on the code and whether the account is consumer or business:


**Code** **Meaning** **Return window** **Applies to**


R01 Insufficient funds 2 banking days All accounts


R02 Account closed 2 banking days All accounts


R03 No account / unable to locate 2 banking days All accounts


R04 Invalid account number 2 banking days All accounts


R05 Unauthorized debit to consumer account 60 calendar days Consumer accounts


R07 Authorization revoked by customer 60 calendar days Consumer accounts


R10 Customer advises originator not authorized 60 calendar days Consumer accounts


R11 Customer advises entry not in accordance with terms 60 calendar days Consumer accounts


A ledger that marks the wallet credit from an ACH debit as settled at posting, rather than at return-window close, has created a provisional-balance problem it won't see for weeks. The user spends against a balance the network can still reverse.


When an R10 return arrives on day 40, the internal books and the bank statement disagree, and the discrepancy surfaces as a reconciliation error at month-end close, long after the funds moved. The correct model holds the credit in a provisional state, then either promotes it or offsets it with a new posting, without mutating the original record.


### RTP mechanics: irrevocable settlement and the always-on credit-push model


Clearing House, the operator of RTP, behaves differently from ACH on two ledger-relevant properties: settlement is final at credit and transfers are credit-push only. Each property changes how the ledger books, reconciles, and audits real-time payments.


#### How RTP settlement is final


The RTP network delivers final credit-push settlement in seconds, around the clock every day of the year, with no batch window and no return path once confirmed.


RTP settles from a fully prefunded joint account at the Federal Reserve; a payment either clears against the sender's prefunded position or is rejected outright, which is why every credit that lands is final. The network has no chargeback mechanism; a sender who errs can only request a voluntary return from the receiving party.


#### Credit-push only with no pull and no chargeback


RTP transfers are credit-push only, which inverts the collection model your product may be used to on ACH. The sender initiates every transfer. Pull-style collection is not available in the same protocol. A business that needs to request payment sends an ISO 20022 Request for Payment message and waits for the counterparty to authorize a push in response. There is no equivalent to an ACH debit, and no equivalent to a card chargeback once the credit lands.


### Real-time payments vs ACH, and the five infrastructure differences


Real-time payments and ACH differ most in finality, cost, limits, reach, and fraud exposure. Each difference changes ledger design.


**Dimension** **ACH** **RTP**


**Settlement speed and finality** 1–3 business days standard; Same-Day ACH within submission day. Finality is probabilistic: Regulation E leaves a 60-day dispute window open on consumer transfers. Seconds. Final on PACS.002 confirmation, with no chargeback path.


**Cost per transaction** $0.20–$1.50 at the processor level, on top of a FedACH network fee of ~$0.0035. $0.045 per credit transfer, no monthly minimums.


**Transaction limits** Same-Day ACH capped at $1M per payment; standard ACH has no published Nacha network per-transaction cap (ODFIs set originator limits). $10M per transaction (effective February 9, 2025).


**Network reach** Every US bank and credit union account. Only rail with universal fallback coverage. 1,260+ participants (May 2026); directly reaches 71% of US DDAs.


**Fraud exposure and controls** Return window doubles as a fraud vector: R10/R11 unauthorized-debit disputes are recredited within 24 hours, no investigation required. 34% BEC impact rate (AFP 2026). Irrevocable; every fraud control must run pre-authorization. 6% BEC impact rate (AFP 2026). APP scams are the growth vector.


#### How settlement speed and finality differ between the two rails


"Settled" means two different things on the two rails, so your ledger's state machine has to encode the finality gap. ACH funds are commonly treated as effectively final two banking days after settlement, but Regulation E leaves a 60-day dispute window open on consumer transfers. An RTP PACs.002 confirmation is final the moment it arrives.


#### Where the real cost of each rail shows up


Network-level RTP pricing is cheap; the premium shows up in integration work, prefunding, and fraud tooling. RTP's $0.045 per credit transfer is justified only when settlement speed produces value the end user can feel: a worker paid tonight instead of Friday, a claim settled during the phone call.


#### How transaction limits shape the routing decision


Limits no longer decide the routing the way they used to. The RTP cap moved to[$10 million](https://www.theclearinghouse.org/payment-systems/Articles/2024/12/Higher_10_Million_RTP_Network_Transaction_Limit_Empowers_New_Uses_12-04-2024) in February 2025, which puts most B2B supplier payments and treasury transfers inside the RTP envelope.


Same-Day ACH remains capped at $1M per payment, while standard ACH has no published Nacha network per-transaction cap and instead relies on ODFI-set originator limits.


For the ledger, the practical consequence is that a single high-value transfer can now clear either rail, so the routing signal has to come from finality requirements and receiver capability rather than from the amount alone.


#### Why RTP still can't reach every account and what that forces on the ledger


RTP does not cover every depository. The[network reaches](https://www.pymnts.com/news/payments-innovation/2026/the-clearing-house-trust-is-the-gatekeeper-for-whats-next-in-payments) more than 1,200 participating banks and touches over 72% of US demand deposit accounts as of early 2026, which still leaves a meaningful tail of receivers unreachable on the rail.


ACH, by contrast, reaches[every US bank](https://www.nacha.org/content/ach-payments-fact-sheet) and credit union account, which makes it the only rail with universal fallback coverage today. Any RTP-first routing strategy has to assume some percentage of attempts will reject on reach alone and fall back to Same-Day or standard ACH. The ledger has to model both outcomes without rewriting the original posting.


#### How fraud exposure inverts between return-window disputes and pre-authorization risk


The rails invert where fraud controls have to run. ACH's[R10 and R11](https://www.nacha.org/rules/differentiating-unauthorized-return-reasons) unauthorized-return codes let a receiver dispute a debit weeks after settlement, and originators that breach Nacha's 0.5% unauthorized return rate threshold face fines up to $500,000 per month.


RTP's irrevocability deletes return fraud but forces every fraud control before authorization, because no recovery path exists after the confirmation message.


### A routing decision framework: when to use RTP and when to stay on ACH


Use RTP when instant finality changes the customer outcome, and use ACH when pull capability or universal reach matters more than instant finality.


#### When should you use RTP?


Use RTP when the receiver's experience depends on the settlement moment. Four flows qualify:


1


**Earned-wage-access disbursements:** TCH lists earned wage access (EWA) as a named RTP use case, and EWA programs can use the network to push earned wages to workers instantly without disrupting the employer's payroll cycle.


2


**Time-critical B2B supplier payments where the receiver's bank participates:** RTP supports large supplier payments and time-sensitive treasury transfers under the $10 million cap.


3


**Insurance claim payouts:** this applies where confirmation is part of the product.


4


**Any flow where a two-day delay has a downstream consequence:** a missed real estate closing, an emergency payroll run, a gig worker's rent.


These RTP cases share the same infrastructure requirement: the ledger must book finality only after the network confirms the credit.


#### When should you stay on ACH?


Stay on ACH when reach or pull-based collection matters more than instant finality. Batch economics also matter for high-volume, low-value disbursements. ACH fits these flows:


1


**Recurring subscription billing and pull-based collections:** ACH is the native pull rail, and its return window doubles as the dispute mechanism your product depends on. RTP requires sender-initiated credits.


2


**High-volume, low-value batch disbursements where per-transaction cost governs the economics:** ACH's batch pricing model beats network-fee-per-transfer economics at scale.


3


**Receivers whose institution does not support RTP:** a meaningful share of US demand deposit accounts still sit outside RTP's direct reach.


In practice, you build a fallback chain. Attempt RTP, treat a network reject as a routing signal, wait for a PACS.002 ACTC status response before booking the final credit, and fall back to Same-Day or standard ACH based on receiver capability and urgency tier.


### Implementing ACH and RTP in one ledger with Numscript


ACH and RTP need separate account paths and posting shapes in the ledger to prevent reconciliation drift between provisional and final states.


An ACH debit used to fund a wallet credit enters a provisional state and only later becomes final, while an RTP credit is final at posting time.


In[Numscript transaction examples](https://www.formance.com/blog/engineering/numscript) , Formance's purpose-built language for describing financial transactions, a $120,000 ACH debit that funds an inbound wallet credit posts through a rail-specific provisional account:


```text
// ACH_WALLET_FUNDING
// Event: ACH debit funds a user wallet through a provisional rail account
send   [USD/2 12000000]   (
source   =   @platform:banks:ach:inbound   allowing   unbounded   overdraft
destination   =   @platform:banks:ach:inbound:provisional
)
send   [USD/2 12000000]   (
source   =   @platform:banks:ach:inbound:provisional
destination   =   @users:1234:wallet
)
set_tx_meta  (  "event_type"  ,   "ach_wallet_funding"  )
set_tx_meta  (  "transfer_id"  ,   "trf001"  )
set_tx_meta  (  "rail"  ,   "ach"  )


```


If the debit comes back on day three, the return is a new posting pair, never a mutation of the original:


```text
// ACH_RETURN
// Event: ACH return reverses the provisional wallet funding as a new posting
send   [USD/2 12000000]   (
source   =   @users:1234:wallet
destination   =   @platform:banks:ach:returns
)
send   [USD/2 12000000]   (
source   =   @platform:banks:ach:returns
destination   =   @platform:banks:ach:outbound
)
set_tx_meta  (  "event_type"  ,   "ach_return"  )
set_tx_meta  (  "return_id"  ,   "ret001"  )
set_tx_meta  (  "return_code"  ,   "R10"  )


```


An RTP credit for the same amount takes a different path with finality encoded at posting time:


```text
// RTP_WALLET_FUNDING
// Event: RTP credit funds a user wallet with finality after network confirmation
send   [USD/2 12000000]   (
source   =   @platform:banks:rtp:settlement   allowing   unbounded   overdraft
destination   =   @platform:banks:rtp:inbound
)
send   [USD/2 12000000]   (
source   =   @platform:banks:rtp:inbound
destination   =   @users:1234:wallet
)
set_tx_meta  (  "event_type"  ,   "rtp_wallet_funding"  )
set_tx_meta  (  "transfer_id"  ,   "trf002"  )
set_tx_meta  (  "rail"  ,   "rtp"  )


```


Without rail provenance in the account path, the two $120,000 credits are indistinguishable in the ledger. When the ACH return debit and the RTP credit land in the same settlement window, reconciliation against the external bank statement sees the same dollar amount moving in both directions and cannot say which movement belongs to which rail.


Timing needs the same discipline.[Bi-temporality in ledgers](https://www.formance.com/blog/financial-operations/card-timeline-2) records both when an event was recorded and when it was effective, so an ACH return that arrives three business days after the originating credit lands with a record time of day three and an effective settlement time that points back to the original entry.


The user's posted balance history is never rewritten retroactively. Conflating record time and effective time is how cut-off queries end up guessing.


### Get a rail-specific ledger before scaling to multiple rails


Three ledger decisions prevent multiple rail transaction failures in RTP and ACH. Rail-specific account paths keep a returned ACH debit and an inbound RTP credit for the same amount from collapsing into an indistinguishable pair.


Bi-temporal timestamps let a late-arriving ACH return post today against an effective date weeks in the past without rewriting balance history. Automated reconciliation against external statements catches drift while it's still a single-transaction problem.


Encode the ledger issue now, on the first rail, or pay for it later during a forensic reconciliation on the second.[Clone the Formance Ledger on GitHub](https://github.com/formancehq/ledger) , run it locally, and post your first ACH and RTP transactions against rail-specific account paths before adding a second payment rail.
