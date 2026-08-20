---
schema_version: "1.0.0"
document_id: "a178b2ae166452eae820dcf4e9b320fd2bf1d69569ed6f2ab8bd63cf08623375"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2026/05/20/assuring-positioning-navigation-and-timing-understanding-gps-gnss-attacks-and-mitigation-solutions/"
published_at: "2026-05-20T14:50:13+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:bead571b7e880924b16ed76c4166b904ac16da78182b10d5b3f6b85490a1d850"
---

# Assuring Positioning, Navigation and Timing: Understanding GPS/GNSS Attacks and Mitigation Solutions

**


# Assuring Positioning, Navigation and Timing: Understanding GPS/GNSS Attacks and Mitigation Solutions


- **Said Jackson
- ** May 20, 2026
- 1


The lack of strength of GNSS signals at the end of their journey from a satellite makes them highly prone to interference. The RF power level of a GPS or Galileo signal, for instance, is[below the thermal signal floor](https://www.ieee.li/pdf/viewgraphs/gnss_fundamentals.pdf) by the time it reaches the earth.


Interference techniques are being used widely, especially around war zones, to disrupt positional data and interrupt both military and commercial operations. It’s now quite typical to hear that[thousands of vessels lost navigation capability](https://x.com/WindwardAI/status/2028034930790256653?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E2028034930790256653%7Ctwgr%5Eb735fc1246a3eb32ac814d8988a7cad75db47972%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fwww.france24.com%2Fen%2Fmiddle-east%2F20260306-surge-gps-interference-strait-hormuz-increases-shipping-risks) following the outbreak of conflict.


Timing attacks can also have critical consequences for a vast range of services, including telecom networks, data centers, financial exchanges, and power grids. Timing attacks have far less visibility in the media, but can result in internet outages, websites crashes, disruption of financial markets, and can even affect power generation and distribution.


In this blog, we outline the core types of interference, how it can be detected, and techniques to mitigate it.


### **GNSS Attack Techniques**


Two major classes of attack exist: jamming and spoofing.


Jamming is a brute-force DoS approach that blocks out the signals from GNSS satellites, located[approximately 20,000 km](https://lweb.cfa.harvard.edu/space_geodesy/ATLAS/gps.html) (12,000 miles) from earth by broadcasting a signal on the same L1 or L2 frequency band.


Spoofing, on the other hand, attempts to provide a signal with the same structure of that from an authentic satellite but containing false position or timing data, and potentially manipulated telemetry data that can make use of known receiver bugs or weaknesses. These can cause crashes or malfunction of GNSS receiver devices. When done subtly, the technique can deliver a series of changes over time that can misdirect a plane or ship and lead it into danger of being[shot down](https://www.cbsnews.com/news/azerbaijan-plane-crash-kazakhstan-riddled-with-holes-report/) ,[crashing](https://www.reuters.com/world/middle-east/uk-maritime-firm-says-it-is-aware-incident-east-uaes-khor-fakkan-2025-06-17/) , or[grounding](https://splash247.com/msc-ship-aground-off-jeddah-likely-victim-of-gps-spoofing/) .


A[report from the GPS Spoofing Workgroup](https://ops.group/blog/gps-spoofing-final-report/) estimated 1,500 flights per day in Q3 2024 were being spoofed, up 500% on Q1/Q2.


In addition to these major classes is meaconing, where an actual signal with intact authentication is captured and transmitted to provide a false timing or positioning signal that can beat cryptographic protections. Similar to meaconing are replay attacks, which take the same approach, but specifically target applications reliant on GPS-timing signals.


Finally, there are data-level attacks (such as navigation message spoofing or ephemeris attacks) that again target timing signals. These provide false orbital data, clock corrections, and GPS time in addition to location data, which makes them particularly hard to identify.


### **Detection of GPS Jamming and Spoofing**


Cryptographic encryption was not originally integrated into civilian GPS signals, meaning that equipment relying on these signals must find another way to ensure signal legitimacy. Even if constellations such as GPS Chimera and Galileo OSNMA have implemented public key encryption, robust detection and mitigation techniques demand a zero-trust approach as set out in the[draft IEEE P1952 standard](https://sagroups.ieee.org/p1952/) . Meaconing attacks can potentially bypass NMA type signal authentication – as a meaconed signal is a real GNSS signal, just shifted in time or position – unless the receiver uses an independent source of time or known position, or alternative source of NMA data such as the VIAVI altGNSS-GEO-L NMA signal transmitted on a non-GNSS frequency.


Multiple methods can be used to identify when a GNSS attack is taking place. Two of the most effective techniques are spatial filtering and RAIM.


Spatial filtering or spatial nulling is a signal processing technique that relies on interference signals being (almost always) ground-based, with attack signals demoted to the antennas blind spot. To achieve this, advanced antenna designs (helical or CRPA) are needed.


VIAVI’s SecureTime altGNSS GEO-L Helical Antenna can be used to identify and mitigate a jamming or spoofing attack


Antennas using a high-gain helical design enable a narrow field of view that can be pointed solely at the sky. Technically, these don’t identify jamming attempts but are simply blind to ground-based signals from jamming and spoofing equipment. Jamming signals can be suppressed by up to c.40dB. Examples of this type include[VIAVI’s SecureTime altGNSS GEO-L Helical Antenna](https://www.viavisolutions.com/en-us/literature/securetime-altgnss-geo-l-helical-antenna-data-sheets-en.pdf) . This antenna can receive a completely GNSS-independent timing signal from a Geostationary satellite, as well as NMA signals to authenticate attached GNSS receivers and signals to correct GNSS timing to sub 1ns accuracy. These signals are sent outside of GNSS frequency bands, and thus also have a lower likelihood of being jammed.


CRPAs (controlled reception pattern antennas), on the other hand, organize elements into a geometric pattern that can be used to actively identify a signal’s angle of arrival (AoA). Then, by adjusting the phase and gain, signals coming from the direction of the attack can be blocked. While CRPAs are more complex, they enable a higher level (c.50 dB) of suppression than helical antennas.


The use of either technique will be dictated by the application, with helical antennas best suited to fixed-site infrastructure such as a power grid or data center. They can be used in dynamic environments including aircraft and ships, but banking sharply or rolling over the waves can lead to temporary loss of signal. As a result, these applications would be better suited to using CRPAs, which can nullify the threat without losing sight of the satellite cluster.


RAIM (Receiver Autonomous Integrity Monitoring) takes a different approach and almost always relies on there being more GNSS signals available than the four that are required to calculate a position and timing


The methodology calculates a position from multiple subsets of the GNSS signals and, if there is a significant discrepancy in one versus the others, it assumes the outlier is the result of a spoofing attempt. At this point it undertakes FDE (Fault Detection and Exclusion) to remove this one signal from its positioning calculation.


An adaptation of the RAIM technique takes this methodology a step further. Instead of cycling through the available GPS signals, the advanced version of RAIM cycles through all available constellations, calculating a position from each PNT cluster including Galileo, BeiDou and GLONASS.


### **GNSS Attack Mitigation**


The techniques described above can help reduce the risk of an attack, but additional steps can and should be taken to ensure continuity of operations in GNSS-denied, -degraded or -disrupted environments(D3SOE).


The higher levels (3, 4 and 5) of IEEE P1952 require that a holdover capability is enacted. This can be temporary (level 3) or indefinite (level 4), with level 5 also requiring the ability to verify trust in a GNSS signal.


For position and navigation, this holdover can be achieved through inertial sensors such as tactical- or military-grade MEMS 3-axis gyroscopes and accelerometers. While these still suffer bias – which will lead to a reduced positional confidence over time – the effect of individual sensor bias is mitigated as more sensors and sensor classes are added. These additional sources can be viewed as the “fourth and fifth” satellite in a GNSS Kalman filter solution. VIAVI’s rubidium clocks can hold time to better than 100ns over 24 hours for example, and can thus be used directly in the GNSS Kalman solution as an additional state for many hours after going into holdover. They can also be used as an absolute source of timing in order to detect timing spoofing attempts.


For example, in addition to using GNSS signals via a CRPA antenna port,[VIAVI’s Inertial Labs IRINS](https://www.viavisolutions.com/en-us/news-releases/inertial-labs-viavi-solutions-company-launches-irins-leo-aided-inertial-navigation-system) system combines MEMS sensors with barometers and magnetometers, plus a rubidium oscillator clock to calculate altitude, bearing and timing. It also integrates the[STL-2600](https://www.viavisolutions.com/en-us/products/stl-2600-low-earth-orbit-receiver-module) for communication with LEO clusters for an alternative, non-GNSS PNT source.


The ability to recalibrate inertial navigation systems in D3SOE situations can also be undertaken using vision systems, as is achieved by[VIAVI’s Inertial Labs VINS](https://www.viavisolutions.com/en-us/news-releases/inertial-labs-viavi-solutions-company-launches-visual-aided-inertial-navigation-system-gps) (Visual Inertial Navigation System) technology. This uses techniques that cross reference outputs from a standard or IR camera against specially created maps to pinpoint an exact location based on visible landmarks.


For applications reliant on GNSS timing signals, a range of holdover technologies are available, with both cesium clocks and GEO-L based services able to meet ITU-T G.8272.1 requirements. This stipulates that ePRTC holdover clocks must have a maximum deviation of less than 30 ns from UTC when entering holdover, and a long-term drift of less than 100 ns over 14 days. GEO-L based services are able to maintain this indefinitely, with the first such system,[VIAVI’s ePRTC360+](https://www.viavisolutions.com/en-us/products/eprtc360-enhanced-primary-reference-time-clock) , launched in February 2026.


In addition to these methods, a final core technique is the use of alternative satellite clusters for PNT data – as is used by[VIAVI’s SecureTime GEO](https://www.viavisolutions.com/en-us/products/securetime-geo-services) and[LEO services](https://www.viavisolutions.com/en-us/products/securetime-leo-services) , and embedded into the[SecurePNT 6200](https://www.viavisolutions.com/en-uk/products/securepnt-6200-series) .


LEO constellations can now feature PNT signals. These satellites are[95% closer to earth](https://www.electronicspecifier.com/products/communications/leo-pnt-reality-or-hype/#:~:text=Main%20differences%20between%20LEO%2C%20MEO,challenges%20and%20opportunities%2C%20such%20as:) than MEO clusters used for GNSS, which creates more powerful signal strengths on earth and far greater Doppler shifts. As a result, (static) earth-based spoofing attempts are more easily identifiable.


While positioned further away from the earth, GEO broadcasts from clusters are transmitted with significantly higher power levels than GPS. The fixed angle of arrival from these geostationary satellites also means any broadcast (even a replayed meaconing attack) that is out of position can be identified as potentially malicious. Antennae with gain such as helical or parabolic antennae can be used to track these geostationary sats with large signal gain and side-lobe jamming attenuation.


Both LEO and GEO satellites implement encrypted data streams, which enable the receiver to check the validity of every packet of data and the rejection of any spoofing attack.


### **Protecting Legacy Systems**


Of course, legacy systems need to be protected as much as new systems. The B-52 ([first flown in 1952](https://www.museumofflight.org/exhibits-and-events/aircraft/boeing-b-52g-stratofortress#:~:text=The%20YB%2D52%20prototype%20first,model%20last%20delivered%20in%201962.) ) bomber and MiG 21 ([1955](https://en.wikipedia.org/wiki/Mikoyan-Gurevich_MiG-21) ) are both still in active use. For commercial aviation, at least one Boeing 737-200 ([1974](https://www.facebook.com/groups/2365210970/posts/10160457686475971/) ) is still flying. And if we look to maritime vessels, the Star of India cargo ship is still sailing from San Diego (albeit as a museum exhibit, and without GPS) despite having been[built in 1863](https://en.wikipedia.org/wiki/Star_of_India_(ship)) .


As upgrading any existing GPS systems would be a challenge, such legacy systems need to use an alternative. RSR transcoders, such as[VIAVI’s second generation system](https://www.viavisolutions.com/en-us/products/rsr-transcoder-20) , enable assured PNT services to be accessed by translating data from verified-trusted sources into a GPS signal for use with any GPS equipment by simply replacing the legacy GNSS antenna with the synthesized GPS RF output of the transcoder.


- ** Categories:[Aerospace and Defense](https://blog.viavisolutions.com/category/aerospace-and-defense/)
- ** Tags:[PNT](https://blog.viavisolutions.com/tag/pnt/) ,[Position Navigation and Timing](https://blog.viavisolutions.com/tag/position-navigation-and-timing/)


-
-
-
-
-
-
