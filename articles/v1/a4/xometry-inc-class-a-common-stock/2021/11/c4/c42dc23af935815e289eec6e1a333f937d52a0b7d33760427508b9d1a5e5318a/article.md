---
schema_version: "1.0.0"
document_id: "c42dc23af935815e289eec6e1a333f937d52a0b7d33760427508b9d1a5e5318a"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/3d-printing/how-to-avoid-out-of-spec-flatness-due-to-part-bowing-in-3d-printing/"
published_at: "2021-11-09T13:00:11+00:00"
first_seen_at: "2026-07-22T21:40:02.880011+00:00"
fetched_at: "2026-07-28T22:26:35.155530+00:00"
content_hash: "sha256:76afde853b9048f4f7ff84c6c0cc38640d7064bf34b2e781d86284a093d3aaf6"
---

# How to Avoid Out of Spec Flatness due to Part Bowing in 3D Printing

Part bowing in 3D printing is a phenomenon whereby the corners of a printed part lift off the build plate. Hobbyists and professional 3D printers alike, whether they use an FDM (fused deposition modeling) or resin printer, will experience part bowing at some point in time.


Before we explore methods to prevent part bowing, we have to understand why part bowing occurs.


## What is part bowing and why does it happen?


There are different causes of part bowing in[FDM](https://www.xometry.com/capabilities/3d-printing-service/fused-deposition-modeling/) and[resin prints](https://www.xometry.com/capabilities/3d-printing-service/stereolithography-3d-printing/) . With FDM printers, it typically occurs because temperature differences between the printed layers are too large, creating tension inside the part that eventually leads to bowing.


With resin printers, part bowing can have many causes – typically one or a combination of the following:


- The part has insufficient support
- The exposure time is too high or too low
- Part orientation is not optimal, causing weakness
- The use of low-quality resins with weaker properties
- Wall thickness is too low
- Layer height is too high for the model
- Over-curing prints


**Design Guide: CNC Machining**


Download our guide for CNC manufacturability recommendations to optimize your designs, reduce machining time, and lower your costs.


[Download for Free](https://www.xometry.com/resources/design-guides/design-guide-cnc-machining/)


## Preventing Part Bowing of FDM Prints


It is not uncommon for FDM printers with mild steel print beds to experience print bed warping, which in turn leads to part bowing. If your printer suffers from print bed warping, consider switching to a glass print bed.


### Temperatures


The temperatures of the filament, print bed, and print-environment are the most important factors affecting part bowing. There are several steps you can take to control temperatures to prevent part bowing.


#### Heated Build Plate


A heated build plate will keep the material just below the glass transition temperature and help even out the temperature in the part. For ABS material the recommended build plate temperature is 100–120°C. A filament manufacturer’s datasheet will recommend build plate temperatures.


#### Enclosed Print Chamber


A heated build plate may not be able to keep the upper layers of your part from cooling too fast. Place your printer inside a chamber that can regulate the build-volume temperature. If your printer does not include a heated enclosure, be sure to keep the printer’s enclosure closed to retain heat.


#### Room Temperature


Keep windows and doors closed to prevent cold drafts from reaching the printer and part. Exposing the part to cold air will cause rapid cooling, introducing stress in the part and promoting part bowing.


#### Cooling


The first few layers are critical to ensure adhesion to the build plate and create a stable base for the print. Switch off cooling fans directed at the part to allow the first few layers to retain heat and cool at a slower rate.


### Adhesives


Ensure that the build-plate surface is free from any oils or dust. If you do not have a specialty product like specially formulated 3D printing bed spray, you can use hair spray or a PVA glue stick. Always do a small test print to ensure that the part is not permanently stuck to the build plate.


### Slicer Settings


There are slicer settings that can help reduce part bowing. These settings can be changed after finalizing your design without impacting the design of the part.


#### Print Speed


Reducing the speed of the print will allow more even cooling of the layers. Remember to also reduce nozzle temperatures when reducing print speed as material flow through the nozzle will be lower. By not reducing the temperature you risk degrading or burning the material.


#### Brims and Rafts


A brim is a single-layered, flat area around the part, attached to the edge of the base of the part, increasing its surface area. This gives better adhesion and reduces part bowing. Figure 1 shows a brim added to a part.


A raft is similar to a brim and is used when a brim will not prevent part bowing. A raft is a thick grid printed between the build plate and the part and ensures that heat is distributed evenly. Figure 2 shows a raft between a build plate and a part.


## Preventing Part Bowing of Resin Prints


Liquid resins undergo many changes as they cure. Curing from a liquid to a solid plastic means there can be shrinkage and expansion, adding stress between the layers of the part. There are several actions you can take to prevent part bowing of resin prints.


### Model Support


You cannot print part features in midair, so ensure you have sufficient light, medium, and heavy supports for overhangs or unsupported features. Insufficient supports may cause the resin suction pressure to lift layers from each other and deform the part.


### Exposure Time


Parts that are underexposed will not be strong enough. You may also find that supports are flimsy and weak, or not fully printed. This in turn jeopardizes the integrity of the model's support system, causing part bowing.


### Part Orientation


Optimal part orientation will minimize the need for supports and potentially eliminate them. For larger models it is recommended that you rotate the model 15–20° away from the build plate to reduce the surface area of each layer, thus reducing the resin suction pressure.


### Resin Quality


A lack of flexibility or toughness of the resin may increase part bowing. Lower-quality resins tend to have weaker properties that also increase the likelihood of part bowing. Use high-quality resins or resins with better toughness and flexibility characteristics. It is possible to mix higher-toughness resins, or resins with higher flexibility, into normal resins as a way to add durability to parts.


### Wall Thickness


Part bowing can occur if the wall thickness of hollow parts is too low. Wall thicknesses are commonly between 1.5 mm and 2 mm. But increasing wall thickness to at least 2 mm whenever possible will give parts more rigidity.


### Layer Height


Large layer heights mean more material that must cure. This in turn increases internal stresses and the potential for part bowing. Reduce your layer height from the typical 0.05 mm to somewhere between 0.025 mm and 0.04 mm to try to reduce part bowing.


### UV Curing


UV curing of your model may not seem an obvious cause of part bowing. But having small or delicate features that absorb most of the UV light will result in uneven curing and then bowing. Try to use a UV curing solution with a rotating turntable that exposes the part’s surfaces more evenly.


Now that you know how to prevent part bowing you can design and 3D print your next project with greater success. And if you’re unsure about the process, reach out to the experienced team at Xometry for help.


**Start Your Instant Quote**


STEP · STP · SLDPRT · STL · DXF · IPT · X_T · X_B · 3DXML · CATPART · PRT · SAT · 3MF · JT


[Upload Your Design](https://www.xometry.com/quoting/home/) All uploads are secure and confidential
