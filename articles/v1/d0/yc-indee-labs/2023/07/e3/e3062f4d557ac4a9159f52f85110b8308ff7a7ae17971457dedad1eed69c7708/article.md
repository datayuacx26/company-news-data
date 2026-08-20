---
schema_version: "1.0.0"
document_id: "e3062f4d557ac4a9159f52f85110b8308ff7a7ae17971457dedad1eed69c7708"
company_key: "yc-indee-labs"
company: "Indee Labs"
source_id: "yc-indee-labs-news-import-c97c2b3ad007"
canonical_url: "https://www.indeelabs.com/articles/automating-primary-human-t-cell-culture"
published_at: "2023-07-02T00:00:00+00:00"
first_seen_at: "2026-07-21T23:42:34.428466+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:76eb5309ed889f93203268febb61e837e9a16193198f699662b9ae7c74e8b3d1"
---

# Automating Primary Human T cell Culture

**By Monomer Bio & Indee Labs**


This article discusses the collaboration between Indee Labs and Monomer Bio aimed at addressing these two key challenges. Indee Labs has developed an instrument, Hydropore, which efficiently delivers DNA, RNA, CRISPR Cas9 RNPs, and more. Monomer Bio provides a cell culture automation platform that enables fully automated expansion, dynamic passaging, and sample preparation for transfection. Together, these innovations allow for end-to-end automation of CAR-T production.


## Abstract


Reliable and effective T cell culture is critical to developing and testing novel CAR-T therapies. When performed manually, it is also a monotonous, error-prone, and time-consuming process. Monomer Bio and Indee Labs have built a fully-automated work cell to address these problems. Herein, we compare the expansion of human primary T cells cultured by hand versus the Monomer work cell.


## Introduction


Advances in cell therapies, particularly Chimeric Antigen Receptor T cell therapies (CAR-T), have resulted in several promising new therapeutics for conditions ranging from cancer to heart disease to rare autoimmune diseases and even aging, where patients can experience decade-long remission \[1\]. These advances have led to six approved CAR-T therapies, with hundreds ** more currently under evaluation in the clinic \[2,3\]. Despite this therapeutic approach's rapid proliferation, numerous and wide-ranging challenges remain in designing, producing, and testing novel cell therapies.


To engineer a CAR-T therapy, peripheral blood mononuclear cells (PBMCs) are isolated from a patient-derived whole blood sample, which is then enriched to select specifically for T cells. Next, T cells are engineered to express the appropriate CAR complex and re-administered to the patient after release testing. While difficulties can arise at any point within this workflow, two areas are ripe for optimization and improvement \[4\].


First, the engineering step required for adequately expressing the CAR construct is particularly finicky. Historically, researchers have relied on lenti- and retroviral delivery mechanisms to efficiently modify host cell DNA, which is costly and creates additional biosafety considerations, making large-scale production and testing challenging \[5\]. Second, the cells must be expanded during culture before and after engineering.


## Materials & Methods


**Figure 1** Overview of the automated cell culture work cell and software platform.


