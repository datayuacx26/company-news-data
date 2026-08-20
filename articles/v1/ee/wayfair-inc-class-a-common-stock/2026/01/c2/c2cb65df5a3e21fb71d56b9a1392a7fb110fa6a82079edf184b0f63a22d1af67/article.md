---
schema_version: "1.0.0"
document_id: "c2cb65df5a3e21fb71d56b9a1392a7fb110fa6a82079edf184b0f63a22d1af67"
company_key: "wayfair-inc-class-a-common-stock"
company: "Wayfair Inc."
source_id: "wayfair-inc-class-a-common-stock-news-import-42cff1750757"
canonical_url: "https://www.aboutwayfair.com/careers/tech-blog/how-we-validate-product-dimensions-at-scale-with-multimodal-genai"
published_at: "2026-01-15T21:17:32.519+00:00"
first_seen_at: "2026-07-22T19:30:59.923738+00:00"
fetched_at: "2026-07-28T22:23:32.114893+00:00"
content_hash: "sha256:caf7460e49033518738ab204d8af8ed4562d3cb673380775b10ee2d7baa8899e"
---

# How We Validate Product Dimensions at Scale With Multimodal GenAI

Imagine using an e-commerce site like Wayfair to find the perfect sofa for your living room, only to discover when it arrives that the six-foot length in the description actually measures closer to seven feet. Now you are debating if it's fine that it overlaps the carpet (your partner is shaking their head) or if you should arrange a return and start your sofa search all over. Situations like this highlight the impact on the customer of inaccurate product dimensions on an e-commerce site. They may be frustrated, lose a little trust in e-commerce purchases and maybe even put that sofa back on a truck to the warehouse.


It seems like it should be a straightforward process for Wayfair to receive product dimensions from a supplier and present them to a customer, but in practice it can get very complicated. A product can have many individual measurements in different units and multiple suppliers providing them. Wayfair may want to have a consistent interpretation of the height, width and depth of a sofa across all sofas that an individual supplier may not adhere to. A specification in a product listing may get changed over time, without cross-referencing other information in the listing. As a result, a customer browsing a listing may be confronted with conflicting confusing product information (Figure 1 shows an example) and not be sure what they are buying.


At Wayfair, we manage a catalog of over 40 million products from over 20 thousand global suppliers, with thousands of new items added daily, presenting us with the challenge of maintaining accurate product dimensions at a massive scale. To address this, we have built a dimension validation solution leveraging the strong capabilities of AI models (Gemini 2.5 flash).


The model flags conflicts between a product’s text and imagery and the dimensional values provided directly by the supplier. With careful prompt design, step-by-step instructions, few short examples and rule-based postprocessing, our model has shown strong performance relative to a human doing the same task. Our model consistently achieves over 85% precision and more than 70% recall for conflict detection, far surpassing earlier ML-based methods, which had less than 50% precision and 7% recall. And since it's more cost-effective to deploy an AI model at scale than to hire a fleet of humans to meticulously review product data around the clock, we can now ensure new products enter the site with accurate and reliable dimensions.


# How Our Model Works


Our model is built on Gemini 2.5 flash, a Google model. We provide the AI model with all relevant product information and let it determine whether these sources support or conflict with the supplier-provided dimension, enabling the model to flag potential conflicts. The model uses two categories of product data:


- **Textual attributes** including product names, descriptions, feature bullets and other values.
- **Visual information** dimensional images that show the product’s measurements.


We evaluate this data in a multistep process. First, the model extracts dimensional values from text and imagery independently and assigns a confidence score for each extraction, which is used during postprocessing. Next, we ask the reasoning model to reconcile the values from both sources and produce a final suggested value. To ensure the precision of this suggestion, the model is prompted to only provide a value when the extractions from different sources do not conflict with each other. Then we compare the model-suggested value with the supplier-provided value and see if it conflicts or not. Lastly, we drop cases where the model’s confidence is low or the difference is suitably small, ensuring that only meaningful and reliable conflicts are surfaced.


While this may seem like a surprising number of steps to do a task a human does in their mind, we found this workflow maximizes precision and recall and gives us the right points to interject the experience-guided direction to help the model make the same kind of subjective judgments as a Wayfair associate. And while our current process requires us to call the AI model several times, we found that using Gemini 2.5 Flash deployed via Gemini Batch API offered the right trade off between strength of reasoning required, model cost and prediction turnaround time.


# What We Learned


During our experiments, we learned several lessons that helped us improve the accuracy and stability of the system.


**1. Clear definitions and instructions matter**
It is a mistake to think the width of a product is always self-apparent. The Wayfair catalog is heterogeneous, including products from thousands of suppliers with diverse designs and conventions and confusing edge cases are abundant. If a product has an irregular shape (e.g., an inflatable palm tree), an extendable shapes (e.g., a dining table with an extension leaf), contains multiple pieces (e.g., a sectional sofa), or has unclear boundaries, it can be challenging for even humans to agree on the primary dimensions. To help the model reason correctly across these cases, our scientists incorporate clear dimension standards defined by the Wayfair Merchandising team and provide explicit, case specific instructions directly in the prompt.


**2. Tolerance is needed for small differences**
Not all disagreements are equally egregious. In practice, small differences generally have little impact on user experience. To avoid exhausting our suppliers with requests for minuscule changes, we introduce tolerance rules that only flags disagreements above a meaningful threshold.


# How We Evaluate the Model


Today, if our model process flags a problem, we ask the supplier to review the issue and change the product data. Thus, our model process does not need to have perfect precision. However, while we want to catch as much bad data as possible if we flood our suppliers with false positives we risk losing their trust and engagement. Therefore we prioritize precision over recall in our model performance.


Evaluation of model performance was performed over three phases. In the first phase, a curated evaluation set of approximately 5,000 human-labeled records was used to choose the best model design. In the second phase, we ran the model process on products not used during development and an internal QA process run by the Wayfair Merchandising team validated what the model flagged. In the last phase, we ran a pilot where we applied the model to over 100 thousand products in the Wayfair catalog and had suppliers themselves review the flagged cases. Successful completion of these three phases, achieving over 85% precision and more than 70% recall, made us confident the model was ready for full production release.


# What's Next


At Wayfair we are always improving! For this model, we see opportunities yet to improve the cost, model performance and business impact of our model.


**Cost -** The per-use cost of a GenAI multimodal reasoning model is always a concern in scaling an application over our massive catalog of products. We will continue refining the model architecture to simplify deployment and lower the cost per application.


**Model Performance** - By incorporating additional sources product data or other features we look for ways to improve the precision of our flagged issues.


**Business impact** - We would like to increase the reliability of the suggested corrections our system makes. If we can achieve that with sufficiently high precision, then we can simply just correct the dimensional value, without requiring the validation of the supplier.


Altogether, these improvements will reduce the need for manual intervention, provide accurate data for every Wayfair product and ensure every customer has a delightful experience sitting on their new six-foot sofa.


*The authors would like to thank*Joe Walt *,*Jeff Arena *,*Ryan Maunu *,*Fuller Henriques *and the entire Product Content Intelligence (PCI) Machine Learning Science and Engineering team, as well as the Merchandising team, for their contributions to this work.*
