---
schema_version: "1.0.0"
document_id: "a2dde6db69e5c6256b55d9289317c9f9f0db41d2b60b599507735faddf751a6b"
company_key: "yc-morphle-labs"
company: "Morphle Labs"
source_id: "yc-morphle-labs-news-import-7623d76337f3"
canonical_url: "https://www.morphlelabs.com/knowledge/digital-pathology-slide-scanner/post/sustained-vs-max-rated-throughput-in-digital-pathology-scanner"
published_at: null
first_seen_at: "2026-07-27T03:52:47.126712+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:0fc2d35bbaebbb5b5a48ff7c37558b22c50d4d1389e9daa5f3caf20096238890"
---

# Sustained vs. Max Rated Throughput in Digital Pathology Scanner

# **Sustained Throughput vs. Max Rated Throughput**


#### **Why the number on the spec sheet almost never reflects what happens in your lab — and how Morphle is engineered differently.**


Every whole-slide scanner vendor publishes a throughput number. That number is almost always a peak, achieved under ideal, carefully staged lab conditions that look nothing like your real-world environment. MorphoLens 400 is built around a different metric: what the scanner actually delivers, hour after hour, without human babysitting.


## **The Gap Nobody Talks About**


A scanner rated at 300 slides per day sounds impressive. But what does that figure actually account for?


In most cases, raw image capture speed on pre-focused, pre-loaded slides is measured in isolation. It does not account for the time a technician spends loading individual slides into proprietary racks, waiting for the previous batch to finish before inserting the next, or the lag between scan completion and image availability while files upload to a remote server.


The result is a lab that runs at 60–70% of its theoretical capacity on a good day, and considerably less when staff is stretched thin.


*The gap between max rated and sustained throughput is not a minor rounding error; it is a structural workflow problem.*


### **Max Rated Throughput**


The best-case number.


Measured under vendor-staged conditions. Assumes pre-loaded slides, immediate staff availability, and no idle time between batches. Rarely reproducible in a busy surgical pathology lab.


### **Sustained Throughput**


What your lab actually gets.


Factors in the complete electro-mechanical cycle, barcode reading, tissue pre-mapping, autofocusing, stitching, across a full continuous shift. This is the number that determines real turnaround time.


## **The Complete Electro-Mechanical Cycle**


Sustained throughput is an honest accounting of everything that must happen for a slide to go from the stainer to a pathologist's screen.


For Morphle, that cycle includes:


- Barcode reading — each slide is identified automatically at intake, eliminating manual logging and transcription errors.
- Automated tissue boundary pre-mapping — the scanner identifies the tissue region before committing to a full scan, skipping blank glass and reducing unnecessary imaging time.
- Real-time autofocusing — focus is continuously adjusted across the tissue surface, not sampled at a handful of fixed points, ensuring sharp imagery even on uneven or thick sections.
- High-fidelity image stitching — individual tiles are assembled into a seamless whole-slide image without visible seam artifacts, enabling reliable primary diagnostic use.


### **Zero Upload Wait Time**


Because Morphle runs on an on-premises server, the completed scan is instantaneously available for remote viewing.


There is no upload step, no cloud transfer queue, and no wait between scan completion and case availability, eliminating one of the most significant hidden throughput drains in cloud-dependent systems.


## **Direct "No-Touch" Loading**


One of the most consequential design decisions in a high-volume scanner is how slides enter the system.


Most scanners require a technician to handle individual glass slides and load them into proprietary trays—a slow, interruption-prone process that becomes a bottleneck as volume grows.


MorphoLens 400 accepts stainer cassettes directly.


Technicians feed fresh slide racks into the system continuously while it is running, no pauses, no batch boundaries, no manual handoffs.


This is what we call Direct No-Touch Loading, and it is the single biggest driver of sustained throughput advantage.


## **Built for High-Volume Surgical Pathology**


MorphoLens 400 is purpose-built for the operating conditions of a high-throughput surgical pathology laboratory, not a research core facility, not an academic reference center processing 20 cases a day.


The scanner handles standard tissue biopsy slides stained with H&E and IHC, the workhorse formats of clinical pathology, at the volume those labs actually produce.


Continuous batch processing means technicians do not pause workflow to reload.


Fresh slide racks feed into the system while it is running, without interrupting scan cycles.


This matters enormously during morning rushes and end-of-day surges, the moments where conventional scanners accumulate queue backlogs that stretch turnaround times.


### **Key Metrics**


- Hundreds of pathology labs use Morphle for primary diagnostic sign-out.
- Zero upload wait time due to on-prem server architecture.
- Native DICOM output with no conversion layer.


## **Native DICOM — Not an Afterthought**


Morphle natively outputs true DICOM format files.


This is not a conversion layer bolted on for hospital IT compliance; DICOM is the native output.


It integrates directly into existing radiology and pathology IT infrastructure without proprietary middleware, enabling straightforward LIS and PACS connectivity.


For labs evaluating digital pathology against long-term infrastructure investments, native DICOM eliminates a category of integration risk entirely.


### **Primary Diagnostic Sign-Out — Backed by Evidence**


Morphle is validated for primary diagnostic sign-out, not merely review or consultation.


Hundreds of pathology labs use it as their front-line diagnostic tool, supported by ongoing quality metrics across routine H&E and IHC staining protocols.


## **The Factory Model — Morphle + Robotome**


The scanning step does not exist in isolation.


Upstream, tissue must be sectioned and mounted.


Morphle's integration with Robotome, a robotic microtome from Morphle Labs, extends the no-touch, continuous-processing philosophy back to the cutting stage.


Together, they are engineered to run a histology lab like a factory: consistent, auditable, and freed from the throughput ceilings imposed by manual human steps at each stage.


"A scanner is only as fast as the slowest step a technician has to perform. Remove those steps, and throughput becomes a function of the machine, not the shift."


## **Ergonomics and Handling**


High-volume design cannot ignore the humans running the system.


Low-touch operation reduces repetitive motion exposure for laboratory technicians, decreases the probability of slide dropping or mishandling, and allows one operator to oversee significantly higher slide volume than conventional systems require.


This is not an ergonomic footnote, in a 500-slide-per-day lab, reduced handling translates directly into lower error rates and more sustainable staffing models.


## **What to Ask Your Vendor**


When evaluating digital pathology scanners against a throughput number, the right questions are not about the peak figure, they are about what conditions produced it:


- Does the throughput figure include barcode reading, autofocus, and stitching, or only image capture?
- Does it assume slides are pre-loaded and pre-staged, or does it reflect the loading cycle your technicians will perform?
- How long between scan completion and image availability for remote sign-out?
- Can the system continue scanning while new racks are being loaded, or does it require a pause?
- Is DICOM output native, or is it a post-scan conversion?


### **The Bottom Line**


Throughput is a lab outcome, not a scanner specification.


The scanner that produces the most slides per hour under benchmark conditions is not necessarily the scanner that produces the most diagnoses per shift in your lab.


**Morpholens Designed for sustained throughput in enterprise-grade histology labs.**


‍
