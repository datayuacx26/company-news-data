---
schema_version: "1.0.0"
document_id: "ce41fb1aa5b26c9d560f1d2bae7e5b2bb7f21478d58f93e6f4f91394871a0446"
company_key: "yc-ferveret-inc"
company: "Ferveret"
source_id: "yc-ferveret-inc-news-import-537315f9408d"
canonical_url: "https://www.ferveret.com/news/15-compute-efficiency-doesnt-sound-like-much-until-you-do-the-math"
published_at: null
first_seen_at: "2026-07-26T08:16:46.241701+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:842f2cda236bb0b6805b98a1f57cb5678f3f04b88fc0ce6c12b9d1d9c4ddbac1"
---

# 15% compute efficiency doesn't sound like much. Until you do the math.

Last week, we published the results of our UCLA benchmark study, showing that Ferveret's Adaptive Phase Cooling delivers 15% more TFLOPs per kW than direct-to-chip liquid cooling at the server level, with zero water consumption and a PUE of 1.03, both direct result of more efficient cooling.[Here is the link to the article.](https://www.businesswire.com/news/home/20260326215352/en/Ferverets-Waterless-Data-Center-Cooling-Delivers-15-Compute-Efficiency-Boost)


Some people looked at that headline and thought: "15%? That's it?" Let me put that number in context.


‍


## Think of it like engine cooling in a car


Every car engine is a thermal management problem. If the engine runs too hot, it loses efficiency, components wear faster, and in extreme cases, the engine throttles itself to prevent damage. Automotive engineers have known for decades that optimizing the cooling system directly improves engine performance and fuel economy (SAE International has published extensively on this, but that is a discussion for another day).


Now apply that same principle to a data center. Imagine two identical facilities with the same GPUs, the same infrastructure, the same software, and the same power contract. One uses a standard cooling system (direct-to-chip liquid cooling). The other uses an advanced cooling system that removes heat more efficiently. Same engines, same fuel. The facility with better cooling extracts more useful work from every watt, just like a car with optimized engine cooling extracts more miles from every gallon.


That is exactly what Ferveret does. We don't change the GPU. We don't change the software or infrastructure. We change how heat is removed from the chip at the server chassis level. That change produces 35% more compute output from the same facility: 15% at the server level from more efficient heat transfer, and an additional 17% at the facility level from dramatically lower cooling overhead.


Here is how:


‍


## Where your power actually goes


Jensen Huang told the world at GTC 2026 that the defining metric for AI infrastructure is tokens per watt. Not tokens per second. Tokens per watt. In his words: "Performance per watt that's unrivaled." He also said there is "a factor of two in efficiency through proper infrastructure design" and called that "gigantic" at the scale the industry is building. I could not agree more. He is absolutely right.


This raises a broader point that I believe our industry needs to address: if tokens per watt is the metric that matters, then data center infrastructure should have an efficiency grade, and a sustainability grade alongside it. Think of it like the EPA fuel economy rating on a car. Every car on the lot has a sticker that tells you its MPG. Today, no data center has a sticker that tells you its tokens per watt. PUE captures part of the picture, but it only measures facility overhead, not the full conversion efficiency from grid power to useful compute. The industry needs a comprehensive metric that captures the entire chain, from the power grid to the token output. But that is a subject for another article. For now, let's do the math with what we have.


*Let's follow 100 megawatts of electricity through two cooling architectures and see what comes out the other end.*


‍


## Direct-to-chip liquid cooling (PUE 1.2):


Out of 100 MW, approximately 17 MW is consumed by the cooling infrastructure: pumps, chillers, cooling towers. That leaves 83 MW actually reaching your GPUs. This is a simplified assumption since power distribution and other components have their own inefficiencies, but it serves as a reasonable baseline. DTC is the current industry standard for high-density AI workloads. It works. But it is not the most efficient way to convert watts into compute.


‍


## Ferveret Adaptive Phase Cooling (PUE 1.03):


Out of 100 MW, only 3 MW goes to cooling. 97 MW reaches your GPUs. That alone is 17% more power available for compute than DTC, like finding extra fuel in the same tank you already paid for. But here is where it gets interesting. Ferveret's APC operates at 45°C inlet coolant with zero water consumption, using controlled two-phase heat transfer. Our collaboration with UCLA showed that this approach produces 15% more TFLOPs per kilowatt at the server level compared to DTC, meaning more compute capacity is available for token generation from the same hardware.


