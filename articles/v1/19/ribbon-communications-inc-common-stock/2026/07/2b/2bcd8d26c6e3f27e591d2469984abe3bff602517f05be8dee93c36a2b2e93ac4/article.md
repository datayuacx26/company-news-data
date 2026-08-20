---
schema_version: "1.0.0"
document_id: "2bcd8d26c6e3f27e591d2469984abe3bff602517f05be8dee93c36a2b2e93ac4"
company_key: "ribbon-communications-inc-common-stock"
company: "Ribbon Communications Inc."
source_id: "ribbon-communications-inc-common-stock-news-import-9103da99152d"
canonical_url: "https://ribboncommunications.com/company/media-center/blog/maximizing-subsea-capacity-programmable-140gbaud-transmission"
published_at: null
first_seen_at: "2026-07-22T11:51:43.108752+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:a6a87a0ae93488aa9470960085b488118d6a56c2774dc6d1098ff64fde031a87"
---

# Maximizing Subsea Capacity with Programmable 140Gbaud Transmission

### Maximizing Capacity Over Speed


While the race to expand networking infrastructure to support AI applications continues on land, the same expansion is occurring undersea. Subsea cables spanning Earth’s oceans must evolve to keep pace.


In November 2025, Ribbon Communications[announced](https://ribboncommunications.com/company/media-center/press-releases/seren-juno-and-ribbon-achieve-20tbps-transmission-10000-km-juno-trans-pacific-cable) the successful optical transmission of an impressive 20Tbps over the 10,000km JUNO trans-Pacific cable. This field trial was performed in conjunction with Seren Juno, the cable operator. The JUNO cable system, which connects Japan and the United States, is currently the largest capacity operational trans-Pacific cable system. The cable is designed to handle a total of 360Tbps, consisting of leading-edge space division multiplexing (SDM) technology utilizing 20 fiber pairs. 20Tbps is also the capacity that Ribbon is delivering to Moratelindo over its 1,055km Jakarta-Singapore link, as was also[announced](https://ribboncommunications.com/company/media-center/press-releases/moratelindo-selects-ribbon-enhanced-submarine-network-capabilities) in 2025. Notably, actual capacities surpassed 20Tbps; however, the reported values include a margin for safety and reliability.


These achievements were enabled by Ribbon’s Apollo optical networking system, which includes transponder cards powered by[Acacia’s CIM 8 pluggable transceiver](https://acacia-inc.com/product/coherent-interconnect-module-8/) , featuring advanced 140GBaud coherent technology. These transponders support flexible modulation ranging from 400Gbps to 1.2Tbps per wavelength, designed to optimize performance tailored to the specific characteristics of each cable.


An interesting aspect of the headlines is that they emphasize a 20Tbps capacity rather than a maximum line rate, which is typical of optical transmission announcements. This distinction arises from the unique nature of submarine optical transmission. In terrestrial applications, the line rate serves as the principal metric because landline networks focus on optimizing speed and can augment capacity by adding more fibers—a process that is relatively straightforward.


Subsea applications are constrained by the limited number of fibers available within submarine cables, and installing these cables is extremely expensive. While line rate remains relevant, overall fiber capacity is the primary metric. The goal is to maximize capacity with the best economics.


## Framework for Maximizing Capacity


Below we discuss how Ribbon targets maximum capacity using Acacia’s CIM 8 programmable 140Gbaud transceiver. Its advanced Jannu DSP technology leverages multiple capabilities to cost-effectively maximize subsea fiber capacity, for any distance and fiber condition.


Figure 1 illustrates the framework for maximizing subsea fiber transmission capacity. This is based on two components: maximizing spectral efficiency and mitigating the effects of transmission impairments.


***Figure 1 – Framework for maximizing submarine fiber transmission capacity***


## Maximizing Spectral Efficiency


Spectral efficiency refers to how efficiently a given allocation of spectral bandwidth is used to transmit data reliably, within acceptable BER (bit error rate) limits.


In subsea data transmission, a technique to enhance spectral efficiency is to eliminate the common practice of assigning each wavelength to its own channel, where a guard band exists between the wavelength and the channel edges. This protective spacing is needed in terrestrial networks to prevent wavelengths from interfering with each other when they pass through multiple nodes and are switched with wavelengths from other fibers. However, subsea transmission is essentially one long point-to-point connection. The coherent wavelengths can be placed close to each other in a single multi-carrier channel, allowing more wavelengths to fit within an assigned wide spectrum window, increasing spectral efficiency.


***Figure 2 – Examples of multi-carrier submarine channels with inferior and excellent spectral efficiency***


Still, there are degrees of how well this can be performed. Figure 2 shows examples of submarine single multi-carrier channels comparing inferior and excellent spectral efficiency. Inferior spectral efficiency is characterized by wavelengths with rounded roll offs and poor use of their allocated spectral width. In contrast, excellent spectral efficiency features wavelengths with sharp spectral roll offs that nearly occupy all of their assigned spectrum, leaving almost no unused “white space” in the multi-carrier channel. The extra energy in a sharper-shaped spectrum means a higher signal to noise ratio, which can translate to more data being transmitted (higher modulation order) with the same BER.


The CIM 8 transceiver delivers excellent spectral efficiency. It uses second generation 3D Shaping technology to optimize line rate and reach for network operators with up to 20% higher spectral efficiency over the previous generation. It achieves this by combining programmatic controls for continuous baud rate, continuous modulation with enhanced probability constellation shaping (PCS) algorithms, and spectral roll-off shaping.


As shown in Figure 3, CIM 8 shapes the wavelengths to maximize spectral efficiency. They are tightly packed together with a sharp roll off and fill their spectral slots almost completely.


***Figure 3 – CIM 8 with Gen2 3D Shaping delivers excellent spectral efficiency***


## Mitigating Transmission Impairments


Although practical maximum spectral efficiency can be achieved, transmission impairments inherent to coherent optical transmission can still degrade the bit error rate (BER) on subsea links. CIM 8 employs a suite of proprietary, power-efficient algorithms designed to address all primary sources of transmission impairment, including:


- Dispersion and polarization related effects, including chromatic dispersion, polarization mode dispersion, and polarization dependent loss
- Non-linear effects
- Equalization-enhanced phase noise (EEPN)


By maximizing practical spectral efficiency and addressing transmission impairments, CIM 8 delivers practical maximum submarine fiber capacity.


## A Word About Baud Rate


As we just discussed, the CIM 8 with a continuous baud rate up to 140Gbaud delivers outstanding submarine transmission and capacity performance. In comparison with other high baud rate solutions, there are two points worth noting:


1. **A higher baud rate does not immediately translate to better spectral efficiency.** Whether the transceiver is at 140Gbaud or 200Gbaud, this class of transceivers operates at the edge of the Shannon limit. This means that although 200Gbaud supports higher capacity per wavelength, it necessitates a proportional increase in spectrum allocation to achieve this. Therefore, there is no improvement in spectral efficiency.
2. **A higher baud rate does not inherently offer lower costs.** Commercially available technologies in the 200Gbaud range rely on indium phosphide and thin film lithium niobate technologies, a relatively expensive solution due to not being in a mature high-volume production state. These higher rate technologies may use multiple packages that are connected via electrical flex cable interfaces, which typically present lower reliability.


In contrast, CIM 8 140Gbaud utilizes mature silicon photonics technology similar to that used in high-volume ZR/ZR+ optics, allowing it to leverage economies of scale. Additionally, CIM 8 integrates its DSP, PIC, driver, and TIA into a single, high-reliability opto-electronic package manufactured with high-volume CMOS technology.


Capacity optimizing solutions such as Ribbon’s Apollo optical networking system have been proven to be very effective for subsea applications. Leveraging the widely deployed Acacia CIM 8 module, network operators are able to leverage mature 140Gbaud technology and advanced coherent DSP features to maximize fiber capacity while mitigating the real-world impairments of subsea fiberoptic transmission. These solutions play a large role in the expansion of available bandwidth under the sea.


Please click on these links for more information on[Ribbon’s subsea optical solution](https://ribboncommunications.com/products/service-provider-products/apollo-optical-networking/max-capacity-140gbaud-submarine-transport) and[Acacia’s CIM 8](https://acacia-inc.com/product/coherent-interconnect-module-8/) .
