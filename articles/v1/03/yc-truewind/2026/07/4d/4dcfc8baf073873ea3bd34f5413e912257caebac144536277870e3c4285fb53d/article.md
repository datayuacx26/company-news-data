---
schema_version: "1.0.0"
document_id: "4dcfc8baf073873ea3bd34f5413e912257caebac144536277870e3c4285fb53d"
company_key: "yc-truewind"
company: "Truewind"
source_id: "yc-truewind-rss-014af42b96d1"
canonical_url: "https://www.truewind.ai/blog/rollforward-gl-automation"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.745562+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:a52b0c4ad299d6f406821c2d2e5bac465e3e9d0cf1d272082ae232e5f666abd4"
---

# Investment Rollforwards for Family Offices (June 2026)

Every family office accountant knows what a rollforward report looks like. Beginning market value, activity during the period, ending balance. The structure is consistent. The problem is everything that happens before the rollforward becomes a GL-ready entry. Brokerage data arrives in three different formats. Income needs breakouts by type. Realized gains require lot matching that the custodian PDF doesn't surface cleanly. Your investment rollforward in Sage Intacct should pull directly from the source and land as journal entries without the spreadsheet layer in between.


**TLDR:**


- A rollforward tracks investment balances from period to period, mapping purchases, sales, realized gains, unrealized gains, and fees to GL-ready journal entries.
- Family offices managing multiple entities across custodians like Schwab, Fidelity, and Pershing face compounding manual work matching different data formats.
- Differences typically hide in four places: income accrual timing, currency translation, unsettled trades, and fee netting.
- Rollforwards serve as audit evidence, tying position-level brokerage data directly to GL entries without additional reconstruction.
- Truewind reads brokerage statements and maps each component to the correct GL account and dimension in Sage Intacct or QuickBooks Online.


## What Rollforward Reports Are and Why Family Offices Need Them


A rollforward report tracks how an account balance moved from one period to the next: beginning balance, plus activity during the period, lands at the ending balance. For a single investment account, it's tractable. For a family office managing entities across multiple custodians, it becomes the mechanism that holds the books together across periods.


Family offices rely on rollforwards because investment activity doesn't arrive pre-formatted for the general ledger. Custodian statements report in their own structure, and someone has to organize that data by income type, tie it to the GL, and preserve it as audit evidence. Without a rollforward tying each period to the last, every month-end risks becoming a rebuild from scratch.


## The Structure of a Brokerage Rollforward: Beginning Balance to GL-Ready Entry


A brokerage rollforward traces every change in an investment account from one period-end to the next. The structure is consistent regardless of custodian or asset class.


### Core Components


Every rollforward moves through the same sequence:


- Beginning balance: the ending fair value from the prior period, which must agree to the prior GL close.
- Purchases and contributions: cash deployed into new or existing positions during the period.
- Proceeds from sales and distributions: cash received on dispositions or income events.
- Realized gains and losses: the difference between cost basis and sale proceeds for closed positions.
- Unrealized gains and losses: mark-to-market changes on positions still held at period-end.
- Fees and expenses: custodian, management, and advisory charges reducing the account value.
- Ending balance: the sum of all the above, which becomes the beginning balance for the next period.


### Mapping to GL Accounts


