---
schema_version: "1.0.0"
document_id: "c8d5d8f4c3fa847a4f04d9452f0e0b10841e6d70c1eb707a31ea25737d29f9fb"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2022-03-31-improving-evaluation-practices-in-natural-language-generation/"
published_at: "2022-03-31T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:b7316ee2d1cc40c1cdd12e3b5c03c387162b60b01337037a05ff7e918b87c56e"
---

# Improving Evaluation Practices in Natural Language Generation

# Introduction


Throughout last year I had the opportunity to participate and collaborate on multiple research initiatives in the field of[Natural Language Generation](https://en.wikipedia.org/wiki/Natural_language_generation) (NLG) in addition to my responsibilities as a Data Scientist at trivago. NLG is the process of automatically generating text from either text and/or non-linguistic data inputs. Some NLG applications include chatbots, image captioning, and report generation. These are application areas of high interest internally within trivago as we seek to leverage our rich data environment to enrich the user experience with potential NLG applications.


In the past we have contributed to the area of NLG through the creation of an NLG system called “Hotel Scribe” to automatically generate descriptions of hotel accommodations from structured data that we have internally within trivago. This system showed significant promise in both internal evaluations and when compared to accommodation descriptions from an external source. The results of our system were published in our[2019 paper](https://aclanthology.org/W19-8647/) , which we presented at International Natural Language Generation Conference (INLG) 2019 in Tokyo Japan. We have also collaborated with other researchers by extensively looking at past human evaluations and finding significant issues in how they are conducted within the NLG research communities. These findings were published in our[INLG 2020](https://aclanthology.org/2020.inlg-1.23/) research paper.


# Evaluation Research


The main theme of research throughout 2021 was exploring diverse issues with respect to evaluation. This is a topic that has been increasingly receiving more attention within the[Natural Language Processing](https://en.wikipedia.org/wiki/Natural_language_processing) (NLP) and Natural Language Generation (NLG) research communities. Especially as more and more shortcomings with current evaluation practices are identified. Issues such as automated metrics correlating poorly with human judgements, human evaluations having extreme diversities in approach, and finally the question of reproducibility in NLP/NLG evaluations.


We looked at human evaluations with our[HumEval 2021 paper](https://aclanthology.org/2021.humeval-1.1/) . In particular, we explored the nature of human evaluations being used in the creation of NLG based systems that incorporate some form of “Common sense” knowledge by analysing past published papers. Our work found that there was large variance in how such systems are evaluated, making it impossible to compare different systems and define baselines. We proposed as part of our HumEval paper a new Commonsense Evaluation Card (CEC) a set of recommendations for reporting evaluations for such systems.


There are also significant issues with automatic evaluations as well. Automatic evaluations such as[BLEU](https://en.wikipedia.org/wiki/BLEU) or[ROUGE](https://en.wikipedia.org/wiki/ROUGE_(metric)) compute the coverage between a given source and target text i.e. the number of tokens that appear in the target text. These metrics are especially used to evaluate large neural language models (e.g. models like[GPT–3](https://en.wikipedia.org/wiki/GPT-3) etc.). In addition to the question of the appropriateness of using such automated metrics, there is also the question of whether these models are being evaluated on diverse enough datasets to give a sufficient representation of their performance. This is a topic we looked at in our[NeurIPS 2021 paper](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/hash/ec8956637a99787bd197eacd77acce5e-Abstract-round1.html) , which looked at the possibility of going beyond summarising the performance of a given model with a single number for a particular criteria e.g. accuracy. Instead we suggested a new approach to allow for more in-depth analyses where researchers can evaluate against multiple challenge test sets generated automatically. In our experiments, we found significant variances between reported scores and those found by the challenge sets. For example we observed differences in how the models perform with regards to gender, language, and ethnicity sub-population datasets, which suggests that the models perform poorly when they encounter new concepts and words. These initial findings suggest there is a need in general to test on a broader set of data to best understand how well a given model will actually perform.


Our work for the NeurIPS paper was achieved through the creation of a new framework called[NL-Augmenter](https://gem-benchmark.com/nl_augmenter) to generate controlled perturbations from a given test dataset. We have open sourced the NL-Augmenter project[on GitHub](https://github.com/GEM-benchmark/NL-Augmenter) and already this has been a substantial collaborative effort to create multiple types of filters and transformations to create a whole variety of perturbations beyond what we had initially created in our initial paper.


Reproducibility was also an issue that was explored for the[ReproGen 2021](https://reprogen.github.io/2021/) shared task at INLG 2021. In[our paper](https://aclanthology.org/2021.inlg-1.29/) paper we looked at reproducing a past experiment that had previously been conducted with colleagues at trivago. This experiment reproduced an[early experiment from 2007](https://aclanthology.org/W07-2325/) that asked participants to compare generated texts with hedged phrases (e.g. “unfortunately”, “sadly”, etc.) against those that contain none. Colleagues were recruited to read these texts in two settings and were asked to fill their response for each setting through a questionnaire. The results from the reproducibility experiment showed that whilst it was straightforward to replicate the procedural aspects from the experiment, we were only able to partially replicate the results from our original results. This work showed two things: Firstly, the results obtained in earlier human evaluation may not be generalisable beyond the cohort of participants that initially participated. Secondly, it shows the imperative need for reproduction of results generally within research.


The need for reproducibility is particularly important as the increasing use of outputs from academic work are utilised in industry settings. For example, within trivago we make applied use of Machine Learning components internally in areas such as imaging tagging, item matching, etc. based on the work from academic research. Understanding how to competently evaluate the performance of these models and reproduce results will enable us to have greater confidence in such models when deployed into production and when making changes to add new features or capabilities.


Finally, the last area that we explored was the issue of underreporting of errors in NLG systems. In our[INLG 2021 paper](https://aclanthology.org/2021.inlg-1.14/) we observe that there is a significant issue with authors in general underreporting errors in general. For example, more than half of the papers we looked at from previous conferences failed to mention any errors. Whilst those that did mention errors were mostly at a superficial level. This means there is a systemic underreporting of the weakness of existing approaches that can give readers an incorrect perception of the robustness of existing approaches. We concluded in the paper that there needs to be a more concerted effort within the research community to improve standards when reporting the outcomes of NLG based experiments and that more detailed error reporting would help better inform the research community at large about the strengths and weaknesses of a given approach.


# Research Project Partnerships


In addition to the above research work on evaluation, there were also two research projects that trivago gave in support as a project partner that were funded by the United Kingdom’s[Engineering and Physical Sciences Research Council](https://epsrc.ukri.org/) (EPSRC):


- [Natural Language Generation for Low-resource Domains](https://gow.epsrc.ukri.org/NGBOViewGrant.aspx?GrantRef=EP/T024917/1) led by[Dr. Dimitra Gkatzia](https://www.napier.ac.uk/people/dimitra-gkatzia) of[Edinburgh Napier University](https://www.napier.ac.uk/) .
- [ReproHum: Investigating Reproducibility of Human Evaluations in Natural Language Processing](https://gow.epsrc.ukri.org/NGBOViewGrant.aspx?GrantRef=EP/V05645X/1) led by[Prof. Anya Belz](https://www.adaptcentre.ie/experts/anya-belz/) of the[ADAPT](https://www.adaptcentre.ie/about/) research centre.


As a project partner trivago will provide support in the form of advice, data, and participation in these funded research initiatives.


# Conclusion


The main takeaway over the past year is that there are significant issues with regards to evaluation practices in general irrespective of whether they are human or automatic. Improving standards in how NLP and NLG systems are evaluated and the reproducibility of such evaluations is imperative not just for academia, but also for those of us working in industry. More robust evaluations will allow us to better understand the advantages and disadvantages of a given approach and have more confidence in the results being presented. However, recent focus on this question gives me confidence that the research community will make significant progress in answering the questions being raised in the coming future.


# Paper References


- [Hotel Scribe: Generating High Variation Hotel Descriptions](https://aclanthology.org/W19-8647/) at[INLG 2019](https://www.inlg2019.com/)
- [Twenty Years of Confusion in Human Evaluation: NLG Needs Evaluation Sheets and Standardised Definitions](https://aclanthology.org/2020.inlg-1.23/) at[INLG 2020](https://www.inlg2020.org/)
- [It’s Commonsense, isn’t it? Demystifying Human Evaluations in Commonsense-Enhanced NLG Systems](https://aclanthology.org/2021.humeval-1.1/) at[HumEval 2021](https://humeval.github.io/) .
- [The GEM Benchmark: Natural Language Generation, its Evaluation and Metrics](https://aclanthology.org/2021.gem-1.10/) at[GEM Workshop 2021](https://gem-benchmark.com/workshop) .
- [Reproducing a Comparison of Hedged and Non-hedged NLG Texts](https://aclanthology.org/2021.inlg-1.29/) at[INLG 2021](https://inlg2021.github.io/) .
- [Underreporting of errors in NLG output, and what to do about it](https://aclanthology.org/2021.inlg-1.14/) at[INLG 2021](https://inlg2021.github.io/) . *Received a commendation for Outstanding Position Paper* .
- [Automatic Construction of Evaluation Suites for Natural Language Generation Datasets](https://datasets-benchmarks-proceedings.neurips.cc/paper/2021/hash/ec8956637a99787bd197eacd77acce5e-Abstract-round1.html) at[NeurIPS 2021](https://neurips.cc/Conferences/2021) .
