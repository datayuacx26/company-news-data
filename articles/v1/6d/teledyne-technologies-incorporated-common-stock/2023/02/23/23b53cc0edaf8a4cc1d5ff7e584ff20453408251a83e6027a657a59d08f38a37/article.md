---
schema_version: "1.0.0"
document_id: "23b53cc0edaf8a4cc1d5ff7e584ff20453408251a83e6027a657a59d08f38a37"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2023/02/new-pcie-60-compliance-pattern.html"
published_at: "2023-02-06T13:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:f25592603813d13cb862f00032d25e17560e95fd0210eca3a4aa9c5d7a08fad2"
---

# New PCIe 6.0 Compliance Pattern Measurements

PCI Express® 6.0 features significant changes from PCIe® 5.0. In particular, PCIe 6.0 achieves its 64-GT/s data rate, double that of PCIe 5.0, by moving from non-return-to-zero (NRZ) signaling to four-level pulse-amplitude-modulation (PAM4) signaling. Consequently, PCIe 6.0 requires some new test methodologies and patterns, including a new PAM4 Compliance Pattern that finds use in multiple measurements.


Figure 1. The new PCIe 6.0 Compliance Pattern signal. Click any image to enlarge.


The new Compliance Pattern is used for calculating signal to noise and distortion ratio (SNDR), as well as ps21TX (the package insertion loss) and the transmitter ratio of level mismatch (RLM). In addition, it is used to measure transmitter equalization coefficients.


### What's in the Compliance Pattern Signal


The new Compliance Pattern signal (shown in Figure 1 above) contains low-frequency patterns, such as strings of 64 0s, 1s, 2s and 3s, enabling a long run of each of the voltage levels found in PAM4. It also contains a big section of pseudorandom binary sequence (PRBS) data using PAM4.


- Section 1 contains a long run of 2s and 1s.
- Section 2 is the big section containing PRBS data.
- Section 3 contains a long run of 3s and 0s followed by a clock pattern.
- Section 4 contains another long run of 3s and 0s.


More PRBS data follows section 4, and the Compliance Pattern signal repeats itself. One or more of these sections in the Compliance Pattern signal can assist in the calculation of SNDR and other parameters.


### SNDR Measurement


Figure 2. SNDR equation variables.


SNDR refers to the Signal to Noise and Distortion Ratio. Figure 2 shows the breakdown of the SDNR equation in terms of the variables P max


(the signal strength), 𝜎


n


(noise) and 𝜎


e


(distortion).


Figure 3. P


max


measurement.


The P max


component of SNDR is calculated by using a pulse response, which is an extracted linear model of the transmitter signal. The pulse response is measured at the transmitter to describe the behavior of the transmitter as a filter if you apply the filter to a 1-UI pulse. P max


is the maximum amplitude of the extracted pulse response (Figure 3), and it can be affected by various factors such as the channel bandwidth.


Figure 4. Noise variable


𝜎


n


measurement.


The noise component of the signal, notated 𝜎


n


, is measured on the 61st UI of the 64-UI-long runs of 0s, 1s, 2s and 3s in the Compliance Pattern signal (Figure 4). The 61st UI is chosen for this measurement because, in theory, the signal has had plenty of time to settle down to its equilibrium state by the time the 61st UI arrives.


Figure 5. Distortion variable


𝜎


e


measurement.


The last component of the SNDR equation is 𝜎


e


, which is the distortion variable. It is a measure of how much the average signal deviates from the extracted pulse-response shape and is measured on the PRBS section of the compliance-pattern signal. In Figure 5, you can see three traces in the top grid. The red trace is the ideal pattern; the blue trace, which is overlapping the red trace, represents the average of the captured pattern; and the green trace is the difference between the average and ideal patterns, which is equal to the distortion 𝜎


e


.


Combining all three components returns the SNDR measurement, but the basic SNDR equation of Figure 2 is missing an important variable—the oscilloscope noise ( 𝜎


scope


). The oscilloscope noise impacts the SNDR of a real-life device, and it must be considered if you want to have an accurate SNDR measurement for your device. Figure 6 shows the equation for SNDR with the oscilloscope noise removed (SNDR NR


).


Figure 6. SNDR


NR


equation with oscilloscope noise removed.


Teledyne LeCroy's PCIe 6.0 Base Transmitter compliance test solution implements several noise-removal methodologies. The bottom of Figure 6 shows two measurements that were made on a Teledyne LeCroy LabMaster oscilloscope. The first is the SNDR without oscilloscope noise removed, and the second is the SNDRNR measurement with the oscilloscope noise removed.


### RLM Measurement


Figure 7. V


0


, V


1


, V


2


and V


3


signal levels
for calculating the transmitter RLM.


Transmitter linearity is defined as a function of the mean signal levels V 0


, V 1


, V 2


and V 3


transmitted for PAM4 two-bit symbols, as shown in Figure 7.


Transmitter RLM is defined by the following equations involving V 0


, V 1


, V 2


and V 3


:


V mid


= (V 0


+ V 3


) / 2


ES 1


= (V 1


– V mid


) / (V 0


– V mid


)


ES 2


= (V 2


– V mid


) / (V 3


– V mid


)


RLM = min((3 x ES 1


), (3 x ES 2


), (2 – (3 x ES 1


)), (2 – (3 x ES 2


)))


RLM is measured using the same section of the Compliance Pattern signal used for 𝜎


n


for the SNDR measurement, which is the 64-UI-long runs of 0s, 1s, 2s and 3s. The RLM measurement is straightforward. The goal is to have evenly spaced transition levels and to have RLM equal to 1.


### ps21 TX


Measurement


Figure 8. ps21 TX


measurement.


A key voltage measurement to make for PCIe 6.0 Base transmitter testing is the ps21 TX


measurement—the effective transmitter package loss. This measurement was also defined in the PCIe 5.0 Base specifications, so it's not a new measurement for PCIe 6.0. The package loss is measured by comparing the 64 0s and 64 1s voltage swing against a 1, 0, 1, 0 clock pattern. The ps21 TX


measurement is conducted with no transmitter equalization, and it is made by averaging over 500 repetitions of the Compliance Pattern signal to reduce noise. The measurement is illustrated in Figure 8, where V111 is the amplitude of a low-frequency signal and V101 is the amplitude of a high-frequency signal. The ps21 TX


measurement is then defined as follows:


ps21 TX


= 20 × *log* 10


(V 101


/ V 111


)


Adding the[SDAIII-PAMx](https://teledynelecroy.com/options/productseries.aspx?mseries=691&groupid=103) and[SDAIII-PCIE6](https://teledynelecroy.com/options/productseries.aspx?mseries=692&groupid=103) options to Teledyne LeCroy oscilloscopes provides all the software tools needed to easily make the new Compliance Pattern measurements to the PCIe standard, including the required oscilloscope noise removal. We'll discuss the noise removal methods in our next post.


Download our application note, *[Making New PCIe 6.0 Compliance Pattern Measurements with Your Oscilloscope](https://cdn.teledynelecroy.com/files/appnotes/pcie6compliance-measurements.pdf) .*


#### Also see:


[Get Ready for PCIe 6.0 Base Tx Testing--Compliance, Jitter and Eye Diagrams](https://blog.teledynelecroy.com/2022/06/get-ready-for-pci-express-60-base.html)
