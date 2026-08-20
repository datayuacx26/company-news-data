---
schema_version: "1.0.0"
document_id: "10cc77e1e44b0674147f5bae4fc9034b33160b6f5b21efd5f9061d8f55a886d0"
company_key: "yc-future-fields"
company: "Future Fields"
source_id: "yc-future-fields-rss-02d8ed225558"
canonical_url: "https://futurefields.io/overview-of-recombinant-protein-expression-systems/"
published_at: "2024-06-20T21:30:00+00:00"
first_seen_at: "2026-07-25T03:17:21.494318+00:00"
fetched_at: "2026-08-14T19:46:00.946955+00:00"
content_hash: "sha256:da1dd8d0934dd37149ba15976c8adf2d28f825bf65c09a8d8692e6d3eb510551"
---

# Overview of Recombinant Protein Expression Systems

[Flylab](https://futurefields.io/category/flylab/)


06/20/2024


# Overview of Recombinant Protein Expression Systems


Diving into today’s expression technologies to address production of difficult-to-express proteins at scale.


Contributors: Ela Dudek PhD, Paige Grant MSc, Diane Jeon BCom, Anzer Khan PhD, Laine Lysyk MSc, Tautvydas Paskevicius PhD, Simon Wu MSc


---


Recombinant proteins represent a cornerstone of modern biotechnology, playing pivotal roles across medical, industrial, and research sectors. These proteins, meticulously engineered through genetic manipulation, offer tailored functionalities ranging from therapeutics to industrial enzymes and research tools. Their importance lies not only in their specificity and efficacy but also in their scalability and accessibility, enabling large-scale production to meet global demands.


In the world of therapeutics and complex biologics, selecting the **appropriate expression host** presents a unique two-fold challenge. Drug developers and researchers are asking:


- Can this expression system **successfully produce my complex protein target?**


- Can the expression system **produce my protein economically at scale?**


Navigating difficult-to-express protein production starts with understanding the spectrum of intrinsic properties that make a protein inherently difficult. These properties include folding, post-translational modifications, multi-subunit complex assembly, solubility, and toxicity, which we covered in a previous article.


Scalability stands as another critical criterion in choosing a protein expression platform. Industrial applications often demand high yields of pure proteins at a reasonable cost, necessitating systems capable of scaling production without compromising product quality or yield consistency. The ability to seamlessly transition from bench-scale research to large-scale manufacturing ensures that promising proteins can progress from concept to commercialization effectively.


The current landscape of recombinant protein production has a spectrum of platforms designed to optimize expression based on specific needs. From the pioneering simplicity of *Escherichia coli (E. coli)* to the sophisticated glycosylation capabilities of mammalian cells, each platform brings unique strengths and challenges to the table. Yeast offers robust protein folding and secretion pathways, while cell-free systems provide flexibility and rapid prototyping capabilities. Plants and insects also contribute unique advantages, such as the ability to perform complex post-translational modifications.


In this article, we will explore each expression platform and their unique strengths, including our own paradigm-shifting recombinant protein technology that fills in the gaps presented in status quo systems today.


## **Protein Expression Systems**


1. *E. coli*
2. Yeast
3. Mammalian Cells
4. Cell-free Expression Systems
5. Plant Cells
6. Insect Cells
7. Transgenic Insects (EntoEngine **** )


Recombinant Protein Expression in *E. coli*


*Escherichia coli* , a gram-negative bacterium, is foundational to recombinant protein expression systems. Its pioneering role began in the late 1970s with the production of the first recombinant protein, shortly followed by the approval of the[first recombinant protein](https://www.science.org/doi/10.1126/science.412251) therapeutic,[E. coli -produced insulin](https://www.fda.gov/about-fda/fda-history-exhibits/100-years-insulin) . This milestone drove the growth of recombinant protein biotechnology and to this day, *E. coli* remains the[most widely used system](https://www.sciencedirect.com/science/article/pii/S2666166723005397?via%3Dihub) for protein expression due to ongoing adaptations and refinements.


Protein expression in *E. coli* is characterized by its simplicity and offering cost advantages compared to alternative platforms. *E. coli* ‘s rapid growth, with a doubling time as short as[20 minutes](https://onlinelibrary.wiley.com/doi/full/10.1002/pro.3668) , facilitates swift and[high-yield](https://star-protocols.cell.com/protocols/3094) production of recombinant proteins, making it a preferred choice for both research and industrial applications.


A rich array of tools supports protein expression in *E. coli* , prominently including expression vectors and specialized bacterial strains. Expression vectors are equipped with diverse elements such as promoters, tags, and antibiotic resistance markers, enabling precise control over protein expression levels, enhancing solubility, and streamlining purification processes. Among these, the BL21(DE3) strain stands out as the gold standard, featuring the phage T7 RNA polymerase under the lacUV5 promoter. This configuration allows for robust protein expression that can be induced by IPTG, independent of bacterial growth. Numerous derivatives of BL21(DE3) have since been developed to cater to specific expression needs. For instance, BL21(DE3) pLysS minimizes basal expression levels, ideal for toxic proteins, while strains like Rosetta and RI(P)L facilitate handling of rare codons found in mammalian proteins and promote disulfide bond formation. Additionally, engineered strains offer tunable expression levels and co-expression of chaperones to aid in proper protein folding, underscoring the versatility of the *E. coli* system for diverse expression demands.


Despite these advancements, *E. coli* is not universally suitable for all protein types due to challenges in achieving precise post-translational modifications (PTMs), particularly[glycosylation](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3449280/) . Current efforts to enhance glycosylation in *E. coli* still encounter limitations such as low efficiency or the necessity for[additional enzymatic interventions](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7923244/) ex vivo, highlighting areas for further improvement. Moreover, proteins larger than[60 kDa](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4029002/) often face low yields and solubility issues in *E. coli* , while many mammalian and transmembrane proteins struggle with proper folding and solubility. Although strategies like temperature reduction, chaperone co-expression, and solubility tags can mitigate these challenges in some cases, they do not always resolve them. Instead, these proteins often aggregate into inclusion bodies, simplifying purification but complicating subsequent refolding processes, which often yield low quantities of improperly folded proteins.


In conclusion, *Escherichia coli* remains a foundational platform in recombinant protein production, renowned for its accessibility, cost-effectiveness, and rapid growth. While continuously evolving, it thrives as an ideal system for expressing simpler proteins and serves as a benchmark against which other expression systems are measured. In particular, *E. coli* excels in expressing small, globular, single-chain proteins that do not require complex PTMs. However, the pursuit of new technologies and strategies is crucial to address the inherent limitations of *E. coli* and advance the field of protein biotechnology further. For more intricate proteins, alternative expression systems should be considered.


Recombinant Protein Expression in Yeast


Yeasts are widely utilized as hosts for recombinant protein expression due to their unique combination of characteristics; they blend the simplicity of unicellular prokaryotic hosts with the complex cellular machinery typical of eukaryotes. *Saccharomyces cerevisiae* and *Pichia pastoris* (now reclassified as *Komagataella* spp.) are the most commonly employed yeasts for heterologous protein production. These yeasts offer advantages similar to bacterial systems, such as rapid growth, facile genetic manipulation, and cost-effective media requirements. A distinguishing feature of yeast-based expression systems compared to E. coli is their capability to perform post-translational modifications (PTMs), like glycosylation and disulfide bond formation. This ability is crucial for producing proteins with intricate structures or those requiring specific PTMs for proper function, such as in therapeutic proteins.


Historically, *S. cerevisiae* has been the primary yeast for recombinant protein production due to its suitability for large-scale industrial applications and its proficiency in producing biologically active proteins that can be secreted. However, *P. pastoris* has gained favor in recent years for its capacity to achieve high cell densities, thereby increasing protein yields. *P. pastoris* , a methylotrophic yeast, is notable for its ability to utilize methanol as a sole carbon source, facilitated by its methanol-inducible AOX1 promoter. This promoter allows tight control over protein expression, with the target protein potentially constituting up to[30% of total cellular protein](https://www.sciencedirect.com/science/article/abs/pii/S1001841717304849?via%3Dihub) , making *P. pastoris* highly efficient for expression purposes. Alternative promoters, both inducible and constitutive, offer flexibility for optimizing expression, especially for challenging targets or toxic proteins.


Yeast-based expression benefits from stable integration of the target gene into the yeast genome, ensuring consistent expression across clones. Industrial fermentation capabilities of *P. pastoris* support robust growth and high yields of both intracellular and secreted recombinant proteins. This system is cost-effective and faster than many other eukaryotic systems, making it advantageous for commercial production of secreted proteins, which simplifies downstream processing.


Glycosylation is a critical aspect of protein expression in yeast, influencing efficacy, half-life, and antigenicity of therapeutic proteins. *P. pastoris* can perform both[O- and N-linked glycosylation](https://pubmed.ncbi.nlm.nih.gov/10574687/) , albeit with[hypermannose](https://www.mdpi.com/2076-2607/6/2/38#B81-microorganisms-06-00038) structures that differ somewhat from human glycosylation patterns. Efforts are ongoing to refine glycan structures towards more human-like patterns, aiming to mitigate potential immunogenicity associated with hypermannosylation.


Producing challenging proteins in yeast requires strategic approaches such as codon optimization, signal sequence engineering, and co-expression of chaperones or folding catalysts to enhance expression and proper folding. Advances in[strain engineering and specialized expression vectors](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9730254/#B25) further improve efficiency and expand the versatility of yeast-based systems for diverse protein targets.


While yeast may not be suitable for all proteins, its cost-effectiveness, scalability, and genetic manipulability make it a compelling choice for many applications in recombinant protein production.


Recombinant Protein Expression in Mammalian Cell Culture


Expression of mammalian proteins in mammalian cells offers the distinct advantage of closely mimicking the protein’s native host system.[Mammalian cell cultures](https://core.ac.uk/reader/55612091?utm_source=linkout) are well suited for producing properly folded mammalian proteins with accurate post-translational modifications, especially for glycoproteins that are secreted, proteins larger than 60 kDa, and multi-subunit protein complexes. Often, mammalian cells are chosen for proteins intended for secretion into the growth media. Since cell lysis is unnecessary and growth media composition is simpler compared to cell lysates, downstream processing of the protein of interest can be streamlined.


There are[two primary strategies](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3394691/) for expressing proteins in mammalian cells: transient transfection and stable transfection. In transient transfection, the DNA encoding the protein of interest remains as an extrachromosomal element and is rapidly lost during cellular replication. In contrast, stable transfection involves integration of the DNA into the cell’s genome, ensuring its retention through cellular replication. However, establishing a stable transfected cell line is a time-, resource-, and labor-intensive process, taking weeks or months to achieve protein production, whereas protein from transiently transfected cells can be obtained within[3-7 days](https://star-protocols.cell.com/protocols/3094) . Therefore, transient transfection is more commonly used in research and early-stage clinical trials where rapid protein production in milligram to gram quantities is crucial. For applications requiring sustained, high-volume, predictable protein expression—such as monoclonal antibody production for therapeutics—[stable transfection is preferred](https://core.ac.uk/reader/55612091?utm_source=linkout) .


The most frequently used mammalian cell lines for recombinant protein production include Chinese hamster ovary cells (CHO) and human embryonic kidney 293 cells (HEK-293). The biopharmaceutical industry predominantly relies on CHO cells due to their high protein yields, well-characterized genetics, scalability in suspension cultures, and ability to perform complex glycosylation crucial for therapeutic protein function. However, CHO cell cultures are costly, their glycosylation patterns may differ from human cells, and developing stable cell lines is time-consuming. HEK-293 cells are popular in research settings for producing recombinant proteins, particularly those requiring human-like post-translational modifications. HEK-293 cells are highly transfectable, proliferate rapidly, and perform human-like modifications, making them suitable for transient protein expression. On the other hand, HEK-293 cells often yield lower quantities than CHO cells, face stability issues with transient expression, and encounter challenges in scaling up for industrial production. Recent variants like HEK293F and Expi293F adapted for suspension culture aim to simplify scaling up protein production.


Recombinant protein production using mammalian expression systems presents several significant challenges. Firstly, these systems entail higher costs compared to *E. coli* or yeast due to specialized media, culture conditions, and longer production timelines. Secondly, developing stable mammalian cell lines for protein expression is laborious and time-intensive, involving clone selection based on thorough characterization of protein expression profiles, growth characteristics, and genetic stability to ensure suitability for large-scale production. Furthermore, while mammalian cells excel in complex post-translational modifications like glycosylation, achieving consistent PTMs can be difficult, leading to heterogeneous protein products. Additionally, mammalian cells typically yield lower amounts of recombinant proteins compared to microbial systems. Scaling up mammalian cell culture production with larger bioreactors and increased infrastructure is possible, but can further elevate operating costs for the developer.


Recombinant Protein Expression in Cell-Free Expression (CFE) Systems


A cell-free protein expression (CFE) system is a method used to synthesize proteins in vitro without the presence of living cells. It relies on transcription and translation machinery to produce proteins in a solution-based environment. Key components of the CFE system include template DNA or mRNA encoding the target protein, a cell lysate containing translation machinery, essential translational factors, and substrates such as amino acids and nucleotides, along with components for energy regeneration. CFE systems can be derived from various sources, such as bacterial cells (e.g., *E. coli* ), eukaryotic cells (e.g., rabbit reticulocyte, wheat germ, yeast, insect, or mammalian cells), and plant cells (e.g., tobacco).


Unlike living cell systems, CFE systems do not face biochemical constraints related to cell survival and growth. Instead, all metabolic and carbon resources in CFE are directed solely towards protein production, eliminating metabolic and cytotoxic burdens typical in living cell-based systems.


There are two main types of CFE systems: extract-based and enzyme-based (reconstituted) systems. Extract-based systems use crude extracts containing all necessary components for protein synthesis, offering simplicity in preparation. In contrast, enzyme-based or reconstituted systems are prepared using purified recombinant enzymes, tRNAs, ribosomes, amino acids, and energy molecules, allowing precise control over the protein synthesis environment and enhancing expression outcomes.


CFE systems are highly flexible and can be customized by adjusting the biochemical environment of protein expression. They accommodate a wide range of additives, including unnatural amino acids, liposomes, nanodiscs, detergents, metals, and other cofactors that are challenging to integrate into cell-based systems. This flexibility enables the co-expression of target proteins with cofactors, ligands, interaction partners, or chaperones to create tailored environments.


Despite their advantages in expressing difficult-to-express proteins, CFE systems have their share of drawbacks. They can be costly, especially for large-scale production, due to expensive reagents like purified enzymes and nucleotides. Furthermore, they often yield lower protein amounts compared to cell-based systems, and scaling up production is challenging due to limited reaction volumes and maintaining efficiency at larger scales. Optimization of CFE systems for each protein target is complex and time-consuming, requiring meticulous preparation of cell lysates and specific adjustments. Moreover, CFE systems may not accurately replicate certain post-translational modifications that occur in living cells, such as complex glycosylations. Stability is also a concern as enzymes and ribosomes can degrade over time, impacting production consistency. Maintaining optimal reaction conditions, including pH, temperature, and ionic strength, is critical but challenging.


Despite these challenges, CFE systems remain valuable for producing proteins that are challenging to express in living cells, offering unique advantages in biochemical customization and experimental flexibility.


Recombinant Protein Expression in Plant Cells


Recombinant protein expression in plants shows promising potential for producing diverse proteins, though it has not yet achieved broad commercial adoption. *Nicotiana benthamiana* , tobacco, maize, and rice are commonly used for this purpose,[offering advantages](https://www.frontiersin.org/articles/10.3389/fbioe.2019.00420/full) such as cost-effectiveness, scalability, safety (minimal animal pathogen contamination), and the ability to perform complex post-translational modifications like N-linked glycosylation. Plants, optimized over millennia for food and industrial use, boast high biomass yields, robust growth traits, and simplified harvesting, positioning them as highly capable hosts for heterologous protein expression.


Advancements in plant biotechnology have led to[diverse expression strategies](https://nph.onlinelibrary.wiley.com/doi/epdf/10.1002/ppp3.10073) , including stable or transient transfection, and utilizing various vectors, promoters, and plant species as hosts. Stable transfection enables continuous protein production on a large scale by integrating the gene of interest into the plant genome, albeit with laborious processes. Conversely, transient expression systems offer rapid protein production within weeks, ideal for screening conditions or meeting urgent demands. *N. benthamiana* -based systems are favored for their rapid growth, high biomass, and efficient protein secretion, though scalability limitations may hinder long-term or large-scale production.


Moreover, plants feature numerous subcellular compartments for targeted protein expression and are capable of performing intricate post-translational modifications. Precision in subcellular targeting ensures correct protein folding and modification, achievable through engineered signal peptides or localization signals directing proteins to organelles such as the endoplasmic reticulum or chloroplasts. Plant-produced glycosylation patterns differ from human patterns, necessitating glycoengineering efforts to align plant glycan structures with human counterparts, thereby enhancing therapeutic protein efficacy and reducing immunogenicity.


Despite these advantages,[challenges persist](https://www.frontiersin.org/journals/plant-science/articles/10.3389/fpls.2019.00720/full) , including regulating transgene expression levels, ensuring proper protein folding and stability, and navigating complex downstream processing. Economic viability is further complicated by challenges in extracting recombinant proteins from plant material and eliminating insoluble debris and host cell proteins. Regulatory hurdles and concerns over environmental containment and contamination also impede widespread adoption of plant-based systems. Nonetheless, ongoing research and advancements in plant biotechnology hold promise for enhancing the efficiency and scalability of these systems, particularly in producing difficult-to-express proteins.


Recombinant Protein Expression in Insect Cells


[Insect cells](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8707663/) are versatile hosts for expressing recombinant proteins, excelling in producing complex intracellular and viral proteins due to their robust protein-folding and high culture densities. Proteins from insect cells are used in structural elucidation, drug design, assay development, and diagnostic reagents.Unlike bacterial systems, insect cells utilize protein secretion and processing pathways similar to mammals, predominantly through the Golgi and ER, ensuring correct folding and activity for most proteins produced. Additionally, insect cells are[proficient in various post-translational modifications](https://pubmed.ncbi.nlm.nih.gov/28232040/) (PTMs) like phosphorylation, acetylation, and palmitoylation. Their glycosylation patterns, simpler and more uniform than those of mammalian cells, enhance their appeal.


Common insect cell lines used for recombinant protein production include Sf9, Hi-5, Sf21, and S2 cells, typically employed in plasmid-based or baculovirus-mediated expression systems. In plasmid-based expression, a gene-carrying plasmid is introduced directly into insect cells, though it faces limitations due to plasmid dilution over successive generations.[Baculovirus-mediated expression](https://www.nature.com/articles/nbt1095) , on the other hand, capitalizes on baculoviruses’ natural ability to infect and replicate within insect cells. Here, the gene of interest is integrated into a baculovirus genome (bacmid), which is then used to infect host cells such as Sf9 or Sf21, facilitating concurrent viral replication and protein production.


This approach offers significant advantages for producing high molecular weight proteins by accommodating large DNA inserts into the baculovirus genome and enabling co-expression via systems like MultiBac. Insect cells utilized in this method ensure that proteins undergo essential PTMs, achieving proper folding and maintaining biological functionality.


While insect cell-based expression systems yield substantial quantities of proteins, making them appealing for industrial applications, they pose challenges such as high production costs associated with specialized media, equipment, and extended cultivation times. Moreover, distinct glycosylation patterns in insect cells compared to mammals can influence protein characteristics, including structure, function, and potential immunogenicity. Scaling production requires significant investment and infrastructure, similar to mammalian cell-based systems, as well as careful optimization to enhance efficiency and cost-effectiveness.


Recombinant Protein Expression in Transgenic Insects (the EntoEngine)


Conventional methods have advanced recombinant protein production, leading to numerous breakthroughs. However, the six mentioned expression platforms have limitations. *E. coli* and yeast excel with simple proteins but *E. coli* and yeast excel with simple proteins but have challenges producing more complex ones. Mammalian and insect cells can handle complex proteins but may become cost-inefficient at larger scales. Despite similarities in protein secretory pathways between plants and mammals, plant cells face challenges with low yields and reduced biological activity of recombinant proteins. Cell-free systems also struggle with yield and cost when scaled up.


This is why Future Fields developed the EntoEngine. Leveraging transgenic *Drosophila melanogaster* , the EntoEngine uses a whole-insect approach to produce stable strains for cost-efficient production of active, difficult-to-express-proteins at scale.


### Genetic Advantages


*Drosophila melanogaster* are extensively studied for their genetics and developmental biology. They serve as ideal model organisms due to their short life cycle, ease of maintenance in labs, and well-characterized genetic pathways. Despite their simpler genome compared to mammals,[fruit flies](https://bmcbiol.biomedcentral.com/articles/10.1186/1741-7007-11-38) share genetic and biological pathways similar to humans, making them invaluable for studying fundamental biological processes like development, behavior, and neurobiology. Over the past century, the *Drosophila* research community has developed sophisticated genetic tools enabling precise manipulation and expression of foreign genes, enhancing their role in biological research and more complex applications.


In *Drosophila* , the UAS/GAL4 system is widely used for gene expression, offering precise temporal and spatial control over gene activity. This system enables targeted gene expression in specific tissues or developmental stages. Additionally, inducible promoters can regulate gene expression in response to specific chemicals or environmental cues. Future Fields EntoEngine leverages these genetic tools in combination with proprietary genetic innovations to optimize expression of complex production strains.


### Complex Protein Production at Scale


A significant advantage of using *Drosophila* for protein production lies in its capability to perform complex post-translational modifications, including glycosylation, similar to mammalian cells. This feature is crucial for producing proteins that require specific modifications for correct folding and biological function, such as therapeutic proteins. Moreover, *Drosophila* allows for cost-effective scalability, with large numbers of flies easily bred and maintained in laboratory conditions, facilitating industrial-scale protein production. In the case of the EntoEngine, Future Fields’ proprietary modular rearing system allows for flexibility and on-demand production of proteins, from milligram to gram quantities. As this expression system does not depend on bioreactor capacity, operating costs are significantly reduced, passing on cost-savings to the developer. In addition, our approach to inducibility allows for potentially complex and cytotoxic proteins to be expressed cost-efficiently. Altogether, these advantages present favorable conditions for those seeking difficult-to-express protein production at scale.


The EntoEngine is primed for economic and larger scale production of difficult-to-express proteins, but no expression methodology can do it all. Differences in PTMs compared to mammalian cells could potentially affect protein efficacy and immunogenicity. While the system’s stable-strain approach has advantages, the associated production timeline may take longer than the highly optimized *E. coli* and yeast expression systems or other transient systems. Finally, protein extraction is more complicated than cell-based systems.


Future Fields’ proprietary innovations circumvent the potential challenges with using a whole-insect expression system. In sum, the genetic tools, biological similarities to humans, PTM capabilities, and ease of scalability positions the EntoEngine as an optimal expression technology in situations where conventional systems may fail.


Summary of Recombinant Protein Expression Systems


The future of recombinant protein production is dynamic and promising, driven by continuous innovation and adaptation across multiple platforms. As technology evolves, so too will our ability to engineer proteins with precision and efficiency, paving the way for new treatments, sustainable industrial practices, and groundbreaking scientific discoveries. The journey from laboratory innovation to real-world impact is propelled by the diverse capabilities of these expression systems, ensuring that recombinant proteins remain at the forefront of transformative biotechnological advancements.


Protein expression systems have evolved significantly since *E. coli* ‘s pioneering days, offering a spectrum of platforms each tailored to specific protein production needs. From the simplicity and cost-effectiveness of *E. coli* to the glycosylation capabilities of yeast, the fidelity of mammalian cells to the flexibility of cell-free systems, and the potential of plants and insects, each system brings unique strengths and challenges to the table. Future innovations, such as those leveraging *Drosophila melanogaster* for protein production, continue to push the boundaries of what’s possible in recombinant protein technology, ensuring a diverse toolkit for researchers and industries alike.


As we delve deeper into the diverse landscape of protein expression systems, each with its unique set of strengths and challenges, the EntoEngine stands poised to redefine the boundaries of biotechnology and pharmaceutical development by enabling difficult-to-express protein production at scale. By offering a robust platform that complements existing methods, the EntoEngine broadens horizons for protein scientists to solve industry challenges in biopharma. The EntoEngine holds tremendous promise for advancing therapeutic protein production and meeting the evolving demands of biomedical research and industrial applications.


---


## More from Future Fields


[Flylab](https://futurefields.io/category/flylab/)


04/17/2024


### [The Art of Protein Expression: A Guide to Understanding Difficult-to-Express Proteins](https://futurefields.io/the-art-of-protein-expression-a-guide-to-understanding-difficult-to-express-proteins/)


[Read More](https://futurefields.io/the-art-of-protein-expression-a-guide-to-understanding-difficult-to-express-proteins/)


[Flylab](https://futurefields.io/category/flylab/)


10/29/2024


### [From Classic Genetics to Cutting-Edge Biotech: Exploring the Genetic Tools Enabling Protein Expression in Fruit Flies](https://futurefields.io/from-classic-genetics-to-cutting-edge-biotech-exploring-the-genetic-tools-enabling-protein-expression-in-fruit-flies/)


[Read More](https://futurefields.io/from-classic-genetics-to-cutting-edge-biotech-exploring-the-genetic-tools-enabling-protein-expression-in-fruit-flies/)


---


## Sources


1. Bretthauer RK, Castellino FJ. Glycosylation of Pichia pastoris-derived proteins. Biotechnol Appl Biochem. 1999 Dec;30(3):193-200. PMID: 10574687.
2. Brookwell A, Oza JP, Caschera F. Biotechnology Applications of Cell-Free Expression Systems. Life (Basel). 2021 Dec 8;11(12):1367. doi: 10.3390/life11121367. PMID: 34947898; PMCID: PMC8705439.
3. Burnett MJB, Burnett AC.Therapeutic recombinant protein production in plants:Challenges and opportunities. Plants, People, Planet.2020;2:121–132.[https://doi.org/10.1002/ppp3.10073](https://doi.org/10.1002/ppp3.10073)
4. Cox MMJ. Innovations in the Insect Cell Expression System for Industrial Recombinant Vaccine Antigen Production. Vaccines (Basel). 2021 Dec 20;9(12):1504. doi: 10.3390/vaccines9121504. PMID: 34960250; PMCID: PMC8707663.
5. Garenne, D., Haines, M.C., Romantseva, E.F. et al. Cell-free gene expression. Nat Rev Methods Primers 1, 49 (2021). https://doi.org/10.1038/s43586-021-00046-x
6. Gregorio NE, Levine MZ, Oza JP. A User’s Guide to Cell-Free Protein Synthesis. Methods Protoc. 2019 Mar 12;2(1):24. doi: 10.3390/mps2010024. PMID: 31164605; PMCID: PMC6481089.
7. [https://www.fda.gov/about-fda/fda-history-exhibits/100-years-insulin](https://www.fda.gov/about-fda/fda-history-exhibits/100-years-insulin)
8. Keiichi Itakura et al. ,Expression in Escherichia coli of a Chemically Synthesized Gene for the Hormone Somatostatin. Science198,1056-1063(1977). DOI:10.1126/science.412251
9. Khambhati K, Bhattacharjee G, Gohil N, Braddick D, Kulkarni V, Singh V. Exploring the Potential of Cell-Free Protein Synthesis for Extending the Abilities of Biological Systems. Front Bioeng Biotechnol. 2019 Oct 11;7:248. doi: 10.3389/fbioe.2019.00248. PMID: 31681738; PMCID: PMC6797904.
10. Kost, T., Condreay, J. & Jarvis, D. Baculovirus as versatile vectors for protein expression in insect and mammalian cells. Nat Biotechnol 23, 567–575 (2005).[https://doi.org/10.1038/nbt1095](https://doi.org/10.1038/nbt1095)
11. Mason M, Sweeney B, Cain K, Stephens P, Sharfstein ST. Identifying bottlenecks in transient and stable production of recombinant monoclonal-antibody sequence variants in Chinese hamster ovary cells. Biotechnol Prog. 2012 May-Jun;28(3):846-55. doi: 10.1002/btpr.1542. Epub 2012 May 21. PMID: 22467228; PMCID: PMC3394691.
12. Pan Y, Yang J, Wu J, Yang L, Fang H. Current advances of Pichia pastoris as cell factories for production of recombinant proteins. Front Microbiol. 2022 Nov 24;13:1059777. doi: 10.3389/fmicb.2022.1059777. PMID: 36504810; PMCID: PMC9730254.
13. Prabhu SK, Yang Q, Tong X, Wang LX. Exploring a combined Escherichia coli-based glycosylation and in vitro transglycosylation approach for expression of glycosylated interferon alpha. Bioorg Med Chem. 2021 Mar 1;33:116037. doi: 10.1016/j.bmc.2021.116037. Epub 2021 Jan 22. PMID: 33515919; PMCID: PMC7923244.
14. Rajan, A., Perrimon, N. Of flies and men: insights on organismal metabolism from fruit flies. BMC Biol 11, 38 (2013).[https://doi.org/10.1186/1741-7007-11-38](https://doi.org/10.1186/1741-7007-11-38)
15. Rita Costa A, Elisa Rodrigues M, Henriques M, Azeredo J, Oliveira R. Guidelines to cell engineering for monoclonal antibody production. Eur J Pharm Biopharm. 2010 Feb;74(2):127-38. doi: 10.1016/j.ejpb.2009.10.002. Epub 2009 Oct 22. PMID: 19853660.
16. Rosano GL, Ceccarelli EA. Recombinant protein expression in Escherichia coli: advances and challenges. Front Microbiol. 2014 Apr 17;5:172. doi: 10.3389/fmicb.2014.00172. PMID: 24860555; PMCID: PMC4029002.
17. Rosano, G.L., Morales, E.S. and Ceccarelli, E.A. (2019), New tools for recombinant protein production in Escherichia coli: A 5-year update. Protein Science, 28: 1412-1422.[https://doi.org/10.1002/pro.3668](https://doi.org/10.1002/pro.3668)
18. Schillberg S, Raven N, Spiegel H, Rasche S and Buntru M (2019) Critical Analysis of the Commercial Potential of Plants for the Production of Recombinant Proteins. Front. Plant Sci. 10:720. doi: 10.3389/fpls.2019.00720
19. Schütz A, Bernhard F, Berrow N, Buyel JF, Ferreira-da-Silva F, Haustraete J, van den Heuvel J, Hoffmann JE, de Marco A, Peleg Y, Suppmann S, Unger T, Vanhoucke M, Witt S, Remans K. A concise guide to choosing suitable gene expression systems for recombinant protein production. STAR Protoc. 2023 Dec 15;4(4):102572. doi: 10.1016/j.xpro.2023.102572. Epub 2023 Oct 31. PMID: 37917580; PMCID: PMC10643540.
20. Tan E, Chin CSH, Lim ZFS, Ng SK. HEK293 Cell Line as a Platform to Produce Recombinant Proteins and Viral Vectors. Front Bioeng Biotechnol. 2021 Dec 13;9:796991. doi: 10.3389/fbioe.2021.796991. PMID: 34966729; PMCID: PMC8711270.
21. Tripathi NK and Shrivastava A (2019) Recent Developments in Bioprocessing of Recombinant Proteins: Expression Hosts and Process Development. Front. Bioeng. Biotechnol. 7:420. doi: 10.3389/fbioe.2019.00420
22. Valderrama-Rincon JD, Fisher AC, Merritt JH, Fan YY, Reading CA, Chhiba K, Heiss C, Azadi P, Aebi M, DeLisa MP. An engineered eukaryotic protein glycosylation pathway in Escherichia coli. Nat Chem Biol. 2012 Mar 25;8(5):434-6. doi: 10.1038/nchembio.921. PMID: 22446837; PMCID: PMC3449280.
23. Vieira Gomes, A.M.; Souza Carmo, T.; Silva Carvalho, L.; Mendonça Bahia, F.; Parachin, N.S. Comparison of Yeasts as Hosts for Recombinant Protein Production. Microorganisms 2018, 6, 38.[https://doi.org/10.3390/microorganisms6020038](https://doi.org/10.3390/microorganisms6020038)
24. Walski T, De Schutter K, Van Damme EJM, Smagghe G. Diversity and functions of protein glycosylation in insects. Insect Biochem Mol Biol. 2017 Apr;83:21-34. doi: 10.1016/j.ibmb.2017.02.005. Epub 2017 Feb 14. PMID: 28232040.
25. Xingpeng Duan, Jiaoqi Gao, Yongjin J. Zhou, Advances in engineering methylotrophic yeast for biosynthesis of valuable chemicals from methanol, Chinese Chemical Letters, Volume 29, Issue 5, 2018, Pages 681-686, ISSN 1001-8417,[https://doi.org/10.1016/j.cclet.2017.11.015](https://doi.org/10.1016/j.cclet.2017.11.015) .


The post[Overview of Recombinant Protein Expression Systems](https://futurefields.io/overview-of-recombinant-protein-expression-systems/) appeared first on[Future Fields](https://futurefields.io/) .
