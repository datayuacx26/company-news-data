---
schema_version: "1.0.0"
document_id: "f59d07dda98ae44d3edb563728a55d6106be3d6344780187b7f73524bb2b280f"
company_key: "yc-mecha-health"
company: "Mecha Health"
source_id: "yc-mecha-health-news-import-dc619b8bf4eb"
canonical_url: "https://www.mecha-health.ai/blog/Why-radiologists-make-mistakes"
published_at: "2025-05-28T00:00:00+00:00"
first_seen_at: "2026-08-10T00:30:28.783990+00:00"
fetched_at: "2026-08-10T00:30:30.164114+00:00"
content_hash: "sha256:2ff5c02430506ee4fe6969822b08c0a6af3e2bc284819194bf80b5760036cc1c"
---

# Why radiologists make mistakes

Modern medicine continues to proceed with an unacceptably high rate of diagnostic error. The burden of these errors is profound. Diagnostic errors still account for 6 to 17% of all adverse events in hospitals and 10% of all patient deaths \[1\]. They are the most common type of medical malpractice claim and the most common cause of preventable harm in healthcare \[1\]. In 2015, a committee of the National Academies of Sciences, Engineering, and Medicine published a report on diagnostic errors in healthcare which concluded:


> **Most people** will experience **at least one diagnostic error** in their lifetime, **sometimes with devastating consequences** .


The idea that *most* people will experience such an error and possibly come to great harm from it is absurd given that diagnostic errors are, in theory, preventable.


Meanwhile, **daily** error rates in Radiology are estimated to be between 3% and 5%. These figures only reflect real-time errors. If we consider errors that are not caught in real time by performing retrospective analyses, the error rate can be as high as 30% \[2\]. One paper found that 19% of lung cancers with a median diameter of 16 mm were missed on chest X-ray.


Let's pause here for a moment.


What we're saying is that when a human evaluates reports written by another human, we find that up to one in three reports has at least one incorrect finding, including cm-scale cancers. Given that most clinical specialties are becoming increasingly dependent on radiology to diagnose medical conditions, this is a staggering figure. In this post, we will explore the reasons behind these errors and what could be done to mitigate them.


## Definition of diagnostic error


Diagnostic error was defined by the 2015 National Academies of Sciences committee as **failure** to: a) Establish an **accurate** and **timely** **explanation** of a patient's health problem(s); or b) **Communicate** that explanation to the patient.


## Diagnostic errors in radiology


Given the above definition, we propose to break down the sub-components of radiological diagnostic errors as follows:


- Issues of **accuracy** .


- **Perceptual errors** - A radiologist fails to detect an abnormality present in a medical image.
- **Cognitive errors** - A radiologist misinterprets a detected abnormality.


- Issues of **timeliness** .
- Issues of **communication** .


### Issues of accuracy


The most fundamental challenge in radiology is achieving diagnostic accuracy—getting the right answer from the images. This deceptively simple goal is undermined by two distinct but interrelated failure modes that together account for virtually all radiological errors.


#### Perceptual errors


Perceptual errors represent the majority of radiological mistakes, accounting for 60-80% of all diagnostic errors. These are fundamentally failures of human visual processing—the abnormality is present in the image, but the radiologist's eye and brain fail to register it. The causes range from basic human limitations like fatigue and distraction to more complex psychological phenomena like satisfaction of search, where finding one abnormality reduces vigilance for additional findings. What makes these errors particularly vexing is that they often involve clearly visible pathology that becomes obvious in retrospect, highlighting the gap between theoretical detectability and practical human performance.


Analysis Framework


### Factors Associated with Perceptual Errors


Explore how human attention, cognitive bias, image quality, and technical factors conspire to make radiologists miss visible abnormalities.


Categories


Risk Factors


Solutions


Missed Finding


Attention


Bias


Image Quality


Order/Protocol/Technique


#### Cognitive errors


