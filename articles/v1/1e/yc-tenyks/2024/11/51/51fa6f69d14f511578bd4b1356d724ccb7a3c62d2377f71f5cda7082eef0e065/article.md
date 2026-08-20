---
schema_version: "1.0.0"
document_id: "51fa6f69d14f511578bd4b1356d724ccb7a3c62d2377f71f5cda7082eef0e065"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/visual-intelligence-foundation-models-satellite-analytics-for-deforestation----part-2"
published_at: "2024-11-29T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:3fd4939e42e4af73887791fecb161aa5bc94008fc0042db58394079acf2012a5"
---

# Visual Intelligence: Foundation Models + Satellite Analytics for Deforestation — Part 2

In Part 2, we explore how Foundation Models can be leveraged to track deforestation patterns.


‍


Building upon the insights from our Sentinel-2 pipeline and Central Balkan case study, we dive into the revolution that foundation models have brought to segmentation tasks.


‍


👉[Part 1 — Vision Intelligence: Foundation Models + Satellite Analytics for Deforestation](https://medium.com/@tenyks_blogger/vision-intelligence-sam2-satellite-analytics-for-deforestation-part-1-85aa4d005aa1)


‍


### Table of Contents


1. Recap from Part 1
2. Segmentation-based Foundation Models for deforestation tracking
3. Conclusions


‍


## 1. Recap from Part 1


### 1.1 Key findings from satellite data analysis


Figure 1. Summary of forest analysis for Central Balkan Park


‍


Our[analysis of Central Balkan National Park](https://medium.com/@tenyks_blogger/vision-intelligence-sam2-satellite-analytics-for-deforestation-part-1-85aa4d005aa1) using satellite data from 2016 to 2021, shown in Figure 1, revealed three critical insights about forest dynamics and health.


- **Forest Health Trajectory**
The park demonstrated remarkable improvement in overall forest health, with scores rising from 0.49 in 2016 to 0.59 in 2021. A particularly significant milestone was reached in 2017–2018 when the park’s health score surpassed the Global Forest Watch’s healthy forest threshold (0.50).
- **Vegetation Vitality Patterns**
The NDVI-based forest vitality analysis revealed a nuanced pattern of decline and recovery:
- Initial period (2016–2018): Slight decline from 0.9 to 0.85 in vitality scores
- Recovery phase (2018–2021): Steady improvement reaching an impressive score of 1.0
- **Forest Coverage Stability**
Coverage analysis positioned Central Balkan National Park within a global context:
- Coverage fluctuated between 54.4% and 59.3%
- Peak coverage was observed in 2018
- Recent stabilization at 56–58%


‍


These findings highlight several key points relevant to our transition into foundation models: 1. Traditional satellite metrics provide robust baseline data but may miss subtle changes; 2. The interplay between health, vitality, and coverage suggests complex forest dynamics that could benefit from more sophisticated analysis.


‍


### 1.2 The Foundation Models Advantage


Figure 2. Four strengths of Foundation Models that could change how forests are analyzed


‍


Figure 2 presents a 2x2 grid highlighting four transformative capabilities of foundation models in forest monitoring.


‍


For instance, the Segment Anything (SAM) \[1\] model, and SAM2 \[2\], might represent a significant shift in forest monitoring capabilities. By potentially applying its zero-shot segmentation abilities to satellite imagery, SAM could automatically identify and delineate individual trees, forest patches, and areas of disturbance without requiring specific training data for each region.


‍


Unlike traditional pixel-based analysis, SAM could understand context and spatial relationships, potentially allowing it to distinguish between natural forest gaps and actual deforestation, or between seasonal changes and permanent forest loss.


‍


## 2. Segmentation-based Foundation Models for deforestation tracking


### 2.1 Evolution of Segmentation Models


Foundation models for segmentation have evolved significantly in recent years, as illustrated on Figure 3, moving beyond simple pixel-wise classification to understanding complex spatial relationships and temporal patterns.


‍


Figure 3. Evolution of Segmentation Models (including Foundation Models such as SAM)


\\These models can now handle multi-spectral satellite imagery and recognize contextual features. This advancement makes them particularly suited for deforestation monitoring, where understanding both spatial and temporal changes is crucial.


‍


### 2.2 Automated Sentinel-2 Pipeline


We developed a specialized pipeline for processing Sentinel-2 satellite imagery as shown in Figure 4.


Figure 4. Tenyks Sentinel-2 Pipeline


‍


Tenyks Sentinel-2 Pipeline transforms raw satellite data (see Figure 5) into deforestation areas (see Figure 6) through three key stages.


Figure 5. Raw satellite images extracted with our pipeline


‍


Finally, the processed sequences feed into our proprietary segmentation model, which analyzes changes and generates maps highlighting deforestation progression. This automated pipeline ensures continuous, reliable monitoring of forest changes while maintaining data quality throughout the process.


‍


### 2.3 Tenyks Segmentation Technology estimates deforestation 5x faster than traditional approaches ⏲


‍


TenyksSegModel, a proprietary segmentation model developed by Tenyks, enables the precise tracking of deforestation through satellite imagery analysis. It analyzes forest changes using satellite imagery from two time points, see the end result in Figure 6.


‍


Figure 6. Side by side comparison of non-segmented vs segmented deforestation for our target area


‍


‍


Here’s an overview of the main steps of our segmentation approach to obtain deforestation patterns:


**1. Segmentation:** Uses TenyksSegModel to identify forest vs. non-forest areas
**2. Mask Processing:** Aligns and cleans segmentation masks, removing noise
**3. Change Detection:** Identifies areas that changed from forest to non-forest
**4. Quantification:** Calculates total deforested area and rates of change
**5. Validation:** Cross-references results with historical data and ground truth


‍


A sample of the pseudo-code for this process is the following:


```text


"""
*** Pseudo-code ***


TenyksSegModel Deforestation Analysis Pipeline
--------------------------------------------
A comprehensive pipeline for analyzing deforestation using the
proprietary TenyksSegModel.


This implementation focuses on temporal change detection between
two satellite images.


Key Features:
- Forest/non-forest segmentation with confidence scores
- Temporal change analysis with noise reduction
- Pattern detection and quantification
- Multi-level validation system


Author: Tenyks
"""


def analyze_deforestation(image_t1: np.ndarray,
image_t2: np.ndarray,
region_of_interest: dict) -> dict:
"""
Main deforestation analysis pipeline using TenyksSegModel.
"""
# =============================================
# Step 1: Image Segmentation
# =============================================
def segment_image(image):
"""Segments single timepoint image using TenyksSegModel."""
model = TenyksSegModel.initialize(weights='pretrained')
return model.segment(image,
labels=['forest', 'non_forest'],
min_confidence=0.7)


# Process both timepoints
mask_t1, conf_t1 = segment_image(image_t1)
mask_t2, conf_t2 = segment_image(image_t2)


# =============================================
# Step 2: Mask Refinement
# =============================================
masks_processed = spatial.process_masks(
masks=(mask_t1, mask_t2),
confidences=(conf_t1, conf_t2),
min_segment_size=100
)


# =============================================
# Step 3: Change Detection & Analysis
# =============================================
changes = metrics.analyze_forest_changes(
before=masks_processed['t1_mask'],
after=masks_processed['t2_mask'],
confidences=(masks_processed['t1_conf'],
masks_processed['t2_conf'])
)


# =============================================
# Step 4: Quantification
# =============================================
deforestation_stats = metrics.quantify_changes(
changes=changes,
region=region_of_interest,
pixel_area=metrics.get_pixel_area(image_t1),
time_interval=metrics.get_time_interval(image_t1, image_t2)
)


# =============================================
# Step 5: Result Validation
# =============================================
validation_results = validation.evaluate_results(
metrics=deforestation_stats,
changes=changes,
historical_data=validation.load_historical(region_of_interest),
confidence_threshold=0.9
)


return {
'deforestation_metrics': deforestation_stats,
'validation_scores': validation_results,
'change_analysis': changes
}


```


## 3. Conclusions


As shown in Part 2, Foundation models have revolutionized our ability to track deforestation through satellite imagery. The progression from traditional CNNs like FCN (2015) to modern foundation models (2023) represents a significant leap in segmentation capabilities.


‍


Our implementation of the Sentinel-2 pipeline demonstrates how these advances can be practically applied, combining automated data extraction, temporal sequence generation, and model integration to monitor forest changes effectively.


‍


Despite foundation models’ capabilities like zero-shot segmentation, an ideal implementation is likely to include a combination of traditional methods (i.e., acting as a baseline),[Part 1](https://medium.com/@tenyks_blogger/vision-intelligence-sam2-satellite-analytics-for-deforestation-part-1-85aa4d005aa1) , and foundation models. However, five or ten years from now, foundation models are likely to become the new baseline, as old approaches lag versus the rapid advance of AI.


‍


### References


‍


\[[1](https://arxiv.org/abs/2304.02643) \] Segment Anything


‍


\[[2](https://arxiv.org/abs/2408.00714) \] SAM 2: Segment Anything in Images and Videos


‍


‍ **Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan, Botty Dimanov


‍


*If you’d like to know more about* ***Tenyks*** *, try*[sandbox](https://www.tenyks.ai/?utm_source=medium&utm_medium=article&utm_campaign=deforestation_part2) *.*


‍


‍
