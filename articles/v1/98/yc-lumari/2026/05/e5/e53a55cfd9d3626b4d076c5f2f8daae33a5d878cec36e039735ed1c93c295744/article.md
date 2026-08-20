---
schema_version: "1.0.0"
document_id: "e53a55cfd9d3626b4d076c5f2f8daae33a5d878cec36e039735ed1c93c295744"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/3-way-match"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-08-09T23:40:56.159161+00:00"
fetched_at: "2026-08-09T23:40:57.548506+00:00"
content_hash: "sha256:34009cf1129c7a096e93839c48990e4092523bc43a6d564e09e0a8786e095905"
---

# 3-Way Match: The AP Process That Catches What Procurement Missed

The 3-way match is an accounts payable control that compares three documents before authorizing payment to a supplier: the purchase order (what was ordered), the goods receipt (what was received), and the supplier invoice (what's being billed). When all three agree on quantity, price, and the right line items, the invoice gets approved for payment. When they don't, the invoice goes into exception handling.


The control exists because companies have been getting overcharged, double-billed, billed for parts they never received, and billed at prices nobody agreed to since invented procurement. The 3-way match is the last gate before money leaves the company. When it works, fraud and errors get caught before they become a write-off.


## What Each Side of the Triangle Carries


Three documents. Each from a different system, and each created by a different team.


The **purchase order** is procurement's commitment. Buyer issued it, supplier accepted it, agreed-upon parts at agreed-upon prices for agreed-upon quantities. Lives in the ERP (SAP, NetSuite, Oracle, Epicor, JDE, whatever's running).


The **goods receipt** is operations' confirmation. The receiving clerk inspected what showed up at the dock, counted it, checked it against the packing slip, and posted it as received in the ERP. This step is where most data quality problems start, because receiving is often understaffed and the clerks are pushed for speed over accuracy.


The **supplier invoice** is the supplier's bill. PDF, EDI, or supplier portal upload. Sent to AP after the supplier ships. AP either keys it in, OCR-scans it, or pulls it via integration depending on how modern the AP setup is.


The match is the system reconciling all three. Quantity on the PO matches goods received matches quantity invoiced. Unit price on the PO matches unit price invoiced. Line items match across all three. When everything aligns within tolerance, the invoice clears. When something's off, AP has to investigate.


## 2-Way vs 3-Way vs 4-Way Match


The naming refers to how many documents get compared.


A **2-way match** compares the PO and the invoice only. Used for services and indirect spend where there's no physical receipt (consulting, software subscriptions, utilities). No goods receipt exists, so the match is just "did we order this and is the bill what we agreed."


A **3-way match** is the standard for goods. PO, goods receipt, invoice. The most common control in any procurement-heavy business.


A **4-way match** adds an inspection step. PO, goods receipt, inspection report, invoice. Common in regulated industries: aerospace under AS9100, medical devices under FDA 21 CFR Part 820, automotive under PPAP requirements. The inspection record proves the goods met spec before payment is authorized. Without it, you've paid for parts that quality might still reject.


Most companies should run 3-way for goods and 2-way for services, with 4-way carved out for parts where regulatory traceability requires it. Running 4-way across everything just adds friction. Running 2-way across everything misses fraud.


## What "Match" Actually Means (and Why Tolerances Exist)


A literal match would require every cent and every unit to line up exactly. That's not realistic. Suppliers ship 100 pieces when the PO said 100 but invoice for 102 because of an overage. Or freight gets billed at $47.83 instead of the PO's estimated $50. Or there's a 1.2% rounding difference because of currency conversion.


So matches run within a tolerance. A typical setup:


- Quantity tolerance: 0% over, 5% under (don't pay for more than ordered, allow short ships under threshold)
- Price tolerance: 2% on unit price (covers minor commercial drift)
- Total invoice tolerance: $25 absolute or 1% of total, whichever is smaller


Set the tolerance too tight and AP drowns in exceptions. Set it too loose and supplier overbilling slips through. The right number depends on category. Commodity fasteners can run wider tolerance than custom CNC parts.


The opinion: most companies have tolerances set to default values that no one has reviewed in years. Pull a report of how many invoices clear inside tolerance versus go to exception. If 90%+ clear automatically, your tolerances might be too loose. If 30% go to exception, they're too tight or your upstream data is bad.


## Why the 3-Way Match Breaks Down


In theory, 3-way is automatic. In practice, AP teams are spending real hours on exceptions. The reasons trace back to upstream data, not the AP system itself.


**The PO doesn't reflect the latest agreement.** Buyer cut a PO at $4.20/unit. Supplier emailed three weeks later asking for a 5% increase due to material costs. Buyer agreed in email. PO was never amended. Invoice comes in at $4.41. Mismatch. AP routes it to the buyer for review. Buyer takes two days to respond because they have 200 other emails. Invoice sits in exception for a week.


**Receiving was sloppy.** Goods showed up. Receiving clerk was busy, posted them as fully received without counting, and walked away. Invoice comes in for the same quantity, matches the receipt, but the actual quantity in the warehouse is short. Match clears. Production finds out two weeks later when they pull the box and it's underfilled.


**Partial receipts that never got reconciled.** PO for 5,000 parts. Supplier shipped 4,800 in two boxes. Receiving posted 4,800. Supplier invoiced for 5,000 because they considered it shipped. Now you have a 200-piece mismatch and a six-week-old PO line still open in the ERP.


**Freight invoiced separately when the PO said included.** Buyer cut PO with FOB destination expecting freight included. Supplier interpreted it as FOB origin. Goods invoice matches PO. Then a separate freight invoice arrives. AP doesn't know if it should pay it. Procurement and finance argue about the Incoterm.


**The invoice references parts in different units.** PO is in pieces. Invoice is in cases of 12. Total quantity matches (12 cases x 12 pieces = 144 pieces) but the line items don't auto-match because the UOM is different.


**Multiple POs collapsed onto one invoice.** Supplier sends one invoice covering three POs. The system tries to 3-way match against one PO and fails because the totals don't line up. AP has to manually break the invoice apart.


We've seen AP teams where 1 in 4 invoices ends up in exception. The exception queue is where their week goes.


## What "Good" Looks Like


The 3-way match is supposed to be invisible. The buyer issued a clean PO. Receiving counted accurately. The invoice came in clean. The system matched in 0.3 seconds and queued the payment. Nobody touched it.


The places we see this actually working share a few habits.


POs get updated when the agreement changes. If a price moves or a date shifts, somebody amends the PO. The supplier's email isn't the system of record. The PO is. This is the single biggest hygiene gap at most companies. Procurement agrees to changes in email, never updates the PO, and AP eats the exceptions.


Receiving has time to do the count properly. Often this is a staffing problem, not a process problem. A short-staffed dock is going to post bulk receipts to keep up, which sets up downstream mismatches.


Tolerances are tuned for the spend category. Indirect MRO with frequent small overages runs a wider tolerance than direct production materials with tight cost control.


Exceptions get routed back to the buyer in real time, not batched at month-end. The faster the buyer answers "yes that price change was approved" or "no we didn't authorize that," the faster the invoice clears.


## Where the 3-Way Match Sits in the Bigger Picture


The 3-way match is the last line of defense, not the first. By the time AP is flagging a price mismatch, procurement could have caught it weeks earlier when the supplier's email asking for the price increase came in. Procurement just didn't have the workflow to route it back into the PO.


Treating the 3-way match as the only control means every supplier conversation gets re-litigated at AP. That's expensive and slow. The fix is upstream: capture supplier responses to POs, ASNs, and price-change requests at the time they happen, and update the PO so the match is clean by default.


## Where Lumari Helps


Lumari reads the supplier inbox, parses acknowledgments, ship-date confirmations, partial-fill notices, and price-change requests, and writes those updates back to the PO record before the invoice ever lands. So when AP runs the 3-way match, the PO already reflects what the supplier actually said. Exception volume drops because the PO and the invoice agreed before the invoice arrived.


If your AP team is the place where every supplier conversation gets re-relitigated,[see how Lumari keeps the PO in sync upstream](https://lumari.ai/) .
