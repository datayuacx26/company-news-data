---
schema_version: "1.0.0"
document_id: "63b2dc2dc5f73152b53cd48c7a9e5b24140d5e527315ae9c1b621e639a37b3a9"
company_key: "yc-electric-air-2"
company: "Electric Air"
source_id: "yc-electric-air-2-news-import-b69578f00d30"
canonical_url: "https://www.electricair.io/posts/push-button---get-heat-pump"
published_at: null
first_seen_at: "2026-07-25T02:28:44.827958+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:13e2b7dd328dd3d4cea49336ec33052fbdddc7a3cc973ac61df60630e818cc67"
---

# Push Button - Get Heat Pump

This is what Jeremy and I tell each other when we're figuring out what direction to push our product development. Electric Air wants to make upgrading your home to a heat pump super simple — and based on our reviews it's working so far. But behind the curtain there's actually a lot of complexity to doing this right.


Here's an incomplete list of what's involved:


**Operational Cost Modeling:** We made a calculator to help homeowners see the before and after energy costs of a heat pump. This is a pretty sophisticated tool. We developed our own UI and wrapper around Energy Plus, a building energy modeling tool developed by NREL. Our tool incorporates PG&E TOU rate structures, equipment performance data, local weather files, and statistical models of home thermal envelopes to generate hourly predicted performance of your home. This gets every interested homeowner closer to the answer to their most burning question: "is this going to save me money?".


**NEC Electrical Load Calcs:** Your home's electrical panel is rated for a certain current, and we have to do the math to make sure there's capacity for adding a heat pump. Sometimes have to use techniques like quad-tandem breakers, derating EV chargers, or sharing 240V loads on a NEMA socket splitter to make 100amp homes heat pump ready.


**Manual J Thermal Load Calculation:** As every armchair expert on /r/heatpumps knows, this determines how big your heat pump needs to be and what every HVAC shop should do during the design stage. We calculate your home's expected heating and cooling loads based on the 99th percentile and 1st percentile temperatures for your location, as well as the condition of insulation, windows, foundation, etc. All this work prevents the too-familiar problem of oversizing HVAC equipment.


**Duct Sizing:** Heat pumps need more airflow than a natural gas furnace. Heat pumps are more efficient when they can send cooler air than a gas furnace into your vents — they do this by increasing airflow relative to the furnace. But most furnaces are oversized, and so existing ductwork ends up being the right size for a heat pump. We also employ different methods to verify each system; via static pressure probes or a simpler visual inspection (tip: check the size of the return grille).


**Custom System Design:** It's not unusual for us to identify parts of the home that are difficult to duct correctly — a master bedroom that's far away from the air handler, an attic with ducts buried in stud bays, or difficult-to-remove asbestos blanketing everything we want to touch. These tough areas often get dedicated mini-splits, ceiling cassettes or mini-ducted units.


**Permitting and Municipal Requirements:** The Bay Area has a huge variety of permitting requirements (Foster City is unusual in requiring NEC load calcs and elevation site plans) as well as equipment setbacks and noise restrictions. We're constantly updating our internal database to stay on top of requirements for each AHJ (Authority Having Jurisdiction).


**Incentives:** Lots of paperwork here. One of our most popular incentives requires specific line items mentioned on the invoice, equipment model and serial numbers, permit numbers, and photos of capped gas lines and old and new equipment. Many local installers skip this incentive because it's so much work, but we don't because these rebates are not only for saving our customers significant money, but they also serve as a signal to energy entities that these programs are working as intended, which helps secure more funding for future phases.


And this is just application engineering! Managing install teams to consistently deliver great installs is another challenge for another post.
