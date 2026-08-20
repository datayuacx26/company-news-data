---
schema_version: "1.0.0"
document_id: "c7efa89f695367ccc591df9bcc62a4263dcc7afc2733a0454f9fc81da6a8ea3c"
company_key: "yc-rally-3"
company: "Rally"
source_id: "yc-rally-3-news-import-75685e7d2a3f"
canonical_url: "https://www.getrally.com/blog/company-car-home-charging-reimbursement-germany-2026"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-22T10:49:14.710560+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:94ab4da280ff69a539d181c67241d3c8289670fd37e58a0b88768139630c6f3d"
---

# Home Charging Reimbursement for German Company Cars in 2026

For decades, paying for a company car's energy in Germany was simple. The driver stopped at a petrol station, tapped a[fleet fuel card](https://www.getrally.com/fuel-cards) , and the cost flowed onto a single monthly invoice that finance could reconcile against the trip log. Mileage was visible. VAT was clean. The Steuerberater nodded along.


Electric vehicles broke that workflow in one direction nobody fully anticipated: most charging happens at home. A German company-car driver with a Wallbox in the garage will run 60–80% of their kWh through their private electricity meter, paid out of their personal household account. The employer never sees the transaction.


That single change has pulled three teams into the mess. Finance needs proof of the kWh consumed for VAT and corporate tax. HR needs the reimbursement on the right payroll line so it isn't taxed as wages. The driver needs to be paid back without filling out a spreadsheet every Friday night. And the BMF (Bundesministerium der Finanzen) has tightened its documentation expectations.


