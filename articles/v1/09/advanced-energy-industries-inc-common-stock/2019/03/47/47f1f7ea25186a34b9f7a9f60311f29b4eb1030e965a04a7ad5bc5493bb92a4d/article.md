---
schema_version: "1.0.0"
document_id: "47f1f7ea25186a34b9f7a9f60311f29b4eb1030e965a04a7ad5bc5493bb92a4d"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/how-does-a-digital-thermometer-work/"
published_at: "2019-03-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:a4a3077ed220bf591a4f1b8419f508ade3304d008499294f83df01a7759548d9"
---

# How Does A Digital Thermometer Work?

***How Do Digital Thermometers Work?***


Thermometers used to be simple.


First, there was a glass tube **filled with a liquid** that expanded up the tube as the temperature increased. We all understood how that worked. Then came the **bi-metal** thermometers. They were made from a layered strip of 2 metals with different coefficients of expansion formed into a coil. When the coil was heated the metal expanded and the coil unwound a small amount. This motion is used to move a needle which points to the temperature. This is a straightforward concept that most people also understand.


*The earliest temperature observations in the world: The Medici Network (1654-1670) – Scientific Figure on ResearchGate.*


Today, the best thermometers are **digital** , both for speed and for accuracy. Yet, most people do not know how a digital thermometer works.


The latest in digital thermometers – the TEGAM[931B Data Logging Digital Thermometer](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/thermocouple-thermometers/datalogging-thermometers/931b/)


### **This is a brief overview of how digital thermometers work.**


It all starts with the **sensor.** Unlike the liquid-filled thermometer and the bi-metal thermometer, a digital thermometer needs a sensor.


There are 4 popular ***sensors*** used today:


1. Thermocouples
2. Resistance Temperature Detectors (RTD or PT100)
3. Thermistors
4. Solid State Sensors


These sensors all produce either a voltage, current, or resistance change when there is a change of temperature. These are **“analog” signals** as opposed to digital signals. (More on this later.)


To understand how the digital thermometer works, you need to know how the sensor works. Without going into great details, each of these sensors has a different analog output.


- **A thermocouple has a self-generated mV signal** that is proportional to the difference in temperature between its two ends.
- An RTD is a resistor that **changes its resistance near linearly with temperature** .
- A thermistor is a resistor that **changes its resistance in a non-linear way** with temperature.
- **A solid state sensor requires external power and emits a small linear voltage** proportional to temperature.


The thermometer must excite and measure the “signal” from the sensor. Each method is different, but the result is an electrical signal that is proportional to temperature.


### Digital Thermocouple Thermometer


A[digital thermocouple thermometer](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/thermocouple-thermometers/) needs two measurements to determine temperature. First, it has a sensor to measure the temperature where the thermocouple connects to it – this is known as “cold-junction compensation (CJC). Secondly, it measures the mV signal from the thermocouple. In order to determine the temperature at the end away from the thermometer, it subtracts the CJC temperature from the hot end signal and then converts that voltage to temperature.


### Digital RTD Thermometer


A[digital RTD thermometer](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/rtd-thermistor-thermometers/rtd-thermometers/) is, in essence, an ohmmeter. It measures the resistance of the sensor. To do this, it applies either a very small excitation voltage or current to the sensor and measure the voltage across the sensor. A 4-wire connection is often used to minimize measurement errors – 2-wires to carry the excitation, 2-wires to measure the voltage across the sensor. Very precise systems also reverse the polarity of the excitation and average the two readings to remove and inductive, capacitive and thermocouple effects. Some also pulse the excitation to minimize self-heating of the sensor due to the excitation power.


### Digital Thermistor Thermometer


A[digital thermistor thermometer](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/rtd-thermistor-thermometers/thermistor-thermometers/) provides an excitation voltage or current to the thermistor. Because the thermistor change in resistance is large compared to the resistance of the leads, a thermistor usually has a 2-wire connection. The excitation is converted to a voltage signal by the thermistor and the thermometer then converts the measured voltage to temperature.


### Digital Solid State Thermometer


A digital solid state thermometer provides an excitation current to the sensor and measures the linear (mV/°) signal from the sensor.


Now we have the thermometer *conditioning the sensor* and making an *electrical measurement* of its output. This signal, voltage or resistance, is the **analog** signal. In the thermometer this signal goes to an **A to D converter** ( *Analog to Digital* ). The A to D converts the analog signal into a series of pulses ( *digital signal* ). Now the signal is in the digital world. Once in the digital world, the signal is matched against the resistance curves of the sensor, CJC added for a thermocouple and the digital signal is sent to a display drive that converts the temperature signal into a signal to turn on certain segments of the display so that you, the human, can “read” actual temperature determined.


The accuracy of the **digital thermometer** mostly lies in both the analog and the digital worlds. The precision of the excitation and the errors making the measurement plus any errors due to the change in ambient temperature in the analog world, and in the digital world, the error in the A to D converter and the precision of the curve fit equations used to match the exact curve of the sensor.


- An RTD thermometer uses the **Callendar-Van Dusen equation** that describes the relationship between the resistance of a platinum resistance sensor (RTD) and temperature.
- A Thermocouple thermometer uses **thermocouple tables or polynomials published by ASTM.**
- Thermistor thermometers use **tables or polynomials provided by the sensor supplier.**
- Solid state thermometers get a linear mV/ᵒ signal, so **no additional digital conditioning is needed** . The accuracy of that signal, however, is dependent upon the precision of the excitation, the A to D converter and the inherent error of the sensor.


### **In closing**


You now have an overview of all the signal processing that goes on inside a digital thermometer. Understanding how the sensor and the thermometer interact should help you apply the sensors properly and make the best possible measurements.


### Resources from TEGAM for Digital Thermometers


- [Temperature & Humidity Product Brochure](https://www.advancedenergy.com/getmedia/7949e884-054a-4c84-af4d-cbf97aa5ec1b/TEG_0130_broch_thermometry_8-5x11-2.pdf)
- [Digital Thermometers & Temperature Probes – Products Main Page](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/thermocouple-thermometers/digital-thermometers/)
- [Temperature Probes Guide Infographic](https://www.advancedenergy.com/getmedia/76aa8155-cb08-44a1-b97a-07b0835cb6e2/TEG_0127_probe_selection_guide_11x8-5.pdf)


Please[Contact us here](https://www.advancedenergy.com/en-us/contact-us-form/) with questions or queries of our digital thermometers.
