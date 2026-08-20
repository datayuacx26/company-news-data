---
schema_version: "1.0.0"
document_id: "1db321ecd695254f0dcb2f3847ed5cce1fa082f34fe55804df550fc69ff97a91"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/medical-imaging-trends-impact-on-advanced-power-technologies-%E2%80%93-part-1-ultrasound-and-x-ray/"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T22:13:05.421454+00:00"
content_hash: "sha256:c3c1b2c831cebeb0c7cc7b02d00acfa25dae714336008ed3935bcfe00a0b31c4"
---

# Medical Imaging Trends Impact on Advanced Power Technologies – Part 1: Ultrasound and X-Ray

[Home](https://www.advancedenergy.com/en-us/) /


[About AE](https://www.advancedenergy.com/en-us/about/) /


[News & Events](https://www.advancedenergy.com/en-us/about/news/) /


[Blog](https://www.advancedenergy.com/en-us/about/news/blog/) /


Medical Imaging Trends Impact on Advanced Power Technologies – Part 1: Ultrasound and X-Ray


# Medical Imaging Trends Impact on Advanced Power Technologies – Part 1: Ultrasound and X-Ray


### Posted


May 18, 2026 by


[Todd Huston](https://www.advancedenergy.com/en-us/about/news/authors/todd-huston/)


Blog Summary


- Growing demand and expanding role of imaging:


Aging populations and better screening are increasing use of imaging technologies (ultrasound, X‑ray), which are now used across the full care cycle, from early detection to guiding minimally invasive treatments.


- Smarter, portable systems drive higher power needs:


Advances like AI, 3D/4D imaging, and mobility (portable/battery-powered devices) improve speed, accuracy, and access, but significantly raise power consumption, complexity, and efficiency requirements.


- Evolving power design requirements


: Imaging systems increasingly need flexible, modular, high-efficiency power architectures to handle higher loads, multiple voltage rails, cooling constraints, and specialized components (e.g., detectors, batteries), especially as miniaturization and portability expand.


---


With people living longer overall, diagnoses of cancer, cardiovascular disease, and other aging-related diseases are increasing. Advancements in medical imaging technologies further enable more widespread screenings, prevention and treatment of various medical conditions.


Medical imaging encompasses technologies such as ultrasound, X-ray, computed tomography (CT), magnetic resonance imaging (MRI), positron emission tomography (PET) or single-photon emission computed tomography (SPECT). Beyond diagnoses, these systems are increasingly also used throughout the treatment cycle from early detection to supporting minimally invasive electrosurgery that shortens recovery times. As imaging technology changes and advances, the requirements for reliable and safe power delivery in imaging equipment are also evolving.


This first part of a two-blog series examines the trends that impact advanced power technologies in the most-commonly used imaging systems: ultrasound and X-ray. Part two will focus on big-iron systems: MRI, CT and PET/SPECT scanners.


Ultrasound


Ultrasound uses reflected sound waves to create its images and is used for a wide range of evaluations ranging from maternity, through sports injuries to cardiovascular checks.


There is a drive to implement more sophisticated techniques, especially the use of 3D- and 4D-imaging and embedded artificial intelligence (AI) machine learning models, to improve visual clarity and reduce the amount of time needed for evaluation. These techniques help improve diagnosis accuracy and speed, reduce the need for (and cost of) specialists to undertake these scans, and allow more patients to be scanned.


The other key trend is portability, with the ability to move the machine (rather than the patient), which allows for easier diagnoses and improves patient outcomes.


All these trends have a significant impact on power delivery.


Looking first at AI, although its introduction is software-driven, there is a clear impact on power requirements to supply the additional computational power needed. While dedicated low-power AI processors can be used, even with these, the implementation of AI translates into higher peak power consumption. Similarly, the move to 3D- and 4D-imaging demands greater data throughput, which also increases the power draw. As a result, a typical ultrasound system can consume 1 kW and above.


Within the power architecture, this increased draw can require additional cooling measures and a re-think of the power infrastructure.


These requirements also increase power system complexity, often needing multiple voltage rails. In this context, modular or configurable power solutions are critical, as they provide the needed flexibility without requiring re-certification when the requirements change.


Portability places strict constraints on the power supply, which must be compact, lightweight, and potentially battery powered. High power conversion efficiency is essential to enable extended clinical use while supporting operation from battery voltages and integrated charging.


A conduction cooled PSU is preferred over a fan-cooled PSU, as it allows for the system to be sealed for a high IPXX rating. An IPXX rating defines the level of protection an enclosure provides against the ingress of solid objects and liquids, with higher numbers indicating greater resistance. Power in these cases should also be generally lower (~600 W) and take the form of a single output PSU, with an auxiliary rail deployed and battery charging or management integrated.


X-Ray


While ultrasound (and its lack of radiation) is the preferred method for many applications such as pregnancy, soft tissue evaluation, and cardiovascular imaging, the most used imaging technology in a hospital setting remains the X-ray, which accounts for over 35% of all imaging equipment deployed. It is also the most diverse, ranging from mobile X-ray units used in intensive care, general diagnostic X‑ray rooms, mobile surgical C-arms, various types of fluoroscopic units, mammography and dental systems to large and highly sophisticated cardiac or vascular angiography systems.


In addition, X-ray systems are rapidly moving toward AI-driven workflows with image enhancement. In fact, AI is increasingly being integrated into radiographic workflows, assisting in optimizing image acquisition, automating patient positioning, and enhancing image quality through post-processing techniques such as denoising and contrast enhancement.


Like ultrasounds, the computational capabilities required for the necessary AI algorithms demands additional power. On the other hand, the increased accuracy from AI analysis reduces the need for repeat scans and thereby lowers overall energy consumption.


Although not applicable to all X-ray systems, there are other trends that will also impact power needs. Portability for point-of-care use is becoming increasingly important, especially for emergency and rural settings, but also for bedside imaging. This is driving a growing need for, and adoption of, mobile and battery-powered X-ray units. As with ultrasounds, this may include battery operation, which requires systems to be designed for energy efficiency, often incorporating digital detectors and low-power generators.


Recent advances in sensors and software have also led to image quality improvements. Resolution gains attributed to detector improvements require increased power delivery. Detectors are complex subsystems, with indirect detectors typically containing the power components, making miniaturization necessary. Modern indirect detectors commonly use 19 V DC, which requires a custom rail or, more commonly, an adjustable or configurable PSU.


Direct detectors, on the other hand, convert received X-rays into an electric charge. The bias voltage of this process will be dictated by the detector crystal’s size and material, typically ranging from 400 to 1,000 V, and powered from a 12 or 24 VDC input. This tends to require a specialized power solution.


Another trend in X-ray is to reduce the exposure of the radiographer and patient to radiation. One potential method to achieve this is the use of carbon nano tubes (i.e. cold cathode emitters) for X-ray generation. This is in the early experimental phase and significant research is taking place, including the power architecture where further adaption of power systems will be necessary.


Part Two


In part two of this blog, AE will discuss the big-iron systems of MRI, CT and PET/SPECT scanners and the innovation trends that these systems are having on power system design.


Advanced Energy offers the broadest portfolio of power solutions including AC-DC front ends as well as low- and high-voltage modules. This is coupled with extensive applications and compliance experience in the medical imaging field, ensuring the right solution for your design. For more information on AE’s medical power supplies, visit:[Medical Grade Power Supplies for OEM Devices | Advanced Energy.](https://www.advancedenergy.com/en-us/applications/medical/)


Share


### Todd Huston


Advanced Energy


As Advanced Energy’s Director of Strategic Marketing for Electrosurgery, Todd Huston develops strategic marketing plans for the company’s broad medical power portfolio of standard and configurable products to power the future of medicine. He is a senior technology and marketing professional with deep knowledge of the global healthcare industry as well as a passionate strategist with a proven track record leading teams in commercializing disruptive technologies at two Fortune 500 Companies. Todd previously served as an electrosurgery product manager for the Tumor Ablation portfolio at a leading medical device company.


[More posts by Todd Huston](https://www.advancedenergy.com/en-us/about/news/authors/todd-huston/)


Browse


[Categories A-Z](https://www.advancedenergy.com/en-us/about/news/categories/)


Latest from AE


[News & Press Releases](https://www.advancedenergy.com/en-us/about/news/press/)[Videos](https://www.advancedenergy.com/en-us/about/news/videos/)[Trade Shows & Conferences](https://www.advancedenergy.com/en-us/about/news/events/)[Webinars](https://www.advancedenergy.com/en-us/about/news/webinars/)


Join Our Mailing List


Subscribe


Recent Posts


[View on X](https://twitter.com/advenergy)
