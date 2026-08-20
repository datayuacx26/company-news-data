---
schema_version: "1.0.0"
document_id: "c5fed112fefce5b35e06ab919165fdde2d5dfef17d111c221aa5cb8014934dad"
company_key: "yc-morphle-labs"
company: "Morphle Labs"
source_id: "yc-morphle-labs-news-import-7623d76337f3"
canonical_url: "https://www.morphlelabs.com/knowledge/digital-pathology-slide-scanner/post/whole-slide-imaging-system-glossary-of-must-know-terms"
published_at: null
first_seen_at: "2026-07-27T03:52:47.126712+00:00"
fetched_at: "2026-07-28T21:16:46.713740+00:00"
content_hash: "sha256:c9517de9406c219f180ace150b0eb9ab17c84bdc83c95bb8c2370187261aa7f4"
---

# Whole Slide Imaging System: Glossary of Must‑Know Terms

# Whole Slide Imaging System: Glossary of Must‑Know Term


‍


## **TL;DR**


Digital pathology is transforming how laboratories diagnose disease; but the field comes with a dense vocabulary that can slow adoption. This glossary decodes the essential terms behind Whole Slide Imaging (WSI), scanner hardware, image analysis, and compliance, so your team speaks the same language from day one.


### **What You'll Learn**


By the end of this glossary you will be able to:


- Define core WSI and digital pathology scanner concepts with confidence
- Understand the technical specifications that differentiate scanners
- Evaluate workflow, compliance, and interoperability requirements
- Ask the right questions when comparing digital pathology companies and solutions


### **1. Foundational Concepts**


#### **Whole Slide Imaging (WSI)**


Whole Slide Imaging is the process of digitizing an entire glass histology slide at high resolution to create a single large digital file, the Whole Slide Image. A WSI system combines a high‑throughput digital pathology scanner, motorized stage, objective lens array, and image‑stitching software to capture thousands of overlapping tiles and assemble them into a seamless, zoomable image. The resulting file allows pathologists to pan, zoom, annotate, and share slides without any physical handling.


#### **Digital Pathology Scanner**


A digital pathology scanner, sometimes called a whole slide scanner or automated microscope slide scanner; is the hardware instrument at the heart of any WSI workflow. It accepts glass slides stained with dyes (H&E, IHC, FISH, and others), moves them under a high‑magnification objective, captures overlapping image fields, and stitches them into a single whole slide image. Modern instruments from leading digital pathology companies support throughputs ranging from a few slides per hour to hundreds per day, covering routine anatomic pathology, research, and pharmaceutical applications.


#### **Histology Scanner / Slide Scanner Histology**


The terms histology scanner and slide scanner histology are used interchangeably with digital pathology scanner in tissue‑based laboratory settings. They specifically describe instruments optimized for bright‑field imaging of stained tissue sections, the primary consumable in surgical pathology and histopathology workflows.


### **2. Technical Specifications — Terms You Must Know**


#### **Spatial Resolution & Objective Magnification**


Resolution in WSI is expressed as microns per pixel (μm/px) rather than simple magnification. A 40× scan typically yields ~0.25 μm/px, sufficient for fine nuclear detail. A 20× scan (~0.50 μm/px) covers routine diagnostic work at roughly half the file size. Always verify both the objective magnification and the resulting pixel pitch, two scanners marketed as "40×" can differ significantly in effective resolution.


#### **Z‑Stack / Focus Depth**


Z‑stacking refers to capturing multiple focal planes at defined depth intervals (e.g., 1 μm steps). It is essential for cytology samples, thick sections, or slides with debris. Each z‑level multiplies storage demands proportionally, so only request z‑stacks when clinically required.


#### **Brightfield vs. Fluorescence Scanning**


