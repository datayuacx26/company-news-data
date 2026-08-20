---
schema_version: "1.0.0"
document_id: "f59e4a58d9b595b1fa649056c93fc44dc545169a30438d7b0be5c1330c99ccef"
company_key: "yc-clidey"
company: "Clidey"
source_id: "yc-clidey-rss-03eb479b7595"
canonical_url: "https://blog.clidey.com/from-a-broken-spreadsheet-to-a-real-pricing-engine-how-one-industrial-fabricator-rebuilt-their-quoting-process-with-whodb/"
published_at: "2026-08-11T14:35:38+00:00"
first_seen_at: "2026-08-11T14:56:16.025314+00:00"
fetched_at: "2026-08-11T14:56:18.120119+00:00"
content_hash: "sha256:1fd52e23f3f6ce3661eb6bf75402e2e7331e206ecd5083a66d0718be3184bd7b"
---

# From a Broken Spreadsheet to a Real Pricing Engine: How One Industrial Fabricator Rebuilt their Quoting Process with WhoDB

If you've ever worked in industrial fabrication, you know the quiet danger hiding in most estimating teams. It's a single spreadsheet built years ago that everyone depends on and nobody fully understands anymore.


That was the starting point for one of our recent projects: a company that fabricates industrial tanks and needed a better way to price them.


Here's what happened.


## **What does pricing a tank actually involve?**


The team quotes tanks and related components, including cylindrical shells, heads, manholes, plates, stiffeners, linings, and other accessories that go into a finished tank. Sales and estimating staff take a client's requirements, plug in specifications like dimensions, materials, and quantities, and turn that into a price.


Simple in concept. Complicated in practice, because tank pricing isn't just "cost times markup." It depends on geometry, material layers, thickness, surface area, volume, and a dozen other variables that all move together. That's exactly the kind of math spreadsheets start to strain under.


## **So what was actually going wrong?**


Their pricing logic lived inside a legacy VBA-based spreadsheet model. It had worked for years, but it had grown fragile. It was hard to maintain, hard to reuse, and nearly impossible to validate with confidence. Every new tank configuration carried a small risk. Was the formula actually calculating the right volume? Was the thickness for each material layer being applied correctly?


It also wasn't built to connect to anything. It couldn't plug into a modern application; it was hard for new team members to trust or verify, and, most importantly, it had real calculation bugs. Some layer-thickness math and unit-conversion logic were quietly producing incorrect numbers, the kind of thing that's easy to miss until a quote goes out wrong.


What they needed was a reliable, centralised way to calculate tank prices and capacities, sitting inside a proper quote-management workflow instead of a spreadsheet passed around between people.


## **How did WhoDB fix it?**


We didn't just put a new interface around the old spreadsheet. We rebuilt the pricing logic and gave it a home inside a real application.


Here's what that involved:


- Refactored the pricing logic into a dedicated server function (calculate_pricing_v2), replacing the fragile spreadsheet formulas with clean, testable code.
- Connected the pricing engine to live data. Quotes, Tanks, and Tank Items are now structured records in WhoDB's ontology layer, not rows in a spreadsheet.
- Built the full workflow teams actually need: creating, editing, duplicating, and deleting quotes, plus detailed tank and item management.
- Made totals recalculate automatically. Line prices, tank totals, volumes, and full quote totals all update together instead of requiring manual recalculation.
- Added safeguards. Confirmation dialogues sit in front of destructive actions, so a deleted quote or tank isn't an accident.
- Built a reference and testing page where the team can compare expected results against what the engine actually calculates, tank by tank.


In short, the same pricing know-how the team relied on for years is now running on solid ground.


## **What can the system do now?**


The rebuilt engine handles a lot more than the original spreadsheet ever could:


- 45 reference calculation cases are used to validate results: 41 for vertical tanks, 4 for rectangular tanks.
- Pricing and volume can be calculated at the item level, the tank level, or including large quotes with many tanks of different dimensions, without losing accuracy or slowing down.
- Quantities scale correctly. Tank and quote totals adjust for quantity without accidentally double-counting anything.
- The engine outputs real engineering detail, not just a price: surface area, material volume, internal volume, material mass, material cost, labour cost, extra costs, and total cost.
- It supports multiple materials and layers, material-specific thicknesses, additional line-item costs, and capacity calculations for each tank.
- Several long-standing calculation issues were fixed along the way, including unit-consistency problems and errors in per-layer thickness and channel cross-section math.


## **Did the scope end at "fix the calculations"?**


No. The brief was "fix our pricing calculations," but a calculation engine on its own isn't very useful without a place for it to live. So we built the surrounding application too:


- A complete, working quote-management app, not just a formula.
- Structured, ontology-backed data instead of scattered spreadsheets, so quotes, tanks, and items are all properly connected and queryable.
- Smart duplication. Copying a quote correctly preserves and re-links all of its tanks and tank items, instead of leaving orphaned data behind.
- Search and filtering across quotes and tank items, so finding a past quote doesn't mean scrolling through a file.
- Engineering outputs, not just price. Surface area, mass, and volume are calculated alongside cost, all of which matter to a fabrication team.
- Validation, error handling, structured logging, and configurable material pricing and density overrides, so the numbers can be trusted and adjusted as material costs shift.
- A built-in reference surface for ongoing testing, so future changes to the pricing logic can be checked against known-good cases before they go live.


## **Why this kind of rebuild matters**


This is a pattern we see often. A business has deep, hard-won expertise trapped inside a spreadsheet that's slowly becoming a liability. The knowledge is real. The tool holding it together isn't.


WhoDB's approach here, pulling business logic out of a spreadsheet, connecting it to structured, governed data, and wrapping it in a proper application, is the same approach that works across ETL pipelines, semantic ontologies, and AI-built decision interfaces more broadly. Tank pricing just happened to be where it showed up this time.


If your team is still one macro away from a pricing mistake, it might be worth a conversation.


[Book a demo →](https://whodb.com/?ref=blog.clidey.com)
