---
schema_version: "1.0.0"
document_id: "f0887781a12e12021bb78da5b3acaa538d69ceac816873039c894253978ccac4"
company_key: "hubbell-inc-common-stock"
company: "Hubbell Inc"
source_id: "hubbell-inc-common-stock-news-import-21f20cca1488"
canonical_url: "https://blog.hubbell.com/en/chancefoundationsolutions/how-to-specify-helical-piles-a-practical-guide-for-engineers"
published_at: "2026-06-30T11:45:00+00:00"
first_seen_at: "2026-07-21T23:15:25.727559+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:afb2b2f09ff8081582daf1cba84d1a1c9fa52198c2d72c3ea4e913214c280d02"
---

# How to Specify Helical Piles: A Practical Guide for Engineers

Helical piles and anchors are widely used in foundation construction due to their efficiency, versatility, and performance. However, specifying them correctly is critical to ensuring both structural reliability and cost-effectiveness.


When specifying helical piles, designers generally choose between two main approaches:


1. Minimum capacity (ultimate resistance)


or


2. Installation torque requirements.


#
Minimum Capacity Specifications (Ultimate Resistance)


**Ultimate resistance** is the maximum load a helical pile can support before failure. Engineers apply a factor of safety or resistance factors to the ultimate resistance to ensure performance under normal service conditions.


Best practice is to design the ultimate resistance to be limited by the geotechnical capacity, not the structural strength.


There are two methodologies used to evaluate pile performance:


-


-


Allowable Strength Design (ASD):


Evaluates performance at service level (unfactored) loads, meaning the required strength is less than or equal to the allowable strength. For ASD design, specifications should clearly define whether loads are ultimate or service loads and identify the associated safety factors. ASD answers the question, *“After applying a safety factor, how much load can this component safely carry?"*


-


**


LRFD (Load and Resistance Factor Design):


Evaluates performance at strength level (factored) loads, meaning the required strength is less than or equal to the design strength. LFRD takes into account that different loads and different levels of uncertainty (a fairly certain dead load vs. an unpredictable wind load). For LRFD design (i.e., per


[ASCE/SEI 7-22 Minimum Design Loads and Associated Criteria for Buildings and Other Structures](https://www.asce.org/publications-and-news/codes-and-standards/asce-sei-7-22) ,


specifications should clearly define the relevant loads and load factors.


Typical standards include a factor of safety of 2.0 for permanent applications using the ASD methodology. Tieback anchors tested with proof loads may use a lower factor of safety of 1.5 by the ASD standard. LRFD resistance factors range from 0.65–0.75 in compression and 0.55–0.65 in tension.


Installation Torque Specifications


Torque can be used as an indirect measure of capacity. The specified torque should be based on the required minimum capacity (ultimate resistance). For tested helical piles and tieback anchors, proof testing governs acceptance. Pre-production testing is also used to confirm site specific torque correlation (Kt) factors. It is very important to use the appropriate torque correlation factor when specifying installation torque. The pile manufacturer, shaft type, shaft size, and overall pile length need to be considered.


The relationship between installation torque and ultimate capacity is expressed using a simple formula. A torque factor, Kt, is used as a multiplier and is based on the type and size of the helical pile shaft.


The torque factor is inversely related to shaft size—the larger the shaft, the smaller the Kt value. Chance square shaft helical piles, have the largest torque factor, meaning they can support the most axial capacity with the least amount of torque.


Qult = Kt x T


Where:


Qult = Ultimate Capacity \[lb (kN)\]


Kt = Empirical Torque Factor \[ft-1 (m-1)\]


T = Installation Torque, \[ft-lb (kN-m)\]


Accurate torque correlation (Kt factor) requires proper installation conditions, including sufficient advancement rate and appropriate Kt values. It is strongly recommended to only specify helical piles manufactured by suppliers who have published pre-qualified torque correlation factors.


To obtain the Chance torque correlation factor by product, download the


[Chance Technical Design Manual](https://info.hubbell.com/en/chancefoundationsolutions/technical-design-manual) .


Capacity is often based on the average torque over the final 3 feet of installation, where soil interaction at the bearing depth is most critical. This is true for both compression and uplift resistance.


# Design Considerations


Penetration into dense soil may require higher-strength helical pile shafts due to increasing torque during installation. Using HeliCAP helical capacity design software, the capacity of a pile can be configured for a user-input soil profile. The software, free to use on any web-connected device, will provide capacity data of selected piles.


[Create an account to start using HeliCAP](http://www.hpsapps.com/helicap) .


Figure: Helical pile specification decision flowchart


START


: DEFINE THE DESIGN BASIS


Loads • soil profile • pile type • shaft size • corrosion requirements • ASD or LRFD


**↓**


PATH 1


Minimum Capacity


Specify required ultimate resistance or allowable capacity.


PATH 2


Installation Torque


Convert required capacity to target torque using the correct Kt factor.


PATH 3


Combined Criteria


Use both capacity and torque requirements, supported by testing.


**↓**


**↓**


**↓**


**Confirm load basis**
Service, ultimate, or factored loads.


**Confirm torque basis**
Shaft type, Kt value, pile length, and final 3 feet average torque.


**Confirm testing plan**
Proof, pre-production, or verification testing.


**↓**


FINAL SPECIFICATION SHOULD DEFINE


Load basis • safety/resistance factors • torque correlation factor • acceptance criteria • testing requirements • documentation for approval


# Conclusion


Proper specification of helical piles requires clear communication, appropriate safety factors, and correct use of torque correlations to ensure safe and economical designs. Whether the specification is based on minimum capacity, installation torque, or a combination of both, it should define the load basis, safety or resistance factors, torque correlation assumptions, testing requirements, and final acceptance criteria.


If you need help with a helical pile design,[reach out to your local distributor](https://www.hubbell.com/chancefoundationsolutions/en/sales-contact-lookup)


for expert engineering assistance.
