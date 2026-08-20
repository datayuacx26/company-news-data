---
schema_version: "1.0.0"
document_id: "fecb070103f3ca86898f112c8f7da4498fc86286408a1d56e3122d643669078e"
company_key: "yc-warp"
company: "Warp"
source_id: "yc-warp-news-import-3eac4f975b78"
canonical_url: "https://www.warp.co/blog/y-combinator-safe-explained"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T18:38:25.905729+00:00"
fetched_at: "2026-07-31T18:38:27.158975+00:00"
content_hash: "sha256:6a95cf631e4221a35e90d1bbfde4df312b3b8a90859a0d518ecafb29a00091ee"
---

# Y Combinator SAFE Explained: Terms, Mechanics & Dilution

**TL;DR:** A Y Combinator SAFE (Simple Agreement for Future Equity) is a contract that lets an investor invest in your startup now in exchange for equity later, typically when you raise a priced round. It's not a loan: there's no interest, no maturity date, and no fixed valuation at signing. The terms that matter are the valuation cap and the discount rate, since those two numbers determine how much of your company an early SAFE investor ends up owning once it converts. Below, we break down how YC's SAFE actually works and walk through the math on a real example. You can also use our[equity dilution calculator](https://www.warp.co/tools/equity-dilution-calculator) to model your own round instead of guessing.


If you're a first-time founder about to raise a pre-seed or seed round, you'll almost certainly be asked to sign one of these. Y Combinator introduced the SAFE in 2013 as a faster, cheaper alternative to convertible notes, and it's since become the default instrument for early-stage US fundraising: SAFEs made up roughly[86% of pre-seed deals](https://carta.com/data/state-of-pre-seed-2025-full-report/) in 2025 and close to[two-thirds of seed deals](https://carta.com/data/pre-seed-and-seed-safes-q3-2024/) in 2024.


## What Is a Y Combinator SAFE?


A SAFE is short for Simple Agreement for Future Equity. It's a contract between a startup and an investor in which the investor hands over cash today, and in exchange, the startup promises to issue shares at a later date, once a specific triggering event occurs (usually your next priced equity round).


The important thing to understand is what a SAFE is *not* . It's not debt. There's no interest rate accruing in the background, and there's no maturity date forcing repayment if things go sideways. Legally, a SAFE behaves more like a warrant: the investor is buying the right to future equity, not a loan they expect to be repaid with interest.


