---
schema_version: "1.0.0"
document_id: "187e7cd82936cd3670b9f8cf00697f9d35ae3f1ffd0c2ed5acc86f6ceedce486"
company_key: "pacific-biosciences-of-california-inc-common-stock"
company: "Pacific Biosciences of California Inc."
source_id: "pacific-biosciences-of-california-inc-common-stock-rss-232046c7699f"
canonical_url: "https://www.pacb.com/blog/customer-success-story-how-uppsala-university-and-the-genome-of-sweden-project-are-scaling-national-genomics-projects-with-revio-and-sprq-nx/"
published_at: "2026-08-17T13:05:53+00:00"
first_seen_at: "2026-08-17T15:01:14.692273+00:00"
fetched_at: "2026-08-17T15:01:16.295193+00:00"
content_hash: "sha256:726bfe762da0b0fa963fcb4a3f4b2aa60060ab34c0f41b73a42c85a64b34383e"
---

# Customer success story: How Uppsala University and the Genome of Sweden project are scaling national genomics projects with Revio and SPRQ-Nx

Adam Ameur, PhD
Associate Professor of Genomics and Bioinformatics at SciLifeLab’s National Genomics Infrastructure (NGI), Uppsala University


When Dr. Adam Ameur first encountered long-read sequencing data, the idea of sequencing thousands of complete human genomes with long reads seemed distant.


