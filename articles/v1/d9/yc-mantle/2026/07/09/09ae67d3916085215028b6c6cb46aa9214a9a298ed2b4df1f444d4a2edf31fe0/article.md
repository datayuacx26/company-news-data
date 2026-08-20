---
schema_version: "1.0.0"
document_id: "09ae67d3916085215028b6c6cb46aa9214a9a298ed2b4df1f444d4a2edf31fe0"
company_key: "yc-mantle"
company: "Mantle"
source_id: "yc-mantle-rss-aa2f82cfb1c8"
canonical_url: "https://blog.withmantle.com/safe-conversion-scenarios-every-founder-should-understand/"
published_at: "2026-07-23T19:48:45+00:00"
first_seen_at: "2026-07-27T03:39:14.723809+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:a7ed4611b250451eac20cb300b76f7f748d61884293352f642d6c8accfd5d7e4"
---

# SAFE Conversion Scenarios Every Founder Should Understand

## Why SAFE conversion deserves more attention than the SAFE itself


A[SAFE](https://blog.withmantle.com/how-safes-work/) , short for Simple Agreement for Future Equity, feels simple on the day you sign it. Money comes in, legal costs stay relatively low, and you postpone a priced equity round. The hard part usually shows up later, when that SAFE converts and changes the ownership math.


SAFE conversion is the point where a SAFE turns into actual shares, and it is an ownership event, not a formality. If your company has multiple SAFEs, an option pool, unvested stock, and a new lead investor asking for a larger pool before closing, small recordkeeping mistakes turn into material dilution surprises.


Y Combinator introduced SAFEs in 2013, and they became widely adopted because they let startups raise capital without setting a valuation upfront, while still giving investors[a contractual right to receive equity later](https://blog.withmantle.com/how-safes-work/?utm_source=openai) . That trade-off is exactly why conversion deserves more planning than the SAFE itself ever gets.


---


## What is a SAFE conversion?


SAFE conversion is the process by which a Simple Agreement for Future Equity turns into equity shares, typically triggered by the company’s next priced financing round. The SAFE holder does not own shares until conversion happens. Before that point, they hold a contractual right to receive equity later, priced according to the terms in the agreement: the valuation cap, the discount, and any most-favored nation (MFN) provision.


---


## What needs to be in place before a SAFE converts


### Clean ownership data comes first


Before you model any conversion, gather the details for every security already on the cap table:


- Each SAFE investor, investment amount, signing date, valuation cap, and discount
- Any MFN provision
- Any side letter with pro rata or other special rights


You also need accurate equity records outside the SAFEs themselves. Confirm the number of common shares outstanding, restricted stock awards, option grants, exercised options, canceled grants, and the current reserved option pool. If vesting schedules are wrong or stale, your fully diluted share count will be wrong too, and that can distort round modeling.


This is why founders should treat the cap table as a living operating record, not a document they clean up only when financing counsel asks for it. A[single source of truth](https://withmantle.com/docs/safes?utm_source=openai) matters most right before conversion, when every security starts interacting with every other security.


### SAFEs don’t exist in isolation


A spreadsheet can work when there is one SAFE and a tiny team. It gets fragile once you add overlapping instruments and timing questions. A post-money SAFE, a pre-money SAFE, a pool expansion, and employee grants issued between fundraising steps can all affect final ownership.


Founders who model those pieces together are better prepared for diligence and less likely to discover cap table issues a week before closing.


---


## How SAFE conversion actually works


### The terms that change the math


Most SAFE conversions turn on a few core terms.


**Valuation cap** sets the maximum company valuation used to calculate the SAFE holder’s conversion price. A lower cap usually means the investor receives shares at a lower effective price, which means more shares.


**Discount** gives the investor a percentage reduction from the next round’s price per share.


**MFN (most-favored nation)** can let an earlier SAFE holder adopt better economic terms from a later SAFE.


**Pre-money** **vs. post-money** **SAFEs** . Post-money SAFEs make ownership easier to estimate because they are designed to define the investor’s ownership after that SAFE investment and before the new priced money arrives. Pre-money SAFEs can be less intuitive because the impact of later SAFEs and pool changes can shift the final dilution outcome, as described in[Mantle’s SAFE guide](https://withmantle.com/docs/safes?utm_source=openai) .


### The usual triggers, and the less obvious ones


The most common conversion trigger is the next priced equity financing. That is the scenario most founders expect.


But SAFEs can also be affected by acquisitions, IPOs, and other liquidity events, depending on the contract. In those cases, the instrument may convert, cash out, or follow another contract-specific path. The important habit is to read the trigger language before you sign.


---


## The conversion scenarios that catch founders off guard


### High-valuation rounds can still create heavy dilution


Founders often assume a strong priced round means old SAFEs will be less painful. Not necessarily. If the next round is priced well above a SAFE’s valuation cap, the SAFE usually converts at the cap, not at the higher round price. That can make the SAFE dramatically more dilutive than the founder expected.


### Down rounds change who bears the pain


In a down round, the economics can shift again. Depending on the terms, the SAFE may still convert at more favorable economics than the new investor price, or the discount may become the better deal. You can’t judge the impact from the headline valuation alone. You have to test the actual conversion formulas.


### Multiple SAFEs stack, they do not average out


Two SAFEs with the same dollar amount can produce very different outcomes if one has a $5 million cap and the other has a $10 million cap. Add MFN rights or a required option pool increase, and the stack becomes even more complex. That is why scenario modeling matters more than rough intuition.


---


## A simple worked example


Assume the company has 8,000,000 fully diluted shares before the next round, plus 1,000,000 shares reserved in the option pool. It has two outstanding SAFEs:


Instrument Amount invested Key term


SAFE A $250,000 $5,000,000 valuation cap


SAFE B $250,000 $10,000,000 valuation cap


Now assume the next priced round is done at a $20,000,000 pre-money valuation.


In that scenario, SAFE A is likely to convert using the $5,000,000 cap, because that is more favorable to the investor than the round price. SAFE B is likely to convert using the $10,000,000 cap. Even though both investors put in the same amount, SAFE A receives meaningfully more shares because its cap is lower.


The founder takeaway isn’t the exact share count in the abstract. It’s that ownership depends on the full stack: existing common stock, all SAFEs, the option pool, and any pool top-up required by the new financing. If you want a more practical way to test those outcomes before a round, it helps to[model dilution scenarios in advance](https://withmantle.com/equity/planning) .


---


## How to prepare for conversion without scrambling


### Ask Clerk before you model anything


You don’t need to open five tabs to check where things stand. Drop your documents or just ask Mantle Clerk directly: which SAFEs are outstanding, what’s the current option pool, has anything changed since the last time you looked. Mantle Clerk encodes operational expertise, the mechanics, the timing, the “what happens if” reasoning that lives between formal legal questions. Legal questions still go to counsel, but the operational picture you need before a round is exactly what Clerk is built to answer.


### Let Clerk handle the closing paperwork


When the round actually closes, this is normally the worst week of the process: SPAs, side letters, six different SAFE conversions, all needing manual entry. With Clerk, you hand over the documents and it does the reconciliation conversationally. “We just closed our seed. Here’s the signed SPA and closing binder.” “Nice work! I’ve found your updated charters with 2 new share classes, 7 new shareholders, 6 SAFE conversions, and an update to your equity plan. Let’s start with the share classes and work our way down.” Instead of reconstructing the cap table from a stack of PDFs, you’re confirming what Clerk already found, one item at a time.


### Run a tie-out before the term sheet


Founders usually only discover a missing side letter during diligence, when it’s expensive to explain. Clerk runs tie-outs methodically and flags the gaps in a closing package that are easy to miss and expensive to explain. Ask it to do this before you’re deep in term sheet negotiations, so any gap gets fixed while it’s still just an email to an investor, not a diligence flag.


### Model the round, then ask Clerk to explain it


Mantle’s Term Sheet Modeler does the heavy lifting on the math. Upload the term sheet and it shows exactly how the round hits your cap table and dilution. You don’t have to interpret the output alone. Ask Clerk directly: why is the low-cap SAFE converting to more shares than the high-cap one, or what does the pool top-up actually cost the founders.


### Hand off clean, on request


Once conversion is final, you can instantly download your cap table spreadsheet to share with legal counsel, auditors, or the next round’s investors.


---


## What to look for in software that supports SAFE management


Good equity software should do three things well.


1. Maintain accurate records for SAFEs, options, and vesting in one place.
2. Update the cap table automatically as those records change.
3. Let you model financing scenarios before you commit to them.


SAFE conversion isn’t a one-line formula. It is a coordinated cap table event. Teams that keep ownership records current, track vesting and grants continuously, and test conversion outcomes before the next financing closes are the ones who aren’t scrambling the week of the deal.


---


## FAQ: SAFE Conversion


**When does a SAFE convert to equity?** Most SAFEs convert at the company’s next priced equity financing round. Some also convert, or follow a separate contract-specific path, in the event of an acquisition, IPO, or other liquidity event.


**Does a SAFE convert at the valuation cap or the round price?** Most SAFEs convert at the company’s next priced equity financing round. Some also convert, or follow a separate contract-specific path, in the event of an acquisition, IPO, or other liquidity event.


**What happens to SAFEs in a down round?** The economics can still favor the SAFE holder over new investors, depending on the cap and discount terms. A down round doesn’t automatically mean SAFE holders take less favorable pricing than the new money.


**Do multiple SAFEs convert at the same price?** No. Each SAFE converts according to its own terms. Two SAFEs for the same amount can convert into very different share counts if they carry different valuation caps.


**What is the difference between a pre-money and post-money SAFE?** A post-money SAFE fixes the investor’s ownership percentage immediately after that investment, before new priced-round money arrives. A pre-money SAFE’s final ownership impact depends on what else converts or gets added to the cap table before the round closes.


---


## Issue, e-sign, track, and convert SAFEs with Mantle


[Get started with a free account](https://equity.withmantle.com/signup?utm_source=blog&utm_medium=website&utm_campaign=safe-conversion-scenarios-every-founder-should-understand)


### Share this:


- [Share on X (Opens in new window) X](https://blog.withmantle.com/safe-conversion-scenarios-every-founder-should-understand/?share=twitter)
- [Share on LinkedIn (Opens in new window) LinkedIn](https://blog.withmantle.com/safe-conversion-scenarios-every-founder-should-understand/?share=linkedin)
-
