---
schema_version: "1.0.0"
document_id: "b38c4944175d924acf38f945a69bd8e19ff1ce7d665fd9c6b46f8814639d63fa"
company_key: "accuray-incorporated-common-stock"
company: "Accuray Incorporated"
source_id: "accuray-incorporated-common-stock-rss-f803293c21e1"
canonical_url: "https://www.accuray.com/blog/ai-in-radiation-oncology-what-it-takes-to-deploy-responsibly/"
published_at: "2026-03-16T20:05:57+00:00"
first_seen_at: "2026-07-20T23:20:13.437207+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:0ed867b32a7909c66c8cd1e26a6419983c9cafc8fee8f325f64d3c455de6ba7e"
---

# AI in Radiation Oncology: What It Takes to Deploy Responsibly

In January 2026, Unicancer hosted a day-long course on artificial intelligence in radiation oncology at the Geneva Innovation Hub in Genolier, Switzerland. Endorsed by ESTRO and ASTRO, sponsored by Accuray, the program drew attendees from 24 countries.


“Clinical deployment is often faster than collective understanding,” said Dr. Arnaud Beddok, a radiation oncologist at Institut Godinot in France who organized the course. “Tools arrive in our departments before we collectively develop shared frameworks to evaluate them.”


With contributors from radiation oncology, medical physics, law, and ethics, the course made the case for why shared frameworks around AI adoption matter and what it will take to build them.


## Supervised AI in thoracic radiotherapy workflows


Thoracic radiotherapy has always required careful management of cardiac dose. But growing attention to dose–effect relationships at the substructure level has outpaced the contouring workflows available to support it.


Dr. Loïg Vaugier, a radiation oncologist at ICO in France, described the traditional approach as treating the heart “like a big bowl without any granularity inside.” Together with co-presenter Dr. Alexandra Moignier, a medical physicist at ICO, he walked through a practical alternative. The approach starts with consensus definitions and then applies supervised automation to make the workflow feasible.


The working group was deliberately broad. Cardiologists and radiologists shaped the anatomical definitions while radiation oncologists and physicists grounded them in planning reality. As Vaugier put it: “Quality in, quality out.” An algorithm learns exactly what it receives, and inconsistent reference contours produce a model that reproduces inconsistency with confidence.


They trained on 80 contrast-enhanced CTs, choosing contrast for visibility of fine structures like coronary arteries. Since many departments plan on non-contrast CT, they tested on non-contrast scans as well. The tool generated 21 heart substructures in under a minute — fast enough for routine use, provided teams review with discipline.


## What validation revealed


Five radiation oncologists across three centers reviewed AI-generated contours on 30 CTs, producing roughly 600 clinician-corrected structures. Reviewers rated the unmodified contours satisfactory in almost 90% of cases. But variability was higher for coronary arteries than for larger chambers.


Vaugier was candid about what that means for supervision: “The radiation oncologist is not trained for such delineation of cardiac substructures. People will probably compensate for the gap in knowledge by using the tools and not being able to review the contours in detail.”


The group also compared 11 commercial solutions on paired contrast and non-contrast scans from 20 lung cancer patients. For coronary arteries, inter-solution variability reached the order of centimeters. Without transparency around training data, clinics have no way to tell whether a poor contour reflects a known limitation or a genuine error.


## The legal framework arriving in August 2026


Prof. Moïse Serero, a judge at the commercial chamber of the Tribunal des activités économiques de Paris, presented the regulatory landscape with a deadline attached: the EU AI Act takes full effect in August 2026.


Under the Act, medical AI falls into the high-risk category. Institutions must document how they use each tool and maintain human oversight at every stage. The final diagnostic or therapeutic decision must remain with the physician, and institutions must be able to demonstrate that it does.


Serero drew a sharp contrast between jurisdictions. In Europe, data protection regulation centers on patient rights — individuals can access, modify, or erase their personal data. The United States takes a more litigation-driven approach, where civil courts assign damages after the fact, and China requires providers of large language models to disclose how their models are built and treats data as a state-controlled asset.


## When your AI crosses borders


For departments deploying AI tools built across multiple jurisdictions, liability becomes difficult to assign. The device might be manufactured in one country, while the servers sit in another, and the software was developed in a third. Serero advised keeping everything within the same legal jurisdiction. If a responsibility issue arises across borders, determining who is competent to judge becomes its own problem.


His closing list of pitfalls: never rely solely on AI, never use public AI with patient data, never use AI off-label, never skip documentation, and never ignore AI errors.


## Ethics as an operating discipline


Prof. Bernice Simone Elger, a professor of ethics at the University of Basel, warned of what she called double paternalism — a dynamic where AI quietly begins to dictate clinical decisions at two levels. First, physicians accept AI recommendations without challenge, trusting the output because they want to appear competent. Then they present those recommendations to patients as settled conclusions. Two layers of unchecked authority, sitting where clinical judgment should be.


“We as doctors have to ask for explainable AI,” Elger said, “because we cannot control or influence in a reasonable way if it’s a black box.” Without that transparency, informed consent becomes performative — physicians cannot explain what they themselves do not understand.


Dr. Kamyar Shahrooz from Northeastern University made the case that clinicians are stewards of AI, not just users, responsible for questioning outputs and protecting patients from automated bias. His action research with clinical teams showed that targeted interventions like bias auditing and governance checkpoints produced measurable shifts. Confidence in identifying bias rose from 40% to 76%. Confidence in explaining or challenging AI-supported decisions increased from 48% to 84%.


## What readiness actually looks like


What each session made visible is how much work sits between a capable tool and a responsible deployment.


The contouring model needs reviewers trained to catch its failures. The legal framework needs documentation habits that most departments haven’t built. And ethical oversight needs governance woven into daily workflow, not added after the fact.


Readiness lives in that infrastructure, and the course showed that building it is the real work ahead.
