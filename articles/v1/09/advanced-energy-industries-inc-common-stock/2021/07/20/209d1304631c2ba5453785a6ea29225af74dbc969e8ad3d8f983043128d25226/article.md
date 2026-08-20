---
schema_version: "1.0.0"
document_id: "209d1304631c2ba5453785a6ea29225af74dbc969e8ad3d8f983043128d25226"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/electrolytic-capacitors-in-power-supplies/"
published_at: "2021-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:8eee6d8e9569966d54b5d4699e078ebe83a78d79843994edbbc6f60cd5ee35ac"
---

# Electrolytic Capacitors in Power Supplies

Electrolytic capacitors play an essential role in the design of switched-mode power supplies. They may be found in the power factor correction boost stage or as part of the wide input voltage range circuitry for energy storage. Electrolytic capacitors are also common components for filtering on the output of the power supply for low ripple voltage and stability. The specification of the power supply often states the lifetime of these electrolytic capacitors as a metric of quality. This article will discuss well-known effects upon electrolytic capacitors and their importance for a power supply design.


An electrolytic capacitor is a type of capacitor which use electrolyte in its internal construction. The electrolyte is a liquid with a high ionic concentration allowing much higher capacitance compared to other capacitor technologies. There are subcategories with liquid or also solid electrolytes but in most applications, the former is the first choice for a cost-effective yet small solution. The basic materials used in this capacitor type are a combination of aluminum foil, aluminum oxide, and electrolyte. Tantalum capacitors can provide better performance but are also more expensive. Ceramic capacitors offer excellent high-frequency performance but require more PCB surface area decreasing the power density of the power supply. The major advantage of the electrolytic capacitor is high capacitance density. The typical capacitance varies between 1µF and 100,000µF. The broad availability of different form factors allows the designers to select the best fitting electrolytic capacitor with respect to width and height. Another advantage of electrolytic capacitors is the impedance response versus frequency in complicated designs where the electromagnetic interference is close to limits. The equivalent series resistance (ESR) of electrolytic capacitors is not the lowest but compared to other capacitor types it remains at the relatively same level with increasing frequency. This is due to increasing resistivity of the electrolyte and so the compensation with capacitive reactance at higher operating frequencies. Very low ESR capacitors are available on the market. Electrolytic capacitors also do not suffer from the derating voltage effect that can be found in ceramic capacitors. The typical capacitance value of electrolytic capacitors is however not accurate, it is common to have a 20% tolerance of stated data in the datasheet due to the manufacturing process.


The global market forces every new power supply design to be more efficient, smaller but also to remain affordable. While manufacturers of active components find new technologies and ways of achieving better performance, the evolution of passive elements is very slow. The challenges of a switched-mode power supply designer remain to be creative not only in how better to control the power transfer chain but also how to achieve the highest power density without stressing the components. When it comes to the electrolytic capacitor, it sets the boundaries due to internal physical characteristics and sensitivity of the liquids to the heat.


The lifetime of the electrolytic capacitor is limited by its construction structure. The key limitation is the fluid electrolyte which evaporates out through the end seal with time. Higher temperature accelerates this physical process. It is therefore obvious, that the lifetime of the power supply is primarily dictated by the electrolytic capacitors among all other components on the PCB board.


The temperature of an electrolytic capacitor is due to two factors: The first is the ambient temperature near the capacitor. The second is the AC ripple current through the capacitor which causes additional internal heat. Designers use both these factors to estimate the lifetime based on information from capacitor manufacturer’s datasheets roughly the estimated lifetime. Manufacturers of the electrolytic capacitors provide an expected lifetime of the part at rated voltage and ripple current at maximum operating temperature. Exceeding these values shortens the lifetime or may even damage the component. Operating at a lower temperature allows extending the total lifetime. The AC ripple current capability of the capacitor is provided in the datasheet or additional application notes from manufacturers as well and has a similar impact on the lifetime of the unit. This combination of both predicts the estimated worst-case capacitor’s lifetime and indirectly the expected safe service life for the power supply itself. In total, the guaranteed working hours by using a selected capacitor in the target application and can be accurately estimated from simulations and validation tests.


There is however also an indirect influence on the capacitor’s temperature that is dependent on the environment or mounting manner of the capacitor on the PCB board. The leads of the capacitor can act as a heat sink but also as a heat absorber, in other words, can contribute to conductive thermal coupling. If the capacitor itself is the main heat creator and the surrounding components are cooler, the leads would transfer heat from the capacitor’s core into the PCB board and so dissipate the heat outside. On the other hand, if there is another component like a transformer, FET, or another hot spot close to it, heat will be conducted via the leads directly into the capacitor. In most cases, this effect has a low impact on total temperature rise but should be kept in mind. Besides the conductive heat transfer, it is common to observe radiated thermal emissions close to a capacitor. This effect is especially observed close to transformers, where for example two or more capacitors with the same value and electrical connections located close to each other on PCB show different behavior. The capacitor mounted near the hot spot will absorb more radiated heat and will have a higher surface temperature. This capacitor needs to be evaluated closer to avoid issues later.


A capacitor that has reached its lifetime can be considered as not functional because the output ripple voltage is not guaranteed anymore or for the intermediate PFC storage capacitors the hold-up time is below the defined ratings.


Deep benchmarking of the power supply under normal and/or harsh conditions investigates the weaknesses of the unit for each scenario and so allows the designs to take these results into the consideration during the integration phase. Assuming the capacitors are the limiting factor with high sensitivity to temperature, the assembly of the system parts needs to be reviewed closely to avoid unnecessary additional stress.


The common practice for[application engineering teams](https://www.advancedenergy.com/en-us/support/) is to work together with the end customer on the thermal concept to avoid unnecessary heat traps close to lifetime limiting components such as electrolytic capacitors in the final system design.
