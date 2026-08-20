---
schema_version: "1.0.0"
document_id: "77e8306e85eecf6484531dc44f13b1d60ac136b0844998d5dd1ecac087a27d4b"
company_key: "yc-finlens"
company: "Finlens"
source_id: "yc-finlens-news-import-eb6409796ae1"
canonical_url: "https://www.finlens.app/blogs/inventory-accounting-methods"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T14:14:47.490481+00:00"
fetched_at: "2026-08-06T14:14:48.910031+00:00"
content_hash: "sha256:c5581b5b6b263315cd8b9a00c229240321c04bf30348e53139693a71308ca3c9"
---

# Inventory Accounting Methods: The CPA Firm's Book and Tax Playbook

Inventory accounting is where biggest book-to-tax difference on a typical SMB return quietly lives. Every top-ranking search result explains FIFO versus LIFO in same three paragraphs with same $10-and-$12-purchase example. Almost none cover actual practitioner mechanics: §471(c) small-business exemption that changed game for most SMB clients, Form 3115 required to switch methods, lower-of-cost-or-NRV write-down that survives an audit, and QBO ledger workflow that keeps book and tax reconciled every close.


## The four methods


**First-In, First-Out (FIFO).** Costs oldest purchases first into COGS; newest costs sit in ending inventory. In rising-price environments, produces highest reported income and highest tax. Ending inventory carries at current-market cost, which lenders and investors prefer. The default for most SMB clients on both book and tax.


**Last-In, First-Out (LIFO).** Costs newest purchases first into COGS; oldest costs sit in ending inventory. In rising-price environments, produces lowest reported income and lowest tax reason it exists. **LIFO conformity rule** (IRC §472(c)): if a taxpayer uses LIFO for tax, it must also use LIFO for financial statements presented to shareholders, creditors, or beneficiaries. Prohibited under IFRS.


**Weighted Average Cost (WAC).** A running average cost per unit total cost of goods available for sale divided by total units. Produces results between FIFO and LIFO. Common in commodities, chemicals, agricultural products, and manufacturing operations where units are indistinguishable. Simplest calculation of three flow assumptions.


**Specific Identification.** Tracks actual cost of each individual unit sold. Only feasible for high-value, uniquely identifiable items cars, jewelry, art, custom fabrication. Required by IRS when items can be specifically identified (Treas. Reg. §1.471-2(d)).


## The §471(c) small-business exemption


The Tax Cuts and Jobs Act rewrote inventory tax accounting for small businesses. For taxable years beginning in 2026, a taxpayer with **average annual gross receipts of $31 million or less** (§448(c) threshold, indexed) can elect under **§471(c)** to:


1. **Treat inventory as non-incidental materials and supplies** (deducted when consumed or used), or
2. **Follow method used in taxpayer's applicable financial statement (AFS)** , or if no AFS, method used in its books and records.


For most SMB clients, this means inventory tax method just tracks book method. No LIFO election. No FIFO election. No accounting-method change every time management wants to switch approaches on books.


This is a massive simplification most CPA firms still haven't fully operationalized on client base. Every eligible client should have a documented §471(c) election on file. Every non-eligible client (above $31M threshold) is on §471(a) inventory and needs full method-tracking discipline below.


## Book method flow assumption