Even when radiologists successfully detect an abnormality, the diagnostic process can still fail through misinterpretation. Cognitive errors account for the remaining 20-40% of radiological mistakes and represent a different class of challenge entirely. Here, the issue isn't perception but reasoning—the radiologist sees the finding but draws the wrong conclusion about its significance. These errors stem from knowledge gaps, flawed clinical reasoning, and the myriad cognitive biases that affect all human decision-making. Anchoring bias may lock a radiologist onto an initial impression, while availability bias skews interpretation toward recently encountered cases. Unlike perceptual errors, cognitive errors often require more sophisticated interventions targeting the reasoning process itself.


Analysis Framework


### Factors Associated with Cognitive Errors


Discover how knowledge gaps, information deficits, and cognitive biases lead radiologists to misinterpret findings they successfully detect.


Categories


Risk Factors


Solutions


Misinterpretation / Wrong Diagnosis


Knowledge


Information


Bias


##### Diagnostic certainty


A closely related cognitive issue which does not necessarily equate to a frank misdiagnosis *per se* is subjective diagnostic confidence. It is not always easy to reason probabilistically -- when a radiologist says there is a 'possible' mass -- what does this mean? 20%? 65%? 0.5% likelihood? Recent work has proposed a guide to more accurately convey the diagnostic confidence of a radiologist \[3\]:


- **Most likely** means very high probability.
- **Likely** means high probability.
- **May represent** means intermediate probability.
- **Unlikely** means low probability.
- **Very unlikely** means very low probability.


It has recently been demonstrated that it is possible to improve calibration across radiologists (and specialists) when expressing uncertainty \[4\]. However, as Wang et al. \[4\] point out, it remains unknown how receptive radiologists are to calibration-improving suggestions and whether they can mentally adjust their use of certainty phrases effectively.


### Issues of timeliness


Getting the right diagnosis matters little if it arrives too late to change patient outcomes. Timeliness failures in radiology create a cascade of delays that can transform treatable conditions into irreversible harm. These delays occur at multiple points: patients may struggle to access imaging services due to geographic barriers, insurance hurdles, or limited availability; radiologists may face overwhelming workloads that create reporting backlogs; and critical follow-up recommendations may disappear into administrative gaps. The urgency varies dramatically—a stroke patient needs interpretation within minutes, while routine screening results can tolerate longer delays—but the underlying systems failures are often similar.


Analysis Framework


### Factors Associated with Timeliness


Examine how access barriers, reporting delays, and follow-up failures create critical gaps between imaging and patient care.


Categories


Risk Factors


Solutions


Timely


Access


Report


Follow-up


### Issues of communication


Perhaps the most preventable diagnostic failures occur not in image interpretation but in information transfer. A radiologist may reach the correct diagnosis yet still fail the patient if that critical information doesn't reach the right person at the right time in a comprehensible form. Communication breakdowns manifest in multiple ways: reports written in impenetrable medical jargon that obscure urgent findings; critical results buried in lengthy text that busy clinicians overlook; unclear chains of responsibility that leave everyone assuming someone else will act; and verbal communications that go unheard or are misunderstood. These failures are particularly tragic because they represent cases where the diagnostic process succeeded technically but failed practically. The solutions often lie not in better imaging or interpretation but in designing communication systems that are resilient, clear, and impossible to ignore when patient safety is at stake.


Analysis Framework


### Factors Associated with Communication


Investigate how accountability failures, unclear reporting, and verbal miscommunications prevent critical findings from reaching those who need them.


Categories


Risk Factors


Solutions


Communication


Accountability


Written


Verbal


## A fundamental challenge: What *is* the 'ground truth'?


Abujudeh et al. \[5\] investigated discrepancy rates between experienced radiologists when reading computed tomography (CT) scans of the abdomen and pelvis. Over 90 studies, they found interobserver and intraobserver major discrepancy rates of 26% and 32%, respectively. Here, 'major discrepancies' were defined as missed findings, different opinions regarding interval changes (e.g., disagreement that a cancer is improving), or indeed whether a recommendation is provided at all.


This raises a profound question: What exactly constitutes 'ground truth' in radiology? When experienced radiologists disagree on a CT scan's interpretation, how do we determine who is right? While this dilemma might seem insurmountable, practical solutions exist. We can establish ground truth through expert panel consensus or by validating findings against definitive clinical outcomes - whether through pathology results, eventual diagnoses, or patient outcomes. This framework allows us to meaningfully assess and improve diagnostic accuracy despite the inherent complexity of radiological interpretation.


