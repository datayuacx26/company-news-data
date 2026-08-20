---
schema_version: "1.0.0"
document_id: "a5725648eb312eee7006d56406bdf10c35ec128c77111d3bf7c6dab4b114d4e6"
company_key: "yc-mecha-health"
company: "Mecha Health"
source_id: "yc-mecha-health-news-import-dc619b8bf4eb"
canonical_url: "https://www.mecha-health.ai/blog/Evaluating-automated-radiology-reports"
published_at: "2025-01-29T00:00:00+00:00"
first_seen_at: "2026-08-10T00:30:28.783990+00:00"
fetched_at: "2026-08-10T00:30:30.164114+00:00"
content_hash: "sha256:d6d07854c094d79240a5cbb9b514580fc5c6de8ef0c01db9bbd3908b91bf1105"
---

# Evaluating automated radiology reports

Automated radiology report generation can reduce radiologists' workload and enhance patient care, yet its adoption in clinical practice remains limited \[1\]. A key challenge is assessing the clinical quality of generated reports. This update explores existing evaluation challenges, reviews current automated metrics, and introduces novel approaches. We conclude with insights and future directions for improving evaluation methods. Before that, we discuss the significance of full radiology report generation.


## Motivating automatic report generation


Most regulated AI solutions today focus on classifying or quantifying specific pathologies \[1\]. However, clinical radiology extends beyond these narrow tasks. Effective reporting must:


- Convey findings with appropriate detail and nuance.
- Integrate findings into the broader clinical context.
- Reflect overall patient impressions.
- Support management decisions and future recommendations.


These complex tasks go beyond simple classifiers. As highlighted by Dr. Matthew Scott Davenport at RSNA 2024, radiology's true value lies not just in extracting information from images but also in ensuring high-quality communication to reduce uncertainty and improve outcomes.


Rendering diagram…


As can be seen from the figure above, framing full-text report generation as the endpoint of radiological AI better aligns with clinical practice. It enables more detailed and nuanced descriptions, incorporating elements such as size, severity, location, clinical context, ambiguity, temporality, and impact on patient management \[2, 3\].


## Current issues with evaluating full text reports


Whilst producing highly accurate radiology reports would add substantial value to the clinical workflow, evaluating free-text reports remains difficult. We describe the main issues below.


**Complexity of Free-text Outputs** The free-form nature of the output introduces many degrees of freedom, making error detection—especially subtle errors—more challenging.


**Contextual Variability** The ideal structure and content of a free-text report varies depending on context (e.g., emergency vs. elective settings). Other factors, such as preferred standardization approaches \[4\] and geographic differences \[5\], also influence what is considered desirable in a report \[2\].


**Limitations of Automated Metrics** Several automated metrics exist for evaluating generated reports, but they have significant limitations:


1. They may fail to verify the accuracy of all clinical details in the report.
2. They may not adequately capture what constitutes a desirable output if they do not compare reports to those produced by human experts.


**Variability Among Radiologists** Even when comparing a generated report to a "ground-truth" report by a certified radiologist, differences in style and quality among radiologists introduce variability \[2\].


**Lack of Insight into AI Preference** Even assuming high-quality human-generated reports, few studies explore *why* an AI-generated report might be preferred over a human-written one \[6\].


**Assumption of Full Autonomy** Most evaluations treat AI-generated reports as stand-alone artifacts \[2\], implicitly assuming full autonomy—i.e., the report is taken at face value and directly incorporated into clinical workflows. However, most AI systems are assistive rather than autonomous, meaning evaluation should consider how clinicians use, edit, and interact with these outputs.


