---
schema_version: "1.0.0"
document_id: "aa086639d00acdceac8ebf7b899cac992b41586d969444d04bcea3829acbeddd"
company_key: "yc-indee-labs"
company: "Indee Labs"
source_id: "yc-indee-labs-news-import-c97c2b3ad007"
canonical_url: "https://www.indeelabs.com/articles/microfluidic-vortex-shedding-enhances-genome-edited-chimeric-antigen-receptor-t-cell-function-sz8fp"
published_at: "2023-11-06T00:00:00+00:00"
first_seen_at: "2026-07-21T23:42:34.428466+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:235aeae8ff14c6c00fa23bfa15917b2fa2f0da9bc07a2dc475224f4a3e821af6"
---

# Microfluidic Vortex Shedding Enhances Genome-Edited Chimeric Antigen Receptor T Cell Function

#### What Is CAR-T Therapy?


Adoptive Chimeric Antigen Receptor (CAR) T-cell therapy is a cutting-edge therapy used to treat certain types of cancer \[ **1,2** \]. It works by removing a patient’s T-cells (immune cells) and modifying them in a lab to express a special receptor called CAR. The CAR helps the T cells recognize and attack cancerous tumor cells.


This modification process involves linking the CAR to other signaling molecules that activate those immune cells when they encounter cells they recognize as tumors. The modified CAR-T cells are then multiplied in the lab and infused back into the patient’s body, where they can specifically target and destroy cancer cells.


Recent studies have shown that this therapy can lead to complete remission (cancer disappearance) in patients with certain types of leukemia \[ **3,4** \]. As a result, the U.S. Food and Drug Administration (FDA) has approved several CAR-T therapies for certain types of blood cancers.


Possible Concerns


Despite its promising potential, there are safety concerns with this therapy. When CAR-T cells become highly activated against tumor cells, they may also trigger a powerful immune response that can lead to severe side effects. These side effects are mainly caused by the release of inflammatory substances, leading to conditions like cytokine release syndrome and neurotoxicity \[ **5-7** \].


To improve the safety and effectiveness of CAR-T therapy, researchers are exploring different methods to modify the T cells. One approach involves using precise genome editing techniques like CRISPR/Cas9 to integrate the CAR gene into the T cells' DNA at specific locations \[ **8,9** \]. This ensures more accurate modification of the cells and reduces the risk of unwanted genetic changes.


#### Nucleofection


In 2004, the Swiss company Lonza introduced nucleofection, which uses electrical pulses to introduce the gene-editing components into the cells. It involves using small electrical pulses to temporarily open tiny holes in the cell's protective outer membrane, allowing the genetic material to enter the cell \[ **10,11** \]. Once inside, the genetic material can modify the cell's DNA, making it capable of performing new functions or fighting diseases \[ **12** \]. Since then, it’s been adopted as an industry standard for intracellular delivery, particularly with human T cells. However, there are still several drawbacks to this traditional method.


For one thing, this method directly damages genomic DNA as well as proteins and lipids within the cell. This typically irreversible damage puts a tremendous strain on the modified cells, which can lead to cell death \[ **13,14** \].


#### Hydroporation


Hydroporation is an alternative way to deliver the CRISPR/Cas9 gene-editing machinery into the T cells. Hydroporation uses a gentle fluid force to permeabilize the cell membrane, allowing the gene-editing components to enter the T cells \[ **15** \]. This method has shown promising results in efficiently modifying the T cells without causing severe damage like nucleofection.


#### Hydroporation vs. Nucleofection


Researchers compared hydroporation with the conventional method nucleofection. They found that hydroporation resulted in higher T cell viability and better expansion, leading to greater CAR-T cell yields (Figure 1A and Figure 1B). Ultimately, hydroporation resulted in 1.9-fold greater yield of CAR-T cells by day 7 post-transfection compared to nucleofection (Figure 1C).


**Figure 1. CAR-T cells transfected via hydroporation showed improved viability, proliferation, and EGFPR+ cells** . (a) viability of cells vs days post-transfection, (b) number of viable cells vs days post-transfection, and (c) number of EGFR+ cells when transfected by either hydroporation or nucleofection. (d) cytotoxicity at different effector-to-target cell ratios.


#### Cytotoxicity


Recent studies suggest a strong connection between CAR-T proliferation, how long they last inside the patient’s body, and how well the patient responds to the treatment \[ **3,16** \]. To better understand the connection, scientists have examined the ability of CAR-T cells to activate the appropriate immune response while simultaneously avoiding activation-induced cell death (AICD) \[ **17,18** \].


The team used CAR-T cells modified with hydroporation and nucleofection by varying the number of CAR-T cells matched with a single cancer cell (effector-to-target ratio) to see if one method performed better. They found that hydroporated CAR-T cells were superior in all tested E:T ratios.


