---
schema_version: "1.0.0"
document_id: "b5d60e5bdf5eeaf205fdd03a5c899d952c7dc14915b3b45212a9ece8c29e9974"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/ps-cal-software-calculations-explained/"
published_at: "2023-02-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:c3f12bb32c62ecb512373763aeeb7d2a07f7c22826953625df188373eae43d13"
---

# PS-CAL Software Calculations Explained

### Automated Calibration of RF Power Sensors: PS-Cal Calibration Factor, Mismatch, and Uncertainty Calculations Explained


The PS-CAL software solution automates calibration of RF Power Sensors to a high degree of accuracy. It executes the following three complex mathematical calculations: The Calibration Factor, The Mismatch Factor, and Uncertainty. This post discloses the calculations within the software so metrologists and engineers can validate the system, document the calibration process, and report results.


This solution is particularly useful for test engineers and/or metrologists with calibration responsibilities for sensitive equipment such as RF power sensors. In many cases, the equipment is utilized by third party calibration organizations where metrologists must often provide documentation for the calibration results they deliver. Likewise, metrologists at military facilities will be able to document and validate their calibration results.


PS-CAL delivers multiple advantages. It automates the calibration process with the capabilities to control the equipment during the calibration process, make the necessary measurements, and perform the complex calculations. It then generates a calibration report. A unique feature of PS-CAL is the flexibility to calibrate sensors from many manufacturers, reducing the need for dedicated proprietary calibration systems.


1. The Calibration Formulas


- Calculating the Power Sensor Calibration Factor
- Determining the RF Power Level


1. Apply power from an RF power source/generator (Pgzo) into an RF power sensor.
2. The power from the source is equal to the power measured by the power sensor (Pm) divided by the power sensor correction or calibration factor (kF). This assumes that the impedances between the two devices match, which results in no reflection (mismatch) factor.


- Determining the Power Sensor Calibration Factor


1. To calculate the formula above, one must first determine the calibration factor (kF) of the power sensor. To do so, divide the measured power reading from the power sensor (Pm) by the known accurate power from the power source (Pgzo), as follows. This calculation still assumes a perfect impedance match.


2. In this power sensor calibration system, the RF source (generator) is monitored by an AE-TEGAM feedthrough power standard to provide an accurately known RF power level. This feedthrough standard will be considered as the RF power source (Pf) in the following calculations.
3. The RF power source (Pf) requires a correction factor (kF). This must be applied to its reading to achieve an accurate power source value as follows.


2. Understanding the Reflection (Mismatch) Factor


- Up to this point, the calculations assume a nearly perfect impedance match between the two devices. However, during real world situations, some impedance variation (mismatch) will always exist between the RF power source (AE-TEGAM feed-through standard) and the RF power sensor (DUT)
- The resulting mismatch causes some of the power to reflect back to the RF power source and therefore not be applied to the RF power sensor.
- The reflection (mismatch) factor is derived from a complex set of values made up of both the amount (magnitude) and the direction (phase) of the reflection of both devices. The reflection values for each device are determined by measurements made with a vector network analyzer (VNA).
- PS-CAL uses the reflection factors for each device to calculate the reflection (mismatch) correction. The software then applies the correction to determine the calibration factor of the power sensor.
- The VNA, used as a 1-port device, measures and collects the reflection data directly from the power sensor. The collected reflection data, taken from VNA scattering parameters, is also referred to as the S11 Rho data. PS-CAL will utilize a VNA, when available and the appropriate procedure is selected, to collect the reflection data during the power sensor calibration process.
- Collecting the reflection data for the source monitor (AE-TEGAM feedthrough standard) requires greater complexity. The data is derived from a set of measurements taken on the feedthrough standard’s internal power splitter during the standard’s calibration process. This reflection data is part of the electronic calibration standards file utilized by PS-CAL when the AE-TEGAM feedthrough standard has been selected.


3. Calculating the Mismatch Correction


- The Mismatch Correction combines the reflection data for the two devices as follows:


1. Combine the linear rho magnitude (p) of the source (p1) and sensor (p2) by multiplying.
2. Combine the phase in degrees (Ф) of the source (Ф1) and the sensor (Ф2) by adding.
3. Convert the combined phase to radians.
4. Convert the Polar data to rectangular (real/imaginary) values by taking the cosine of the combined phase in radians x the combined magnitude (real) and the sine of the combined phase in radians x the combined magnitude (imaginary).
5. Convert the rectangular coordinates (1 – real) and (-1 x imaginary)
6. Square the absolute value of the real and complex numbers (converted real^2) and (converted Imaginary^2).
7. Combine the squared values of the complex numbers by adding the squared real and imaginary values together to determine the combined mismatch correction factor (r).


4. Apply the Reflection (Mismatch) Correction Factor


- Apply the calculated combined mismatch factor (r) above to the calculated sensor correction factor as shown in the formula below to determine the sensor’s corrected calibration factor (kF).


- Note that the combined mismatch and calibration factors need to be calculated for each frequency point required for the sensor calibration.


5. Calculating Uncertainty


- PS-Cal calculates the “system” uncertainty by taking the “root-sum-square” of the following contributors:


1. The uncertainty of the RF standard at the set frequency
2. The uncertainty of the standard power meter
3. The uncertainty of any attenuators/adaptors at the set frequency
4. The uncertainty of the DUT power meter
5. The standard deviation of the test runs (if more than one)
6. The standard deviation of the measurement samples of the 1830A
7. The calculated mismatch uncertainty from the S11 (rho) test


6. Summary: PS-Cal Calculations


- The Calibration Factor, Mismatch, and Uncertainty formulas performed automatically in the PS-Cal software:


1. Sensor Calibration Factor:
2. Mismatch Factor:
3. Uncertainty: Root-Sum-Square of contributors


The PS-Cal software solution automates the power sensor calibration process and executes these complex mathematical calculations. Contact Advanced Energy to learn more about what PS-Cal can do for you.
