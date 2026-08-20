---
schema_version: "1.0.0"
document_id: "39f3f7d529a2a1f8737ab9740a7840d4928e8fdaeb7461a2bef2df519398591e"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/ct-imaging-navigating-the-complex-power-needs-of-current-and-next-generation-ct-systems/"
published_at: "2023-10-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:43f4b19181c3a87f15073ec13c08e45898b0fff276e717909fd2489452bde257"
---

# CT Imaging: Navigating the Complex Power Needs of Current and Next Generation CT Systems

[Home](https://www.advancedenergy.com/en-us/) /


[About AE](https://www.advancedenergy.com/en-us/about/) /


[News & Events](https://www.advancedenergy.com/en-us/about/news/) /


[Blog](https://www.advancedenergy.com/en-us/about/news/blog/) /


CT Imaging: Navigating the Complex Power Needs of Current and Next Generation CT Systems


# CT Imaging: Navigating the Complex Power Needs of Current and Next Generation CT Systems


### Posted


October 09, 2023 by


[Dr. Simone Baer-Lang](https://www.advancedenergy.com/en-us/about/news/authors/simone-baer-lang/)


Computed Tomography (CT) is a computerized imaging technique used in radiology. A rapidly rotating X-ray beam and detector are used to generate cross-sectional images – so-called slices – that form the volumetric and very detailed internal image of the body.


The external (visible) components of a CT scanner are the gantry, which includes a control panel, setup lasers and the bore along which the patient couch will traverse during the scan. While the idea of this imaging technique is more than 100 years old, it wasn’t until the early 1970s that sufficient computing power became reality. And although CT is now a very well-established and widely used diagnostic tool, there are still a lot of physics and engineering R&D activities being undertaken to improve and extend capabilities.


Currently the state-of-the-art detectors used in CT are energy integrating detectors (EID) but it looks like that use of a new upcoming technology based on photon counting detectors (PCD) might be the next leap ahead in CT imaging from a hardware perspective. While PCDs have become well-known and used for quite some time in SPECT (single photon emission computed tomography) systems, it was not possible until recently to also deploy them in CT Imaging systems.


EIDs use an indirect conversion for the signal creation. First the detector converts the X-ray energy into light, and then this light (i.e. multiple photons) is converted into an electrical current via photodiodes on the rear side of the detector. PCDs, on the other hand, allow for the detection – and energy discrimination – of single photons. The different detector material and the different method of data acquisition allows for energy-resolved CT images with the benefits of higher spatial resolution, decreased electronic noise and a potentially reduced radiation dose.


Figure 1: Schematic of EID technology principle \[5\]


Figure 2: Schematic of PCD technology principle \[5\]


**CT Power Architecture**


Using the new detector technology means that the power architecture inside the scanner must also be updated to ensure optimized support of the system.


The overall power architecture can be divided into three different segments: the power distribution unit, the stationary part of the gantry and the rotating part of the gantry. The challenge when designing a power solution for CT systems is balancing the huge range of power needs inside the system with the need to deliver a compact form factor, high efficiency and, preferably, digital control and communications. Additionally, there is a requirement within the rotating part of the gantry for physical strength to withstand high rotating forces.


In this blog we’ll focus on the power elements for the gantry.


Figure 3: Schematic of a general CT gantry power block diagram


The power needs for the stationary part of the gantry are in the range of 600 W to 1.5 kW, depending on the potential application and overall CT performance. Usually the motion sensors, any internal gantry PC, displays, audio and switches in the gantry will need to be supplied with different power requirements, as well as the fans which consume the highest draw. Due to an output range from 5 V up to 48 V and the varying power needs, the most suitable solution would be a configurable AC-DC PSU with multiple outputs, such as an Advanced Energy Artesyn®


[iMP](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/configurable-modular-psus/imp/) ®


or


[uMP](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/configurable-modular-psus/ump/?bz-page=1&bz-results-per-page=10) series or the Excelsys


[CoolX](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/configurable-modular-psus/coolx/?bz-page=1&bz-results-per-page=10) ®


series. However, a single output PSU such as the


[LCM](https://www.advancedenergy.com/en-us/products/ac-dc-power-supply-units/enclosed-psus/lcm/?bz-page=1&bz-results-per-page=10) series can also work. The advantages of Advanced Energy’s configurable power supply are not only the flexibility offered by the modular approach, but also the long experience in using such power supplies within a CT


application and the overall ability to customize any of the above products to a specific customer need.


The most challenging element is the power supply unit for the rotating part, which will have to withstand high g-forces. The “rotating PSU” gets its input via sliprings and mainly powers the detector units - including heating / cooling for thermal stability of the detector (where applicable) and in some cases this PSU will also power the collimator. Depending on the detector technology, the number of fully separate detector elements and basic data computational system used, the total power requirements inside the rotating part could be as high as 3.5 kW. On the other hand, there is once again a limitation in the space available and, thus, the size of the PSU.


With this in mind, it might make sense to combine two or three configurable power supplies. For the correct bias voltage on the detector unit (in the case of a PCD), it would be recommended to consider Advanced Energy’s


[UltraVolt® C series](https://www.advancedenergy.com/products/high-voltage-products/high-voltage-power-supplies/standard-size-dc-to-high-voltage-modules/high-power-c-series/) of high-power regulated DC-DC converters along with the


[2M](https://www.advancedenergy.com/en-us/products/dc-dc-conversion-products/high-voltage-boost-(u-v)/microsize-(0-1w-to-6w-up-to-60kv)/m-series/) or


[2LE](https://www.advancedenergy.com/en-us/products/dc-dc-conversion-products/high-voltage-boost-(u-v)/single-o-p-(to-30w-40kv)/le-series/) series. Here, modifications or customization to meet any unique requirement would be possible, if requested. For powering the basic computational system - regardless of whether this is based on ASICs, FPGAs or a combination of both Advanced Energy’s


[LGA80D](https://www.advancedenergy.com/en-us/products/dc-dc-conversion-products/non-isolated-pcb-mount/lga-d/lga80d/) or


[LGA110D](https://www.advancedenergy.com/en-us/products/dc-dc-conversion-products/non-isolated-pcb-mount/lga-d/lga110d/) DC-DC modules could be a perfect fit. With conversion efficiency up to 96%, these modules offer some of the industry's highest efficiencies, alongside digital control functions, two independent 40 A outputs and the possibility to connect up to four units in parallel.


In addition to the power requirements mentioned above it should be noted that the patient couch also requires power. Typically, this couch will have its own PSU in the range of 300 W to 1 kW.


**Conclusion**


CT scanning is already more than 50 years old and has become one of *the* most used diagnostic modalities. However, it still offers many R&D opportunities in areas of software and hardware. In the case of the latter, power supplies are crucial components of the CT systems. These PSUs not only have to reliably deliver the right power with the required output parameters, but they must do so with high efficiencies and small form factors while being physically robust. Choosing the right power supply design, therefore, is fundamental to ensuring that CT systems provide safe, effective treatment for a range of medical applications.


With the broadest AC-DC, low-voltage and high-voltage DC-DC standard and modified products and platforms, coupled with extensive applications and compliance experience, Advanced Energy offers systems designers an unrivalled range of proven power solutions for any CT power supply design.


References:


1. A brief history of Tomography and CT, Steve Webb,


[28028148.pdf (iaea.org)](https://inis.iaea.org/collection/NCLCollectionStore/_Public/28/028/28028148.pdf#:~:text=The%20first%20practical%2C%20clinical%2C%20routine%20computed%20tomography%20%28CT%29,follows%2C%20together%20with%20a%20consideration%20of%20historical%20antecedents.)


2. Radiation Detection Systems: Sensor Materials, Systems, Technology, and Characterization Measurements (Devices, Circuits, and Systems), Iniewski, Krzystof; Iwanczyk, Jan S., DOI: 10.1201/9781003147633


3. [Photon-counting x-ray detectors for CT - IOPscience](https://iopscience.iop.org/article/10.1088/1361-6560/abc5a5)


4. Photon-counting CT: Technical Principles and Clinical Prospects. Martin J.Willemink, Mats Persson, Amir Pourmorteza, Dominik Fleischmann, Radiology, November 2018, DOI:


[https://doi.org/10.1148/radiol.2018172656](https://doi.org/10.1148/radiol.2018172656)


5. Photon-counting Detector CT: System Design and Clinical Applications of an Emerging Technology. Shuai Leng, Michael Bruesewitz, Shengzhen Tao, Jishore Rajendran, Ahmed F. Halaweish, Norbert G. Campeau, Joel G. Fletcher, Cynthia H. McCollough, RadioGraphics Vol39 No 3, May 2019, DOI:


[https://d.org/10.1148/rg.2019180115](https://doi.org/10.1148/rg.2019180115)


Share


### Dr. Simone Baer-Lang


Advanced Energy


Simone Baer-Lang is a Senior Strategic Marketing Manager, Medical Power Products at Advanced Energy dedicated for medical Imaging. She joined Advanced Energy in 2023 and has previously held a number of senior roles in product& BusinessLine management, product & technical marketing, and business development in a number of leading medical technology and Imaging companies. Simone holds a Higher Diploma and PhD in Physics from RWTH Aachen University, during those times she was doing research at DESY as well at the Forschungszentrum Jülich.


[More posts by Dr. Simone Baer-Lang](https://www.advancedenergy.com/en-us/about/news/authors/simone-baer-lang/)


Browse


[Categories A-Z](https://www.advancedenergy.com/en-us/about/news/categories/)


Latest from AE


[News & Press Releases](https://www.advancedenergy.com/en-us/about/news/press/)[Videos](https://www.advancedenergy.com/en-us/about/news/videos/)[Trade Shows & Conferences](https://www.advancedenergy.com/en-us/about/news/events/)[Webinars](https://www.advancedenergy.com/en-us/about/news/webinars/)


Join Our Mailing List


Subscribe


Recent Posts


[View on X](https://twitter.com/advenergy)
