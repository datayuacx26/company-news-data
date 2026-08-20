---
schema_version: "1.0.0"
document_id: "7096aaffeef28ba7495bcd8232759ecbf85275ac2354d5624a67d1af395b086c"
company_key: "yc-ansa-biotechnologies"
company: "Ansa Biotechnologies"
source_id: "yc-ansa-biotechnologies-news-import-e9d93bf98b8b"
canonical_url: "https://ansabio.com/blog/toward-genome-engineering-at-massive-scale/"
published_at: "2025-07-01T16:11:14+00:00"
first_seen_at: "2026-07-23T22:03:04.723228+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:84f0cef5b7089236fc4e12e43b59ae1fcdbe8cc3839c553b132fa98122efffcc"
---

# Toward Genome Engineering at Massive Scale

*By Jared Ellefson, PhD*
*Ansa Head of DNA Assembly and co-founder*


When I co-founded Ansa Bio, my aim was to move synthetic biology beyond the punch-card era. Digital software has reshaped the modern world, and many predict that mastery of biology will have a comparable impact. Yet imagine if compiling a single line of Python took days, a simple script required weeks, and only a handful of programs had ever been completed—that scenario still describes life sciences today. Discovery is throttled by slow, fragile DNA synthesis, and design tools rarely scale beyond individual genes. I’m excited that Ansa has begun to chip away at this bottleneck: our new early access service lets researchers order up to 50 kb of DNA in a single step, giving biologists the confidence to pursue bold ideas without worrying their “software” will never arrive.


### **Re-engineering DNA Synthesis from the Ground Up**


My research career began in the Ellington Lab at UT Austin, an unconventional and well-funded environment that tackled diverse projects in synthetic biology and directed evolution. As a rotation student, I was stunned that ordering just twenty clonal genes from Blue Heron cost more than my entire annual stipend. Later, while engineering transcription factors and genetic circuits, I repeatedly hit a wall: regulatory elements often contained repetitive sequences, making commercial synthesis impractical. Necessity forced me to learn how to build complex gene fragments myself.


That experience laid bare a major inefficiency in biological research. Graduate students everywhere grappled with cloning—a skill rarely aligned with their core scientific questions. Those who mastered it advanced quickly, while others lost months to repetitive construct-building—at least until they pledged fealty to the “dark cloning arts.” This persistent frustration highlighted a fundamental problem: the lack of easy DNA access was crippling research progress. Custom sequences were slow, unreliable, and often impossible to obtain. Worst of all, entire fields—such as synthetic genomics—remained accessible only to the best-funded labs.


Around the time I considered leaving academia, I read a paper describing enzymatic DNA synthesis with terminal deoxynucleotidyl transferase (TdT). The concept resembled a molecular 3-D printer: add nucleotides one by one under gentle, aqueous conditions instead of the harsh chemistry that limits phosphoramidite synthesis to ~300 nt. If we could extend that reaction thousands of times in series, painstaking assembly steps would vanish.


Ansa’s platform now relies on proprietary TdT-nucleotide conjugates that produce the longest, highest-fidelity oligonucleotides available. Because we synthesize genes directly, we bypass the polymerase-chain-assembly steps that force competitors to stitch together dozens of short fragments. By removing this weak link, our entire pipeline accelerates, improving speed and reliability simultaneously.


Reliability is critical. We tested 500 bp–5 kb sequences through a leading supplier’s API and watched rejection rates soar with length; at 5 kb, fewer than one in five constructs were accepted, often because of homopolymer runs, extreme GC content, or repeats. Those same designs often pass through our system on the first attempt. These parts seamlessly integrate into Ansa Assembly, a one-pot, sequence-agnostic approach capable of assembling many fragments in one go—overcoming limitations inherent to Gibson and Golden Gate methodologies. The same process can be iterated until plasmids are 50 kb and simply fall off the line on the first‑try.


Competitors treat a 50 kb plasmid as a gamble, demanding milestone payments and offering uncertain timelines because every added kilobase increases the chance of running into an “impossible” sequence. We built our platform by mastering these sequences first, so what others see as edge cases are routine for us—one robust workflow, no hidden hurdles. At Ansa, you upload the sequence, receive a tracking number, and move on to your next experiment. Tasks once deemed heroic become standard practice, and genome re-synthesis shifts from aspiration to routine.


### **Unlocking Large-Scale Biological Research**


When DNA length is no longer a constraint, formerly daunting projects become everyday science. Recoded bacterial genomes like Syn57 promise virus-proof industrial strains capable of novel chemistries. Humanized drug-discovery models, exemplified by Alloy’s ATX-Gx mice, show the value of swapping hundred-kilobase immune loci to accelerate therapeutic development. As synthetic DNA moves from scarcity to abundance, agricultural teams will engineer complex traits in a single season, industrial microbiologists will refactor entire pathways at once, and natural-product chemists will transplant full biosynthetic clusters into amenable hosts—progress that once required years. These examples are not speculative—there are early adopters already generating compelling data. Wider dissemination of these capabilities is the next essential step.


Wet-lab horsepower alone will not usher in a new era of biological coding. Biology is still drafted “one gene at a time” because mistakes are catastrophic and abstraction layers are thin. But signs of a coming shift are everywhere. Large language models trained directly on raw genomic text could act as grammar-checkers and auto-completion engines for DNA, catching frame-shifts, missing Kozak sequences, and missing stop codons before they cost real money. Multi-modal AI approaches could combine structural insights from tools like AlphaFold with gene-expression profiles and mutational impacts, simplifying analyses previously requiring PhD-level expertise. Crucially, these computational innovations become truly transformative when coupled with dependable long-DNA synthesis. In software, rapid compilation cycles foster innovation; the biological equivalent—design-build-test—is similarly dependent on swift, reliable DNA synthesis. When DNA synthesis achieves cloud-compute economics, genomes can finally be treated as fully malleable code bases.


> As costs fall, the question shifts from “What can we afford?” to “What can we imagine?”


### **Writing the Human Genome**


The Human Genome Project taught us to read DNA; the next frontier is learning to write it. Reliable 50 kb building blocks lay the groundwork for fully understanding—and eventually engineering—the human genome. A compelling application is a functional atlas of human variation. Today we infer biology by statistically linking variants to phenotypes, yet genome-wide association studies have delivered only a handful of unequivocal causal discoveries. Whole-genome synthesis allows us to engineer specific variants and observe their effects directly in stem cells or organoids, clarifying how sequence dictates health, disease, and therapy response. Developmental biologists can replay embryogenesis under controlled genetic perturbations, and longevity researchers can order custom haplotypes to probe resilience to aging.


Therapeutic possibilities follow. Imagine replacing single-gene AAV cargo with programmable human artificial chromosomes carrying full pathways, tunable promoters, and built-in safeguards. Such chromosomes could restore metabolic functions in rare diseases, rejuvenate immune systems with embedded CAR libraries, or equip transplanted organs with tolerance genes—curative edits without double-strand breaks or oncogenic risk.


Each vision demands millions of flawless bases. Enzymatic synthesis and robust assembly make that scale practical; as costs fall, the question shifts from “What can we afford?” to “What can we imagine?”


Software innovation exploded once compilation became cheap and instantaneous. By extending the length, speed, and reliability of DNA synthesis—and pairing it with ever-smarter design algorithms—Ansa Bio aims to push biology across the same tipping point. The punch-card era is ending, and genomes are poised to become the next programming language.
