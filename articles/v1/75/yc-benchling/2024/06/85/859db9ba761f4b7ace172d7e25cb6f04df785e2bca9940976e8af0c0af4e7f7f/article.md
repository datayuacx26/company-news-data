---
schema_version: "1.0.0"
document_id: "859db9ba761f4b7ace172d7e25cb6f04df785e2bca9940976e8af0c0af4e7f7f"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/protein-language-models-promises-pitfalls-and-applications"
published_at: "2024-06-18T07:00:00+00:00"
first_seen_at: "2026-07-31T17:50:58.980836+00:00"
fetched_at: "2026-07-31T17:51:03.853606+00:00"
content_hash: "sha256:a2ec0bf62fe2010cfe80d9155ed8817e6aa79e12f6ddbe1028c54df37f4ca538"
---

# Protein language models: promises, pitfalls and applications

### Introduction


Recently, tremendous advancements in the field of artificial intelligence have been achieved, especially the development of large language models (LLMs). Within the last few years, we have been introduced to numerous LLMs that showed the potential to improve our life due to their unprecedented performance in not only daily use, but also in antibody engineering and biologics design 1.


### How do large language models (LLMs) work?


LLMs learn statistical patterns from large text training data derived from natural languages (e.g. English, Danish or Arabic), after breaking down the bulk text into smaller tokens such as words and symbols (Figure 1). Based on what they learn during the training process, LLMs could perform several tasks such as text completion, translation, summarisation and question answering **.** Famous examples of LLMs include BERT (Google)2, RoBERTa 3 (Facebook AI) and GPT3[4](https://paperpile.com/c/exS9Qf/sFfX) (OpenAI).


**Figure 1** . Simple example of tokenization in the setting of natural language taken from a piece of English literature 5.‍


### The emergence of protein language models (PLMs)


‍The good performance of these LLMs was mainly attributed to the unique transformer **-** based deep neural network structures and the attention feature **** 6 which allows the models to learn the long-range text dependencies. As such, LLMs are able to learn the relative importance of different tokens (technically known as “weight”) in relation to the rest of the tokens in a given sentence or paragraph 7.


‍Because of their good performance, and the hierarchical similarities between natural languages and protein sequences (Figure 2), scientists are applying similar approaches to develop protein language models (PLMs). In PLMs – Analogously to LLMs (Figure 2) – amino acid sequences are treated as a stretch of text to train machine learning models 8.


‍Thus, PLMs aim to learn the relationship between protein sequences, their structures, and ultimately their functions **** (sequence-structure-function) 9,10.


However, in natural language, texts contain integral symbols, spaces and punctuation (orthography - Figure 1) that divide linguistic sequences into meaningful tokens which makes learning the rules and the grammar of the language feasible and interpretable 9,11. Such symbols and means of division are absent in abstract protein sequences. Also, proteins are multi-dimensional molecules with significant complexity that is far beyond their linear sequences (Figure 2). All together, these factors hinder achieving meaningful protein sequence tokenization beyond the single amino acid level.


**Figure 2.** The conceptual similarities and hierarchical structure as seen in natural languages and proteins. Inspired by 8. Monomer IgM from 12. Pentamer IgM structure from 13. This analogy is for approximation and communication purposes only.


### Applications of PLMs in protein design


‍While the core differences between textual and protein sequences present a challenge, PLMs have proven very valuable to learn the underlying patterns of protein sequences even in the absence of prior biological knowledge (technically known as self-supervised or unsupervised learning) 14–16.


‍When biological knowledge is included during training (technically known as supervised learning), PLMs outperform better across a variety of protein engineering and function prediction tasks, as such knowledge is important for learning relationships and patterns that are not obvious in the raw protein sequences 17. In this context, biological knowledge can be in the form of gene ontology (GO) 18, multiple sequence alignment (MSA) 19, cDNA 20 and/or cellular compartment 21,22.


‍PLMs could perform several tasks *in silico* that promise to reduce the time and effort required to perform these tasks using the gold-standard lab-based techniques. For example, Meier et al. developed the ESM-1v model which could investigate the effect of sequence modifications on protein function 23, surpassing the need for labour-intensive deep mutational scanning experiments 24. Other tasks that PLMs could help with include structure 25 and binding site prediction 26.


‍When PLMs are trained on large datasets of proteins from public databases – such as UniProt 27 – they are referred to as general-purpose PLMs. Few examples of these PLMs are showcased in Table 1. However, antibodies are among several protein families which harbour structural uniqueness (as we discussed in this[article](https://pipebio.com/blog/ab-structure-and-developability) ) and are under-represented in these databases. This jeopardises the generalisability of the rules learned from all proteins on antibodies and their design for therapeutic purposes. These factors translated into voices that either praise 28 or criticise 16,29 the performance and the advantages of using general PLMs for antibody-specific design tasks. Also, they motivated the development of antibody family-specific language models (AbLMs), by either fine-tuning pre-trained general PLMs, or training similar machine learning models solely on antibody sequence data.


**Table 1** . Few examples of general protein language models


PLM


Developed by


Training set


Prior biological knowledge


Reference


ESM-1v


Facebook AI (USA)


UniRef90: 98 million sequences


No


23


ESMFold


Meta AI, NYU, Stanford and MIT (USA)


UniRef and PDB


No


25


ProGen


Salesforce and Profluent Bio (USA)


UniParc: 280 million


Yes: Cellular compartment, biological process, molecular function, etc


22


proteinBERT


University of Jerusalem (Israel)


UniProtKB/UniRef90: 106M proteins


Yes: gene ontology (GO)


18


### Antibody-specific protein language models (AbLMs)


‍Monoclonal antibodies are one of the most prominent and successful biotherapeutics, but their clinical development and market success rate remain hindered by lengthy discovery and engineering steps 11,30. AbLMs are rapidly evolving to address these hurdles by enabling computational-based generation, screening and design of antibody candidates. These AbLMs (Table 2) are mainly trained on the large-scale (and still evolving) databases of natural antibody sequences which are made available via public domains such as iReceptor 31 and Observed Antibody Space (OAS) 32.


‍Several currently-available AbLMs could aid the optimisation of either developability or target binding for antibody candidates. For example, Sapiens and DeeAb are two models which have shown good results in suggesting antibody variants with decreased immunogenicity and enhanced thermal stability 33,34. AntiBERTa, an AbLM which was fine-tuned for paratope prediction tasks, showed strong performance in predicting paratope location on the antibody structure in the absence of target information (antigen-agnostic approach) 16.


### Challenges & perspectives


While major advancements have been made in the development of general PLMs and AbLMs and the benefits they offer to the scientific and medical communities, areas of improvement do exist.


‍Generally, the biassed nature of scientific publishing tends to highlight successes over failures which hinders our overall understanding of the applicability of PLMs and how much we can really trust them.


‍Technically, as most PLMs are trained in a self-supervised manner, they are regarded as *black box models* , as even when they perform well, it is challenging to pinpoint what rules they extract from the training data, and how biologically relevant they are 8,9.


‍Also, AbLMs were trained using only unpaired antibody sequences until recently due to the scarcity and high-cost associated with generating paired-chain antibody sequence data 35. Thus, with the exception of two newly introduced models 30,36, most current AbLMs miss out on chain pairing information: a vital factor for a complete and functional antibody molecule.


‍Additionally, the absence of large-scale antibody-antigen data (specificity-labelled antibodies), present another challenge for the implementation of AbLMs 37. A reasonable approach to tackle this issue is to attempt learning the rules of target binding from a large-scale of antibody-antigen variants that originates from a single pair and harbour only subtle differences among them on the sequence level 38,39. This could provide a “baseline” for understanding these rules starting from a relatively less diverse training data before attempting learning them from a more complex and fundamentally different small set of antibody-antigen pairs.


‍Overall, we are invited to go back to the basics of linguistics in order to identify linguistic features in antibody sequences and formalise the language of antibodies (immunolinguistics), which will ultimately aid us to design interpretable and biologically relevant AbLMs 40. Finally, we summarise below a few AbLMs with the aim to present short summaries for some of them in our next blog posts, while introducing our readers to more concepts and techniques of machine learning used to develop these AbLMs.


**Table 2** . Examples of antibody-specific PLMs (AbLMs).


AbLM


Developed by


Paired chain training


Reference


IgLM


John Hopkins University and University of California (USA)


No


41


AbLang and AbLang2


OPIG at Oxford University and GSK (UK)


No


42, 43


AntiBERTa


Alchemab Therapeutics Ltd (UK)


No


16


Bio-inspired Antibody Language Model (BALM)


Fudan University (China)


No


44


BALM-paired and unpaired (trained on BALM)


The Scripps Research Institute (USA)


Yes


30


OAS-trained RoBERTa


AbSci corporation (USA)


No


45


IgBert and IgT5


Exscientia and OPIG at Oxford University (UK)


Yes


36


AntiBERTy


John Hopkins University, USA


No


46


Immune2Vec


Bar Ilan University, Israel


No


47


Sapiens


Merck (USA) and University of Chemistry and Technology (Czech Republic)


No


33


FAbConSapiens


Alchemab Therapeutics Ltd (UK)


Yes


48
