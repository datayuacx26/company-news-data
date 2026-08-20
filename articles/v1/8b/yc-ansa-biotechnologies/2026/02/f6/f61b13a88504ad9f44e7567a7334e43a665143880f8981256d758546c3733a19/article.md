---
schema_version: "1.0.0"
document_id: "f61b13a88504ad9f44e7567a7334e43a665143880f8981256d758546c3733a19"
company_key: "yc-ansa-biotechnologies"
company: "Ansa Biotechnologies"
source_id: "yc-ansa-biotechnologies-news-import-e9d93bf98b8b"
canonical_url: "https://ansabio.com/blog/ansas-enzymatic-dna-synthesis-technology-where-it-all-began/"
published_at: "2026-02-23T22:10:39+00:00"
first_seen_at: "2026-07-23T22:03:04.723228+00:00"
fetched_at: "2026-07-28T21:26:27.452652+00:00"
content_hash: "sha256:d99d73fb2c4c31ec792f62bcf6ea5dd53bc1cca688f350afea78e32f61d4dd5e"
---

# Ansa’s Enzymatic DNA Synthesis Technology: Where It All Began

Ansa was founded to break through the market’s low expectations and poor customer service. Our technology shines where conventional DNA synthesis stumbles. But we understand the skepticism and the need for evidence (hey, we’re scientists too).


We often wind up directing folks back to our foundational[Nature Biotechnology publication](https://www.nature.com/articles/nbt.4173) , the one where we first reported our unique approach to enzymatic DNA synthesis. If you don’t have a subscription, the University of California has made the paper freely available as a PDF[here](https://escholarship.org/uc/item/9d4221mc) . And if you don’t have time to read the whole paper, we’ve summarized some highlights below.


## Why we went enzymatic


We didn’t start off as DNA synthesis technology developers; we started off as customers. We were stunned to learn how difficult it was to acquire the DNA we needed for our experiments — and when we traced the problems back to their roots, most of them stemmed from the chemical synthesis approach used by most DNA manufacturers. While chemical synthesis has been a huge boon to the life sciences over the decades, its error rate and sequence length appear to be near peak performance. Unfortunately, those metrics are still not good enough to build the wide variety of DNA required for today’s experiments.


We believed that an enzymatic approach to DNA synthesis would be better because it would allow us to harness the power of biology to build DNA with greater accuracy, length, and speed. It struck us that an enzymatic method would be able to handle the kinds of complex sequence elements that challenge chemical synthesis, a huge area of need as the demand for synthetic DNA soars. We also liked the idea that, unlike chemical techniques, enzymatic synthesis produces no hazardous waste.


## A different take on TdT synthesis


We certainly weren’t the first scientists to get interested in enzymatic DNA synthesis — we weren’t even the first to turn to terminal deoxynucleotidyl transferase (TdT) for the polymerase that would help build our DNA. But most prior attempts to use TdT had been impractical as a commercial synthesis technique, plagued by inefficient incorporation of nucleotides.


We came up with a different take on using TdT: a two-step reaction that would be effective and efficient for adding new bases. Here’s how we described it in the 2018 paper: “We conceived of an approach for reversible termination wherein each polymerase molecule is site-specifically labeled with a tethered nucleoside triphosphate.” That latter element, known as dNTP, is crucial. The paper describes it this way: “When a polymerase incorporates its tethered dNTP into a primer, it remains covalently attached to the 3′ end, blocking further elongation by other polymerase-dNTP conjugates. The linker can then be cleaved to deprotect the 3′ end of the primer for subsequent extension.”


What we wound up with is a two-step reaction, cycling between extension and deprotection, that can be used over and over to synthesize the desired DNA sequence.


## Why TdT–dNTP conjugates matter


Even in its earliest stages, one of the advantages of our approach compared to other enzymatic synthesis techniques was that the elements we use — including the tethered dNTPs — were as close as possible to the elements used in naturally occurring DNA synthesis. Scientists had tried to use 3′ O-modified reversible terminator dNTPs instead, but as we put it in the Nature Biotech paper, those “are typically poor substrates for natural polymerases.”


“By contrast, tethered 3′-unblocked dNTPs are identical to the native substrates in the region that contacts the highly conserved catalytic site,” we added, “so we expect they can achieve native-like incorporation kinetics.” We hadn’t ironed out all of the technical wrinkles in the approach at the time, but we believed there were avenues to pursue that would speed up the reaction time and make our synthesis method highly efficient. (Spoiler alert: they worked!)


## Feasibility testing FTW


While our first paper didn’t show huge amounts of data, we did demonstrate the feasibility of our approach. We built a 10-mer oligo using 10 rounds of our extension and deprotection cycle, with conjugates for our specific sequence: 5′-CTAGTCAGCT-3′. To check our work, we poly-A tailed the oligo and then amplified and sequenced it. More than 80% of the sequencing reads contained the complete intended sequence. We also made an attempt at a repeat — the sequence 5′-CCC-3′ — since these are biologically important but challenging to synthesize with chemical methods. In this case, 88% of sequencing reads contained the correct repeat sequence.


We concluded our report on an optimistic note: “We believe that the presented scheme offers a promising starting point for the development of a practical enzymatic DNA synthesis technology.” It’s safe to say that at the time we couldn’t have realized just how promising this approach was.
