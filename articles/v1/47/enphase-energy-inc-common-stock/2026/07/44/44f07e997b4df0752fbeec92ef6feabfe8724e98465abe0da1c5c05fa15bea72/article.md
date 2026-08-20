---
schema_version: "1.0.0"
document_id: "44f07e997b4df0752fbeec92ef6feabfe8724e98465abe0da1c5c05fa15bea72"
company_key: "enphase-energy-inc-common-stock"
company: "Enphase Energy Inc."
source_id: "enphase-energy-inc-common-stock-rss-346818b8220d"
canonical_url: "https://investor.enphase.com/news-releases/news-release-details/enphase-energy-publishes-white-paper-kestrel-asic-its-fifth"
published_at: "2026-07-27T12:00:00+00:00"
first_seen_at: "2026-07-27T12:29:31.016826+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:b19b1930d1b57dbc2e7da9ceec7062d073f2094e63bc4ad7008981ce1a7f413e"
---

# Enphase Energy Publishes White Paper on Kestrel ASIC, Its Fifth-Generation Silicon Platform for Intelligent Power Conversion

**Purpose-built chip integrates control, sensing, communications, and security to enable high-frequency GaN power conversion and solid-state transformer applications for data centers**


FREMONT, Calif., July 27, 2026 (GLOBE NEWSWIRE) --[Enphase Energy, Inc.](https://www.globenewswire.com/Tracker?data=QzbOqSuHtltoLULA1HcghPAudmkTPdgTwqcKsop-1vspDBs_RThe4TwIgTU_nTGLXcA0ODmXYgnrBTnh_5NiTWozlsHraA2ydv-ooZhJSvU=) (NASDAQ: ENPH), a global energy technology company, today announced the publication of a new technical white paper, "The Enphase Kestrel ASIC: A Purpose-Built Platform for Intelligent Power Conversion." The white paper is available[here](https://www.globenewswire.com/Tracker?data=bffU-_402STDk-DcxXxu0D1sjrWEkCHUD2tvYa9TW7kHDGu7q2ZXR_ByVzza1NOwHeSeYfKMGS-Q2m3w_FGhT_u5Fjc50Ej84T4OvxICkEapb8it0jwIey1iVreumwRF) .


Kestrel is Enphase's internally developed application-specific integrated circuit (ASIC) for intelligent power conversion and the company's fifth generation of custom silicon. The paper describes how the chip extends Enphase's custom silicon investment across microinverters, next-generation IQ® Battery systems, the IQ® Bidirectional EV Charger, and the Enphase[IQ](https://www.globenewswire.com/Tracker?data=AcUYI3AEZaEgumzGTFJ5HB2fxNc90ZTE2PzAi1end4P9LXpucEdwwu30WaSOCE20frdl34dWm75hyk2Skgtx9IpSDPdlhixKjUxtIfoHnr0=)[®](https://www.globenewswire.com/Tracker?data=mbSWLeiZqrw921w_0LqEib06Zyx9yg4ZK-2aCrLpRvZt5om9nwSpiExYRXWUyG2FgQT7krOfP259LbaZrwQQ5pfFvOnArgFv941OG4S5VLQ=)[Solid-State Transformer (IQ SST)](https://www.globenewswire.com/Tracker?data=6RhmCGWzIhD21Eo3dkMTuxVTJ7kn83xNHbXCb3npkTc8AXhyic9Ibq7RG_UZusBxPNYKsBBYkj6a14ZYvwRBem8CQI63vZHDIRKCdduD_ShxlUrmE-cLhWzfksTadBdRDj41SnAOf4vMqSiXXaM7jA==) .


Fabricated using a mature 22 nm complementary metal-oxide-semiconductor process, Kestrel brings Enphase's power conversion control, sensing, communications, protection, security, and safety functions into one silicon platform. The chip integrates deterministic high-speed control hardware, four 100 Msps analog-to-digital converters (ADCs), custom power line communication and low voltage differential signaling (LVDS) controllers, hardware-enforced isolation, cryptography, secure boot, and functional safety features. By moving these capabilities into silicon, Kestrel is designed to reduce system complexity while preserving the speed, cost structure, and field reliability required for grid-connected power electronics.


"Kestrel is an important milestone because it turns Enphase’s custom silicon into a reusable platform for multiple product families," said Raghu Belur, co-founder and chief product officer of Enphase Energy. "The white paper shows how one ASIC can help us carry proven power conversion intelligence across microinverters, batteries, bidirectional EV charging, and solid-state transformer modules while supporting the reliability, security, and cost structure required for products deployed at grid scale."


The paper highlights Kestrel's role in enabling high-frequency operation with gallium nitride (GaN) bi-directional switch technology. Enphase's single-stage, dual-active-bridge, series-resonant cycloconverter topology requires cycle-by-cycle knowledge of resonant tank current and voltage, with gate transitions timed at hundreds of kilohertz and nanosecond-scale precision. Kestrel moves this control loop out of firmware and into deterministic custom hardware connected to the ADC outputs and gate-drive command infrastructure, supporting closed-loop control bandwidth exceeding 100 kHz while maintaining soft switching and low switching losses.


For the IQ SST power module, Kestrel supports a tandem control architecture across a medium-voltage isolation boundary. Two Kestrel ASICs – a controller on the low-voltage side and a device on the medium-voltage side – exchange time-critical measurements, FET control information, shutdown requests, and remote chip access over fiber-optic links driven by Kestrel's custom LVDS controller. This approach eliminates an external processor or flash device on the medium-voltage board.


The paper connects this architecture to a key AI data center challenge: GPU rack loads that can swing from low to high utilization multiple times per second. Kestrel's hardware power controller and tandem SST architecture are designed to support sub-millisecond transient response, native three-phase operation, and high-frequency soft-switching conversion between medium-voltage AC distribution and low-voltage DC buses. By responding in less than 1 ms, the IQ SST architecture is intended to eliminate local super-capacitor or battery energy buffering near compute racks.


"Kestrel was designed because the control problem cannot be solved well enough with a commodity microcontroller," said Hans van Antwerpen, chief technology officer of Enphase Energy. "The ASIC integrates the high-speed ADCs, deterministic power-control hardware, protection units, security engines, and tandem fiber-link functions needed to control GaN-based resonant converters safely, securely, and at very high frequency. That is what allows the same silicon foundation to support microinverters and also scale into solid-state transformer applications for dynamic data center loads."


The white paper also covers Kestrel's security and functional safety architecture, including processor islands, cryptography accelerators, identity-based access control, ROM-rooted secure boot, hardware AES and SHA acceleration, true random number generation, certificate-based public key infrastructure support, lifecycle management, single-bit error correction, double-bit error detection protection, DMA memory scrubbing, watchdog timers, and centralized fault handling.


To read the white paper, please visit the Enphase[website](https://www.globenewswire.com/Tracker?data=SgYFXzuIvlWQ55smaZ0cwICNau7l-cy-1UM2Qzq1XNv48JRR0Ac4gwY3eQvwvEqSn4s52I-nILH5Y-YoRn4i4hoY8X2sV9MgEhkXJ5OObXXoSrczPhwCog1qMX8uwGHTTJRdkStixjoFcLmiu6MTpA==) .


**About Enphase Energy, Inc.**


Enphase Energy, a global energy technology company based in Fremont, CA, is the world's leading supplier of microinverter-based solar and battery systems, EV chargers, home energy management systems, and virtual power plant (VPP) solutions. Enphase products enable people to harness the sun to make, use, save, and sell their own power, all controlled through the Enphase App. The company revolutionized the solar industry with its microinverter-based technology and has shipped approximately 87.8 million microinverters, with more than 5.2 million Enphase-based systems deployed in over 165 countries. For more information, visit[https://enphase.com/](https://www.globenewswire.com/Tracker?data=kNGO6zp2m3yHtGg-LQ6lFuCQeGg1mvKlkjLymiLdYgnhmKFAvPWypjUJ5IHnU4w8aPUFPxl6g3i_QPCBhvXYe1bTWRddEbN2Mzlx5QFPeFo=) .


©2026 Enphase Energy, Inc. All rights reserved. Enphase Energy, Enphase, the “e” logo, IQ, and certain other marks listed at[https://enphase.com/trademark-usage-guidelines](https://www.globenewswire.com/Tracker?data=kNGO6zp2m3yHtGg-LQ6lFtYm-msyhajDyvk7PeEx2hRqp2qP_nJSo2FpjjEEczbxWB2FyPvyBVNFqozS8olMOT4-6O5jv91jm5dTSLaWbiu--7iQURYHc3rrT7VM0inp5QKjbnzec7Zghs6AKOHQhWlPK2L0GQIS9BDYlBB1CyY=) are trademarks or service marks of Enphase Energy, Inc. Other names are for informational purposes and may be trademarks of their respective owners.


**Forward-Looking Statements**


This press release may contain forward-looking statements, including statements related to the expected capabilities, features, performance, efficiency, reliability, security, safety, cost advantages, architecture, functionality, and benefits of the Kestrel custom ASIC platform in Enphase products; the expected use of Kestrel in various Enphase products. These forward-looking statements are based on Enphase Energy’s current expectations and assumptions and inherently involve significant risks and uncertainties. Actual results and the timing of events could differ materially from those contemplated by these forward-looking statements as a result of such risks and uncertainties. Such risks include, but are not limited to, technological development and validation risks; semiconductor supply availability and qualification timing; the ability to achieve targeted performance, efficiency, reliability, security, safety, voltage, and cost metrics at scale; execution risks related to new product development and new market entry; and other factors discussed in Enphase Energy’s filings with the Securities and Exchange Commission, including those risks described in more detail in Enphase Energy’s most recently filed Annual Report on Form 10-K and Quarterly Report on Form 10-Q, and other filings made from time to time with the Securities and Exchange Commission. Enphase Energy undertakes no duty or obligation to update any forward-looking statements contained in this release as a result of new information, future events, or changes in its expectations, except as required by law.


**Contact:**


Enphase Energy
[press@enphaseenergy.com](https://www.globenewswire.com/Tracker?data=QrcHwzUhnfCcv7zN3CYcMRUuKhXGtgEvweCWxnLUHmS3OeVM5r3fWbFrUggYkQO0IA46qTVfHocUGWsoMdUt4GG625v7smzyFa0-XWA6F5Mbb-yTCAq4RQTpURh4DjVl)
