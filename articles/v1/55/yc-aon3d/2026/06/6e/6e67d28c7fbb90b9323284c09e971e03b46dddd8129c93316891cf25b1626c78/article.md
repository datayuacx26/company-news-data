---
schema_version: "1.0.0"
document_id: "6e67d28c7fbb90b9323284c09e971e03b46dddd8129c93316891cf25b1626c78"
company_key: "yc-aon3d"
company: "AON3D"
source_id: "yc-aon3d-rss-fb9d9c5b4952"
canonical_url: "https://www.aon3d.com/applications/ultem-3d-printing-guide/"
published_at: "2026-06-29T15:20:38+00:00"
first_seen_at: "2026-07-26T22:38:07.846328+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:b279d6fb5caaaaac6c648b094a4738e218cc7b35653ae01183d2a4cbdcf459a8"
---

# ULTEM 3D Printing – Temperatures, Requirements and Pitfalls

\[PhD-Reviewed\]


Industrial 3D Printer Buyers Guide


Over 77% of printers marketed for high performance polymers can’t reliably print PEEK or ULTEM. Make your next investment with confidence, not consequences.


[Download Now](https://cta-service-cms2.hubspot.com/web-interactives/public/v1/track/click?encryptedPayload=AVxigLKtcCrWtpf5k1LijSMDjT4pe1612P7SaFHJMt%2B%2FC4fFYofS9OIgKnS7ylLNWhymXLObm6BUeowUWOefhyuQOIQGX5j7dG9hiDhDhcFebeCvloAdICQFjrvyZ3RMV1IlCiajRgIHzaVjlXEp9nOKRWrLYUErjEsMymR%2FzMkDTc5r5LmPZxdN3Z%2FgYyEayQe70jO0GyLRDC%2BlyQBKPV3a&portalId=4776083)


Polyetherimide (or PEI for short) is a high-performance amorphous thermoplastic in the polyimide class. Also known by its brand name “ULTEM”, it is known for its high strength, rigidity, thermal stability and excellent FST (flame, smoke and toxicity) characteristics.


Originally developed in the early 1980s by General Electric, the ULTEM brand of PEI is now produced by Sabic, while other non-branded versions are produced by third parties. ULTEM is used in a wide range of industries requiring high specific strength, chemical stability, and flame resistance.


While it’s certainly a useful material, printing ULTEM puts a strong emphasis on thermal management, as the right environment results in a well-behaved and dimensionally stable material delivering on the promise of[ULTEM’s excellent material properties](https://www.aon3d.com/material-science/ultem-material-properties-and-applications/) . Get it wrong and you’ll be fighting warping, weak layer adhesion and failed prints.


So let’s discuss the necessary conditions and the demands it puts on the printer hardware, to get the best out of ULTEM prints.


## Material Structure and Its Relevance to Printing


The molecular structure of a thermoplastic feedstock has a direct impact on how the plastic itself prints.


Thermoplastics such as PEEK are semi-crystalline polymers, meaning that the internal structure becomes crystalline (more ordered) in particular regions as it cools, post-extrusion. Conversely, **PEI is amorphous** , meaning that when the molten plastic cools and solidifies, the polymer chains remain random and disordered.


Both semi-crystalline and amorphous polymers come with their own sets of advantages and disadvantages, not only in the printing process itself, but in the final printed part.


For example, PEEK’s crystalline structure contributes significantly to its enhanced chemical, thermal, and wear resistance. But that same structure undergoes shrinkage as it crystallizes, which can result in residual stress, warping, and part distortion, if the cooling isn’t managed properly.


Because PEI is amorphous, it does not undergo such crystallization-related shrinkage, and so it can offer greater dimensional stability during printing than PEEK. However, because PEI softens gradually rather than transitioning through a distinct melting and crystallization cycle, it can be more sensitive to[thermal creep](https://www.sciencedirect.com/topics/engineering/thermal-creep) , sagging, and geometric softening during deposition if print temperatures and chamber conditions are not carefully controlled.


While annealing PEEK after printing can result in a higher degree of crystallization, PEI can also be annealed, although this is typically done for stress relief and improved dimensional stability rather than structural transformation. In both cases, it is **preferable to print at higher build chamber temperatures in the first instance** , rather than relying on post-process annealing to compensate for inadequate thermal control during printing.


ULTEM has multiple different variants, but there are two specifically that are used as 3D printing filaments –[ULTEM 9085](https://www.aon3d.com/materials/ultem-9085/) , and[ULTEM 1010](https://www.aon3d.com/materials/ultem-1010/) . Carbon fiber and glass filled versions of these grades are also available where extra stiffness and dimensional stability are needed, and they carry one extra hardware requirement that is covered further down.


## ULTEM Printing Temperatures


PEI is a high performance thermoplastic, with a rigid, aromatic backbone and high heat resistance. This is by design, and means that it requires a lot more thermal energy to make the polymer chains mobile enough to flow. In addition, due to PEI being an amorphous plastic, it does not have a discrete melting point like semi-crystalline plastics. Instead, it has a high glass transition temperature (Tg), over which softening occurs gradually.


Generic/raw PEI can have a Tg above 215°C, while additive manufacturing-specific formulations such as ULTEM 9085 are engineered for improved printability and may exhibit lower effective thermal transition behavior depending on grade and formulation.


In other words, if you want to extrude ULTEM filaments, you’re going to need a much hotter nozzle than what can be found on a typical consumer-grade 3D printer if you want to print it.


### Hot End Temperature


As mentioned, PEI needs high temperatures in order to flow correctly for 3D printing, and the ideal nozzle temperature range is in the region of 350–380°C.


### Chamber Temperature


While it’s relatively straightforward to install a hotter nozzle (even on consumer grade systems), achieving the chamber temperatures needed for printing ULTEM requires a dedicated 3D printer with high chamber temps right off the bat.


Typically, chamber temperatures around 160°C or higher are preferred for reliable high-quality ULTEM printing, though exact requirements vary by geometry, machine, and material grade.


### Bed Temperature


Uneven cooling at the base of a part can result in internal stresses and deformations. And because ULTEM prints at extremely high temperatures, it is important to keep the print bed at a similarly high temperature. Keeping the thermal gradient between the chamber temperature and the bed temperature low can help promote adhesion and reduce warpage from internal stresses. For these reasons, bed temperatures typically fall in the region of 180–200°C depending on system and build surface. PEI sheets and glue stick-type adhesives can help bond ULTEM to the build plate.


## What Your Printer Needs to Run ULTEM


AON’s previous generation M2 printing ULTEM


ULTEM is amorphous, so there is no crystallization threshold to clear. Thus, the hardware has a single job of bringing the whole part up to a temperature where it flows and bonds well, keeping it uniformly hot to avoid warping or sagging.


Below is a checklist for finding a machine that can reach as well as hold those temperatures throughout the printing process.


### An All-Metal Hot End


The nozzle has to hold the 350–380°C temperature range. That rules out any simple hot end with a PTFE liner as the upper limit for those is around 260°C. A fully metal melt path is the only option.


### An Actively Heated Chamber


The hot chamber has the job of flattening out temperature differences across the part. An amorphous part left to cool unevenly locks in stresses resulting in warping. ULTEM’s high printing temperatures make those differences steep unless the chamber is actively heated. Simple printers with only a warm bed settle at roughly 50–60°C, far short of the 160°C or so ULTEM expects. Closing that gap takes a dedicated heated chamber with proper insulation to maintain the temp as well as keep the fumes in.


### A High-Temperature Bed


ULTEM calls for a bed in the 180–200°C range, higher than most engineering plastics ask for, and the reasoning is the same uniformity argument. A cooler bed leaves a steep thermal gradient at the base of the part, which is precisely where warping takes hold. The plate build has to withstand the temperatures without losing its flatness.


### A Nozzle to Match the Grade


Unfilled ULTEM 9085 and 1010 are not abrasive, so an ordinary brass nozzle handles them without trouble. Carbon fiber and glass filled grades are a different matter, since the filler wears a brass nozzle out of tolerance quickly.


### A Way to Dry the Filament


ULTEM picks up moisture from the air, and as covered further down, a normal filament dryer will not get it dry. Treat drying capability as part of the printer setup, not a separate problem to solve later.


The practical takeaway from all of the above is this:


You can usually fit an all-metal hot end and a high-temperature sensor to a prosumer printer that lacks them. What you cannot retrofit is an actively heated chamber and the heat-tolerant motion system that has to come with it. That is the line. In practice, reliable ULTEM printing means a[printer engineered for ULTEM and other high-temperature materials](https://www.aon3d.com/3d-printers/hylo/) , not a modified consumer or prosumer machine.


## Common Issues with ULTEM Printing Process


The main culprit behind the issues is a sub-par machine, as all of the above emphasizes the importance of maintaining the correct temperatues. However, some issues may also stem from a poor process, or complex part geometry.


### Warping


Left: unheated chamber; right: heated chamber


While PEI is free from crystallization shrinkage, it does still shrink due to thermal contraction as it cools. If this contraction occurs unevenly throughout the part, or if sections of the part are mechanically constrained during cooling, thermal stresses can accumulate and manifest as warping or distortion.


In order to minimize this warping, printing should take place inside a properly heated chamber, allowing the part to cool more uniformly and reducing the thermal gradients that cause residual stress.


### Poor Bond Strength


Because PEI softens gradually as it approaches its glass transition temperature, effective interlayer bonding depends on maintaining sufficient thermal energy for polymer chain mobility at the layer interface.


In other words, the extrusion temperature has to be high, but the previously deposited layer must also remain sufficiently warm to allow polymer chains across both layers to diffuse and entangle properly.


Higher nozzle temperatures improve melt flow and polymer diffusion, while elevated chamber temperatures help prevent previously deposited layers from cooling too far below their bonding-effective temperature range, together improving interlayer adhesion.


### Layer Strength and Part Orientation


FDM parts are weaker across layers than along them, and ULTEM is no exception. A printed ULTEM part can be somewhat weaker in the Z direction (that really depends on the printer too, as 80% Z-axis strength is doable) than in the plane of the layers, so orient the part on the build plate to have the main loads run along the layers rather than across them.


This is also why part cooling fans are normally left off when printing ULTEM. Active cooling helps bridging and overhangs on lower-temperature plastics, but with ULTEM it chills the previous layer below its bonding-effective temperature and weakens the bond. Let the heated chamber, not a fan, manage cooling.


### Moisture Absorption


PEI is generally moderately hygroscopic. For context, while PEEK can absorb 0.1% moisture by mass over 24 hours, PEI/ULTEM can absorb more than double, ranging between 0.2…0.3% over the same period.


So while it is not as hygroscopic as nylon (~0.5–1.5%), care should still be taken to keep ULTEM filament dry, as the high temperatures involved in the printing process can result in trapped water flashing into steam in the nozzle. It can be dried before use, if any popping or other moisture-related issues are observed.


Drying ULTEM properly is not a quick job. Effective drying needs sustained heat well above what a typical filament dryer reaches, in the region of 120–150°C for several hours, so an oven or an industrial dryer is usually needed. Also repeated or improperly controlled drying cycles can gradually contribute to material degradation, so keeping it dry can be preferable to drying it.


## Key Material Properties Relevant to Printing


For printing, three properties matter the most.


Its glass transition temperature, above 215°C for raw PEI and somewhat lower for printing grades like ULTEM 9085, sets the nozzle, chamber and bed temperatures discussed above.


Its moisture absorption, in the 0.2 to 0.3% range, is what drives the drying requirement.


And its thermal contraction on cooling, even without crystallization shrinkage, is what makes a heated chamber non-negotiable for warp-free parts.


Everything else, tensile strength, modulus, chemical and flame performance, matters for deciding whether to use ULTEM for engineering purposes, but is not that relevant to ULTEM printing.


## ULTEM Applications


ULTEM is chosen when a part has to hold up to heat, chemicals or fire where a standard engineering plastic would fail. Its FST performance in particular opens doors that few other printable polymers can. That puts printed ULTEM into four main sectors.


Sector Why ULTEM Typical printed parts


Aerospace and aviation FST performance, with ULTEM 9085 and 1010 both compliant with FAR 25.853 Aircraft seats, cabin HVAC ducts, electrical connectors, interior panels


Space High specific strength and low outgassing CubeSat structures, satellite internal components, antenna parts


Medical ULTEM 1010’s chemical and temperature resistance, holds up to repeated sterilization Sterilizable instruments and tooling


Automotive Heat and chemical resistance in under-the-hood service Fluid system components, sensor housings, electrical connectors


ULTEM is the high-performance plastic reached for when a part has to survive an environment that would defeat any commodity polymer, without yet needing the[extreme chemical resistance PEEK offers](https://www.aon3d.com/material-science/peek-material-properties-and-applications/) .


## When to Choose PEI over Other Plastics


When compared to consumer-grade filaments, it is easy to see why PEI would be preferred for high performance applications.


PEI generally offers superior thermal and environmental performance, along with competitive specific strength, but it really comes into its own in terms of thermal performance, chemical resistance, and FST characteristics.


But while comparing PEI to regular plastics shows obvious gains, comparing PEI variants to PEEK can be less obvious.


While they are fundamentally different at the molecular level due to PEI being amorphous and PEEK being semi-crystalline, when you look at the mechanical and physical performance of the printed part, both PEI and PEEK fare quite similarly.


So why pick one over the other?


Here is a side by side comparison regarding key parameters.


ULTEM 9085 ULTEM 1010 PEEK


Density 1.34 g/cm³ 1.27 g/cm³ 1.29 g/cm³


Tensile Strength (yield) 69.7 MPa 81 MPa 85.0 MPa


Elastic Modulus 2322.5 MPa 2760 MPa 3120 MPa


HDT 153°C 216°C 240°C


Chemical Resistance Good resistance to many common solvents, though more vulnerable to strong solvents, chlorinated chemicals, and aggressive alkaline environments Good resistance to many common solvents, with improved thermal and chemical durability over 9085, though still less chemically resistant than PEEK in harsh solvent environments Excellent chemical resistance, including strong resistance to hydrocarbons, solvents, and harsh industrial chemicals


FST Rating FAR 25.853 compliant (aerospace interior standard), OSU 65/65, UL94 V-0, low smoke / low toxicity, specifically engineered for aerospace FST applications UL94 V-0, strong FST performance and commonly cited for transport / industrial applications, but less specifically aerospace-interior focused than 9085 UL94 V-0 (grade dependent), inherently flame retardant with low smoke and toxicity, but FST compliance is highly formulation- and certification-dependent


Cost (per kg) Lower ($250/kg to $500+/kg depending on form/supplier) ~$300–$500/kg Higher (~$300 to $800+/kg depending on grade/supplier)


As the table shows, the cost of each plastic generally increases alongside its performance envelope, with ULTEM 9085 at the lower end and PEEK typically occupying the premium tier.


Due to its compliance in various aviation-related standards, ULTEM 9085 offers an excellent opportunity for aircraft interior manufacturers, who don’t wish to pay a premium for the enhanced chemical resistance of PEEK.


For many applications, processing difficulty and certification requirements may influence material choice as much as raw mechanical performance.


## It Comes Down to Thermal Control


PEI/ULTEM sits alongside high performance plastics such as PEEK, PEKK and PAEK, at the top of the polymers pyramid. The high performance polymers tower high above other plastics such as regular engineering plastics (PA, PC), and commodity polymers (PE, ABS, PLA) in terms of overall material performance. But naturally, this levelling up to the highest tier of performance plastics comes at an increased financial cost, in terms of material and machine cost.


ULTEM and PEI can be thought of as excellent general plastics for high performance applications. PEEK is better suited for edge cases that even PEI can’t reach (such as extreme chemical resistance or higher thermal stability).


Whatever the grade you land on, the rule is the same. ULTEM is not difficult to print because the plastic is temperamental, it is difficult because it demands a machine that can reach and hold extreme temperatures at the nozzle, chamber and bed all at once. Get the thermal environment right, and the rest of the process falls into place.
