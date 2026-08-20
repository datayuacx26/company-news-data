---
schema_version: "1.0.0"
document_id: "66dfdd172a368a9fc2f2e10c05c3a873ec5c8761a48d2f9ae2e28d59902fc6b0"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/3d-printing/vapor-smoothing-petg/"
published_at: "2023-08-26T04:00:13+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:65fbd69fbc0d5fc8c221024cbc7bc5918472db0aba03a5102f549c5079c70626"
---

# Vapor Smoothing PETG: Definition, Purpose, How It Works, Advantages, and Examples

Vapor smoothing is a low-labor post-processing technique for PETG parts that can produce a moderate to high surface gloss and significantly improve smoothness. The process works by exposing the printed part to a controlled, solvent-rich atmosphere. The solvent partially dissolves the outermost polymer layer, allowing the surface to flow and fill in microscopic valleys. Once the solvent evaporates, the re-solidified surface forms a continuous, glossy film.


This method can yield aesthetically appealing prototypes and improve the tactile feel of parts. However, it may reduce surface dimensional accuracy and obscure fine details due to the material flow during smoothing. Vapor smoothing is particularly useful for producing cosmetic prototypes that require a polished appearance, and for sealing porous parts produced by FDM (Fused Deposition Modeling) or FFF (Fused Filament Fabrication) into watertight or even airtight components. PETG is resistant to many common solvents, so only certain chemicals, such as dichloromethane or tetrahydrofuran, are effective, and they must be handled under strict safety protocols due to their toxicity and volatility.


This article will explain the vapor smoothing process for PETG, describe the underlying material science, and discuss the benefits, limitations, and safety considerations.


## What Is Vapor Smoothing PETG?


Vapor smoothing PETG refers to the post-processing technique used to improve the surface irregularity of 3D-printed parts made from polyethylene terephthalate glycol (PETG) filament or powder. These parts are usually made using FDM, FFF, and the various methods of PBF (powder bed fusion, sometimes SLM (selective laser melting)). These processes tend to build resolution, resulting in visible Z-axis steps that mar the surface.


Vapor smoothing is a method most commonly used with ABS (acrylonitrile butadiene styrene) prints, but it is increasingly applied to PETG. However, it's important to note that vapor smoothing PETG may not yield the same results as ABS. It might not also be as effective in achieving a glossy finish due to the differences in material properties. PETG is more resistant to commonly used solvents, so the effect is reduced compared to its application to ABS.


### What Is Vapor Smoothing PETG Also Known As?


In most material applications, vapor smoothing is also known as chemical smoothing, acetone smoothing, and chemical vapor smoothing.


## What Is the Purpose of Vapor Smoothing PETG in Manufacturing?


The purpose of vapor smoothing is to partially liquefy the surface of a 3D-printed part through controlled solvent exposure. This softens the outermost material, allowing adjacent layers to blur and merge. The result is a surface with reduced layer-step discontinuities, creating a smoother and more uniform appearance.


In manufacturing, this process serves three primary purposes. First, it enhances surface adhesion at the outer skin, effectively sealing pores and closing small gaps between extruded filaments. Second, it delivers a cosmetic improvement, as visible layer lines diminish and a glossy finish can often be achieved. Third, when performed under controlled conditions, vapor smoothing does not significantly weaken the original interlayer bonds (Z-direction strength) because the solvent penetration is limited to the surface rather than affecting the bulk of the material.


### What Are the Industries That Use Vapor Smoothing PETG?


Various industry sectors employ vapor smoothing techniques for 3D-printed parts, including PETG, such as the automotive, consumer goods, and medical sectors. Vapor smoothing can improve the surface finish of specific automotive components, improve non-precision parts such as soft or bone tissue models used for surgical planning, and improve consumer product cosmetic parts, such as electronics casings and fashion accessories.


Vapor smoothing with PETG is not as widespread as with other materials like ABS. PETG is chosen for its specific properties, such as toughness and transparency. Sometimes, its natural surface finish is preferred for specific applications. It is important to note that the vapor smoothing process requires very aggressive and harmful solvents, reducing the technique's applicability.


## How Does Vapor Smoothing PETG Work?


Vapor smoothing of PETG works by suspending the printed part in a controlled solvent vapor atmosphere. As solvent vapors condense onto the surface, they soften the polymer matrix by temporarily weakening intermolecular bonds between polymer chains, without causing significant depolymerization or chemical breakdown. This controlled surface dissolution allows material from adjacent print layers in the Z direction to partially flow and merge, reducing the sharp boundaries between them. As the solvent evaporates, the softened polymer re-solidifies, forming a more continuous and uniform surface layer. The final result is a part with an aesthetically smoother finish and reduced surface roughness.


