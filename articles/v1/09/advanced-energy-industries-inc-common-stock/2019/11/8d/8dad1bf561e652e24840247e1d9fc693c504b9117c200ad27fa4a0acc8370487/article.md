---
schema_version: "1.0.0"
document_id: "8dad1bf561e652e24840247e1d9fc693c504b9117c200ad27fa4a0acc8370487"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/considerations-when-using-an-open-frame-power-supply/"
published_at: "2019-11-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:5bb83eb4816b32de8225b5392075fbf0186dee9dc6c3b24c76b5ceda934c76e1"
---

# Considerations When Using an Open Frame Power Supply

When selecting an external power supply for a device (e.g., a medical tablet or industrial imaging display) the most basic electrical considerations are to match the required input (AC) and output (DC) voltages, the required (rated) current, and to define the connector (e.g., USB, barrel, USB-C®, or some other type). However, matching the most basic requirements is not your best option, especially if you want to prevent damage. So, what else should you be looking for? Here, we will be discussing switched-mode power supplies (SMPS) that operate off an AC line (i.e., external AC/DC adapters). We’ll save application-specific considerations for another time.


Consider some critical specifications that impact long term reliability and go a long way towards preventing damage to your devices. Start the selection process by listing the requirements below:


- Input voltage range
- Line frequency
- Output voltage, current, peak current, and voltage regulation
- Operating temperature range
- Whether or not you want Power Factor Correction (PFC)
- Required agency safety approvals: RoHS, CE, FCC, or other compliance requirements for your application (e.g., medical)
- Enclosure type and size
- Whether or not you need a cooling system (integral fan, system fan, convection-cooled, liquid-cooled, etc.


## Input voltage and line frequency


The input voltage range and line frequency typically depend on the line source, which varies by country. You will also need to know if your application can tolerate slight variations in voltage. If the power supply cannot condition spikes or dips in the line voltage, then those variations will get piped directly into your device. If your device does not have a robust internal regulator, then damage can ensue if the line voltage (i.e., what comes directly out of the socket) is “dirty” or “brown”. Damage control boils down to circuit protection devices that are integrated into the power supply. Cheap power supplies often are lacking protective components to save on cost.


## Peak Current & Voltage Regulation


When selecting a commercial off-the-shelf (COTS) power supply, match the output voltage as closely as possible to what your device needs, followed by the current (30% or higher than what you need is recommended). Often, the output power rating that’s published for an external power supply or adapter is frequently the maximum rating. However, the peak (maximum) current should be the most current that the power supply can momentarily deliver. Always size your power supply or adapter at 20% to 30% above the published peak current (or published wattage).


For example, if your device needs 1A most of the time, then do not select a power supply rated for 1A peak (maximum) current. Doing so will constantly drive your power supply at its maximum capability, heat it up past normal operating temperatures, and damage it or at the least, wear it out much faster. Peak current is for driving devices that have a momentary peak load when they are first turned on, for example, not for the normal operating current draw. Peak currents can exceed the maximum rating of the integrated chips (ICs) in your device. The damage from exceeding the maximum power rating of the ICs is not readily apparent but will cause latent or erratic chip failure over time. Therefore, consider a power supply that’s rated to safely bear peak demand loads (and not exceed it). The continuous power drawn should be less than the maximum power rating. For reliability’s sake, your power supply should have a maximum power rating that’s 30% more than what’s necessary under normal operating circumstances. Thus, if you have a device that requires 100W to operate, size the power supply at 130W and remember that the maximum power supply rating is often the published value.


Minimum current rating is also a consideration. At a very low load, a switching power supply delivers power in very small pulses. The pulses can only go down to a minimum energy level that can be more than the external load can consume. Ultimately, this scenario can cause a pulse width modulation (PWM) IC to shut down, restart, and initiate a “hiccup” cycle. The minimum load specification provides a margin to ensure that there is always enough load to run without a hiccup. Learn more about minimum load requirements in the application note Minimum – Why is it required and What Happens if I Don’t Have Enough? (PDF).


## What choices are you exchanging for long term reliability?


You don’t want to unnecessarily oversize the power supply, either. A higher continuous output power rating translates to a larger power supply at greater cost with higher thermal ratings. Remember that the published power rating for most power supplies is often based on the peak or maximum current rating. It’s up to you to oversize the power supply by 30% or more so that it doesn’t get continuously driven at the maximum output that it can possibly produce.


Power is voltage times current (P = V x I). Let’s say that you get the power supply rated at 130W when you know the device should only ever need 100W because you know that the power supply will last much longer if you do. What if a load surge increases output current by 30 percent? Your device could be happily operating at 100W and then suddenly start drawing the maximum 130W, perhaps due to a component failure. The fix for that is an overcurrent protection limit (circuit protection) for the power supply, which is designed for short durations of 20 – 30% more than rated continuous power. However, remember to choose a power supply such that the average load demand is always less than the maximum current rating.


## Operating temperature range


The power supply’s rated operating temperature is directly affected by the ambient temperature in which the device operates. Most power supplies are designed with an assumption of operating at ambient room temperature. If a power supply operates consistently at higher temperatures, the case may get too hot to touch, cause burns, and the power supply will suffer damage in the long-term. Make sure that the published external case temperature is acceptable. To learn more about safety standards for maximum allowable temperatures, see[SL Power Electronics’ application note Design Considerations for Maximum Allowable Temperature (PDF).](https://www.advancedenergy.com/getattachment/de0a5180-3b23-4825-8c83-038fc29cde83/AN_Maximum_Allowable_Temperature.pdf?lang=en-US)


## PFC


Power Factor Correction can be sought in a power supply to increase efficiency, especially if there are a lot of reactive components in the device (e.g., inductors, motors, transformers). Ultimately, PFCs reduce the cost of electricity by improving efficiency.


## Regulatory agency approvals


Over the years, more certifications have been required for products that use electricity. The certifications vary by country, but include requirements for safety, protecting the environment, and not creating electrical noise that can interfere with other devices. However, it’s not difficult to find non-certified products being sold online these days. It’s difficult for agencies to enforce compliance with overseas vendors who pop up and disappear online at will. Counterfeit power supplies can sport the certification symbol with zero compliance or consideration for safety. In the end, it’s the local end-user or seller that can face fines for dangerous or disruptive devices, so buyer beware. The reason why airlines ask you to turn off devices during take-off and landing is the possibility that some devices can cause enough electrical noise to interfere with the pilot’s communication to the tower.


## Enclosures / Cases


External power supplies are not enclosed inside another product’s enclosure or case. Common names for external power supplies include the term “brick,” because the earlier versions were about the size and shape of a brick from a house. A wall-mounted power adapter is commonly called a “wall wart” – no clarification is needed on that one. External power supplies will be housed in an enclosure that will get hot, especially if driven continuously at peak loads. Take care in choosing a power supply that will operate at peak loads yet is sized as small as possible to avoid the higher cost. The trade-off for cost reduction may be a smaller case, but don’t forget to look at the temperature profile when operating at higher ambient temperatures and/or operating continuously at high loads. The external power supply is very likely rated with the design assumption that it will operate with enough air movement around the enclosure to maintain an acceptable ambient temperature around the power supply. If your power supply includes forced air cooling, it must have an open passage for air to flow.


## Cooling


Once you’ve determined the electrical requirements necessary for your external power supply, you might find that there are no trade-offs (e.g., size, environmental conditions, cost) left to play with that will ensure safe operation of the power supply without integrated cooling.


To learn more about how SL Power balances the key performance characteristics for a power supply, using the acronym TREPP, and how our external power adapters consistently outperform the competition, please contact us.


**Don’t miss out on the latest and greatest from SL Power Electronics** – make sure to fill out the request for more information from below to start receiving notifications of SL Power news, updates, new content, and more!
