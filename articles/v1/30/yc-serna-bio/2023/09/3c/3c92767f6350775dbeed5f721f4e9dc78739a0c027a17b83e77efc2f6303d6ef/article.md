---
schema_version: "1.0.0"
document_id: "3c92767f6350775dbeed5f721f4e9dc78739a0c027a17b83e77efc2f6303d6ef"
company_key: "yc-serna-bio"
company: "Serna Bio"
source_id: "yc-serna-bio-news-import-10fe1061b09a"
canonical_url: "https://www.serna.bio/blog-post/star-small-molecules-targeting-rna-defining-the-chemical-rules-that-govern-rna-binders"
published_at: "2023-09-24T00:00:00+00:00"
first_seen_at: "2026-07-25T23:35:45.687739+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:687777805d5d415a850bdf8ab1d8574e2e7072ac2e809b2dc3b1ff4e60e10527"
---

# STaR: Small Molecules Targeting RNA: Defining the Chemical Rules that Govern RNA Binders

UPDATE: This work is published[here](https://www.biorxiv.org/content/10.1101/2024.01.31.578268v1)


The physicochemical rules of thumb governing drug-likeness published by[Lipinski](https://www.sciencedirect.com/science/article/abs/pii/S0169409X00001290?via%3Dihub) ,[Veber](https://pubs.acs.org/doi/10.1021/jm020017n) and[Ghose](https://pubs.acs.org/doi/10.1021/cc9800071) are well-known and commonly used in the spheres of cheminformatics, medicinal chemistry and drug discovery. These rules undoubtedly have their weaknesses, and a number of publications have emerged referencing[drug discovery beyond the rule of five (including noting that around half of the orally used FDA-approved small molecule drugs do not comply with the rule of five)](https://www.sciencedirect.com/science/article/abs/pii/S0958166907001279) particularly[in the area of PROTACs](https://www.sciencedirect.com/science/article/abs/pii/S0960894X19302495) . However, their[impact on the pharmaceutical industry cannot be understated](https://www.tandfonline.com/doi/full/10.1080/17460441.2023.2228199) . At Serna Bio we wondered - could we use the wealth of RNA Binder data that we have collected as a jumping-off point to try and create our own rules of thumb for chemical space enriched with RNA binders?


To do this we began by gathering together our own data along with publicly available RNA binding data from[R-BIND](https://pubs.acs.org/doi/10.1021/acschembio.2c00224) ,[DRTL](https://www.biorxiv.org/content/10.1101/2023.07.31.551350v1) and[ROBIN](https://onlinelibrary.wiley.com/doi/10.1002/anie.202211358) . In addition we would like to acknowledge the contributions of[the Inforna platform](https://pubs.acs.org/doi/abs/10.1021/acschembio.6b00001) to the landscape of RNA-small molecule interactions but were unable to access compounds in this dataset to be added to this study.


Using publicly available datasets and the Serna Bio dataset, we then conducted a comprehensive physicochemical analysis, assessing how the distributions of calculated physicochemical properties changed between RNA binders and non-binders. By mapping out these distributions for our own data we were able to build a picture of which properties generally increase in RNA binding small molecules and which decrease - using a high bar of statistical significance to prevent a very high proportion of the properties appearing statistically significant. This was the case in[R-BIND where 20 properties were compared and 18 were found to be statistically significantly different](https://pubs.acs.org/doi/10.1021/acschembio.2c00224) .


Taking those properties that look to differentiate RNA binders and non-binders most effectively, a statistical grid search involving the testing of different combinations of physicochemical properties at different thresholds was implemented to explore how different combinations of the most informative properties enriched chemical space for RNA binders using the Serna Bio dataset - the largest single dataset we have access to, containing nearly 200,000 unique small molecules. The most promising rules were then rounded to form rules of thumb that could be applied to the public data to examine their generalizability. The rules we established for Small molecules Targeting RNA (STaR rules) are as follows:


- As a rule of thumb, RNA Binders have at least two of the following four properties:


- CLogP ≥ 1.5
- Molar Refractivity ≥ 130
- Number of Aromatic Rings ≥ 4
- Relative Polar Surface Area ≤ 0.30


*Figure 1 - Radar plots showing the change in physicochemical properties between RNA binders and non-binders in the datasets R-BIND 2.0 (upper left, yellow), DRTL (upper right, turquoise), ROBIN (lower left, purple) and Serna Bio (lower right, red). In these plots, an extension of the line to the edge of the circle indicates a statistically significant increase in that property in RNA binders compared to non-binders, while a contraction to the centre indicates a statistically significant decrease in that property in RNA binders compared to non-binders. Here, statistically significant changes must involve a p-value < 0.01 and a change in median value for the physicochemical property in question using Mood’s median test with Benjamini-Hochberg correction.*


The chemical space carved out by the STaR rules contained more than 50% of the Serna Bio RNA binders, but only 29% of the non-binders. For ROBIN this space contained 34% of the RNA binders and only 26% of the non-binders, and for R-BIND and DRTL 46% and 65% respectively of the RNA Binders in those datasets passed the STaR rules. The RNA-binding approved drug[Risdiplam](https://pubs.acs.org/doi/10.1021/acs.jmedchem.8b00741) also conforms to the STaR rules. These results gave us confidence that we had been able to carve out an area of chemical space that was enriched for RNA binders and was somewhat generalizable.


We emphasize that, like Lipinski’s Rule of Five for druglike compounds, our STaR rules-of-thumb describe physicochemical tendencies of RNA binders rather than strict, dichotomous cutoffs. Proven RNA binders need not pass any of these rules, though compounds which do are more likely to bind RNA. Some of the aspects of the STaR rules align well with existing public information on the characteristics of RNA binding small molecules. For example,[R-BIND noted that G-quadruplex binding ligands have a high number of aromatic rings](https://pubs.acs.org/doi/10.1021/acschembio.2c00224) , and[the RNA binders identified in the DRTL study tended to have relatively high CLog P](https://www.biorxiv.org/content/10.1101/2023.07.31.551350v1) . In the work we conducted as part of the ROBIN study,[descriptors associated with higher numbers of aromatic rings were associated with RNA binders](https://onlinelibrary.wiley.com/doi/10.1002/anie.202211358) . Shared conclusions such as these provide confidence that the STaR rules are based on some sort of ground truth for RNA binders. This is particularly important given that RNA-targeting small molecule drug discovery is a relatively data-poor field.


To round out our study, a final objective was to consider how the chemical space we have carved out interacts with approved drugs. To do this, we applied the STaR rules to a[commercially available approved drug compound library](https://www.selleckchem.com/screening/fda-approved-drug-library.html) and found that around one in three of those molecules pass - suggesting this area of chemical space is drug-like. Furthermore, the rule we have identified that intersects with the Rule of Five concerns CLogP, and as the Rule of Five indicates that compounds having CLogP > 5.0 are less likely to be druglike while our rule indicates that compounds with CLogP ≥ 1.5 are more likely to be RNA binders, this leaves a considerable amount of space for compounds to pass both the STaR rules and the Rule of Five.


The STaR rules that we have devised are part of a larger effort at Serna Bio to map the druggable transcriptome and understand the chemistry driving functional RNA small molecule interactions. The above rules represent a rapidly and easily calculable set of guidelines for RNA binding chemical space and furthermore are in some cases intuitive for a chemist to consider when designing a new RNA targeting molecule. We are keen to encourage feedback and further discussion on this topic given the growing interest in the field. We believe that through the discovery of these rules, and other structural and machine learning insights, we can build a solid foundation for our drug discovery campaigns targeting RNA.
