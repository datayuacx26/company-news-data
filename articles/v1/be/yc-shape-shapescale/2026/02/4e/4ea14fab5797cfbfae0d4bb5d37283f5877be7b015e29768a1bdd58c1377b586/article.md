---
schema_version: "1.0.0"
document_id: "4ea14fab5797cfbfae0d4bb5d37283f5877be7b015e29768a1bdd58c1377b586"
company_key: "yc-shape-shapescale"
company: "Shape (ShapeScale)"
source_id: "yc-shape-shapescale-news-import-6a7c420ccdb0"
canonical_url: "https://business.shapescale.com/content/posts/shapescale-visceral-fat-assessment-from-3d-body-mesh"
published_at: "2026-02-19T00:00:00+00:00"
first_seen_at: "2026-07-25T23:41:06.033086+00:00"
fetched_at: "2026-07-28T21:57:40.692150+00:00"
content_hash: "sha256:cdb9e7c274b3c07d75302140ba4a400ca01674712cf52ea63e3a967130e1f0f9"
---

# ShapeScale: Visceral Fat assessment from 3D body mesh

## **Abstract**


Visceral fat is an essential biomarker of cardiometabolic health, as it reflects the amount of fat surrounding internal organs and is strongly associated with insulin resistance, systemic inflammation, and increased risk of cardiovascular disease, type 2 diabetes, and mortality. DXA provides high accuracy but is costly and exposes participants to radiation.


We developed and internally validated ShapeScale, a 3D optical body scanner that predicts visceral fat using an AI model trained on paired 3D surface geometry and DXA data from 1,000 adults, incorporating both shape-derived and anthropometric features.


Within this training cohort, ShapeScale achieved a mean absolute error of 0.13 kg and an R² of 0.83 relative to DXA, indicating high agreement and supporting its potential as a fast, non-ionizing, and practical alternative for use in clinical, fitness, and consumer settings.


## **Introduction**


Precise quantification of visceral adipose tissue (VAT) enables the assessment of abdominal fat distribution, the identification of individuals at elevated cardiometabolic risk, and the stratification of susceptibility to insulin resistance, type 2 diabetes, cardiovascular disease, and mortality \[1\].


Optical 3D body scanning provides a safe, rapid, and repeatable alternative to conventional imaging for assessing abdominal adiposity. Earlier studies commonly relied on anthropometric proxies such as waist circumference, waist-to-hip ratio, or BMI, which reflect overall or central obesity but cannot distinguish visceral from subcutaneous fat. Recent work demonstrates that visceral adipose tissue can instead be accurately estimated from 3D surface geometry combined with anthropometric and demographic features using statistical and machine learning models, highlighting the potential of non-ionizing 3D scanning to capture internal fat distribution with high precision \[2\], \[3\].


Here we evaluate the performance of ShapeScale, a mesh-based optical approach, in 1,000 adults. Our analysis focuses on agreement with DXA, prediction accuracy across a broad visceral fat spectrum, and the potential of optical methods as a fast, non-ionizing alternative for assessing abdominal adiposity.


## **Background**


**DXA (Dual-energy X-ray Absorptiometry).** Widely used in body composition research and clinical settings, DXA provides visceral fat estimates but exposes participants to radiation, requires costly equipment, and can misestimate the body composition in bone-dense regions \[4\], \[5\].


**Optical Scanning.** Optical approaches avoid radiation and can be deployed in a variety of settings, offering a promising balance of safety, speed, and scalability.


## **ShapeScale Approach**


ShapeScale uses structured optical capture to generate a high-density, watertight 3D mesh of the human body in seconds. This process records detailed surface geometry with about one million points capturing abdominal contours and overall body shape.


An AI model trained on data extracted from these meshes estimates visceral fat mass in adults.


1. **Mesh Creation.** The system builds a precise 3D model of the body surface from multiple optical viewpoints.
2. **Pre-processing.** Meshes are divided into standardized body regions and anatomical cross-sections.
3. **AI Prediction.** A Gradient Boosting Regressor trained on paired 3D mesh–derived features, demographic variables, and DXA outcomes predicts visceral fat mass.


This pipeline enables ShapeScale to convert detailed body geometry and metadata into fast, non-ionizing estimates of visceral adiposity validated against DXA.


## **Participants and Data Collection**


We enrolled 1,000 adult volunteers, primarily residents of the San Francisco Bay Area, under written informed consent. Inclusion criteria: age ≥18 years and ability to stand unaided.


