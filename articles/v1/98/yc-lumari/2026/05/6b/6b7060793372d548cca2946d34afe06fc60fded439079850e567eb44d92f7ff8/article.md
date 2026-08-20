---
schema_version: "1.0.0"
document_id: "6b7060793372d548cca2946d34afe06fc60fded439079850e567eb44d92f7ff8"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/lead-time-procurement"
published_at: "2026-05-16T00:00:00+00:00"
first_seen_at: "2026-08-09T23:40:56.159161+00:00"
fetched_at: "2026-08-09T23:40:57.548506+00:00"
content_hash: "sha256:9493f3a37bef3233489ec6e8d005aa5ab8e3c14d593a6ac5fa0ac04520a3fd7a"
---

# Lead Time in Procurement: Why the Number on the BOM Is Lying to You

Lead time in procurement is the elapsed time from placing a purchase order with a supplier to the goods being received and available to use at your facility. It includes the time the supplier needs to manufacture or pull from stock, ship, clear customs if international, and the receiving and putaway time on your end. The number lives on the BOM and feeds MRP. The number is also, at most companies, wrong.


That's the inconvenient truth. Procurement runs on lead times that haven't been audited in years. MRP plans against them. Production schedules build on top of them. And then a part shortage happens and everyone is surprised, except the buyer who knew the real lead time was 14 weeks, not the 8 weeks in the system.


## The Anatomy of a Lead Time


A lead time that says "8 weeks" is hiding several different clocks running in series.


**Order processing time** is the day or two between the PO being cut and the supplier formally acknowledging it. For some suppliers this is hours. For others (especially overseas with language and time-zone gaps), it's a week before they even confirm receipt of the PO.


**Manufacturing lead time** is the actual production time. For a stocked part, this is zero. For a build-to-order CNC part, it might be 4 weeks. For a custom casting requiring tooling, 12+ weeks.


**Component or raw material lead time** is the part nobody sees on your BOM but the supplier sees on theirs. Their lead time depends on their supplier's lead time. The 16-week semiconductor lead time you're feeling is your supplier feeling a 24-week lead time from a wafer fab.


**Shipping lead time** is the transit. Ocean from Shanghai to LA: 3-4 weeks plus port congestion. Air from Shenzhen to Memphis: 4 days. Truck from Monterrey to Texas: 2 days. Rail from Ohio to Kentucky: a day.


**Customs clearance** for international shipments is a wildcard. Most clearance is fast (under 48 hours). When it's not, it can be a week. Random inspection holds, ISF filing errors, anti-dumping duty audits.


**Receiving and inspection** at your end is real time. A part can sit on the dock for 2-3 days before it's counted, posted, and moved to its bin location. For inspection-required parts (aerospace, medical, regulated), add another 2-5 days for incoming QA.


When teams "shorten lead time," they usually focus on the manufacturing piece. The other pieces add up to more total time and are where the real wins are.


## Why the Lead Time on Your BOM Is Wrong


The lead time field in your ERP got populated when somebody first set up the part. Maybe in 2018. Maybe when a buyer asked the supplier "how long does this take" and typed in whatever the supplier said. Then nothing updated it.


A few patterns we see in real BOMs.


**The supplier quoted "stock" but the part hasn't been stock for two years.** Original quote said 2 weeks ARO (after receipt of order). They had the part on the shelf. Then demand picked up, they stopped stocking, and now it's a 10-week build. Buyer never updated the system.


**The original lead time was for the original quantity.** Quoted at 2-week lead time for 100 pieces. Now you order 5,000. The supplier needs 12 weeks for that volume. The BOM still says 2 weeks because it's a single field, not a quantity-tiered table.


**The lead time hasn't been re-audited since semiconductor allocation.** A part that was 12 weeks pre-2021 went to 52 weeks during the chip shortage and then dropped back to 18 weeks post-shortage. The BOM still says 12. Nobody updates it because the buyer is too busy chasing the parts that are actually late.


**The "lead time" is just the supplier's PR number.** Some suppliers quote 4-week lead time when they internally know it's 6, because 4 sounds better. Buyers find out the real number after their first three POs all arrive 2 weeks late. Some buyers update the system. Most don't.