Even at a ratio of 1 CAR-T cell to 8 cancer cells, the hydroporated CAR-T cells were able to eliminate more than 90% of the cancer cells, whereas the nucleofected CAR-T cells (Nu-CAR-T) only managed to clear 52% of the cancer cells at the same ratio.


#### TIMING Assay


**Figure 2.** **Image showing cells within the TIMING assay nanowells** . CAR-T effector cells (green) and NALM6 target cells (orange) in TIMING assay nanowells at various effector-to-target cell ratios (E:T) at t=0.


Using a Time-lapse Imaging Microscopy in Nanowell Grids (TIMING) assay, scientists compared CAR-T cells from five donors after processing them with either hydroporation or nucleofection. Then, they put the modified CAR-T cells in very small wells (nanowells) with cancer cells (NALM6 target cells) to observe the interaction (Figure 2).


During the interaction, the team measured tSeek (the time it takes for the CAR-T cell to establish contact with the cancer cell), tContact (how long the CAR-T cell and cancer cell stay in contact), and tTarDeath (how long it takes the cancer cell to die after making contact with the CAR-T cell).


**Figure 3.** **CAR-T cells modified through hydroporation showed no significant differences in tSeek, tContact and tTarDeath values when compared to nucleofected cells.** Time-based measurements of T cell target engagement of hydroporated cells (red) and nucleofected cells (green), including (a) time to synapse formation, (b) duration of synapse formation, (c) time to target cell death, (d) percentage of synapse formation, (e) percentage of spontaneous T cell death, and (f) percentage of target cell lysis with one or more target cells.


The scientists found no significant differences in the tSeek, tContact, tTarDeath, synapse formation when comparing the CAR-T cells modified with hydroporation vs. those modified with nucleofection (Figure 3A-D). In other words, the methods were equally efficient in causing the CAR-T cells to seek and kill the cancer cells in this specific experiment.


Also using the TIMING assay, the team analyzed CAR-T cell behavior before and after activation by a target. For spontaneous death (when no target cells are present), a slight increase was observed in nucleofected CAR-T cells, and this requires further study (Figure 3E). In Figure 3F, scientists observed no difference in killing percentage of target cells.


**Figure 4.** **Hydroporated cells showed reduced AICD and improved motility when compared with nucleofected cells.** Activation-induced cell death and cell motility upon target cell engagement for hydroporated cells (red) and nucleofected cells (green) across 5 donors. Showing % of AICD at different ratios of effector cells to target cells at either (a) 1E:1T, (b) 1E:2+T, (c) 1E:1+T and (d) summary E:T ratios. Motility during synapse at (e) 1E:1T, (f) 1E:2T, (g) 1E:2T, and (h) motility summary for different targets and E:T ratios.


When CAR-T cells are activated by cancer cells, the CAR-T cells sometimes die because of this activation. This undesirable phenomenon is called activation-induced cell death, or AICD. Hydroporated CAR-T cells displayed significantly less AICD compared to nucleofected CAR-T cells, indicating that hydroporation is less likely to induce AICD (Figure 4A-D), and therefore promotes a longer killing lifespan for the CAR-T cells.


The team also observed that when CAR-T cells were activated and formed synapses (cell-to-cell contact) with cancer cells, the hydroporated CAR-T cells were significantly faster when moving to subsequent target cells when compared to the nucleofected CAR-T cells (Figure 4E-H). In simple terms, hydroporated CAR-T cells not only survived better when they encountered cancer cells but were also more agile in their movements when they interacted with these targets. These qualities would be advantageous for attacking and destroying cancer cells in the body in a more rapid manner.


#### Cytokine Profiles


Not only did hydroporated cells show significantly higher viability 24 hours after transfection compared to nucleofected cells (~60% vs. 20% in Figure 1A), but they also showed reduced perturbation of the cytokine profile (Figures 5 & 6). Scientists used nELISA, a protein profiling program \[ **19** \]. This may explain why hydroporated CAR-T cells exhibit reduced AICD and increased motility. This also suggests hydroporated CAR-T cells may reduce the risk of cytokine-associated toxicity compared to nucleofected CAR-T cells (Figure 5).


**Figure 5.** **Hydroporated cells showed reduced perturbation of cytokine profiles** . Volcano plot depicting resting T cell cytokine secretion fold changes and P values for supernatants collected 24 hours after processing (a) hydroporated TCR knockout, (b) nucleofected TCR knockout, c) hydroporated AAV CAR-T knock-in and (d) nucleofected AAV CAR-T knock-in. (e) summary of up/down regulated proteins relative to the NTC.


