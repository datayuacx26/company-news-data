---
schema_version: "1.0.0"
document_id: "82c82f67998949c2d1c3c58dd97de023bc060d7a7388585d183442a9f9b4daa0"
company_key: "lam-research-corporation-common-stock"
company: "Lam Research Corporation"
source_id: "lam-research-corporation-common-stock-news-import-7e1aef20b6ae"
canonical_url: "https://newsroom.lamresearch.com/accelerating-gaa-logic-yield-optimization-with-digital-twins"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-24T09:27:58.257666+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:f28b097168b60def426aadf912b41af4622f6756042fdd5468c1c345b13006b5"
---

# Accelerating GAA Logic Yield Optimization With Digital Twins

# Blog


Accelerating GAA Logic Yield Optimization With Digital Twins


[QingPeng Wang](https://newsroom.lamresearch.com/blog?author=103)


May 28, 2026


|


- [Industry](https://newsroom.lamresearch.com/blog?cat=2)
- [Technology](https://newsroom.lamresearch.com/blog?cat=3)


- Print Page


- Email URL


- [RSS Feed](https://newsroom.lamresearch.com/rss?rsspage=34141)


- [Facebook Share Button](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fnewsroom.lamresearch.com%2Findex.php%3Fs%3D34141%26item%3D634)


- [Twitter Share Button](https://twitter.com/share?url=https%3A%2F%2Fnewsroom.lamresearch.com%2Findex.php%3Fs%3D34141%26item%3D634)


- [Linkedin Share Button](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Fnewsroom.lamresearch.com%2Findex.php%3Fs%3D34141%26item%3D634)


- Digital twins allow engineers to minimize costly wafer experiments by simulating the entire GAA logic process upfront
- Machine learning applied to virtual data simultaneously reduces multiple critical failure modes, boosting yield from 1.6% to 87.2%
- This methodology offers a repeatable, cost-efficient path to accelerate advanced node manufacturing as device complexity grows


As logic devices transition from FinFETs to more complex gate-all-around (GAA) architectures, manufacturing variability has become a major barrier to achieving high yield.1,2 Hundreds of tightly coupled process steps now contribute to yield loss, making traditional wafer-based optimization slow, expensive, and often limited to addressing one failure mode at a time.3,4,5


To overcome these challenges, Lam has developed a digital twin–driven yield optimization methodology that enables engineers to explore process changes virtually, reduce failure rates, and accelerate advanced logic development.6,7


### From Wafer Experiments to Virtual Fabrication


The core of this approach is a full-flow digital twin of the GAA logic fabrication process. The virtual process faithfully reproduces front-end, middle-of-line, and back-end-of-line (BEOL) manufacturing steps—including SRAM, logic, and I/O regions—within a single simulation domain (Figure 1).


Key modules such as fin formation, shallow trench isolation, dummy gate processing, source/drain (SD) epitaxy (EPI), metal gate formation, self-aligned contacts, and BEOL metallization are modeled in sequence.


*Figure 1. Major steps of the GAA fabrication process*


### Making Failure Modes Visible Early


More than 10 common GAA failure modes were defined in the digital twin workflow shown in Figure 1. Representative examples shown in Figure 2 include fin top damage, poly gate residue, EPI-to-EPI shorts, EPI mushroom and collapse defects, SiGe residue, source/drain-to-metal-gate (SD-to-MG) shorts, and contact opens.


- These failures emerge directly from structural and material anomalies created during virtual processing and are detected using built-in structure search and virtual metrology algorithms. Calibration with real silicon inline data ensures close alignment with actual manufacturing behavior.


*Figure 2. Typical failure modes considered and their locations in the device structure*


### Optimizing Yield With Machine Learning


Key process parameters were statistically varied around baseline conditions, and failure ratios were evaluated across all modes (Figure 3a). A machine learning optimization engine then retargeted inline metrology specifications to minimize total failure rates simultaneously. After optimization, SD-to-MG shorts were reduced from ~80% to near zero, N/P EPI shorts from ~69% to ~4%, and SD opens were nearly eliminated. **The overall pass ratio increased from 1.6% to 87.2%** (Figure 3b).


*Figure 3. Failure ratios before (POR) and after optimization (OPT)*


### Turning Virtual Insights Into Actionable Targets


The optimized manufacturing parameters include actionable metrology targets that can be implemented across the full manufacturing sequence. Figure 4 illustrates one example: reducing epitaxy growth time and EPI size mitigates EPI-to-EPI shorts. Similar guidance is produced for hundreds of parameters, enabling engineers to efficiently reduce variability and improve yield in manufacturing.


*Figure 4. EPI size metrology and structure comparison before and after optimization*


### Conclusion


This study demonstrates how digital twin technology, combined with machine learning, can fundamentally transform yield optimization for advanced GAA logic technologies. By shifting experimentation to a virtual environment, engineers can reduce development cost, shorten cycle time, and simultaneously mitigate multiple failure modes. This digital twin–driven methodology provides a scalable and efficient path forward for yield improvement as device complexity continues to increase.


### References


1 S.B. Samavedam, J. Ryckaert, E. Beyne, K. Ronse, N. Horiguchi, Z. Tokei, I. Radu, M.G. Bardon, M.H. Na, A. Spessot, and S. Biesemans. 2020. "Future Logic Scaling: Towards Atomic Channels and Deconstructed Chips," 2020 IEEE International Electron Devices Meeting (IEDM), San Francisco, CA, pp. 1.1.1-1.1.10.


2 R.R. Das, T.R. Rajalekshmi, and A. James. 2024. "FinFETto GAA MBCFET: A Review and Insights," *IEEE Access,* Vol. 12, pp. 50556-50577.


3 M. Karbalaei, D. Dideban, and H. Heidari. 2023. "Impact of high-k gate dielectric with different angles of coverage on the electrical characteristics of gate-all-around field effect transistor: A simulation study," *Journal of Computational Electronics,* Vol. 22, No. 2, pp. 897-906.


4 T. Li, J. Hou, J. Yan, R. Liu, H.Yang, and Z. Sun. 2020. "Chiplet Heterogeneous Integration Technology—Status and Challenges," *Electronics* , Vol. 9, No. 4, p. 670.


5 A. Mallik, S. Borkar, and S. Narendra. 2019. "Economics of semiconductor scaling: a cost analysis for advanced technology node," 2019 Symposium on VLSI Technology, Kyoto, Japan, pp. T202-T203.


6 Q. Wang, Y. Zhong, L. Sun, B. Vincent, I.Chakarov, and J. Ervin. 2025. "Embracing Semiverse™ Solutions: Semiconductor Virtual Fabrication and Its Applications," 2025 9th IEEE Electron Devices Technology & Manufacturing Conference (EDTM), Hong Kong, China, pp. 1-3.


7 K.J. Kanarik, W.T. Osowiecki, Y. Lu, D. Talukder, N. Roschewsky, S.N. Park, M. Kamon, D.M. Fried, and R.A. Gottscho. 2023. "Human-machine collaboration for improving semiconductor process development," *Nature* , Vol. 616, pp. 707-711.


### Related Articles


- [Process Model Precision: Calibrating for Accurate Predictions of FinFET Device Profiles](https://newsroom.lamresearch.com/calibrating-for-accurate-predictions-of-finfet-device-profiles?blog=true)
- [Minimizing Voltage Loss and Improving Yield in Advanced GAA Chips](https://newsroom.lamresearch.com/minimizing-voltage-loss-advanced-gaa-transistors?blog=true)
- [Seeing Double: How Digital Twins Can Revolutionize Chipmaking](https://newsroom.lamresearch.com/how-digital-twins-can-revolutionize-chipmaking?blog=true)
