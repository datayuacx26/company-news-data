---
schema_version: "1.0.0"
document_id: "99cc78b3138882aad6323cb6c94a4ed66cd96e99981ef9fbc264367194c0c8a4"
company_key: "lam-research-corporation-common-stock"
company: "Lam Research Corporation"
source_id: "lam-research-corporation-common-stock-news-import-7e1aef20b6ae"
canonical_url: "https://newsroom.lamresearch.com/optimizing-the-nano-tsv-to-bpr-connection-in-backside-power-networks"
published_at: "2026-06-29T00:00:00+00:00"
first_seen_at: "2026-07-24T09:27:58.257666+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:ebfc8b155e078cd6b7eb9db68a51ad60fdbbeb1468a50c860a0b54418dc6f06b"
---

# Optimizing the Nano-TSV-to-BPR Connection in Backside Power Networks

# Blog


Optimizing the Nano-TSV-to-BPR Connection in Backside Power Networks


[Assawer Soussou](https://newsroom.lamresearch.com/blog?author=91)


Jun 29, 2026


|


- [Industry](https://newsroom.lamresearch.com/blog?cat=2)
- [Technology](https://newsroom.lamresearch.com/blog?cat=3)


- Print Page


- Email URL


- [RSS Feed](https://newsroom.lamresearch.com/rss?rsspage=34141)


- [Facebook Share Button](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fnewsroom.lamresearch.com%2Findex.php%3Fs%3D34141%26item%3D639)


- [Twitter Share Button](https://twitter.com/share?url=https%3A%2F%2Fnewsroom.lamresearch.com%2Findex.php%3Fs%3D34141%26item%3D639)


- [Linkedin Share Button](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Fnewsroom.lamresearch.com%2Findex.php%3Fs%3D34141%26item%3D639)


- Using SEMulator3D®, engineers can create a realistic virtual model of the connection between nano-TSVs (nTSVs) and buried power rails
- A study was conducted on the impact of nTSV overlay and patterning effects
- The results showed that rounded corners increase sensitivity to overlay-induced resistance


As technology nodes continue to scale, maintaining power and performance while reducing the footprint has become increasingly challenging. Backside power delivery networks (BSPDNs) address this challenge by moving the power delivery network to the wafer backside.


### What Is Backside Power Delivery (BSPDN)?


The problem being solved by BSPDNs is similar to getting people out of a crowded office. If everyone uses the same front hallway, it gets packed and slows people down. But if you add a second hallway in the back just for deliveries, the front hallway is less crowded, and everything moves more smoothly. In the same way, backside power delivery sends power through the “back hallway,” leaving the front side of the chip less congested.


To enable BSPDNs, nano-through-silicon vias (nTSVs) are connected to buried power rails (BPRs), which move some of the power wiring lower into the chip. This relieves frontside routing congestion and improves signal integrity.


### Key Process Challenges: Overlay and Patterning Effects


While effective, this design introduces critical process challenges:


- Overlay variation during nTSV alignment
- Corner rounding during patterning


These factors directly impact nTSV-BPR connection resistance and thus chip performance and yield.


The Semiverse® Solutions team, in conjunction with our colleagues at imec, recently analyzed BSPDN technology processes and performance using a predictive simulation in SEMulator3D®. We evaluated the impact of nTSV overlay and patterning effects on nTSV resistance to identify possible margins for process optimization.


### Modeling nTSV Resistance Using SEMulator3D


A SEMulator3D virtual measurement was used to perform resistance extraction of a cross-bridge Kelvin resistor (CBKR) structure to optimize the nTSV-BPR connection, since the CBKR isolates the resistance of the connection from the contribution of the metal lines (Figure 1).


*Figure 1. Kelvin resistance of the BSM-TSV-BPR segment*


The left drawing displays the TSV line resistance, which measures the resistance of the complete electrical path that passes through the TSV, including the copper conductor inside the TSV and any top and bottom TSV connections. The drawing on the right displays the TSV Kelvin resistance, which isolates out the resistance of the metal lines. Only the resistance of the TSV connections is measured in the right-hand diagram.


Using SEMulator3D virtual fabrication, a full process step sequence was simulated to reproduce realistic process effects in the model of a full CBKR structure.


Figure 2 displays an nTSV-BPR TEM cross-section along with the matching process simulation result. The simulation accurately reproduced the resulting nTSV-BPR profile and shape.


*Figure 2. BSM-nTSV-BPR TEM cross sections and process simulation result*


To move beyond idealized cylindrical or polygonal vias, the full process flow was simulated in SEMulator3D so that realistic profile details could be captured. The modeled CBKR structure reproduced the observed nTSV-BPR cross-section, enabling resistance extraction on process-realistic geometries and improving the relevance of comparisons with electrical measurements.


In our study, we simulated both TSV patterning corner-rounding effects and TSV overlay effects.


Figure 3 depicts two nTSV patterning configurations simulated using SEMulator3D: one without corner rounding and one with corner rounding.


*Figure 3. Edge placement error (EPE) measurement top view and 3D view of nTSV patterning rounding simulation*


The simulations compared square and rounded-corner nTSV profiles while sweeping the overlay from 0 to 45 nm (Figure 4).


*Figure 4. Modeling of nTSV corner rounding and various overlay (OVL) dimensions*


### Simulation Highlights: Key Findings


The results showed that rounded corners increase sensitivity to overlay-induced resistance, making realistic patterning effects essential for accurate modeling.


Simulation results demonstrated that the nTSV overlay needs to be less than 30 nm to guarantee a safe connection between the nTSV and the BPR. The nTSV-BPR connection is more sensitive to overlay problems when using rounded corners compared to when using a square section. Thus, TSV patterning rounding effects are critical and need to be considered in any resistance simulation.


The process simulation using realistic profiles was then coupled to a virtual electrical simulation to provide a predictive nTSV-BPR resistance model. A process variability evaluation of low nTSV resistance was then performed. BPR contact area optimization and the impact of nTSV overlay variation on nTSV-BPR chain link resistance was also explored.


Figure 5 displays the impact of the nTSV overlay on resistance using an nTSV with patterning rounding.


*Figure 5. The impact of nTSV overlay on resistance*


Simulated resistance was also benchmarked against actual nTSV resistance measurements. Our simulations reproduced the minimum values of the measurement distribution, validating the simulated resistance model. Simulations indicated that resistance reduction and tighter control were achieved when the nTSV overlay was limited to about 15 nm.


### Conclusion: Predictive Modeling for Better Process Windows


SEMulator3D process modeling is highly useful in performing a predictive process evaluation of a backside-to-frontside connectivity integration via an nTSV connection. A model can be created that predicts the resistance of an nTSV-BPR structure and includes realistic process effects. Both TSV patterning corner rounding and TSV overlay effects can be simulated to enable realistic resistance predictions and an assessment of the impact of process variations. The predictive BPR model developed in this study could be used to develop optimal process windows and to improve yield when BPR dimensions are scaled down further.


***Assawer Soussou** is a senior semiconductor process & integration engineer with France Semiverse® Solutions R&D.*


### Acknowledgements


The author gratefully acknowledges Michele Stucchi, Anne Jourdain, and imec for their invaluable contributions to this work.


This work was supported by a joint undertaking by imec and Electronic Components and Systems for European Leadership (ECSEL). The ID2PPAC project objective is to demonstrate that performance, power, area, and cost (PPAC) requirements can be achieved for the 2-nm node generation of leading-edge logic technology.


### References


J. Ryckaert, A. Gupta, A. Jourdain, B. Chava, G. Van der Plas, D. Verkest, E. Beyne, "Extending the roadmap beyond 3nm through system scaling boosters: A case study on Buried Power Rail and Backside Power Delivery", Electron Devices Technology and Manufacturing Conference (EDTM), pp. 50-52 (2019).


A. Jourdain, M. Stucchi, G. Van der Plas, G. Beyer, and E. Beyne, "Buried power rails and nano-scale TSV: Technology boosters for backside power delivery network and 3D heterogeneous integration," in Proc. IEEE 72nd Electron. Compon. Technol. Conf. (ECTC), pp. 1531–1538 (2022).


D. Prasad; S. S. T. Nibhanupudi; S. Das; O. Zografos; B. Chehab; S. Sarkar, "Buried power rails and back-side power grids: Arm CPU power delivery network design beyond 5 nm", IEDM Tech. Dig., Dec. 2019, pp. 19.1.1–19.1.4.


M. Stucchi, F. Fodor and E. J. Marinissen, "Accurate Measurements of Small Resistances in Vertical Interconnects with Small Aspect Ratios", 2020 IEEE European Test Symposium (ETS), Tallinn, Estonia, 2020, pp. 1-6.


### Related Information


- [SEMulator® 3D product page](https://www.lamresearch.com/product/semulator3d/)
- [The Other Side of the Wafer](https://newsroom.lamresearch.com/transistor-channel-stress-backside-power-delivery-networks?blog=true)
- [Minimizing Voltage Loss and Improving Yield in Advanced GAA Chips](https://newsroom.lamresearch.com/minimizing-voltage-loss-advanced-gaa-transistors?blog=true)