For financial reporting,[GAAP](https://www.finlens.app/resources/gaap-compliant) under **ASC 330** allows FIFO, LIFO, weighted average, or specific identification. Once selected, method must be applied **consistently** period over period; a change requires disclosure and prior-period restatement of comparatives.


The choice drives four downstream metrics:


- **COGS on income statement** periodic cost of inventory sold.
- **Ending inventory on balance sheet** residual cost pool.
- **Gross margin** revenue minus COGS.
- **Inventory turnover ratio** COGS ÷ average inventory.


In a rising-price environment (default assumption for most product categories), ranking from highest to lowest reported income is FIFO → WAC → LIFO. In falling prices, ranking reverses.


For book purposes, method matches physical flow when possible. Grocery and pharmacy inventories are FIFO because that's how merchandise actually moves. Bulk commodities are WAC because units are fungible. High-ticket unique items are specific ID because that's how they're actually tracked.


## Tax method where differences live


For taxpayers not electing §471(c), IRC §471(a) requires tax inventory method to "clearly reflect income." The IRS accepts FIFO, LIFO, average cost, or specific identification. The tax method must be applied consistently, and a change requires **Form 3115** with prior IRS consent (automatic consent under Rev. Proc. 2024-23 for most inventory method changes).


**Uniform Capitalization (§263A / UNICAP).** Producers, resellers with gross receipts above small-business threshold, and taxpayers on §471(a) must capitalize certain indirect costs into inventory: purchasing, storage, handling, quality control, allocable overhead, and interest on production loans. Direct materials and direct labor are always in inventory; UNICAP adds indirect layer.


The §471(c) election exempts small-business taxpayers from UNICAP entirely. This is single biggest reason to document election on eligible clients it eliminates UNICAP compliance burden.


**LIFO reserve.** Taxpayers using LIFO for tax must disclose LIFO reserve difference between LIFO inventory and what inventory would be under FIFO. The reserve is a required disclosure and a common M-1 reconciling item. The LIFO reserve also drops out of covenant calculations on many banking relationships.


## Lower of cost or net realizable value (LCNRV)


Under **ASC 330-10-35** , inventory measured under FIFO, weighted average, or specific identification is written down when net realizable value falls below cost. LIFO inventories continue under older "lower of cost or market" rule.


**Net realizable value (NRV)** = estimated selling price – reasonably predictable costs of completion, disposal, and transportation.


The LCNRV write-down is audit workpaper every inventory-heavy client owes on every fiscal-year-end:


1. Identify slow-moving, obsolete, or damaged inventory by SKU.
2. Estimate net realizable value for each category.
3. Compare NRV to book cost.
4. Write down difference to COGS through an inventory reserve (or directly against inventory).
5. Reversal of write-down in later periods is prohibited under US GAAP (unlike IFRS).


The write-down is a **permanent** book-tax difference for most SMBs tax deduction is not available until inventory is actually disposed of, sold at a loss, or written off under taxpayer's method.


## Perpetual vs. periodic


**Perpetual inventory system.** Every purchase and every sale is recorded to inventory in real time. Ending inventory and COGS are always current in GL. Standard for e-commerce clients on Shopify, WooCommerce, or A2X; for QBO Advanced with inventory; and for any client on a real ERP.


**Periodic inventory system.** Purchases go to a Purchases account; ending inventory and COGS are calculated at period-end using a physical count. Common on QBO Simple Start and Essentials, and on cash-basis or §471(c) clients.


The perpetual system produces a defensible balance sheet every month. The periodic system produces a defensible balance sheet only after count.


Almost every inventory-heavy client should be on perpetual for management reporting, even if tax return is prepared on §471(c). The cost is a QBO subscription upgrade or a Shopify-to-QBO connector; return is a real-time gross margin and a defensible month-end.


## The inventory reserve for obsolescence


Distinct from LCNRV write-down (which is per-SKU) is general **obsolescence reserve** a percentage-of-inventory allowance that anticipates future write-downs based on historical experience.


The reserve is calculated on aged inventory buckets (0–90 days, 91–180 days, 181–365 days, over 365 days) with escalating reserve percentages by aging bucket. Every fiscal-year-end, reserve is trued up to calculated balance, with adjustment flowing through COGS.


The obsolescence reserve is not deductible for tax until inventory is actually disposed of another permanent-until-realized book-tax difference to carry on ledger.


## Standard costing and variance analysis


Manufacturing clients often use **standard costs** for inventory predetermined per-unit costs applied at point of production. Actual costs are tracked separately, and variances (price, usage, efficiency) are posted to COGS.


The book/tax distinction:


- **Book** : standard costs applied to inventory; variances closed to COGS at period-end. GAAP-compliant if variances are immaterial or if ratio of variances to total inventory is disclosed and adjusted.
- **Tax** : §471(a) requires actual cost, adjusted for method (FIFO, LIFO, WAC). Standard costs are permitted only if they approximate actual cost. A variance-to-inventory reconciliation is required.


The variance workpaper is a common audit finding for manufacturing clients variances left in a suspense account or spread arbitrarily across inventory rather than flowing systematically to COGS.


## Journal entries five inventory events


**1. Purchase (perpetual)**


```text
Dr. Inventory                                  XXX
Cr. Accounts Payable / Cash                      XXX
```


**2. Sale (perpetual two entries)**


```text
Dr. Accounts Receivable / Cash                 XXX
Cr. Sales Revenue                                XXX


Dr. Cost   of   Goods Sold                         XXX
Cr. Inventory                                    XXX
```


**3. Physical count adjustment**


```text
Dr. Shrinkage Expense (or COGS)                XXX
Cr. Inventory                                    XXX
```


**4. LCNRV write-down**


```text
Dr. Cost   of   Goods Sold (or Inv. Write-Down)    XXX
Cr. Inventory Reserve (or Inventory)             XXX
```


**5. Obsolescence reserve adjustment**


```text
Dr. Cost   of   Goods Sold                         XXX
Cr. Inventory Obsolescence Reserve               XXX
```


Periodic systems replace entry 2 with an end-of-period COGS calculation: Beginning Inventory + Purchases – Ending Inventory = COGS.


## Changing methods Form 3115


An accounting method change requires **Form 3115 (Application for Change in Accounting Method)** with either:


- **Automatic consent** under Rev. Proc. 2024-23 for most inventory method changes (FIFO ↔ WAC ↔ specific ID; adoption or revocation of §471(c) election; LIFO adoption or termination).
- **Non-automatic consent** for rest, with a user fee and up-front IRS approval.


The 481(a) adjustment cumulative income effect of change if it had always been applied under new method is taken over one year if favorable (decreasing income) or four years if unfavorable (increasing income).


Form 3115 is a substantial workpaper. For a LIFO termination in particular, §481(a) adjustment recaptures entire accumulated LIFO reserve as ordinary income often a material multi-year drag on client's return.


## How Finlens keeps inventory reconciled


Finlens keeps QBO general ledger for inventory-heavy clients[reconciled month-by-month](https://www.finlens.app/resources/accrual-and-schedule-automation) , so inventory number on balance sheet is always defensible and book-tax difference is carried on ledger, not reconstructed at return time.


- **Perpetual system tie-out.** Finlens reconciles QBO Inventory Asset account to source-system inventory (Shopify, WooCommerce, A2X, or an ERP). Physical count adjustments and shrinkage post monthly, not annually.
- **§471(c) documentation.** For eligible clients under $31M gross receipts threshold, Finlens tags §471(c) election in client file and applies "materials and supplies" treatment consistently across purchases.
- **LCNRV workpaper.** Finlens produces aged-inventory report by SKU each month, so LCNRV write-down at fiscal year-end works from a current dataset instead of a reconstructed spreadsheet.
- **Obsolescence reserve.** The percentage-of-inventory reserve calculation is refreshed every month from aging buckets and posted as a standing journal entry.
- **Book-tax parallel.** For §471(a) clients, Finlens carries LIFO reserve, UNICAP capitalization, and inventory method-specific reconciliation on ledger, feeding directly into M-1 or M-3 on return.
- **Form 3115 support.** When a client changes methods, Finlens produces §481(a) adjustment calculation with a complete prior-year comparison.


Finlens is not an inventory management system SKU-level tracking still lives in Shopify, an ERP, or an inventory app. Finlens is ledger-side reconciliation and workpaper layer that keeps balance sheet honest and return preparation short.


## Conclusion


**Inventory accounting is where largest recurring book-tax difference on most SMB returns quietly hides.** The four flow assumptions are textbook. The practitioner value is §471(c) documentation, LCNRV workpaper, obsolescence reserve schedule, and ledger discipline that keeps QBO tied to source system every close.


§471(c)


$31M small-biz election


LCNRV


ASC 330-10-35


Form 3115


method change


## Inventory-heavy client?
The ledger needs to tie.


Finlens reconciles QBO Inventory Asset to Shopify or the ERP, produces the aged-inventory report driving LCNRV, and carries the LIFO reserve or §471(c) election on the ledger every close.


[Book a Walkthrough →](https://cal.com/finlens/intro)[See how it works →](https://www.finlens.app/accountants)


see how Finlens reconciles QBO Inventory Asset to Shopify or ERP, produces aged-inventory report that drives LCNRV write-down, and carries LIFO reserve or §471(c) election on ledger.


Bring file for client with $4M of inventory on balance sheet, no aged report, no obsolescence reserve, and a §471(c) election that was made verbally in 2022 and never documented. That's file this workflow is built for.


## Frequently asked questions


### **Which inventory method is best for a small business?**


**‍** For most SMB clients with average annual gross receipts under $31M, best answer is to elect §471(c) and let tax method track book method (typically FIFO or WAC). This eliminates UNICAP and simplifies method changes.


### **Can I use different methods for book and tax?**


Yes, except for LIFO, which requires conformity under IRC §472(c). For FIFO, WAC, and specific ID, book and tax can differ, but reconciliation must flow through Schedule M-1 or M-3.


### **How often do I need to physically count inventory?**


For audited financial statements, at least annually with auditor observing count. For non-audited compilations, annually as a minimum; monthly or cycle counting is defensible if perpetual system is reliable.


### **When do I write down inventory to net realizable value?**


Under ASC 330-10-35, whenever NRV falls below cost. Practically, at every fiscal year-end for audit purposes, and quarterly for management reporting on inventory-heavy clients


### **Is LIFO worth tax savings?**


Only for taxpayers with material inventory in rising-price environments, above §448(c) small-business threshold, and without foreign operations that require IFRS reporting. For most SMB clients, answer is no LIFO conformity rule (must also use LIFO on financials) usually outweighs tax benefit.


### **What is LIFO reserve?**


The difference between inventory reported under LIFO and what inventory would be under FIFO. It's a required disclosure for LIFO taxpayers, a recurring M-1 reconciling item, and a common banking-covenant addback (many lenders add LIFO reserve back to working capital).


### **Does §471(c) apply to producers and manufacturers?**


Yes, if taxpayer meets §448(c) gross-receipts test ($31M for 2026, indexed). The election is available regardless of whether taxpayer produces, resells, or provides services alongside inventory.


The authoritative reference on tax accounting methods for inventory (including §471(c) election and Form 3115 mechanics) is[IRS Publication 538 Accounting Periods and Methods](https://www.irs.gov/publications/p538) . For Schedule M-1 side of book-to-tax reconciliation this creates, see Finlens guide to[book-to-tax reconciliation](https://www.finlens.app/blogs/book-to-tax-reconciliation) .


‍
