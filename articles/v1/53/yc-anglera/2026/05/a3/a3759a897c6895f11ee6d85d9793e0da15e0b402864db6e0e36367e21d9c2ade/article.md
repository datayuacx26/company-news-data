---
schema_version: "1.0.0"
document_id: "a3759a897c6895f11ee6d85d9793e0da15e0b402864db6e0e36367e21d9c2ade"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/hvacr-attributes"
published_at: "2026-05-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:12:51.498055+00:00"
content_hash: "sha256:875f8412917d467cf60d2984c977b620e5024b15e55d0c782ea089ec05a87cc4"
---

# The hvac/r attributes buyers filter on — and most catalogs miss

A distributor rep can tell you the difference between a` 24SCA436` and a` 24SPA636` in about four seconds. A filtered search box can't, unless someone put that difference into structured fields. Most condensing unit catalogs still describe products the way a spec sheet PDF does, in prose, which means the exact attributes buyers and AI answer engines filter on are sitting in a paragraph nobody can query.


## The attributes that actually drive a condensing unit purchase


Contractors and engineers aren't browsing HVAC/R catalogs. They're filtering. A rep sizing a replacement unit, or an estimator matching a spec, works down a checklist before they ever open a product page. Based on how AHRI's own certified product directory is structured, and what shows up on every serious manufacturer submittal sheet, the attributes that actually gate a decision are:


- **Nominal cooling capacity** — tons and BTU/h, plus the specific staging (single-stage, two-stage, variable-capacity/inverter)
- **Refrigerant type** —` R-410A` ,` R-454B` , or` R-32` , and increasingly whether the unit is A2L-rated
- **Efficiency rating** — SEER2 and EER2 (the DOE moved to SEER2/EER2/HSPF2 testing metrics starting January 1, 2023, per[SEER2.com](https://seer2.com/) )
- **Electrical characteristics** — voltage/phase (208-230V/1ph, 460V/3ph, etc.), minimum circuit ampacity (MCA), and maximum overcurrent protection (MOP)
- **Compressor type** — scroll, variable-speed, or reciprocating
- **Sound rating** — dBA at a stated distance
- **Refrigerant line set sizes** — liquid and suction line diameters, and maximum equivalent line length
- **Low-ambient operating range** — the minimum outdoor temperature the unit can start and run at, with or without a kit
- **AHRI certified reference/match number** — the specific indoor coil combination the rating applies to
- **Coil construction** — fin-and-tube vs. microchannel, and corrosion protection (e.g. e-coated)
- **Physical footprint** — dimensions, weight, and service clearance


AHRI's own directory guidance confirms this is exactly how buyers narrow a search: filtering "by manufacturer, model number, voltage, and cooling capacity" to find[certified condensing units that meet a specific requirement](https://www.ahrinet.org/certification/cee-directory/how-use-directory) . If your catalog only has three of those eleven fields filled in, you're filterable on three axes. Everything else, a buyer assumes doesn't exist.


## Why the refrigerant field is the one that's breaking right now


This isn't a theoretical taxonomy problem. As of January 1, 2025, manufacturers can no longer build new residential and light-commercial equipment on` R-410A` — the industry has shifted to A2L refrigerants, primarily` R-454B` and` R-32` . Critically, the EPA does not allow an A2L unit to be dropped in as a straight retrofit for R-410A equipment; the outdoor unit and indoor coil have to be a matched, certified pair, and the whole system typically needs a redesigned coil with integrated safety features (see[ACDirect's transition guide](https://www.acdirect.com/blog/r454b-refrigerant-2026-homeowner-guide/) ).


That means "refrigerant type" isn't a nice-to-have attribute anymore — it's a compatibility gate. A distributor whose catalog doesn't cleanly separate R-410A legacy stock from R-454B/R-32 replacement units is going to get filtered out by any contractor (or any AI tool helping one) searching "3-ton R-454B condensing unit, matched coil." If that field is buried in a title string or missing outright, the SKU doesn't just rank lower. It doesn't show up.


## Ask an answer engine


Try asking an AI assistant something like: *"What 3-ton R-454B condensing unit is rated for low-ambient operation down to 0°F without a kit?"*


That query requires four structured facts to line up simultaneously: tonnage, refrigerant, low-ambient rating, and kit-dependency. If a manufacturer's feed has those as four separate, consistently-labeled attributes, the SKU is answerable. If they're scattered across a PDF spec sheet and a marketing paragraph, the answer engine has nothing to extract, and it recommends a competitor whose data happens to be structured.


## Before and after: a 3-ton condensing unit


Here's what a typical raw feed record looks like next to what a buyer (or an AI answer engine) actually needs.


**Raw feed description (typical):** "Carrier Comfort 3 Ton Condensing Unit R-410A. Reliable outdoor unit for residential cooling applications. Scroll compressor. 15 SEER2. Contact rep for specs."


**Enriched attribute table:**


Attribute Value


Nominal capacity 3 ton (36,000 BTU/h)


Refrigerant R-410A (legacy, matched coil required)


SEER2 / EER2 15.0 / 11.5


Compressor type Single-stage scroll


Voltage / Phase 208-230V / 1-phase / 60Hz


MCA / MOP 18.2A / 30A


Sound rating 72 dBA


Liquid / suction line 3/8 in / 3/4 in, up to 250 ft equivalent


Low-ambient operation Down to 0°F with accessory kit


AHRI reference number Matched coil required for certified rating


Coil type Aluminum fin-and-tube


Dimensions / weight 30 in x 30 in x 34 in / 165 lb


Application Residential, ducted split system


The description isn't wrong, exactly. It's just unusable for filtering. Nothing in that first paragraph can populate a faceted search filter, a comparison table, or an answer-engine response. Every line in the table on the right can.


## How to structure it so it survives


The fix isn't a longer product description. It's a fixed attribute schema, applied consistently across every condensing unit SKU in the line, with controlled values (not free text) for refrigerant type, voltage class, and application. Distributors carrying multiple manufacturers need that schema to be brand-agnostic, so a 3-ton R-454B unit from one OEM filters the same way as the equivalent from another.


That's the mechanical work most catalogs quietly skip, because pulling MCA, MOP, line sizes, and low-ambient ratings out of a stack of PDF spec sheets runs about 30-45 minutes of manual work per SKU, multiplied across thousands of condensing units, coils, and package units in a full HVAC/R line.


This is the exact gap Anglera is built to close. It doesn't replace your PIM or touch your CRM — it plugs into Akeneo, Salsify, inriver, or a flat file, extracts the real values sitting in supplier spec sheets, scores each SKU for completeness, and fills the attribute table so condensing units, coils, and package units show up in every filter a buyer or an AI answer engine actually uses. Most catalogs can be live within 30 days, starting from whatever data they already have.
