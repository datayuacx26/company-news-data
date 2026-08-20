---
schema_version: "1.0.0"
document_id: "2a39c4d6b209165227ad1a263bd15a0628670139f09a3fc54567ef2895073c98"
company_key: "astera-labs-inc-common-stock"
company: "Astera Labs Inc."
source_id: "astera-labs-inc-common-stock-rss-6ea615f4bb0d"
canonical_url: "https://www.asteralabs.com/resources/blog/the-1-2-meter-challenge-understanding-your-passive-copper-options-at-224g/"
published_at: "2026-07-15T17:46:34+00:00"
first_seen_at: "2026-07-20T23:21:25.038635+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:14b7bbe2216dfaa85834e1ed7f8554acdb05212d200e293c8165d8ab2f56f726"
---

# The 1.2-Meter Challenge: Understanding Your Passive Copper Options at 224G

You have 40 dB of channel budget and 1.75 meters to traverse in a cable backplane in a modern AI rack. At 224G, those two numbers don’t play nicely together. But the evolution of the AI infrastructure landscape is making this a problem we can’t ignore: We’ve gone from thinking about compute in terms of an 8-GPU chassis to designing systems around 72, 144, or even more accelerators in a single rack. We’re rethinking the rack itself as the unit of compute.


But while industry hype rightly points toward[optics as the logical next step for scale-up connectivity](https://www.asteralabs.com/the-400g-per-lane-inflection-point-where-copper-and-optical-meet-in-ai-infrastructure/) , copper is still an attractive solution in the near term, especially when it comes to your supply chain and TCO. You have several copper options that can help you stay within your channel budget, but it’s more important than ever to understand the physics behind them before you make the choice for your backplane.


## The Physics Problem Nobody Can Ignore


Let’s start with the fundamental constraint: insertion loss. As we’ve moved from 112G to 224G signaling, we’ve doubled the Nyquist frequency from 26 GHz to 53 GHz. This is a significant leap that affects every millimeter of copper in your signal path.


At 53 GHz, a standard 30AWG twinaxial cable exhibits approximately 10.23 dB of insertion loss per meter. In a typical integrated AI rack where compute and switches sit in the same enclosure, you might get away with 1.2-meter cable runs. Doing that math indicates about 12.3 dB of loss just from the cable in this deployment, leaving you roughly 28 dB for everything else: PCB traces, connectors, package losses, and margin for aging and temperature variation.


Now consider what’s happening in next-generation rack architectures. We’re disaggregating systems into discrete and adjacent compute, power, cooling, and networking racks to free up space for more accelerators. That’s great for compute density, but it creates new challenges in terms of cable length. In a compute-only rack configuration, the longest cable run can stretch to 1.75 meters, a 46% increase. With 30AWG cable, that’s approaching 18 dB of loss before you’ve accounted for anything else in the channel.


The 40 dB channel budget has served us well at lower speeds, but at 224G, we’re pushing right up against its limit with passive copper alone.


### Pros and Cons of Thicker Cable


Your instinctive response to the channel budget crunch might be, “Use 26AWG cable for lower loss.” It’s a good idea: At 53 GHz, 26AWG exhibits about 7.14 dB/m insertion loss, roughly 30% less than 30AWG. For that 1.75-meter compute rack run, you’re looking at 12.5 dB instead of 18 dB, a meaningful improvement that could keep you within budget.


But in the real world, this would create a problem of physical space. A bundle of 64 differential pairs (128 conductors) in 26AWG cable is approximately 50% larger in cross sectional area than the same bundle in 30AWG. That might not sound like much until you consider what happens in the back of an AI rack. In a modern backplane cartridge, you’re routing thousands of connections that already compete for space with high-voltage busbars delivering kilowatts of power and liquid cooling manifolds managing heat loads that would have seemed absurd five years ago.


And then there’s bend radius. Industry practice calls for a minimum bend radius of 5-7 times the cable diameter. That 50% increase in bundle size translates directly to more space required for service loops and routing flexibility. In an OCP Open Rack V3 with a 21-inch width, every millimeter counts. The rear face plate is fighting for space, and proper cable management must account for airflow, serviceability, and the ability to actually build these systems at scale.


I’ve seen rack designs where switching to 26AWG would have required a complete mechanical redesign. Changes on that scale are program-level decisions with implications for manufacturing, supply chain, and time-to-market.


### The Disaggregation Dilemma


Another trend impacting the geometry of the backplane is disaggregation. Disaggregation is the right architectural direction for all the reasons everyone talks about: independent scaling of compute and infrastructure, better resource utilization, easier maintenance. But it also introduces new signal reach challenges.


When power, cooling, and networking lived in the same rack as your GPUs, cable lengths were constrained by the height of that rack, typically 16 rack units for the longest run, translating to the 1.2 meter length we’ve been discussing. Move those infrastructure components to adjacent racks, and suddenly you need to reach farther. The compute tray at the top of one rack needs to connect to switches that might be in the middle of the same rack, or increasingly, in a neighboring rack entirely.


A 1.75-meter reach is no longer your worst-case scenario. In row-scale deployments or with Open Rack Wide (ORW) double-wide configurations, you could be looking at 2+ meter runs. At those distances, passive 30AWG copper simply doesn’t have the channel budget, and 26AWG copper creates mechanical challenges that may be unsolvable within the constraints of standard rack dimensions.


## Passive Copper Trade-Offs: Optimizing What You Can Actually Control


So where does this leave system architects? You have three variables to work with:


### 1. Cable Length


This is often the least flexible variable because it’s dictated by your rack topology. If you’re deploying 72 GPUs with switches in the middle of the rack, geometry determines your cable lengths. You can optimize around the margins by placing switches more strategically or using shorter chassis, but fundamentally, you can’t cheat physics or spatial relationships.


### 2. Cable Gauge


Here you have real choices, but they have the pros and cons we discussed above:


- **30AWG:** Smaller bundles, better flexibility, easier cable management, but higher insertion loss
- **26AWG:** Lower insertion loss, extended passive reach, but 50% larger bundles, more difficult routing, greater bend radius requirements


The decision isn’t purely technical. I’ve worked with customers who chose 30AWG despite the signal integrity challenges because their rack mechanical design couldn’t accommodate 26AWG bundles. Others went with 26AWG and redesigned their entire backplane cartridge to handle the larger cables. Both approaches can work, but you need to make the decision consciously, understanding the downstream implications.


### 3. Channel Budget Allocation


This is where signal integrity engineering really matters. Your 40 dB budget has to cover:


- PCB traces on the GPU and switch boards
- Package losses
- Connector interfaces (backplane connectors, typically high-density designs from vendors like Amphenol and TE Connectivity)
- The cable itself
- Margin for manufacturing variation, temperature effects, and aging


If you can reduce losses in the PCB through careful stack-up design, better materials, or shorter traces, you’ll free up more budget for the cable. Some teams are exploring flyover cable assemblies that bypass PCB traces entirely, though these add cost and complexity.


## Three Practical Options For Passive Copper Backplanes Today


Given these constraints, what can you actually do?


### Option A: Stay at 1.2m or Less With Passive Copper


This works if you can constrain your rack architecture. Integrated racks where compute, switches, and infrastructure coexist can often stay within these limits. You’re typically limited to classic scale-up configurations with smaller-scale deployments of maybe 8-16 GPUs per chassis. For many applications, particularly inference workloads that don’t require massive GPU counts, this remains viable.


**The upside:** Lowest power consumption, simplest architecture, proven reliability.


**The downside:** You’re capped on scale-up cluster size within a single rack.


### Option B: Move to 26AWG Cable for Longer Runs


If you need to extend to 1.5-1.75 meters passively, 26AWG becomes attractive from a signal integrity standpoint. You’ll stay within your channel budget (barely) but you’re taking on mechanical challenges.


This approach works best if you’re designing a new rack from scratch and can accommodate larger cable bundles, you have flexibility in your rear face plate layout, or your deployment scale justifies the engineering investment.


**The upside:** Extends passive reach significantly.


**The downside:** Bundle size, bend radius, potential rack redesign, and you’re still limited beyond 2 meters.


### Option C: Accept Shorter Effective Reach With 30AWG


Some architectural approaches flip the problem. Instead of extending reach, they bring the switches closer to the GPUs. This might mean more switches per rack, different topologies, or accepting that some connections will be shorter while optimizing the overall system around that constraint.


This is actually more common than you might think. I’ve seen clever rack designs that place switch trays strategically so that most connections are sub-1 meter, with only a few longer runs that might need special treatment.


## What’s Coming Next for Copper: Emerging Options


The reality for many next-generation AI deployments is that passive copper alone won’t be sufficient. The combination of higher speeds, longer distances, and increasing GPU counts per rack is pushing us beyond what passive solutions can support.


The industry is converging on three architectural approaches for copper cabling to extend beyond passive limits:


1. **Universal retiming at system boundaries:** Placing signal regeneration on every GPU and switch board.
2. **Selective mid-span retiming:** Strategically placing signal conditioning only where needed.
3. **Hybrid passive/active strategies:** Mixing shorter passive links with longer active links in the same backplane.


Each approach has different implications for power consumption, cost, complexity, and performance. Some deployments might use 512 retimers across a rack; others might need only 183 by selectively placing them where the signal budget demands it.


## Why Your Copper Choices Matter, Today and Tomorrow


Right now, engineering teams are making decisions about rack architectures, cable specifications, and backplane designs that will shape their AI infrastructure for the next 2-3 years. Get it wrong, and you’re either over-engineering (spending more on solutions you don’t need) or under-engineering (hitting performance walls that force expensive redesigns).


You need to make informed decisions based on your specific deployment requirements:


- What’s your cluster size?
- What protocols are you running?
- What’s your rack mechanical design?
- What’s your power budget?
- How much flexibility do you need for future expansion?


These aren’t questions with universal answers., but understanding the physics constraints of insertion loss, cable gauge trade-offs, and channel budgets should give you the foundation to make the right choices for your architecture.


Cable backplanes aren’t going anywhere. They’re the densest, most cost-effective copper solution for rack-scale connectivity. But as we push to 224G and beyond, we need to be smarter about how we design around their limitations. Check out[our recent white paper on reinventing the backplane](https://www.asteralabs.com/document-requests/?document=Reinventing%20the%20Backplane%3A%20Why%20AI%20Demands%20an%20Active%20Approach) for some more creative active copper solutions.
