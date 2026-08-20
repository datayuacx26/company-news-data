---
schema_version: "1.0.0"
document_id: "3a2b9f8d61fd3305b86f28dcf77b909d683b29be5b6a57bfe274e1bbf9f2632b"
company_key: "yc-mecha-health"
company: "Mecha Health"
source_id: "yc-mecha-health-news-import-dc619b8bf4eb"
canonical_url: "https://www.mecha-health.ai/blog/Mecha-net-v0.2"
published_at: "2025-06-24T00:00:00+00:00"
first_seen_at: "2026-08-10T00:30:28.783990+00:00"
fetched_at: "2026-08-10T00:30:30.164114+00:00"
content_hash: "sha256:7c3e05cdd7541a622c69342fbaa5f1ce2a97d966a3db114b4fd3d4930f8bf50c"
---

# Mecha Net v0.2 — Reaching Human Performance

## Algorithmic innovation


We've trained a system which pushes the boundaries of what is possible in automated radiology report generation. Mecha Net v0.2 is a generalist system that aims to produce full reports by direct analysis of a scan.


*For this update, we restricted ourselves to training both parameter and data efficient models* . We fixed the parameter count of our new model to match Mecha Net v0.1 and also fixed the training data to the same set of images as before. We used fewer than 500K CXRs for model training, yet still outperformed systems trained on much larger datasets (2M+ images).


We rely solely on several algorithmic innovations to push performance further. These innovations have allowed us to achieve state-of-the-art performance not previously possible.


It should be noted that further data (and parameter) scaling of our systems is expected to yield even better results, and we are currently working with our partners to create tailored and ultra-performant models for specific use cases.


## Evaluation methodology


In this section we discuss the evaluation methodology used in this analysis. We focus on the metrics used to evaluate the performance of Mecha Net v0.2, and how these metrics relate to the task of automated radiology report generation.


### Clinical Findings as Primary Evaluation Criteria


We quote the CheXbert metric in this analysis \[1\]. Any reporting system for chest X-rays is expected to report accurately on clinically important findings such as fracture, consolidation, pneumothorax, lung lesions, enlarged cardiomediastinal silhouette, and more. Being able to do so is a key requirement for any automated radiology report generation system because all additional details in a report (such as laterality, severity, temporality, and so on) are secondary to the accurate reporting of such findings.


### Limitations of Word-Level Metrics


Calculating the precision of two (or more) consecutive words in a report is a common way to evaluate automated report generation, however we believe this to be inappropriate for this task. In particular, clinical radiology rarely follows a strict grammar, and the same finding can be reported in many different ways. For example, a report may say "There is a fracture of the left humerus" or "Left humeral fracture". Both of these reports are correct, but they would not match if we were to use a 4-gram word-level precision metric.


