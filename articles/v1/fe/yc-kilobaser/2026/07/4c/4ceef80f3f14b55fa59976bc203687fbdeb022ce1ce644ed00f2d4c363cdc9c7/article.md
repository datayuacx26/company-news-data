---
schema_version: "1.0.0"
document_id: "4ceef80f3f14b55fa59976bc203687fbdeb022ce1ce644ed00f2d4c363cdc9c7"
company_key: "yc-kilobaser"
company: "Kilobaser"
source_id: "yc-kilobaser-news-import-9f375b6e1738"
canonical_url: "https://kilobaser.com/post/digna-webinar-dna-synthesis-libraries-microarrays"
published_at: null
first_seen_at: "2026-07-25T10:46:06.691006+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:179bcb1f41395cd7e850e119d79fcd8d1a90c29d0ec089732630cbaeb8e61126"
---

# Webinar Recording: Chemical Synthesis of DNA - From Solid-Phase to Large-Scale Libraries

# Webinar Recording: Chemical Synthesis of DNA - From Solid-Phase to Large-Scale Libraries


Watch the recording of our webinar on chemical DNA synthesis! Kilobaser's CSO Martin Jost and Prof. Mark Somoza from the University of Vienna explored the evolution of DNA synthesis - from traditional solid-phase methods to cutting-edge large-scale microarray synthesis for applications including digital data storage in DNA.


### [DigNA Webinar Series](https://kilobaser.com/digna-dna-data-storage) - Webinar 5


**Date:** Thursday, February 19, 2026


**Speakers:** Martin Jost (Kilobaser) and Mark Somoza (University of Vienna)