Vapor smoothing can also seal porosity, improving water resistance and, in some cases, airtightness. Additionally, by reducing the severity of layer-step interfaces, vapor smoothing may offer a modest improvement in the anisotropic mechanical properties of FDM/FFF-printed PETG parts. However, this effect is generally secondary to the cosmetic and sealing benefits.


### What Is the Step-by-Step Process of Vapor Smoothing PETG?


Listed below is the general process of vapor smoothing PETG:


#### 1. Gather the Necessary Materials


The primary material needed for vapor smoothing is a chemical solvent that can dissolve the outer layer of the 3D-printed part, effectively smoothing the surface. For PETG, solvents like ethyl acetate, MEK, or dichloromethane are required. Prepare a gas-tight and solvent-resistant container (the vapor chamber) large enough to hold the 3D-printed part and the vaporizing solvent. A support or rack to suspend the 3D-printed part inside the container will also be needed. Additionally, prepare all safety equipment such as PPE and fire extinguishers.


#### 2. Prepare the Part


Most of the preparatory steps before vapor smoothing are simply those of post-print model completion. This includes removing any supports or soluble support material residues. Clean up any support blemishes that are part of the printed part. Fit the hanging support to the part to minimize scarring from support contact during vapor smoothing, ensuring the model's utility is not compromised. Finally, suspend the model in the vapor chamber.


#### 3. Safety Precautions


Various safety precautions should be followed, such as ensuring the space is well-ventilated, having a carbon filter VOC respirator available, and using it when handling the solvent or vapor-smoothing parts. On completion, use a forced vent to clear the chamber, then pass the air through an activated charcoal filter and outside. Use solvent-resilient gloves (latex, for example) when handling the solvent. Have a fire extinguisher on hand and leave post-smoothing parts suspended in a well-ventilated space to finish the solvent evaporation process.


#### 4. Preheat the Container (Optional)


Preheat the vapor chamber to an appropriate temperature, based on experience and tests. Don’t exceed a safe level (below 70 °C) to avoid the risk of bubbles developing on the prototype. Ideally, the chamber should be maintained at your target temperature for the period of the process. And if possible, this temperature and the timing of the process should be automated, for maximum consistency.


#### 5. Vapor Exposure


The duration and intensity of the vapor exposure are highly dependent on print material, model geometry, solvent type, solvent heating, and your experimental evaluations. The critical time/intensity required is sufficient to soften the surface without penetrating deeper into more vulnerable areas. The part should be left until a satisfactory degree of smoothing has been achieved. However, it should not be long enough to degrade model accuracy excessively or trigger distortion in vulnerable areas of the part.


#### 6. Seal and Monitor the Chamber


The vapor chamber must be gas-tight during processing to contain solvent vapors and should include a venting system with activated charcoal filtration before release. Assume that the surrounding workspace may still accumulate volatile organic compounds (VOCs), as solvent vapors can impair the sense of smell, making exposure harder to detect.


While the chamber is sealed, closely monitor the smoothing process, particularly for new geometries or unfamiliar print settings. Thin walls, unsupported sections, and low-infill regions are especially vulnerable: softened material can distort, collapse, or allow solvent to penetrate more deeply. Such damage is irreversible, so exposure duration should be carefully controlled and observed to balance surface quality with part integrity.


#### 7. Ventilation and Purging


When exposure is complete, purge the chamber through a filter system before opening. Assume the area immediately around the chamber is contaminated, as olfactory fatigue may prevent detecting solvent concentration.


#### 8. Post-Exposure Evaporation


After smoothing, leave the part suspended in a ventilated area to allow residual solvent to evaporate completely. This prevents surface tackiness and ensures mechanical stability.


#### 9. Post-Process Cleaning


Many parts will not require any cleaning after vapor smoothing if they went into the process contaminant-free and the process itself is clean. However, it's important to ensure all traces of solvent are evaporated using a post-vapor-smoothing stage. A simple wash with isopropyl alcohol and then a mild detergent solution, followed by a deionized water rinse and drying, will leave the models clean and ready to use.


#### 10. Final Inspection


Vapor smoothing can result in loss of detail, distortion, and other imperfections, so it is worthwhile to thoroughly examine and check for excessive dimensional changes, loss of symmetry, warping, etc., before passing parts as complete and ready for use.