Back in 2013, he was using the PacBio RS II platform to study targeted amplicons and was exploring whether[long-read sequencing](https://www.pacb.com/technology/long-read-sequencing/) could solve difficult questions in human genetics. Now more than a decade later, that vision has become reality. Today, Ameur and his colleagues at Uppsala University and the[SciLifeLab National Genomics Infrastructure](https://www.scilifelab.se/units/ngi/) (NGI) are building one of Europe’s largest population-scale long-read sequencing efforts through the Genome of Sweden project as part of the broader[Genome of Europe](https://genomeofeurope.eu/) initiative.


The project recently became an early adopter of the PacBio SPRQ-Nx chemistry on the[Revio system](https://www.pacb.com/revio/) , helping the team increase throughput and lower sequencing costs while maintaining the high-quality HiFi data their established workflows rely on.


We spoke with Dr. Ameur about his journey with PacBio, lessons learned from scaling national genomics projects, and why he believes long-read sequencing is becoming the future of population genomics.


[See what’s possible with SPRQ-Nx](https://programs.pacb.com/l/1652/2026-04-21/45hbps)


---


## Q:


You’ve been working with PacBio for more than a decade. How has the technology evolved during that time?


Dr. Adam Ameur:


The first time I started working with PacBio data was back in 2013. At that point we were looking at specific amplicons using the RS II instrument. We had relatively short reads compared to today and not as many of them, but I was always interested in what kinds of human medical applications we could use the technology for.


Over time the throughput has increased dramatically. I remember assembling human genomes back in 2018 using Sequel system data and needing dozens of SMRT Cells.


Now everything is much more streamlined, both from a sequencing perspective and computationally. With Revio, it suddenly became feasible to start doing human[whole genome sequencing](https://www.pacb.com/products-and-services/applications/whole-genome-sequencing/) at population scale.


The SciLifeLab NGI team at the installation of their Revio system in 2023.


## Q:


What led the Genome of Sweden project to choose long-read sequencing from the beginning?


It was an intentional decision for all the new sequencing for this project to be with long reads from the start.


> ### We wanted high-quality data because our goal was to sequence this cohort just once. We wanted data we could trust so we wouldn’t have to go back and sequence it again.


Our clinical collaborators had already shown the value of long-read sequencing for rare disease. They knew they could identify disease-causing variants, but they also needed a reference dataset because long-read sequencing discovers so many more variants. We needed a population reference so researchers could distinguish common variants from rare ones.


That really helped move the whole project toward long-read sequencing.


## Q:


Scaling thousands of genomes requires balancing quality, throughput and cost. How have you approached that challenge?


Answer:


It’s been an exciting journey because things change all the time.


We wanted to use the instruments as efficiently as possible to drive down costs. We settled on around 20× coverage because we know from previous studies and discussions with collaborators that we find the vast majority of variants at that level.


Using DeepVariant for SNVs and indels and sawfish for structural variants, a similar number of variants is found in all samples and the majority of variants are captured at 20x coverage.


We’ve optimized multiplexing by sequencing larger pools of samples, automated much of the laboratory workflow, and streamlined the bioinformatics pipelines.


We initially started slowly, but have been continually increasing in volume and output, and have even been able to achieve upwards of 167 Gb per SMRT Cell from ten-year-old biobank DNA. Ultimately, we hope to continue to make this technology available for large-scale projects.


## Q:


Your team participated in the early SPRQ-Nx program for Revio. What interested you most about evaluating the new chemistry?


Answer:


Our Genome of Sweden project had already been underway for about a year, and we were looking for the highest throughput at the lowest possible cost.


SPRQ-Nx was very suitable for this type of project because we can create large pools of libraries. In the early access program, we sequenced a pool of 16 samples across four reusable SMRT Cells. By running each SMRT Cell twice we were able to reach 20× coverage per sample at lower cost.


More recently we’ve even started using them a third time as a way to increase the size of these pools and continue driving down costs.


## Q:


Did adopting SPRQ-Nx chemistry require significant workflow changes?


Answer:


It’s been quite straightforward in the lab. Using the SMRT Cell twice doesn’t require touching the instrument. To use it three times there are a few additional steps, but that hasn’t been an issue.


> ### For me, the important thing is the data. The data are consistent across uses, and that’s exactly what we’re looking for.


## Q:


Beyond sequencing genomes, what scientific opportunities excite you most?


Answer:


It’s really exciting that long-read sequencing provides single-molecule information with both the genetic variants and the methylation.


We’re only at the beginning of exploring these multiomic aspects. We have collaborators interested in methylation for rare disease diagnostics, others studying regulatory biology, and we’re also interested in[transcriptomics](https://www.pacb.com/products-and-services/applications/rna-sequencing/) and eventually[Fiber-seq](https://www.pacb.com/blog/how-pairing-epicyphers-fiber-seq-with-hifi-sequencing-delivers-an-all-in-one-multiomic-view/) .


> ### If you really want to understand the biological mechanisms behind a specific variant, it is especially valuable to connect all those layers of information on the same molecule.


## Q:


Q: What still needs to happen before long-read sequencing becomes routine at population scale?


Answer:


I think increasing throughput is still an important step.


If you compare Revio with today’s highest-throughput short-read systems, they can still process very large numbers of genomes very quickly.


But sequencing is only one piece of the puzzle. We also need the broader ecosystem around it, including laboratory automation, robust bioinformatics pipelines, tools for variant interpretation, and database infrastructure.


Most of today’s variant databases were built using short-read sequencing. The next step is to build new variant databases built from long-read data. I hope projects like the Genome of Sweden can help move that forward, but it ultimately needs to become an international effort.


## Q:


Q: Looking ahead, where do you see long-read sequencing taking the Genome of Sweden project?


Answer:


Our immediate goal is to sequence around 2,000 Swedish genomes.


Beyond that, I hope the project expands into transcriptomics, Fiber-seq and other multiomic technologies. Longer term, it would be fantastic to perform genome-wide association studies directly using long-read genomes instead of relying on SNP arrays and imputation.


With enough samples, we’ll be able to study all of these complex variants much more directly, which would be really exciting.


---


Over the past twelve years, Adam Ameur has watched long-read sequencing evolve from a specialized research tool into a practical platform for national genomics initiatives. Today, as the Genome of Sweden project scales toward thousands of HiFi genomes with Revio and SPRQ-Nx chemistry, the conversation is no longer about whether long-read sequencing can support population genomics, it’s about what becomes possible once it does.
