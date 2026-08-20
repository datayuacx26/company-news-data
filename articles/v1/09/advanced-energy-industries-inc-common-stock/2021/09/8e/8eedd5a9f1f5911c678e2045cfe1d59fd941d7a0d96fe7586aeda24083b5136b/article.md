---
schema_version: "1.0.0"
document_id: "8eedd5a9f1f5911c678e2045cfe1d59fd941d7a0d96fe7586aeda24083b5136b"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/electrical-fast-transients-and-bursts-from-power-supply-perspective/"
published_at: "2021-09-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:00316d362a0ed91e7c468c7ba068b090ad122c30509c94766f6bb0f6a59864a5"
---

# Electrical Fast Transients and Bursts from Power Supply Perspective

### Introduction


It has been established for decades that electrical equipment connected to an electrical grid must withstand several tests required by international and/or national standards to ensure proper functionality of the device and of other equipment connected to the same mains network. Standards are prescribing safety-related requirements and test procedures with a focus to standardize protection against electrical shock while using a product.


This is dependent on the area of use such as IEC60601-1 for medical, IEC601010-1 for test and measurement, or new IEC62368-1 for audio/video/communication and information equipment. An additional and often more complicated part of product development is to design a system with sophisticated electromagnetic immunity and compatibility within a given environment. Country EMC standards usually reference IEC61000-4-4 to define test methods for electrical fast transients and bursts (EFT/B).


### What is EFT in real life?


It is common to have multiple equipment types connected to a utility network with inductive loads like motors, transformers, chokes, relays, and/or switches. Upon disconnecting such devices from the AC mains, the stored energy can discharge and create voltage spikes on power lines. This effect will be seen on other devices connected to the same AC phase as fast voltage transients or bursts. These fast transients or bursts can cause malfunction of the product if its protection is insufficient.


Considering the discharge paths there are two ways of having fast transients in the electrical product, common-mode or differential mode. While common-mode transients propagate simultaneously via AC line and AC neutral towards the earth connection of the device, the differential mode is a path between AC neutral and AC line connections. Since 2nd edition of IEC61000-4-4:2006 only common mode tests are required.


A necessary step during product design is to decide on adequate protection in accordance with the environment where the product will be used. The IEC61000-4-4 standard defines four clear test levels depending on the area of use and type of connection to the device and lets product committees specify the additional fifth level of test requirements for special situations. The standard defines voltage peak values and repetition rates of each test level. By rule of thumb, the system designer should consider 2x higher voltage on power connections versus standard I/O signals, data, or control ports. All levels typically require 5 kHz or 100 kHz repetition rate and allow the product committee to determine which one is more relevant. In practice, both frequencies are being tested. The difference between test levels is based is determined by the installation area where the device should typically operate. For example, medical equipment, it is to expect a higher protection level for home healthcare products due to less power grid protection in residential areas compared to professional units for hospital use. Industrial or lighting equipment for outdoor use will also have the highest immunity requirements versus indoor office with good protection. Annex B of IEC61000-4-4 gives more detailed information to test levels with additional descriptions. The following are some examples:


o Level 1 Well protected: Computer room


o Level 2 protected: Control room in industrial environment


o Level 3 industrial: Industrial process environment


o Level 4 heavy industrial: Outdoor or area with undefined protection


o Level X: Special situations


Analyzing the power supply market, it is common to see open frame or board mount power supplies up to Level 3 protection and external power supplies with endurance to level 3 or 4 depending on the application. To remember, even if the datasheet of the power supply states to withstand EFT at high levels, this is in a standardized test conditions and not necessarily representing real life situations with different system characteristics. This will be discussed in the next chapter “System Design Considerations” more in detail.


### System Design Considerations


The Pass/Fail criteria for each test level are defined by IEC61000-4-4 where A (normal operation) is likely to pass, B (short interruption, self recovery) and C (Interruption, recovery by operator possible) might be acceptable and D (loss of functionality) is obviously to fail the test. It is critical to understand the effect of EFT Pass/Fail criteria on devices with digital data processing as those can create unwanted data strings on the communication path.


In such situations, a power supply generally will ignore short EFT events and continue to perform without noticing them. A powered device with low protection of the data communication might have modified data or clock string and the fault result/measurement/functionality will be overseen. It is up to the designer to prepare the test platform to ensure not only good protection against EFT at system level but also consider undefined states in the software that can be created. The best practice is to separate all power lines and cables in the system from data cables and traces. Shielding of AC power cables may also be necessary to decouple electric the electric field generated by the EFT from signal lines.


These measures reduce electromagnetic coupling of EFT from the power distribution network to sensitive digital I/O ports. The power supply is the first stage between the AC power network and the designed device to reduce the EFT impact. It can help reduce the impact of the EFT but may not block it enough. It is still advisable to have some protection on entry of PCB for instance against captured coupled electromagnetic noise on cables or capacitively bypassed through the power supply. In the appropriate layout of the filter on the input of the equipment may allow EFT spikes to find a path and “travel” on the board across the filter toward the earth or a return path making this protection negligible although it exists on schematics. So for example while transferring from schematics to layout, electrically many options would be correct and the CAD tool will show all valid connections, but the EFT spike will look always for the lowest impedance discharge path and so can easily bypass the filter. A correct layout example would force the EFT spike to “see” the filter before it reaches the rest of the device.


It is important to remember, especially if it’s a Class II device physically not connected to earth ground, the energy induced by EFT will discharge, for example via air or parasitic capacitance of the system due to the higher frequency content of the EFT. Some applications require sensors to be connected to humans’ body or use capacitive touch screens and so similar to ESD Human Body Model 100pF capacitive coupling may be present. Systems with enough surface can create a kind of plate capacitor while elevated over the earth and so build capacitance with respect to the earth.


Using equation Xc=1/wc=1/2Pifc to calculate the impedance for 100pF shows the impedance at 100kHz tested frequency already near 16kOhm, 100Ohm impedance would be at the same frequency with 16nF capacitance only. IEC61000-4-4 prescribes the insulation to use or elevation above the tested device and ground depending on the type of the system under test. The system designer shouldn’t forget to take into consideration the real use conditions where the system might have various conditions with respect to grounding and coupling capacitance to earth as this affects system stability across worldwide usage.


### EFT/B and SL Power Electronics Corp. Competences


With more than 60 years of experience in the power industry, SL Power offers OEMs superior performance with solutions that are optimized for thermal management, reliability, EMI/EMC, power density, and power efficiency. When it comes to pre-compliance tests, SL Power offers additional support from its Customer Experience Center (CEC) in Sharon, MA USA to collaborate during the system design or assist in troubleshooting performance or design issues during the system integration. Mechanical modifications within the system or for example, re-spin of a PCB layout more often risks the design cycle of a new product that has a very tight schedule to release the product to the market. The mission of the CEC and global field application team is to help customers during such critical integration phases, compliance to EFT and other EMC requirements is a good example of it. Working together with the CEC allows users to determine critical load or test conditions of the system and helps to build the entire system to desired protection level. The target of this approach is not only to pass compliance tests but also to build a more reliable and stable system as well as to build a certain level of confidence before going to a test house saving on unnecessary costs if the system would fail at the first run.
