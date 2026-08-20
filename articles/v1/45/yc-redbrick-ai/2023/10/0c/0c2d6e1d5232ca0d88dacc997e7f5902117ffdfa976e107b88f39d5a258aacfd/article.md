---
schema_version: "1.0.0"
document_id: "0c2d6e1d5232ca0d88dacc997e7f5902117ffdfa976e107b88f39d5a258aacfd"
company_key: "yc-redbrick-ai"
company: "RedBrick AI"
source_id: "yc-redbrick-ai-news-import-4b943afcd495"
canonical_url: "https://www.redbrickai.com/blog/2023-10-17-digital-twins"
published_at: "2023-10-17T00:00:00+00:00"
first_seen_at: "2026-07-24T11:23:16.380780+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:9d40110e8622706d677414671a9c285c617e79ae2624f5eceafa1c0776e7bc2c"
---

# The Power of Digital Twins

**The Power of Digital Twins**


**Introduction**


Unlike traditional evidence-based medicine, which relies heavily on average treatment effects from randomised clinical trials, personalised medicine aims to provide individualised treatments to patients.


By utilising advanced algorithms and AI, personalised medicine analyses complex datasets containing genomic information, medical imaging, and lifestyle factors to identify the most suitable treatment for each patient.


At the heart of this approach is the creation of high-resolution computational representations of individual patients, known as digital twins, which allow for the simulation and testing of numerous drugs and interventions to find the optimal treatment strategy.


**What are digital twins?**


Imagine a patient, Sarah, who has been diagnosed with a chronic heart condition that requires continuous monitoring and treatment. In this scenario, a digital twin is created to replicate Sarah's physiological characteristics and disease progression by feeding all available data including her medical history, lab results, genetic information and past diagnostic imaging data to the virtual model. Sarah is provided with a wearable device that continuously monitors her heart rate and blood pressure and updates the digital twin real-time. The virtual representation of Sarah that has now been created can simulate how Sarah's heart responds to different medications and optimise her drug regimen, offer personalized lifestyle recommendations to Sarah based on her data and even allow surgeons to plan an operation more precisely. Sounds impressive, right?


In fact, the term digital twinning has been around for some time as a concept from the field of engineering, which has been applied to complex systems such as airplanes or even cities. Digital twinning revolves around *in silico* (virtual) replications of a real object, system, or process based on a variety of data collected from it by multiple connected sources.


Digital twinning or 3D reconstruction of New York


Through the vast amounts of data that is fed to the virtual model, numerous simulations can be ran to predict treatment outcomes and personalised prognosis for the real-life twin. As early as 2009, *in silico* testing of an artificial pancreas algorithm that facilitated continuous monitoring of blood glucose and insulin delivery was approved by the Food and Drug Administration (FDA) as a substitute to animal studies (1). Similarly, in 2011 the FDA approved *in silico* evidence on the safety of a new cardiac pacemaker, which rested primarily on robust mathematical modelling that was validated with bench studies and studies in animals (2). One important difference is that the field of medical AI has rapidly advanced alongside an exponential increase in computational power and big data since that time, laying the foundation for a new era of digital twinning.


**How are we using these AI-driven digital twins today?**


Creating digital twins requires a comprehensive understanding of the physiological and anatomical complexities of specific organs and systems. For instance, the development of a virtual heart would require detailed modelling of the electrical propagation through the heart muscle, the mechanical contraction of the heart chambers, and the underlying structures of the cardiac tissue. In the past 5 years, scientists have made considerable strides in this domain, resulting in state-of-the-art virtual heart models. These models integrate cardiac imaging data from MRI or echocardiography, along with computational modelling of the electrical activity and genetic information, to provide an accurate and personalised representation of the heart.


One example of such a virtual heart model that has gained the attention of the scientific community is a model for the diagnosis of arrhythmogenic right ventricular cardiomyopathy (ARVC). ARVC is an inherited disease of the heart that affects approximately one in 1,000 people and can lead to dangerous heart arrhythmias. Diagnosing and predicting prognosis of ARVC in its early stages can be challenging. The condition can affect individuals both with and without known underlying genetic variants, and a considerable number of cases go unrecognised until the first presentation with cardiac arrest. To address the difficulty diagnosing and predicting prognosis of ARVC, scientists have recently developed and validated virtual heart models for these patients, integrating cardiac MRIs and genetic information. Using these personalised virtual hearts, the scientists were able to unveil complex mechanisms leading up to dangerous heart arrhythmias in ARVC patients. In particular, the virtual hearts made clear that each patient has a unique mechanism leading up to the onset of the arrhythmia, depending on the presence and type of genetic mutations. In some cases, the virtual models pointed towards scar tissue build-up, while in others, it was a combination of slower electric conduction and structural abnormalities in the heart. In this way, the digital twin was able to inform cardiologists on the exact causes and mechanisms that lead to the onset of dangerous heart arrhythmias for a specific ARVC patient, each with its own severity and therapeutic implication (3).


