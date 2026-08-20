---
schema_version: "1.0.0"
document_id: "57dc8c346cc6cc6707fc5a6d302cda6ff5b3060789697d3f77d20c26ef6c8c38"
company_key: "yc-propexo"
company: "Propexo"
source_id: "yc-propexo-news-import-1c9cf0eb2f62"
canonical_url: "https://propexo.com/blog/multifamily-bi-same-promise/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-12T03:27:59.234779+00:00"
fetched_at: "2026-08-12T03:28:02.539824+00:00"
content_hash: "sha256:85eb2712c5e42a9304be24185cfc6ae3df43c9004691ce9bec96183badc5816c"
---

# Why Multifamily BI Keeps Selling the Same Promise

Pull up the Wayback Machine and load Rentlytics’ website as it looked in 2016. The headline copy reads: “full visibility into all your properties, even if the data lives in multiple systems.” Centralized portfolio view, spot trends, capture maximum NOI. Now open a tab with any 2026 proptech BI vendor’s homepage next to it. If you swapped the logos, nobody on your team would notice. The promise is word for word the same, and that should bother anyone who signed a check for it the first time.


Rentlytics earned the right to make that promise before anyone else. Justin Alanís and Phil Plante built one of the first proptechs focused exclusively on stitching together data from disparate systems and turning it into business intelligence. The client list they built read like a who’s who of multifamily: Greystar, Avenue5, BH, and many more. These were not credulous buyers. They were the most sophisticated operators in the industry, and they looked at their fragmented portfolio data and concluded that this was the company solving it. After a six-year run, RealPage acquired Rentlytics, and the product was eventually wound down inside its new owner.


Ten years after that 2016 copy went live, the same pitch is back on the market, made by new vendors to the same operators. The natural conclusion is that the first generation failed and the new one will not. The more uncomfortable conclusion is that something about the way the industry buys this category keeps producing the same outcome, and that a decade-old pitch resurfacing intact is evidence the problem was never solved, only resold.


## What 2016 Got Right


Give the founding team its due, because the diagnosis was correct and early. Rentlytics saw that a multifamily operator’s most valuable asset, the operational record of its own portfolio, was scattered across systems that did not talk to each other. They saw that executives were making capital decisions off month-old spreadsheets assembled by analysts doing manual exports. The Wayback Machine capture is striking precisely because so much of the diagnosis holds up a decade later.


What they got right was the problem statement. What broke was everything downstream of it, and the breakage had a specific shape worth studying rather than a vague “they were too early.” Rentlytics shipped one set of dashboards for the whole industry: one rigid analytical layer, sold to operators whose portfolios and reporting needs diverged wildly from one another. The product could show every client the same picture, and the clients needed different pictures.


There is a second structural lesson in how the story ended. A neutral data layer acquired by the industry’s largest software vendor stops being neutral on the day the deal closes. Whatever RealPage’s intentions, an operator running Yardi or Entrata was never going to route its portfolio data through a competitor’s BI product, so the acquisition capped the market at exactly the moment the category needed to be system-agnostic.


## The Stack Exploded While Nobody Was Watching


Here is what makes 2026 a different market from 2016, and it is not that the vendors got smarter. The number of separate applications operators run has exploded over the decade, and the proliferation itself is now a cost center. In 2016, “fragmented data” meant your PMS and maybe an accounting system. Today the leasing journey alone can touch an AI leasing agent, a CRM, a screening tool, a smart-lock platform, and a resident app before a single rent payment lands in the ledger. Most of those tools did not exist when Rentlytics came to market, and every one of them is generating operational data that never makes it back to a system you control.


The cost of that fragmentation changed in kind, not just in degree. A decade ago the penalty was slow reporting: your portfolio review ran on stale numbers, and an analyst spent a week a month reconciling exports. Painful, survivable. The penalty in 2026 is that your AI initiatives quietly die. A pilot that worked in the demo fails in production because the model can only reach a fraction of the data it needs, and what it can reach is inconsistent across properties. The vendors selling you AI rarely say this part out loud, because the fix is unglamorous plumbing that sits outside their product.


