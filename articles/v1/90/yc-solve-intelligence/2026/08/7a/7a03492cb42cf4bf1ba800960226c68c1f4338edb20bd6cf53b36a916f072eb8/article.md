---
schema_version: "1.0.0"
document_id: "7a03492cb42cf4bf1ba800960226c68c1f4338edb20bd6cf53b36a916f072eb8"
company_key: "yc-solve-intelligence"
company: "Solve Intelligence"
source_id: "yc-solve-intelligence-news-import-a6d4afe4e85a"
canonical_url: "https://www.solveintelligence.com/blog/post/ptab-case-studies-of-ai-disclosure-requirements-part-ii"
published_at: "2026-08-05T06:55:13.150+00:00"
first_seen_at: "2026-08-04T23:30:40.561607+00:00"
fetched_at: "2026-08-05T01:38:03.459815+00:00"
content_hash: "sha256:ca1c73ae6600a0c0a0afa3c24807a258fdba71c6b748404a82fa073f4146e5c0"
---

# PTAB AI Patent § 112 Case Studies: Part II

## Key Insights


- Detailed machine learning training methodology across the specification can satisfy § 112(a) written description requirements, even without specifying a precise model architecture.
- Relying on the knowledge of a person having ordinary skill in the art (PHOSITA) to satisfy § 112 requirements creates heightened exposure to § 103 obviousness challenges.
- Under the Manual of Patent Examining Procedure (MPEP), the absence of working examples in the specification cannot be the sole basis for an enablement rejection.


As AI and machine learning technologies continue to expand into many fields, the number of patent filings for AI-related inventions has also grown. When rejections of such patent filings are appealed, concrete interpretations of how patentability requirements apply to AI and machine learning inventions emerge. This article continues a series of case studies from the PTAB (also referred to as “the Board”) to understand AI disclosure requirements for patent applications.


