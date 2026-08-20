---
schema_version: "1.0.0"
document_id: "0c438b6b5b33f3332ccfb3e45a79fc2caa7b47813bcc6ea3684c2efe072f339e"
company_key: "yc-adaptyv"
company: "Adaptyv"
source_id: "yc-adaptyv-news-import-3c8571c35bcb"
canonical_url: "https://www.adaptyvbio.com/blog/anthropic-1"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T08:34:20.893763+00:00"
fetched_at: "2026-08-19T08:34:23.193831+00:00"
content_hash: "sha256:021ad463c7d50062e68586e33504b5879007c7e6716995ca952cb9f83af2ffeb"
---

# Case study: Benchmarking Claude’s protein designs in the wet lab

/


TL;DR


- -


The Anthropic team used Claude in Claude Science to design protein binders against 16 targets and sent them to our lab.


- -


We ran all the binding experiments in our automated wet lab, going from digital protein sequence, to DNA, expressed proteins, then measurement and quality control.


- -


Out of 1,320 designs, 354 bound their target, giving Claude an average hit rate of 26.8%. Only a single target yielded 0 binders.


- -


Compared to the results of our Protein Design Competitions, Claude’s designs had noticeably higher success rates and would have won 5 out 6, yielding tighter binders.


- -


At the end of the post we expand more on how we think about the future of agentic science, protein design and the long all road to curing all diseases.


The promise of bioengineering is to design new biological tools: diagnostics that help us detect diseases, antibodies that can destroy cancers, new vaccines to protect against viruses. To build those tools, AI will ultimately need to interact with the physical world and not just with a static


dataset


.


The next phase of AI in biology is an agentic science loop: an AI system that can reason over scientific knowledge, use specialised tools to design thousands of molecules, send those designs into an automated wet-lab, and learn from the resulting data.


Three layers


need to work together:


- **Generalist models**


to interpret a scientific goal, plan a campaign, and orchestrate workflows.


- **Specialist models and tools**


to perform tasks such as


protein structure prediction


and protein design models.


- **Automated wet labs**


to turn digital designs into measurements from the physical world.


In this post, we showcase how Anthropic benchmarked Claude Mythos Preview and Opus 4.8 at designing novel protein binders that we tested in our automated wet lab. Overall, Claude models showed expert level skill at protein design, matching or exceeding human experts on many tasks. Together with the Anthropic team, we’re releasing the actual protein sequences Claude designed as well as the experimental data on


