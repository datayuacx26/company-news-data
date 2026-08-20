---
schema_version: "1.0.0"
document_id: "f1c16bd9669485782df051b9ddbb06f85c25b2758f7eee57f04fdabe2e660195"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/engineering-signals-for-the-next-generation-of-medical-devices/"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:d421928b7afa865ee483d61a30dfd4f4a5965e7c0597b082a64e95f19305fb5c"
---

# Engineering Signals for the Next Generation of Medical Devices

[Home](https://www.advancedenergy.com/en-us/) /


[About AE](https://www.advancedenergy.com/en-us/about/) /


[News & Events](https://www.advancedenergy.com/en-us/about/news/) /


[Blog](https://www.advancedenergy.com/en-us/about/news/blog/) /


Engineering Signals for the Next Generation of Medical Devices


# Engineering Signals for the Next Generation of Medical Devices


### Posted


March 04, 2026 by


[Todd Huston](https://www.advancedenergy.com/en-us/about/news/authors/todd-huston/)


BLOG SUMMARY


• Medical devices are becoming more energy intensive, compute heavy, image guided, and system integrated, driving original equipment manufacturer (OEM) engineering teams toward tighter electromagnetic compatibility (EMC) performance, higher isolation, stable multi rail architectures, and designs optimized for artificial intelligence (AI), robotics and dense multi modal workflows.


• Energy based therapies across RF, microwave and emerging non thermal modalities (PFA/IRE, histotripsy, electrochemotherapy) are rapidly advancing, each placing new demands on power electronics, including ultra stable rails, high voltage pulse capability, low leakage designs, and robust thermal and EMC management.


• Next generation interventional oncology and electrophysiology suites now operate as interconnected platforms, combining imaging, navigation, compute, and therapy systems. This elevates the power subsystem from a background component to a core enabler of clinical precision, regulatory compliance and overall system reliability.


---


The Society of Interventional Oncology (SIO) Annual Scientific Meeting, the 31st Annual AF Symposium, and MD&M West, all held the first week of February 2026, collectively offered a clear forward look at the engineering needs shaping next‑generation medical devices. These conferences illustrate a consistent trend across interventional oncology, electrophysiology and surgical device development. What the trend indicates is that modern medical care is rapidly becoming more energy‑intensive, more compute‑dependent, more image‑guided, and more system‑integrated. This evolution carries direct consequences for original equipment manufacturer (OEM) engineering teams, especially those working in embedded power design, system architecture, high‑voltage energy modalities, isolation and leakage control, robotics, and integrated imaging workflows.


Interventional Oncology as a Multi‑Modal, Multi‑System Platform


SIO 2026 highlighted the shift toward interventional oncology suites functioning as dense networks of interoperable systems rather than isolated devices. Workflows now frequently integrate ablation, embolization, intratumoral delivery, and systemic therapies, all supported by tumor boards, planning and confirmation workshops, and technology‑enabled clinical decision support.


Typical Interventional Oncology (IO) suites include multi‑modality imaging consoles (ultrasound, CT, MRI integration); RF, microwave, pulsed field ablation (PFA)/ irreversible electroporation (IRE), histotripsy, and electrochemotherapy platforms; artificial intelligence (AI)‑assisted planning workstations; robotics and navigation systems; and pumps, infusion systems and drug‑delivery catheters.


Such an environment requires power systems designed for electrically dense and sensitive co‑location, including:


- Strict electromagnetic compatibility (EMC) performance to prevent interference between imaging, therapy and compute devices,
- Low‑leakage, high‑isolation designs (2×Means of Patient Protection, or MOPP) for patient‑adjacent electronics,
- Stable rails for sensors, compute and motion systems, and
- Surge‑tolerant architectures, especially where electrosurgical or ablation generators operate near other devices.


RF Ablation: Precision Current Requires Precision Power Electronics


RF ablation remains a cornerstone therapy across liver, kidney, lung, bone, and thyroid applications. This year’s sessions presented advances in impedance‑tracking, predictive thermal modeling, and imaging‑based margin confirmation, each of which demands exceptionally stable power delivery.


RF platforms are increasingly integrated with real‑time impedance feedback loops, AI‑supported prediction of ablation zones, robotic catheter placement and navigation, and imaging workflows that demand clean EMC behavior.


RF systems are acutely sensitive to noise, ripple and rail instability. Any disturbance directly affects waveform integrity and tissue heating dynamics. RF system requirements include:


- Ultra‑stable, low‑noise DC rails for RF power amplifier stages,
- EMC behavior that does not perturb ultrasound, CT, or fluoroscopy,
- 2×MOPP isolation between high‑energy patient circuits and logic domains,
- Fast‑response power rails supporting impedance‑controlled modulation, and
- Thermal management strategies protecting closed‑loop controllers.


RF remains one of the most demanding modalities from a power quality perspective. Even small deviations can propagate into clinically significant heating errors.


Microwave Ablation: Higher Frequency, Higher Power, Higher Electrical Demands


Microwave ablation continues gaining momentum due to its reliability in perfused organs and its ability to produce large, predictable ablation zones. It is now a primary modality for liver ablation in the U.S., with expanding use in lung and other soft tissues. Current system trends include 100 to 200+ watt generator designs, multi‑applicator outputs for simultaneous ablation, closed‑loop monitoring of reflected power and thermal data, and integrated imaging for margin assessment.


Microwave generators push embedded power systems hard. High‑power amplifier rails, often 48 V, require and excellent transient response. Strong EMC containment is needed to limit radiated and conducted emissions. High‑isolation boundaries help to protect patients from leakage currents. Thermal robustness sustains high‑power operation, while rail stability preserves phase, frequency and power delivery accuracy.


As power levels increase, the quality of the internal power architecture becomes directly tied to generator reliability and clinical performance.


Rapid Growth of Non‑Thermal Energy Modalities


New non‑thermal techniques, such as histotripsy, PFA/IRE, and electrochemotherapy, received extensive attention across SIO and the AF Symposium. Presenters emphasized that regulatory reviews and device approvals in these categories are accelerating.


- Histotripsy relies on high‑amplitude ultrasound to produce mechanical tissue destruction with minimal thermal spread.
- PFA/IRE uses high‑voltage pulses to create permanent membrane pores, selectively destroying cancer cells while sparing structures like nerves and vessels.
- Electrochemotherapy pairs electroporation with localized chemotherapy to improve intracellular uptake and reduce systemic exposure.


The AF Symposium provided valuable cross‑disciplinary evidence relevant to IO that support non‑thermal energy. First, multi‑year data demonstrated durable clinical outcomes for pulsed‑energy ablation. Second, zero‑fluoroscopy workflows showed how integrated mapping and imaging reduce procedural burden. Finally, new waveform designs achieved near‑instantaneous lesions with sub‑second high voltage (HV) pulses.


Based on this evidence, there are several implications for IO. Oncology clinicians will expect similar long‑term durability from non‑thermal tumor therapies. In addition, integration of mapping, imaging and therapy will redefine IO workflows, while sub‑second HV pulses will aid procedures where anatomical motion limits catheter stability.


In terms of engineering requirements, non‑thermal HV systems require Kilovolt‑capable modules with tightly regulated rise times, reinforced insulation and 2×MOPP‑compliant HV sections, strategies to minimize parasitic capacitance and pulse distortion, and thoughtful printed circuit board (PCB) layout, creepage and clearance management, as well as derating to meet the IEC 60601‑1 safety standard.


The conference proceedings also discussed four implications for medical-grade power supplies. First is safety and isolation as competitive differentiators. More patient‑adjacent sensors and electrodes increase the need for ultra‑low leakage and enhanced isolation. Second, EMC requirements are also tightening. Devices must function in electrically crowded environments without affecting imaging, navigation, or AI systems. Third, there are specialized high‑voltage and high‑power needs. RF and microwave (MW) systems require hundreds of watts, while non‑thermal platforms require fast‑rise, multi‑kilovolt pulses. Finally, the relevance of compute‑optimized multi-rail architectures. AI, 4K imaging, and sensor‑rich workflows need efficient, low‑noise power supporting GPUs, FPGAs, and real‑time controllers.


Conclusion


These early‑2026 conferences collectively point to a future defined by energy‑based therapies, robust imaging integration, and compute‑heavy decision support. For OEM engineering teams, the direction is clear, greater electrical complexity across RF/MW and HV pulsed modalities, more sensors, feedback loops and motion systems demanding stable isolated power, higher compute loads requiring clean multi‑rail architectures, and increasingly strict safety, EMC and reliability expectations. In this landscape, the power subsystem is no longer a background component. It is a core enabling technology governing clinical precision, regulatory viability and overall system performance.


To learn more about medical power supplies at Advanced Energy and how AE can address some of these trends, visit:[Medical Power Supplies | Advanced Energy.](https://www.advancedenergy.com/en-us/applications/medical/)


Share


### Todd Huston


Advanced Energy


As Advanced Energy’s Director of Strategic Marketing for Electrosurgery, Todd Huston develops strategic marketing plans for the company’s broad medical power portfolio of standard and configurable products to power the future of medicine. He is a senior technology and marketing professional with deep knowledge of the global healthcare industry as well as a passionate strategist with a proven track record leading teams in commercializing disruptive technologies at two Fortune 500 Companies. Todd previously served as an electrosurgery product manager for the Tumor Ablation portfolio at a leading medical device company.


[More posts by Todd Huston](https://www.advancedenergy.com/en-us/about/news/authors/todd-huston/)


Browse


[Categories A-Z](https://www.advancedenergy.com/en-us/about/news/categories/)


Latest from AE


[News & Press Releases](https://www.advancedenergy.com/en-us/about/news/press/)[Videos](https://www.advancedenergy.com/en-us/about/news/videos/)[Trade Shows & Conferences](https://www.advancedenergy.com/en-us/about/news/events/)[Webinars](https://www.advancedenergy.com/en-us/about/news/webinars/)


Join Our Mailing List


Subscribe


Recent Posts


[View on X](https://twitter.com/advenergy)