All experiments were performed using human primary T cells enriched from human PBMCs ([STEMCELL Technologies, CAT# 72005](https://www.stemcell.com/human-peripheral-blood-mononuclear-cells-frozen.html#section-overview) ) using the EasySep Human T Cell Isolation Kit ([STEMCELL Technologies, CAT# 17591](https://www.stemcell.com/products/easysep-human-t-cell-isolation-kit.html) ). Manual and automated cell culture were performed concurrently with the same donor, and results were compared using flow cytometry data.


### Manual Cell Culture


#### Media Addition


*Media addition volumes are calculated before executing this step.*


1. Begin by warming up culture media (X-VIVO-20 + 1% HS) in the water bath for thirty minutes (or until the culture media reaches 37C.)


2. After culture media has warmed, move it to the hood along with a tube of IL-2.


3. To each well, add the appropriate amount of warmed media.


4. After media addition, add the appropriate amount of IL-2 (10 µL per mL of media) and mix.


5. Place the well plate back into the incubator.


#### Passaging


*Passaging protocol calculations are manually done before starting.*


1. Begin by warming up culture media (X-VIVO-20 + 1% HS) in the water bath for thirty minutes (or until the culture media reaches 37C.


2. After the media has warmed, place it into the hood alongside the warmed media and a new well plate.


3. Remove culture plates from the incubator, mix well that will be passaged with a pipettor, and then pull up the appropriate volume of cells and dispense them into the new well plate.


4. Afterward, dispense the proper amount of fresh media to the well to reach the final volume.


5. Repeat for each well of interest.


6. Place the new well plate into the incubator.


Optional: After all wells have been moved, add 10 µL of IL-2 (1:100 dilution) per 1 mL of media to each new well.


### Automated Cell Culture by Monomer


#### Media Addition


*Media addition volumes are calculated before executing this step.*


1. In a 96-deep well plate, add the appropriate amount of media to each well equal to the number of wells that will be fed.


2. In a 96-deep well plate, add 110 µL of IL-2 (1:20 dilution) for every 2 mL of media for each well that will be fed.


3. Place both 96-deep well plates into the automatic fridge.


4. From the Monomer software interface (Figure 1b), enter the culture values and execute the program.


#### Passaging


*Passaging protocol calculations are done before starting.*


1. In a 96-deep well plate, add the appropriate media volume to each well equal to the number of wells that will be passaged. Place well plate into the automatic fridge.


2. Place a fresh well plate onto the hotel stand.


3. From the Monomer software interface (Figure 1b), enter the passaging calculations, indicate the consumable locations, and execute the program. After it is finished, dispose of the old well plate from the fridge.


Optional (Future Implementation): In a 96-deep well plate, add 110 µL of IL-2 (1:20 dilution) for every 2 mL of media that will be fed.


## Results


**Figure 2** Results comparing automated and manual T cell culture and passaging.


Primary human T cells were expanded via manual culture (n=3) and automated culture (n=3) on the Monomer work cell. T cells showed no differences in growth rate (proliferation), viability, or total yield (see Figure 2a-c.) Passaging yield and viability on the Monomer work cell (n=12) were not different from passaging yields performed by hand (n=3) (see Figure 2d,e). Generally, there was reduced variability between operators on the automated work cell compared to manual culture.


Preparing experiments for the Monomer work cell requires about 15 minutes, and this work cell cultures four 24-well plates (or 96 total samples) concurrently. When performed manually, a simple media exchange takes a scientist, on average, 15 minutes to process 12 samples. This demonstrates that the work cell enables an **8-fold increase in sample throughput without increasing operator time.**


Furthermore, media exchanges and passages can be scheduled multiple days in advance, all within the same 15-minute window. **This scheduling eliminated the need for Indee Labs’ scientists to come into the lab over the weekend.** Experiments can also be monitored remotely with the Monomer software platform.


## Conclusions


Herein, we successfully demonstrate that primary human T cells cultured on the Monomer work cell show equivalent proliferation to those cultured via traditional, manual techniques. This is evidenced by the data presented in the Results section (see Figure 2), which show no differences between automated and manual cell culture across four key metrics:


(1) proliferation,
(2) viability,
(3) cumulative yield, and
(4) passage yield.


**We also demonstrate that automated cell culture on the Monomer work cell allows operators to process 8-fold more samples with equivalent operator time compared to traditional, manual techniques. The work cell eliminates the need for weekend shifts at the bench or in the hood.**


Per operator, sample capacity could easily be increased further by integrating a larger, automated cell culture incubator. These preliminary results must be verified at scale. Still, they indicate that automated T cell culture on the Monomer work cell is viable for increasing T cell culture throughput and reproducibility while eliminating weekend lab work.


Moving forward, Monomer and Indee Labs will continue with our collaboration on a fully automated CAR-T production work cell, including an SBS-format Hydropore instrument and cassette. Initially, this will involve the implementation of dynamic passaging; wherein passaging media volumes are calculated well-to-well based on cell count data derived from an automated flow cytometry assay. T cells will be isolated and activated using[Dynabeads](https://www.thermofisher.com/order/catalog/product/11161D#:~:text=Dynabeads%E2%84%A2%20Human%20T%2DActivator%20CD3%2FCD28%20are%20for%20the,expansion%20of%20human%20T%20cells.&text=Dynabeads%E2%84%A2%20Human%20T%2DActivator%20CD3%2FCD28%20offer%20a%20simple,%2Dpresenting%20cells)%20or%20antigen.) in preparation for the cell count assay. These steps take 3 hours to perform manually but will be reduced to only about 15 active minutes for a scientist using the Monomer work cell, including dynamic passaging.


Previously, Hydropore has been shown to permeabilize cell membranes allowing for the incorporation of foreign material, enabling gene editing, and compatibility with high-throughput screening workflows \[7,8\]. Upon successfully implementing the sample preparation and flow assay steps, Monomer and Indee Labs will have demonstrated that the upstream and downstream processes from transfection via Hydropore are automatable. This would establish a promising foundation for developing an off-the-shelf cell therapy work cell, enabling cell therapy researchers to rapidly iterate on therapeutic designs and techniques, at scale, with efficient use of hands-on time and materials \[1\].


Together, Monomer Bio and Indee Labs strive to provide hardware, software, and automation solutions that match the rapid pace of scientific innovation in cell therapies, enabling researchers to efficiently carry out experimentation and reach reliable and actionable insights.


#### Citations


1. Melenhorst, J. J. *et al.* Decade-long leukaemia remissions with persistence of CD4+ CAR T cells. *Nature* **602** , 503–509 (2022).


2. Moreno-Cortes, E., Forero-Forero, J. V., Lengerke-Diaz, P. A. & Castro, J. E. Chimeric antigen receptor T cell therapy in oncology – Pipeline at a glance: Analysis of the ClinicalTrials.gov database. *Critical Reviews in Oncology/Hematology* 159, 103239 (2021).


3. Maalej, K. M. *et al.* CAR-cell therapy in the era of solid tumor treatment: current challenges and emerging therapeutic advances. *Mol Cancer* 22, 20 (2023).


4. CAR T Cells: Engineering Immune Cells to Treat Cancer - NCI.[https://www.cancer.gov/about-cancer/treatment/research/car-t-cells](https://www.cancer.gov/about-cancer/treatment/research/car-t-cells) (2013).


5. Harris, E. & Elmer J. Optimization of electroporation and other non‐viral gene delivery strategies for T cells. *Biotechnology Progress* 37, 1 (2020).


6. Jarrell, J. A. *et al.* Intracellular delivery of mRNA to human primary T cells with microfluidic vortex shedding. *Sci Rep* **9** , 1–11 (2019).


7. Jarrell, J. A. *et al.* Numerical optimization of microfluidic vortex shedding for genome editing T cells with Cas9. *Sci Rep* 11, 1–13 (2021).


8. Kasper, S. H. *et al.* A high-throughput microfluidic mechanoporation platform to enable intracellular delivery of cyclic peptides in cell-based assays. *Bioengineering & Translational Medicine* , e10542.


#### **Interested in using Hydropore™ in your lab or developing Hydropore™ for other applications?**


[Learn more](https://indeelabs.com/contact)
