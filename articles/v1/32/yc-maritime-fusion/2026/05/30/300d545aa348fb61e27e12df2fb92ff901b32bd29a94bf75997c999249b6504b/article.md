---
schema_version: "1.0.0"
document_id: "300d545aa348fb61e27e12df2fb92ff901b32bd29a94bf75997c999249b6504b"
company_key: "yc-maritime-fusion"
company: "Maritime Fusion"
source_id: "yc-maritime-fusion-news-import-e177c5f92499"
canonical_url: "https://maritimefusion.com/blog/fusion-first-big-market-ocean-not-grid"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-22T03:23:18.981080+00:00"
fetched_at: "2026-07-28T22:12:51.498055+00:00"
content_hash: "sha256:8717fb50738535aeac43179bd15d6fa3243f7cc7abfd092ec28a86ca64d0372d"
---

# Yinsen Physics Basis Paper

# Why Fusion's First Market Could Be Off-Grid Power


Most compact fusion programs are chasing the same prize: the highest possible power density, packed into the smallest possible machine, to make fusion cheap enough to compete on the electric grid. Our new reactor concept called **Yinsen** asks a different question. What if the first useful fusion plants aren't built to beat the cheapest electron on the grid at all — but to serve places where power is expensive, fuel logistics are painful, and energy independence is worth a premium?


