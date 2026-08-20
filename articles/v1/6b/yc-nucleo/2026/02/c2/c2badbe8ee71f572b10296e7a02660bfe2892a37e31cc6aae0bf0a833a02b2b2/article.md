---
schema_version: "1.0.0"
document_id: "c2badbe8ee71f572b10296e7a02660bfe2892a37e31cc6aae0bf0a833a02b2b2"
company_key: "yc-nucleo"
company: "Nucleo"
source_id: "yc-nucleo-rss-fea5ba462861"
canonical_url: "https://nucleoresearch.substack.com/p/whats-new-in-nucleo"
published_at: "2026-02-05T17:21:19+00:00"
first_seen_at: "2026-07-27T04:02:27.005796+00:00"
fetched_at: "2026-07-28T22:21:32.388193+00:00"
content_hash: "sha256:45164bc002af170283b4bc5061edbfffe4652e6f07f0e6999836e92702192798"
---

# What’s new in Nucleo

# What’s new in Nucleo


### A brief overview of the new features in the Nucleo platform


[Angelica Iacovelli](https://substack.com/@angelicaiacovelli)


Feb 05, 2026


We’re excited to share a few recent updates to the Nucleo platform. Our team has been focused on making image analysis faster and more actionable for clinical and research workflows. While many improvements are happening behind the scenes, here are our


**top three feature highlights for January** .


**Pick 1: Colormap-based visualization of body-composition masks**


Adjusting the colormap range in the skeletal muscle mask at the L3 vertebra.


In addition to our classic colors for body composition masks (red for skeletal muscle, blue for subcutaneous fat, yellow for visceral fat), Nucleo now supports colormap-based visualization of Hounsfield Unit (HU) values within each mask. Users can interactively adjust the minimum and maximum HU range directly in the UI, making it easy to emphasize specific tissue characteristics. This enables use cases such as exploring muscle quality and fatty infiltration, highlighting regions of unusually high or low tissue density, and visually assessing subtle changes that may not be obvious with uniform coloring alone.


**Pick 2: Side-by-side comparison of scans**


Side-by-side comparison of two CT scans with body composition masks at the L3 vertebra.


Nucleo now supports longitudinal, side-by-side comparison of scans. With a DICOM image selected in the viewer, right-clicking another scan from the same subject opens a synchronized comparison view. Scans are automatically aligned at the L3 vertebra detected by our algorithm, and a single scroll bar allows simultaneous navigation through axial slices. The body-composition panel displays trends in skeletal muscle index and cross-sectional areas, making it easy to assess changes over time at a glance.


**Pick 3: Automatic lung-nodule detection (experimental)**


Example of a lung nodule detected with 100% confidence.


Building on Nucleo’s existing lung-nodule segmentation and LLM-based benign vs malignant classification tools, we’ve added an experimental machine-learning algorithm for automatic lung-nodule detection. The model identifies nodule locations, estimates size, and reports confidence scores. Internal testing shows promising recall (true positive rate). Over the coming months, we plan to validate this feature in clinical settings and further refine performance using additional data.


We’d love to hear your feedback. If you’d like more information, a walkthrough of these features, or have suggestions for what you’d like to see next in Nucleo, feel free to reach out. We’re always happy to hear from the community.


[Book a meeting](https://angelicatazr.setmore.com/angelica)