[Proteinbase](https://proteinbase.com/)


, the open protein data


platform


.


### How to test AI-designed proteins in the real world


At Adaptyv, we have built an automated, AI-native wet lab for turning protein designs into experimental data.


Instead of scientists in lab coats pipetting samples in tiny tubes one by one, we have built automated workcells that run the same experiments


many times faster


and cheaper.


Human protein designers and agents can access the lab via our web platform and API to send digital protein sequences for wet lab validation on different assays.


Our platform automatically processes the protein sequences (a string of amino acids) and converts it into a special DNA sequence that encodes the biological instructions for how to create the specific protein. This digital DNA sequence is then turned into an actual physical DNA molecule in the lab by assembling the DNA building blocks one


by


one


. Next, cell-free protein synthesis takes the machinery a cell uses to read DNA and build proteins (ribosomes, enzymes, amino acids, an energy supply) and runs it with no cell


around it


. This is done using automated robots able to pipette incredibly small amounts of liquid really fast and at high-throughput.


At this point we have now made physical proteins in the lab from the digital AI-designed sequence. But we haven’t yet tested if the protein actually performs well at what it was designed to do. Proteins are the molecular machinery of all of life and can do many different things: digesting the food we eat, breaking down harmful molecules and making ones that we need to survive, generating energy in our cells, cutting and editing DNA, and a million other things.


Here, we’re testing binders: proteins that should stick to one target and nothing else.


Most antibody cancer drugs are binders. So are the reagents in a pregnancy test and the capture


molecules in most diagnostics. The “sticking strength” of the protein binder to its target can be measured with special instruments, giving us a so called


K_D


value. A lower


K_D


means a better binder, higher affinity binder, an often desirable property when developing new therapeutics. At Adaptyv, we run these specialized instruments with our own software to process all raw data to obtain clean


K_D


values against a wide range of target proteins that are relevant for therapeutic, diagnostic, and research


applications


.


Behind this simple input-output interface is a complex experimental process: many reagents, instruments, protocols and measurements must be coordinated and verified. At Adaptyv we package that complexity into an automated, quality-controlled workflow that answers a biological question reproducibly and generates a defined type of data at scale.


#### So how good is Claude at designing proteins?


The team at Anthropic chose 16 existing protein targets from our previous public


[competitions](https://proteinbase.com/competitions)


, hackathons, and


[BenchBB](https://targets.adaptyvbio.com/collections/benchbb)


to benchmark different Claude versions for de novo binder design, all using


[Claude Science](https://www.anthropic.com/news/claude-science-ai-workbench)


. They were all prompted similarly and had access to the same publicly available tools.


Then, designs against all these targets were sent to our wet lab, in an anonymized way. We did not have any information on which Claude model designed which protein. We ran our Affinity Characterization assay, to measure the binding strength on SPR using 5 target concentrations and with duplicate measurement, ensuring the results are robust, accurate, and matching our quality control standards.


95% of the designs expressed, which three years ago would have been an impressive headline, showing the rapid progress of AI tools for protein design in recent years. This number matched the best expression rates of our EGFR competition which had hundreds of expert protein designers, and surpassed other challenges such as the RBX1 one. Out of these, 354 of all designs (1,320) bound their target, an overall hit rate of 26.8%, and the per-target hit-rates vary quite widely. One target (MBP) yielded no binders. Overall, Claude designed proteins that bound 14 out of 15 targets, with 1 target (GDF-8 mature) being excluded due to low-quality measurements in our setup, likely due to the target aggregating, meaning it was binding to other molecules of itself.


When compared to our competitions, Claude surpasses all their hit rates, especially when looking at every single run for each target Anthropic submitted as in the plot above. For a fair comparison, we have subsetted each competition’s results to only include de novo minibinders. Claude achieved an 80% hit rate on TREM2, greatly improving over the 38.3%


[we reported in our competition](https://www.adaptyvbio.com/blog/agents-vs-humans)


, and even on trickier targets such as 15-PGDH, it has a success rate more than 3-fold higher than


[observed on Proteinbase](https://proteinbase.com/collections/berlin-bio-x-adaptyv-15-pgdh-binder-design-competition)


.


The best Claude binders also show greater binding affinities compared to 5 out of 6 competition winners. It greatly surpassed the best 15-PGDH binder, going from 1.7 uM to 33.4 nM, and similarly for RBX1 (from 25.7 nM in the competition to 3.9 nM). Surprisingly, it was unable to beat the best binder on the Nipah Virus Competition. For GDF-8, RBX1, and Nipah, Claude seems to have explored a wide range of possible epitopes, while it converges on single well-defined epitopes in case of TREM2. Overall, Claude would have been a prolific protein designer if it were to participate in our Protein Design Competitions.


Looking at all these impressive results, Claude seems to have matched or even surpassed expert protein designers when it comes to orchestrating openly available design tools. We are definitely excited to see where this is heading towards and what the future of agentic science and protein design might look like. We’re sharing some of our thoughts down below.


### Agentic science and curing actual diseases


So can AI now solve all diseases? Well no, not yet.


This case study shows that Claude is at least expert-level at orchestrating protein design tools. That’s great news, since protein design tools are hard to use. Before AI, even setting up a protein structure prediction tool could take hours of debugging opaque


[conda](https://anaconda.org/anaconda/conda)


errors.


Now, imagine a researcher working on a new cancer therapeutic who needs an assay that can distinguish between two mutated receptor variants. They can give Claude the sequencing results from their cell samples and ask it to help design binders. Claude would sift through papers and research databases, find where in the genome the receptors are encoded, compare the two sequences, and map the differences. It would use protein folding models like Boltz to generate 3D structures of the receptor variants, then use protein design models like BindCraft to generate binders that attach to one variant but not the other. It would generate thousands of computational designs and score them to identify the most promising candidates to test. Then, thanks to our


[cloud lab API](https://www.adaptyvbio.com/api)


, Claude could submit the best candidates to the lab and have them tested experimentally in a couple of weeks. Once the results are in, Claude could analyze the data and find that the strongest binders are actually too promiscuous and bind both variants. Informed by the first round, it could then launch another design campaign to reduce that cross-reactivity and produce better proteins in the next round. Those can be linked to a fluorescent marker and then bind and mark the cancer cells to differentiate them from healthy cells.


Just a few years ago, this process alone would’ve required a year’s worth of a whole lab’s work. Today, anyone who is scientifically curious can run this campaign end-to-end from their laptop in a few weeks and for a couple thousand $ in AI tokens, cloud compute and wet lab credits.


### The road to curing all disease needs to be built first


Of course, those proteins that we tested here are not real therapeutics. They completed only the first step of the process: demonstrating that they can function as binders. Still, this study shows a path towards making actual therapeutics with AI.


Imagine making a drug is like climbing a mountain. We have clearly been able to climb some mountains, as humanity has made many drugs already. But the way to the top is a dangerous narrow path and climbing it takes many years and costs billions of dollars (and the lives of many biotechs).


The goal of AI for drug discovery is turning this narrow mountain path into a highway, making it easier and cheaper to get to the top so that we can develop 100x more therapeutics than we have right now. Similarly, w


riting code was a more of a high-expertise craft before LLMs, now it’s mostly automated and it has made generating software accessible for anyone.


In practice, for drug discovery, that means:


- **Automating molecular design and bioinformatics work**


to make better drug candidates


- **Building good experimental readouts**


that answer relevant biological questions


- **Automating those experimental workflows**


to increase throughput and lower costs at


scale


On the wet-lab side, the road then looks roughly like this:


1. **Expression:**


Can we reliably synthesise newly designed proteins?


2. **Binder design:**


Can we make proteins that bind their intended target?


3. **Therapeutic formats and developability:**


Can a candidate be made in the formats industry uses, remain stable and be manufactured reliably?


4. **Cell-based function:**


Does binding produce the intended effect in a living-cell system?


5. **Organoids and more realistic models:**


Does it work in a model that better captures human biology?


6. **Translational and in-vivo evidence:**


Is it safe and effective in the settings that ultimately matter for patients?


At every step, the mountain gets steeper and more challenging, requiring better models (both intelligence/generalists and domain-specific tools), more compute and more wet lab data.


#### Closing the physical feedback loop


This Anthropic campaign was an


**open-loop experiment**


. Claude designed proteins, Adaptyv tested them experimentally, and here we have presented the results. The next step is to


**close that loop**


. An agent proposes a batch, receives experimental data, learns from the failures and successes, and chooses the next batch on the strength of what it just learned.


The long-term vision is to give AI a real path to help cure all disease. It will not happen in one leap. It happens by extending the physical feedback loop from binding to each harder biological question that follows, until AI can learn from the evidence needed to develop medicines.


### Resources


- Read Anthropic’s blog post here:


[https://www.anthropic.com/research/Claude-accelerates-protein-design](https://www.anthropic.com/research/Claude-accelerates-protein-design)


- Check out the protein designs and experimental data here on Proteinbase:


\[


[COMING SOON](http://proteinbase.com/)


\]


- Benchmark your own protein designs:


[adaptyvbio.com](https://adaptyvbio.com/)


- Connect your agent to our


[API](https://docs.adaptyvbio.com/)


and


[MCP server](https://docs.adaptyvbio.com/api-reference/mcp-server)


- Join Adaptyv:


[adaptyvbio.com/careers](http://adaptyvbio.com/careers)
