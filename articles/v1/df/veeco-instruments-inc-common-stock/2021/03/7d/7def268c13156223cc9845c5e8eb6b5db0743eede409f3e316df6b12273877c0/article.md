---
schema_version: "1.0.0"
document_id: "7def268c13156223cc9845c5e8eb6b5db0743eede409f3e316df6b12273877c0"
company_key: "veeco-instruments-inc-common-stock"
company: "Veeco Instruments Inc."
source_id: "veeco-instruments-inc-common-stock-rss-ca17b9656fba"
canonical_url: "https://www.veeco.com/company/blogs/do-you-have-the-right-process-tools-for-manufacturing-photonics-devices-in-high-volume/"
published_at: "2021-03-11T15:40:19+00:00"
first_seen_at: "2026-07-25T01:53:19.132600+00:00"
fetched_at: "2026-07-28T22:26:36.656438+00:00"
content_hash: "sha256:5c55a68f5bb9612af8e2a673c499488bb4cb374ae3ffe948db8dcf6f058bba90"
---

# Do You Have the Right Process Tools for Manufacturing Photonics Devices in High Volume?

## Do You Have the Right Process Tools for Manufacturing Photonics Devices in High Volume?


Blogs | Mar 11, 2021


The growth outlook for the photonics market is promising thanks to the exploding demand for data—particularly in the midst of the global stay-at-home economy—and the rise in cloud computing and IoT applications. According to Yole Développement, by 2025, the data transmission market will nearly double its 2020 levels, with data communications (24% annual growth) and telecommunications (14% annual growth) serving as the primary drivers.


Unprecedented investments in data centers over the past five years will continue to dominate growth in the datacom space. According to IDC and Seagate, data consumption will grow from around 44 terabytes in 2019 to over 175TB in 2025. In addition, much of the data generated will need real-time processing.


For both datacom and telecom, optical signal transmission enables faster networks. This is driving increased demand for photonics devices, particularly indium phosphide edge emitting lasers (InP EELs) at 1.3/1.5 micron wavelengths; gallium arsenide vertical-cavity surface emitting lasers (GaAs VCSELs) at 850nm; and photodetectors. High performance and reliable devices with excellent uniformity are vital for these applications. Multiple technologies play a key role in enabling reliable high-volume manufacturing at scale. This paper discusses the advanced processes required to manufacture photonics devices in high volumes, and what is required of today’s process tools to make that happen.


**Why MOCVD Matters: Efficiency and Reliability**


Two of the main parameters vital to semiconductor lasers are power conversion efficiency (PCE) and reliability. Metal Organic Chemical Vapor Deposition (MOCVD) impacts two key aspects of PCE: external efficiency and series resistance.


*External efficiency* must be high to achieve optimal PCE, and is affected by:


- Materials quality. Lasers are made from materials synthesized by MOCVD—the better the quality of the materials, the better the external efficiency.
- Interface sharpness. Since compound semiconductors comprise many layers of different materials, the MOCVD system must enable the sharpest possible interface between layers.
- Background doping and memory effect. If the MOCVD reactor in which the structures are grown isn’t clean, excessive amounts of impurity or material from a prior layer can linger in the chamber and become incorporated; for example, carbon may infiltrate an n-type aluminum GaAs (AlGaAs) layer, degrading conductivity.


*Series resistance* must be low to achieve the highest possible PCE. This also requires high interface sharpness, as well as low memory effect. Therefore, when gas sources and dopant materials are switched between layers, the reactor should evacuate the prior material completely so that new layers can be grown on top of each other with no overlap. If this is not done properly, problems with device performance may ensue.


Additionally, building the highest-reliability devices requires high quality materials with low background concentration and high mobility. MOCVD growth defines materials quality, interfaces and background impurities, and therefore is highly critical for device reliability.


**MOCVD Uniformity Is Critical**
Higher data traffic drives more channels and, therefore, increasingly tighter wavelength uniformity. The current 100Gb optical interface specification calls for using four lasers at 25Gbps, each emitting a different wavelength in a different color so that the detector can differentiate them.


Datacom uses coarse wavelength division multiplexing (CWDM), which is based on 20nm spacing between the channels. However, as data volumes continue to increase, more channels will be needed, so spacing will get tighter. Telecom, on the other hand, uses dense wavelength division multiplexing (DWDM), where between-channel spacing is 0.8nm to accommodate the high number of channels and tunability requirements, which in turn drives even more stringent uniformity and yield demands.