Each line maps to a[specific GL account and transaction type](https://www.truewind.ai/sage-intacct-reconciliation) . Unrealized gains post to an OCI or income account depending on how the investment is classified. Realized gains post separately. Fees reduce cash or an accrued liability. Every line needs a debit, a credit, and the right dimensions before it touches the ledger.


The rollforward is not complete when the math ties. It is complete when every component has a corresponding GL-ready journal entry.


## Common Data Sources: Custodian Statements, PDFs, and Investment System Exports


Family offices pull investment data from a surprisingly wide range of sources, and almost none of them arrive in a format ready for journal entry posting.


The most common inputs include:


- Custodian statements from firms like Schwab, Fidelity, or Northern Trust, which typically export as PDFs or structured CSVs with position-level detail but inconsistent column naming across custodians.
- [Brokerage trade confirmations](https://www.truewind.ai/workpaper-automation-sage-intacct) that break out realized gains, accrued interest, and dividend income as separate line items, each requiring its own GL account mapping.
- Investment manager reports delivered as Excel files or PDFs, often formatted for investor readability instead of accounting ingestion.
- Capital account statements from private funds, where income allocations and fee offsets arrive monthly or quarterly with no standard schema.


The formatting inconsistency across these sources is where rollforward preparation gets slow. A single portfolio held across three custodians may require three separate parsing passes before you can build a unified beginning-to-ending position schedule.


## Breaking Out Investment Income by Type: Interest, Dividends, Realized Gains, Unrealized Gains


Family offices track investment income across four distinct categories, and each one lands differently in the GL.


Interest and dividends are straightforward: they hit income accounts when received or accrued, depending on the entity's accounting basis. Realized gains require matching a disposal event to the original cost basis pulled from the rollforward's beginning balance or a subsequent purchase lot. Unrealized gains sit in accumulated other income or flow through the P&L depending on how the security is classified under the applicable accounting standard.


Income Type Recognition Trigger Cost Basis Required Typical GL Treatment


Interest Received or accrued No Income account on receipt or accrual date


Dividends Received or accrued No Income account on receipt or accrual date


Realized Gains Disposal event Yes Gain/loss account matched to original purchase lot


Unrealized Gains Period-end mark No Accumulated OCI or P&L depending on security classification


[The rollforward report](https://www.truewind.ai/sage-intacct-month-end-close-automation) holds all four in one place. Beginning market value, purchases, disposals at cost, income received, and the period-end mark all appear as discrete line items, which means each income type can be traced back to a specific transaction or valuation event before it touches the GL.


## Multi-Entity and Multi-Custodian Complexity: When One Rollforward Isn't Enough


For family offices managing multiple entities, each one typically requires its own rollforward. A fund, a holding company, and a trust may all hold positions at different custodians, with different account structures, and on different reporting cycles.


The volume compounds quickly:


- Each entity needs a beginning balance, purchases, sales, income, and ending balance tracked separately, even when the same underlying security appears across several of them.
- Custodians like Schwab, Fidelity, and Pershing format their data differently, so consolidating across accounts means working through format differences before you can even start on the numbers.
- Interentity positions and cross-allocations add another layer, since a position partially allocated across entities needs to tie at both the entity level and the consolidated level.


The question for[family office accountants](https://www.truewind.ai/product/ai-bookkeeping) isn't whether to run separate rollforwards. It's whether the process for producing them scales without a corresponding increase in manual hours each month.


## Tying Rollforward Outputs to the General Ledger


Producing a rollforward report is only half the work. Getting those outputs to tie back to the GL is where family office accountants tend to lose hours.


The core check is straightforward: the ending market value on your rollforward should match the investment account balance on the trial balance as of the same date. If it does not, the gap lives somewhere in one of four places.[APQC research](https://www.apqc.org/resource-library/resource/cycle-time-perform-monthly-close) indicates that most surveyed organizations take 5-6 days to complete their monthly close, and rollforward reconciliation issues are one of the primary drivers of delays.


### Where Differences Tend to Hide


- Timing differences on income accruals: interest or dividend income that the brokerage has recognized but your GL has not yet posted will create a mismatch between the rollforward ending value and the book balance.
- Currency translation: for non-USD positions, the exchange rate applied in the brokerage feed may differ from the rate used when the GL entry was recorded, leaving a residual that needs a separate FX adjustment entry.
- Unsettled trades: positions that traded before period-end but settle after will appear in the brokerage data at trade date while the GL may reflect settlement date, depending on your accounting policy.
- Fee netting: some custodians net advisory or custody fees directly against account proceeds without breaking them out, so the gross rollforward figure will not foot to the net GL posting without an explicit fee line item.


[Working through these in order](https://www.truewind.ai/blog/automate-sage-intacct-reconciliation-without-replacing-gl) gives you a repeatable close checkpoint instead of an open-ended search for the difference.


## Common Breakpoints: Where Rollforward Processes Fail in Practice


Family offices running brokerage data through a manual close process tend to hit the same failure points, regardless of fund count or AUM.


### Stale or Incomplete Brokerage Data


Custodians like Schwab, Fidelity, and Pershing publish confirmations on different schedules. If your team pulls data before all positions are settled, the beginning balance on your next rollforward won't agree to the prior ending balance. That discrepancy has to be chased down before anything else can close.


### Lot-Level Cost Basis Gaps


Brokerage statements report realized gains at the lot level, but most GL imports don't carry that granularity. Cost basis detail gets dropped in translation, leaving the accountant to reconstruct it manually from trade confirms or custodian reports.


### Multi-Custodian Aggregation


When a single entity holds accounts at two or more custodians, the rollforward has to be built from separate feeds and tied to a single GL position. The more custodians involved, the more places a timing difference can hide.


## How Rollforwards Support Audit Readiness and Compliance


Auditors don't verify ending balances in isolation. They trace[the path from opening to ending](https://www.truewind.ai/blog/reconciling-payroll-registers-sage-intacct) . The rollforward is the document that makes each step in that path visible and defensible.[PCAOB auditing standards on internal control](https://pcaobus.org/oversight/standards/auditing-standards/details/AS2201) explicitly cover roll-forward procedures as part of audit evidence requirements.


For family offices, audit readiness often comes down to whether brokerage data can be tied back to GL entries at the position level. When rollforward schedules are built from consolidated brokerage statements and posted as GL-ready journal entries, auditors get the transaction history they need without requesting additional support.


There are a few specific places where a well-structured rollforward earns its weight during audit fieldwork:


- Realized gain and loss support: auditors can agree disposal proceeds and cost basis directly to the schedule without reconstructing each lot-level calculation from raw brokerage data.
- Accrued income validation: dividend and interest accruals recorded in the GL need to tie to the rollforward's income recognition entries, giving auditors a single source to test against.
- Opening balance agreement: the prior period ending balance on the rollforward should match the opening GL balance exactly, so year-over-year continuity can be confirmed without additional reconciliation work.


The compliance angle follows the same logic. Regulatory filings that require investment schedule disclosures draw directly from positions and valuations that should already exist in a properly maintained rollforward. Teams that keep rollforward schedules current throughout the year instead of reconstructing them at year-end typically find the audit and filing process less disruptive.


## Automating Brokerage Rollforwards: From Manual Spreadsheets to GL-Ready Entries with Truewind


Truewind reads brokerage statements directly and maps each line to the[correct account, cost center, and dimension](https://www.truewind.ai/blog/sage-intacct-reconciliation-gap) or QuickBooks Online. No spreadsheet intermediary. No manual re-keying.


The output is a set of GL-ready journal entries that reflect the full rollforward structure:


- Beginning market value pulled from the prior period's closing balance in the GL
- Purchases, sales, and transfers mapped to the appropriate investment accounts by security type and entity
- Realized and unrealized gain/loss entries calculated and split correctly across holding periods
- Ending market value tied back to the custodian statement


Your team reviews the prepared entries before anything posts. Truewind handles the construction; your accountants own the approval.


## Final Thoughts on Family Office Rollforward Workflows


Rollforwards hold your books together across periods, but only if someone builds them before the GL can close. For family offices running multiple entities and custodians, that process compounds fast. The math has to tie, the income has to split correctly, and every line needs audit support before it posts. If your team is rebuilding rollforwards from scratch every month,[see how Truewind automates brokerage rollforwards](https://www.truewind.ai/see-a-demo) without the manual build.


## **FAQ**


### Can I build brokerage rollforwards without maintaining separate Excel files each month?


Yes. Truewind ingests custodian statements directly and builds rollforward schedules automatically, mapping each component to the correct GL account in Sage Intacct or QuickBooks Online. The system generates GL-ready journal entries without requiring manual spreadsheet construction or period-over-period file maintenance.


### Brokerage rollforward Excel vs Truewind automated workpapers?


Excel requires manual data extraction from PDFs, manual lot-level cost basis tracking, and separate reconciliation work each month to tie the rollforward back to the GL. Truewind automates the entire sequence: it reads the statement, breaks out income by type, calculates realized and unrealized gains, and produces journal entries that tie directly to your trial balance without intermediate spreadsheet steps.


### How do I tie multiple custodians when each one formats statements differently?


Truewind processes statements from different custodians in parallel and normalizes the data into a unified rollforward structure. Whether your positions sit at Schwab, Fidelity, or Northern Trust, the tool maps each custodian's output to the same GL account framework so you get consistent journal entries across all accounts without manual reformatting.


### What's the best way to handle lot-level cost basis for realized gain calculations?


The rollforward should track cost basis at the lot level so when a position is sold, you can match the disposal proceeds to the specific purchase lot. Truewind pulls lot-level detail from brokerage trade confirmations and ties it to the rollforward's beginning balance to generate accurate realized gain entries without requiring manual cost basis lookups.


### When should I move from manual rollforward spreadsheets to an automated system?


If you're managing more than five entities with investment accounts, or if your team spends more than two hours per entity each month building rollforwards and tying them to the GL, the manual process no longer scales. At that volume, automation becomes the only way to maintain consistent close quality without adding headcount.
