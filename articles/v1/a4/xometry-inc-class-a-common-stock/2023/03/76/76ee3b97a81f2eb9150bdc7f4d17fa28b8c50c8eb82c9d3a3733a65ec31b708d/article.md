---
schema_version: "1.0.0"
document_id: "76ee3b97a81f2eb9150bdc7f4d17fa28b8c50c8eb82c9d3a3733a65ec31b708d"
company_key: "xometry-inc-class-a-common-stock"
company: "Xometry Inc."
source_id: "xometry-inc-class-a-common-stock-rss-9e4e2af01343"
canonical_url: "https://www.xometry.com/resources/3d-printing/compression-set/"
published_at: "2023-03-30T04:00:13+00:00"
first_seen_at: "2026-07-20T23:17:15.190216+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:67ed724fdefca8ee47deac692aa209b29c44a8ae6f32e0c9512bda4baad9b1a7"
---

# Compression Set: Meaning, How It Works, and Understanding the Graph

## How to Understand the Compression Set Graph?


To understand the compression set graph, follow the five steps. First, identify the y-axis as compression set percentage (%) and interpret lower values as better elastic recovery after the test. Second, identify the x-axis as aging time (22 hours, 70 hours, 168 hours) or aging temperature (70°C, 100°C, 150°C) and treat higher values as more severe test exposure. Third, confirm the test condition details (ASTM D395, 25% deflection, 30 minutes recovery) because different deflection levels change the baseline values. Fourth, compare material curves by reading the percent change at the same time and temperature, then rank the materials by the lowest compression set at the target service condition. Lastly, interpret steep upward slopes as faster permanent deformation growth, and interpret flatter curves as better long-term sealing force retention.


## What is Compression Deflection?


A compression deflection is the thickness reduction applied during a compression test and is reported as a percentage of the starting thickness. Compression deflection sets the strain level in ASTM D395 compression set testing, with 25% and 40% deflection used for many elastomer evaluations. Compression deflection differs from compression set because deflection describes the imposed squeeze during loading, while compression set describes the unrecovered thickness after unloading and a defined recovery period. Larger deflection levels place more strain on the elastomer network and commonly lead to higher measured compression set under identical aging conditions (22 hours, 70 °C to 150 °C, 30 minutes recovery). Compression deflection is used to represent the installed squeeze in gaskets, O-rings, pads, and vibration isolators. Designers choose deflection targets to match assembly squeeze, while compression set quantifies rebound loss after aging.


**How does Deflection Different from Compression set?** Deflection is different from compression set because deflection is the applied thickness reduction during loading, while compression set is the permanent thickness loss remaining after unloading and recovery. Deflection acts as the controlled input that defines the squeeze level (25% or 40%) during ASTM D395 aging. The compression set acts as the measured output reported after recovery (30 minutes). Higher deflection raises internal strain and tends to increase compression set under the same time and temperature.


### How is Compression Deflection Measured?


To measure compression deflection, follow the five steps. First, measure the initial specimen thickness using a calibrated thickness gauge with consistent contact force. Second, select the target deflection percentage (25% or 40%) and calculate the required compressed thickness from the initial thickness. Third, place the specimen in a compression fixture with flat, parallel platens and apply compression until the calculated thickness is reached. Fourth, verify the compressed thickness using gauge blocks, spacers, or fixture stops that lock the deflection level during the test. Lastly, record the deflection percentage, fixture type, specimen thickness, and the referenced standard (ASTM D395) to ensure repeatable compression deflection control.


## How Does the Compression Set Relate to 3D Printing?


The compression set relates to 3D printing because flexible printed polymers (TPU, Thermoplastic Elastomer (TPE), elastomer-like resins) lose thickness recovery under sustained load, which affects seals, pads, and damping features. Printed parts show different behavior than molded rubber because layer interfaces and porosity change stress distribution.


