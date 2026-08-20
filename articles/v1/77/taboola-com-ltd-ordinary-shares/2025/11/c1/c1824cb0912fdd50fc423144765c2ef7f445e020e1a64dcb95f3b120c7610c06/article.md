---
schema_version: "1.0.0"
document_id: "c1824cb0912fdd50fc423144765c2ef7f445e020e1a64dcb95f3b120c7610c06"
company_key: "taboola-com-ltd-ordinary-shares"
company: "Taboola.com Ltd."
source_id: "taboola-com-ltd-ordinary-shares-news-import-1349b39c90c0"
canonical_url: "https://www.taboola.com/engineering/recsys-2025-ai-recommendation-trends/"
published_at: "2025-11-18T15:11:39+00:00"
first_seen_at: "2026-07-22T15:33:14.584366+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:c57c1940cc29c966224a26ece69f19b42e2a99b3d961360c59a76d3ee3f160a5"
---

# Inside RecSys 2025: How LLMs Are Rewriting the Rules of Recommendations

At[RecSys 2025](https://recsys.acm.org/recsys25/) in Prague, one trend was impossible to miss: Large language models (LLMs) and recommender systems are converging, signaling a new era for[personalization](https://www.taboola.com/marketing-hub/ad-personalization/) .


Over several days of keynotes, poster sessions, and insightful panels, researchers and practitioners explored how[AI](https://www.taboola.com/marketing-hub/artificial-intelligence/) is shaping the very core of personalization, as well as what it will take to turn these breakthroughs into real-world systems. Here are some of my top takeaways.


## Relational Foundation Models: The Next Leap for Structured Data


Imagine being able to train one model that could power predictions across dozens of use cases, from click-through rates to content engagement. That’s the promise behind Relational Foundation Models (RFMs), the focus of the standout keynote by[Jure Leskovec](https://cs.stanford.edu/~jure/) , professor of computer science at Stanford University.


Foundation models have transformed our understanding of unstructured data like text, images, and code. But as Leskovec put it, structured data such as transaction logs and customer journeys are still locked behind brittle pipelines and handcrafted machine learning models.


RFMs aim to change that. As Leskovec’s presentation pointed out, RFMs provide a single, general-purpose model pretrained on relational structures that can perform in-context learning across a variety of downstream tasks. Leskovec compares their behavior to how LLMs handle text.


For companies working with large-scale structured data, this technology promises:


- Faster iteration without repetitive model design or feature engineering.
- Improved predictive accuracy across diverse business use cases.
- Simplified infrastructure for teams managing multiple models.


For those building and maintaining predictive systems at scale, the potential is impressive. An RFM backbone could eventually support multiple use cases, from CTR prediction to user engagement modeling, all from one unified architecture. At Taboola, it’s exciting to consider how RFMs could streamline the predictive layers behind ad CTR models, personalization systems, and more.


## Tackling the Cold Start Problem in Sequential Recommendations


Another standout presentation was[Let It Go? Not Quite: Addressing Item Cold Start in Sequential Recommendations With Content-Based Initialization](https://dl.acm.org/doi/10.1145/3705328.3748038) . This session highlighted one of the industry’s biggest challenges: handling new items with little or no interaction data.


By limiting embedding, and retraining and seeding models with content-aware signals, the method outlined by the authors can improve sequential recommendation accuracy. This is particularly relevant for dynamic creative optimization workflows that frequently introduce new creatives or formats.


It’s worth deeper exploration for anyone using GRU4Rec or other session-based models in high-velocity environments.


## Data Quality and Shapley-Based Filtering


When it comes to data, the old adage, “quality in, quality out” applies. For that reason, several researchers are looking at data from new angles.


A particularly interesting study applied Monte Carlo-based approximations of Data Shapley values to identify harmful data points in recommender training sets. These included issues like bot traffic and poor metadata, which can affect model performance. By filtering these outliers, researchers achieved measurable gains in performance on KNN-style recommenders, serving as a reminder that model improvements often start with better data hygiene.


## Negative Feedback and Bias Correction in Recommendations


When building recommendation models, positive feedback like clicks, views, and purchases get most of the attention. But at RecSys, the keynote by Xavier Amatriain, VP of AI Products at Google, titled[Recommending in the Age of AI: How We Got Here and What Comes Next](https://www.youtube.com/watch?v=TlR7douxQRM) highlighted the value of negative feedback.


In his presentation, Amatriain highlighted the paper[Balancing Fine-tuning and RAG: A Hybrid Strategy for Dynamic LLM Recommendation Updates](https://dl.acm.org/doi/abs/10.1145/3705328.3748105) . Its authors showed that combining periodic fine-tuning with retrieval-augmented generation helps models adapt to changing user interests. This approach avoids driving up compute costs, while keeping recommendations fresh. In large-scale YouTube tests, this hybrid method delivered measurable performance gains.


If you’d like to read more on some interesting topics including, but not limited to negative feedback and bias correction, I find these papers especially helpful:


- [Benefiting From Negative Yet Informative Feedback by Contrasting Opposing Sequential Patterns](https://dl.acm.org/doi/abs/10.1145/3705328.3759333) : The authors trained two transformer models: one for positive actions and one for negative, and compared the patterns between them, slightly outperforming the state-of-the-art deep model, SASRec.
- [Unobserved Negative Items in Recommender Systems: Challenges and Solutions for Evaluation and Learning](https://dl.acm.org/doi/abs/10.1145/3705328.3759315) : Models often assume that items a user has never seen are “negative,” which can distort training results. The paper proposed a statistical correctional method called inverse probability weighing as a way to make these evaluations more reliable.
- [Addressing Multiple Hypothesis Bias in CTR Prediction for Ad Selection](https://dl.acm.org/doi/abs/10.1145/3705328.3759299) *:* This is one of the most relevant papers in our domain, proposing a post-processing calibration method that corrects bias in[click-through rate (CTR)](https://www.taboola.com/marketing-hub/click-through-rate/) predictions. This approach can be paired with any model and has already improved cost per acquisition and CTR at LinkedIn.
- [RecViz: Intuitive Graph-based Visual Analytics for Dataset Exploration and Recommender System Evaluation](https://dl.acm.org/doi/abs/10.1145/3705328.3759335) : Proposes using a graphics processing unit-accelerated visualization tool that helps teams quickly explore datasets and evaluate recommender models through interactive graph views.
- [Revisiting the Performance of Graph Neural Networks for Session-based Recommendation](https://dl.acm.org/doi/pdf/10.1145/3705328.3748156) : Shows that with proper tuning, older models like GRU4Rec can outperform newer graph neural networks. The results show that optimization and evaluation practices matter as much as investing in new architecture.


## The Consensus


The dominant theme at RecSys 2025 was the growing integration of LLMs into recommendation systems. While these models show great promise, the field is still waiting for a true breakthrough. Key challenges include the dynamic nature of user interest and the trade-off between computational complexity and real-time performance. Those constraints continue to limit widespread adoption in production environments.


A few studies, such as the one presented by Google, demonstrated ways to combine advanced LLM techniques to better meet real-world applications. That signals that the industry is making meaningful progress.


Notably, it was striking how few sessions at RecSys 2025 focused on advertising or CTR prediction: a sign that some of the field’s most competitive advances remain behind closed doors, underscoring how much innovation is happening privately even as the broader research community moves forward.
