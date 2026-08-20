---
schema_version: "1.0.0"
document_id: "7a3c9ec3735faaf785c04955eda1b1f2fd004aa05be7900ff6031a55c3b8fc1f"
company_key: "yc-unravel-carbon"
company: "Unravel Carbon"
source_id: "yc-unravel-carbon-news-import-f0c750fc3a29"
canonical_url: "https://www.unravelcarbon.com/blog/activity-based-vs-spend-based-carbon-accounting"
published_at: null
first_seen_at: "2026-07-22T18:03:08.439701+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:0ad32f51b9cb97f8439a542592f8a786c1b6f3635cc81723156dc23daa357404"
---

# Activity-Based vs Spend-Based Carbon Accounting

## **What's the Difference Between Activity-Based and Spend-Based Carbon Accounting?**


The difference is the data each method uses. **Activity-based** carbon accounting calculates emissions from the physical activity, such as the quantity of a material or the volume of a fuel. **Spend-based** accounting estimates them from money spent multiplied by an industry-average factor. Activity-based data is more accurate and easier to audit. Spend-based data is faster but coarser. The **waterfall method** combines both: it uses the most specific factor available on each line and falls back to a broader estimate only when nothing better exists.


### **Key takeaways**


- Activity-based accounting ties each emission to a physical activity. Spend-based accounting ties it to money spent.
- Spend-based estimates miss supplier-level changes and move with price, which makes them harder to defend in an audit.
- The waterfall method uses the most specific factor available on each line and drops to spend-based only when nothing better exists.
- Unravel Carbon applies this across all 15 Scope 3 categories and ensures a 100% audit pass rate.


### **What is the waterfall method in carbon accounting?**


The waterfall method is a ranked order of emission factor sources, applied line by line. Calculation starts with the most granular factor available for an activity and moves to less precise options only when granular data is missing. Each activity settles at the highest tier it qualifies for, so the footprint rests on the best data each line can support rather than one blunt method applied across the board.


‍


In practice the ranking runs from supplier-specific measurement at the top down to sector averages at the bottom:


1. **Supplier-specific product carbon footprint (PCF)** : a cradle-to-gate figure for the exact product purchased, calculated and supplied by the producer. It is the most granular input because it reflects how that specific item was made.
2. **Supplier Scope 1 and 2 emissions allocated by output** : the supplier’s measured emissions divided by physical units produced, then applied to the quantity bought. Used when there is no product-level PCF but the supplier reports its footprint and its output.
3. **Supplier Scope 1 and 2 emissions allocated by revenue** : the supplier’s measured emissions allocated by the buyer’s share of supplier revenue. The fallback when output volumes are not available but the supplier still reports its emissions.
4. **Spend-based factor (Exiobase / EEIO)** : a sector-average emissions figure per unit of spend, drawn from an environmentally-extended input-output database such as Exiobase. This is the backstop for lines where no supplier data exists.


The first three tiers are activity-based and the fourth is spend-based, so the waterfall reaches the spend-based backstop only when no supplier data is available.


The hierarchy of emission factors in the waterfall method used by Unravel Carbon.


### **Why spend-based estimates fall short**


A spend-based estimate multiplies money spent by an industry-average emission factor: 100,000 dollars of steel times the average emissions per dollar. It is fast and it fills gaps, but it carries three costs:


- It treats every supplier in a category as identical. A supplier that has switched to low-carbon production looks the same as one that has not, because the only input is spend, so supply-chain progress stays invisible.
- It moves with price rather than emissions. If steel gets more expensive, the footprint rises even when the physical activity did not change, which makes year-over-year comparison unreliable.
- It is hard to defend line by line. An auditor can check the arithmetic, but the underlying factor is a sector average with little traceability to the specific purchase. That gap is where audit findings come from.


### **How activity-based data changes the number**


Activity-based accounting maps each line to the physical thing involved: a quantity of a specific material or a volume of a named fuel. Because the factor attaches to the activity rather than the invoice, the footprint reflects what happened on the ground.


‍


That shift fixes the problems above. Supplier improvements show up in the data instead of being masked by a sector average, and each figure traces back to a documented factor, which is what auditors test.


‍


