---
schema_version: "1.0.0"
document_id: "cf45fdca456b6d020a8d1f2fc40fcd27ad16886d6f6e0ef763a599195aa37058"
company_key: "yc-vista-power"
company: "Vista Power"
source_id: "yc-vista-power-news-import-2590aab523a6"
canonical_url: "https://www.vistapower.com/blog/california-title-24-solar-battery-rules-explained"
published_at: "2023-01-01T00:00:00+00:00"
first_seen_at: "2026-07-22T19:03:41.963784+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:dcc1aa189250835770673a399fca3a56202feeea98a66912b30695fff23992a8"
---

# California Title 24: Solar + Battery Rules Explained

### Title 24 - What Is It & Why Does It Matter?


California’s Energy Code— **Title 24, Part 6** —quietly rewrote the playbook for all new real estate projects - both commercial and residential - on **January 1, 2023** . What began as a “solar‑ready” roof rule now demands fully installed **solar‑photovoltaic (PV) + battery storage (BESS)** systems on most non‑residential permits. Miss the requirement and local building officials will not sign your Certificate of Occupancy. The next code cycle, already drafted for **January 1, 2026** , raises the bar again. For ground breaks in 2024‑25, you have one design cycle to get this right—or pay twice in redesigns and change orders.


‍


### Who's Covered & Who Isn't


Title 24’s energy rules are tied to the building’s “occupancy group,” a fire‑code term, rather than the NAICS code. Linking energy requirements to that same label lets cities evaluate compliance in seconds—but it also means owners sometimes mis‑classify a space and get surprised at permit time. If your asset falls into **Group B, F, M, A‑3, H‑2, H‑3, or high‑rise R‑1/R‑2** , you are in scope for Title 24.


That means you must install PV + BESS If you are involved in a project that is an:


- Office
- Retail
- Grocery
- Warehouse
- School
- Hotel
- Multifamily with ≥4 habitable floors


You must install PV only, but BESS is optional for:


- Low-rise multifamily (<4 habitable floors)


And finally, you are not required to install either if the project is a:


- Parking garage
- Unconditioned shed


Apartments below four stories fall under a different, residential‑leaning section of the code (Part 11, CALGreen). Once the building is four stories or higher, it flips to high‑rise multifamily and inherits the full commercial PV + BESS mandate. Many mixed‑use podium projects cross this threshold. Additionally, the state sees warehouses and logistics centers as low‑hanging fruit: giant, flat roofs and big evening peaks from forklifts and refrigeration. Expect plan checkers to enforce the CFA method (the largest PV size - details below!) unless you prove roof shading or structural limits. Lastly, while parking structures and structures like telecom shelters can skip PV and batteries, but the surrounding retail or office space cannot piggy‑back on that exemption. Be ready for plan reviewers to ask for separate electrical meters and single‑line diagrams when zones mix.


**Key Considerations:**


- Confirm occupancy groups **before** schematic code checks.
- If you want to re‑classify (e.g., flex industrial vs. warehouse), lock that in with the fire marshal early.
- In mixed‑use, size PV/BESS for each group individually, then sum.
- Keep an email trail of city guidance—lenders will ask for proof of exemption.


‍


### The Three Solar Sizing Methods & Exceptions


#### 1. Conditioned Floor Area (CFA)


Multiply floor area in square feet by a table look‑up factor based on building type and climate zone. A **100 k ft² warehouse in Climate Zone 3** lands at **≈ 170 kWdc** of PV. This method usually results in the largest array, so designers use it only when roof real estate is ample.


#### 2. Solar Access Roof Area (SARA)


Take usable roof (minus skylights, setbacks, vents) and multiply by **14 W per square foot** . If that usable area is < 3 % of CFA *or* < 80 ft², PV can be waived entirely. SARA is popular on grocery stores and big‑box retail with busy roofs.


#### 3. Performance Energy Model Method


Model the whole building in **EnergyPro** or **CBECC‑Com** . A tighter envelope or high‑efficiency HVAC can offset some PV capacity. This is the weapon of choice for design‑build teams that already run Title 24 compliance models.


**Developer tip:** You may pick whichever method gives the *smallest* PV array—there’s no penalty for switching methods mid‑design as long as compliance forms are updated.


‍


### The Battery Sizing Method


#### Prescriptive Method


The prescriptive equation in Section 140.10‑B is simple:


**Battery usable energy in kWh = 2.5 × required PV power in kWDC**


So a mandated **200 kWdc** solar array compels a **500 kWh** battery. The code does **not** dictate duration (2‑hour vs. 4‑hour), but most owners choose 4‑hour to maximise demand‑charge savings. All batteries must meet **JA12** : ≥ 80% round‑trip efficiency and ≥ 70% end‑of‑life capacity at year 10. Containerised UL 9540A systems check those boxes out of the gate.


