---
schema_version: "1.0.0"
document_id: "c4d043637d1eedfd9eb019d1d3ebe2e5f97850b9ec9cdcae3079fb1a94fab1d6"
company_key: "emerson-electric-company-common-stock"
company: "Emerson Electric Company"
source_id: "emerson-electric-company-common-stock-rss-55c4887b1417"
canonical_url: "https://www.emersonautomationexperts.com/2026/industrial-software/quantifying-risk-sharpen-capital-operating-decisions/"
published_at: "2026-07-17T13:00:39+00:00"
first_seen_at: "2026-07-25T01:09:56.979097+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:0c6f136a4c7784b25f51281b1fae6d202691e4a491a713f0a9ea69fdca1de84f"
---

# Quantifying Risk to Sharpen Capital and Operating Decisions

In a recent Emerson presentation, **[Webinar with Becht: Maximizing Business Performance Through Quantified Risk](https://www.aspentech.com/en/resources/on-demand-webinars/maximizing-business-performance-through-quantified-risk?src=tw-global)** , Becht’s[Stephen DeLude](https://www.linkedin.com/in/stephen-delude-7084272b/) and Emerson’s[Michael Strobel](https://www.linkedin.com/in/mike-strobel-a2a67b7/) walked through how[Aspen Fidelis](https://www.aspentech.com/en/products/engineering/aspen-fidelis) quantifies risk across complex industrial systems so engineering teams can defend their investment choices. Their message was direct: every design and operating decision changes the flow of feedstocks, products, costs, and revenues, and those flows can be modeled before money is committed.


### Why It Matters


Project engineers and engineering, procurement, and construction (EPC) contractors live with constant tension between overspending capital and underspending opportunity. Equipment-level reliability models help, but they often miss what happens when storage, logistics, recycle flows, weather, and demand interact across the full system. Aspen Fidelis was built 26 years ago to close that gap by modeling the business, not just the equipment. The result is a clearer sense of the probability of meeting production, revenue, and return targets for each modeled scenario.


### Key Takeaways


- Aspen Fidelis is a System Performance Simulation that includes a reliability, availability, and maintainability (RAM) modeling toolkit that also drives production through the system flow sheet.
- Models can range from unit-level with a single event representing a whole plant to component-level detail inside a piece of equipment.
- Defining the critical question first sets the model boundaries, level of detail, and required data sources.
- “Key routines” let engineers code custom behavior such as yields versus catalyst activity, seasonal blending, and logistics queues.
- Most site models are built in three to eight weeks; very large, enterprise-level integrated networks may run three to five months.
- Financial outputs, including revenue, fixed and variable costs, return on investment (ROI), and net present value (NPV), can be tied directly to the model.


### Why Equipment-Only Models Fall Short


Michael explained that Aspen Fidelis was created specifically because traditional RAM modeling handled groups of equipment well, but struggled once the scope widened to include storage, flows, logistics, recycle flows, and turn-up capacity. He framed all business as flow: the flow of energy and emissions, of feedstocks and products, and of costs and revenues. Every decision changes those flows, and they are all linked. The platform’s purpose is to quantify what the future looks like under decisions A, B, and C, so teams can estimate the probability of success for production, revenue, and return on investment.


### Setting Up a Model for Success


Stephen walked through the build sequence. Engineers start with a system flow sheet that captures the full material balance: units, subunits, tankage, pipes, manifolds, and downstream dispositions to tank trucks, rail cars, or marine shipping. Inside each block, an event network functions as a reliability block diagram that covers single-point failures, redundant pairs, triply redundant systems, weather events, and maintenance outages, each with its own failure consequence, time to repair, and mean time between failures.


Modelers then set the flow sheet culpability reference unit (the area the culpability statistics are based on), prioritize flow paths, choose lifecycle length and seasonality, and select the number of Monte Carlo replications before running and analyzing results. Stephen emphasized that defining the critical question first is non-negotiable. That question dictates whether the model needs piping and instrumentation diagram (P&ID)-level detail or a process flow diagram (PFD)-level view, what reliability data is required, which operating modes matter, and which special features, such as weather or regulatory constraints, require custom code in key routines.


### Real Projects, Real Decisions


Becht’s recent work showed the range. In a petrochemical complex with three distinct feeds, the team modeled feed availability, feed-quality balance in storage, a simplified yield reaction, catalyst activity decline, and fouling over time, so the operator could commit to customers at a 90% production-reliability target and treat anything beyond that as export upside.


A refinery case answered a tankage question: could crude tankage be shifted to intermediate or product service to debottleneck back-end production? Modeling seasonal yields, gasoline blending for Reid vapor pressure (RVP) and octane, multiple production modes, and a ten-year tank balance confirmed the new configuration would not overfill and would still cover seasonal swings.


A world-scale natural gas conversion project tested an export terminal on the west coast of North America, including dock queues, storage tank levels, ship arrival and docking checks, weather impacts, and ultra-large carrier handling, and confirmed the dock fleet was correctly sized for the production rate.


A nitrogen rejection study for a North American energy producer sized the unit to meet tighter nitrogen specifications, driven by more gas heading to liquefied natural gas (LNG) service. The model showed that the base configuration met the 3% nitrogen target most of the time, that compressor availability was adequate, and that valve failures (with roughly 20% from instrumentation) were the right focus for improvement.


### What This Means for Project Teams


Michael closed on the practical balance: because the platform can model anything, discipline about what to include matters as much as capability. Every decision comes back to spending the least to make the most, and Aspen Fidelis is designed to show where a project is overspending capital or underspending opportunity. For EPC teams scoping new builds and for owner-operators defending reliability/improvement investments, that quantified view replaces argument with evidence.


**[Watch the on-demand webinar](https://www.aspentech.com/en/resources/on-demand-webinars/maximizing-business-performance-through-quantified-risk?src=tw-global)** and see how[Aspen Fidelis](https://www.aspentech.com/en/products/engineering/aspen-fidelis) simulates scenarios as a first line of defense against unquantified risk to plant performance and revenue **.**
