---
schema_version: "1.0.0"
document_id: "68bbff7d307aef7b2a5aeb0098b35ec942fced258ff2bc99184276236e14f98a"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/what-exactly-is-cold-junction-compensation-how-does-it-relate-to-the-use-of-thermocouples-as-temper/"
published_at: "2019-03-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:3b525999cf8447bbed3aed2b7b4d13ee215d38a9dd923a4dd87242973bed80d6"
---

# What Exactly is Cold Junction Compensation? How Does It Relate to the Use of Thermocouples as Temperature Sensors?

I was recently asked, ***“What does Cold Junction Compensation do?”*** This question requires that I summarize how thermocouples work and the physics behind them.


## How Thermocouples Work: Measuring Temperature with Precision


**A thermocouple develops a thermoelectric voltage** based upon the principle known as the **Seebeck Effect** . Now, what is the Seebeck Effect?


In the 1800s, Thomas Seebeck was trying to make electricity from heat and he experimented with a circuit of Bismuth-Copper and Bismuth-Antimony and showed that when the two junctions of the two materials were at different temperatures they produced a sustained current; converting thermal energy to electric energy.


I am going to try to turn this into an analogy that will make it more understandable:


Think of a hollow tube with low pressure on one end and high pressure on the other. What will happen? Air will flow through the tube from the high pressure end to the low pressure end (like blowing through a straw).


To relate this to a thermocouple, at the hot end the heat has excited the electrons and they are moving faster (high pressure). At the cold end the electrons are closer together and moving slower, low pressure. The result is that electricity flows from hot to cold. **The greater the difference in temperature, the greater the voltage – The Seebeck effect** .


### Thermocouples Measure Temperature Differences, Not Absolute Temperature.


That is the basis of a thermocouple. What is not clear yet is how the voltage measured is converted to a temperature measurement.


The amount of thermoelectric voltage that the thermocouple produces is based upon the difference in temperature between the two ends (and the material). ***Difference in Temperature?*** Yes, a thermocouple actually measures a differential temperature – not the actual temperature at the hot end.


In order to know what the temperature is at the hot end, you need to know the voltage produced AND the temperature at the other (cold) end. To make the thermocouple a useful tool in the 1800s, the cold end was put in an ice bath; something that any laboratory could readily reproduce. Next, thermocouple voltage tables were developed based upon the cold junction end being in an ice bath. At that point, the thermocouple wires were connected to copper wires with no thermoelectric effect and carried by the copper wires to the measuring device. The voltage was recorded and looked up in the tables referenced to the ice bath (0°C /32°F) and the hot end temperature was derived.


## How Cold Junction Compensation Works Today


***COLD JUNCTION COMPENSATION* replaces the ice bath.** An electronic circuit replaces the ice bath by adjusting the voltage, compensating as if the cold end was in an ice bath – hence **cold junction compensation** .


A[thermocouple thermometer](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/thermocouple-thermometers/) has a connection point where the thermocouple attaches to it. This is the “cold junction.” It is not at the ice point (0°C /32°F) so the thermoelectric voltage measured does not correlate to the thermocouple tables. The point where the thermocouple is connected to the copper connections of an instrument is the cold junction. Either a precision RTD or thermistor is used to measure the temperature at that point. The electronics of the system determine the temperature at this point and then computes the voltage that a thermocouple would make from the ice point to that temperature.


The hot end is only producing voltage equivalent to the difference in temperature from the hot end to the instrument connection. This signal is not properly referenced to the ice bath tables. The cold junction circuit compensates for this missing voltage by adding ( *or subtracting if the cold junction temperature is below the ice point* ) to the measured voltage coming from the hot end.


The combined voltage is then properly referenced to a cold junction at the ice point and can be accurately converted to the true hot end temperature using the standard thermocouple tables.


## In Summary


Cold junction compensation compensates for the missing thermoelectric voltage due to the fact that the thermocouple cold end at the instrument is not at (0°C /32°F). This then allows **electronics** to use the established thermoelectric voltage tables ( *or polynomials* ) to determine the temperature at the hot end. **Cold Junction Compensation is the reason that the thermocouple moved from the laboratory to the most widely used temperature sensor in industry today.**


You can view Advanced Energy's TEGAM[Thermocouple Thermometers here](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/thermocouple-thermometers/) . TEGAM also carries[RTD Thermometers](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/rtd-thermistor-thermometers/rtd-thermometers/) and[Thermistor Thermometers](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/rtd-thermistor-thermometers/thermistor-thermometers/) . We also design and manufacture[Temperature Calibrators](https://www.advancedenergy.com/en-us/products/sense-and-measurement/thermal-sensing/temperature-calibrators/) . As always, if you have questions about this blog post, Thermometry technology in-general, or if you wish to talk to TEGAM about our products,[please Contact Us here](https://www.advancedenergy.com/en-us/contact-us-form/) .