Beyond the diagnostic and prognostic value of these digital twins, these virtual hearts have practical applications in the management of heart arrhythmias as well. Dangerous heart arrhythmias are typically treated with catheter ablation, a procedure that delivers energy to specific areas of the heart that are causing arrhythmias to occur. However, the challenge of this procedure lies in the accurate localisation of lesions in the heart that need treatment, which can lead to ineffective ablations and an increased risk of the arrhythmias to recur. To circumvent this, scientists recently developed digital twins with the goal of providing clinicians with a roadmap for the treatment of these lesions before the actual procedure (4). By simulating the delivery of electrical stimuli to different parts of the virtual heart and observing the propagation of this electrical current through the heart, they were able to pinpoint the exact location of the heart lesions responsible for the dangerous arrhythmia. The virtual model not only provided cardiologists with more accurate mappings of vulnerable heart lesions, it is also a non-invasive alternative that does not require any incisions, puncturing of the blood vessels or sedation. The findings of this study were published in the scientific journal *Nature Biomedical Engineering* , and marked a major step forward in the prevention and treatment of dangerous arrhythmias, in particular for patients with an elevated risk of these arrhythmias already.


**What are the risks?**


With all the high expectations surrounding digital twins in personalised medicine, it is natural to wonder why they are not yet ubiquitous. The answer lies in the significant risks and challenges associated with their widespread implementation.


First, the development of digital twins is extremely computationally expensive. Creating accurate virtual representations of individual patients demands a combination of mechanistic and statistical modelling to fully capture the complexity of an individual patient's physiology. This results in an incredibly high computational burden.


Second, the issue of explainability is a critical concern. As digital twins rely on complex algorithms and AI models to simulate and predict patient responses to treatments, the lack of transparency in these models can hinder their adoption in clinical practice. Clinicians and patients need to understand how and why specific treatment recommendations are made by the digital twin, and the "black-box" nature of some AI models can be a barrier to their widespread acceptance.


**Round-up**


Digital twins are revolutionising the landscape of personalised medicine. Gone are the days of one-size-fits-all treatments, as patients now receive tailored care that can maximise their chances of treatment benefit. As technology continues to advance, computational capabilities improve and our understanding of these models grow, digital twins are poised to play a crucial role in empowering clinicians and patients to optimise clinical decision-making.


**References**


Kovatchev, B. P., Breton, M., Man, C. D., & Cobelli, C. (2009). In silico preclinical trials: a proof of concept in closed-loop control of type 1 diabetes. Journal of diabetes science and technology, 3(1), 44--55.[https://doi.org/10.1177/193229680900300106](https://doi.org/10.1177/193229680900300106)


Faris, O., & Shuren, J. (2017). An FDA Viewpoint on Unique Considerations for Medical-Device Clinical Trials. The New England journal of medicine, 376(14), 1350--1357.[https://doi.org/10.1056/NEJMra1512592](https://doi.org/10.1056/NEJMra1512592)


Zhang, Y., Zhang, K., Prakosa, A., James, C., Zimmerman, S. L., Carrick, R., Sung, E., Gasperetti, A., Tichnell, C., Murray, B., Calkins, H., & Trayanova, N. (2023). Predicting Ventricular Tachycardia Circuits in Patients with Arrhythmogenic Right Ventricular Cardiomyopathy using Genotype-specific Heart Digital Twins. medRxiv : the preprint server for health sciences, 2023.05.31.23290587.[https://doi.org/10.1101/2023.05.31.23290587](https://doi.org/10.1101/2023.05.31.23290587)


Prakosa, A., Arevalo, H. J., Deng, D., Boyle, P. M., Nikolov, P. P., Ashikaga, H., Blauer, J. J. E., Ghafoori, E., Park, C. J., Blake, R. C., 3rd, Han, F. T., MacLeod, R. S., Halperin, H. R., Callans, D. J., Ranjan, R., Chrispin, J., Nazarian, S., & Trayanova, N. A. (2018). Personalized virtual-heart technology for guiding the ablation of infarct-related ventricular tachycardia. Nature biomedical engineering, 2(10), 732--740.[https://doi.org/10.1101/2023.05.31.23290587](https://doi.org/10.1101/2023.05.31.23290587)


‍