In[Part I](https://www.solveintelligence.com/blog/post/ptab-case-studies-of-ai-disclosure-requirements-part-i) of the series, we studied three PTAB decisions from 2021, *Ex parte El-Masri* , *Ex parte Kirti* , and *Ex parte Allen* , which featured affirmations and reversals of § 112(a) rejections. In this article, we analyze another decision from the past year, *Ex parte Husain* , to further explore the disclosure requirements for AI inventions under § 112(a) with respect to written description and enablement.


Recent related developments, such as the Appeals Review Panel decision of the United States Patent and Trademark Office (USPTO) in *Ex parte Desjardins* , released on September 26, 2025, and its subsequent incorporation into the MPEP announced in the December 5, 2025[USPTO memorandum](https://www.uspto.gov/web/offices/pac/mpep/ANC-Desjardins-Memo-12-5-25.pdf) , mark an updated approach to evaluating the subject matter eligibility of computer-related technologies, especially AI and machine learning-related inventions.


In *Ex parte Desjardins* , the Appeals Review Panel reversed a § 101 rejection of a patent application related to the training of machine learning models, warning that “\[c\]ategorically excluding AI innovations from patent protection in the United States jeopardizes America’s leadership in this critical emerging technology” (p. 10 of the[Desjardins decision](https://www.uspto.gov/sites/default/files/documents/202400567-arp-rehearing-decision-20250926.pdf) ). This decision and the corresponding MPEP update promote AI patentability by cautioning Examiners against the overuse of § 101 rejections.


This article informs applicants’ approach to satisfying § 112(a) written description and enablement requirements: for both core machine learning and applied machine learning inventions, it is important to adequately describe how the invention provides a practical solution or improvement to a technical problem. Part I of this series provides an introduction to the classification of AI-related inventions and the methodology of these case studies. Here, we examine *Ex parte Husain* , a decision in which the Board reversed § 112(a) rejections pertaining to written description and enablement.


## *Ex Parte Husain* (SparkCognition): § 112(a) Rejection Reversed on January 29, 2026


*Ex parte Husain* (Appeal 2025-002663) centers on an applied machine learning invention. Ultimately, the Board found that the Examiner had imposed an unduly high standard for both written description and enablement, leading to a reversal of the § 112(a) rejections. Below, we examine the characteristics of the disclosure that led to this outcome.


U.S. Patent Application No. 17/468,135, entitled “Machine-Learning Augmented Synthetic Aperture Sonar” was filed on September 7, 2021, with SparkCognition, Inc. listed as the assignee. The application describes a machine learning-augmented sonar system for autonomous underwater devices. Specifically, the claims feature a machine learning algorithm that performs synthetic aperture sonar, which involves generating sonar images based on sonar input signals obtained from various receiver arrays. At the time of appeal, claim 1 read as follows:


The Examiner rejected claim 1 under § 112(a) for lack of written description and enablement. Regarding the written description, the Examiner indicated that the Specification does not “provid\[e\] any specific type of machine learning model used, any specific number of layers/node, any specific processing techniques performed by any of the particular layers within the machine learning model, or any specific training step performed on the machine learning model.”


The PTAB disagreed with the Examiner’s position, finding that the Specification incudes “a detailed description of how to generate and train a machine learning model to perform the claimed tasks.” Given the detailed description of generating and training various types of machine learning models in the specification, a PHOSITA would be able to select a “suitable” model for the claimed task.


This decision also highlights the trade-off between relying on the abilities of a PHOSITA to satisfy § 112 requirements and ensuring the invention remains non-obvious under § 103. The Board cautioned: “We note, however, that our decision here is premised, in large part, on Appellant’s assertion that certain aspects of the claimed invention would be known to a person of skill in the art. Should there be further prosecution in this application, the Examiner is encouraged to take these statements into consideration when evaluating whether the claimed invention is novel and non-obvious as required by 35 U.S.C. §§ 102 and 103.”


The Examiner’s argument and the PTAB’s reversal of the § 112 enablement rejection follow similar reasoning. The Examiner argued that, because the Specification did not disclose specific examples, a PHOSITA would require an undue amount of experimentation to implement the invention. However, the PTAB, citing the MPEP, emphasized that the “lack of working examples or lack of evidence that the claimed invention works as described should never be the sole reason for rejecting the claimed invention on the grounds of lack of enablement.” In this case, the Board concluded that a PHOSITA could implement the system without undue experimentation.


## Conclusion


In this article, we reviewed *Ex parte Husain* , a recent PTAB decision reversing rejections under § 112(a) for lack of written description and enablement. The Board found the claims at issue to be adequately supported by the detailed description, in particular by the sections that describe how to generate and train a machine learning model to perform the claimed tasks, even though the specification did not identify a particular model architecture or provide any working examples.


When considering the level of disclosure needed to comply with § 112, it is not only the content of the specification that matters, but also the content of the claims: whether a disclosure is deemed sufficient depends on what is being claimed. Indeed, the Board explains that “although these descriptions do not provide algorithm-level detail for each claimed operation, such detail is not always necessary, particularly when, as here, the claim does not specify a certain level of performance for its functional limitations (e.g., a particular degree of sonar-interference reduction).” Accordingly, applicants should consider whether their invention is applied or core machine learning, and identify which novel aspects warrant the most thorough description.


To ensure sufficient disclosure, applicants should avoid describing core machine learning components of an invention as black boxes, such as by merely reciting the desired inputs, outputs, or functionality. Providing explicit examples and implementation details, such as model architectures, training details, and data sets, can satisfy enablement and written description requirements without restricting the scope of claims solely to the specific examples in the specification.


## Frequently Asked Questions


**What do the § 112(a) written description and enablement requirements mean for AI patent applications?**


35 U.S.C. § 112(a) requires the specification to demonstrate that the inventor possessed the claimed invention (written description) and to teach a PHOSITA how to make and use the invention without undue experimentation (enablement). For AI and machine learning inventions, recent PTAB decisions such as *Ex parte Husain* indicate that a detailed account of how the claimed models are generated and trained can satisfy both requirements, even without source code or exhaustive implementation detail.


**Must an AI patent application disclose a specific model architecture to satisfy § 112(a)?**


Not necessarily. In *Ex parte Husain* , the Board found the written description requirement satisfied where the specification described, across more than 35 paragraphs, how to generate and train various types of machine learning models, even though it identified no particular architecture, layer configuration, or training step. Because the claims did not require a specific model configuration or level of performance, a PHOSITA could select a suitable model from the disclosed approaches.


**Can an Examiner reject an AI patent claim solely because the specification lacks working examples?**


No. The MPEP provides that a lack of working examples, or a lack of evidence that the claimed invention works as described, should never be the sole reason for an enablement rejection. The Board applied this principle in *Ex parte Husain* , reversing an enablement rejection premised on the absence of specific examples.


**What is the risk of relying on PHOSITA knowledge to satisfy § 112 requirements?**


Asserting that aspects of the claimed invention would be known to a skilled artisan can satisfy written description and enablement, but those same assertions may invite scrutiny under §§ 102 and 103. In *Ex parte Husain* , the Board expressly encouraged the Examiner to consider the Appellant's statements about PHOSITA knowledge when evaluating novelty and non-obviousness in any further prosecution.


**How should the level of disclosure differ between core machine learning and applied machine learning inventions?**


Core machine learning inventions improve the machine learning pipeline itself (for example, training techniques or model architectures), while applied machine learning inventions use machine learning to solve a problem in another technical field (for example, sonar imaging). In either case, the novel aspects of the invention warrant the most thorough description: core machine learning components should not be presented as black boxes defined only by their inputs, outputs, or desired functionality.


‍