**Exceptions that eliminate the battery:**


**< 10 kWh threshold.**
Think server closet or small coffee kiosk add‑on. Anything needing less than 10 kWh usable capacity escapes the battery rule. In practice that is rare, because a PV system small enough to drive < 10 kWh is itself probably exempt (see next item). **‍**


**15% sizing relief.** ‍
When the SARA or Performance method shrinks your PV array relative to the CFA method, and that new PV size is below 15% of the original CFA value, the code lets the battery go. Developers use this for tight urban sites where shading reduces roof yield. **‍**


**Low‑rise multifamily carve‑out.**
**‍** For three‑story garden apartments, the state chose carrots over sticks: install a battery and you may trim the required PV by 25%. Otherwise, battery is optional. **‍**


**Disaster‑resilience projects.** ‍
Emergency‑only generators backed by a small battery system for egress lighting sometimes qualify for alternate compliance under CBC Chapter 27. Get the building official to sign off on *function* first, then file the energy code paperwork.


‍


### Permitting, Inspection, and Commissioning


#### The Process


**Permitting**
It all starts with schematics submitted to the electrical plan reviewers, consisting of single- and three-line diagrams. Electrical plan reviewers usually pass the specs to the fire department. Fire then checks UL9540A test reports, setback distances, and ventilation. If either side is missing data, the permit stalls. Submitting a single PDF package—PV one‑line, battery data sheet, fire layout—will reduce the review time significantly.


**Interconnection**
Utilities now treat export systems with power ratings over 500 kW almost like small power plants. PG&E’s WDAT queue is 9–12 months for final PTO. Start the process in schematic design or the site could be built but unable to energise the array.


**Commissioning**
JA12 demands that round‑trip efficiency, charging limits, and state‑of‑charge readings are verified *on site* . Container OEMs typically embed this test in their factory, but the California Energy Commission (CEC) still wants a field report. Conduit and ran inspections are also part of the process.


‍


### Financing & Ownership Options


The regulation allows the property owner to use different business models for procurement of the required solar + storage equipment. Below are the three most common options for ownership and financing options.


#### Cash Upfront


Companies with access to low-interest loans or with a surplus of cash may choose to buy and own the systems. By doing so, property owners can not only satisfy the Title 24 requirements but also claim the Investment Tax Credit (ITC) of 30% (though that may go away soon depending on how the new tax bill is structured in the Senate). However, the equipment will show up as depreciating assets. Equity partners often make the case that cash purchases for energy infrastructure can force trade-off with features and amenities that would improve property rentability.


#### C-PACE Financing


Commercial Property Assessed Clean Energy loans spread the cost over 20–25 years via a tax assessment. This financing structure would also give the ITC to the property owner. Rates are attractive and non‑recourse, but the lien sticks with the property into the next sale, sometimes forcing buyers to request a rate re‑set or discount on purchase price, which could be a complicating factor for investors with lower holding periods.


#### PPA or Lease


A Power Purchase Agreement (PPA) or Lease for the equipment will also satisfy the Title 24 requirements. In a Power Purchase Agreement - typically a 20-30 year contract - a block of electricity is pre-purchased at a pre-agreed upon price per unit of electricity ($/kWh). The installer could either pay a lease for the roof space while charging a similar price to the utility (minus the unpredictable price escalations), or not pay a lease in exchange for lower electricity rates. Under a PPA, there is typically a low or significantly reduced CapEx requirement, and there is a way to structure them off-balance sheet. An equipment lease is similarly low-to-0 CapEx and usually not a liability, but there are no performance guarantees - meaning that even if the energy infrastructure underperforms, the lease payment would stay the same. In both cases, the ITC would go to the company issuing the PPA or leasing the assets. Vista Power supports both leases and PPAs.


#### Energy-as-a-Service


Under an Energy-as-a-Service (EaaS) model, an energy developer like Vista Power would retain ownership of the assets and claim the ITC. In return, Vista Power's model is both $0 upfront and off-balance sheet, becoming a recurring operational expense instead. The payments for the system will be tied to actual performance and modeled in advance, giving both parties assurance that the project is profitable. The contract terms range from 5-15 years, with the higher end satisfying the Title 24 requirement. Developer‑owners focused on IRR and exit cap rates often prefer the EaaS model because it satisfies Title 24 without upsetting the balance sheet or impacting the CapEx calculation while offering performance assurance to future building owners and operators.


‍


## Need Compliance With Title 24?
Let's talk.


Vista Power offers $0 upfront, off-balance sheet energy storage and solar hardware. Reach out to us[here](https://vistapower.com/contact) to get a free Title 24 assessment - and, we'll do system sizing for free as part of our preliminary proposal for either an individual property or across an entire portfolio.


‍