The waterfall keeps spend-based estimates for lines where granular data does not yet exist, but stops them from being the default.


### **Where to get activity data**


Activity-based accounting only works if you can find the underlying data, and most of it does not arrive on its own. Collecting it manually means going to the source for each line:


- Energy and fuel: utility bills, meter readings, and fuel purchase records give consumption in physical units (kWh, litres, cubic metres) rather than dollars.
- Materials and goods: production logs, bills of materials, and weighbridge or shipment records give quantities and weights.
- Supplier-specific footprints: request product carbon footprints and Environmental Product Declarations (EPDs) directly from suppliers. An EPD is a third-party-verified document, built to a Product Category Rule under ISO 14025, that reports cradle-to-gate emissions for a specific product. Public EPDs are searchable in libraries such as[EPD International](https://www.environdec.com/)


When supplier data is missing, you fall back to public emission factor databases. The most widely used include:


- [UK Government (DESNZ/DEFRA)](https://www.gov.uk/government/collections/government-conversion-factors-for-company-reporting) greenhouse gas conversion factors
- [US EPA GHG Emission Factors Hub](https://www.epa.gov/climateleadership/ghg-emission-factors-hub)
- [IPCC Emission Factor Database (EFDB)](https://www.ipcc-nggip.iges.or.jp/EFDB/main.php)
- [Ecoinvent](https://ecoinvent.org/) , for life-cycle inventory data
- [Exiobase](https://www.exiobase.eu/) , for spend-based input-output factors


The manual route works, but it scales badly. Pulling the right factor from the right database for every line, keeping each one current as the source updates, and recording where it came from is the work that breaks down once a footprint spans thousands of suppliers. That is the problem the waterfall automates.


### **How Unravel Carbon applies the waterfall**


Unravel Carbon prioritises item-level emission factors over spend-based estimates across all 15 GHG Protocol Scope 3 categories. The engine reaches for the most specific factor first and falls back only when it must, so most of a footprint rests on activity data wherever the inputs allow.


‍


That order is only as good as the library behind it. Unravel Carbon's proprietary database holds roughly 132,000 emission factors from government, intergovernmental, academic, and industry sources. Around 122,000 are activity-based and 10,000 spend-based, the split a waterfall needs: deep activity coverage first, spend-based as the backstop. Each factor is quality-scored, version-controlled, and tied to its source, and the methodology is shared with customers rather than hidden.


‍


AI handles the matching, mapping messy procurement and activity data to the right factor. Teams can set their own source priority and upload custom factors when they hold better data than the library.


### **Why the waterfall matters for audit**


Audit-readiness comes from traceability. Every factor stores its lineage, so a reviewer can follow any number back to its source. Unravel Carbon's methodology is certified against the GHG Protocol, ISO 14064 for corporate footprints, and ISO 14067 for product footprints, with the calculation audited by TUV Rheinland.


The result shows up where it counts at assurance time: a 100% audit pass rate and 99% match accuracy. A footprint built from the most specific data available, with a documented trail behind it, is one an auditor can sign off without sending back.


### **Frequently asked questions**


**What is the difference between activity-based and spend-based carbon accounting?**


Activity-based accounting calculates emissions from the physical activity, such as kilograms of a material or litres of a fuel. Spend-based accounting calculates them from money spent multiplied by an industry-average factor. Activity-based data is more accurate and easier to audit. Spend-based is faster but coarser.


‍


**What is a hybrid waterfall methodology?**


It ranks emission factor sources and applies the most specific one available to each activity, falling back to broader estimates only where granular data is missing. “Hybrid” means it combines activity-based and spend-based factors in a single footprint rather than relying on one method.


‍


**Is activity-based data required for audit?**


No standard mandates 100% activity-based data, and most footprints mix methods. Auditors test whether each factor is traceable to a credible source and applied consistently, which a waterfall approach supports by maximising activity-based coverage and documenting every factor's lineage.


‍


**Does Unravel Carbon measure all Scope 3 categories?**


Yes. The platform calculates emissions across all 15 GHG Protocol Scope 3 categories, prioritising item-level factors over spend-based estimates in each.


‍


See how the waterfall method works on your own data:
‍[Book a demo now.](https://www.unravelcarbon.com/request-demo)


‍
