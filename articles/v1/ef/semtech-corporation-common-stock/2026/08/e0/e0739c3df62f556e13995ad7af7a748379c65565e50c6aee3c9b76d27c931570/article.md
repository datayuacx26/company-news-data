---
schema_version: "1.0.0"
document_id: "e0739c3df62f556e13995ad7af7a748379c65565e50c6aee3c9b76d27c931570"
company_key: "semtech-corporation-common-stock"
company: "Semtech Corporation"
source_id: "semtech-corporation-common-stock-rss-bc5ca864e78b"
canonical_url: "https://blog.semtech.com/200g-lpo-power-reach-and-loss-real-numbers"
published_at: "2026-08-18T17:00:00+00:00"
first_seen_at: "2026-08-18T20:52:01.956063+00:00"
fetched_at: "2026-08-18T20:52:04.251531+00:00"
content_hash: "sha256:e9742e37cb6c35ea4bd9547b7a763e3798d02ca01b7ba11f825e4bb68df06b82"
---

# 200G LPO Power, Reach and Loss: Real Numbers

Decision-makers evaluating linear pluggable optics (LPO) want numbers, not abstractions. If you're new to LPO, start with


[Blog 1: What Is Linear Pluggable Optics?](https://blog.semtech.com/ai-date-center-basics-what-is-linear-pluggable-optics-lpo) , then come back here for the key quantitative parameters for 200G LPO deployment: power consumption by module type, optical reach, electrical loss tolerance, and host port calibration requirements.


## 200G LPO Power Consumption vs. RTLR and Retimed Modules


The power story is the primary commercial driver for LPO adoption in modern data centers. For 200G links:


- Retimed Digital Signal Processor (DSP) modules: currently 23 to 25 watts per module; expected to reduce to approximately 20 watts with next-generation DSPs


- Retimer-based Linear Receive Optics (RTLR / LRO) (transmit-side retimer only): currently approximately 16 watts; expected to reduce to 14 to 15 watts


- LPO: targeting approximately 10 watts — well under half the current retimed figure


These are module-level figures. At the system level, cooling power scales roughly in proportion, so the total saving from switching to LPO is roughly double the module-level difference. On a 512-port switch built on the Tomahawk-6 generation, moving from retimed to LPO modules saves approximately 500W at the module level alone, climbing to nearly 1kW once cooling is factored in. Double the port count to 1,024, and those savings double as well.


## What Is the Optical Reach of 200G LPO?


For 200G LPO, the current optical reach target is 500 meters, corresponding to a 3 dB optical loss budget. This covers the vast majority of practical data center link lengths.


Extension beyond 500 meters is feasible under the right conditions. With appropriate receiver sensitivity and transmit power, reach could extend to approximately 2 kilometers in a DR (low-dispersion) context — roughly a 4dB optical budget. Current standards target the 3dB / 500-meter envelope, and exceeding it moves from a reach problem into a power budget problem: more transmit power or more sensitive receivers, not simply longer fiber.


## LPO Electrical Loss Budget: The Die-to-Die Tolerance


LPO currently targets 22dB die-to-die port loss — the electrical loss between the switch chip and the module die on both transmit and receive sides. Semtech is actively working to extend this to potentially 26 dBonce sufficient measurement capability confirms feasibility. For context, fully retimed modules operate with port losses up to 32dB,


so LPO's margin is noticeably tighter. That gap shapes board design directly: platform teams targeting LPO compatibility need to hold their die-to-die electrical channels within that 22dB budget. It's achievable in a well-designed printed circuit board (PCB) layout, but it leaves less margin than a retimed deployment allows.


## LPO Host Port Calibration: Requirements and Process


The goal is to ensure the host port delivers a correctly shaped electrical eye at the module input — right precursor and postcursor equalization matched to the CTLE in the module driver. The calibration procedure:


- Insert a host compliance board into the switch


- Set the required CTLE target for the module being deployed


- Apply an equalizer with tightly constrained taps and no precursors to isolate the calibration


- Measure output swing and verify correct eye shape


- Confirm electrical channel losses are accounted for in the equalization


Once calibration is correctly performed for a given switch platform and module type, the switch can be software-configured to apply the correct CTLE settings on module insertion — enabling plug-and-play operation without per-port or per-module manual tuning.


## What These 200G LPO Numbers Mean for System Design


- 22dB die-to-die electrical loss budget constrains PCB and channel design


- 500m / 3dB optical budget covers the vast majority of data center topologies


- ~10W module power enables aggressive port-density and cooling optimization


- Host port calibration is a one-time per-platform exercise, not a per-module overhead


## **Frequently** **A** sked ****


**Q** **uestions**


### Can LPO modules be used in existing switch platforms?


Yes, with calibration. The electrical channel must fall within the 22dB die-to-die loss budget and the host port calibration methodology must be applied. Platforms with generous loss margins for retimed modules may need channel assessment before LPO qualification.


### Is the 22dB loss limit likely to be raised?


Semtech and the Optical Internetworking Forum (OIF) are actively working to extend the target to 26dB. Designers should plan to the current 22dB standard while monitoring standards progress.


Learn more about the


****[Semtech LPO Products.](https://www.semtech.com/products/signal-integrity/lpo)
