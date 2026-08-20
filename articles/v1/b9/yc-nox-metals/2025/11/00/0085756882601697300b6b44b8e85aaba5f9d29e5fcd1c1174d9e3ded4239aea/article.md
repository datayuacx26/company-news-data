---
schema_version: "1.0.0"
document_id: "0085756882601697300b6b44b8e85aaba5f9d29e5fcd1c1174d9e3ded4239aea"
company_key: "yc-nox-metals"
company: "Nox Metals"
source_id: "yc-nox-metals-news-import-4d0abe59faf1"
canonical_url: "https://noxmetals.co/blog/how-to-read-mill-cert"
published_at: "2025-11-15T00:00:00+00:00"
first_seen_at: "2026-07-24T06:33:38.984322+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:a6721cb6660367e54b871a60e05202219be1e22cbe51412de4e6a179832143c3"
---

# How to Read an Aluminum Mill Certificate

## What a Mill Certificate Is


A mill certificate is a document produced by the aluminum producer (mill) at the time of manufacture that records the actual chemical composition and mechanical test results for a specific heat or lot of material. It certifies that the material meets the requirements of the referenced specification (AMS, ASTM, or both). The cert travels with the material through the distribution chain and should accompany every purchase, whether from a mill direct or from a service center.


## Heat Number and Lot Number


The heat number (sometimes called cast number) identifies the specific melt from which the material was produced. All plate rolled from the same cast shares a heat number, and this number is the foundation of traceability. The lot number further identifies the specific rolling or processing batch within a heat. For aerospace procurement, every piece of material should be traceable to a heat number, and that heat number should appear on both the cert and any labeling on the plate or packaging.


Note


Some distributors consolidate multiple heats onto a single location in their warehouse. When receiving material, physically verify that the heat number stamped or labeled on the plate matches the heat number on the cert you received. A mismatch is a non-conformance that requires a corrective action before the material can be used.


## Alloy and Temper Identification


The cert must state the alloy and temper explicitly - for example, 7075-T651 or 6061-T651. These are not interchangeable, and a cert that states only the alloy without the temper is incomplete. The temper affects the mechanical properties and the applicable specification limits, so a cert missing the temper cannot be used to verify conformance to mechanical requirements. If you receive a cert with no temper designation, contact the distributor for a corrected document.


## Chemistry Section: Limits vs Actuals


The chemistry section of a mill cert lists each alloying element with the specification minimum and maximum limits alongside the actual tested values from the heat. For 7075, for example, you will see columns for Zn, Mg, Cu, Cr, Si, Fe, Mn, Ti, and other elements. The actual values must fall within the limits - values at the boundary are acceptable, but values outside the limits mean the heat was rejected (and should never appear on a cert you receive). Comparing actuals to limits confirms the alloy is correctly formulated.


## Mechanical Test Results


The mechanical test section reports the actual tensile test results from samples taken from the lot: ultimate tensile strength, yield strength (0.2% offset), and elongation. These are reported alongside the specification minimum values so you can directly confirm conformance. Pay attention to the test direction - longitudinal (L), long transverse (LT), and short transverse (ST) tests have different minimums, and the cert should state which direction was tested. Aerospace structural parts often have requirements in multiple directions.


## AMS and ASTM Compliance Statement


A complete cert will include an explicit statement that the material was produced in conformance with the referenced specification, such as AMS-QQ-A-250/12 or ASTM B209. This statement, signed by a quality representative from the mill, is the legal certification of conformance. If the cert shows chemical and mechanical data but does not include a conformance statement, it is a data sheet rather than a certification and does not satisfy AS9100 incoming inspection requirements for aerospace-grade material.


## Country of Origin and Producer Information


The cert should identify the producing mill by name and location. For DFARS-applicable contracts, the country of melt and country of manufacture must be documented (see the DFARS compliance article for detail). Many domestic certs will simply state the mill name and U.S. address, which implicitly confirms domestic production - but for explicit DFARS compliance documentation, a statement of country of origin is preferred and can be requested as a supplemental certification.


## How to Match the Cert to Your PO


When a shipment arrives, the receiving checklist should confirm that the alloy, temper, thickness, and dimensions on the cert match what was ordered on the purchase order. The heat number on the cert should match the identification mark on the material. The cert quantity should cover the quantity received. Any discrepancy - wrong temper, missing heat number, quantity mismatch - should trigger a material hold and supplier corrective action before the material is accepted into stock.


## Common Red Flags


-


Missing temper designation (e.g., cert says '7075' with no T6/T651 designation)
-


No heat number on cert or no heat number marked on material
-


Faxed or scanned copy with illegible data fields
-


Mechanical test results missing or showing only specification limits without actuals
-


Producer name or location not stated
-


Conformance statement absent or unsigned
-


Chemistry actuals outside specification limits
-


Cert date significantly older than the purchase order (material may have been in storage and re-certified)