Compression set rises in printed elastomers when print settings create voids, low interlayer bonding, or uneven infill. A printed gasket that relaxes loses sealing force, which increases leakage risk. Material choice matters because TPU grades vary in hardness and rebound, and resin elastomers creep under heat. Designers reduce compression set risk by increasing wall thickness (2 mm to 4 mm), selecting higher quality flexible filaments, and avoiding service temperatures above 60°C to 80°C for TPU grades related to[compression set to 3D printing](https://www.xometry.com/resources/3d-printing/) .


## What is the Importance of Compression Set Testing?


The importance of compression set testing is the process of quantifying permanent deformation after controlled compression, which predicts long-term sealing and cushioning performance. The test matters because elastomer failures occur gradually through loss of rebound rather than sudden fracture. Compression set data supports material selection for O-rings, gaskets, vibration mounts, and medical seals. Testing identifies compounds that resist heat aging, oil exposure, and sustained load. Manufacturers use test results to validate curing chemistry, filler loading, and batch consistency. Designers use the data to reduce warranty risk by choosing materials that retain contact pressure. Testing becomes relevant for parts exposed to heat, where relaxation accelerates.


## What is ASTM D395?


ASTM D395 is a standard test method for measuring rubber compression set, which quantifies permanent thickness loss after an elastomer specimen is compressed for a defined period, thermally aged, unloaded, and given a controlled recovery interval. The standard specifies specimen types (Type 1 and Type 2), fixture configurations, and reporting requirements for Method A (constant force) and Method B (constant deflection) compression set testing. Many common test setups use deflection levels around 25% or 40%, exposure times like 22 hours or 70 hours, and oven temperatures that range from about 70 °C up to 150 °C, depending on the material specification. The procedure records initial thickness, applies controlled deflection using spacer control or fixture stops, maintains compression during aging, releases the load, and measures recovered thickness after the defined recovery period. The compression set percentage is calculated from the unrecovered thickness change relative to the applied deflection. ASTM D395 supports elastomer comparison for sealing performance, rebound retention, and long-term stress relaxation behavior.


### How is ASTM D395 Conducted?


To conduct ASTM D395, follow the six steps. First, prepare a rubber specimen with a standardized geometry and record the initial thickness using a calibrated thickness gauge. Second, select the test method (Method A, constant force or Method B, constant deflection) and set the target compression deflection (25% or 40%). Third, place the specimen in the compression fixture between flat, parallel platens and apply compression using spacers or fixture stops to lock the deflection. Fourth, age the compressed specimen for a defined duration (22 hours or 70 hours) at a defined temperature (70°C, 100°C, 125°C, or 150°C). Fifth, remove the specimen from the fixture while hot, release the load, and allow recovery for a defined time (30 minutes) at standard laboratory temperature (23°C). Lastly, measure the final thickness and calculate the compression set percentage from the unrecovered thickness change relative to the applied deflection.


#### Is ASTM D395 Important for Material Selection?


Yes, ASTM D395 is important for material selection because compression set performance directly affects long-term sealing load and elastic recovery in service. ASTM D395 provides a consistent compression set test method for vulcanized rubber and rubber-like materials, including many elastomer families used in seals and gaskets. The standard defines fixture methods, deflection levels (commonly 25% or 40%), thermal exposure duration (22 hours or 70 hours in many specifications), exposure temperature ranges, and recovery timing, which supports meaningful compound comparison. A material measured at 10% compression set at 100 °C after 22 hours retains more recoverable thickness than a material measured at 40% under the same conditions. The results help estimate sealing force loss in gaskets and O-rings and thickness loss in pads under sustained compression. ASTM D395 data supports material specifications, supplier qualification, and reduced failure risk in compression-loaded elastomer applications.


## How Do They Measure the Compression Set of a Material?


They measure the compression set of a material by recording the initial thickness, compressing the specimen to a defined deflection, holding the deflection during thermal aging for a defined time and temperature, releasing the load, allowing a defined recovery interval, and calculating the percent of unrecovered thickness relative to the applied deflection. ASTM D395 is a common standard for rubber and elastomer compression set testing and defines Method A and Method B fixtures and reporting requirements. Sample preparation uses a die-cut specimen or molded button with flat, parallel faces to reduce thickness variation and improve repeatability. The test uses a compression set fixture with rigid platens and spacer control to maintain target deflection levels, with common deflection settings at 25% or 40%, depending on the method and material. Aging is performed in a controlled temperature oven, with common conditions including 70 °C to 150 °C for 22 hours, and some specifications extending to longer durations. Recovery is measured after a defined time at standard laboratory temperature, and the final compression set percentage is calculated from the thickness loss after recovery.


## What is a Compression Tester?


A compression tester is a measurement device that applies a controlled compressive load or controlled deflection to a material specimen to evaluate thickness change, force response, and permanent deformation after recovery. A compression tester setup uses flat, parallel platens, a calibrated force or displacement control system, and a thickness gauge or displacement sensor with a resolution of 0.01 mm. The operation starts by measuring initial specimen thickness, applying a defined deflection (25% or 40%) or a defined force, holding the condition for a set duration (22 hours or 70 hours) at a controlled temperature (70°C to 150°C), then measuring final thickness after a defined recovery time (30 minutes). Accurate results require platen alignment within 0.05 mm, lubricated contact surfaces, and repeatable specimen placement to prevent edge loading. Compression testers support ASTM D395 compression set testing and compression deflection force testing for rubber, silicone, EPDM, FKM, and TPU.