If you want a sobering exercise, pull a report of last quarter's actual delivery dates versus the lead time field on the BOM. Find the parts where the average actual lead time is more than 25% longer than the BOM number. We see 15-30% of part numbers off by that much at most companies. That's a planning failure waiting to surface as a parts shortage.


## Lead Time vs Cycle Time vs Takt Time


These terms get confused. Worth being precise about, because they answer different questions.


**Lead time** is the customer-facing duration: order placed to order received.


**Cycle time** is the supplier-side production duration: the time it takes to make one unit, or one batch, on the line.


**Takt time** is the demand-driven pace: how fast a unit needs to come off the line to meet demand. Takt = available production time / demand units.


A supplier can have a fast cycle time and a long lead time if they're queued behind other orders. A short takt time can require a longer lead time because the supplier needs to ramp capacity. Don't conflate these in conversations with engineering or planning, because they each control different decisions.


## How Lead Time Decisions Cascade


Set the lead time too short on the BOM and MRP plans against it. The buyer follows MRP and places the PO 8 weeks before it's needed. Actual lead time is 12 weeks. The part arrives 4 weeks late and the line goes down.


Set the lead time too long and you carry too much safety stock and tie up working capital. Finance asks why inventory is up 18% versus plan. Procurement says "supplier reliability." The real answer is the BOM is padded.


The right answer is to keep the lead time field accurate and use safety stock or safety lead time as a separate buffer. Conflating them buries the variance.


The opinion: most companies treat lead time as a static field and adjust safety stock to compensate when reality doesn't match. That's backwards. Lead time should be reviewed quarterly per supplier per part and updated based on actual data. Safety stock then handles true variability, not chronic systemic mis-data.


## What Buyers Should Actually Track


Three numbers, per supplier, per part:


**Quoted lead time.** What the supplier promises.


**Actual realized lead time.** What it actually was, averaged over the last 4-6 deliveries.


**Lead time variability.** Standard deviation or range. A part that's reliably 10 weeks plus or minus 2 days is different from a part that's "10 weeks" but ranges 6 to 18.


The first two answer "is the BOM accurate." The third answers "how much safety stock do I really need." Most ERPs have fields for the first one and nowhere obvious to track the others. So buyers track them in side spreadsheets. Or don't track them at all and run on instinct.


A supplier with 4-week lead time and high variability is worse than a supplier with 8-week lead time and zero variability. The 8-week one is plannable. The 4-week one is gambling.


## Industry-Specific Lead Time Realities


Aerospace: lead times for forgings, castings, and machined fitments commonly run 26 to 52 weeks. Single-source allocation rules. A change in source requires First Article Inspection (FAI) under AS9102, adding 6-10 weeks before any part can ship.


Automotive Tier 1/2: PPAP-required parts add 8-12 weeks before production parts can ship from a new supplier. Production lead times are tighter (often 2-4 weeks for in-production parts) but the qualification overhead is real.


Medical devices: design changes trigger validation cycles that add 8-16 weeks before parts can re-enter the supply chain. FDA 21 CFR Part 820 requires the change to be documented in the DMR, which gates supplier release.


Electronics: semiconductor lead times have settled in the 12-26 week range post-shortage but are highly category-dependent. Power management ICs and microcontrollers are the lingering pinch points.


Industrial: motors, gearboxes, pumps from major OEMs commonly run 16-32 weeks for new orders. Aftermarket replacement parts can be longer than new builds because they're treated as low-priority spares production.


Knowing your industry's normal helps you spot when a supplier is quoting something off-trend. A 14-week quote on a part that's normally 8 means something. A 14-week quote on a part that's normally 14 is just Tuesday.


## How Lumari Helps With Lead Time Reality


Lumari reads supplier emails and ASNs and ties promised dates and actual ship dates back to the PO. Over time, that data becomes the actual realized lead time per supplier per part. The buyer can see where the BOM number is off and update it. Exceptions (a date that just slipped, a shipment confirmed earlier than promised) surface in real time so the planner can adjust before the variance becomes a shortage.


If your BOM lead times are stale and your buyers are running on memory,[see how Lumari turns supplier emails into real lead time data](https://lumari.ai/) .