That structure is exactly why YC built it. Before 2013, early-stage rounds mostly ran on convertible notes, which carry interest rates, maturity dates, and (because they're debt) more legal negotiation. YC's law firm at the time wanted something a founder and an angel investor could sign in an afternoon without a lawyer on either side re-drafting the term sheet. The SAFE cut the negotiable terms down to essentially two: the valuation cap and the discount rate.


## Pre-Money vs. Post-Money: The Type of SAFE Y Combinator Uses Today


YC has actually shipped two different versions of the SAFE, and mixing them up is one of the most common sources of confusion.


The original 2013 SAFE was a **pre-money SAFE** . Its ownership math depended on the size of the company's pre-money share count at the time of conversion, which meant a founder stacking multiple SAFEs from different investors couldn't tell any single investor their exact ownership percentage until every SAFE in the round converted at once.


In 2018, YC replaced it with the **post-money SAFE** , which is the standard document YC issues today. The post-money SAFE fixes the ownership percentage an investor buys at the time of investment, calculated against the company's valuation immediately after that SAFE is added to the cap table. If you're raising a SAFE round in 2026, this is almost certainly the version you're signing: it's the one available on Y Combinator's own documents page, and it's what most lawyers and platforms default to now.


The practical difference matters for founders because post-money SAFEs make dilution easier to calculate per SAFE, but harder to eyeball across an entire round. Stack five post-money SAFEs at different caps, and you can't just add up the percentages in your head. This is one of the reasons founders underestimate how diluted they'll be by the time a priced round actually closes.


## The Terms You're Actually Negotiating


A SAFE strips fundraising down to a short list of variables. Here's what each one does:


TERM WHAT IT MEANS


Valuation Cap The maximum company valuation used to calculate the investor's conversion price. A lower cap means the investor converts at a lower effective price, and gets more shares per dollar invested.


Discount Rate A percentage discount off the price per share paid by investors in your next priced round. A 20% discount means the SAFE holder pays 80 cents on the dollar relative to new investors.


Most Favored Nation (MFN) A clause that lets an earlier SAFE investor claim the better terms if you later issue a SAFE with a lower cap or bigger discount. Common in "uncapped" SAFE rounds.


Pro Rata Rights The right (not obligation) for the investor to participate in your next priced round to maintain their ownership percentage. YC's post-money SAFE includes an optional pro rata side letter.


Conversion Trigger The event that converts the SAFE into actual shares, typically your next priced equity round, but can also include an acquisition or IPO.


Some SAFEs carry a valuation cap only, some carry a discount only, and some carry both, with whichever term is more favorable to the investor applying at conversion. Uncapped SAFEs (no valuation cap at all) show up occasionally, usually paired with an MFN clause, but they're riskier for founders because there's no ceiling on how much of the company an investor ends up owning if your valuation jumps before the SAFE converts.


## How a SAFE Converts Into Equity


A SAFE sits on your cap table as a liability of sorts (though not literally debt) until a trigger event fires. The most common trigger by far is your next priced equity round, typically a seed or Series A, where a new investor sets an actual valuation for the company.


At that point, every outstanding SAFE converts based on whichever is more favorable to the investor: the valuation cap or the discount rate against the new round's price. The SAFE holder receives shares of the same class being issued in that round (or a shadow series in some cases), and that's when their ownership stake becomes real and diluting to everyone else on the cap table, including the founders.


Less commonly, a SAFE can also convert on an acquisition or IPO before a priced round ever happens, in which case it typically converts into common stock or pays out the greater of the investment amount or the as-converted value.


## What a SAFE Actually Costs You in Dilution: A Worked Example


Say your startup raises $500,000 on a SAFE with a $5 million valuation cap and no discount. Twelve months later, you raise a $3 million Series A at a $15 million pre-money valuation, with 10 million shares outstanding pre-round. (That's an $18 million post-money valuation for the round, worth noting since most dilution calculators, including Warp's, ask for post-money rather than pre-money when you model a funding round.)


- Series A price per share: $15,000,000 ÷ 10,000,000 shares = $1.50/share
- SAFE investor's capped price per share: $5,000,000 cap ÷ 10,000,000 shares = $0.50/share
- Since $0.50 is lower than the Series A price, the SAFE converts at the capped price
- Shares issued to the SAFE holder: $500,000 ÷ $0.50 = 1,000,000 shares


That SAFE investor now owns roughly 8.3% of a fully diluted cap table that also has to absorb the new Series A shares and any option pool top-up, on top of whatever the founders already gave up. Run that same math across three or four SAFEs at different caps from a pre-seed round, add in a 10% option pool refresh most Series A leads require, and founders are frequently surprised by just how much of the company is gone before the Series A ink is even dry.


Instead of doing this math by hand or guessing, plug your own numbers, cap table, and round terms into Warp's[equity dilution calculator](https://www.warp.co/tools/equity-dilution-calculator) to see the exact post-round ownership percentages for every stakeholder, SAFE holders included.


## SAFE vs. Convertible Note


Founders sometimes hear the term "convertible security" and assume that SAFEs and convertible notes are interchangeable. They're not.


SAFE Convertible Note


Legal structure Warrant-like, not debt Debt instrument


Interest None Typically accrues (often 4-8%)


Maturity date None Yes, forces a conversion or repayment decision


Negotiable terms Valuation cap, discount Cap, discount, interest rate, maturity, covenants


Typical close time Days Often weeks, more legal review


The SAFE's appeal is speed and simplicity. The tradeoff is that investors get fewer formal protections (no maturity date forcing a resolution, no interest compensating them for time), which is part of why SAFEs took over the earliest stage of startup fundraising almost entirely.


## Common Mistakes Founders Make With SAFEs


**Stacking uncapped SAFEs without modeling the outcome.** Every uncapped or high-cap SAFE feels harmless in isolation. Stacked together across a pre-seed round, they can quietly hand away 20-30% of the company before a priced round ever sets an actual valuation. Our[founder's guide to startup equity dilution](https://www.warp.co/blog/startup-equity-dilution-what-founders-need-to-know-before-their-next-round) walks through how dilution compounds across SAFEs, option pools, and priced rounds.


**Not accounting for the option pool shuffle.** Series A investors frequently require you to expand the option pool before their round, and that dilution comes out of existing shareholders, including your SAFE holders and you.


**Treating the valuation cap as the company's actual valuation.** A $10 million cap is a ceiling for conversion math, not a valuation anyone agreed the company is worth. Using it as a talking point with later investors is a common and avoidable mistake.


**Raising a SAFE round with no runway model behind it.** How much you raise on SAFEs should be tied to how long that money needs to last. If you haven't mapped your monthly burn against the raise, our[startup burn rate guide](https://www.warp.co/blog/burn-rate-startup-guide) is a good place to start before you finalize the round size.


## Where to Get the Official YC SAFE Documents


Y Combinator publishes the current post-money SAFE templates, along with a user guide explaining each version, directly on its[SAFE financing documents page](https://www.ycombinator.com/documents) . That's the canonical source for the actual legal language. Everything above is meant to help you understand what you're signing and what it costs you, not to replace legal review of the specific document your investor sends over.


## Frequently Asked Questions


### **Is a SAFE the same as equity?**


Not immediately. A SAFE is a promise of future equity, not equity itself. You don't get actual shares (and the investor doesn't get actual ownership) until a trigger event, like your next priced round, causes the SAFE to convert.


### **What's the difference between a pre-money and post-money SAFE?**


A pre-money SAFE calculates the investor's ownership based on the company's valuation before the round; a post-money SAFE fixes it based on valuation immediately after that SAFE is added. YC has used the post-money version as its standard since 2018, and it's the type of SAFE most founders sign today.


### **Do SAFEs have a valuation?**


Not a fixed one. Most carry a valuation cap, which sets a ceiling on the conversion price, but that's different from the company having an agreed-upon valuation, as it would in a priced round.


### **Can a SAFE convert without a new funding round?**


Yes, in less common cases. Most YC SAFEs include conversion triggers for an acquisition or IPO, in addition to a priced equity round, though the priced round is by far the most common trigger in practice.


### **How much equity does a SAFE investor end up with?**


It depends entirely on the valuation cap or discount rate relative to the terms of your next priced round. There's no fixed percentage; it's a function of the cap, the discount, and the eventual round size. Modeling it with a[dilution calculator](https://www.warp.co/tools/equity-dilution-calculator) before you sign is the only reliable way to know for sure.


### **Do SAFEs need a lawyer?**


YC's standard SAFE is designed to be signable without extensive legal negotiation, which is part of its appeal. That said, it's still worth having counsel review the specific terms (especially cap size, discount, and any MFN or pro rata provisions) before you sign, particularly if you're stacking multiple SAFEs in one round.


---


*This article is for informational purposes and isn't legal or financial advice. Review your SAFE terms with an attorney before signing, as state-level securities requirements and deal-specific terms can vary.*
