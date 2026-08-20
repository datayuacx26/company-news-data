---
schema_version: "1.0.0"
document_id: "63cfdbc9342e8e97cdd31e1fbec573d4edd9549bc14d98a59c53ad9b90385f0b"
company_key: "yc-aon3d"
company: "AON3D"
source_id: "yc-aon3d-rss-fb9d9c5b4952"
canonical_url: "https://www.aon3d.com/applications/pekk-3d-printing-guide/"
published_at: "2026-06-29T14:06:34+00:00"
first_seen_at: "2026-07-26T22:38:07.846328+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:eeab672fbcd2fdcacb90bfbba8021b0ce2a226723ad3ffa693631e34303f0ae9"
---

# PEKK 3D Printing – Temperature, Requirements and Pitfalls

\[PhD-Reviewed\]


Industrial 3D Printer Buyers Guide


Over 77% of printers marketed for high performance polymers can’t reliably print PEEK or ULTEM. Make your next investment with confidence, not consequences.


[Download Now](https://cta-service-cms2.hubspot.com/web-interactives/public/v1/track/click?encryptedPayload=AVxigLKtcCrWtpf5k1LijSMDjT4pe1612P7SaFHJMt%2B%2FC4fFYofS9OIgKnS7ylLNWhymXLObm6BUeowUWOefhyuQOIQGX5j7dG9hiDhDhcFebeCvloAdICQFjrvyZ3RMV1IlCiajRgIHzaVjlXEp9nOKRWrLYUErjEsMymR%2FzMkDTc5r5LmPZxdN3Z%2FgYyEayQe70jO0GyLRDC%2BlyQBKPV3a&portalId=4776083)


PEKK (polyetherketoneketone) is a high-performance thermoplastic polymer belonging to the[PAEK (polyaryletherketone) family](https://en.wikipedia.org/wiki/Polyaryletherketone) . Like other polymers in that family (such as PEEK or PEK), PEKK is valued for its chemical resistance, heat resistance, flame resistance, low outgassing, and high strength.


What sets PEKK apart from other PAEKs however, is the tunability of the polymer to produce either amorphous or semi-crystalline plastics, which can in turn be used in additive manufacturing.


This tunability means that manufacturers can produce[PEKK feedstocks in either amorphous, slow-crystallizing, or semi-crystalline forms](https://www.aon3d.com/material-science/pekk-material-properties-and-applications/) .


Amorphous PEKK 3D printer filaments can be identified by the moniker “PEKK-A”, and benefit from easier processing and better dimensional stability.


Semi-crystalline PEKK feedstocks are sometimes referred to as “PEKK-SC”, and while they are harder to print, they benefit from improved mechanical properties, higher thermal resistance and superior chemical resistance compared to PEKK-A.


Resin manufacturer Arkema divides their PEKK feedstocks into their Kepstan 6000 series (amorphous) and Kepstan 7000 series (semicrystalline) products.


## Material Structure and Its Relevance to Printing


The backbone of the general PAEK polymer consists of aromatic aryl rings, linked by ether and/or ketone groups. This structure is what gives PAEK materials their high strength, thermal stability, chemical resistance, flame resistance, and dimensional stability at elevated temperatures.


The ratio and arrangement of ether and ketone groups influence the crystallinity in the polymer, and the number and sequence of ether and ketone linkages in the repeat unit determines whether the polymer is PEK, PEEK, PEKK, or another PAEK


PEKK specifically has one ether and two ketone linkages in its repeat unit.


Another feature of PEKK is that its crystallization behavior can be tuned by adjusting its terephthaloyl/isophthaloyl ratio (or T/I ratio, for short).


Biasing the ratio in favor of terephthaloyl typically results in a straighter molecular chain, easier meaning better crystal packing, faster crystallization, and more semi-crystalline behaviour.


On the other hand, having a more isophthaloyl-heavy ratio produces a kinked backbone, resulting in poorer crystal packing, and consequently, slower crystallization. In this case, the polymer behaves in a more amorphous fashion.


The tunability of PEKK’s crystallization means that there have also been powdered variants developed for[SLS 3D printing](https://www.hubs.com/knowledge-base/what-is-sls-3d-printing/) . By tuning the polymer to have a slower crystallization rate and optimal melting behavior, the material is given a wider processing window for the SLS process. By controlling the thermal environment, crystallization-related shrinkage, and other warping can be reduced, while retaining the favorable mechanical characteristics of PEKK.


## PEKK Printing Temperatures


The table below shows values for a typical grade of PEKK-A.


**Description** **Value**


Printer nozzle temperature 320 – 360°C


Heated bed temperature 120 – 160°C


Chamber temperature 135°C


Print Speed 20 – 40 mm/s


Bed Material PEI or PPSU sheet; glass with high-temperature adhesive


Bed Adhesion High-temperature build adhesive / glue stick when printing on glass


Pre drying recommendations If exposed to moisture, dry at 110 – 120°C for 4 – 6 hours


## Printer Features


PEKK-A filament


How do those temperatures and other print parameter requirements translate into hardware needs?


For a start, we can see that the nozzle temperature requirement of this particular PEKK-A (320 – 360°C) is much lower than[PEEK printing](https://www.aon3d.com/applications/peek-3d-printing-guide/) needs (365-440°C). Because it is an unfilled plastic, there is no need for an abrasion-resistant nozzle, and a standard high-temperature all-metal nozzle can be used.


There are filled varieties of filament such as PEKK-CF, and abrasion-resistant nozzles such as hardened steel or ruby should be used when printing with such materials.


As for the heated chamber, it is recommended to use a temperature of around 135°C for PEKK-A. This is lower than the chamber temperatures typically required for high-temperature polymers such as PEI/ULTEM, but it is sufficient for PEKK-A because the material is amorphous and does not need to undergo controlled crystallization during printing.


Semi-crystalline PEKK variants might benefit from increased chamber and hot end temperatures.


Overall, for amorphous PEKK, the heated chamber mainly helps reduce thermal gradients, improve interlayer bonding, and limit residual stress, warping, and dimensional distortion during the build.


For semi-crystalline PEKK variants, the heated chamber also helps manage crystallization during printing, although additional post-processing or annealing may be required to achieve the desired level of crystallinity.


In terms of the print bed, it should be kept at 120 – 160°C, and the surface should be glass, PEI or PPSU. Bed adhesives can certainly help reduce build failures, particularly if using a glass bed surface.


Finally, in terms of drying, PEKK is not highly hygroscopic.


Amorphous PEKK-A shows low water absorption of around 0.20% after 24 hours at 23°C (ISO 62), while semi-crystalline PEKK-SC-type resin shows even lower water absorption of around 0.11% under the same conditions.


However, like PEEK and other high performance plastics, PEKK filament should still be kept dry before printing, as even small amounts of moisture trapped in the filament can result in adverse effects. Use a dry cabinet or other protection to keep it safe from moisture, and if you do need to dry it, use a thermal vacuum chamber / desiccant oven for optimum results.


## Common Issues with PEKK Printing


Despite being an “easier” PAEK material to print with due to its low melting point /[glass transition temperature](https://www.specialchem.com/plastics/guide/glass-transition-temperature) , the same issues can arise as when printing with other high performance/high temperature plastics.


As mentioned previously, moisture can cause issues with printing. While PEKK-A has low hygroscopic behavior, absorbing only 0.20% of water after 24 hours at 23°C (by ISO 62), the nozzle temperature is still hot enough to cause trapped moisture to flash into steam and bubble.


This can not only result in a poor surface appearance but can affect bond strength and other mechanical characteristics, so it’s better to keep it dry, and force dry it only when absolutely necessary.


As with most other plastics, uneven thermal gradients through the part can result in warping and distortion, so proper temperature guidelines should be adhered to, especially regarding chamber and bed temperatures.


Additionally, if the bed temperature is not hot enough, poor bed adhesion can result on the first layer, which can result in print failure if the print becomes detached from the build plate.


In order to avoid these issues, it is recommended to follow the manufacturers’ recommendations for printer settings.


## PEKK Applications


**Sector** **Why PEKK?** **Typical printed parts**


Aerospace and aviation High strength-to-weight ratio, chemical resistance, heat resistance, low outgassing. Some varieties are FAR 25.853 compliant. Aircraft wing hydraulic-component bracket.


Space and rocketry Low outgassing, high thermal performance, chemical resistance, and availability of ESD-capable PEKK-based grades[Orion spacecraft](https://media.visionminer.com/nasas-orion-spacecraft-now-flies-nearly-200-3d-printed-parts/) docking hatch door / hatch-cover components.


Rail Flame, smoke, and toxicity suitability is the key driver. Some variants are certified to EN 45545-2 across HL1/2/3. Railway battery cover printed from PEKK-A


Electrical and electronics ESD-capable PEKK grades, heat resistance, chemical resistance, and dimensional stability during high-temperature processing Fixture for mounting and securing a ceramic capacitor and temperature sensor during a high-temperature structural epoxy cure cycle.


Industrial, chemical, and fluid handling Chemical resistance, heat resistance, low material waste versus machining, and ability to print complex internal geometry Fluid mixer printed from Kepstan PEKK / PEKK-A. Semi-crystalline PEKK seals printed using Kepstan PEKK.


Medical and dental research Biocompatibility potential, patient-specific geometry, smoother surfaces than PEEK in some dental mesh research, and good mechanical performance Patient-specific cranial implant research using 3D-printed PEKK. 3D-printed PEKK thin meshes studied for customized alveolar bone augmentation.


## **When to Choose PEKK Over Other Plastics?**


In short, PEKK offers comparable performance to other PAEK plastics, but with the added advantage of being easier to process due to its lower extrusion temperatures. The lower temperature at the hot end means that lower temperatures can be used in the chamber and build plate as well.


So PEKK is accessible to a wider user-base, by virtue of the lower thermal needs. However, these needs still require a[high-temp printer](https://www.aon3d.com/3d-printers/hylo/) that can raise to the necessary temperature levels and hold them uniformly.


Additionally, PEKK is available in both amorphous and semi-crystalline varieties. So if a user prefers printability and dimensional stability, but doesn’t mind taking a slight penalty of chemical resistance, then the amorphous PEKKs are a great solution.


And conversely, if a user wants the higher mechanical strength, plus enhanced thermal and chemical resistance, but doesn’t mind slightly diminished dimensional stability, then the semicrystalline versions are also available for those with more extreme requirements.


Fnally, many brands of PEKK filament come with various industry-specific qualifications and certifications, such as UL94 for FST ratings, FAR 25.853 (aerospace), and EN45545 (rail). So if you are looking to design components for any of those industries, knowing that a certain polymer already passes such requirements is a big advantage.