**Figure 6.** **Hydroporated cells showed a significant change in cytokine profiles when directly compared to nucleofected cells** . Volcano plot depicting resting T cell cytokine secretion fold changes and P values for supernatants collected at 24 hours (a) nucleofected cells vs hydroporated cells in NTC, (b) nucleofected cells vs hydroporated cells in TRC KO treated cells and c) nucleofected cells vs hydroporated cells in TRC KI treated cells. (d) summary of up/down regulated proteins relative to hydroporated cells.


In Figure 5, scientists compared the cytokine profiles of both hydroporated and nucleofected TCR knockout (KO) and CAR-T knock-in (KI) to untreated cells to assess perturbation or how the process itself affects the cytokine profile. In Figure 6, scientists directly compare hydroporated TCR KO and CAR-T cells to nucleofected cells. In this study, scientists see more significant differences in the cytokine profiles.


**Figure 7.** **Hydroporated CAR-T cells showed a significant difference in cytokine profiles compared to nucleofected CAR-T cells.** Volcano plot depicting resting T cell cytokine secretion fold changes and P values for supernatants collected at 24 hours (a) nucleofected vs hydroporated CAR-T cells at a 1:1 E:T ratio, (b nucleofected vs hydroporated CAR-T cells at a 1:2 E:T ratio, (c) nucleofected vs hydroporated CAR-T cells at a 1:4 E:T ratio (d) nucleofected vs hydroporated CAR-T cells at a 1:8 E:T ratio. (e) heat map showing significant differences in up and down-related protein changes in hydroporated CAR-T cells vs nucleofected CAR-T cells.


The team also looked at how nucleofection and hydroporation affected the cytokine profiles of CAR-T cells while they were killing cancer cells. Even after being deployed against cancer cells, hydroporated CAR-T cells showed statistically significant differences in cytokine profiles when compared to nucleofected CAR-T cells (Figure 7), meaning they were well-prepared and coordinated outside of a resting or native state.


For example, granzyme B (a factor used by T cells to kill target cells) is more highly expressed in CAR-T cells treated with hydroporation compared to nucleofection, especially when multiple target cells are present. Researchers have shown that inhibition of granzyme B can cause cancer cells to become resistant to T-cell-mediated killing, which can cause relapse in patients \[ **20** \].


#### In Summary


In this research, scientists demonstrated the efficacy of hydroporation and how it improves the cell modification process compared to the traditional standard of nucleofection. Overall, hydroporation shows promise as a potential method to improve CAR-T cell therapy's safety and effectiveness, paving the way for more efficient and safer treatments for cancer patients.


#### Future Work


To build upon these findings, the scientists are continuing to look for differences in cytokine profiles of CAR-T cells generated using the two separate methods. Additionally, scientists are in the process of comparing the performance of both types of CAR-T cells in vivo while also collecting additional cytokine profiles. These same methods are then being applied to chimeric antigen receptor regulatory T cells (CAR-Tregs) *in vitro* and *in vivo* .


#### References


1. Zhang, C., Liu J., Zhong J.F., and Zhang X.. 2017. Engineering CAR-T cells. *Biomarke Res.* 5:22. 10.1186/s40364-017-0102-y


2. Mardiana, S., Solomon, B. J., Darcy, P. K. & Beavis, P. A. Supercharging adoptive T cell therapy to overcome solid tumor-induced immunosuppression. *Sci. Transl. Med.* 11, eaaw2293 (2019).


3. Melenhorst JJ, Chen GM, Wang M, Porter DL, Chen C, Collins MA, Gao P, Bandyopadhyay S, Sun H, Zhao Z, Lundh S, Pruteanu-Malinici I, Nobles CL, Maji S, Frey NV, Gill SI, Loren AW, Tian L, Kulikovskaya I, Gupta M, Ambrose DE, Davis MM, Fraietta JA, Brogdon JL, Young RM, Chew A, Levine BL, Siegel DL, Alanio C, Wherry EJ, Bushman FD, Lacey SF, Tan K, June CH. Decade-long leukaemia remissions with persistence of CD4+ CAR T cells. Nature. 2022 Feb;602(7897):503-509. doi: 10.1038/s41586-021-04390-6. Epub 2022 Feb 2. Erratum in: Nature. 2022 Dec;612(7941):E22. PMID: 35110735; PMCID: PMC9166916.


4. Braendstrup, P., Levine, B. L. & Ruella, M. The long road to the first FDA-approved gene therapy: chimeric antigen receptor T cells targeting CD19. *Cytotherapy* 22, 57–69 (2020).


5. Brudno, J. N. & Kochenderfer, J. N. Recent advances in CAR T-cell toxicity: mechanisms, manifestations and management. *Blood Rev.* 34, 45–55 (2019).


6. Hay, K. A. et al. Kinetics and biomarkers of severe cytokine release syndrome after CD19 chimeric antigen receptor–modified T-cell therapy. *Blood* 130, 2295–2306 (2017)


7. Chou CK, Turtle CJ. Assessment and management of cytokine release syndrome and neurotoxicity following CD19 CAR-T cell therapy. Expert Opin Biol Ther. 2020 Jun;20(6):653-664. doi: 10.1080/14712598.2020.1729735. Epub 2020 Feb 24. PMID: 32067497; PMCID: PMC7393694.


8. Schumann, K., Lin S., Boyer E., Simeonov D.R., Subramaniam M., Gate R.E., Haliburton G.E., Ye C.J., Bluestone J.A., Doudna J.A., and Marson A.. 2015. Generation of knock-in primary human T cells using Cas9 ribonucleoproteins. *Proc. Natl. Acad. Sci. USA* . 112:10437–10442. 10.1073/pnas.1512503112


9. Singh, N., Shi J., June C.H., and Ruella M.. 2017. Genome-editing technologies in adoptive T cell immunotherapy for cancer. *Curr. Hematol. Malig. Rep.* 12:522–529. 10.1007/s11899-017-0417-7


10. Hu B, Zou Y, Zhang L, Tang J, Niedermann G, Firat E, Huang X, Zhu X. Nucleofection with Plasmid DNA for CRISPR/Cas9-Mediated Inactivation of Programmed Cell Death Protein 1 in CD133-Specific CAR T Cells. Hum Gene Ther. 2019 Apr;30(4):446-458. doi: 10.1089/hum.2017.234. Epub 2018 Apr 27. PMID: 29706119.


11. Thiel C, Nix M. Efficient transfection of primary cells relevant for cardiovascular research by nucleofection. Methods Mol Med. 2006;129:255-66. doi: 10.1385/1-59745-213-0:255. PMID: 17085816.


12. Zhang, J., Hu, Y., Yang, J. *et al.* Non-viral, specifically targeted CAR-T cells achieve high safety and efficacy in B-NHL. *Nature* 609, 369–374 (2022).[https://doi.org/10.1038/s41586-022-05140-y](https://doi.org/10.1038/s41586-022-05140-y)


13. Chicaybam L., Sodre A.L., Curzio B.A., Bonamino M.H. An Efficient Low Cost Method for Gene Transfer to T Lymphocytes. *PLoS ONE.* 2013;8:e60298. doi: 10.1371/journal.pone.0060298.


14. Zhang M., Ma Z., Selliah N., Weiss G., Genin A., Finkel T.H., Cron R.Q. The impact of Nucleofection® on the activation state of primary human CD4 T cells. *J. Immunol. Methods.* 2014;408:123–131. doi: 10.1016/j.jim.2014.05.014.


15. Jarrell, J.A., Sytsma, B.J., Wilson, L.H. *et al.* Numerical optimization of microfluidic vortex shedding for genome editing T cells with Cas9. *Sci Rep* **11** , 11818 (2021).[https://doi.org/10.1038/s41598-021-91307-y](https://doi.org/10.1038/s41598-021-91307-y)


16. Porter DL, Levine BL, Kalos M, Bagg A, June CH . Chimeric antigen receptor–modified T cells in chronic lymphoid leukemia. *New Engl J Med* 2011; 365: 725–733.


17. Rafiq, S., Hackett, C.S. & Brentjens, R.J. Engineering strategies to overcome the current roadblocks in CAR T cell therapy. *Nat Rev Clin Oncol* **17** , 147–167 (2020). https://doi.org/10.1038/s41571-019-0297-y


18. Huan T, Chen D, Liu G, Zhang H, Wang X, Wu Z, Wu Y, Xu Q, Yu F. Activation-induced cell death in CAR-T cell therapy. Hum Cell. 2022 Mar;35(2):441-447. doi: 10.1007/s13577-022-00670-z. Epub 2022 Jan 15. PMID: 35032297.


19. Milad D, et. al. nELISA: A high-throughput, high-plex platform enables quantitative profiling of the secretome. bioRxiv 2023.04.17.535914; doi:[https://doi.org/10.1101/2023.04.17.535914](https://www.biorxiv.org/content/10.1101/2023.04.17.535914v2)


20. Kimman T, Slomp A, Martens A, Grabherr S, Li S, van Diest E, Meeldijk J, Kuball J, Minnema MC, Eldering E, Bovenschen N, Sebestyén Z, Peperzak V. Serpin B9 controls tumor cell killing by CAR T cells. J Immunother Cancer. 2023 Mar;11(3):e006364. doi: 10.1136/jitc-2022-006364. PMID: 36931661; PMCID: PMC10030924.
