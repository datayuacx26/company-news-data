---
schema_version: "1.0.0"
document_id: "5e7556eb0b5b505f1739f0cf66eda7c13ab4d82444a9c473c773c1fcb9eea003"
company_key: "yc-nucleo"
company: "Nucleo"
source_id: "yc-nucleo-rss-fea5ba462861"
canonical_url: "https://nucleoresearch.substack.com/p/whats-new-in-nucleo-5b6"
published_at: "2026-08-04T15:01:07+00:00"
first_seen_at: "2026-08-04T15:43:43.285353+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:67487e0ff91d438e3ef6a411c097a80d51f33131dabe21657f5c305922248cd2"
---

# What's new in Nucleo

# What's new in Nucleo


### A brief overview of recent company and product updates from the Nucleo team.


[Angelica Iacovelli](https://substack.com/@angelicaiacovelli)


,[Luca Pegolotti](https://substack.com/@lucapegolotti)


, and[Tisha Mazumdar](https://substack.com/@tishamazumdarnucleo)


Aug 04, 2026


Three updates from the past few weeks: a Nebius AI Discovery Award, a considerably more detailed lung-nodule report, and an investment from Northwestern Medicine.


## Nucleo wins a Nebius AI Discovery Award 2026


Nucleo has won the Nebius AI Discovery Award 2026 in the medical imaging category, which carries $100,000 in GPU cloud credits on Nebius infrastructure.


The credits go into model development. We are training across larger and more varied CT and MRI cohorts, spanning different scanners, reconstruction kernels and acquisition protocols, which is what makes segmentation and detection generalize beyond the data they were built on.


## Lung nodules: from detection to characterization


In January the module localized nodules on chest CT and returned a confidence score for each candidate. It now produces a structured characterization report for every nodule it finds.


For each nodule the report gives a detection confidence, a benign versus malignant classification with its own confidence level, and the lobe it sits in. Measurements are volumetric: segmented volume, largest axial area, and maximum, minimum and mean axes in the axial, coronal and sagittal planes. Each nodule is typed as ground-glass, part-solid, solid, or calcified and high attenuation, with mean and median HU, the tenth and ninetieth percentiles, and the proportion of the volume falling in the solid and ground-glass ranges.


Every figure comes from a segmentation mask shown against the scan, with and without the overlay, so each measurement can be checked on the image rather than taken on trust. Detection confidence and classification confidence are reported separately, since they answer different questions.


With these improvements to the lung-nodule module, a single CT can now be analyzed in Nucleo to derive nodule characterization as well as quantitative tissue measurements, using our existing body-composition and organ-segmentation pipelines.


The module is adjunctive and for research use only, not for clinical use. The interpreting physician is responsible for confirming all results.


If you run a screening program, a nodule clinic, or a study in which nodule burden is an endpoint, we would love to hear from you.


## Northwestern Medicine invests in Nucleo


Northwestern Medicine Ventures, the venture arm of Northwestern Medicine, has invested in Nucleo. The investment comes through the first cohort of its healthcare accelerator, run with Founders Factory and announced this month in


[Axios](https://lnkd.in/eEeVMvje) , which backed six companies from the US and Europe.


Northwestern Medicine is one of the largest academic health systems in the United States, with 11 hospitals, more than 5,400 affiliated physicians and over 200 outpatient and diagnostic sites, anchored by Northwestern Memorial Hospital in Chicago. The program is built around testing and deploying inside that system rather than alongside it.


We welcome comments and criticism, particularly from readers running screening or oncology imaging workflows. If you would like more information, a walkthrough of these updates, or have suggestions for what you would like to see next in Nucleo, feel free to reach out.


[Book a meeting](https://angelicatazr.setmore.com/angelica)


If a colleague working in body composition or thoracic imaging would find this useful, please pass it on.