This guide explains the 2026 framework, the two methods you can use, and how a modern[fleet EV charging card](https://www.getrally.com/ev-charging) plus a clean payroll workflow keeps the Finanzamt happy without burying your team in receipts.


## Why Home Charging Reimbursement Got So Messy


In the diesel-and-petrol era, fleet finance had it easy. One card, one invoice, one VAT line per fuelling. The driver never paid out of pocket; the employer never had to ask where the energy came from. The whole process was designed around a forecourt transaction.


Electric company cars change three things at once:


- **The transaction moves into the driver's home.** The household electricity meter records both the family fridge and the company car. Without isolation, the employer can't tell which kWh belongs on the corporate invoice.
- **The reimbursement crosses a tax line.** Paying an employee for "energy used on company business" is not the same as paying them wages. If documented correctly, it's tax-free under § 3 Nr. 46 EStG. If documented sloppily, the entire amount can be re-classified as taxable income.
- **The data is split across systems.** Public charging flows through a fleet card. Home charging flows through the driver's domestic energy contract. Workplace charging flows through the employer's own utility bill. Without a consolidation layer, finance ends up matching three datasets by hand.


That's why home-charging reimbursement has become one of the most asked questions in German fleet WhatsApp groups and the most common topic raised in quarterly Steuerberater reviews. For broader context on the shift to electrified fleets, see our[fleet manager's guide to EV charging](https://www.getrally.com/blog/a-fleet-managers-guide-to-electric-car-charging-cards-in-europe) . The good news: the BMF has provided a framework, and the operational tooling has caught up.


## The 2026 BMF Framework in Plain Language


Germany handles employer reimbursement of company-car electricity through two complementary rules:


- **§ 3 Nr. 46 EStG** — a specific tax exemption for "elektrische Aufladung" of company cars at the employer's premises and for the temporary use of charging equipment. This is the legal anchor for paying drivers tax-free for charging an electric or plug-in hybrid company car.
- **Auslagenersatz** — the general principle that an employer can reimburse an employee for costs incurred on the employer's behalf without creating taxable wages, provided the cost and purpose are documented.


On top of those statutes, the BMF issued a clarifying letter — commonly cited as the "BMF-Schreiben vom 29. September 2020" and updated in subsequent BMF guidance — that gives employers two practical ways to handle home charging:


1. A **flat-rate (Pauschale)** monthly amount the employer pays tax-free without requiring detailed receipts.
2. An **actual-cost** reimbursement based on measured kWh and the driver's verified household electricity tariff.


Both methods are tax-free for the employee within the published limits. Both treat home charging of the company car as a business cost, not a benefit-in-kind. And both require the employer to document which method has been chosen and to retain the supporting evidence for the standard German retention period.


Because the BMF figures have been revisited several times and may change again, treat the numbers in the next section as illustrative of the current framework and verify the live values with your Steuerberater before locking them into policy. This guide is general information, not tax advice.


## The Flat-Rate (Pauschale) Method


The Pauschale is the easier path for most fleets. The employer pays a fixed monthly amount per company-car driver who charges at home, the payment is tax-free up to the BMF cap, and there is no requirement for the driver to submit kWh logs.


Under the most recent BMF guidance, the flat-rate amounts currently sit at the following levels:


Charging situation Battery electric vehicle (BEV) Plug-in hybrid (PHEV)


**Employer also provides workplace charging** ~€30 / month ~€15 / month


**No workplace charging available** ~€70 / month ~€35 / month


The logic is straightforward: if the employee already has a free charging option at the office, they're using their home Wallbox less, so the Pauschale is lower. If there's no workplace charger, home charging is doing most of the work and the Pauschale rises accordingly.


Documentation is minimal:


- A written election by the employee confirming they charge the company car at home and which Pauschale tier applies.
- A copy in the payroll file showing the recurring tax-free line item.
- A short policy document referencing § 3 Nr. 46 EStG and the relevant BMF letter.


The trade-off is simple. The Pauschale is fast to administer but caps the reimbursement. If a driver runs a high-mileage EV that mostly home-charges, the real electricity cost can exceed €70 per month — sometimes by a wide margin. In that case, the actual-cost method becomes the better economic answer.


Verify the live figures with your Steuerberater before publishing them in employment contracts. The BMF has revised these numbers in the past and is expected to keep them under review.


## The Actual-Cost Method


When the Pauschale leaves money on the table, the BMF allows employers to reimburse the documented, measured cost of home charging instead. The mechanics are stricter, but the ceiling is higher.


To use the actual-cost method, the employer needs three things:


1. **A way to isolate the company-car kWh from household consumption.** A separate sub-meter on the Wallbox circuit is the cleanest option. A smart Wallbox that logs each charging session with timestamps, kWh, and a vehicle identifier is the modern equivalent and is generally accepted by the Finanzamt.
2. **A verified electricity tariff.** Most fleets ask the driver to submit their annual electricity invoice (Stromabrechnung). The cent-per-kWh figure from that invoice is used to value the metered kWh.
3. **A monthly reconciliation.** Either the driver exports the smart-charger log or the sub-meter is read each period, and payroll pays out kWh × tariff as tax-free Auslagenersatz.


For a driver pulling 350 kWh per month at €0.32 per kWh, that works out to roughly €112 per month — well above the €70 Pauschale and entirely tax-free when properly documented.


The administrative cost is real. Someone has to ingest the wallbox logs, match them to the right driver, apply the tariff, and produce an audit-ready monthly statement. Where this is worth it:


- Long-distance commuters and field engineers running BEVs.
- Fleets with employer-provided wallboxes installed at the driver's home, because the smart logging is already in place.
- Companies whose Steuerberater wants a defensible per-kWh trail rather than a flat number.


A growing number of fleet EV charging providers can stream wallbox data straight into the same invoice as the public-network charging sessions, which collapses the work back down to something close to flat-rate effort.


## When to Use Which Method


The decision usually comes down to how the driver actually uses the car. Use this matrix as a starting point:


Driver profile Recommended method Why


BEV, mostly home-charged, > 25,000 km/year Actual cost Real consumption exceeds the Pauschale; the higher reimbursement matters.


BEV, mixed home + workplace + public Pauschale (€30 tier) Workplace charging absorbs most kWh; admin savings outweigh the cap.


BEV, no workplace charging, average mileage Pauschale (€70 tier) Simple, tax-free, and close to actual cost for typical commuting patterns.


PHEV, mostly home-charged Pauschale (€35 tier) PHEV electricity consumption is naturally lower; flat rate usually fits.


PHEV, low electric-mode usage Pauschale (€15 tier) The car is largely fuelled at petrol stations through the fuel card.


Field-service team with installed wallboxes Actual cost Smart wallbox data already flows into the fleet platform.


A practical pattern many German fleets adopt: default to the Pauschale for the whole fleet, but allow any driver who can document at least three months of actual costs above the cap to switch to actual-cost reimbursement. That keeps payroll simple for the majority while protecting the highest-utilisation drivers.


Whichever method you choose, write it down. The Finanzamt cares less about which method you pick than whether you can produce the policy, the evidence, and the matching payroll entries on request.


## How a Fleet EV Charging Card Fits In


Reimbursement is only half of the picture. The other half — public charging at motorway stops, customer car parks, urban superchargers — needs to flow through the company's books in the first place. That's the job of a[fleet EV charging card](https://www.getrally.com/ev-charging) .


A modern fleet card consolidates three transaction streams:


- **Public charging** across AC and DC networks. The driver taps a fob or scans a card; the kWh and tariff land directly on the corporate invoice with VAT broken out per session.
- **Roaming across borders.** German fleets that drive into Austria, the Netherlands, France, or Italy get a single rate sheet and a single monthly invoice rather than a stack of receipts in multiple currencies.
- **Home-charging data** , where the fleet card provider integrates with the driver's smart Wallbox. The kWh consumed at home appears on the same monthly statement as the public sessions, valued at the driver's verified household tariff.


When that data sits on one invoice, the actual-cost method stops being painful. Payroll receives a single line per driver per month; the Finanzamt sees a clean, consolidated audit trail; and the driver isn't chasing PDF receipts from five different operators. Pair that with[company expense cards](https://www.getrally.com/company-cards) for non-charging costs and the entire fleet's energy footprint lives in one system.


Rally Charge is built on this model — a single Visa-backed card that handles public charging, home-charging reimbursement, fuel where the fleet still runs ICE vehicles, tolls, and parking. It sits inside a broader set of[EV charging cards for European fleets](https://www.getrally.com/blog/the-12-best-electric-vehicle-charge-card-solutions-for-european-fleets-in-2026) we've reviewed in a separate guide.


## What the Finanzamt Actually Wants to See


A Lohnsteuer-Außenprüfung (payroll tax audit) for company-car electricity reimbursement isn't common, but when it happens the auditor follows a predictable checklist. Knowing what they look for makes the policy easy to design.


**Required documentation:**


- **Method election.** A signed statement from each driver indicating Pauschale or actual cost, the applicable tier, and the date from which it applies.
- **Evidence per method.** For Pauschale: nothing beyond the payroll record and the policy reference. For actual cost: wallbox logs or sub-meter readings, the driver's annual Stromabrechnung, and a monthly calculation sheet.
- **Policy document.** A written employer policy referencing § 3 Nr. 46 EStG and the BMF Pauschale guidance.
- **Payroll trace.** The reimbursement booked on a dedicated, identifiable line so the auditor can reconcile against the policy.


**Audit-risk patterns to avoid:**


- Reimbursing actual kWh without a separate meter or a smart charger; auditors assume the driver's household appliances are being mixed in.
- Stacking the Pauschale and actual-cost reimbursement in the same month.
- Paying above the BMF cap without supporting actual-cost evidence — the excess becomes taxable wages.


**Retention.** All supporting documents (policy, election forms, wallbox logs, invoices) must be retained for the standard ten-year German tax document period. Most fleets keep this inside their[fleet accounting integrations](https://www.getrally.com/accounting) alongside the rest of the vehicle and expense records, so retrieval during an audit takes minutes rather than days.


If your Steuerberater hasn't already drafted a one-page employer policy on company-car home charging, that's the first artefact to create. Everything else flows from it.


## 2026 Checklist for German Fleets


Use this as a starting agenda for the next finance and HR meeting:


1. **Confirm the live BMF Pauschale figures with your Steuerberater.** Treat any number older than twelve months as provisional.
2. **Decide your default method.** Most German fleets default to the Pauschale and allow opt-in actual-cost reimbursement for high-mileage BEV drivers.
3. **Draft a one-page employer policy.** Reference § 3 Nr. 46 EStG and the current BMF letter. Include the method election form, the tier table, and the documentation each driver must provide.
4. **Install or formalise wallbox arrangements.** Where the business case supports it, employer-provided smart wallboxes simplify both reimbursement and audit.
5. **Consolidate public-charging payments onto a fleet EV charging card.** Pull cross-border sessions, VAT, and tariffs onto a single monthly invoice.
6. **Wire the data into payroll and accounting.** A monthly reimbursement file feeding payroll on one side and the general ledger on the other prevents double entry and reconciliation gaps.
7. **Schedule a quarterly review.** Check that drivers haven't migrated from one charging pattern to another in a way that would justify changing tiers or methods.
8. **Re-test before audit season.** Once a year, pull a sample of three drivers and walk the full chain — election form → wallbox log or Pauschale entry → payroll line → general ledger — exactly as the Finanzamt would.


None of this is exotic. It just needs to be written down, owned by a specific person, and reviewed once a quarter. Get those three things in place and home-charging reimbursement stops being a recurring question and starts being a routine line in the monthly close.


If you'd like to see how Rally consolidates public charging, home-charging reimbursement, and the rest of your fleet spend on a single Visa-backed platform,[book a demo](https://www.getrally.com/demo?source=blog_inline) .