What does that mean in practice? If each server produces 15% more compute per watt, you need fewer watts per unit of useful work. That means you can fit more servers into the same power envelope. More servers, more GPUs, more tokens, more revenue, all without adding a single watt to your power contract. Think of it this way: if your car gets better mileage, you can either drive farther on the same tank or carry a heavier load the same distance. Either way, you get more value from the same fuel.


‍


## The combined effect


When you stack these two advantages, 17% more power reaching the chip (from PUE) and 15% more compute per watt from the chip (from thermal efficiency), the gains compound. The total improvement is not 15%. It is approximately 35% more useful compute output than direct-to-chip cooling from the same power input.


Going back to the car analogy: you started with 30 MPG. Better engine cooling gave you 40.5 MPG. Same engine, same fuel, same road. You just improved the cooling.


‍


## What 35% improvement means in dollars value


With DTC cooling, approximately 83 MW of your 100 MW reaches compute. At $8M in annual revenue per MW of effective compute (based on current AI data center economics, with industry estimates ranging from $4M to $12M per MW depending on workload), that is $664M per year.


‍


‍


> **With Ferveret, you get approximately 35% more compute output from the same facility: roughly $896M per year. Approximately $230 million more annual revenue. Same building. Same power contract. Same GPUs.**


‍


**That is what 15% improvement in compute efficiency actually means.**


‍


*Same 100 MW. Same GPUs. Better cooling. +$230M/yr. Here's the math.*


###### **‍**


## And it fits in the same rack


This is the part that surprises people. Ferveret's Adaptive Phase Cooling is not a forklift upgrade. It is not immersion cooling where you submerge servers in a tank. Our system is a sealed, rack-compatible chassis that connects to the same liquid-cooled rack infrastructure data centers already use. Blind mates, quick disconnects, standard coolant loops. If your facility runs direct-to-chip today, Ferveret plugs into the same plumbing.


Think of it like upgrading the cooling system on the same engine block. The car looks the same. The engine is the same. The fuel is the same. But every mile produces more useful work and less wasted heat. You do not need a new garage, a new driveway, or a new mechanic. You need a better thermal system.


Ferveret can even run alongside existing DTC-cooled servers in the same rack, so operators can transition incrementally rather than all at once. No facility redesign. No new cooling plant. No retraining of staff.


‍


## And then there's water


US data centers are projected to consume more than 700 billion gallons of water annually. Most of that water evaporates in cooling towers, and it is becoming a real problem. In Texas, Arizona, and across the Middle East, water permits are now a binding constraint on where new data centers can be built.


Ferveret uses zero water. Not less water. Zero. Our system rejects heat through dry coolers in a fully closed loop. Nothing evaporates. Nothing is consumed.


This is not just an ESG talking point. It is a permitting accelerant. Operators using Ferveret can build in locations where traditional cooling would not be approved, and get permitted faster in locations where others are waiting years for water allocation.


‍


## Why this matters now


NVIDIA's next-generation Vera Rubin platform is 100% liquid cooled and specifies 45°C hot water inlet, a significant departure from the 25°C chilled water that most DTC systems use today. Most operators are not ready for that transition. They would need to eliminate chillers, redesign their secondary cooling loops, and rethink their entire thermal architecture.


Ferveret already operates at 45°C. We are not catching up to NVIDIA's spec. We are already there. And because our system uses zero water and operates above ambient temperature in most climates, we can help operators build AI infrastructure in locations that are currently unviable due to water scarcity and high ambient temperatures, from West Texas to the Arabian Peninsula.


Last week, KKR sold CoolIT Systems, a direct-to-chip cooling company, to Ecolab for $4.75 billion, generating 15x returns on their 2023 investment. CoolIT provides the exact category of cooling technology that our Adaptive Phase Cooling outperforms by 15% at the server level and approximately 35% at the system level.


The market has spoken: data center cooling is a multi-billion dollar opportunity. The question is no longer whether liquid cooling wins. It is which liquid cooling technology produces the most tokens per watt, uses the least water, and fits into the infrastructure operators are already building.


‍


‍


> **The next time someone tells you 15% efficiency gain is not a big number, ask them what an extra $230 million revenue a year looks like. That is what better cooling is worth.**


‍
