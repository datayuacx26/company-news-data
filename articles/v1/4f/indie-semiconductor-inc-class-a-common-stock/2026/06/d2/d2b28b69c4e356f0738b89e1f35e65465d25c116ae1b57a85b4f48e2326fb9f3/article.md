---
schema_version: "1.0.0"
document_id: "d2b28b69c4e356f0738b89e1f35e65465d25c116ae1b57a85b4f48e2326fb9f3"
company_key: "indie-semiconductor-inc-class-a-common-stock"
company: "indie Semiconductor Inc."
source_id: "indie-semiconductor-inc-class-a-common-stock-rss-0cfdbd838f6e"
canonical_url: "https://www.indie.inc/blog/ultra-stable-semiconductor-laser/"
published_at: "2026-06-04T16:21:39+00:00"
first_seen_at: "2026-07-20T23:17:37.540760+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:e62cf6214df08c31317f018410d84dfc78b19b51400b74334a08119feef7070f"
---

# Ultra-Stable Semiconductor Laser

> *Simon Ayotte, Principal Scientist – Lasers and Photonics, indie’s Photonics BU*


The LXM is a compact semiconductor laser module exhibiting world-class linewidth and frequency noise performance, surpassing even high-end fiber lasers, as detailed in the white paper Rethinking Laser Performance:[Advancing Narrow-Linewidth Laser Characterization with PSDFN – indie](https://www.indie.inc/blog/rethinking-laser-performance-advancing-narrow-linewidth-laser-characterization-with-psdfn/) .


Figure 1. Semiconductor laser chip, butterfly package and LXM module.


For advanced applications such as distributed acoustic sensing (DAS), atomic reference locking, optical clocks, continuous-variable quantum key distribution (CV-QKD), radio frequency (RF) and terahertz generation, long-term wavelength (and frequency) stability is critical. In addition, the laser must maintain this stability across variations in ambient temperature. In this application note, we demonstrate that the LXM delivers exceptional stability both over time and under changing environmental conditions.


## Applications


In DAS systems, optical pulses are launched into a sensing fiber, and the Rayleigh backscatter is coherently detected to extract acoustic or vibration information. Because measurements rely on comparing successive acquisitions of this backscatter signature, any drift in the laser wavelength is indistinguishable from a change in the fiber response itself. This directly degrades the signal-to-noise ratio (SNR) and can limit sensitivity, particularly for low-frequency acoustic or vibration signals where long-term stability is critical. In distributed Rayleigh-based strain or temperature sensing, wavelength instability also translates directly into measurement error, limiting absolute accuracy. The LXM addresses these challenges through its excellent long-term wavelength stability and low frequency noise, ensuring a stable optical reference over time. This results in improved SNR, better low-frequency detection capability, and higher accuracy in distributed sensing measurements.


In CV-QKD systems, a local oscillator (LO) at the receiver enables coherent detection. Unlike conventional coherent communication systems, CV-QKD operates extremely close to the shot-noise limit, leaving essentially no margin for signal processing corrections. Any phase or frequency instability in the laser directly translates into excess noise, and, critically, this noise cannot be compensated digitally. As a result, laser instability directly reduces the achievable key rate and can ultimately compromise the security of the system. The LXM’s ultra-low frequency noise and excellent wavelength stability minimize phase errors at the source, ensuring that excess noise remains well below critical thresholds and enabling robust, high-performance quantum key distribution.


Laser wavelength stability is also critical for atomic reference locking. In these systems, the laser must be tightly locked to a specific atomic transition using a feedback loop. If the free-running laser exhibits significant drift or noise, the locking loop must compensate with higher bandwidth and gain, increasing complexity and reducing robustness. This can limit achievable stability and make the system more sensitive to environmental perturbations. By starting with an intrinsically stable, narrow-linewidth source such as the LXM, the lock is easier to acquire and maintain, requiring less correction and enabling higher spectral purity and better long-term accuracy.


Stable narrow-linewidth lasers are also essential for generating low-noise RF, microwave, and terahertz (THz) signals through optical techniques. When two lasers are beaten on a photodetector, any frequency noise or drift in the optical sources directly maps into phase noise on the generated RF signal, degrading its spectral purity and limiting system performance in applications such as radar, RF-over-fiber links, and high-frequency signal generation. The LXM’s narrow linewidth and low frequency noise enable the direct generation of low-phase-noise RF signals through optical heterodyning. Furthermore, the LXM supports high-performance optical phase-locked loops (OPLL), allowing two lasers to be tightly phase-locked with a controlled frequency offset. In this configuration, the generated RF signal inherits the stability of a low-noise RF reference, enabling ultra-low phase noise signal generation that is not achievable with free-running lasers alone. When combined with optical frequency comb technologies, these capabilities can be extended to even higher frequencies. By locking LXM lasers to a comb or using OPLL techniques in conjunction with comb stabilization, it is possible to generate ultra-low-noise and highly stable THz signals. This approach is particularly attractive for advanced applications such as high-resolution radar, THz communications, spectroscopy, and photonic frequency synthesis.


In all these approaches, system performance is critically determined by the linewidth, frequency noise, tunability, and environmental stability of the lasers. The LXM’s combination of ultra-narrow linewidth, low frequency noise, fast tuning response, and exceptional environmental stability enables superior system-level performance. Compared to high-end fiber lasers and external cavity lasers, the LXM delivers better stability with significantly improved robustness and compactness, making it a highly attractive solution for next-generation photonic systems.


## Wavelength stability


To evaluate wavelength stability, we implemented the simple heterodyne setup shown in the figure 2. A first laser operates in a free-running configuration at ambient temperature, while a second laser is mounted on a temperature-controlled metal plate using a thermoelectric cooler (TEC). The outputs of the two lasers are combined on a photodetector to generate a beat note, which is then analyzed using either a frequency counter or a spectrum analyzer. For all measurements presented here, two identical lasers were used (e.g., two LXMs or two high-end fiber lasers), ensuring a fair comparison of intrinsic performance.


Figure 2 : Wavelength stability test setup.


The figure below shows the beat frequency stability over a 10-hour acquisition period for both the LXM and a high-end fiber laser. The LXM exhibits a standard deviation of 0.74 MHz and a maximum peak-to-peak frequency deviation over 1h of 2.4 MHz, whereas the fiber laser shows a standard deviation of 5 MHz and a maximum peak-to-peak frequency deviation over 1h of 23 MHz. In addition, short term fluctuations of the fiber laser reach approximately 10 MHz peak-to peak within just a few minutes, significantly degrading measurement stability.


Figure 3. Beatnote stability versus time between two LXM and between two fiber lasers.


To further quantify this behavior, the frequency data were processed to extract the power spectral density (PSD) of frequency noise. The results show that the LXM consistently exhibits lower noise across all measured frequencies, with an improvement of up to two orders of magnitude in the low-frequency range between 0.001 Hz and 0.01 Hz. This low-frequency regime is particularly critical for applications such as DAS and CV-QKD, where long-term drift directly impacts system performance.


Figure 4. Power spectral density of frequency noise of the beat note between two LXMs compared with the beat note between two fiber lasers.


Using the same heterodyne configuration, a measurement of the time required for the LXM wavelength to stabilize once it is locked on its internal frequency discriminator is made. As shown in the figure below, the laser reaches a stable operating wavelength within approximately 7 seconds. At turn-on, it takes less than 60 seconds for the LXM to lock plus 7 seconds for its wavelength to stabilize. This fast stabilization time is a key advantage in practical systems, reducing initialization delays and enabling rapid deployment in field applications.


Figure 5. Wavelength versus time of the LXM after it locks to its internal frequency discriminator.


Finally, we investigated the sensitivity of the laser wavelength to environmental temperature variations. The TEC was used to apply a controlled temperature ramp to one of the lasers, and the resulting shift in beat frequency was recorded. As illustrated in the figure below, a 13 °C temperature decrease results in a total frequency shift of only 5 MHz peak-to-peak, corresponding to less than 0.04 pm in wavelength.


Figure 6. Wavelength versus time of the LXM after it locks to its internal frequency discriminator.


These results demonstrate that the LXM significantly outperforms conventional high-end fiber lasers in terms of long-term stability, turn-on time, and wavelength robustness to temperature variations. In addition, the LXM exhibits superior immunity to mechanical noise, acoustic disturbances, and vibration, as detailed in[MKT-APPNOTE-LXM-Laser-source-for-DAS-1.1_En.pdf](https://www.indie.inc/wp-content/uploads/2024/12/MKT-APPNOTE-LXM-Laser-source-for-DAS-1.1_En.pdf) .


## Wavelength tuning (digital & analog) and locking


For applications requiring the laser frequency to track or lock to another laser or to a specific spectral reference, the LXM provides two complementary tuning mechanisms.


First, digital control enables coarse frequency tuning with a resolution of 5 MHz and a frequency range of 50 GHz, allowing precise and repeatable wavelength positioning over a wide range. Second, an analog control input provides fine and high-speed frequency tuning with a sensitivity of approximately 40 kHz/mV over a maximum frequency range of 200 MHz.


Thanks to the intrinsically flat frequency response of the DFB laser chip and the high-bandwidth frequency discriminator locking loop (≈100 MHz), the analog tuning port exhibits a flat modulation response extending up to approximately 10 MHz, as shown in the figure below. This wide and flat response enables high-performance dynamic frequency control and fast feedback operation.


Figure 7. Radio frequency modulation (RFM) response of the LXM analog modulation port.


A key application of this capability is in optical phase-locked loops (OPLLs). For example, the analog tuning port can be used to lock the frequency of one LXM laser to another with a defined offset. As illustrated schematically in the figure below, the outputs of two free-running lasers are combined on a balanced photodetector, generating a noisy RF beat note. This signal is then mixed with a low-noise RF reference, producing an error signal proportional to the phase difference between the optical beat and the reference. This error signal is processed by a loop filter and fed back to one of the lasers via the analog tuning input, thereby closing the loop and enforcing phase and frequency locking at the desired offset.


Figure 8. Upper schematic shows two free-running LXM beating together and lower schematic shows two LXM locked to one another by an OPLL.


When operating in this regime, traditional metrics such as wavelength or frequency stability become secondary, as the optical beat frequency directly inherits the stability of the RF reference. Instead, performance is characterized in terms of residual phase noise, which quantifies the small phase deviations of the locked signal. Using two LXM modules, we demonstrate an OPLL with a loop bandwidth of approximately 10 MHz and an integrated phase noise (from 1 Hz to 20 GHz) below 0.1 radians.


Figure 9. Differential single side band phase noise (SSB PN) between the LXM lasers in native condition and in locked mode.


Furthermore, by co-packaging DFB laser chips within a common housing and leveraging silicon photonics for both passive and active optical functions, loop bandwidths up to 100 MHz have been achieved. These advanced results are reported in our Optica publication:[Silicon Photonics-Based Laser Modules for FM-CW LIDAR and RFOG](https://opg.optica.org/abstract.cfm?uri=OFS-2020-T3.98) .


## Conclusion


The[LXM](https://www.indie.inc/product/ultra-narrow-laser-module-lxm/) laser module combines exceptional intrinsic stability with advanced tuning and control capabilities, addressing the most demanding requirements in sensing, metrology, and quantum applications. Its superior long-term wavelength stability, rapid turn-on time, and minimal sensitivity to environmental temperature variations provide a clear performance advantage over conventional high-end fiber lasers.


These performance characteristics translate directly into tangible system-level benefits. In Distributed Acoustic Sensing (DAS), the LXM enables higher signal-to-noise ratio and improved low-frequency detection by minimizing wavelength drift. In CV-QKD systems, its ultra-low frequency noise ensures minimal excess noise at the receiver, supporting higher key rates and more robust security. For atomic reference locking, the LXM’s intrinsic stability significantly reduces the required loop bandwidth, simplifying system design while improving lock robustness and long-term accuracy. In photonic RF and THz generation, its narrow linewidth and low noise enable ultra-low phase noise signal synthesis, further enhanced through advanced locking techniques such as OPLLs.


In addition, the combination of high-resolution digital tuning and wideband analog control enables precise, high-speed frequency manipulation and robust locking architectures.


These capabilities allow the LXM to operate not only as a highly stable standalone source but also as a key building block in tightly synchronized photonic systems.


Together, these results position the LXM as a versatile and high-performance laser solution for applications including Distributed Acoustic Sensing, atomic reference systems, quantum key distribution, and photonic RF and THz signal generation.


[Request a quote](https://www.indie.inc/photonics/request-a-quote/)


## Related Products


Product Description


[Narrow DFB Laser Module – LXM](https://www.indie.inc/product/ultra-narrow-laser-module-lxm/)


Narrow linewidth, rugged, compact and highly stable laser module for challenging applications.
