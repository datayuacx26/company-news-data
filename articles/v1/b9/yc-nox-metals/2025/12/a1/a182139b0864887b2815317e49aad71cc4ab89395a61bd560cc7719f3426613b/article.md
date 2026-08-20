---
schema_version: "1.0.0"
document_id: "a182139b0864887b2815317e49aad71cc4ab89395a61bd560cc7719f3426613b"
company_key: "yc-nox-metals"
company: "Nox Metals"
source_id: "yc-nox-metals-news-import-4d0abe59faf1"
canonical_url: "https://noxmetals.co/blog/aluminum-tempers-guide"
published_at: "2025-12-01T00:00:00+00:00"
first_seen_at: "2026-07-24T06:33:38.984322+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:4d292bed85c65fe437f9ae3c2c105dae327cf5dda4a3aa86dbba9e0c791eeb43"
---

# T651, T6, and T7351: A Practical Guide to Aluminum Temper Designations

## The ANSI Temper Designation System


Aluminum temper designations in the United States follow ANSI H35.1, a standardized system that describes the sequence of mechanical and thermal processing applied to the material after casting. The designation consists of a letter (F, O, H, or T) followed by one or more digits that further specify the processing. Understanding the letter prefix immediately tells you the class of treatment: F means as-fabricated, O means annealed, H means work hardened, and T means thermally treated.


## F - As-Fabricated


The F temper designates material in the condition as it came from the mill process - no special thermal treatment or work hardening has been applied after the primary forming operation. Properties in F temper vary and are not controlled to specific minimums in most specifications. F temper plate is rarely specified for structural or precision machined applications. It may appear as a transient state during processing before final heat treatment.


## O - Annealed


The O temper designates fully annealed material - the softest and most ductile condition achievable for a given alloy. For heat-treatable alloys like 6061 and 7075, O temper means the material has been solution heat treated and then slowly cooled to relieve all strengthening from prior treatment. O temper is used when maximum formability is required (bending, hydroforming) and strength is not the primary requirement. It is not used for structural machined parts.


## H Series - Work Hardened (5000 Series)


The H series applies to non-heat-treatable alloys like the 5000 series (5052, 5083, 5086). These alloys cannot be strengthened by heat treatment - they are strengthened by cold working (rolling). H32 means strain hardened and then stabilized to one-quarter hard. H116 and H321 are specific tempers for marine-grade 5083 that meet both mechanical and corrosion resistance requirements. For heat-treatable alloys like 6061 and 7075, the H tempers do not apply.


## T4 and T451 - Solution Heat Treated, Naturally Aged


T4 temper means the alloy was solution heat treated (heated to dissolve alloying elements into solid solution, then rapidly quenched) and then allowed to naturally age at room temperature to a stable condition. T451 adds stress relief by stretching after quench. Natural aging develops partial strength and good ductility - properties are typically lower than the peak-aged T6 condition. T4 is sometimes used when further forming is required after delivery, since the material is softer and more workable.


## T6 and T651 - Peak Aged


T6 is the most common temper for 6061 and 7075 in bar, extrusion, and thin plate form. It consists of solution heat treatment followed by artificial aging at elevated temperature to maximum strength (peak age). T651 is T6 with an added stress relief step: after quenching, the plate is stretched 1 to 3 percent to relieve residual quench stresses before aging. The 51 suffix is critical for thick plate used in machined applications - without stress relief, the residual stresses locked into the plate will be released during machining, causing warping and dimensional drift.


Note


Always specify T651 (not T6) for aluminum plate that will be heavily machined. T6 plate may have significant residual stress that is released as material is removed, causing the part to move off dimension during or after machining. The 51 designation confirms stress relief stretching was performed.


## T7 Series - Overaged


T7 temper designates material that has been aged beyond peak strength (overaged). The additional aging time and temperature reduce tensile strength by 5 to 15 percent compared to T6, but substantially improve stress corrosion cracking resistance and fracture toughness. T7 tempers are the standard for thick-section 7075 and 7050 in aerospace applications where SCC is a design concern. The T7 temper is not used for 6061 because 6061 already has good SCC resistance in T651.


## T7351 vs T7451 - Stress Relief in T7


T7351 and T7451 are both overaged tempers with stress relief, but the stress relief method differs. T7351 uses compression (for forgings and thick extrusions), while T7451 uses stretching (for rolled plate). For aluminum plate, T7451 is the correct designation - it confirms the plate was stress relieved by controlled stretching, which removes quench residual stresses and ensures dimensional stability during machining. 7050-T7451 per AMS 4050 is the standard specification for thick aerospace structural plate.


## Temper Reference Table


Temper Treatment Description Typical Application


F As-fabricated, no additional treatment Transient state only


O Fully annealed, maximum ductility Sheet forming, bending


H32 Strain hardened + stabilized (1/4 hard) 5000 series plate, marine


H116 Strain hardened, corrosion resistant 5083/5086 marine plate


H321 Strain hardened + stabilized for marine 5083 marine, pressure vessels


T4 Solution heat treated + natural age Pre-form condition, limited structural


T451 T4 + stress relief stretch T4 with lower residual stress


T6 Solution heat treated + artificial age (peak) Extrusions, bars, thin plate


T651 T6 + stress relief stretch Machined plate (6061, 7075 <3")


T73 Solution HT + artificial overage 7075 with SCC resistance (old spec)


T7351 T73 + stress relief compression Thick forgings and extrusions


T7451 T74 + stress relief stretch Thick plate (7050-T7451, 7075-T7351)