Brightfield scanning captures transmitted white‑light images of conventionally stained slides (H&E, Masson's trichrome). Fluorescence scanning uses specific excitation and emission filters to image fluorescent labels, critical for FISH, immunofluorescence, and multiplexed biomarker panels. Many digital pathology scanner platforms now support both modalities within a single chassis.


#### **File Format & Compression**


Common WSI formats include SVS (Aperio), NDPI (Hamamatsu), MRXS (3DHISTECH), and the open TIFF‑based formats. Compression choices, lossless (LZW), near‑lossless (JPEG 2000), or lossy (JPEG); directly affect file size, image fidelity, and downstream AI analysis accuracy. Interoperability hinges on support for open standards such as DICOM WSI and openslide‑compatible formats.


#### **Scanning Speed & Throughput**


Scanning speed is expressed as minutes per slide at a defined magnification and tissue area. A high‑throughput automated microscope slide scanner capable of scanning a 15 × 15 mm tissue section at 40× in under 90 seconds is considered fast by current benchmarks. Throughput-slides per hour or per day, determines whether a system fits low‑volume research labs or high‑volume clinical production environments.


### **3. Workflow & Integration Terms**


#### **LIS & LIMS Integration**


A Laboratory Information System (LIS) manages patient data and orders in clinical settings; a Laboratory Information Management System (LIMS) serves research labs. Seamless bidirectional integration between the digital pathology scanner and the LIS/LIMS ensures that each scanned image is automatically tagged with the correct accession number, patient ID, and stain type, eliminating manual transcription errors.


#### **Image Management System (IMS) / Digital Pathology Platform**


An IMS, sometimes called a digital pathology platform or viewer, is the software layer that stores, indexes, serves, and displays whole slide images. Key capabilities include multi‑user concurrent viewing, annotation tools, case management, and API hooks for AI algorithm plug‑ins. Cloud‑hosted IMS solutions remove the need for on‑premises server infrastructure and enable telepathology at scale.


#### **Telepathology**


Telepathology is the real‑time or store‑and‑forward transmission of digital slide images for remote diagnosis or consultation. Static telepathology uses pre‑captured WSI files; dynamic telepathology streams live microscope video. Both modalities depend on the underlying quality of the digital pathology scanner to ensure diagnostic‑grade image fidelity at the receiving end.


#### **Image Analysis & AI Algorithms**


Image analysis software extracts quantitative data from WSI, cell counts, staining intensity, tumor area, mitotic index, and more. AI‑driven algorithms (deep learning, convolutional neural networks) can triage slides, flag regions of interest, and provide decision support. These algorithms run on top of the image data produced by the digital pathology scanner, making scanner image quality a direct determinant of AI accuracy.


#### **4. Benefits vs. Limitations at a Glance**


Digital pathology scanner adoption delivers clear and measurable advantages: it eliminates physical slide handling and shipping logistics, enables multiple pathologists to access the same case concurrently from any location, and creates a permanent lossless digital archive that never fades, breaks, or gets lost. AI‑assisted analysis and quantitative biomarker scoring become possible only once slides exist in digital form, making the scanner the foundational investment for any data‑driven pathology program.


That said, institutions must go in clear-eyed about the trade-offs. Premium scanner platforms carry a significant upfront digital pathology scanner price, and large WSI file sizes demand robust storage infrastructure and high-bandwidth networks. Workflow redesign and staff retraining are non-negotiable investments, not optional add-ons. Regulatory clearance pathways also vary considerably by country and intended use, a system validated for research may not be cleared for clinical primary diagnosis without additional steps.


The bottom line: the benefits decisively outweigh the limitations for any lab serious about scale, quality, and future readiness, but success depends on planning for the full cost of deployment, not just the hardware.


‍


### **5. Compliance & Standards**


#### **DICOM WSI**


DICOM (Digital Imaging and Communications in Medicine) is the universal healthcare imaging standard. The DICOM WSI supplement extends the standard to whole slide images, enabling seamless integration with hospital PACS and RIS infrastructure. Vendors that export natively to DICOM WSI significantly reduce interoperability friction in multi‑vendor environments.


#### **GDPR & HIPAA**


Patient slide images are protected health information (PHI). Digital pathology deployments must comply with HIPAA in the United States and GDPR in Europe, governing data encryption at rest and in transit, access logging, de‑identification procedures, and breach notification requirements. Cloud‑based image management platforms must provide Business Associate Agreements (BAAs) and data residency guarantees.


### **6. Key Applications**


Understanding the terminology unlocks the full range of use cases where WSI systems deliver value:


- **Surgical Pathology & Frozen Sections** — rapid intraoperative consultation via telepathology
- **Oncology Biomarker Quantification** — automated scoring of PD‑L1, Ki‑67, HER2 IHC
- **Pharmaceutical Drug Development** — high‑content tissue phenotyping in preclinical studies
- **Medical Education & Competency Assessment** — virtual slide libraries replacing glass collections
- **Second‑Opinion Consultation Networks** — expert pathologist review across geographies
- **AI Model Training & Validation** — curated annotated datasets for deep learning pipelines


### **7. Buying Guide — Key Evaluation Checklist**


When comparing digital pathology scanner options, evaluate each vendor against these criteria:


- **Resolution & Magnification:** Does it support 20×, 40×, and optional 60× or 100× oil‑immersion scanning?
- **Throughput:** How many slides per hour at your primary use‑case magnification?
- **Slide Capacity:** Single‑slide manual loader vs. 6‑ to 400‑slide automated cassette?
- **Modalities:** Brightfield only, or multi‑modal (fluorescence, polarization, darkfield)?
- **Digital Pathology Scanner Price vs. TCO:** Consider reagent costs, service contracts, and storage infrastructure.
- **Software Ecosystem:** Open API for third‑party AI apps, or closed proprietary platform?
- **DICOM & LIS Integration:** Native export and bidirectional HL7/FHIR connectivity?
- **Whole Slide Scanning Speed:** Benchmark on your actual tissue types and stain portfolio.
- **Vendor Support & Training:** Onsite installation, remote diagnostics, SLA commitments?


### **8. Future Trends**


The vocabulary of digital pathology is still expanding. Watch for these emerging terms:


- **Computational Pathology** — the convergence of WSI, multi‑omics data, and machine learning for integrated disease profiling
- **Multiplexed Imaging (mIHC/IMC)** — simultaneous visualization of 20–40 biomarkers on a single tissue section
- **Spatial Transcriptomics Integration** — overlaying gene‑expression data onto WSI spatial coordinates
- **On‑Device AI Inference** — edge computing built directly into the digital pathology scanner chassis for real‑time analysis without cloud dependency
- **Federated Learning** — training AI models across distributed hospital datasets without sharing raw patient images


### **Why Morphle Labs Stands Apart**


Morphle Labs has engineered a digital pathology scanner purpose‑built for the demands of modern pathology, combining ultra‑high‑resolution brightfield and fluorescence scanning, a compact footprint designed for space‑constrained labs, and an open software architecture that integrates with leading AI analysis platforms. Unlike legacy vendors locked into proprietary ecosystems, Morphle delivers native DICOM WSI export, LIS/LIMS connectivity, and transparent pricing models that make whole slide scanning accessible to institutions of every size, from single‑site hospitals to multi‑center research networks. As one of the most agile digital pathology companies in the market today, Morphle pairs cutting‑edge hardware with an unmatched commitment to customer support, compliance, and continuous software innovation.


### **Ready to Modernize Your Pathology Lab?**


See how Morphle Labs' digital pathology scanner fits your workflow — request a live demo, get a tailored quote, or download our technical datasheet today.


**→ Visit**[www.morphlelabs.com](http://www.morphle.com/) **|**[Contact our team](https://morphlelabs.com/about/contact) **|**[Request a Demo](https://morphlelabs.com/demo)


‍
