---
schema_version: "1.0.0"
document_id: "7a3766c98f5257ebf6fe70b7d62f927b9b4cfaf1208ab1c10357890cbf62cb91"
company_key: "fingermotion-inc-common-stock"
company: "FingerMotion Inc."
source_id: "fingermotion-inc-common-stock-atom-00741c8e7130"
canonical_url: "https://fingermotion.com/news/205-the-ballast-problem"
published_at: "2026-08-14T00:11:29+00:00"
first_seen_at: "2026-08-14T00:17:54.639903+00:00"
fetched_at: "2026-08-14T00:17:56.584620+00:00"
content_hash: "sha256:07889015614fc98e9c8e1f4a2ea7d476f829709af7fec85a28b43cd11e11fc8e"
---

# The Ballast Problem

*The hardest engineering problem in small-scale behind-the-meter compute is also the least discussed. It has an elegant solution.*


## Two machines with incompatible preferences


Put a gas reciprocating engine and an AI inference cluster on the same electrical bus and you have created a conflict.


The engine wants to run at or near its rated output, continuously. That is where its heat rate is best, where fuel consumption per kilowatt-hour is lowest, where its maintenance intervals are designed to sit, and where its capital cost is amortized most efficiently. Run it at forty percent load and the fuel efficiency degrades, the maintenance schedule tightens, and the cost per kilowatt-hour you are actually producing climbs sharply.


The inference cluster wants something else entirely. Its load follows customer demand, which is diurnal, bursty, and outside your control. It has quiet periods overnight and during model updates. It has spikes when a customer’s application sees traffic. Its average utilization is materially below its peak, and the gap between the two is not small.


This is the ballast problem, and at hyperscale it barely exists. A large grid-connected facility simply draws what it needs and the system operator handles the balancing across thousands of other loads and generators. The variability disappears into a much larger pool.


Behind the meter at sub-10 MW scale, there is no pool. You are the balancing authority for your own island, and the mismatch between what your generation wants and what your load provides is entirely your problem.


## Both obvious answers are bad


Size the generation to the compute peak, and your engines spend most of their life at partial load. You have paid for capacity you use intermittently, you are burning fuel inefficiently whenever you are below rated output, and your effective cost per delivered kilowatt-hour can be far above the number in your model. The economics that looked compelling on a spreadsheet assuming full utilization do not survive contact with a real load curve.


Size the generation to the compute average, and you have created a facility that cannot serve its customer during peaks. For a compute buyer, curtailment is close to a disqualifying property. They are running production inference against a service level commitment of their own; being told to reduce load at their busiest hour is not a trade they will accept at any discount.


Add batteries and you improve things at the margin, but storage sized to fill multi-hour troughs at these power levels is expensive enough to erase the cost advantage you built the site to capture.


## The shape of the actual solution


What the problem calls for is a second load with a very specific set of properties.


It has to be genuinely interruptible — not ‘interruptible with notice’ but capable of shedding in seconds, without human intervention, without damage, and without a customer to apologize to. It has to be able to absorb any quantity of power up to the full output of the plant. It has to have no service level obligation of its own. And it has to generate enough revenue per kilowatt-hour to be worth running rather than simply being a resistive dump load.


Bitcoin mining satisfies every one of these conditions, and it is close to the only thing that does.


A mining load can be curtailed to zero and restored in seconds under automated control. It has no customer, no SLA, and no state to lose — an interrupted hash attempt is simply abandoned with no consequence. It scales granularly, machine by machine. And it converts marginal electricity into a liquid commodity at a price that, while volatile, is knowable and hedgeable.


With that second load in place, the operating logic inverts. You size generation to serve the compute peak with proper redundancy. The compute load takes priority absolutely, at every instant. Every kilowatt-hour the compute load is not consuming goes to hashing. When compute demand rises, hashing sheds instantly to make room.


The generation runs at or near rated output continuously, which is where its economics are best. The compute customer never sees curtailment. And the marginal cost of serving that customer falls, because the fixed costs of the generation plant are spread across a machine that is fully utilized rather than one that idles.


## What this is not


This is not a bitcoin mining business with a data center attached, and the distinction matters.


In the configuration we are describing, the compute load is the priority load and the economic anchor. The mining load is infrastructure — a mechanism for converting otherwise-wasted generation capacity into revenue, and for keeping the prime movers in their efficient operating band. If mining revenue disappeared entirely, the site would still function; it would simply be less efficient.


That ordering has to be enforced in the control system rather than in a business plan, and it is worth asking any operator making this claim how the dispatch priority is actually implemented. A site where the mining load can outbid the compute load under some price condition is a mining site with marketing attached.


It is also worth being honest about the exposures this introduces. Hashprice is volatile and has been unkind to unhedged operators. Mining hardware depreciates aggressively. Some jurisdictions have taken explicit policy positions against mining load — British Columbia has permanently barred new grid connections for it, which is one of several reasons why behind-the-meter siting matters here rather than being merely convenient.


## Why this only matters at small scale


There is a reason this problem is not widely discussed: at hyperscale it does not arise, because the grid absorbs the variability.


Which is precisely why the solution is interesting. The load-following architecture is not a compromise forced on small operators by their lack of scale. It is a capability available specifically to operators who own their generation, control their dispatch, and are small enough to be nimble about it.


A 500 MW grid-connected campus cannot do this. A 5 MW behind-the-meter site can, and it converts what looks like a structural disadvantage — no grid to lean on — into a genuine cost advantage.


The ballast problem is real. Most of the small-scale behind-the-meter models that have failed did so because they did not solve it, or assumed full utilization and discovered otherwise. Solving it properly is most of the difference between a compelling model and a working site.
