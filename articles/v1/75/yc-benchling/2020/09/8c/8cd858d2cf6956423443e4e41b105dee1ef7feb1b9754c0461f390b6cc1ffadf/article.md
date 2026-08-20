---
schema_version: "1.0.0"
document_id: "8cd858d2cf6956423443e4e41b105dee1ef7feb1b9754c0461f390b6cc1ffadf"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/introducing-new-antibody-property-prediction-tools"
published_at: "2020-09-22T15:18:42+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:8649d2b9c2e101d63f37a3800006b6f356b57a0742d3b7a21a94f84d901a5bce"
---

# Introducing New Antibody Property Prediction Tools

We’ve added a few new enhancements to Benchling’s[Molecular Biology](https://www.benchling.com/molecular-biology) application to help accurately predict the molecular properties and binding behaviors of antibodies.


Benchling’s new **Antibody Property Prediction Tools** automatically analyze antibody chain variable domains on individual and aligned amino acid sequences to help you accurately and consistently identify:


-


Amino acid residue positions


-


Complementarity determining regions (CDRs)


-


Framework regions


-


Post-translational modifications that predict sites of sequence liability


The need for more robust antibody annotation and analysis tools has never been more important. The imprecise definition of antigen-binding sequences or their corresponding structural residues may lead to design decisions that inadvertently[reduce the affinity and specificity of antibodies binding to their targets](https://www.frontiersin.org/articles/10.3389/fimmu.2018.02278/full) . For institutions[racing to develop COVID-19 treatments](https://www.benchling.com/2020/04/07/how-biotechs-are-racing-to-develop-covid-19-treatments/) , those design decisions will have significant implications in the effectiveness of any new antibody therapeutics.


## Identify Antibody Amino Acid Residue Positions


Standardizing how antibody residues are numbered is critical for accurately and consistently identifying the position of structurally corresponding residues across human and non-human antibodies. Because the amount and composition of amino acid residues can vary between antibodies, antibody numbering schemes help identify and compare the corresponding positions of residues across similar molecules.


Benchling’s antibody numbering schemes are assigned using[ANARCI](http://opig.stats.ox.ac.uk/webapps/newsabdab/sabpred/anarci/) (Antigen receptor Numbering And Receptor ClassificatIon) tool, which was[created by the Oxford Protein Informatics Group (OPIG](https://academic.oup.com/bioinformatics/article/32/2/298/1743894) ). We support antibody numbering annotations on:


-


Variable heavy and light domains for antibodies


-


Variable alpha, beta, gamma, and delta domains for T-cell receptors


Benchling uses ANARCI to annotate amino acid sequences and alignments with the Kabat, Chothia, and IMGT antibody numbering schemes. To learn more about the differences between our supported antibody numbering schemes, we recommend[this review from Dondelinger et al. (2018)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6198058/) .


## Annotate CDRs and Framework Regions


Modern antibody engineering requires the accurate identification of CDRs (antigen-binding regions) and framework regions (the scaffolding that enables CDRs to be properly accessible for binding antigens) to achieve high target affinity and specificity.


Benchling’s new CDR and framework region annotation feature automatically labels these regions on your individual and aligned amino acid sequences. Our CDR and framework annotations are assigned using[SCALOP](http://opig.stats.ox.ac.uk/webapps/newsabdab/sabpred/scalop) , which was also developed by the Oxford Protein Informatics Group. Since CDRs and framework regions are specified based on their antibody numbering scheme, we allow you to change the settings that define the numbering scheme used to annotate these regions.


Benchling’s CDR annotations allow you to distinguish between:


-


Variable-heavy (VH) and kappa (κ) or lambda (λ) variable-light (VL) chains


-


Alpha (α), beta (β), delta (δ), and gamma (γ) T-cell receptors.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Identify Sequence Liability Sites


Certain post-translational modifications (PTMs) on antibodies are considered “sequence liability sites,” and can impact an antibody’s stability and ability to bind antigens. Minimizing the number of liability sites on antibody chain candidates — especially within CDRs — helps reduce the chance of downstream issues with manufacturing and patient-dosing in clinical trials.


Benchling now detects and displays the following sequence liability sites:


-


N-linked glycosylation


-


Asparagine deamidation


-


Aspartate isomerization


-


Methionine oxidation


-


Tryptophan oxidation


## Get Started With Benchling’s Antibody Property Prediction Tools


Benchling’s new antibody property annotations are included within our suite of Molecular Biology analysis and design tools.[Request a demo](https://www.benchling.com/request-demo/?utm_source=benchtalk&utm_medium=web&utm_campaign=antibody_tools_release) today! If you already have a Benchling account, simply[log in](https://benchling.com/signin/welcome) and get started!
