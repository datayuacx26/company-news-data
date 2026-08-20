---
schema_version: "1.0.0"
document_id: "3f043c7315d0d294a8a56931454fd2b66c8a5c20328c9f1cd63e47ef9259c5a5"
company_key: "yc-helion-energy"
company: "Helion Energy"
source_id: "yc-helion-energy-news-import-5ae3e56c82e0"
canonical_url: "https://www.helionenergy.com/blog/more-on-helions-pulsed-approach-to-fusion"
published_at: "2025-02-10T00:00:00+00:00"
first_seen_at: "2026-07-21T22:45:07.501995+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:e4c65491c64165449911d0a626af9e1a73b8e93040bfb4d5d926bd88bd764aaa"
---

# More on Helion’s pulsed approach to fusion

A key differentiator for Helion’s approach to fusion is that we do our fusion reactions in a stream of pulses where the fuel is heated and expanded repetitively to generate electricity. This can efficiently and directly generate electricity from short bursts of fusion that add up to significant, commercial levels when operated at a high repetition rate. In contrast, steady-state machines aim to create ‘ignited’ plasmas in which fusion reactions are sustained by self-heating. In such a machine, energy is extracted from fusion neutrons through a steam cycle.


The pulsed approach to fusion also helps us overcome some of the[instabilities inherent to field reversed configuration (FRC) plasmas](https://www.helionenergy.com/articles/a-note-on-frc-instabilities/) and engineering challenges of steady-state machines. It also helps our overall system in other, perhaps unexpected, ways.


### Direct electricity recovery


FRCs are intrinsically high-beta—the ratio of plasma pressure to magnetic pressure is close to 1. Compared to a low-beta plasma found in steady-state machines, the same magnetic field can confine an FRC plasma with higher pressure or energy density.[1](https://pubs.aip.org/aip/pop/article-abstract/18/7/070501/262397/Review-of-field-reversed-configurations?redirectedFrom=fulltext) We can therefore efficiently use magnetic fields to compress our FRCs to fusion conditions. The energetic charged particles produced by fusion cause the plasma to expand and push back on the external magnetic field. That increases the current in the coils of the machine’s magnets and thereby converts fusion energy into electricity.


For this inductive energy recovery to work, we need a cycle of plasma compression, fusion, and plasma expansion. Compressing the FRC only requires increasing the magnetic field, and recovering the compressional energy is a matter of relaxing the field and expanding the plasma.[The cycle is akin to a heat engine](https://www.youtube.com/watch?v=5nHmqk1cI2E&t=4s) in which fuel is injected and burned to create a hot expanding gas that pushes on a piston. However, in place of a chemical fuel we have the FRC plasma, in place of chemical reactions we have fusion, and in place of a piston we have magnetic fields. Thus, our magnetic-compression approach is naturally pulsed and cyclic.


### Circuit efficiency


Steady-state fusion tries to sustain plasma ignition for a long time—in principle the confinement should be longer than the confinement lifetime, ideally infinite. It is something of a chicken and egg problem: you need long confinement lifetimes and large energy yields to make up for the enormous energy investment needed for those long lifetimes. To get those large energy yields, a steady-state system must reach the ignition condition: the byproducts of fusion can further heat the plasma and increase fusion reactions even more. But that further pushes the system to new physics regimes.


Our pulsed approach bypasses that issue. Having a circuit that recovers electricity means we recover much of the invested magnetic energy around the FRC, as well as the energy invested to heat the FRC. Additionally, we capture any newly produced fusion energy. Although highly sophisticated, our system is basically an RLC circuit with a resistor (R), inductor (L), and capacitor (C). As much as 90% of the energy in the system ends up in the magnetic fields. Physics dictates the energy returns to the capacitors, though with some resistive losses. Additionally, the energy that went into compressing the FRC can return with a high thermodynamic efficiency, because the plasma itself is extremely hot.[2,](https://iopscience.iop.org/article/10.1088/0029-5515/13/4/006)[3](https://www.sciencedirect.com/book/9780126852011/fusion#book-info) With proper design and fast electrical switches, we can recover most of the initial charge on the capacitor bank. This would be true with or without fusion. Thus, the captured fusion energy need only be greater than the system losses during each pulse. And that means we don't need huge yields to get on the positive side of the energy in–energy out balance sheet.


We do not aim to build a giant device that generates gigawatts of fusion power. Instead, we will build much smaller machines that generate tens of megawatts. A facility can reliably and economically operate commercially with one or more such machines.


### Lower risk


A system that generates gigawatts of power comes with great risk. Large energy releases, such as disruptions, could destroy a tokamak or other magnetic fusion machine. A disruption is when plasma instabilities lead to a sudden release of the thermal and magnetic energy in the plasma. Not so for our pulsed FRCs—each of our FRCs don’t contain enormous amounts of energy. Even if there is an instability that develops in our FRC, we will be more resilient. Trenta showed that the energy from such disruptions can be safely routed to the divertors without damaging the machine. That isn’t to say plasma instabilities are inconsequential for us. However, they are at a level that we can guard against with good engineering and design. Also, because we don’t need high yields, we can design and operate a system that doesn’t require the most demanding physics in fusion: ignition.


These three aspects of our pulsed approach form a virtuous circle. Pulsed FRCs naturally lend themselves to inductive energy recovery. Inductive energy recovery enables efficient commercial scale operations even at low fusion yields. Low-yield operation minimizes the risks of disruptions or machine damage. The lowered risk makes possible the relatively simple pulsed fusion system that we utilize.


Rather than attempt to solve ever more complex and difficult physics problems, our pulsed approach to fusion strives for not just the simplest engineering solutions possible, but actually less challenging physics. That will be the path to abundant, sustainable, carbon-free energy.
