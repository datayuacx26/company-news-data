---
schema_version: "1.0.0"
document_id: "fb0fd4273d89fe39ee30c817534b4d8d541406749d769d745a8a4a96313ec5e5"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/experimenting-outside-the-models-non-model-organisms-the-broad-host-range-solution-and-benchling"
published_at: "2019-09-04T12:31:46+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:4933fba9a32bddd83a004043fb4a06a0a42536d834d3d13f413792f891ffc3a3"
---

# Experimenting Outside the Models — Non-Model Organisms, the Broad Host Range Solution, and Benchling

## The Problem of Working with Non-Model Organisms


Our 2018 UT Austin iGEM team, the Broad Host Rangers, sought to address the problem of genetic part incompatibility in non-model organisms. *E. coli* is the go-to chassis for many synthetic biologists. While this focus on a single organism (or a small subset of organisms) makes it much easier to conduct research, it also makes it much trickier to work with non-model organisms. This hampers research to discover and characterize novel biological pathways found in non-model organisms. This also reduces our ability to exploit and innovate in non-model organisms because we lack the necessary genetic parts. Our iGEM team wanted to develop a toolkit that could overcome this hurdle. The Broad Host Range Kit (BHR Kit) will make it much easier for a researcher to find the most compatible genetic parts for the organism with which they were working. The kit will reliably reveal the most compatible parts for any prokaryote in which it is tested. BHR research has ultimately provided our lab with a large repository of useful part plasmids and contributed to an increased understanding of complex synthetic biology techniques.


### The Plasmid Assembly Line


Richard Feynman famously stated, “What I cannot create, I do not understand.” Relatedly, we cannot design functional broad host range plasmids from parts we do not understand. One of the most critical steps in our work is the characterization of genetic parts. Once we know the basic details of a part, such as the DNA sequence, we can determine how best to work with the part. We can then engineer these genetic parts into plasmids with emergent functions. Although nature has made most of our parts, we can combine, separate, and reorder part elements and modify sequences to make “new,” more efficient, more compatible genetic parts. For instance, because we understand the relative strength of promoters and the relative intensities of chromoproteins (proteins that produce a color visible to the unaided eye and are excellent screenable markers), we are making a series of promoters fused to chromoproteins to fine-tune their expression levels so that color generation is of similar intensity over time.


The core of the BHR Kit is a series of multi-part assembly plasmids. These assemblies, when mixed into a single tube, can be transformed as a batch to show which plasmids contain compatible genetic parts for a given organism; these parts include the origin of replication, coding sequences, or promoters+RBS. Each plasmid contains a single, different chromoprotein. Thus, part compatibility can be assessed by looking at the transformant colonies, where each color correlates to a specific plasmid with a unique DNA sequence. In our schematic below, five different assembly plasmids, each containing a different origin of replication paired to a specific chromoprotein, have been transformed into a non-model organism. The resulting agar plate produces colonies of three colors. Thus, we know that the “blue”, “green”, and “red” plasmids (and their origins of replication) are compatible with our organism. The other two origins (“yellow” and “purple”) do not appear to be compatible with our non-model organism. However, since some chromoproteins may fail to produce color in a non-model organism, each plasmid also contains a unique “barcode” region. Thus, colonies lacking color can have their plasmids sequenced to determine the compatible plasmid’s identity.


**An overview of the BHR kit.** On the left, the five assembly plasmids, each containing a different reporter are combined into one tube and then transformed into a non-model bacterium. The resulting colonies with color (middle, top) allow for easy identification of the functional DNA parts. For the resulting colonies without color (middle, bottom), we can sequence the unique barcode contained within each plasmid.


## The Many Uses of Benchling


Benchling has been essential while working on this project, providing us with tools that are invaluable at nearly every step in our plasmid production and testing pipeline. We utilize Benchling for designing primers, sequence alignments, plasmid assemblies, and notebook keeping.


