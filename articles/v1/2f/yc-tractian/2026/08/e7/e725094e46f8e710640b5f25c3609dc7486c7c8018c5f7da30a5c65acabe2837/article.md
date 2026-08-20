---
schema_version: "1.0.0"
document_id: "e725094e46f8e710640b5f25c3609dc7486c7c8018c5f7da30a5c65acabe2837"
company_key: "yc-tractian"
company: "Tractian"
source_id: "yc-tractian-news-import-9393e6926c82"
canonical_url: "https://tractian.com/en/blog/machine-monitoring-for-high-mix-manufacturing"
published_at: null
first_seen_at: "2026-08-08T03:33:42.348710+00:00"
fetched_at: "2026-08-08T03:33:43.856238+00:00"
content_hash: "sha256:cc40a88c27a0774894753417436be7d0b508737c1f3f2c397de929a3def775e6"
---

# Machine Monitoring for High-Mix Manufacturing

## Key Points


- Vibration amplitude scales with speed and load, so on shared equipment a fixed threshold is a statement about the machine running one product being applied to every product it runs.
- Judging every reading against one envelope produces nuisance alerts during heavy work and silence during light work, which is how a single defect makes a monitoring program noisy and blind at the same time.
- Mechanical failure modes hold constant as multiples of shaft speed even as their absolute frequencies move, so a diagnosis referenced to speed stays valid across a changeover when a fixed threshold does not.
- Separating an asset's operating regimes instead of averaging them turns product variety into more evidence about the machine rather than less.


### **The Assets That Never Made the List**


