---
schema_version: "1.0.0"
document_id: "7640a76065477723b197fcb09b96b49e6063251bf8c26aa5cdfc43501c43e136"
company_key: "yc-miracle"
company: "Miracle"
source_id: "yc-miracle-news-import-3cf27986a0e2"
canonical_url: "https://www.miracleml.com/blog/why-clinical-trial-drug-supply-still-runs-on-excel-and-how-to-fix-it"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-25T15:34:06.478198+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:0ace981a9dcb1581ef8d8a5f315b7ac9790c65e47c6cf6ae6442af48f09ed52d"
---

# Why Clinical Trial Drug Supply Still Runs on Excel and How to Fix It

In clinical trials, ensuring the right drug supply is available at the right site, at the right time, is far more complex than simple inventory tracking. It often requires tight coordination between Clinical Operations (ClinOps) and Chemistry, Manufacturing, and Controls (CMC) teams, two groups that often operate with different systems, data sources, and priorities.


At the heart of this challenge lies a deceptively simple question: Do we have the right number of kits at each site to support upcoming patient visits without overstocking and risking waste?


## The Core Problem: Disconnected Data, Shared Responsibility


ClinOps teams are responsible for patient activity: tracking enrollment, randomization, and visit schedules, typically through EDC systems. Meanwhile, CMC teams manage drug supply logistics through RTSM/IRT systems and depot inventories.


These systems rarely communicate seamlessly.


As a result, reconciling supply and demand becomes a manual, error-prone process:


- ClinOps must estimate upcoming visits based on patient status
- CMC must assess available inventory across depots and sites
- Both teams must align on whether supply meets projected demand


Without integration, this often leads to one common workaround: spreadsheets.


## The Reality: Manual Reconciliation on Excel


In many biotech companies, teams export data from multiple systems and attempt to reconcile it manually:


- Pull kit inventory and shipment data from RTSM/IRT or depot systems
- Extract patient visit schedules or statuses from EDC
- Project visit windows for each enrolled patient based on the study’s schedule of assessments
- Estimate kit demand based on protocol-defined dosing schedules
- Compare projected demand vs. on-site inventory


This process is not just time-consuming, but also it’s inherently fragile. Small discrepancies in timing or the person responsible for trackers being out of office can lead to significant downstream issues.


## The Balancing Act: Avoiding Shortages and Waste


There are several ways to optimize drug supply inventory at sites.


### 1. Preventing Stockouts at Active Sites


Sites with a high number of upcoming visits must have adequate kits available. This requires:


- Accurate projection of visit windows based on randomization or baseline events
- Understanding protocol-specific dosing requirements
- Accounting for variability in patient adherence and visit timing


Failure here risks missed doses, protocol deviations, and compromised study integrity.


### 2. Avoiding Overstocking at Low-Activity Sites


Conversely, sites with few or no upcoming visits often accumulate excess inventory. Over time, this leads to:


- Kit expiration and waste
- Increased storage and handling burden
- Inefficient use of limited drug supply


### 3. Redistributing Supply Intelligently


To mitigate imbalance, teams may need to:


- Reallocate kits between sites
- Adjust resupply thresholds
- Pause shipments to low-activity sites


But these decisions depend on accurate, real-time visibility across both supply and demand.


## The Missing Link: Forward-Looking Demand Projection


A key complexity in reconciliation is that upcoming visits at sites need to have projected visit windows and also accommodate out of window visits as necessary.


Teams must anticipate:


- When each patient is likely to return for their next visit
- Which visits require drug dispensing
- How many kits will be needed per visit
- Adequate buffer for potential out of window visits


This often involves building visit forecasts from:


- Randomization dates
- Baseline visits
- Protocol-defined visit windows


Without automation, these projections are manually constructed and difficult to maintain as studies evolve.


This view illustrates how projected patient visits translate into cumulative kit demand over time, compared against available and incoming supply. As demand begins to exceed supply, teams can proactively identify potential stockout risks and adjust resupply or redistribution strategies accordingly.


## Why Current Workflows Break Down


The core issue is the lack of connectivity between systems that forces teams to track this information in trackers manually.


- EDC systems capture what visits have occurred (patient visits)
- RTSM/IRT systems track what is available (kits and inventory)
- Depot systems manage where supply resides


But without a unified view, teams are forced to bridge the gap themselves.


This leads to:


- Delayed decision-making
- Reactive supply management
- Increased operational risk


## Toward a More Integrated Approach


To move beyond manual reconciliation, organizations need a more cohesive strategy:


### 1. Unified Data Layer


Bringing together EDC, RTSM/IRT, and depot data into a single source of truth enables real-time visibility into both supply and demand.


### 2. Automated Visit Forecasting


Leveraging patient-level data to dynamically project upcoming visits reduces reliance on static assumptions and manual tracking.


### 3. Intelligent Supply Optimization


With integrated data, teams can:


- Identify sites at risk of stockout
- Flag excess inventory before it expires
- Recommend redistribution or resupply actions


### 4. Cross-Functional Alignment


A shared view allows ClinOps and CMC teams to operate from the same playbook—improving collaboration and reducing friction.


## From Reactive to Proactive: A New Operating Model


At Miracle, we’ve been working closely with clinical teams to address this exact challenge by unifying fragmented data and making it actionable.


By integrating directly with EDC systems, we enable teams to visualize patient activity in a forward-looking way: surfacing upcoming visit windows based on protocol-defined schedules of assessments, including minimum and maximum visit days. This allows ClinOps and CMC teams to move beyond static snapshots and understand *when* demand will occur, not just *what* has already happened.


In parallel, we aggregate drug supply data across RTSM/IRT and depot systems to provide real-time visibility into inventory levels at the site and country level. This includes the flexibility to account for different protocol versions, ensuring that supply and demand are aligned even as studies evolve.


The real impact comes from connecting these datasets. With a unified view, Miracle enables proactive alerts that flag potential risks ahead of time. Whether it’s an impending stockout at a high-enrolling site or excess inventory at a low-activity site that may expire. Instead of relying on manual reconciliation in Excel, teams can take timely, data-driven actions to rebalance supply, reduce waste, and maintain study continuity.


Another capability that has proven especially valuable is the ability to visualize cumulative visit demand alongside drug supply projections over configurable time horizons. Shorter-term views help teams make informed ordering and resupply decisions, while longer-term projections support budgeting and forecasting at both the study and country level.


This also ties into how many teams operationalize site strategies. Within site trackers, sites are often categorized based on expected enrollment behavior, such as high-supply or low-supply strategies. By layering projected visit demand against actual supply and enrollment trends, teams can validate whether sites are tracking against expectations, identify deviations early, and adjust allocation strategies accordingly. This creates a feedback loop between planning assumptions and real-world performance, improving both supply efficiency and financial predictability.


## Conclusion


Reconciling clinical trial drug supply can be viewed as a data integration challenge that currently requires teams to use manual trackers to bridge systems together.


As trials grow more complex and timelines tighten, relying on manual Excel-based workflows is increasingly unsustainable. By connecting patient activity with supply data and introducing forward-looking projections, organizations can move from reactive firefighting to proactive optimization.


The result: fewer stockouts, less waste, and a more efficient path to delivering therapies to patients.


‍
