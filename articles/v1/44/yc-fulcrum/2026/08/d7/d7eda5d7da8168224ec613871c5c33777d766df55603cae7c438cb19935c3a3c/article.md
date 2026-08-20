---
schema_version: "1.0.0"
document_id: "d7eda5d7da8168224ec613871c5c33777d766df55603cae7c438cb19935c3a3c"
company_key: "yc-fulcrum"
company: "Fulcrum"
source_id: "yc-fulcrum-news-import-4f6fb51d9f9d"
canonical_url: "https://www.fulcrumapp.com/blog/what-water-utilities-need-to-document-storm-damage-for-fema/"
published_at: "2026-08-20T00:54:26+00:00"
first_seen_at: "2026-08-20T01:49:37.320458+00:00"
fetched_at: "2026-08-20T01:49:40.618342+00:00"
content_hash: "sha256:12d9212ba43f3091d88b61326490717873ce767c448e4e513cea8df464aee1f5"
---

# What water utilities need to document storm damage for FEMA

Water utilities have 60 days to identify and report storm damage to FEMA, and paper forms or verbal radio reports can leave that record built mostly on memory. Missing GPS coordinates, photos, and asset-level detail can put reimbursement at risk before that 60-day window even closes. Utilities running water utility management software before severe weather hits get a prioritized repair list, records that map cleanly to FEMA’s funding categories, and disaster recovery documentation built to stand up to federal review.


**Key insights**


- A water utility’s Supervisory Control and Data Acquisition (SCADA) system logs pressure changes, but only a physical inspection confirms which asset failed.
- Verbal radio reports on water utility storm damage skip the GPS stamps and photos FEMA requires.
- Fast, asset-level confirmation lets water utility crews isolate a compromised main and issue boil-water notices before health risks spread.
- Storms knock out cellular networks, so water utility crews need field capture that keeps working offline until they reconnect.
- Water utility management software configured before storm season ties emergency response directly to a utility’s existing AWIA resilience planning.


After a storm, a drop in water distribution system pressure could mean a main break, a flooded pump station, or a treatment facility running on backup power. SCADA water utility monitoring logs the drop in real time, but it can’t tell those causes apart. Only a crew standing at the asset can confirm which one happened.


Water utility storm response runs on that kind of field confirmation. Pump stations, valves, hydrants, and treatment infrastructure all need eyes on them before crews can prioritize repairs. Water utility GIS shows where each asset sits, but not what condition it’s in once the storm has passed. For water and wastewater teams managing aging infrastructure, that distinction matters: GIS data establishes where an asset is located, while field evidence establishes its post-storm condition.


At many utilities, closing that gap still falls to paper forms and a radio channel. Crews report whatever they remember once they’re back in coverage.


The cost runs deeper than lost time. FEMA reimbursement and the public health decisions a utility makes during a storm both depend on what crews confirm in the field.


## The assets crews have to check before repairs can start


Storm damage sorts into a few distinct mechanisms, and each one calls for its own field check.


Flooding hits pump stations and lift stations first. Rising water can knock a station offline for days once it reaches the control panels.


Valves and hydrants tend to fail mechanically. A pressure regulating valve that sticks or shears off throws off the pressure zone isolation a distribution system depends on. A hydrant buried under debris counts as offline even if it looks fine from the street.