**Potential for Hallucinated Details** A more insidious issue, described by[Dr. Woojin Kim](https://www.linkedin.com/in/woojinkim/) , involves models generating seemingly correct statements based on overfitting or data contamination.


For example, a vision-language model (VLM) trained on a dataset with temporal data might learn statistical patterns (e.g., cardiomegaly often remains stable over short time spans). Additionally, stylistic biases may emerge—radiologists at a particular hospital may frequently write, "cardiomegaly is unchanged."


At inference time, if the VLM is given a cross-sectional image and outputs, *"cardiomegaly is unchanged,"* it may be statistically correct (and indeed might be a clinically correct statement). However, since the model lacks prior studies to confirm stability, this constitutes an inappropriate hallucination of detail.


## Evaluation approaches


### General metrics


#### Natural language generation metrics


Many natural language generation metrics focus on measuring congruence or similarity between phrases.


For example, Bilingual Evaluation Understudy with 2-gram precision (BLEU-2) \[7\] evaluates how well a generated text aligns with the ground truth by calculating the precision of consecutive two-word pairs. Consider the following example:


Rendering diagram…


In this example, the phrase **air bronchograms** appears in both reports, resulting in an exact match. BLEU-2 reliably detects such repeats of *two consecutive words* , which works well when identical terms are always used. However, clinical radiology rarely follows this pattern. For example, the following synonyms would not be recognized as correct:


- "left lower" ≠ "left base"
- "consolidation" ≠ "opacity"
- "effusion" ≠ "fluid collection"


Additionally, while some variants account for negations, not all do. This can lead to misleading scores—for instance, if the ground truth states *"NO air bronchograms present"* and the generated report says *"air bronchograms ARE present"* , BLEU-2 would still register a match for *"air bronchograms"* .


Despite their popularity in Machine Learning (ML) research, these metrics are unsuitable for evaluating generated radiology reports unless all radiologists within a hospital or network adhere to an identical ontology.


#### Embedding-based metrics


These metrics attempt to match words with similar meaning using language embeddings. The general process is illustrated below:


Rendering diagram…


BERTScore \[8\] is an example metric which uses embeddings to evaluate report quality. Because embeddings are used to make comparisons rather than the words themselves, such a score *can* account for semantic similarity, i.e.:


- "lower lobe" ≈\\approx


≈


"base"
- "effusion" ≈\\approx


≈


"fluid collection"
- "consolidation" ≈\\approx


≈


"opacity"


However, semantic similarity alone does **not** account for meaning. For instance, the words "with" and "without" look similar and will have similar embeddings. The word "discharge" will have the same embedding irrespective of whether it's referring to *patient* discharge or *fluid* discharge. Therefore, embedding-based metrics are not guaranteed to capture negations (or indeed the clinical meaning) of statements correctly.


### Radiology-specific metrics


#### CheXbert


CheXbert \[9\] is a clinical accuracy metric which aims to evaluate medical content rather than just text. The idea is to take the AI-generated report and extract pathological information using a language model (a fine-tuned version of the BERT model - "CheXbert"). Extractions attempt to preserve negation correctly by including the presence/absence of each pathology and also try to map synonyms to standard terms. The process by which CheXbert was trained is shown below:


Rendering diagram…


However, CheXbert has limited scope. The metric operates on a set of 14 pre-defined findings. It does not consider much of the detail that is relevant in clinical radiology, for instance anatomical context mapping, location, severity, size, etc. Also, much of the extraction quality is based on a previously trained automatic labeler (the 'CheXpert' model), which is imperfect. Like with other language models, the final extractions themselves can also be incorrect.


#### RadGraph-F1


RadGraph-F1 \[10\] attempts to capture relationships between medical findings and represents them as a graph. An example is illustrated below, where "ANAT" stands for "anatomy", and "OBS-DP" is "observation - definitely present":


Rendering diagram…


This score attempts to preserve anatomical context as well as link to locations. It also extends the scope of findings. However, the score is harder to generalize to other modalities and has no normalization over entities - meaning it can treat different ways of expressing the same medical concept as distinct entities (e.g., "fluid collection" and "effusion" would be considered different entities, even though they refer to the same clinical finding). There also exist cases that do not fit the schema as defined by the authors due to ambiguities in the findings sections.


#### HeadCT-One


Some metrics such as HeadCT-One \[11\] try to map all terms to a knowledge ontology. For instance, the terms "small vessel disease changes" and "microvascular ischaemic changes" would both map to a fixed descriptor of "small vessel disease". A comparison is then made between the mapped reports. An illustrative mapping (from left to right, where orange and blue are terms from a ground-truth and generated report, respectively) is shown below:


Rendering diagram…


#### RadCliQ


RadCliQ \[11\] is a weighted combination of BLEU, BERTScore, CheXbert vector similarity, and RadGraph F1. RadCliQ has a higher alignment with reports written by radiologists than the individual metrics which constitute it:


Rendering diagram…


#### FineRadScore


All the previously discussed metrics fail to describe which *parts* of a report are problematic, or how clinically significant the errors are. FineRadScore \[12\] attempts to evaluate which parts of the generated report contains errors and scores how severe they are.


Rendering diagram…


FineRadScore was shown to be more strongly aligned on average with radiologists than other metrics:


Metric Kendall tau b correlation (95% CI)


BLEU 0.414 (95% CI, 0.156 - 0.635)


BERTScore 0.505 (95% CI, 0.273 - 0.671)


CheXbert 0.537 (95% CI, 0.330 - 0.717)


RadGraph 0.528 (95% CI, 0.357 - 0.687)


RadCliQ 0.615 (95% CI, 0.450 - 0.749)


**FineRadScore (GPT-4)** **0.701 (95% CI, 0.523 - 0.841)**


**FineRadScore (Claude-3 Opus)** **0.737 (95% CI, 0.593 - 0.850)**


It should be noted that FineRadScore performs a line-by-line evaluation, which provides more detail than other similar LLM-as-a-judge approaches such as G-Rad (which ultimately provides a single score), or the GREEN metric, which returns the most 'representative' error explanations within a fixed set of six possible categories \[12\]. FineRadScore performs similarly with regard to radiologist alignment as these scores.


One of the main limitations of FineRadScore is that LLMs can be distracted by stylistic differences between a ground-truth and a generated report. Ideally, the score should focus solely on identifying clinically relevant errors and ignore differences in phrasing.


#### RadFact


RadFact \[13\] relies on the logical inference capabilities of LLMs to evaluate reports. A subtle difference with FineRadScore is that instead of a per-sentence evaluation per se, RadFact can be framed as a per-claim evaluation.


RadFact evaluates reports through logical inference by LLMs:


-


**Logical Precision** : Measures truthfulness of generated reports.


- Calculates fraction of clinical statements entailed by ground-truth.
- Penalizes hallucinated content.


-


**Logical Recall** : Measures completeness of generated reports.


- Calculates fraction of ground-truth statements entailed by generation.
- Penalizes omitted content.


The following is a simple example:


**Generated Report:** "Nodule observed in the left lung. There are bilateral pleural effusions. The cardiac size is normal. No pneumothorax. Small patchy density in the right apical region."


**Ground Truth Report:** "Questionable nodule in left upper lobe. Moderate right pleural effusion. Left pleural effusion is now small. There is no pneumothorax. Cardiomegaly again noted."


Hypotheses Premises Evidence Status


**A** : Nodule observed in the left lung. 1. Questionable nodule in left upper lobe. ✔ ✅ Entailed


**B** : There are bilateral pleural effusions. 4. Moderate right pleural effusion. 5. Left pleural effusion is now small. ✔ ✅ Entailed


**C** : The cardiac size is normal. 6. Cardiomegaly again noted. ❌ ❌ Not entailed


**D** : No pneumothorax. 3. There is no pneumothorax. ✔ ✅ Entailed


**E** : Small patchy density in the right apical region. *(No supporting premise)* ❌ ❌ Not entailed


Therefore, logical precision would be calculated as:


Precision=Entailed generated statementsTotal generated statements=∣{A,B,D}∣∣{A,B,C,D,E}∣=35=0.6\\begin{aligned} \\text{Precision} &= \\frac{\\text{Entailed generated statements}}{\\text{Total generated statements}} \\\\ &= \\frac{|\\{A,B,D\\}|}{|\\{A,B,C,D,E\\}|} = \\frac{3}{5} = 0.6 \\end{aligned}


Precision


​


=


Total generated statements


Entailed generated statements


​


=


∣


{


A


,


B


,


C


,


D


,


E


}


∣


∣


{


A


,


B


,


D


}


∣


​


=


5


3


​


=


0.6


​


(1)


Where 3 represents statements A, B and D which were entailed by the ground truth, and 5 represents all generated statements (A through E).


It is plausible that by splitting the report into individual statements as opposed to passing the entire text into an LLM (as with FineRadScore), we are more likely to account for stylistic differences between the two, though it is likely that this issue still exists to some extent.


RadFact assumes the ground-truth report is exhaustive and perfect. If a generated report correctly identifies findings that were omitted from the ground-truth, the precision score will incorrectly decrease - potentially underestimating the model's true clinical accuracy. For instance, if the generated report includes the statement 'cardiac contour within normal limits', and the ground truth report doesn't mention the cardiac region at all (because it is, in fact, normal), then the generated statement may nevertheless be parsed as a non-entailed statement, which would be inappropriate.


## Future evaluation of generated reports


Having described the current assessment metrics, a few issues become clear:


- Equivalent but different medical jargon can mislead or otherwise distract LLMs
- Hallucinated detail can appear correct but still lack support
- There is a high variability in how radiologists write reports
- Ground truth reports might omit or otherwise miss correct findings


We therefore propose a high-level evaluation metric, which we are currently designing and would be interested in discussing further. We call this the 'Clinically Aligned Radiology Evaluation (CARE)' score.


### Clinically Aligned Radiology Evaluation (CARE) Score


The CARE score is a multi-stage metric that integrates text normalization, bi-directional entailment, severity scoring, and complementary references to handle the possibility of an incomplete ground truth.


CARE functions as follows:


1. **Text normalization** : An LLM is used to normalize terms between a generated and ground-truth report. This establishes a common ontology. An example is shown[here (O1-based ontology mapping)](https://chatgpt.com/share/679acc8c-9b48-8005-93d1-e0cba6041601) .
2. **Decomposition of reports** : The reports are then broken down into sets of clinical statements. An example is shown[here](https://chatgpt.com/share/679acde7-d614-8005-b46d-01841c776e88) .
3. **Bi-directional entailment** : Bidirectional entailments are performed between the clinical statements as per RadFact. We show an example of recall[here](https://chatgpt.com/share/679acfb1-e29c-8005-ab1b-ee4bf9b95f23) , where a pleural effusion is missed in the generated report.
4. **Severity measurement** : An LLM is used to judge the clinical significance of errors in the generated report. Here, auxiliary information about the patient is passed in-context (where available), to help judge whether there is a potentially beneficial additional detail in the generated report when compared to the ground truth report. An example which estimates error severity and returns severity-weighted scoring can be found[here](https://chatgpt.com/share/679ad0d7-6fd4-8005-9592-eadb89a0d33e) .
5. **Final outputs** : Precision, recall, severity weighted variants and the adjustments themselves can all be returned as a structured output for downstream analysis. This can also be seen[here](https://chatgpt.com/share/679ad0d7-6fd4-8005-9592-eadb89a0d33e) .


It should be noted the examples shown above are illustrative and the approach may require adjustment.


The CARE score aims to incorporate both correctness and clinical relevance while mitigating common pitfalls of prior evaluation methods. By normalizing terminology, decomposing reports into discrete clinical statements, and applying bidirectional entailment, CARE ensures a robust comparison. Severity weighting helps distinguish minor stylistic differences from critical errors, addressing the limitations of strict lexical or embedding-based metrics.


## Conclusion


In summary, evaluating automatically generated radiology reports is challenging. We are excited by the development of novel metrics which address current limitations and can act as appropriate proxies for human analysis. Future work should focus on refining such methods to ensure evaluations align with real-world clinical utility and patient outcomes.


If you would like to cite this work, please use the following BibTeX entry:


```text
@misc{mecha2025evaluating,
author = {Ahmed Abdulaal},
title = {Evaluating automated radiology reports},
year = {2025},
month = {January 29},
url = {https://mecha-health.ai/blog/Evaluating-automated-radiology-reports},
note = {On assessing the quality and accuracy of automatically generated radiology reports}
}


```


## References


1. Milam, ME and Koo, CW. The current status and future of FDA-approved artificial intelligence tools in chest radiology in the United States. Clinical Radiology, 78(2):115-122, 2023.
2. Tanno, Ryutaro, et al. "Collaboration between clinicians and vision–language models in radiology report generation." Nature Medicine (2024): 1-10.
3. Bannur, S. et al. Learning to exploit temporal structure for biomedical vision–language processing. In Proc. IEEE/CVF Conference on Computer Vision and Pattern Recognition 15016–15027 (2023).
4. Kahn Jr, Charles E., et al. "Toward best practices in radiology reporting." Radiology 252.3 (2009): 852-856.
5. Hartung, Michael P., et al. "How to create a great radiology report." Radiographics 40.6 (2020): 1658-1670.
6. Tu, Tao, et al. "Towards generalist biomedical AI." NEJM AI 1.3 (2024): AIoa2300138.
7. Papineni, Kishore, et al. "Bleu: a method for automatic evaluation of machine translation." Proceedings of the 40th annual meeting of the Association for Computational Linguistics. 2002.
8. Zhang, Tianyi, et al. "Bertscore: Evaluating text generation with bert." arXiv preprint arXiv:1904.09675 (2019).
9. Smit, Akshay, et al. "CheXbert: combining automatic labelers and expert annotations for accurate radiology report labeling using BERT." arXiv preprint arXiv:2004.09167 (2020).
10. Yu, Feiyang, et al. "Evaluating progress in automatic chest x-ray radiology report generation." Patterns 4.9 (2023).
11. Acosta, Julián N., et al. "HeadCT-ONE: Enabling Granular and Controllable Automated Evaluation of Head CT Radiology Report Generation." arXiv preprint arXiv:2409.13038 (2024).
12. Huang, Alyssa, et al. "FineRadScore: A Radiology Report Line-by-Line Evaluation Technique Generating Corrections with Severity Scores." arXiv preprint arXiv:2405.20613 (2024).
13. Bannur, Shruthi, et al. "Maira-2: Grounded radiology report generation." arXiv preprint arXiv:2406.04449 (2024).
