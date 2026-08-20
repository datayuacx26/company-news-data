---
schema_version: "1.0.0"
document_id: "e363a7d2dbd68a0e63957f6358d8656c1137d4ab350d71acbf859e10ae3e03e5"
company_key: "yc-helion-energy"
company: "Helion Energy"
source_id: "yc-helion-energy-news-import-5ae3e56c82e0"
canonical_url: "https://www.helionenergy.com/blog/measuring-electricity-production-from-fusion-electrical-diagnostics"
published_at: "2025-11-17T00:00:00+00:00"
first_seen_at: "2026-07-21T22:45:07.501995+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:860a6cbaf24cec5f0b059fbe19450ccef4d82a776e3107b2afc9130f6d3f16cc"
---

# Measuring electricity production from fusion: Electrical diagnostics

Helion takes a unique approach to generating electricity directly from fusion, removing the costly and inefficient steps associated with steam generation. For us to understand exactly how much electricity we are directly recovering from fusion, it is critical we measure accurate data. We do this through a suite of custom-built electrical diagnostics developed and integrated by our team. They give us an immense amount of data to help us understand machine performance.


### A fusion system with no steam cycle


Helion’s machines recover fusion energy directly in our electromagnetic system via induction, unlike traditional steam-cycle power plants. This approach greatly increases efficiency, reducing the amount of fusion needed to produce meaningful power.


Here’s how it works: during each fusion pulse, energy from capacitors powers magnetic coils that form and translate two plasmoids into a central compression section, where they merge. Once merged into a single plasmoid, it is further compressed and the particles within the plasma fuse, and the energy released by the fusion process causes the plasma to expand and push back on the electromagnetic field, driving current that recharges the capacitors (think Faraday’s Law of Induction).


(As an aside: Crucially, if there were no plasma present and no fusion occurring, the system would still recover a majority of the energy in a discharge, barring resistive losses from electrical resistance in the system. Hence, the amount of fusion needed is simply a factor of electrical loss, which is only a small portion of the total energy in the system.)


In contrast, steam-cycle plants convert fuel to heat, then to steam, then to turbine motion before finally generating electricity by spinning generators. This takes many more steps to do essentially the same thing (use magnets to generate electricity), but with greater losses.


Our direct energy recovery makes the process highly efficient and raises an important question: How much electricity are we creating?


We have[plasma diagnostics](https://www.helionenergy.com/articles/how-we-know-whats-happening-inside-a-fusion-plasma-plasma-diagnostics/) to understand how our plasmas are performing and[neutron diagnostics](https://www.helionenergy.com/articles/measuring-fusion-reactions-neutron-diagnostics/) to help us understand what energy we’re creating in neutrons, but one final piece to the fusion electricity puzzle is establishing how much of the fusion energy ends up in our capacitor bank, which we get through electrical diagnostics.


### Measuring fusion electricity


At its core, the math is simple. The energy balance for our fusion machines is:


E_final = E_initial + E_fusion – E_loss


Where E_final and E_initial represent energy stored at the end and beginning of a pulse, respectively, E_fusion represents the energy created through fusion, and E_loss represents the losses during each pulse (heating in the current carrying components, snubbing components, etc.).


We know that energy on a capacitor is 1/2 CV2. For the equation above then, we can replace E_final and E_initial with this calculation at pulse start and end.


This means that we need to know the following information to calculate energy created from fusion, and measured as our final energy on the capacitor bank:


**•** Capacitance


**•** Initial and final voltage


**•** Losses that occur in the fusion process


My team has executed a rigorous process to understand each of these.


**Capacitance.** Our capacitance is established by our capacitor bank, which is made up of thousands of high-voltage capacitors. Some that we built in-house and some we purchased from external vendors. When built, each capacitor comes with some tolerance on its capacitance. Our measurements require us to know capacitance more accurately than the stated tolerances, so we test every capacitor’s capacitance prior to installation. The team tracks each capacitor throughout its lifetime, so we know at all times how much capacitance is in our system, enabling precise measurements for every pulse.


**Initial and final voltage.** Our team has built a set of hardware dedicated to making precision measurements of energy on the bank. It is comprised of a set of diagnostic circuits that provide high accuracy measurements of voltage on our capacitors before and after every pulse. One to do precise voltage division at high voltage, one for analog-to-digital conversion, and another for very practical low noise galvanic isolation, these boards give us more than 99% accuracy (in a bank charged to tens of kilovolts, I cannot overstate how impressive this is).


**Losses.** Even at circuit efficiencies above 90%, we know there are still losses in our system that we must factor into our calculations. To understand them, we review two things: 1. Baseline circuit efficiency, achieved by running the machine with no fuel, and 2. Efficiency of the system with fuel, but with no fusion occurring, giving us an idea on whether losses change when fuel is present.


Our team has taken care to review each variable in great detail, providing us with a path to calculating energy created from fusion and measured back on the capacitors after each machine pulse.


Alongside plasma and neutron diagnostics, our electrical diagnostics offer a direct view into how our fusion machines perform. This system is essential to demonstrating that we can convert fusion energy into usable electricity and at what conversion efficiency, a core part of Helion’s mission to deploy commercial fusion power.
