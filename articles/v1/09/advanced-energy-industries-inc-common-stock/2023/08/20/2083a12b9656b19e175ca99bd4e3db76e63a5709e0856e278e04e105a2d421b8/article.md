---
schema_version: "1.0.0"
document_id: "2083a12b9656b19e175ca99bd4e3db76e63a5709e0856e278e04e105a2d421b8"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/revolutionizing-solid-organ-tumor-ablation-with-high-voltage-solutions/"
published_at: "2023-08-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:49a33b164f80cdc793c9e133f3879c487d9abb8b35fbfdf9dabb21dee06a2bb4"
---

# Revolutionizing Solid Organ Tumor Ablation with High Voltage Solutions

Pulsed field ablation (PFA) is a non-thermal energy modality that is being used to reduce and remove solid organ tumors. This technique applies a high voltage electrical field to cells to increase the permeability of the cell membrane, which leads to targeted cell death. While PFA has proven to be effective, researchers have recently developed a new type of irreversible electroporation (IRE) technique called high-frequency IRE (H-FIRE), which offers unique benefits.


There are two types of electroporation: reversible and irreversible. Reversible electroporation creates non-permanent pore formation where a low-intensity electric field is applied, permitting chemicals, drugs or DNA to enter the cell. Irreversible electroporation is the creation of permanent pores, leading to cellular homeostasis disruption and cell death. IRE has primary applications in cardiac and tumor ablation.


From an electrical engineering perspective, the difference between the two techniques is a function of the electrical field strength and the duration for which the field is applied. Established PFA approaches typically apply 80 to 120 unipolar pulses with a pulse duration of 50 to 100 ms and an electric field that exceeds 1,000 V/cm. While this can be effective, this protocol may cause muscle contraction, leading to pain for the patient and displacement of electrode needles.


In contrast, H-FIRE uses a set of bipolar pulse bursts consisting of individual pulses with durations from 0.5 to 10 ms, grouped into a burst of up to 100 ms. H-FIRE offers unique benefits, including selective targeting of cardiomyocytes while sparing collateral damage to the connective tissue structure.


Multiple studies have documented the application of IRE to liver tumors, with the earliest dating back to 2011. The applied voltage was in the range of 1.5-3 kV/cm with a duration between 70-100 µs, and the best results were obtained using plate electrodes to deliver 80 pulses of 100 ms at 0.3 Hz with an electrical field magnitude of 2.5 kV/cm across the tumor.


**PFA Power**


The primary challenge of power solution design for PFA systems is the need to deliver high energy HV (High Voltage) pulses reliably and repeatably. For life science applications, typically a low power system in the 2-300 W region is required. For surgical applications, individual pulses has an average power for the system in the kilowatt region.


The voltage required is typically in the 1 kV to 3 kV range with some applications requiring up to 5 kV with currents up to 65 A. Pulse widths are typically in the range 100 ns to 100 µs with burst mode frequencies up to 5 MHz. The associated slew rates for the pulse are both significant and challenging.


To meet these challenges, a power solution for a PFA application consists of multiple stages, converting from the AC mains input to the high-voltage pulsed DC output required for treatment. The first stage is the AC input and power factor correction (PFC), which addresses critical compliance parameters including primary-secondary isolation, leakage current, EMI filtering and PFC.


The second stage is the DC-DC converter. This stage will convert the input voltage of typically 380 VDC to the high voltage pulse output required for the application. The topology for this stage depends on the waveform requirements of the application. For example, a flyback converter may be suitable for unipolar pulse applications, while a forward converter may be better suited for bipolar pulse applications.


One technology suitable for this stage is Advanced Energy’s[UltraVolt® C series](https://www.advancedenergy.com/products/high-voltage-products/high-voltage-power-supplies/standard-size-dc-to-high-voltage-modules/high-power-c-series/) of high-power regulated DC-DC converters, which are optimized for laser, capacitor-charging and pulsed power applications. This series features 17 devices with voltages from 125 to 60,000 V and a typical ripple of less than 10,000 ppm. Available in single-output positive or negative polarities, the converters deliver up to 250 W of fast-rise charging power with less than 1% overshoot from a 24 VDC input. In addition, several analog interfaces enable control and monitoring of the high-voltage output and current.


The third stage is the output filter. This stage will attenuate any residual switching noise or ripple on the high voltage output before it is delivered to the load. The filter topology will be dependent on the application requirements, with options including low-pass filters, Pi filters and T filters.


In addition to the power supply, other components of the PFA system include the pulse generator, electrode needles and control software. The pulse generator creates the high voltage pulses that are delivered to the tissue through the electrode needles, while the control software allows the operator to configure the waveform parameters and monitor the treatment progress.


**Conclusion**


Pulsed field ablation is a promising technique for solid organ tumor ablation and cardiac ablation, with the potential to deliver precise, non-thermal energy to target tissues while minimizing collateral damage to surrounding tissue. The power supply is a critical component of the PFA system, requiring the reliable and repeatable delivery of high voltage pulses with the required waveform parameters. The right power supply design ensures that PFA systems provide safe, effective treatment for a range of medical applications.


With the broadest AC/DC, DC/DC, and high voltage standard and modified products and platforms, coupled with extensive applications and compliance experience, Advanced Energy offers systems designers an unrivalled range of proven power solutions for PFA power supply design.