Look at the covered asset list for a[condition monitoring program](https://tractian.com/en/glossary/condition-monitoring) in a plant that runs a wide product mix, and there is often a pattern in what made the cut. The plant air compressor is on it. The chilled water pumps are on it. The dust collector fan is on it. The presses, extruders, and packaging lines that actually run the mix are on a list for a later phase.


This pattern is rarely a budget decision, which means it’s not a statement about which assets matter most to the schedule. It is usually a technical retreat from a problem that surfaced during the pilot. What was the problem? It’s the fact that those machines were difficult to baseline. They kept producing readings the team could not interpret with confidence, so they were set aside in favor of equipment that holds steady.


The reason they were difficult is worth stating plainly, because everything else follows from it. Vibration amplitude scales with rotation speed and with load, and in high-mix production both of those change by design several times a week.


A threshold learned while an asset ran one product is an accurate description of that asset during that product and a progressively worse description of it during every other one. The machine has not become unpredictable. It has become several machines, and the monitoring system is still measuring it as one.


None of this is an argument for monitoring. If you are reading this, the sensors are installed, the data is arriving, and a program already exists in some form. So, the real question is narrower and considerably harder to answer. It is whether that program produces a diagnosis a[reliability engineer](https://tractian.com/en/who-we-serve/reliability-engineer) is willing to defend to production, on equipment whose operating conditions will be different by Thursday.


Most plants running a wide mix can say which assets their monitoring program covers. Fewer can say what each of those assets was running when its baseline was set.


This article examines why a baseline stops describing a machine in high-mix production, what the usual adaptations to that problem cost, what monitoring has to do differently to stay valid across a product mix, and what to verify before committing to an approach.


## Why the Baseline Breaks in High-Mix Production


***In a plant that runs many products across shared equipment, a condition threshold describes the machine doing one specific thing, and the machine spends most of its time doing something else.***


### **Speed and load set the amplitude before any fault does**


Vibration amplitude scales with rotation speed and with load. A servo running at sixty percent of the speed its baseline was learned at produces a measurably different signature while remaining mechanically identical. A CNC spindle cutting aluminum after a week of steel draws less current, runs cooler, and vibrates differently, and none of that is degradation.


Changeover adds a second variable that has nothing to do with wear at all. New tooling, different fixturing, and a different material change mass distribution and mounting stiffness on the machine itself, so the signature moves before the first part is produced.


This is not a marginal condition in high-mix work. It is the defining one. Research on high-mix, low-volume manufacturing lists[high variance in cycle time depending on product type](https://www.mdpi.com/2076-3417/13/3/1687) among the characteristics that distinguish the environment, alongside many product numbers in small quantities and non-standard routings. The equipment is being asked to behave differently on purpose.


Conventional[vibration analysis](https://tractian.com/en/glossary/vibration-analysis) was not built for that. The methods in widest use[assume a stationary operating environment](https://www.sciencedirect.com/science/article/abs/pii/S1474034625001831) , meaning a machine whose speed and load hold reasonably steady while it is being measured. High-mix production violates that assumption by design.


### **Alarms you get and alarms you don’t**


The first consequence is the one everyone talks about. Thresholds set during one product fire during another, and the team investigates a machine that turns out to be fine.


The second consequence is quieter and considerably more expensive. A threshold calibrated on heavy-duty operation sits above the amplitudes a light-duty run produces. A bearing genuinely degrading while the asset runs its lighter products can progress for weeks underneath that line without ever crossing it.


### **Nobody logs an alert that doesn’t happen**


The first error generates a work order and a complaint. The second generates nothing at all until the machine stops.


Both errors come from the same defect, which is evaluation against a single envelope rather than against like conditions. That is why a monitoring program in a high-mix plant can be noisy and blind at the same time. Those are not two problems that happen to coexist. They are one problem observed from two directions.


One threshold across every product


Fixed threshold


Heavy duty


Alert fires, nothing found


Light duty


Fault develops, no alert


Medium duty


Within range


One baseline per operating state


Per-state limit


Normal for this state


Deviation caught


Within range


## What the Workarounds Cost


***Teams will work out problems one way or another. This is exactly why the cost of “what works” is easy to miss - it’s what it took to work.***


**Widening the thresholds until the noise stops.** A team cannot investigate what it cannot trust, and an alert stream that is mostly wrong is worse than no alert stream. The cost is that detection moves rightward along the[P-F curve](https://tractian.com/en/glossary/p-f-curve-potential-failure-curve) , toward the point where the fault is unambiguous and the planning window has already closed. Quiet is purchased with lead time.


**Routing every alert through the one person who knows.** Usually a senior technician or a reliability engineer who can look at an alert, remember what was running, and decide whether it matters. That person is genuinely good at it. The cost is that the program now scales at the rate of one calendar, and the judgment that makes it work lives nowhere but in that person's head.


**Keeping the manual route running underneath as a confidence check.** Verifying before acting is correct engineering practice, so this rarely feels like a compromise. The cost is that the plant is paying for continuous monitoring and route-based inspection at the same time, and has effectively reverted to the second with extra steps in front of it.


**Leaving the hardest assets out of the program.** The assets with the most changeovers are the hardest to baseline, so they are the ones a pilot quietly excludes. The cost is that the equipment carrying the most product variety, and often the most schedule exposure, is the equipment nobody is watching.


On the assets with the most changeovers, pull the last quarter of alerts and count how many closed with no action found. Then ask what the team learned to do with alerts on those assets afterward. The first number measures data quality. The second is what it costs.


## What Regime-Aware Machine Monitoring Requires


***The correction is delivered when monitoring that knows what the machine was doing when the reading was taken, and evaluates that reading against the machine's own history in that state.***


### **Condition evaluated per operating state**


Operating state is not metadata attached to a measurement. It is what determines whether the measurement means anything. A reading of 4.2 mm/s is a fact about the machine only once you know whether the asset was loaded, idling, or running its lightest product at reduced speed when the sample was taken.


An asset that runs eleven products accumulates eleven histories rather than one blurred average. Deviation gets measured inside the relevant history, so a light-duty run is compared against previous light-duty runs and a heavy one against previous heavy ones.


This is the point where the high-mix condition stops being a liability. A plant running the same machine across many regimes eventually holds a richer picture of that machine than a plant that has run one product on it for six years, because it has observed the asset under stress it would otherwise never see. That advantage only exists if the regimes are separated. Averaged together, they cancel each other out.


### **Failure modes are ratios, thresholds are numbers**


A threshold is an absolute quantity in millimeters per second or in g. Absolute quantities move when speed moves, which is why they fail in this environment.


A[failure mode](https://tractian.com/en/glossary/failure-mode) is not an absolute quantity. A bearing outer race defect frequency, a gear mesh frequency, and a looseness harmonic are all defined as multiples of shaft speed. Expressed in orders rather than in hertz, they hold constant across operating regimes even though they land at completely different absolute frequencies when the shaft turns at 900 RPM and at 1,750 RPM.


The prerequisite is knowing shaft speed at the moment of capture, and this is precisely what most plants running variable-speed and drive-controlled equipment do not record alongside the reading.[Speed reference](https://tractian.com/en/resources/videos/vibration-sensor/rpm-encoder) is the difference between analysis that adapts to the machine and analysis that misreads it.


What changes for the reader is the output. Instead of a report that something rose, the finding names a defect, a severity, and how much time remains. The second is defensible in a conversation with production. The first is an opinion with a number attached.


### **Sampling by machine**


Short runs break interval-based sampling in a way that is easy to overlook. A sampling interval scheduled every four hours can land squarely inside a changeover and record a stopped machine, and on a two-hour run it can miss the asset entirely.


[Motion-triggered capture](https://tractian.com/en/resources/videos/vibration-sensor/always-listening) removes the need to build a sampling schedule around the production plan, which in a high-mix plant changes weekly.


### **A second domain fills in when the first one is unstable**


When mechanical baselines are unstable, an independent read helps. Current signature data referenced to load does not depend on the asset having a clean steady-state mechanical history, and it is cause-side rather than consequence-side. Mechanical sensing describes what is happening to a machine. Electrical sensing often explains why.


Research on[bearing and gear fault detection under variable speed and load](https://www.mdpi.com/1996-1073/17/3/643) works from exactly this premise, treating vibration and electrical signals together as the practical response to machines that operate under changing regimes.


Either domain alone is a real improvement over periodic inspection. The two together detect a class of fault that begins in one domain and surfaces in the other, which neither finds reliably by itself.


## What to Verify Before You Commit


***These are questions worth putting to any monitoring approach under evaluation, including one already installed and running.***


1. **Speed reference at the moment of capture.** Does the system know shaft speed for every reading rather than for some of them, and does it derive that speed without a tachometer or a control system tap that half the plant floor cannot provide? On older equipment, this is usually where the answer gets complicated.
2. **Operating state captured in the same data stream as condition.** Not reconciled afterward against a production system on a different clock. The specific question is whether readings are compared against like conditions or against one envelope covering everything the asset does.
3. **Output that names a failure mode with severity and remaining time.** A system that reports a change has handed the diagnosis back to you. Ask what an alert actually says on arrival, and whether answering it requires someone to open a spectrum.
4. **Findings that reach the system where work already gets executed.** Without replacing that system, and without a person retyping a finding into a work order. A diagnosis that stops in a dashboard has not reduced any risk.


Together, these separate a monitoring program that survives a changeover schedule from one that quietly stops being trusted while remaining fully installed.


## How Tractian Monitors Machines That Never Run the Same Way Twice


***Tractian built its machine monitoring on the assumption that a machine's operating state changes.***


### **Speed is derived from the signal itself.**


The RPM Encoder in Tractian's[machine monitoring](https://tractian.com/en/solutions/condition-monitoring) uses a proprietary algorithm to determine real-time rotation speed from the sampled vibration, across a very wide speed range, with no external tachometer and no control system integration. Every reading arrives with the speed that produced it. On variable-speed and drive-controlled equipment, that single fact is what allows order-based analysis to work at all, and it is available on assets that predate any protocol a modern system might expect to find.


### **Operating state and sampling on intermittent equipment**


The platform distinguishes idling, loaded, and stopped conditions so that readings are compared against like conditions, which exists specifically to keep false alarms from eroding a team's trust in the system.


Alongside it, motion detection triggers sampling when the asset actually runs.[Intermittent equipment and short production runs](https://www.youtube.com/watch?v=ejSj4LaVnWY&ref=ghost.tractian.com) get measured at the moments that matter rather than on a fixed interval built around a production calendar that will change next week.


### **Diagnosis grounded in what the machine is supposed to do.**


Auto Diagnosis draws on a library of motor and bearing specifications, including bearing fault frequencies, so the system knows the asset's physical characteristics before it evaluates anything. Detections resolve to named failure modes that carry severity and progression rather than threshold crossings, and[the reasoning behind a detection](https://tractian.com/en/solutions/condition-monitoring/insights-and-diagnosis) is visible to the engineer who has to justify acting on it.


The electrical layer extends the same approach.[Current profiling](https://tractian.com/en/solutions/oee/current-monitoring) distinguishes running, idle, reduced speed, setup, and stopped states directly from a machine's electrical signature, with no PLC integration and no control system access, which is what makes coverage practical on the mixed-vintage equipment most high-mix plants are actually running. Findings route into Tractian's own maintenance execution or[enrich the maintenance platform](https://tractian.com/en/solutions/cmms) a plant already runs, so a diagnosis becomes scheduled work rather than a notification.


**Learn more about Tractian's machine monitoring for variable-speed and high-mix equipment** to see how high-quality, decision-grade IoT data transforms your program into AI-powered maintenance execution workflows.


## FAQs about Machine Monitoring for High-Mix Manufacturing


### **Can condition monitoring work if our machines change speed and load between products?**


Yes, provided the system captures shaft speed and operating state alongside the condition data itself. What fails in high-mix production is not monitoring as a discipline but fixed-threshold alerting, which assumes an operating profile that does not hold. Once a reading carries the speed and state that produced it, the analysis can adjust rather than misfire.


### **Why does our monitoring system produce false alarms after every changeover?**


Because the alert threshold was learned while the machine was running something else. Vibration amplitude scales with speed and load, and changeover also alters tooling, fixturing, and material, all of which shift the machine's signature without any change in its mechanical condition. The system is correctly reporting a difference and incorrectly interpreting it as a fault.


### **What does regime-aware machine monitoring mean?**


It means condition is evaluated against the asset's own history in the operating state it was in when the reading was taken, rather than against a single baseline stretched across everything the machine does. An asset running eight products builds eight reference histories, and a light-duty reading is judged against previous light-duty readings.


### **Do we need a vibration analyst on staff to monitor high-mix equipment?**


Not if the platform produces named failure modes rather than raw signal changes. The staffing question is really a question about output format. A system that reports that a value rose requires an expert to interpret it, especially when operating conditions vary. A system that reports a specific defect with a severity and a time estimate can be acted on by the existing maintenance team.


### **How do you monitor machines that only run in short batches?**


With sampling triggered by machine motion rather than by a fixed schedule. Interval-based sampling can land inside a changeover and record a stopped asset, or miss a two-hour run entirely. Motion-triggered capture measures the machine while it is working, which is the only time the reading carries information.