**Recording:**[Watch on YouTube](https://www.youtube.com/watch?v=0xTcPJKWVJQ)


This webinar was part of the[DigNA portfolio](https://kilobaser.com/digna-dna-data-storage) webinar series on DNA Data Storage Technology.


### About the Webinar


Modern chemical synthesis of DNA follows the highly efficient phosphoramidite approach developed in the 1980s by Marvin Caruthers and Serge Beaucage. Initially focused on the solid-phase synthesis of single oligonucleotides, the approach's versatility has enabled adaptation to large-scale applications, specifically automated in situ microarray synthesis of up to several million oligonucleotides in parallel.


These microarrays were originally used for genomic applications, but their use has been expanded in recent years to include massively parallel synthesis of libraries for applications including digital data storage in DNA.


This webinar outlined the basics of solid-phase synthesis as well as a variety of modern approaches for large-scale synthesis of DNA libraries and microarrays.


## Meet the Speakers


### Martin Jost


**Co-founder and Chief Scientific Officer at Kilobaser**


Martin Jost is a co-founder and Chief Scientific Officer at Kilobaser, where he contributed to the development of a personal desktop DNA and RNA synthesizer based on microfluidic chip technology. He studied molecular biology and informatics at Graz University of Technology and the University of Graz. In 2013, he co-founded the Open Biolab Graz, Austria (OLGA), a community laboratory for molecular biology and biohacking, where the idea for Kilobaser emerged from the founders' own frequent need for DNA primers. Within the DigNA portfolio, Kilobaser is a partner in the DNAMIC project, contributing decentralized on-site oligonucleotide synthesis capabilities for DNA-based data storage applications.


### Mark Somoza


**Associate Professor of Chemistry at the University of Vienna**


Mark Somoza is an Associate Professor of Chemistry at the University of Vienna. His research is in nucleic acid chemistry and chemical biology, focusing on large-scale synthesis of nucleic acids using photolithographic approaches and the applications of the resulting microarrays and libraries, including digital data storage in DNA, nucleic acid-binding proteins, spatial transcriptomics, and CRISPR-Cas systems. He is a partner in DiDAX, part of DigNA – a portfolio of projects funded under the 2022 EIC Pathfinder challenge: DNA-based digital data storage. He is also a co-founder of Vienna-based Helices Biological Photolithography, a company that manufactures equipment for photolithographic nucleic acid synthesis.


## Watch the Recording


Whether you're a researcher exploring DNA synthesis technologies, working on[DNA data storage](https://kilobaser.com/post/dna-data-storage) applications, or simply curious about the future of molecular biology, this webinar offers valuable insights from leading experts and 80 participants.


Explore more about the[DigNA portfolio](https://kilobaser.com/digna-dna-data-storage) and Kilobaser's role in the[DNAMIC project](https://kilobaser.com/dnamic-dna-data-storage) .


[Watch Recording on YouTube](https://www.youtube.com/watch?v=0xTcPJKWVJQ)


## Full Transcript


Read the full transcript of the webinar below. Co-organised by[PEARL-DNA](https://www.pearl-dna.eu/) and moderated by Robert Grass, this session featured insights from Martin Jost and Mark Somoza with 80 participants.


-


**Robert Grass:** Let's start. My name is Robert Grass. I'm the host of this webinar, which is hosted by the[DigNA DNA data storage portfolio](https://kilobaser.com/digna-dna-data-storage) of the European Union. Today we are focusing on[DNA synthesis](https://kilobaser.com/technology) . Martin Jost and Mark Somoza will in two parts give you an introduction into the aspects of chemical DNA synthesis.


Martin Jost is co-founder and Chief Scientific Officer at[Kilobaser](https://kilobaser.com/company) , a company that sells devices that make[DNA synthesis](https://kilobaser.com/dna-and-rna-synthesizer) possible. Mark Somoza is Associate Professor of Chemistry at the University of Vienna and has worked for the past 15 years on DNA synthesis on arrays. There's an interesting side note: all three of us have a connection to Austria — Martin and Mark both work there, and I originate from Austria.


This webinar is recorded and will be published on YouTube. If you have any questions, please wait until the end and ask via the Q&A chat — we'll collect questions at the end. With that, Martin, may I ask you to start?


-


Thank you for the introduction, Robert.


Let me start by giving some more context on myself. As a kid, I enjoyed exploring software and got some early job experience as an autodidact programmer. But after finishing technical high school, I realized I didn't want to work on other people's projects for my entire life — and at the same time, I wanted to know how life really works on a molecular level.


I started studying Molecular Biology and some computer science in Graz, Austria. After some time, me and a few friends wanted more hands-on time in the lab to work on our own projects. So we founded OLGA, a community lab for students and the general public. We even obtained a license to work with genetically modified organisms.


After a few months, we stumbled upon a paper describing a microfluidic DNA synthesis chip. By chance, we got access to a few thousand euros of venture capital, so me and my co-founders dropped out of university in 2014 to focus on[Kilobaser](https://kilobaser.com/company) . Against many odds and with the help of early believers and investors, we managed to develop the basic idea into a product and bring it to market. I believe we even introduced a few nice things to the field.


In 2022, we joined the[DNAMIC consortium](https://kilobaser.com/dnamic-dna-data-storage) — a journey to integrate decentralized[DNA synthesis](https://kilobaser.com/technology) capabilities into an autonomous robotic platform.


-


Since most of you probably have some interest in[DNA data storage](https://kilobaser.com/post/dna-data-storage) , you're probably not completely alien to the idea that DNA is a lot more than just a biopolymer. This is the thing that really excites me: it is actually one of the very few ways we have to speak to the molecular world in its own language.


From my perspective as a computer nerd, DNA ticks all the boxes: it is, of course, the programming language of life. Everything is data. In theory, we can use it to reprogram cells to make anything we want, or even start from scratch and design new forms of life. By now I've learned that things are in practice a little bit more tricky, of course.


Nevertheless, there are hundreds of examples where DNA is used exactly for this. A very prominent one are the COVID mRNA vaccines, which really only provide the code or recipe, and utilize the body's molecular machinery to make the actual antigen. DNA can be selectively copied, extracted, put together, and inserted into other organisms — the classic example being human insulin made by microorganisms in bioreactors. And[synthetic DNA](https://kilobaser.com/dna-and-rna-synthesizer) is of course being used to guide the molecular scissors of CRISPR and similar techniques, probably one of the most direct ways of interaction with the biological world on a molecular level.


This was my personal reason to join the field, and I'm still quite excited about what we will be able to do with this power. If you're interested in such visionary perspectives, I can warmly recommend the book *The Genesis Machine* , written by one of my first mentors, Andrew Hessel.


-


We all have a certain image in mind when thinking about DNA. DNA can be seen as a fine string with the naked eye. Under an electron microscope, the famous double helix becomes visible. In its natural form, it's a huge chain of bases folded in the most amazing ways inside a tiny cell.


But in the context of actually *writing* DNA, we think of it in a very different, quite abstract way: we don't care anymore about the well-known double helix. Not even the complementary bonding of A and G, C and T, is overly interesting for our purposes. Synthetic DNA is made as a single strand — a single chain of bases. We want to build it artificially, base by base.


-


In order to do that, we have to solve some fundamental questions. First: how do we control the sequence?


Chemistry works in liquids. If we put a molecule and potential reaction partners in a liquid, and the molecule has reactive sites, it will not react once — it will instead form a chain or polymer as long as there are reaction partners in the same liquid. This is quite obviously not what we want.


In chemistry, this is solved by protecting the reactive site with **protective groups** . In a next step, some protective groups are removed by exposing the product to a deblocking reagent with specific properties. Only then does the reactive site become reactive again. Using a different deblocking reagent removes a different protective group. So we can precisely control where the next reaction will happen.


Specifically for[DNA synthesis](https://kilobaser.com/technology) , we are talking about a backbone of circular sugar molecules connected with phosphate groups. The well-known bases adenine, guanine, cytosine, and thymine — short A, G, C, T — are attached at the other end of the sugar. So the principle is simple: deblock the chain, couple the next base. The next base then becomes the new end of the chain. This is repeated in a cyclic fashion: deblock, couple, deblock, couple, and so on.


Remember: all of this takes place in solution. This means all chains float around freely in a liquid while another liquid is added to the mix.


The issue is that for each step, the emerging chain, or product, has to be taken out of its solution, filtered, purified, and then submerged in the next solution for the next reaction step. If you're going to make a long chain base by base, this becomes very tedious.


A very clever solution to this is tying one end of the chain to a surface while flushing all the reagents over it. This is called **solid-phase synthesis** — if you were asking yourself what this webinar's title refers to. With this approach, we can conveniently flush reagent after reagent over a surface, and the nucleotide chain grows without being flushed away.


An actual solid phase looks more like a very porous powder filled into columns than a flat surface — though there are exceptions, as we'll see later today. Another thing to keep in mind: in reality, there are trillions of chains with exactly the same sequence on the same surface.[DNA synthesis](https://kilobaser.com/dna-and-rna-synthesizer) in the lab usually happens at pico- or nanomolar scale — that means about 10¹⁴, or roughly 100 trillion, molecules are being made at the same time.


So we have all the ingredients, and we can repeat this in a cyclic fashion over and over until we have a DNA chain with the sequence and length we desire. Problem solved.


-


Maybe let's now take a quick step back and look at the history of[DNA synthesis](https://kilobaser.com/technology) .


**1955 — Michelson & Todd:** In 1955, Michelson and Todd reported the first directed chemical synthesis of a dinucleotide — two thymidine residues joined by a phosphate link. This was purely liquid-phase chemistry, painstaking and low-yield, but it proved that chemical synthesis of nucleic acid fragments was possible.


**1960s — Har Gobind Khorana:** In the 1950s and 1960s, Har Gobind Khorana developed the phosphodiester approach, which allowed the systematic assembly of defined oligonucleotide sequences. These synthetic oligonucleotides were instrumental in cracking the genetic code, earning Khorana the 1968 Nobel Prize in Physiology or Medicine, shared with Nirenberg and Holley.


Khorana's group developed most of the protective groups still used in modern synthesis, such as the DMT (dimethoxytrityl) group. This group is bright red and can be used to monitor the progress of a synthesis in real time. The groups protecting the nucleobases are also still in common use. In 1970, Khorana's group achieved the first total synthesis of an artificial gene — a monumental accomplishment, but an incredibly laborious one.


**1965 — Robert Letsinger:** In 1963–1965, Robert Letsinger introduced two transformative ideas. He introduced the phosphotriester method, and later the more reactive phosphite triester approach, which dramatically improved coupling efficiency. Most importantly, he pioneered solid-phase oligonucleotide synthesis — borrowing from Robert Merrifield's solid-phase peptide synthesis concept. Merrifield received the Nobel Prize for this work in 1984.


**1981 — Marvin Caruthers:** In 1981, Beaucage and Caruthers published the **phosphoramidite method** . Marvin Caruthers, working at the University of Colorado, made what appears in hindsight to be a modest chemical modification of Letsinger's phosphite triester approach: he swapped one leaving group (a chloride) for another (a diisopropylamino group). But this subtle change was pivotal.


The resulting phosphoramidite building blocks were stable enough to be manufactured, stored as solids, and shipped — then activated on demand by simply adding a weak acid (tetrazole). This made commercial-scale production and distribution of[DNA synthesis reagents](https://kilobaser.com/webshop) viable.


**1980s — The Golden Age of Automation:** The collaboration between Caruthers and Leroy Hood at Caltech led to the founding of Applied Biosystems, Inc. (ABI), which commercialized the first phosphoramidite-based[DNA synthesizer](https://kilobaser.com/dna-and-rna-synthesizer) . Meanwhile, Biosearch developed the first commercially successful instrument, the SAM 1 (Synthesis Automation Machine). Suddenly, labs around the world had routine access to custom oligonucleotides. The timing was perfect: the invention of PCR in the 1980s created explosive demand for short[DNA primers](https://kilobaser.com/post/dna-printer) , driving the industry into a golden age.


**Enzymatic DNA Synthesis:** More recently, enzymatic DNA synthesis has attracted significant attention. The core idea is to use a template-independent polymerase — TdT (terminal deoxynucleotidyl transferase) — to add nucleotides one at a time under mild conditions in water. The principle, first proposed in 1962, is the same cyclic chain extension: nucleotides are blocked at the 3' end, and after each addition, the blocking group prevents further extension until it is removed. The appeal is clear: aqueous, room-temperature chemistry, no hazardous organic solvents, and potentially longer chains.


However, the first commercially available synthesizer based on this technology was only released by French company DNA Script in 2022. It is interesting that both approaches share a fundamental challenge: in cyclic chain extension reactions, the error or failure rate multiplies with each added base. Regardless of the chemistry, it is extremely important to bring the yield of a single cycle to extremely high values of over 99% — otherwise the approach does not lead to practical results. I'll come back to that in a moment.


-


Let's have a closer look at the phosphoramidite cycle. Remember the fundamental concept: first deblocking, then coupling of the next base, which contains the next protected group for chain extension.


In the phosphoramidite cycle, we have five steps: **deblocking, activation, coupling, oxidation, and capping** .


**Deblocking:** A linker is attached to the solid surface. Either the linker or the previous base is protected with a DMT group. This is removed by flushing with a weak acid.


**Activation:** The activation step removes the protective group blocking the phosphate group of the next amidite. The activation solution is mixed with the desired phosphoramidite, then flushed through the column.


**Coupling:** The activated amidite connects with the deblocked chain at the 5' end, forming a phosphite triester group.


**Oxidation:** Oxidation of the phosphite triester group to a phosphate triester group strengthens the bond between the chain and the newly added nucleotide — otherwise it would be cleaved under the acidic conditions of the following deblocking step.


**Capping:** Now, here is an interesting part: the cycle actually contains a step to block unreacted chains from further participation in the next cycle. The capping step puts an acetyl cap on any unreacted but already unblocked chains. This is a very efficient reaction — I'll come back to that in a minute.


**Cleavage / Final Deprotection:** Finally, the solid support is exposed to basic conditions — often ammonia — to release the chain from its support. Then the product is treated for an extended period of time to remove the remaining protective groups from the backbone and nucleobases. This is typically done on a separate machine and is not part of the automatic synthesis cycle.


-


Let's come back to a few core concepts. Synthesis always happens in 100 trillion instances at the same time. Some of these reactions fail. Even with 99% synthesis fidelity, 1 trillion chains fail to extend with each cycle — meaning no base is added.


If the process continues without capping, the next base is attached at the unreacted site, leaving out the intended base. This introduces a **deletion** , which in the final chain is a mutation of the actual sequence we wanted. After some cycles, many kinds of different sequences will appear. The intended one is still dominant, but the number of mutations increases with each cycle. In the end, we have a mixture of various sequences in our final product. To make things worse, if only a single base is missing, the length is similar to the intended product — which makes separation and purification much harder.


If **capping** is introduced, the failed chains are not extended further. With capping, we still have truncated sequences in the final mixture, but for most applications this is preferable over a wild mix of different sequences with deletions at several points.


The multiplicative effect of stepwise yield can be shown clearly:


Length 98.5% yield 99.0% yield 99.5% yield


20-mer 74% 83% 90%


50-mer 47% 61% 78%


100-mer 22% 37% 61%


150-mer 10% 22% 47%


200-mer 5% 13% 37%


An already very good value of 98.5% for a chemical synthesis process leads to 25% product missing after only 20 bases. After 50 bases, only half of the product is left — it doesn't make sense to go much beyond that length with such a yield. But even with an extremely high stepwise yield of 99.5%, 200 bases is quite the limit for any meaningful synthesis result. With phosphoramidite chemistry, 200 bases are typically only reached under optimal conditions by service providers specifically optimizing for this use case.


If one needs longer sequences — or even plasmids with thousands of bases — this is done by making an oligo pool using parallel synthesis of different sequences, then assembling them with enzymatic processes.


**Types of synthesis errors:**


In reality, we never get pure product directly out of any[synthesizer](https://kilobaser.com/dna-and-rna-synthesizer) . There are three main types of errors:


-


**Truncated sequences** (most common): Capped failures — chains that didn't extend and were permanently terminated. Shorter than the target.
-


**Deletion sequences** (most insidious): Capping failed to catch a coupling failure. The chain resumes extending but is missing one or more bases. Near full-length, and therefore the hardest to remove.
-


**Mutant sequences** (rare but real): Substitution errors from chemical side reactions (e.g. G→A from guanine amination during capping). Correct length, wrong bases.


Purification methods include desalting, RP-HPLC (removes truncations), PAGE (single-base resolution), and ion-exchange HPLC.


The possible applications have different sensitivity to impurities.[PCR](https://kilobaser.com/post/real-time-qpcr-application-note) , for example, is quite tolerant and even works with unpurified crude product. In DNA assembly for longer sequences, on the other hand, you want product that is as pure as possible, because errors will propagate even further.


This means, depending on the application, quite a few laborious purification steps are needed before the DNA can actually be used. Specifically for[data storage](https://kilobaser.com/post/dna-data-storage) , there are ways to circumvent this hard requirement or even exploit the properties of error characteristics. This is the expertise and goal of several working groups within the[DigNA portfolio](https://kilobaser.com/digna-dna-data-storage) — an excellent example of out-of-the-box thinking that can push an entire industry forward. I'm very glad[Kilobaser](https://kilobaser.com/company) and I can be part of this initiative.


-


Of course, this presentation wouldn't be complete without a quick mention of what[Kilobaser](https://kilobaser.com/company) has contributed to the field.


The idea for Kilobaser's[chip](https://kilobaser.com/product/standard-chip) is based on a series of microfluidic papers published by the Quake group around 2006. Unfortunately, microfluidic technology is infamous for being hard to mass-produce. If active actuation of liquids is embedded into the chip, things become even more difficult. For[DNA synthesis](https://kilobaser.com/technology) , the whole system must also be resistant against harsh solvents, or those solvents need to be substituted.


We transformed the concept to **injection-molded polymer chips** , where liquids are actively controlled with pneumatic on-chip membrane valves. The benefits: all fluidic channels are embedded in a[disposable chip](https://kilobaser.com/product/standard-chip) — everyone who has ever operated a classic synthesizer knows that tubing and valves need constant attention and maintenance. Since the distance between column and valves is much smaller and there are no tubes to fill and empty with each cycle, the device also needs a lot less reagent.


The overall volume of reagents needed for a 150-base synthesis is only around **18 milliliters** . This means we are able to store all reagents — and even the waste — inside a single compact[cartridge](https://kilobaser.com/product/standard-dna-cartridge) . Scientists without experience in chemistry, or even robots, can operate the device.


Another key innovation: cleavage and final deprotection are integrated directly into the device. Usable product comes directly out of the machine — no separate steps required.


This also means that a robot, like the microfactory developed within our[DNAMIC consortium](https://kilobaser.com/dnamic-dna-data-storage) , can access and use freshly made DNA fully autonomously and without human intervention.


With that, I'd like to hand over to Mark, who will now take us from the single oligo to the library scale.


-


Part 2 of the webinar was presented by Prof. Mark Somoza, Associate Professor of Chemistry at the University of Vienna. His talk covered the transition from single-oligonucleotide synthesis to massively parallel[DNA synthesis](https://kilobaser.com/technology) using photolithographic microarray approaches, including applications in[digital data storage in DNA](https://kilobaser.com/post/dna-data-storage) .


[Watch the full presentation in the recording](https://www.youtube.com/watch?v=0xTcPJKWVJQ) .


## Want to read more exciting articles in the future?


Are you interested in our new products and don’t want to miss a thing? Subscribe to our newsletter and stay up to date on what’s going on!


[Join our e-mail list](https://kilobaser.com/newsletter-registration)


Share this article:


[previous Introducing the New Hexynyl Cartridge: Revolutionary Click Chemistry at Your Fingertips](https://kilobaser.com/post/introducing-the-new-hexynyl-cartridge-revolutionary-click-chemistry-at-your-fingertips)[next SCDNA26: Storage and Computing with DNA Conference in Rome](https://kilobaser.com/post/scdna26-storage-and-computing-with-dna-conference-rome)