In order to create new part plasmids, we must design primers for PCR and sequence confirmation, and then align our sequencing results to our reference sequence. Our first step, like many iGEM teams, is to identify DNA parts of interest, often from the iGEM Distribution Kit. Once we identify a DNA part, we use Benchling to build a *plasmid map* with annotations and detailed notes. These plasmid maps serve as templates for us to reference when we create primers or conduct alignments. Next, we design sequencing primers to guarantee that the genetic parts from the distribution kit have the correct sequence. Benchling’s *Primer Wizard* enables us to create primers for many different functions (PCR, adding short sequences to bigger parts, sequencing, etc.), all while being very easy to use. When the sequencing results come back, Benchling’s *Alignment* tool helps us visualize the data. A Benchling alignment shows us if there are any differences between DNA sequences, such as point mutations. The Alignment tool also allows many DNA sequences to be aligned at once, so we can align all of the sequencing results for a single plasmid at once. We also create annotations and translations (DNA → protein) in Benchling, streamlining part characterization. We then apply insights gained through this process to part construction.


The completion of our team’s major goals depended on our ability to create new part and assembly plasmids. For this, we depended on the Primer Wizard to create primers and another Benchling tool to help us model our assemblies: Benchling’s *Assembly Wizard.* This tool allows us to combine multiple sequences together by selecting the appropriate restriction enzyme (normally BsmBI or BsaI, for us) and telling the wizard to assemble the constituent DNA sequences (normally plasmids, but sometimes also PCR products or gBlocks). The Wizard serves as a way to double check our plan. It accurately assembles multiple DNA sequences together, confirming that the overhangs are both specific and directional. Our more complex assemblies could contain half a dozen or more sequences, and on more than one occasion, it caught an oversight, saving us precious time prior to ordering DNA sequences or conducting wet lab experiments. Once the wizard tool gives us a figurative “thumbs up,” we can confidently move forward. Ultimately, we would return to the Benchling Alignment tool to confirm that our assemblies have worked, generating the correct sequence.


It should go without saying that a huge part of our data, notebook keeping, and organizational structure involves Benchling. By having files on the cloud, we can communicate between teammates as well as with our faculty and graduate student mentors to confirm primer designs and share interesting results as well as disseminate our large and expanding database of DNA sequences. This is particularly important as we hand off the project to younger students who are now working with us and learning not only about our project, but about how to use Benchling.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


## Achievements During iGEM


After many cycles of characterization and assembly, our iGEM team had a large repository of genetics parts. The creation of this repository fulfilled several major project goals. The first and most obvious goal was to make as many useful part plasmids as possible. Our team made 48 part plasmids — all of which were sequence-confirmed and labeled clearly on Benchling. From these plasmids, we built eight assembly plasmids. Each assembly plasmid had a unique sequence and could be differentiated from the others. Although eight assembly plasmids were far short of our goal, we were still able to show that these assemblies could be distinguished from one another when transformed into *E. coli* . Finally, we showed that our broad host range plasmids lived up to their name — we were able to successfully transform BHR assembly plasmids into *Vibrio natriegens* and *Serratia marcescens* . The iGEM team at Rice University, with whom we collaborated throughout 2018, replicated our procedure using our assemblies in their lab. Unfortunately, two major hurdles prevented us from completing the BHR Kit: difficulties working with certain origins of replication and the persistent challenges of transforming non-model bacteria.


## Triumphs and Further Experiments


Following our presentation at the 2018 iGEM Giant Jamboree, we continue to design new part plasmids as well as new assemblies. We are designing combinations of promoters and chromoproteins to ensure that color develops at around the same time for each part. This also spares the chassis from unnecessarily over-expressing an easy-to-see chromoprotein, and, in turn, should make the parts more stable. A subgroup within our 2018 iGEM team has taken the project in another direction. They have streamlined the assembly plasmids, focusing on origins of replication and paying special attention to the assembly plasmids’ stabilities and compatibilities in non-model organisms. This same group won the Thermo Fisher Scientific Award for Excellence in Biological Sciences for their presentation of the 2018 iGEM project at the University of Texas at Austin Undergraduate Research Forum in Spring 2019.


---


*This article was contributed by:*


**RYAN BAILEY**


Undergraduate studying Biology, UT Austin / UT Austin iGEM Team Member


*This project was supported by:*


**DR. DENNIS MISHLER**


Assistant Professor of Practice, The Freshman Research Initiative, College of Natural Sciences, University of Texas at Austin / UT Austin iGEM Advisor