For example, on a 3-inch InP substrate, typically used for datacom lasers, the requirement for wavelength uniformity within the wafer is σ <1.5nm (or ~9nm range). Thus, to realize 100% yield, all the wavelengths of devices on the wafer must be within 9nm, with 20nm spacing.


A well-designed MOCVD reactor enables high-performance and uniform optical devices. Conversely, a poorly designed MOCVD reactor leads to poor device performance and large variation of device characteristics (such as wavelength) across wafers and runs, leading to low yield and high cost. While post-MOCVD processing can create its own yield loss issues, a well-designed reactor provides the highest device performance with the least amount of variation and, therefore, the highest yield.


**Meeting HVM Requirements for EEL**
Producing EEL devices at scale calls for a high-performance, low-maintenance tool that can grow layers for multiple devices with different structures, wavelengths and materials, over multiple campaigns and across preventive maintenance (PM) cycles. Thermal stability and run-to-run repeatability are also critical for production continuity and high production yield.


All of this requires a well-designed reactor with no drift, minimal recipe tuning, and ease of maintenance with fast post-PM recovery. Performance should be consistent within recipes as well as between recipes when changing over to a new device production schedule, which should be accomplished quickly with no downtime between runs. The tool should offer full in situ measurement and control over two key elements to ensure reactor stability:


- **Flux (gas) delivery** . Having a flux controller for each source used to manufacture devices enables process stability over time by real-time delivery control.
- **Temperature** . Emissivity-compensated temperature control is necessary for lattice matching, doping and composition uniformity, and to eliminate run-to-run process drift. Lack of optimal temperature uniformity, either within or between runs, will lead to low yield or the need for constant recipe tuning.


**Veeco’s Solutions for High Volume Manufacturing or Photonics Devices**
Veeco’s solution to MOCVD process challenges is Lumina® MOCVD platform, its most advanced batch MOCVD system, designed specifically to create high-performance next-generation photonics devices in the volumes needed for datacom/telecom applications. The Lumina platform can deposit high quality arsenide phosphide (AsP) epitaxial layers on wafers up to 200mm in diameter. At the heart of the Lumina platform is the TurboDisc® reactor, specifically designed to grow epitaxial structures with the industry’s highest uniformity and lowest defectivity.


Designed for rapid development and high volume manufacturing (HVM) ramp, the TurboDisc technology enables uniform film growth. In the reactor, alkyls and hydrides are distributed in an alternating pattern across the injector with laminar flow, while precursors are separated to avoid premature mixing and parasitic chemistry. This prevents unwanted material deposition in the reactor wall or the injector, thus eliminating reactor drift. Gas distribution is determined only by the physical geometry of the hardware, not by process tuning, thus allowing a wide process window.


The reactor’s vertical injection and high-speed rotating disc together work to quickly “pull” gas down, and the chamber is then evacuated by a vacuum pump through exhaust below the wafer carrier. All of this happens quickly, so gas resides only briefly in the chamber without lingering or coating areas outside of the deposition plane. The resulting materials feature sharp interfaces with no background doping or memory effect.


Typical recipe time is about 4.5 hours for VCSELs and 2.5 hours for thinner EELs or micro-LEDs. Uptime averages more than 300 runs between PM, which can be performed in less than one shift (about four hours) with zero recovery time—one quick bake, and it can go right back into production. Data indicates PM recovery within two runs is ±0.2nm wavelength repeatability, with no post-PM conditioning or recipe tuning.


**Results Showcase Performance**
Actual run results underscore Lumina’s high performance and repeatability. For example, typical Lumina runs of a Q1300 bulk InGaAsP VCSEL for telecom on 3 inch wafers yielded virtually identical average wavelengths (1324 and 1325nm over 2 runs), with σ average of 1.24nm and run-to-run range of ±0.5nm, as well as within-wafer wavelength uniformity (1 σ ) across 3 inch wafers.


Additionally, the system exceeds requirements for VCSEL HVM uniformity specs. For a typical 7×6 inch 940nm VCSEL for 3D-sensing, it delivers average within-wafer σ <0.12%; wafer-to-wafer uniformity of less than +/- 0.5nm; and within-wafer yield higher than 98%.


