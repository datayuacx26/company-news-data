---
schema_version: "1.0.0"
document_id: "2e3fffc57805cd8bf2666feaf2fa838d0a2e03c509f5fbee9260a265bb6b8de8"
company_key: "yc-humane-genomics"
company: "Humane Genomics"
source_id: "yc-humane-genomics-rss-c97c4b414edc"
canonical_url: "https://humanegenomics.com/news/improving-the-success-rate-of-engineering-rna-viruses/"
published_at: "2024-11-21T22:12:50+00:00"
first_seen_at: "2026-07-25T08:43:32.933852+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:cd197c5ea14c0107c3555b76f18196cde4385290b9670fc950b991c68390ccfe"
---

# Improving the success rate of engineering RNA viruses

In our previous post on this topic, we introduced the details on how to make a replicating RNA virus from its genome (DNA). In this post, we want to zoom in on the details a bit more and share how we have improved the success rate of engineering viruses to over 70%.


If you have not read the previous post on


[the science of viral engineering](https://humanegenomics.com/news/the-science-of-engineering-viruses/) and want to get a first level understanding on how the process works, go ahead and read that first.


To get from an engineered viral genome to a replicating virus we need several components to be present in the cytoplasm of a cell. If they are not all present, it does not work. These components are all packaged and present in a replicating virus and they are the L-protein (polymerase), the N-protein (nucleocapsid), the P-protein (phosphoprotein) and the viral genome itself.


*Fig.1. Schematic overview of transfection process*


## Ensuring all the right components are present


Let’s start by exploring how to ensure the proteins N, P, and L are available in cells. The challenge lies in the difficulty of working with RNA, keeping it stable and transfecting (inserting) the correct amounts of proteins and RNA into cells. Instead, it is more practical to transfect DNA. However, this approach involves additional steps. A widely used technique involves the use of plasmids. These are small, circular DNA molecules that contain three essential components. A promoter to initiate transcription, the gene encoding the target protein, and a terminator to signal the end of transcription. Many plasmids also include a bacterial backbone, which facilitates their production and amplification in bacteria. To express the desired proteins in cells, we transfect plasmids encoding the N, P, and L proteins. Once inside the cells, these plasmids serve as templates for cellular machinery to produce the necessary proteins.


In addition, the viral genome needs to be transcribed from DNA to RNA. To achieve this, we use a common technique involving a helper virus (vaccinia virus), which is coinfected to deliver a gene expressing the T7 polymerase. The transfected viral genome includes a T7 promoter, enabling the T7 polymerase to transcribe the viral genome into RNA.


## Increasing the success rate


In summary, for successful “boot-up” of our virus, three plasmids, the viral genome, and a successful vaccinia infection must all be present in the same cell. While it might seem tempting to use large quantities of these components to increase the likelihood of success, each is toxic to the cells. Adding to the challenge, the vaccinia virus is a replicating virus that kills cells and must be removed later. It’s no surprise that, until the publication of “


[this paper](https://www.nature.com/articles/s41598-024-65384-8#Sec10) ,” success rates for viral boot-up were


[reported to be](https://journals.asm.org/doi/pdf/10.1128/msphere.00376-16) less than 1%.


To reliably test our viral components and therapeutic viruses, we require a robust platform. Considerable effort has gone into improving the success rate of viral boot-up while reducing complexity. Determining the optimal amounts of each plasmid, the viral genome, the vaccinia virus, and the relative number of cells is critical. Over the past six months, we have achieved an average success rate of over 70%.


## Further simplifying the process by removing vaccinia


To further improve on that, we have successfully replaced vaccinia virus infection with a T7 polymerase plasmid, greatly simplifying the process. This change eliminates the need for labor-intensive and time consuming steps such as plaque purification or virus filtration. It removes a competing factor during transfection, further improving success rates. Importantly, it also removes a biosafety risk, enhancing lab safety for the team.


We are very pleased with the progress we have made and believe that, in the near future, the success rate for established, well-optimized genomes could exceed 90%.