Yinsen, described in a[May 2026 arXiv preprint](https://arxiv.org/pdf/2605.04190) in collaboration between us and Columbia University, is a high-temperature-superconducting (HTS) tokamak designed for off-grid applications: maritime propulsion, remote power, and industrial energy supply. Its central move is counterintuitive. Instead of pushing power density up, the design deliberately pulls it *down* — trading peak performance for a machine that is far easier to build, operate, and keep running.


## The core idea: build a reactor that lasts


Aggressive grid-fusion concepts generally need a blanket-area-normalized fusion power density of roughly 5 MW/m² or more to recover the capital cost of a complex nuclear plant. At those loadings, the machine has to survive hundreds of megawatts of power funneled into the divertor, intense neutron flux, and rapid radiation damage to its internal structures. Even under optimistic assumptions, that regime implies tearing out and replacing major in-vessel components on a regular schedule — and every replacement campaign means remote handling, activated materials, long outages, and expensive downtime.


Yinsen takes the opposite bet. The design is anchored to a materials-limited fusion power density of just **0.7 MW/m²** . That number isn't arbitrary: it falls directly out of four assumptions — a 35 displacements-per-atom (DPA) structural damage limit for the vanadium-alloy (V-4Cr-4Ti) vacuum vessel, a 20-year plant lifetime, a conservative 40% utilization floor, and a first-order correction for where damage peaks. Put plainly, the reactor is sized around what its solid structures can survive over two decades, not around what the plasma could momentarily produce.


The payoff is that routine vacuum-vessel replacement never has to become part of the business model. The vessel is treated as a lifetime component, with a budget of about **1,040 MW·yr** of integrated fusion energy before it reaches its damage limit — exactly 20 years at the 130 MW, 40%-utilization baseline.


## Why ships are an attractive first customer


The electric grid is one of the most cost-sensitive markets on earth. Solar, wind, gas, storage, and existing plants all compete relentlessly on price, and a first-of-a-kind fusion plant would be walking into that fight at a disadvantage.


Ships are different. Large vessels need enormous, reliable power; they operate far from infrastructure; and they're exposed to fuel-price swings, port constraints, geopolitical supply risk, and tightening emissions rules. In that setting, the *attributes* of the power source — dispatchability, fuel security, islanded operation, decarbonization — often matter more than levelized cost alone.


Yinsen targets a reactor class around **130 MW of fusion power** , enough to clear **25 MWe of net electric output** once realistic recirculating loads (cryogenics, heating, pumping, tritium processing) are subtracted. The paper works through a marine case built around a 25–70 MWe permanent-magnet propulsion motor fed by a 34 kV medium-voltage backbone. That's not enough to light up a city — it's enough to move serious industrial assets: container ships, remote facilities, islanded grids, and energy-hungry sites where delivering fuel is itself the binding constraint. Notably, the 130 MW design point sits *below* SPARC's projected peak fusion power of roughly 140 MW, underscoring that this is a deliberately modest machine.


## The "less aggressive" design is the more buildable one


What makes Yinsen interesting is that lowering power density relaxes a whole chain of hard problems at once: vacuum-vessel lifetime, shielding burden, divertor heat flux, cryogenic duty, tritium inventory, and balance-of-plant transients all get easier together. The result is a more buildable reactor that still produces useful net power.


The supporting analysis bears this out across the plant:


- **Plasma.** Integrated FUSE modeling refines the concept into a self-consistent high-field baseline — a shaped 9.29 T, 9.67 MA plasma in a machine 3.18 m in major radius — sustained with just 15 MW of ICRH at 140 MHz. The plasma gain is Q ≈ 9.6 at 130 MW and Q ≈ 13.7 at the higher 185 MW operating point. The point isn't that the plasma is pushed to its limit; it's that it doesn't need to be.
- **Divertor.** Without impurity radiation, the unmitigated peak heat flux on the divertor target approaches 100 MW/m². Adding a modest amount of neon to drive detached operation cuts that by nearly two orders of magnitude — below 10 MW/m², and in the deepest detached cases down to roughly 1 MW/m².
- **Magnets and neutronics.** OpenMC calculations with a double-layer tungsten-carbide / tungsten-boride (WC/W₂B₅) shield confirm that the vacuum vessel is the lifetime-limiting structure, while the HTS magnets are comfortable lifetime components. Total toroidal-field nuclear heating is only about 7.4 kW at 20 K at the baseline, and the magnets' fast-neutron limit corresponds to roughly *sixteen* vacuum-vessel lifetimes — about 315 calendar years.
- **Fuel cycle.** The blanket reaches a tritium breeding ratio of about 1.1 with only ~30% lithium-6 enrichment and *no* dedicated neutron multiplier, thanks in part to the relative neutron transparency of the vanadium-alloy structure.


## A plant, not just a plasma device


A reactor only matters if it behaves like a usable power plant. Yinsen pairs its FLiBe-cooled core with a supercritical CO₂ balance of plant — chosen because the FLiBe operating window (roughly 800–973 K) is well matched to compact Brayton-cycle conversion, and because supercritical CO₂ turbomachinery is dense enough to fit aboard a ship. Across a 130–480 MW fusion-power sweep, cycle efficiency holds near 47.5%, while overall plant efficiency climbs from about 43% to 50% as fixed parasitic loads get diluted at higher power.


To handle pulsed operation, the design uses a 34 kV medium-voltage backbone with solid-state transformers feeding a regulated DC bus, and a hybrid battery-plus-capacitor storage plant to buffer the fast magnet transients during plasma ramp-up. This isn't a side detail — for an off-grid machine in this power class, the pulsed-power architecture is part of the core viability argument.


## The takeaway


The paper is careful about what it is: a concept study with many assumptions still needing validation, not a finished commercial reactor. Its FOAK direct-capital screening estimate lands around 713 MUSD, falling toward ~534 MUSD with manufacturing learning — still above immediate parity with conventional marine propulsion, but within striking distance for high-speed, carbon-priced shipping rather than an order of magnitude away.


Its strongest contribution is strategic. It shifts the conversation from "Can fusion make net power?" to "Where can first-generation fusion be useful *before* it's cheap?" New technologies rarely start by replacing the cheapest incumbent everywhere; they start where the incumbent is most painful. For fusion, those pain points — fuel logistics, emissions pressure, remote operation, energy security, high-power autonomy — are sharpest at sea.


Yinsen isn't arguing that low-power-density fusion is the destination. It's arguing that a deliberately less aggressive reactor may be the fastest route to a useful one. In that market, fusion doesn't have to be the cheapest electron on the grid. It has to be the most valuable power source onboard.
