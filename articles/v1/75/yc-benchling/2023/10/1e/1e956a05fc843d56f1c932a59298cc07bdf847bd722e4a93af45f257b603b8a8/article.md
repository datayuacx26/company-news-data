---
schema_version: "1.0.0"
document_id: "1e956a05fc843d56f1c932a59298cc07bdf847bd722e4a93af45f257b603b8a8"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/unlocking-data-insights-for-rna-delivery-methods"
published_at: "2023-10-12T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:05b115835d5373687ed9f71792274d283ceb732f754d3fe604c91e2a2c00629f"
---

# Unlocking data insights for RNA delivery methods

Benchling gives RNA scientists an unfair advantage in their research. Want to learn more? Recently, HDT Bio joined us for a webinar where we delved into the intriguing science behind their novel emulsion platform, known as LION™.


We heard from HDT Bio Associate Scientist, Adrian Simpson, on how this cutting-edge platform enables the efficient delivery of self-amplifying RNA for vaccines and therapeutic intervention.


**Adrian Simpson, Associate Scientist, HDT Bio**


Adrian has worked in formulation development for the past 8 years primarily focused on formulations for vaccine delivery. He began his career at the Infectious Disease Research Institute where he worked on the stability program for the formulations department, then began to develop formulations for vaccine applications. From there he moved to the SF Bay Area and worked at Verily, an Alphabet life science company, where he worked on DNA barcoding for tumor targeting and gene editing using LNP's (lipid nanoparticles)


Adrian was then joined by Benchling’s Senior Product Marketing Manager, Vega Shah, for a look into how Benchling can support efficient and effective data management.


**Vega Shah, PhD, Senior Product Marketing Manager, Benchling**


Vega is an experienced biologist and former research scientist. In her current role, she helps biotech companies use software to accelerate their research. She previously was a product manager for an electronic lab notebook for biologics and screening. Prior to her industry career she was a scientific researcher at Lawrence Berkeley National Lab, University of Washington, and UC Berkeley.


The[hour-long conversation](https://www.benchling.com/webinars/rna-therapeutics-unlocking-data-insights-for-rna-delivery-methods) provided an overview of vaccine formulation at HDT Bio, the data architecture used to capture formulation data, challenges faced on QC and stability, and how they manage all of this information with Benchling.


## Immunotherapies for all


Founded in 2018, HDT Bio is a biopharmaceutical company that started out to address a simple goal: developing immunotherapies for all. When just a year later the COVID-19 pandemic hit, HDT Bio was well positioned to respond.


The team at HDT Bio sprung into action by rapidly developing a self amplifying COVID vaccine using a novel delivery mechanism. The vaccine candidate, known as LION™, exists as an oil and water emulsion, using a cationic lipid incorporated into an emulsion to bind RNA via charge interactions. This emulsion acts as a delivery vehicle for the emulsion RNA complex as it is delivered to the cell, allowing the RNA to then replicate and produce the encoded protein.


**Vega:** What are some of the advantages of using LION™ as an RNA vaccine delivery mechanism?


**Adrian:** LION™ has several distinct advantages over the currently approved RNA delivery mechanisms. First of all, it is advantageous because the formulation forms a shelf stable emulsion without the addition of RNA, allowing for the mixture of RNA at bedside instead during its manufacture. LION™ can also be stockpiled and stored independently of RNA, increasing the vaccine component shelf life. Lastly, our oil-in-water emulsion shows an improved safety profile especially for the delivery of replicating RNA.


### Like oil and water: The LION™ formulation process


To better understand how the emulsion formulation is manufactured, Adrian walked us through the end-to-end process.


The process begins by separately weighing out the hydrophobic and hydrophilic components of the formulation. The oil and aqueous phases are then heated and melted, before being combined and mixed to create a crude emulsion. The crude emulsion is then added to a microfluidic device, which utilizes a pressure of up to 30,000 psi to push the crude emulsion through the interaction chamber. This causes shearing forces on the larger oil droplets, which reduce in size to form stable nanoemulsions. From there, the formulation may be sterile filtered, and is then ready to use to deliver RNA.


This process is advantageous because it is incredibly scalable. Using this workflow, the HDT Bio team is able to manufacture between ~100 mL up to 1 L in a single run.


### Running the numbers


To better understand the correlation between factors that affect the formulations, Adrian and the HDT Bio team regularly utilize Design of Experiment (DoE) principles to interrogate these interactions.


DoE gives us two powerful tools: it defines and reduces the number of samples — in this case, formulations — that need to be tested, and it helps by showing trends between variables.


Adrian walked us through an example of experimental outputs using a DoE approach. Through this overview, we learn how these principles allow for a reduction in manufacturing burden, as well as increased insights into data trends.


### Getting the data model right


HDT Bio began working with Benchling on designing their tenant data architecture, allowing the team to optimize how to best capture data.


Adrian provided a deep dive of the design to help our audience understand how they were able to capture and surface data in manageable ways. The following is a list of Benchling Registry entities created by Adrian and his team to establish the right framework:


**Formulation Raw Material:** This is the chemical component of a formulation, such as a helper lipid, for example. It is a simple entity, serving as the building blocks for every formulation.


**Formulation Raw Material Lot:** From here, there is a parent-child link to the formulation raw material lot. One can think of this as being representative of a physical chemical sample in the lab.


**Formulation Entity:** The formulation entity, in comparison, is likened to a recipe for a formulation. It primarily consists of formulation raw material entities and formulation metadata.


**Formulation Lot:** Finally, the formulation lot is created. This is a digital representation of the physical entity in the lab.


**Formulated Complex:** The above entities can be used to generate formulated complexes by mixing it with an RNA.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


### Formulation QC and stability challenges


One of the key challenges to overcome when designing HDT Bio’s database architecture was the breadth of data captured, as well as the management of tasks and samples.


Perhaps one of the more unique problems encountered throughout this process was the generation and management of stability tasks. HDT Bio’s formulations were put on long term stability programs to help understand the effects of chemical changes on stability of formulations.


Another problem that HDT Bio has in common with many other labs is the dispersion of data. Between experiments, varied lots and formulation entities, and projects and grants, data is bucketed and distributed in many ways. This can pose difficulties when writing final reports, especially with long projects when data is scattered across a long series of experiments.


### Benchling provides operational oversight


Luckily, the team at HDT Bio was able to implement solutions to many of the commonly aforementioned challenges for large data sets using a combination of Benchling tools. With Benchling, HDT Bio implemented functional and relatively low effort workflows for their largest and most common problems in the lab.


### Benchling insights


One of the most significant issues Adrian’s team faced was generating tasks for their stability program, monitoring hundreds of notifications and notifying the team when they needed to run certain assays.


Adrian’s team was able to work up a set of insights blocks, which individually query the formulation lot database for the manufacturer dates, and surface formulations that fall within specific time windows, allowing for more effective and timely action.


Being able to rapidly aggregate data sets is an unsung hero. When it comes to data architecture, the parent child link proves invaluable here. It’s often the case that the team wants to look at the results of all the lots of a formulation at once, which can be painful to aggregate if not in an easily digestible format.


### Templates for Zebra printer


Another useful feature that has been widely adopted by HDT Bio’s lab is templates specifically created for a[Zebra printer](https://help.benchling.com/hc/en-us/articles/9684246740493-Label-Printing-with-Zebra-Browser-Print) , which utilize Benchling’s built-in inventory system. Being able to easily find and run samples for analysis is a critical component of a program being successful.


Want to learn more about how Benchling can be leveraged as a powerful tool for efficient data management?[Let's chat](https://www.benchling.com/request-demo) .
