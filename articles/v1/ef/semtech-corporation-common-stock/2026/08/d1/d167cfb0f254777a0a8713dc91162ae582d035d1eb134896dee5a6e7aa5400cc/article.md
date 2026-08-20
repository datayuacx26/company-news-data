---
schema_version: "1.0.0"
document_id: "d167cfb0f254777a0a8713dc91162ae582d035d1eb134896dee5a6e7aa5400cc"
company_key: "semtech-corporation-common-stock"
company: "Semtech Corporation"
source_id: "semtech-corporation-common-stock-rss-bc5ca864e78b"
canonical_url: "https://blog.semtech.com/ai-date-center-basics-what-is-linear-pluggable-optics-lpo"
published_at: "2026-08-04T17:00:00+00:00"
first_seen_at: "2026-08-04T17:56:53.038491+00:00"
fetched_at: "2026-08-04T18:43:13.169222+00:00"
content_hash: "sha256:40596e6b1238422a4d323e40988c67da3f75d55918facb8b9b093a0e713e66ac"
---

# AI Data Center Basics: What Is Linear Pluggable Optics (LPO)?

Linear pluggable optics (LPO) is one of the most significant shifts in data center optical interconnect design in a decade — but the term still causes confusion. Is it a module type? A standard? A replacement for digital signal processing function or DSP-based optics entirely? This post answers those questions from first principles.


## The one-sentence answer


LPO is a high-speed, optical transceiver that uses analog processing to move the equalization burden from the module back to the host switch SerDes, eliminating the DSP retimer, reducing power consumption and enabling lower-latency direct connectivity in Artificial Intelligence (AI) and[data center interconnec](https://www.semtech.com/products/signal-integrity/data-center)[t](https://www.semtech.com/products/signal-integrity/data-center) applications.


## How a conventional retimed optical link works


In a traditional DSP-retimed module, the host switch SerDes transmits an electrical signal that arrives at the module degraded by the electrical channel. A DSP inside the module retimes, reshapes and re-amplifies that signal to drive an optical modulator and generate an optical output. On the receive side, the signal is detected with a photodiode and converted to voltage using a transimpedance amplifier (TIA). A DSP then retimes, reshapes and re-amplifies the electrical signal to clean it up before delivery to the host SerDes.


That DSP is powerful. It compensates for significant signal degradation and reflections. But it consumes substantial power — typically 23 to 25 watts for a 1.6T DR-8 retimed module at current data center deployments.


## How LPO changes the architecture


LPO removes the DSP retimer from the module entirely. The module itself is substantially simpler. On the transmit side: a continuous time linear equalizer (CTLE) for input equalization, a driver and a modulator to generate the optical signal. On the receive side: a transimpedance amplifier (TIA) and potentially a linear equalizer. No DSP, no retiming, no clock recovery.


The host switch SerDes — which already performs equalization for its own purposes — takes on primary signal conditioning. The module handles analog amplification and optical conversion. Through careful optimization of equalization down the link, this analog architecture delivers the performance required for 200G AI data center interconnects.


## Why this matters: the power case


The power reduction is substantial. For 200G links: retimed modules consume approximately 23 to 25 watts today, expected to drop to around 20 watts with next-generation DSPs. LPO modules target approximately 10 watts — less than half. The intermediate approach, Retimed Transmitter Linear Receiver (RTLR), sometimes called Linear Receive Optics (LRO), sits at roughly 16 watts today.


Scale example:


At 512 ports (current Tomahawk-6 generation), LPO saves approximately 500W at the module level and up to 1kW system-level when cooling is included. At 1024 ports, those savings double.


## What LPO is not


LPO is not a passive module — it still contains active analog components. What it lacks is a DSP retimer with clock recovery. LPO is also not a universal replacement for retimed modules: high-loss electrical channels, significant reflection environments or deployments requiring maximum TXFIR flexibility may still favor RTLR or fully retimed approaches. The right choice depends on system architecture.


## The standards landscape


LPO is governed by an emerging set of standards. On the electrical side, the OIF CEI 224 linear specification targets 22 dB die-to-die port loss. On the optical side, the Institute of Electrical and Electronics Engineers (IEEE) sets requirements for 200G performance. The Linear Pluggable Optics Multi-Source Agreement (LPO MSA) Group — where[Semtech is a founding member](https://www.semtech.com/company/press/semtech-to-showcase-new-linear-pluggable-optical-links-for-ai-and-ml-data-center-interconnects-at-ofc-2024)


— combines both into a unified standard.


## Frequently asked questions


### Is LPO the same as an analog module?


Largely, yes. LPO modules use analog rather than digital signal processing. The key distinction from a passive module is that LPO still contains active analog components and requires configuration and calibration.


### Does LPO require new switch ASICs?


Not necessarily new ASICs, but the host SerDes must be correctly calibrated to deliver the equalization LPO modules expect. This is a configuration and methodology question as much as a hardware question.


### Is LPO ready for production?


Standards work is active and advancing. The calibration methodology and standards framework are sufficiently mature to support deployment planning today.


*Semtech® and the Semtech logo are registered trademarks or service marks of Semtech Corporation or its affiliates. Other product or service names mentioned herein may be the trademarks of their respective owners.*
