---
schema_version: "1.0.0"
document_id: "6fae963464bedb86d43c1883362668e4b48ed85e36fabd2965fa4ab9e8d13846"
company_key: "yc-segmed"
company: "Segmed"
source_id: "yc-segmed-news-import-4f813eeed56d"
canonical_url: "https://www.segmed.ai/resources/blog/why-most-clinical-ai-studies-disappear-a-successful-colorectal-surgery-prediction-model"
published_at: null
first_seen_at: "2026-07-22T12:58:23.262945+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:4fd018cfffa7176949c82e5ff78c0dd7db9794b85cf44ed2daf3dae443108311"
---

# Why Most Clinical AI Studies Disappear: A Successful Colorectal Surgery Prediction Model

#### TL;DR


- *A Danish AI model predicting 1-year mortality after colorectal cancer surgery was successfully deployed in clinical practice - rare for clinical AI.*
- *Patients receiving AI-guided care had 36% fewer postoperative complications compared to standard care.*
- *Cost savings reached $2,847 per patient in the first year.*
- *Success relied on a high-quality national dataset, surgeon involvement, and prediction linked directly to treatment.*
‍


---


‍


### Most clinical AI models stop at performance metrics. Here’s a Danish colorectal surgery model that made it into practice, and what we can learn from it.
‍


AI-based tools for personalized clinical decisions are still largely limited to research settings. We face substantial complexity in both building and implementing these models. Studies often develop a model, report an area under the curve (AUC), and then simply disappear.


This week, I came across an interesting article.[Andreas Weinberger Rosen and his Danish co-authors](https://www.nature.com/articles/s41591-025-03942-x) developed a model to predict 1-year mortality after colorectal cancer (CRC) surgery. This was a rare case where the model didn’t stop at development and actually made it into clinical practice. You can read the article[here](https://www.nature.com/articles/s41591-025-03942-x) . As the authors conclude:
‍


> “We demonstrate the clinical utility of AI technologies and offer an adaptable framework for further scalability to other health care fields.”


This model actually made it into real-world practice. What made it work?


The authors used a nationwide dataset of 18,403patients for model development and internal validation. They then used a retrospective cohort of 806 patients for external validation, testing the model on real-world data, this cohort served as the control group. After that, they implemented the model prospectively in clinical practice, applying personalizedperioperative treatment. This became the intervention group, later comparedwith the control group.


The final model included 58 variables out of aninitial 8,694. It achieved an AUROC of 0.82 (95% CI, 0.81–0.84) in thedevelopment set, 0.77 (95% CI, 0.74–0.80) in internal validation, and 0.79 (95%CI, 0.71–0.87) in external validation. The model maintained solid performancefrom development to real-world testing.


The authors defined four risk groups based onpredicted 1-year mortality: A ≤1%, B 1–5%, C 5–15%, and D >15%. Moreintensive perioperative intervention bundles were then applied according torisk level in the intervention group.


Postoperative medicalcomplications occurred in 23.7% of the intervention group versus 37.3% in thecontrol group, a roughly 36% relative reduction (OR 0.53; 95% CI, 0.36–0.76;P<0.001). This also translated into cost savings, an estimated US$2,847 perpatient during the first year after surgery.


The authors suggest a few reasons this worked: ahigh-quality dataset prospectively collected and widely used for clinical careand research in Denmark; close involvement of colorectal surgeons, which keptthe model focused on a real clinical problem; validation on local data, wherethe model would actually be used; and linking prediction to predefinedperioperative intervention bundles.
‍


> Clinical Al succeeds when it starts with a real problem,uses high-quality data, and is built to work in practice.


However, some limitations remain. The study cannotestablish a causal relationship between personalized treatment and improvedoutcomes, nor identify which specific component is driving the benefit. Theinterventions were not fully individualized, and the prospective cohort wassmall. The authors also suggest that future studies could use moredata-efficient evaluation methods. I also think performance bias is hard toignore here, since the prospective cohort was managed by a team aware of thestudy (classic Hawthorne effect).


So yes, they developed a clinically useful AI tool in this study. Building useful models in medicine is not like predicting mechanical failure from machinery data. We need a real clinical problem, a high-quality dataset, and a way to actually make it work in practice. You don’t often see all three come together. I’d like to see more studies like this: clinical AI tools that don’t just perform well, but actually make it into practice.


‍


---


‍


## Frequented Asked Questions - F.A.Q.


‍


### **What did the AI model actually predict?**


**‍** The model predicted 1-year mortality risk after colorectal cancer surgery, classifying patients into four risk groups (≤1%, 1–5%, 5–15%, and >15%). Each group then received a matched perioperative care bundle based on their risk level.


‍


### **How is this different from other clinical AI studies?**


**‍** Most clinical AI studies stop at reporting performance metrics and never reach patients. This one went through development, internal validation, external validation on local data, and prospective deployment in real clinical practice - a rare full journey from model to bedside.


‍


### **Can these results be trusted?**


**‍** The findings are promising but come with caveats. The study cannot prove causation, the prospective cohort was small, and the care team knew they were part of a study - introducing potential performance bias (Hawthorne effect). Larger, blinded trials would be needed to confirm the magnitude of the benefit.


‍


---


‍


## Reference


Rosen AW, Sørensen MS, Achiam MP, Gögenur I, Rasmussen LS, Achiam MP, et al. AI-guided personalized perioperative treatment in colorectal cancer surgery. Nature Medicine \[Internet\]. 2025 \[cited 2026 May 20\]. Available from:[https://www.nature.com/articles/s41591-025-03942-x](https://www.nature.com/articles/s41591-025-03942-x)


‍
