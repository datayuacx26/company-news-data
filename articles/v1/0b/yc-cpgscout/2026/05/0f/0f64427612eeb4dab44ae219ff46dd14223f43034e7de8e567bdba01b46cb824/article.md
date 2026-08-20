---
schema_version: "1.0.0"
document_id: "0f64427612eeb4dab44ae219ff46dd14223f43034e7de8e567bdba01b46cb824"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/trade-spend-roi"
published_at: "2026-05-14T00:00:00+00:00"
first_seen_at: "2026-07-22T12:52:59.406044+00:00"
fetched_at: "2026-07-28T21:44:24.455372+00:00"
content_hash: "sha256:daf71b8e9bd01009331b53f0115d5859d00649783155c2236ca3c2ae079ef02a"
---

# Trade Spend ROI: How to Calculate It, and Pitfalls

[← Back to Blog](https://www.cpgscout.ai/blog)


A finance analyst at a mid-market RTD coffee brand has a recap deck open and a buyer meeting in two days. The deck says last quarter's Kroger feature-and-display event returned a[trade spend](https://www.cpgscout.ai/glossary/trade-marketing) ROI of 1.8: every dollar of trade spend came back as $1.80 in profit. Her VP of Sales wants to repeat it at Albertsons and Safeway. But she's run the post-promo weeks herself, and the brand's baseline at Kroger sat below its pre-promo level for almost two months afterward. The 1.8 doesn't feel like the whole story. It isn't.


This is the most common failure in trade promotion finance: a number that is arithmetically correct and decision-wrong. The formula is not hard. The honesty is. This guide walks through how trade spend ROI gets calculated, what each input actually means, and the three structural reasons the headline figure overstates returns, then rebuilds the RTD coffee event from baseline units up so you can see the gap for yourself. For the forward-looking version of this,[how to forecast trade spend ROI](https://www.cpgscout.ai/blog/forecast-trade-spend-roi-for-promotions) covers sizing an event before it runs; this post is about reading one after.


The uncomfortable finding first: across the promotion sets we've analyzed with mid-market CPG brands, a large share of events return below 1.0 once cannibalization and the post-promo dip are netted out. They lose money. And many of those same events show a headline ROI well above 1.0. The arithmetic below is how that happens.


## Key takeaways


- ROI = incremental margin / trade spend; the trap is the numerator — only truly incremental units count.
- Headline ROI usually overstates by 30%+ because it counts non-incremental units and ignores forward-buying and cannibalization.
- A worked feature-and-display example shows how the honest number falls out.
- Many events land near or below break-even on a strictly incremental basis.


## The trade spend ROI formula and its inputs


Trade spend ROI is one division:


Trade Spend ROI = Incremental Profit / Trade Spend


An ROI of 1.0 is break-even: the promotion returned exactly the trade dollars it consumed. Above 1.0 the event made money; below 1.0 it destroyed it. Some teams express the same idea as ROMI or a payback ratio, but the structure is identical. The difficulty isn't the division. It's that every term on the right-hand side is a modeling decision, and small choices in each one swing the answer by half a turn or more.


Input Definition Where teams get it wrong


Baseline units Units that would have sold with no promotion Using last year's same weeks instead of a modeled counterfactual


Promo units Total units sold during the promo window Including stores that never executed the display


Incremental units Promo units minus baseline units Forgetting to subtract baseline at all


Incremental revenue Incremental units times promo price Using full shelf price instead of the discounted price


Incremental cost COGS on the incremental units Omitting freight, slotting, or returns


Trade spend All dollars paid to drive the event Counting only the per-unit allowance, not display fees and scan-downs


Two inputs deserve emphasis. Trade spend is rarely a single line. For a feature-and-display event it usually bundles a per-unit scan-down allowance, a flat display or ad fee paid to the retailer, and sometimes a slotting or off-invoice component. A common error at Kroger and Albertsons in particular is to total only the scan-downs the syndicated data shows and miss the flat ad fee sitting in a separate accrual. That one omission can understate trade spend by 20-40% and inflate the ROI to match.


Incremental cost is the other quiet one. It's the cost of goods on the incremental units, not on total promo units; you only pay incremental COGS on the units the promotion actually caused. But it should include freight and any promotion-specific costs (custom shipper packaging, returns on perishable RTD inventory) that a clean shelf week wouldn't carry.


## What is actually incremental


Every unit sold during a promotion is one of two things: baseline or incremental. Baseline units would have sold anyway; loyal Whole Foods shoppers who buy your cold brew every week do not need a $1 scan-down to do it. Incremental units exist only because the promotion ran. Trade spend ROI is a question about the incremental units alone, because the discount on baseline units is pure margin you handed to people who were already buying.


This is why the baseline number is the single most important input, and the easiest to fudge. Set the baseline too low and every unit above it counts as incremental, incremental profit balloons, and ROI looks great. The honest baseline is a modeled counterfactual: recent non-promoted velocity, adjusted for seasonality, distribution (ACV) changes, price changes that landed just before the event, and known disruptions like a stockout. It is not last year's same weeks, and it is not the four quiet weeks right before the promo if those weeks were themselves depressed by a competitor's event.


Three behaviors then chip away at how much of the headline increment is real, and each one is covered in the section on why ROI misleads:


- Cannibalization: the promoted SKU steals volume from your other SKUs on the same shelf.
- The post-promo dip: loyalists who pantry-loaded during the event buy nothing for weeks afterward.
- Forward-buying: the retailer buys deep at the discounted cost and sells it through later at full margin, so your shipment spike is not consumer demand at all.


A genuinely incremental unit is one bought by a shopper who would not otherwise have bought your brand, in a quantity they would not otherwise have bought, in a week they would not otherwise have bought it. Most promo-week volume fails at least one of those tests. The job of an honest calculation is to find the units that pass all three.


## A worked calculation: an RTD coffee feature-and-display event


Take the RTD coffee brand from the opening. The event: a four-week feature-and-display on its 11 oz oat-milk cold brew, single retailer (Kroger), 220 stores, priced down from a $3.49 shelf price to $2.79. The trade spend has three parts: a $0.55-per-unit scan-down allowance, a $9,000 flat display-and-ad fee, and nothing else. Here is the full calculation as most recap decks would present it.


Line Value How it is derived


Baseline units (4 wks) 48,000 12,000/wk modeled non-promo velocity


Promo units (4 wks) 92,000 23,000/wk actual scanned sales


Incremental units 44,000 92,000 - 48,000


Promo price $2.79 Discounted shelf price


Incremental revenue $122,760 44,000 x $2.79


Incremental COGS $57,200 44,000 x $1.30 unit cost


Trade spend: scan-downs $50,600 92,000 units x $0.55 allowance


Trade spend: display/ad fee $9,000 Flat fee paid to Kroger


Total trade spend $59,600 $50,600 + $9,000


Incremental gross profit $65,560 $122,760 − $57,200


Net profit after trade spend $5,960 $65,560 − $59,600


Headline trade spend ROI 1.10 $65,560 / $59,600


Two details in that table catch teams out. First, the scan-down allowance is paid on all 92,000 units sold during the event, not just the 44,000 incremental ones; the retailer scans every discounted unit, the baseline ones included, and bills you for each. Second, incremental COGS applies only to the 44,000 incremental units, because the baseline units' cost is already in the base business. Get those two backwards and the ROI can move by a full point in either direction.


So the headline says 1.10, a thin win. Now net out the post-promo dip. In the eight weeks after the event, the brand's Kroger baseline ran at roughly 10,400 units/week against the 12,000/week pre-promo baseline. That's about 1,600 lost units per week for eight weeks, or 12,800 units that loyalists pulled forward into the promo window and then didn't buy afterward.


Those 12,800 units were never incremental. They were baseline sales relocated in time, and they were sold at the $2.79 promo price instead of the $3.49 shelf price. The correction has two parts: remove them from the incremental count, and recognize the margin lost by selling that borrowed volume on deal.


Line Headline view Net-of-dip view


Incremental units 44,000 31,200


Incremental revenue $122,760 $87,048


Incremental COGS $57,200 $40,560


Margin lost on pulled-forward units $0 $8,960


Incremental gross profit $65,560 $37,528


Net profit after trade spend $5,960 −$22,072


Trade spend ROI 1.10 0.63


Same event, same scanned data. The headline ROI cleared the 1.0 break-even line; the net-of-dip ROI did not.


The margin-lost line is 12,800 pulled-forward units times the $0.70 gap between the $3.49 shelf price and the $2.79 promo price. The brand sold volume on deal that it would have sold at full price anyway. Net of the dip, the event did not return 1.10. It returned 0.63, below the 1.0 break-even line, which means it lost money. After trade spend, that's roughly negative $22,000 of profit on $59,600 of spend. Same Kroger event, same scanned data, one read a win and the other a loss. The recap deck showed the first one.


ROI calculator


Plug in your numbers — the estimate updates live.


Annual revenue $


Doors


Out-of-stock rate 8%


Trade spend (% of revenue) 15%


Estimated annual upside with Scout


$220,435


≈ $441 back per door, per year


Recovered out-of-stock sales$130,435


Trade spend recovered$90,000


Email me this breakdown


Get your numbers as a shareable report, plus how brands your size close the same gaps.


[Or see your number in a 15-min demo](https://calendar.app.google/mmFRwxfQuV8CYeuC6)


Estimate only, from the inputs above and deliberately conservative assumptions (30% of out-of-stock losses recovered, 12% of trade spend freed). Not a guarantee of results.


## Why headline trade spend ROI misleads


The RTD coffee example shows one of three structural reasons the headline number runs high. All three push the same direction, inflating ROI, which is why naive trade spend ROI is biased optimistic, not just noisy.


### The post-promo dip and pantry loading


This is the one quantified above. A shopper who buys four units of cold brew on deal doesn't buy more coffee for a month. The promo-week scan looks like demand; it's partly inventory transfer into the shopper's pantry. If your analysis window stops the day the promotion ends, you book all of the pull-forward as incremental and none of the dip as a cost. An eight-week post-window is the standard correction. In the RTD example it moved ROI from 1.10 to 0.63, from a thin win to a clear loss.


### Cannibalization across your own SKUs


The RTD brand promoted only its 11 oz oat-milk cold brew. But it also sells a 14 oz size and a dairy latte on the same Kroger shelf. If the feature pulled shoppers off those siblings (a 14 oz buyer trading down to the discounted 11 oz) then some of the 44,000 "incremental" units are not incremental to the brand at all. They're cannibalized. The fix is to compute ROI at the brand or category level alongside the SKU level: if the promoted SKU lifted 44,000 but the rest of the line fell 9,000, true brand-incremental units are 35,000, not 44,000. SKU-level ROI flatters; brand-level ROI corrects.


### Forward-buying by the retailer


This one corrupts shipment-based ROI rather than POS-based ROI. When you offer an off-invoice allowance, the retailer's buyer can order far more than the promotion will sell, warehouse the difference, and ring it through at full margin after the deal ends. Your shipment data spikes; consumer offtake doesn't. A trade spend ROI built on shipments will count forward-bought cases as[incremental sales](https://www.cpgscout.ai/glossary/incremental-sales) . Costco and large Albertsons divisions are structurally set up to do this well. The defense is to build ROI on retailer POS or syndicated scan data, what shoppers actually bought, not on what you shipped.


### Gross versus net ROI


Tying it together: gross trade spend ROI uses headline incremental units and ignores the dip, cannibalization, and forward-buying. Net trade spend ROI strips all three out. The RTD event was 1.10 gross and 0.63 net. Gross ROI isn't useless, it's a fast directional read, but it should never be the number in a repeat-or-kill decision. When a deck quotes an ROI without saying gross or net, assume gross, and assume it's high.


## Realistic trade spend ROI benchmarks


The single most useful thing to internalize about trade spend ROI benchmarks: a large share of CPG promotions return below 1.0 once everything is netted out. They lose money. This is not a sign of a broken brand. It is the base rate, and it has been documented across the industry for years. Plenty of widely cited analyses put the share of trade promotions that fail to break even somewhere around half or more.


Rough orientation for a net trade spend ROI, after the dip and cannibalization corrections:


Net ROI Read


Below 1.0 Lost money. Common, likely the plurality of events.


1.0 to 1.5 Modest win. Worth repeating, worth tuning depth.


1.5 to 2.5 Strong. Usually a well-targeted mechanic at the right retailer.


Above 2.5 Rare on a net basis. Audit the baseline before celebrating.


Two cautions on benchmarks. First, they're category- and retailer-specific. A feature at Sprouts or Whole Foods on a premium natural SKU behaves nothing like an everyday-low-price account, and an RTD coffee event behaves nothing like shelf-stable center-store. Treat the table as orientation, not a target. Second, be suspicious of any net ROI above 2.5. In our experience the most common cause is not a brilliant promotion but a baseline set too low, which manufactures incremental units out of thin air. When the number looks too good, re-audit the counterfactual before you take it to the buyer. The deeper point, that you should run the whole trade budget as a portfolio of these returns, is the subject of[trade spend optimization](https://www.cpgscout.ai/blog/trade-spend-optimization) .


## Doing this in Scout


Everything above is doable in a spreadsheet for one event. The work is reconciling the inputs and carrying the post-promo window for every event, every retailer, every cycle. That's what Scout is built to take off your plate.


[Scout](https://www.cpgscout.ai/) runs on SPINS and retail syndicated data and computes trade spend ROI both gross and net: modeling the baseline counterfactual, carrying the eight-week post-promo window automatically, and reconciling scan-down allowances against the flat display and ad fees so the trade spend denominator is complete. It computes ROI at the SKU level and the brand level side by side, so the cannibalization gap is visible rather than buried. The RTD coffee event in this post would surface as 1.10 gross and 0.63 net in the same view, not in two separate analyses a month apart.


It won't invent a number the data can't support; a baseline is still a model, and panel-level questions like new-household acquisition still need panel data. What Scout removes is the manual reconciliation and the temptation to stop the clock at promo end. To see how that closes the loop with pre-event planning,[how to forecast trade spend ROI](https://www.cpgscout.ai/blog/forecast-trade-spend-roi-for-promotions) is the companion read on the forecasting side. The[ROI calculator](https://www.cpgscout.ai/roi-calculator) embedded above is also a standalone page you can share, with its assumptions spelled out.


## Summary and further reading


- Trade spend ROI is incremental profit divided by trade spend. Every input is a modeling decision, and the baseline is the one that moves the answer most.
- Only incremental units matter: units a shopper would not otherwise have bought, in that quantity, in that week. Most promo-week volume is baseline in disguise.
- Headline ROI overstates returns for three structural reasons: the post-promo dip, cannibalization across your own SKUs, and retailer forward-buying. All three bias the number high.
- In the worked RTD coffee example, a Kroger feature-and-display event read 1.10 gross and 0.63 net once 12,800 pulled-forward units were corrected out.
- A large share of CPG promotions return below 1.0 on a net basis. Be especially suspicious of any net ROI above 2.5; re-audit the baseline first.
- Always quote ROI as gross or net. A repeat-or-kill decision should only ever use the net figure.


For the bigger picture:[trade spend](https://www.cpgscout.ai/blog/trade-spend) is the pillar guide to the whole discipline,[trade spend optimization](https://www.cpgscout.ai/blog/trade-spend-optimization) covers allocating the budget as a portfolio across retailers and mechanics, and[how to forecast trade spend ROI](https://www.cpgscout.ai/blog/forecast-trade-spend-roi-for-promotions) is the forward-looking companion to this retrospective calculation. Questions on any of it? Reach us athello@cpgscout.ai .


## Frequently asked questions


What is the trade spend ROI formula?Incremental margin generated divided by trade spend invested. The trap is the numerator: only margin from truly incremental units counts, not total promoted sales. It is defined the same way as any[return on investment](https://www.investopedia.com/terms/r/returnoninvestment.asp) .


Why does headline ROI overstate returns?Because it usually counts non-incremental units (sales that would have happened anyway) and ignores forward-buying and cannibalization. Correcting for those routinely cuts the headline number by 30% or more.


What is a realistic trade spend ROI benchmark?It varies by category and tactic, but many feature-and-display events land near or below break-even on a strictly incremental basis. Test your own events with the[ROI calculator](https://www.cpgscout.ai/roi-calculator) .


## See this on your own data


Scout gives CPG sales teams the analytics infrastructure they need — without spreadsheets.


[Get a 15-min demo](https://calendar.app.google/mmFRwxfQuV8CYeuC6)


## Get posts like this in your inbox


Retail data, trade promotion, and category management for CPG teams. Roughly weekly. Unsubscribe any time.