Linear combinations of individual metrics can better 'align' with human radiologists. However, notwithstanding the[usual limitations](https://www.mecha-health.ai/blog/Evaluating-automated-radiology-reports) of such metrics, as well as the fact that straightforward human preference is often[not a sensible gold standard](https://arxiv.org/abs/2309.16349) \[2\], we argue that improved alignment is due to additional detail in the report, but such details are predicated on the accurate reporting of the clinically important findings in the first instance.


### CheXbert Metric and Dataset


CheXbert works by reading the written text of a report and then extracting labels for 14 clinically important findings (the labels for each finding are 'present', 'absent', or 'unsure'). Uncertain findings are taken as negative, as per prior work. These labels can then be used to compute an F1 score. The CheXbert labeller recovers 99.13% of the performance of a board certified radiologist on reporting these 14 findings \[1\]. We discuss CheXbert in greater detail in our[evaluations post](https://www.mecha-health.ai/blog/Evaluating-automated-radiology-reports) . We use the MIMIC-CXR test set for this evaluation, which is a large publicly available dataset of chest X-rays and their associated reports \[3\].


## Performance Evaluation


We compare Mecha Net v0.2 to the current state-of-the-art (SOTA) models for the task of report generation for CXRs.


### CheXbert - Micro F1 scores


First, we quote the micro-averaged F1 scores for all 14 labels as well as the F1-5 (which considers the top 5 labels in a report):


Performance comparison


### CheXbert Micro F1 scores


Higher is better


As can be seen, Mecha Net v0.2 outperforms all prior models, including our earlier system, Mecha Net v0.1.


### CheXbert - Macro F1 scores


Next, we report the macro-averaged F1 scores in the same way:


Performance comparison


### CheXbert Macro F1 scores


Higher is better


Mecha Net v0.2 outperforms all prior models on the F1-14 metric, but slightly underperforms on the F1-5 metric relative to Mecha Net v0.1. This is likely due to the fact that the current version of Mecha Net (v0.2) does not yet have access to auxiliary data.


### MedGemma analysis


In order to calculate F1 metrics for MedGemma using the CheXbert labeller, we ran inference with both pretrained (pt) and instruction-tuned (it) versions of MedGemma \[4\] (as in the plots above).


We began by running inference on MedGemma-it, limiting generation to 200 tokens per report as recommended in the CXR report generation example on the[MedGemma model card](https://huggingface.co/google/medgemma-4b-it) . However, we found that 200 tokens were occasionally insufficient to generate complete reports. To address this, we increased the token limit to 500 tokens per report, which proved sufficient for generating full reports across all test set images and resulted in a modest improvement in F1 scores.


To this end, we also ran inference with MedGemma-pt, allowing up to 500 tokens to be generated for each report. We quote results for both MedGemma-it and MedGemma-pt in the plots above. We find that the pretrained variant outperforms the instruction-tuned variant on both the micro and macro F1 scores, which is consistent with findings by the MedGemma team \[4\].


While this performance difference has been attributed to variations in reporting style, we observed multiple instances where MedGemma-pt generates more accurate reports than the instruction-tuned variant. A qualitative example demonstrating this difference is shown below:


### Report Comparison


Qualitative analysis across model variants


#### Ground Truth


Reference


Left hilar mass, increased vascular markings in remaining lung fields, new left-sided pleural effusion, mild-to-moderate cardiomegaly, sternotomy wires intact, multiple surgical clips in left hemithorax, no pneumothorax.


#### MedGemma-pt


Pre-trained


There is a large left perihilar mass measuring 5.5 x 4.5 cm. There is a small left pleural effusion. There is no pneumothorax. There is no right pleural effusion. There is no right lung consolidation. There is no pulmonary edema. The cardiomediastinal silhouette is enlarged. Impression: large left perihilar mass.


#### MedGemma-it


Instruction-tuned


Okay, here's a description of the chest X-ray based on the image provided:


**Overall Impression:**


The image shows a PA (Posterior-Anterior) chest X-ray. The patient is likely an adult, based on the skeletal structures visible. There are some abnormalities present, which warrant further investigation.


**Key Findings:**
- **Cardiomegaly:** The heart appears enlarged (cardiomegaly). The cardiac silhouette occupies a significant portion of the chest.
- **Pulmonary Vascular Congestion:** There is increased prominence of the pulmonary vasculature, suggesting possible pulmonary edema or congestion. This can be seen as increased interstitial markings.
- **Possible Pleural Effusion:** There is a blunting of the left costophrenic angle, which could be suggestive of a small pleural effusion.
- **Mediastinal Structures:** The mediastinum appears relatively normal in width, with the trachea midline.
- **Bones:** The ribs and clavicles appear intact.


**Additional Considerations:**
- **Technique:** The image quality is adequate for initial assessment.
- **Comparison:** It would be helpful to compare this image to previous chest X-rays of the same patient, if available, to assess for any changes over time.


**Recommendations:**


Based on these findings, I would recommend further evaluation, such as:


- **Clinical Correlation:** Review the patient's clinical history and symptoms to determine the significance of these findings.
- **Further Imaging:** Consider additional imaging, such as a CT scan of the chest, to better characterize the cardiac silhouette, pulmonary vasculature, and any potential pleural effusion.
- **Echocardiogram:** An echocardiogram may be helpful to assess cardiac function and rule out other causes of cardiomegaly.


**Disclaimer:**


This is a preliminary interpretation based on a single image. A definitive diagnosis requires a comprehensive evaluation by a qualified radiologist, taking into account the patient's clinical history, physical examination findings, and other relevant information.


##### Key Finding


Notice that **MedGemma-pt** captures the left perihilar mass (a critical finding), despite hallucinating incorrect measurements for it. By comparison, **MedGemma-it** does not identify the mass at all.


## Human assessment


Recent research has demonstrated that Google's MedGemma \[4\] outperformed radiology residents in answering LLM-generated multiple choice questions related to CXR interpretation. These questions were derived from the original text reports for CXR images \[5\].


To assess whether these models can match radiologist performance on the broader task of report generation, we compared the reports generated by Mecha Net v0.2 and MedGemma-pt to human-generated reports.


### Setup


We randomly sub-sampled a set of 1000 images from the MIMIC-CXR test set and had them reported by board-certified radiologists. All radiologists in this analysis were of attending (US) or consultant (UK) level. The models used in this analysis did not have access to any auxiliary data, such as patient history, prior studies, or lateral scans. Therefore, to allow for comparison, the radiologists were given the following instruction:


> This is a CXR for a research project. There is no clinical information or prior images. Please report everything you see.


Radiologist Instructions


The radiologist-generated reports were then compared to those generated by Mecha Net v0.2 and MedGemma-pt. For this analysis we quote BLEU-2, BLEU-4, METEOR, ROUGE-L, the BERTScore, RGER, CheXbert Similarity, and the CheXbert F1 metrics, as it is possible that humans outperform the models on certain metrics, such as those that measure the use of specific words or phrases in the report, but not others.


### Results


Text Similarity


Semantic Similarity


Radiology-Specific


Clinical Findings


The results demonstrate that Mecha Net v0.2 outperforms both human radiologists and MedGemma-pt across all evaluation metrics. Notably, while MedGemma-pt shows improvements over human performance on most metrics, Mecha Net v0.2 consistently achieves the highest scores across all categories - from text similarity metrics (BLEU, METEOR, Rouge-L) to semantic similarity (BERTScore), radiology-specific measures (RGER), and clinical findings assessment (CheXbert metrics).


## Limitations


There are a number of limitations to consider in this analysis.


- **Automated evaluation metrics are not perfect** , and all have at least some limitations. For example, the CheXbert metric is not able to capture all clinically important findings, and so it is possible that a model could achieve a high CheXbert score but still miss clinically important findings.
- **Defining what constitutes the 'ground truth' for a report is difficult** . Both the radiologists who first read the MIMIC CXR images, and then those who double-read them, will have made errors in their reports. Deciding which information is 'correct' in a systematic way is difficult. In this work we have assumed the original reports are the ground truth, but this is not necessarily reflective of the true ground truth.
- **Human evaluation was conducted on a subset of the test set (1,000 images)** rather than the full dataset. However, our variability analysis in theAppendix demonstrates that 1,000 datapoints provides a stable and reliable estimate for CheXbert-based metrics, with substantially reduced variability compared to smaller evaluation sets.


## Conclusion


To the best of our knowledge, this is the first work to evidence that radiology foundation models can systematically match or exceed human performance for the task of report generation by analysing medical images.


We have shown that when trained or fine-tuned on large datasets within distributions similar to those seen in practice, Mecha Net v0.2 not only matches but exceeds the performance of both human radiologists and competing AI models on the task of CXR report generation.


If you would like to cite this work, please use the following BibTeX entry:


```text
@misc{mecha2025mechanetv02,
author = {Ahmed Abdulaal and Hugo Fry and Ayodeji Ijishakin and Nina Montaña Brown},
title = {Mecha Net v0.2 - Approaching Human Performance},
year = {2025},
month = {June 24},
url = {https://mecha-health.ai/blog/Mecha-net-v0.2},
note = {Performance update for our Chest X-ray report generation model}
}


```


## References


1. Smit, Akshay, et al. "CheXbert: combining automatic labelers and expert annotations for accurate radiology report labeling using BERT." arXiv preprint arXiv:2004.09167 (2020).
2. Hosking, Tom, Phil Blunsom, and Max Bartolo. "Human feedback is not gold standard." arXiv preprint arXiv:2309.16349 (2023).
3. Johnson, Alistair EW, et al. "MIMIC-CXR, a de-identified publicly available database of chest radiographs with free-text reports." Scientific data 6.1 (2019): 317.
4. Google. (2025, May 20). MedGemma (google/medgemma‑4b‑it) \[Model\]. Hugging Face. Retrieved June 23, 2025, from[https://huggingface.co/google/medgemma-4b-it](https://huggingface.co/google/medgemma-4b-it)
5. Pal, Ankit, et al. "ReXVQA: A Large-scale Visual Question Answering Benchmark for Generalist Chest X-ray Understanding." arXiv preprint arXiv:2506.04353 (2025).


---


## Appendix


### CheXbert Variability Analysis


To assess the reliability and stability of automated radiology report evaluation metrics, we conducted a systematic variability analysis using the CheXbert model, a BERT-based clinical NLP model specifically trained for chest X-ray report labeling.


**Experimental Design**
We evaluated metric stability across varying dataset sizes using a paired comparison between ground truth and reports generated by an earlier checkpoint of Mecha Net 0.2. Dataset sizes ranged from 200 to 3,403 reports (full dataset), with intermediate sizes of 400, 600, 800, 1,000, and 2,000 reports.


**Sampling Protocol**
For each dataset size (except the full dataset), we performed 50 independent random sampling runs without replacement to capture metric variability. Each run involved:


- Random selection of *n* report pairs from the full dataset
- Preprocessing and tokenization using BERT tokenizer
- Batch processing with mixed precision (16 reports per batch)
- Calculation of micro-F1 and macro-F1 scores


**Statistical Analysis**
We computed mean, standard deviation, and individual run distributions for each dataset size to quantify metric stability and assess convergence behavior.


### Variability Analysis Results


#### Micro F1 Score Variability


Loading variability analysis…


#### Macro F1 Score Variability


Loading variability analysis…


These plots show individual run results (orange dots) along with the mean performance and standard deviation (black line with error bars) across different dataset sizes. Both micro-F1 and macro-F1 scores demonstrated decreasing variability with increasing dataset size, following expected statistical convergence patterns.


**Micro-F1 Performance** :
Micro-F1 scores ranged from **0.636 ± 0.018** (mean ± SD) at 200 reports to **0.635** at the full dataset size (3,403 reports). The coefficient of variation decreased from **2.83 %** at 200 reports to **0.51 %** at 2,000 reports, indicating improved metric reliability with larger evaluation sets.


**Macro-F1 Performance** :
Macro-F1 scores showed similar convergence patterns but with higher overall variability, ranging from **0.431 ± 0.024** at 200 reports to **0.432** at full scale. The higher variability in macro-F1 likely reflects its sensitivity to performance on rare clinical conditions. The coefficient of variation dropped from **5.54 %** (200 reports) to **1.18 %** (2,000 reports).


**Convergence Characteristics** :
*Both metrics demonstrated substantial variability reduction beyond 1,000 reports* , with diminishing returns in stability improvements for datasets larger than 2,000 reports. The standard deviation for micro-F1 decreased by approximately 64 % (from 0.018 to 0.006) when increasing the dataset size from 200 to 1,000 reports.


### Statistical Independence and Central Limit Theorem


The calculation of the standard deviation of the ChexBert scores assumes that the subset samples are statistically independent. Due to the limited test set of 3403 data points, this statistical independence is not valid for some of the larger subsets (eg 1000, 2000 and 3403). Indeed, at a sample size of 3403 there is no independence at all, which is why our standard deviation is 0%, despite the fact that two independent samples of 3403 clearly have stochastic differences.


However, the subsets of size 200 are statistically independent since the average overlap between two samples is around 11 \[with replacement we expect 200^2/3400 = 11.76, without replacement the overlap will be lower\]. We can therefore use the standard deviation for the subset of 200 to estimate the true statistically independent standard deviation at larger subset sizes by invoking the Central Limit Theorem (CLT). The CLT provides an asymptotic equation for the distribution of the sample mean for a sample of independent identically distributed (iid) random variables. It states that in the asymptotic limit:


sample mean∼N(μ,σ2n)\\text{sample mean} \\sim N(\\mu, \\frac{\\sigma^2}{n})


sample mean


∼


N


(


μ


,


n


σ


2


​


)


where μ\\mu


μ


is the true mean, σ2\\sigma^2


σ


2


is the variance of a single data point and nn


n


is the number of samples.


The mathematical relationship between standard deviations at different sample sizes can be derived from the CLT. Starting with the empirical standard deviation for n samples, we can establish the relationship to the true population standard deviation, and then derive the scaling formula:


σn=empirical standard deviation for n samples\\sigma_n = \\text{empirical standard deviation for n samples}


σ


n


​


=


empirical standard deviation for n samples


σ=σ0=n×σn\\sigma = \\sigma_0 = \\sqrt{n} \\times \\sigma_n


σ


=


σ


0


​


=


n


​


×


σ


n


​


σn=σ0n=200×σ200n=200n×σ200\\sigma_{n} = \\frac{\\sigma_0}{\\sqrt{n}} = \\frac{\\sqrt{200} \\times \\sigma_{200}}{\\sqrt{n}} = \\sqrt{\\frac{200}{n}} \\times \\sigma_{200}


σ


n


​


=


n


​


σ


0


​


​


=


n


​


200


​


×


σ


200


​


​


=


n


200


​


​


×


σ


200


​


We can therefore estimate the true standard deviation as follows:


Micro-F1 standard deviation=(200n)×1.8%\\text{Micro-F1 standard deviation} = \\left(\\sqrt{\\frac{200}{n}}\\right)\\times 1.8 \\%


Micro-F1 standard deviation


=


(


n


200


​


​


)


×


1.8%


Macro-F1 standard deviation=(200n)×2.4%\\text{Macro-F1 standard deviation} = \\left(\\sqrt{\\frac{200}{n}}\\right)\\times 2.4 \\%


Macro-F1 standard deviation


=


(


n


200


​


​


)


×


2.4%


Sample size 1000 2000 3403


Micro-F1 standard deviation 0.80% 0.57% 0.44%


Macro-F1 standard deviation 1.07% 0.76% 0.58%


### Discussion


The observed metric instability at smaller dataset sizes (n < 1,000) suggests that comparative studies using limited evaluation sets may yield unreliable conclusions about model performance differences.


**Implications for Evaluation Protocols** :
**Our findings recommend minimum evaluation dataset sizes of 1,000–2,000 report pairs** for stable CheXbert-based metrics, with confidence intervals mandatory for smaller evaluation sets. The persistent variability even at larger scales underscores the need for multiple evaluation runs or bootstrapping approaches.


---


Talk to us


## Interested in next-generation AI for radiology?


[Talk to a founder](https://cal.com/team/mecha-health/30mins)


From the journal


## Continue reading


[Research Foundation Models Evaluations Research · February 7, 2025 · 5 min read Mecha Net v0.1 Our first performance update for Chest X-ray report generation. Read article](https://www.mecha-health.ai/blog/Mecha-net-v0.1)[Research Foundation Models Research · November 12, 2025 · 20 min read Qualitative Case Studies for Assessing Foundation Models Most metrics used to evaluate machine learning models are quantitative, but qualitative assessment is crucial. Read article](https://www.mecha-health.ai/blog/Assessing-models-qualitatively)
