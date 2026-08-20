---
schema_version: "1.0.0"
document_id: "6645ef034e8ed4b97aef4540dc52aaa38ca3be0c37fd0cc80cb61cd35abf465a"
company_key: "yc-talus-bio"
company: "Talus Bio"
source_id: "yc-talus-bio-rss-3371821d42a6"
canonical_url: "https://blog.talus.bio/p/why-proteomics-is-still-stuck-in"
published_at: "2025-06-06T13:02:00+00:00"
first_seen_at: "2026-07-24T03:16:47.351121+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:f43433290cc442804b36985f79441ea125032e7102c97d08ba1355fbac2927c8"
---

# Why Proteomics is Still Stuck in the Basement

# Why Proteomics is Still Stuck in the Basement


### Uniting a divided field under a common purpose


[Lindsay Pino](https://substack.com/@lkpino)


and[Alex Federation](https://substack.com/@afederation)


Jun 06, 2025


This blog post is a love-letter to my fellow proteomicists, not just the mass spectrometrists but the protein folders and the NMR spectroscopists, the CUT&RUNners and yes even the Western blotters. It’s also a rallying cry for us to get out of our echo chambers and collaborate to push the greater field of biology forward.


I’m headed home from my twelfth ASMS conference, on a flight to Baltimore as I write this out.


My first, the 2013 conference in Minneapolis, was both overwhelming and awe-inspiring. Although I’d done a bit of undergraduate research in a mass spectrometry proteomics lab, I didn’t get a chance to really use the instruments until I joined Dr. Steve Carr’s Proteomics Platform at the Broad Institute as a research associate several years later. There, my mentor Dr. Sue Abbatiello not only gave me access to the instruments but actively encouraged me to dive into the hardware, working collaboratively with a big instrument vendor (Thermo) on a hardware accessory for mass spectrometers (the early prototype of the FAIMS).


This is where the spark finally caught for me, and I was inspired by the potential of proteomics so much that I decided to go all-in and get my PhD, focusing on mass spectrometry proteomics.


Over the next few years, I managed to land (partially intentionally, but mostly serendipitously) among the front-lines of researchers developing and applying the “big data” method du jour for quantitative proteomics: data independent acquisition mass spectrometry (DIA-MS). Training at the University of Washington’s Genome Sciences department, my grad school cohort mostly consisted of genomicists and computational biologists, with us proteomicists frequently the outliers, so despite DIA-MS being a Big Deal in my mass spec circles, my departmental research reports mostly got glazed-over eyes and scattered polite claps. Amongst my fellow trainees, the joke was that proteins weren’t real – they’re “gene products” and therefore just a consequence of genomics – and to an extent, it’s true. Proteomics has definitely lagged behind genomics, for a variety of reasons.


I think the biggest is the lack of a shared space.


Genomics and transcriptomics had a commonality: the technology. DNA sequencing brought everyone together under a common readout, even if the sample prep and the data interpretation diverged. Proteomics is different, with dozens of instruments with hundreds of data outputs and interpretations. This difference became stark in the Genome Sciences department, where my fellow trainees could easily converse in the same “language” with each other, whether they were working on chromatin structure with ATAC-seq or mapping evolutionary trajectories with ancient DNA. They may have been using different sample preps or different interpretations, but it always seemed to me that they were united under a big “DNA Sequencing” umbrella.


Although I dearly love my fellow mass spec nerds at ASMS, I have to admit that we’re not the most welcoming field to outsiders.


Dr. Ben Orsburn (News in Proteomics Research, www.proteomics.rocks) often jokes about mass spectrometrists being some “guy in the basement”, with whom other scientists have to beg and bargain to get their samples run.


While mostly hyperbolic, there’s a grain of truth there. Mass spec proteomics data is not easily acquired, nor analyzed, and God forbid you have to actually make a biological interpretation from it. Mass spectrometry doesn’t even have a common file format, broken into vendor-specific proprietary file formats that require special converters, let alone even simple post-processing tools like batch correction. We mostly just borrow software from transcriptomics or even protein microarrays. Besides, just generating the data involves purchasing an LC-MS, which is going to cost $1M a pop, so nobody is casually getting one to tinker with like they might a minION for sequencing. And we haven’t even talked about sample prep, which historically has been home-brewed recipes and protocols that changed lab-to-lab, and only recently has started to conform into standardized kits.


But despite being low throughput and expensive and computationally frustrating, proteomics is still important, and the Free Market has noticed. Alternative technologies for protein detection and measurement have made quick work of expanding the market, making proteomics more accessible. Biofluid proteomics has been the beachhead for these technologies, representing a sample type with great therapeutic potential but also technically challenging for mass spectrometry with its large dynamic range. (Dynamic range refers to the distribution of protein abundances. Plasma has some proteins that are incredibly abundant, like albumin, and other proteins like metabolic enzymes that are very low abundance.) Understanding the plasma proteome is of such high commercial value that the Pharma Proteomics Project, a pharmaceutical consortium composed of 14 companies, is funding the measurement of 300,000 samples in the


[UK Biobank](https://www.ukbiobank.ac.uk/learn-more-about-uk-biobank/news/launch-of-world-s-most-significant-protein-study-set-to-usher-in-new-understanding-for-medicine) . 300,000 samples is unheard of for this type of quantitative proteomics work, but its enabled by these new technologies.


That’s not to say that there haven’t been other ambitious projects in proteomics before these next gen technologies. As a natural consequence of the completion of the Human Genome Project, the Human Proteome Organization (HUPO) launched the


[Human Proteome Project](https://en.wikipedia.org/wiki/Human_Proteome_Project) with the goal of assigning “a function for every protein” by mapping all the predicted proteins encoded by the human genome (). There’s also spatiotemporal aspects of proteomics to consider, with projects like the Human Protein Atlas.


We can’t ignore the elephant in the room either.


The 2024 chemistry


[Nobel Prize](https://www.nature.com/articles/d41586-024-03214-7) was awarded for proteomics – specifically, protein folding. Here’s where maybe I’ll get controversial, but I would consider protein folding to be under the broader umbrella of proteomics. After all, AlphaFold and Rosetta were only made possible by their underlying training data: decades and decades of painstakingly crystallized and measured protein structures, curated and deposited in a central


[database](http://pdb.org/) . The triumph of AlphaFold is really an homage to the PDB, a warehouse dedicated to structural proteomics no matter what the readout, whether X-ray crystallography or nuclear magnetic resonance (NMR) or electron microscopy (EM) techniques.


So as I’m getting ready to commune with my fellow mass spec-nerds at ASMS, I’m also thinking about how we – speaking for all protein-focused scientists here – could do a better job of reaching out to our fellow proteomicists that happen to use other readouts. The imaging microscopist, the protein folder, and yes even you, Western blotter, you’re all measuring proteins and I want to learn more about why and how you’re interested in proteins.


I want to learn how to link our collective data together in a way that will enable the next big advance, on the scale of AlphaFold or even bigger, maybe model a


[virtual cell](https://www.sciencedirect.com/science/article/pii/S0092867424013321) ? To do something like that will require trustworthy data, and to have trustworthy data means it will be validated and reproduced with orthogonal technologies. Sure there may be a dozen people all studying the same protein in the same disease space, but if they’re all using different approaches, that’s not a risk of competition, that’s a potential for collaboration. The existence of competing tech doesn’t shrink the individual slices of the Proteomics Market Size Pie, it expands the size of the field overall. Instead of building up taller and stronger fences, I’d love to see us open up the gates and collaborate across instrumentation, making bridges from protein catalogs to structure to function, and to scientific breakthroughs.


In the meantime, what can we do


*literally* today?


We’re headed home now from an amazing ASMS conference, let’s keep the spirit of the conference going. Invite someone you don’t already know at your institute out for a “coffee chat” and talk shop. (Don’t forget those early career researchers, they’re the future of the field!) See a cool talk? Get in touch now with the presenter and propose a collaboration. Peruse the conference proceedings and check out the posters that are way outside of your depth and email the presenter to ELI5. Watch a recording from an oral session you’ve never been to before and, even better, get in touch with the authors to ask a question. And beyond the ASMS hangover, longer term, attend a conference outside of your field. Reach out to a researcher using a different technique, but asking similar biological questions, and offer to share data or run a follow-up experiment. If you’re really ambitious, reach out beyond researchers to nonprofit organizations, to investors, to federal policymakers, and ask them what


*they* think needs to happen for proteomics to make the next big breakthrough. Is it technology development? Is it training? Is it funding? Having a more holistic approach over the next decade will benefit the individual (greater impact, greater reach), the scientific community (virtual cell), and humanity (therapeutics, diagnostics, agriculture, forensics).


That’s what I’ll be doing on the plane back to Seattle, following up on the brilliant things I learned and the amazing new people I met.


We have such a unique community in mass spectrometry and the ASMS conference is such an overwhelming yet awesome experience. This year I got uncomfortably out of my depth, and talked to people about how mass spec proteomics can be doing a better job of making new connections beyond our own community. The future of proteomics is interactions. Interactions between techniques, interactions between data, and interactions between people. The more we connect, the more we discover.


[Leave a comment](https://blog.talus.bio/p/why-proteomics-is-still-stuck-in/comments)