Multifamily is not even the worst case of this disease, which is its own kind of warning. Anyone who finds real estate data fragmented should look at healthcare, where interoperability failures became so expensive that fixing them grew into an entire subsector with its own regulations. Real estate has no HL7 standard coming to save it, and no regulator forcing the systems open. Whatever gets fixed here gets fixed because operators demand it in procurement.


This is where an honest self-audit is worth two minutes of any operator’s time. Ask where your leasing conversation data lives right now, and whether anyone in your organization could point to it if the AI vendor’s contract ended tomorrow. The last time a system in your stack changed its API, consider who found out first, your team or your broken reports. And look hard at whether your most recent technology purchase added a data source you can query or another silo you rent access to. Operators who answer those questions honestly usually discover they are better provisioned for dashboards than for the decade they are walking into.


## It Was Never a Dashboards Problem


The reframe that the last ten years should force on the industry is that the dashboard was never the product. Every generation of portfolio analytics has sold the picture on the glass, and the picture is the cheap part. The expensive part, the part that killed the first generation, is the layer underneath: how the operational data gets extracted and normalized, and who owns it once it has been. Rentlytics bundled that layer inside a BI application, which meant the plumbing lived and died with the app. When the app hit its structural ceiling, the plumbing went with it, and every client’s integration investment evaporated.


The architecture available to operators now inverts that bundle, and the inversion matters more than any feature comparison. Portfolio data lands in a warehouse the operator owns, Snowflake or BigQuery or whichever platform its data team already runs, and the analytical layer becomes swappable on top. REBA’s founding premise, flexible dashboards instead of one fixed set, is one answer at that layer. So is a data science team with notebooks, or an executive’s favorite BI tool. The point is that losing a vendor no longer means losing the data, because the data never leaves your control in the first place.


That leaves extraction as the remaining hard problem, which is exactly the problem companies like Propexo now treat as a standalone product:[managed connectors](https://propexo.com/connect/integrations/) that pull data out of the PMS and the operational stack and land it, normalized, in the customer’s own warehouse. Whether an operator buys that capability or builds it, the architectural test is the same, and it is the test Rentlytics’ clients never got to apply. If the analytics vendor disappeared tomorrow, would your data and its pipelines survive the funeral?


Call that the Rentlytics test, and I will stake a claim on it that the polite version of this piece would hedge. Every product in this category that bundles the analytics with the plumbing will end the way Rentlytics ended, because the failure mode is built into the bundle. The plumbing carries the switching costs and the glass carries the valuation, and sooner or later someone buys the valuation and strands the plumbing. That is a falsifiable prediction, and I would like to be wrong about a category this useful. Run the test on any 2026 entrant’s pitch, and you will know which side of it they are on before the demo starts.


## What to Do Before the Next Pitch


The directive for operators evaluating this category in 2026 is concrete. Separate the two purchases that the 2016 generation bundled. Buy your data layer first: demand raw, queryable access to your own operational data in a warehouse you control, and treat any vendor who cannot deliver that as a rental, priced accordingly. Only then evaluate the analytical layer, where the switching costs are now low enough that being wrong is survivable. Write API change notification and data egress terms into vendor contracts the way you write insurance requirements into construction contracts, because a connector that breaks silently is the modern version of the month-old spreadsheet. And when a vendor’s pitch promises full visibility across all your systems, ask them precisely where your data will physically live and who holds the keys. The 2016 copy never had a good answer to that question. The 2026 vendors worth hiring do.


Zoom out far enough and the pattern is the lesson. Industries do not usually get sold the identical promise a decade apart unless the promise is real and the delivery model is broken, and multifamily has now paid for that tuition once. The operators who internalize it will spend the next ten years compounding on data they own while their peers audition the third generation of the same pitch.