## The promise of AI: A new ally in the fight against diagnostic error


The systematic nature of the challenges we've outlined—from perceptual limitations to communication breakdowns—suggests that purely human solutions may be insufficient. This is where artificial intelligence emerges not as a replacement for radiologists, but as a powerful ally in addressing each category of diagnostic error.


**For perceptual errors** , AI systems offer superhuman consistency. Unlike human radiologists, AI doesn't experience fatigue after reading hundreds of studies, doesn't suffer from satisfaction of search after finding one abnormality, and can maintain peak attention across every pixel of every image. Recent advances in medical AI have demonstrated the ability to detect subtle patterns invisible to the human eye—identifying early-stage cancers, spotting fractures in complex anatomical regions, and flagging critical findings that might otherwise be missed.


**For cognitive errors** , AI can serve as an intelligent second opinion, offering differential diagnoses ranked by probability and highlighting relevant clinical context. Rather than replacing human reasoning, AI can augment it by providing calibrated uncertainty estimates, surfacing rare diagnoses that availability bias might suppress, and offering evidence-based recommendations that counter anchoring effects. The technology can even help standardize the expression of diagnostic certainty, transforming vague phrases like "possible mass" into precise probabilistic statements.


**For timeliness issues** , AI offers the promise of immediate, 24/7 availability. Urgent cases can be triaged automatically, critical findings can trigger immediate alerts, and routine studies can receive preliminary reads that expedite care. This doesn't eliminate the need for human oversight, but it can dramatically compress the time between image acquisition and actionable diagnosis.


**For communication failures** , AI can ensure critical findings never get buried in lengthy reports. Natural language processing can automatically highlight urgent findings, generate patient-friendly summaries, and trigger appropriate notification cascades. The technology can standardize terminology, flag unclear language, and ensure that every critical finding receives appropriate follow-up tracking.


**[Mecha Health's](https://www.mecha-health.ai/) systems can learn from the very errors they help prevent.** Every missed finding and every communication breakdown becomes training data for more robust future performance. This creates a virtuous cycle where the technology becomes progressively better at anticipating and preventing the failure modes that plague human-only systems.


The goal isn't to create infallible machines, but to build human-AI partnerships that are more accurate, more timely, and more reliable than either could be alone. For the millions of patients who will inevitably encounter the healthcare system in the coming years, this collaboration represents the best hope for ensuring they receive the accurate, timely diagnoses their wellbeing depends on.


The technology is here. The evidence is mounting. The only question is how quickly we can responsibly integrate these capabilities into clinical practice to start preventing the preventable errors that continue to harm patients every day.


If you would like to cite this work, please use the following BibTeX entry:


```text
@misc{mecha2025mistakes,
author = {Ahmed Abdulaal and Ayodeji Ijishakin and Nina Montaña Brown and Hugo Fry},
title = {Why radiologists make mistakes},
year = {2025},
month = {May 28},
url = {https://mecha-health.ai/blog/Why-radiologists-make-mistakes},
note = {On the reasons radiologists make mistakes and how to improve report quality}
}


```


## References


1. Ball, John R., Bryan T. Miller, and Erin P. Balogh, eds. "Improving diagnosis in health care." (2015).
2. Lee, Cindy S., et al. "Cognitive and system factors contributing to diagnostic errors in radiology." American Journal of Roentgenology 201.3 (2013): 611-617.
3. Center for Evidence-Based Imaging. (n.d.). Diagnostic certainty scale. Brigham and Women's Hospital.[https://rad.bwh.harvard.edu/diagnostic-certainty-scale/](https://rad.bwh.harvard.edu/diagnostic-certainty-scale/)
4. Wang, Peiqi, et al. "Calibrating Expressions of Certainty." arXiv preprint arXiv:2410.04315 (2024).
5. Abujudeh, Hani H., et al. "Abdominal and pelvic computed tomography (CT) interpretation: discrepancy rates among experienced radiologists." European radiology 20 (2010): 1952-1957.