**Related reading:**[Connecting hydrant, valve, and meter records](https://www.fulcrumapp.com/blog/water-asset-management-starts-with-connecting-hydrant-valve-and-meter-records/) for day-to-day water asset management.


Ground movement causes a third kind of damage. Saturated soil shifting beneath a main is often what cracks the pipe open. The same shift can compromise a storage tank’s structural integrity or its contamination seals.


SCADA and GIS can’t confirm any of this on their own. SCADA only reports symptoms: a pressure change, a flow change, or a station that’s gone offline entirely. It can’t distinguish a failed valve from a main break three streets over. GIS was built to support water infrastructure management planning, but confirming storm damage still requires a crew on site with access to current GIS data and the ability to document what they find.


Pump station inspection, valve checks, and[hydrant assessments](https://www.fulcrumapp.com/apps/hydrant-flow-testing-form/) only become useful once each finding attaches to a specific, identified asset. A general damage estimate for a zone or sector doesn’t tell a repair crew which valve to open first. Water distribution system management during a storm depends on that same asset-level precision. Getting crews to the right asset quickly is a utility workforce management problem as much as a technical one.


> **The public health stakes behind a water main break**
>
>
> Among all the assets crews check after a storm, one carries consequences well past the repair bill: a main break. Once pressure in a line drops low enough, contaminated groundwater or floodwater can enter the system through the break. Isolation procedures and boil-water notices exist because of that exact risk.
>
>
> Utilities move quickly once they confirm such a risk. Crews isolate the affected pressure zone first. The utility then notifies customers and the state drinking water program, following[Safe Drinking Water Act](https://www.epa.gov/sdwa) compliance requirements. Each step depends on knowing exactly which main, which pressure zone, and which customers sit downstream of the break.
>
>
> [America’s Water Infrastructure Act of 2018](https://www.epa.gov/ground-water-and-drinking-water/americas-water-infrastructure-act-2018-awia) (AWIA 2018) adds a planning requirement on top of that response. The law requires any community water system serving more than 3,300 people to develop and certify an[Emergency Response Plan](https://www.epa.gov/waterresilience/awia-section-2013) . Storm damage documentation is one part of carrying out that plan, proving the isolation and notification work happened at the right place and the right time.
>
>
> **Related reading:**[Resilience by design: using GIS and field data to plan for the unexpected](https://www.fulcrumapp.com/blog/using-gis-and-field-data-for-resilience-planning/)


## Verbal reports and paper forms slow recovery


Knowing which asset failed doesn’t help unless a crew can turn that finding into a record someone else can trust. By the time a crew radios in what they found, hours have often passed. The report that follows leans on memory: what the damage looked like, roughly where it sat, how bad it seemed. Since memory carries no timestamp and no GPS coordinate, it can’t attach to the asset that failed.


Structured field input tied to a specific asset record is what most water utility software expects. A phone call relayed hours later can’t provide it, since nothing about it is structured or asset-specific. Legacy field inspection software needs a live connection to work, and storms are exactly when that connection disappears.


Back at the office, ranking repairs from mismatched, unstructured reports slows things down further. A verbal report has no fixed structure, so one account might flag a valve issue while another skips coordinates entirely. Records built the same way every time make that ranking possible, and verbal reports rarely deliver that consistency.


A successful Public Assistance claim takes more than a radio report.[Federal guidance](https://www.fema.gov/sites/default/files/documents/fema_pa-simplified-procedures-policy.pdf) calls for a location, a photo, and a cost estimate tied to a specific damaged asset. That evidence helps create a defensible damage inventory and supports the scope-of-work and cost-estimating process. Applicants generally have[60 days after the Recovery Scoping Meeting](https://www.ecfr.gov/current/title-44/chapter-I/subchapter-D/part-206/subpart-G/section-206.202) to identify and report disaster-related damage and emergency work to FEMA; utilities should confirm the applicable deadline for their declaration. Incomplete field records burn through that window fast.


> **Customer story:**[How Brown and Caldwell cut water infrastructure data management time by 30%](https://www.fulcrumapp.com/customer-stories/brown-and-caldwell/) .


Each claimed repair also has to land in the correct FEMA funding category. A single storm can spread a utility’s damage across more than one category. A repair to a flooded pump station belongs in a different category than emergency isolation of a contaminated main. Getting that category wrong, or leaving it undocumented, can cost the utility part of its reimbursement. A utility’s water asset management software already ties every maintenance repair to a specific asset. A FEMA category works the same way, attaching to one asset at a time.


Paper forms and radio calls never produce a structured, asset-specific record, and every gap covered here comes back to that one fact.


## How water utility management software closes the field data gap


A storm-ready record needs three things in order: a form built before the storm, capture that works in the field, and a sync the moment coverage returns.


Fulcrum provides all three. It’s the platform field organizations use to run, record, and report and close out their work. Water utility crews use it as their water utility management software, built for the field.


Each capability below fits into that sequence. The resulting records give operations teams the structured inputs needed to update EAM/CMMS work orders, connect field findings to GIS data, and keep evidence linked to the affected asset.


- **Configurable forms.** A utility builds and loads a[damage assessment form](https://www.fulcrumapp.com/apps/damage-assessment-app/) before the storm, so it’s ready before the first crew heads out.
- **GPS stamping.** A GPS stamp attaches the instant a crew logs a finding, replacing memory-based location guesses.
- **Photo and video capture.** Fulcrum captures photos and video at the asset level, giving FEMA reviewers the visual evidence a claim needs.
- **Offline capture and sync.** Fulcrum keeps recording even when a storm takes cellular coverage down, then syncs automatically once a crew reconnects.


This approach helps turn emergency observations into usable water utility data management software records instead of isolated storm reports. Teams can create follow-up work orders, assess asset performance, and use the same history to inform predictive maintenance after recovery work is complete.


Routine asset mapping provides the direct blueprint for AWIA resilience planning. Utility teams convert the GIS spatial layers into pre-configured mobile forms before severe weather strikes. Running[emergency response](https://www.fulcrumapp.com/industries/water-utilities/) through those same everyday maintenance tools keeps field capture structured under pressure. Finally, on-site verifications then flow automatically into official, audit-ready records.


## Transforming emergency response data into long-term infrastructure intelligence


Speed and asset-level accuracy dictate how fast a utility recovers from severe weather. Pinpointing damaged mains, valves, and pump stations on day one gives dispatchers what they need to isolate breaks, control pressure zones, and protect public health.


Accurate field data supports asset recovery documentation and federal funding reviews. Attaching GPS stamps, damage photos, and precise cost categories directly to specific assets creates a clear evidence trail for FEMA reviewers. Clean records help teams meet required deadlines and substantiate eligible costs.


Emergency capture ultimately drives everyday asset management. Mobile entries sync automatically into existing GIS platforms, updating permanent service histories without adding manual paperwork. Complete storm documentation secures full federal reimbursement today and leaves utility operators with a clear, reliable record of their entire infrastructure for the storms to come.


> **Related reading:**[The lifecycle of field data collection for water infrastructure](https://www.fulcrumapp.com/blog/the-lifecycle-of-field-data-collection-for-water-infrastructure/)


## **See Fulcrum in action**


Water utility leaders rely on the AI-native Fulcrum Field Operations Management platform to support mobile field data collection from the initial inspection through final closeout workflows.


[Request a free custom demo](https://www.fulcrumapp.com/demo-request/) to see how accurate data, audit readiness, and field efficiency come together for your water infrastructure operations.