- **Demographics:** 662 male, 378 female, ages 18–75 years, BMI 15.0–59.3, VAT 0.0 - 3.05 kg.
- **Reference method:** Whole-body GE Lunar iDXA.
- **Pre-scan protocol:** Light clothing, no metal, no strenuous morning exercise, fasting ≥2 hours.
- **ShapeScale scans:** Three consecutive scans per participant, in a standardized stance, wearing skin-fitting underwear or sportswear(men without a top). Long hair tied up, no wrist jewelry. Scans performed the same day, typically within one hour of DXA.
- **Quality control:** Trained operators excluded implausible cases, including individuals with highly pronounced abdominal protrusion but minimal VAT, very low body mass with disproportionately high VAT estimates, and those of advanced age with normal body composition and near-zero visceral fat.


## **Results**


**ShapeScale vs DXA.** ShapeScale predictions showed strong agreement with DXA-derived ALM:


- Mean Absolute Error: 0.13 kg
- Root Mean Square Error: 0.20 kg
- R²: 0.83


Regression analysis (Figure 1) showed tight clustering along the line of identity with no systematic bias across the range.


## **Limitations**


Accuracy depends on mesh quality and adherence to scanning protocol, and individual body morphology:


- **Clothing:** Participants were instructed to wear skin-fitting underwear or sportswear with no top for men. Deviations from these guidelines (e.g., loose garments, compression clothing, t-shirts on male subjects) can introduce local surface artifacts on the mesh and affect predictions.
- **Posture and stillness:** The model assumes a standardized still stance and good posture. Variations such as moving, slouching, bent knees, asymmetric or abnormal arm placement alter surface geometry in ways that may perturb predictions.
- **Morphological extremes:** Overestimation can occur in individuals with highly developed abdominal musculature and minimal visceral fat, while underestimation may arise in obese participants with proportionally smaller waistlines. Compression wear will always alter prediction accuracy. The model is not designed for use in pregnant women, whose abdominal geometry does not reflect visceral adiposity.


Mitigation strategies include operator training, posture reminders, and automated QC checks to flag anomalous cases.


## **Conclusion**


ShapeScale estimates VAT with high agreement to DXA and strong repeatability from a single, non-ionizing scan. Its mesh extracted data and gradient boosting model enable DXA-comparable predictions without costly imaging or radiation exposure, making the method safe and practical across populations.


By combining the fidelity of optical 3D meshes with AI modeling, ShapeScale offers a safe, scalable solution for:


- **Clinical research:** longitudinal, radiation-free monitoring.
- **Medical spas and aesthetic practices:** objective tracking of interventions.
- **Fitness and health centers:** member engagement and progress visualization.
- **Consumer wellness:** at-home, repeatable monitoring.


ShapeScale bridges the gap between accuracy, accessibility, and safety, setting the stage for optical methods to become the new standard in body composition assessment.


## About the author


Kate Wayenberg is a data scientist specializing in applied machine learning and health technology. Her work focuses on developing and validating predictive models from complex 3D datasets, particularly in the field of body composition analysis. With expertise in statistical modeling, AI, and large-scale data collection, she bridges the gap between algorithm development and clinical application. At ShapeScale, she has contributed to building robust, data-driven methods that transform optical body scans into accurate, clinically relevant metrics.


## **References**


1. Ofenheimer A., Breznik K., Buch A., Reiter R., Föger-Samwald U., Kässmann H., Oberbauer H., Suppan M., Reiter C., Fahrleitner-Pammer A., Brix B., Pietschmann P., Winhofer-Stöckl Y., Crevenna R., Schweighofer N. Reference values of body composition parameters and visceral adipose tissue (VAT) by DXA in adults aged 18–81 years. *European Journal of Clinical Nutrition* Volume 74, Pages 1186–1197 (2020).
2. Bennett J.P., Brandner C.F., Tinsley G.M. Three-dimensional optical body shape and features improve prediction of metabolic disease risk in a diverse sample of adults. *Obesity (Silver Spring)* Volume 30, Issue 8 (2022).
3. Wong M.C., et al. Accuracy and precision of 3-dimensional optical imaging for body composition by age, BMI, and ethnicity. *The American Journal of Clinical Nutrition* Volume 118, Issue 3 (2023).
4. Kim T.N. Use of DXA for body composition in chronic disease management. *Clin Physiol Pharmacol* (2024).
5. Tavoian D., Ampomah K., Amano S., et al. Changes in DXA-derived lean mass and MRI-derived cross-sectional area of the thigh. *J Cachexia Sarcopenia Muscle* (2019).


‍