**Wet Etch Technologies: A Gentler Approach**
Today many device manufacturers use dry etch techniques to pattern the mesa and waveguide structures of telecom devices like InP lasers.


While offering precise process performance, dry etch tends to damage the underlying epitaxial layer and negatively impact performance. Wet etch offers a gentler approach to pattern the device without the damage. However, it is difficult to run high volume processes using beakers or bench-scale wet etch techniques. A high-volume, production worthy wet etch system is needed to deliver repeatable process performance. The critical features for production customers are etch stability, uniformity and endpoint detection.


*Etch stability*
A typical wet bench or beaker approach for etch can suffer from chemistry degradation and etch instability. In the figure below, the blue line shows typical chemistry degradation over an eight-hour period leading to instability in etch rate. The WaferEtch platform has a chemistry spiking feature (green line) that adds etchant chemistry periodically to maintain the chemistry concentration and stabilize etch rate.


*Etch non-uniformity*
The WaferEtch platform features a hyperbolic arm-scan function that allows users to control arm speed as it dispenses etchant across the wafer. The figure below shows how the user can manipulate the arm-scan profile to optimize the etch profile on the wafer. Compared to beaker or wet bench, the WaferEtch can achieve nonuniformity of <3%.


*Endpoint detection*
Detecting when the etch process has finished is critical to minimize over-etching, reduce chemistry use and increase throughput. Veeco’s endpoint system monitors the color change of the wafer and detects the endpoint by color change. The figure below shows the endpoint detection scan of InP etch over InGaAs substrate. The endpoint scan shows the RGB signals changing as the InP layer is etched. These signals then stabilize as the InP is removed and InGaAs is reached.


The WaferEtch platform offers the critical features to enable repeatable, high-volume production.


**Ion Beam Sputtering for EEL Diode Longevity**
The need for faster datacom networks is increasing as companies like Amazon and Facebook invest in hyperscale data centers to accommodate cloud usage. 5G networks promise to deliver the communications speed needed to keep the data flowing. The 5G backbone comprises not only wireless but also fiber-optic communication, which is critical for data exchanges between cell towers, over the internet and between city-block macro-cells.


One of the challenges in realizing widespread 5G telecom infrastructure is keeping the cost low. This leads to the requirement for low-cost DWDM filters (100 and 50 GHz) with low insertion loss and high isolation, while maintaining a wide pass band (per ITU specs). Band pass filters comprising >100 layers of alternating high and low index material deposited with high thickness precision helps meet these challenging requirements.


On a different front, EEL diode material is continuously exposed to self-generated laser damage and unpredictable environmental conditions. Therefore, key requirements to maintain EEL diode longevity in telecom and datacom applications are high-laser-induced damage threshold (LIDT) and film stability regardless of environmental conditions.


Ion beam sputtering (IBS) produces environmentally stable, near-bulk density films with Angstrom-level surface roughness and low film defects. Anti-reflection coatings (with R ~0.01% or better) and high reflection coatings (R ~99.9% or better) with superior optical properties are also possible. If the IBS platform used has an assist source capable of operating at low energies and with a variety of gas species, energy-sensitive material like GaAs and InP can be efficiently precleaned before further processing. In situ cleaning and passivation of both sides of the laser bars before deposition will save them from detrimental effects of exposure to environmental oxygen. The resulting EEL diodes have best-in-class LIDT values (exceeding 140 J/cm2).


For decades, Veeco’s SPECTOR Ion Beam Sputtering platform has been the go-to equipment in the market for synthesizing band pass filters for DWDM application. Supplemented with laser optical monitoring, the system enables high layer thickness precision along with high process stability through the duration of the production run (as high as 48 hours of coating time). The resulting yields for challenging 100 GHz filters can exceed 10,000 mm2 in a single run, making it a highly profitable investment for DWDM filter manufacturers. The new SPECTOR 5G platform can be used to synthesize filters (besides other optical applications) for telecom applications including DWDM, CWDM, LANWDM, beam splitter, etc.


**Conclusion**
Manufacturing photonics devices from compound semiconductor materials in volumes high enough to meet today’s data centers demands calls for a unique set of process capabilities and the right set of tools to perform them. Photonics devices manufacturers are turning to advanced MOCVD, wet etch and ion beam sputtering processes to improve performance, reliability, efficiency and yield of devices. Successful application of these processes requires the right tool set. Veeco offers a full suite of solutions designed for successful HVM of photonics devices.
