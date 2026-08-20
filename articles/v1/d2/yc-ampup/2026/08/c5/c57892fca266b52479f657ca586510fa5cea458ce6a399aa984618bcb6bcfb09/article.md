---
schema_version: "1.0.0"
document_id: "c57892fca266b52479f657ca586510fa5cea458ce6a399aa984618bcb6bcfb09"
company_key: "yc-ampup"
company: "AmpUp"
source_id: "yc-ampup-rss-7e069a0fe3a1"
canonical_url: "https://www.ampup.io/blog/time-of-use-ev-charging-rates"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T18:34:11.170009+00:00"
fetched_at: "2026-08-05T03:48:27.623827+00:00"
content_hash: "sha256:d8835a5ed65bdaefee25b6cfda3e612f7bb3a409ae63306c4d3f542ea17a7b5e"
---

# Time-of-Use EV Charging Rates: Price Every Hour on Your Terms

Electricity does not cost the same at 5 PM as it does at midnight, and in most of the country, your utility bill proves it. Under time-of-use (TOU) rate plans, now the default for major utilities like PG&E, SCE, and SDG&E, peak-hour power can cost[roughly twice as much](https://qmerit.com/blog/what-are-time-of-use-rates-a-guide-to-tou-for-electric-vehicle-owners/) as off-peak power, and in some territories closer to[three times as much](https://getneocharge.com/a/blog/time-of-use-rates-smart-charging-save-money-2026) . Yet many commercial charging sites still bill drivers one flat rate all day, which means every peak-hour session quietly eats into the margin.


Time-of-use charging rates close that gap. Site hosts using[AmpUp Community Manager](https://www.ampup.io/products/ev-charging-management-software) can now set exact $/kWh prices for specific time windows, and drivers see the full day's rates in the app before they plug in. This post covers how TOU pricing works, why it matters for your revenue and your local grid, and how to set it up well. If you're still comparing pricing structures more broadly, start with our guide to[EV charging pricing models](https://www.ampup.io/blog/ev-charging-pricing-models) .


## TL;DR: What Are Time-of-Use EV charging rates?


Time-of-use (TOU) EV charging rates are per-kWh prices that change based on the time of day. Site hosts set higher rates during peak utility hours and lower rates overnight or midday, so charging prices track what electricity actually costs. For commercial properties, TOU pricing delivers:


- Margin protection when utility rates spike during peak hours
- Lower prices that reward drivers for charging off-peak
- Demand shifting that eases load on your site and the grid
- Price transparency that satisfies California's CTEP requirements


## Why Flat Pricing Loses Money On A TOU Utility Plan


Utilities moved to TOU rates for a reason. In solar-heavy states like California, the grid produces abundant cheap power midday, then strains between 4 PM and 9 PM when solar output falls and demand spikes. Utilities price those evening hours steeply to push usage elsewhere, which is why shifting sessions off-peak can cut charging electricity costs by[40 to 70 percent](https://solartechonline.com/blog/electric-time-of-use-rates-guide/) .


Now put a busy charging station on that meter with one flat price. Every session that runs through the evening peak costs you more to serve than the same session at 10 PM, but earns exactly the same revenue. Multiply that across ports and months, and flat pricing becomes a subsidy your property pays so drivers can charge at the most expensive hour of the day.


TOU charging rates flip that. Price the 4 to 9 PM window to reflect your real costs, discount the overnight and midday windows, and drivers respond. The sessions that can move, move. The ones that can't, pay their way.


## What's New in Community Manager: Dollars, not Decimals


TOU pricing in AmpUp isn't new, but until now hosts configured it as percentage adjustments layered on a base rate. Feedback from site operators was consistent: percentages made it hard to know what drivers would actually pay. The updated feature removes the math entirely.


- **Direct $/kWh entry.** Click "Add Time of Use Adjustments," define a time window and days, and type the exact energy rate for that window. No more percentage conversions.
- **Clean pairing with energy-based pricing.** TOU adjustments apply to fixed energy plans, so the per-kWh price is always explicit. Tiered and time-based pricing no longer stack with TOU, which keeps every rate unambiguous.
- **A 24-hour price timeline for drivers.** The[AmpUp driver app](https://www.ampup.io/products/ev-charging-app-for-drivers) displays the full midnight-to-midnight rate schedule for the station, with the active price band highlighted. Drivers know the rate before they tap start, and they can see exactly when a cheaper window opens.


## Transparency Isn't Optional in California


California's Type Evaluation Program (CTEP) sets weights-and-measures rules for commercial EV charging, and pricing transparency is central to it: stations that use multi-tiered pricing must make[every tier's price visible before a session begins](https://chargelab.co/blog/what-is-ctep) . A TOU schedule drivers can't see doesn't just frustrate them. In California, it's a compliance problem.


That's why the driver-side timeline matters as much as the host-side controls. Every rate window a host configures in Community Manager is automatically published to the app, satisfying the disclosure requirement with zero extra effort. AmpUp's platform is[CTEP certified across five charging hardware brands](https://chargedevs.com/newswire/ampups-ev-charging-management-software-earns-ctep-certification-for-five-charging-hardware-brands/) , so hosts get compliant TOU pricing without being locked into a single manufacturer.


Not every network takes this approach. Some platforms support time-varying rates but bury the schedule, leaving drivers to discover the peak price on their receipt. Others only offer duration-based tiers (one price for the first hour, another after), which manages dwell time but does nothing to align prices with utility costs. Direct $/kWh windows plus a full-day driver timeline is the combination that does both.


## How to Set TOU Windows


A good TOU schedule starts with your utility tariff. Pull your rate plan, note the peak, off-peak, and any shoulder windows, and mirror them in Community Manager with margins that hold in every window. From there:


- **Make off-peak a real incentive.** A token discount won't move behavior. The gap between your peak and off-peak price is what shifts sessions.
- **Match windows to your parking patterns.** A workplace can price midday attractively when solar power is cheap; a multifamily property can reward overnight charging.
- **Revisit quarterly.** Utilities adjust TOU tariffs seasonally, and your prices should follow.


On the AmpUp Pro plan, the[Pricing Recommendation Engine](https://www.ampup.io/products/ev-charging-management-software) does the benchmarking for you, combining utility rates, demand charges, and competitor data into rate suggestions with a Pricing Health Scorecard.


## Time-of-Use Charging FAQs


### What is time-of-use pricing for EV charging?


Time-of-use pricing means the per-kWh price of a charging session depends on when it happens. Site hosts set higher rates during peak utility hours and lower rates off-peak, so charging prices follow the real cost of electricity.


### Do drivers see TOU rates before they charge?


On AmpUp, yes. The driver app shows a 24-hour price timeline for every station with TOU pricing, with the current rate highlighted. This upfront disclosure is also what California's CTEP rules require for multi-tiered pricing.


### Does TOU pricing work with every pricing model?


In AmpUp Community Manager, TOU adjustments apply to fixed energy ($/kWh) pricing. That keeps every posted rate exact and unambiguous, which is better for drivers and required in some states.


## Price Every Hour on Your Terms


Your utility already prices electricity by the hour. TOU charging rates let your stations do the same, protecting margins at peak, rewarding drivers off-peak, and keeping every price in plain sight. It takes minutes to configure in Community Manager.


[Schedule a demo](https://www.ampup.io/request-a-demo) to see kWh-based TOU pricing in action, or[request a site assessment](https://www.ampup.io/ev-charging-site-assessment) to get started.
