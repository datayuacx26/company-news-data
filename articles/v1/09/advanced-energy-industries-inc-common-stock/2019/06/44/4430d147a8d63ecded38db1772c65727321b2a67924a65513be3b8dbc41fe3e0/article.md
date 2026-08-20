---
schema_version: "1.0.0"
document_id: "4430d147a8d63ecded38db1772c65727321b2a67924a65513be3b8dbc41fe3e0"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/considerations-when-using-an-open-frame/"
published_at: "2019-06-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:59a9e5f8d392d76ed81fbb837f89e850a10732c88ccea27e27d77539ad5c86cb"
---

# Considerations When Using an Open-Frame

Power supplies are often supplied in an open-frame format. Open-frame typically means that the power supply will consist of a populated PCB minus any type of enclosure. The enclosure of the end-equipment that’s running the power supply will, by default, provide physical protection for the open frame power supply. In some mission, critical application extra protection to internal power supplies can be achieved by conformal coating. This protects the power supply from dust, contamination and humidity. Nevertheless, thermal management, safety, electromagnetic compatibility, and input voltage derating are all major considerations when employing an open frame power supply.


Figure 1: The GB60 is a 60-watt convection cooled open frame power supply meant for 1U applications.


### Thermal Management


An open-frame power supply reflects adherence to specifications as-is, with no enclosure included to encourage heat build-up. Many unique factors stemming from the application will affect the thermal performance of the power supply, such as the space surrounding it, heat dissipation of adjacent components, mounting position and orientation, the load to be applied, and available system cooling.


The open-frame power supply’s datasheet may list a power-rating that reflects expected results as it has been tested with forced-air cooling, convection-cooling, or both. However, once an open-frame power supply is installed, you should test the whole end-product’s system under any possible conditions that might push the end-products internal temperature to the max. If a single critical component’s maximum temperatures are met or exceeded, then additional cooling or remounting in a different location might be sufficient to reduce the operating temperature. If the open-frame power supply is in an enclosure with inadequate cooling, components that exceed their maximum temperature ratings will suffer damage. Damaged components might not affect performance immediately, but the damage will cause premature failure or a faster degradation in performance.


Thermal management is already part of the five performance dimensions of power conversion as well as a standard that SL Power engineers strictly adhere to. The five performance dimensions are Thermal Management, Reliability, EMI/EMC Performance, Power Efficiency, and Power Density, known altogether as TREPP.


### Safety


Spacing problems can cause last-minute failure once an application is built if the measurements of the open-frame power supply aren’t considered at the design stage. Mounting an open-frame power supply inside your equipment enclosure requires careful observation of clearance and creepage distances on all sides. Creepage refers to a path between a conductive part and the boundaries of the equipment enclosure as measured along the surface of any insulation that’s present. Insulation can create a conductive path that can cause a short circuit, and shorts can cause fires, so observing a distance that avoids creepage is a safe practice.


Figure 2: Creepage and clearance. (Source: Safety Considerations in Power Supply Design, Mammano & Bahra).


The main components in power supplies are power cords or terminal assemblies, circuit protection devices, EMI filters, wiring, power switches, transformers, rectifiers, output terminals, capacitors, and several other possible components. Preventing electric shock is a primary goal, and open-frame power supplies are traditionally meant to be enclosed as part of the end-equipment. Nevertheless, it’s not as uncommon as it should be to see an open-frame power supply on a hobbyist’s bench that’s being used as a power supply for bench testing purposes. It’s best to keep any open-frame power supply in a protective enclosure to prevent accidental shocks or contact with liquid spills. Open-frame power supplies are not meant to be used without an enclosure that insulates us from chance physical encounters.


Class I equipment uses protective earth grounding with basic insulation between the enclosure and components with hazardous voltage levels present. Creepage in Class I systems is 3 to 4 mm between any primary part and earth-grounded metal parts of the open-frame power supply, depending upon the application. A mounting point on the input side of the open-frame power supply is for a safety ground connection. Non-conductive enclosures can help to avoid creepage. A Class II power supply may require larger clearance and creepage distances if the enclosure is going to be made of metal.


Safety regulations corresponding to a particular application will direct you in attaching ground connections in order to comply, typically via the provided mounting holes and conditionally requiring several earth-ground connections. It’s possible that several earth connections to the open-frame power supply assembly may be required, so safety regulations should be considered at the design phase to avoid the impact of last-minute realizations. For more information on product safety, see SL Power Electronics’ Guide to Product Safety AN-G004.


### Electromagnetic Compatibility (EMC)


An open frame doesn’t have an enclosure that also acts to shield electromagnetic interference. EMC control technology is meant to suppress noises that electronics generate, as well as protect an electronic device from external noise. Input and output wiring for equipment or devices should be placed far apart, while also avoiding placement near the open-frame power supply.


Different countries have different EMC standards around the world, but all of them are meant to reduce conducted and radiated noise. Noise can be conducted as differential or common mode noise. It is common mode noise that passes through metal cases or frames. Noise can be reduced with shielding, bypassing noise to ground by mounting the open-frame power supply on a metal sheet and using noise-reducing components to reflect or absorb noise. An open-frame power supply usually has two or three mounting holes for connecting to ground. Mounting an open-frame power supply to a metal enclosure also offers grounding that improves EMC performance. An open-frame power supply with a partial enclosure on two sides and the bottom is called a U-channel enclosure. The top and two sides of the open-frame power supply are still open, and grounding is part of the ease in using a U-channel enclosure.
