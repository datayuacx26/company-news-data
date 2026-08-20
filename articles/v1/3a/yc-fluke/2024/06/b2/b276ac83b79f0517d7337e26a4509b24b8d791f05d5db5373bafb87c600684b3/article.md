---
schema_version: "1.0.0"
document_id: "b276ac83b79f0517d7337e26a4509b24b8d791f05d5db5373bafb87c600684b3"
company_key: "yc-fluke"
company: "fluke"
source_id: "yc-fluke-news-import-b5cacff74b18"
canonical_url: "https://www.fluke.com/en-us/learn/blog/temperature-calibration/fluke-calibration-bath-safety-features"
published_at: "2024-06-24T00:00:00+00:00"
first_seen_at: "2026-07-25T05:28:17.177808+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:3bf07aa1a98666722766aecde62ee920ef2e0ad122c9dcb8ffc2a1cb47b43b9a"
---

# Fluke Calibration Bath Safety Features

# Fluke Calibration Bath Safety Features


[Temperature calibration](https://www.fluke.com/en-us/learn/blog/temperature-calibration)


The Fluke Calibration metrology baths use the 2100 temperature controller which has the following built-in safety devices.


### **Microprocessor Watchdog**


The microprocessor that controls the functions of the temperature controller has an independent watchdog circuit that monitors the life signs of the microprocessor. If the microprocessor has a cardiac arrest or slips into a coma, a possibility if there happens to be a large nearby static-electric discharge for instance, the watchdog will force a reset of the microprocessor in an attempt to revive it. However, the digital electronic circuits are designed to be resistant to static discharges.


### **Analog Temperature Control**


The temperature control function is realized by analog circuitry rather than the microprocessor. The solid-state relay is connected to the output of the analog temperature control circuit. The microprocessor is only capable of setting the temperature set-point. Thus, steady temperature control will continue even if the microprocessor freezes up.


### **Normally-Open Heater Relay**


The heater relay is placed in series with the heater and solid-state relay. It provides a backup heater disconnect in addition to the solid-state relay. It has two sets of contacts that break the circuit to both the high and low legs of the heater. The relay is normally open and can only be closed when both the microprocessor and the hard cutout together drive it to the operating state. The heater relay is an electromechanical device that is immune to static-electric discharges and current surges.


### **Soft Cutout**


The microprocessor is programmed to act as a cutout that switches the heater off if the temperature of the bath gets too hot. It switches the heater off when it senses, using the control sensor, that the bath temperature exceeds the set-point by more than 20°C. This soft cutout is also activated when the control sensor appears to be outside the normal temperature range, as it would if the sensor was open or short circuited. This feature is designed to protect against failure of the solid-state relay, temperature control circuit, or control sensor. It switches the heater off using the heater relay so it is effective even if the solid-state relay has failed.


### **Hard Cutout**


The bath has a completely independent cutout circuit that switches off power to the heater if the bath temperature exceeds the cutout set-point. It senses the bath temperature using a separate, very reliable thermocouple. The maximum set-point of this hard cutout is fixed by the setting of a potentiometer and is set at the factory to about 15°C above the maximum specified temperature of the bath. The microprocessor can set the cutout set-point to a temperature below this maximum setting but is not capable of setting it higher. When triggered, the hard cutout disables the bath by opening the heater relay. Hysteresis of the cutout keeps the heater off until the bath cools at least 30°C. When the cutout mode is set to manual reset the microprocessor senses when the hard cutout has triggered and keeps the heater off until the user resets the cutout. The hard cutout is designed so that a break in the thermocouple wires causes it to remain in the cutout state with the heater disabled.


### **Safety Fuses**


Each leg of the AC supply has a safety fuse with a rating just higher than the maximum normal operating current of the bath. This protects against an electrical short circuit in the heater, cooling compressor, or other component.


### **Grounded Chassis**


All exterior metal surfaces are solidly connected to the ground of the AC supply so that any potential short circuit condition would not produce voltage on the chassis but instead cause the fuses to open.


## Failure Conditions


The various safety devices work to prevent excessive temperatures, currents, or voltages from occurring under any possible failure condition. Following are various possible failure modes along with a description of how the safety devices work to ensure safety under the condition.


### **Microprocessor Freeze-Up**


Typically, when the microprocessor freezes up the controller continues to maintain steady temperature control since this is done by the analog temperature control circuits. Nevertheless, the watchdog forces a reset of the microprocessor and operation resumes as normal. A worst case situation might be if the microprocessor latched up and in the process set the control set-point to a high temperature, set the cutout set-point to its maximum, and was not able to be reset by the watchdog. In this case the bath might heat up until it reached the cutout limit at which point the hard cutout would activate and disable the heater.


### **Shorted or Open Control Sensor**


A shorted or open control sensor will appear to the microprocessor as if the bath temperature is abnormally low or high. It will consider this to be a failure and disable the heater via the heater relay. A faulty sensor that produces a signal within the normal range of the bath might cause the bath to heat to an unintentional high temperature but could not exceed the hard cutout set-point.


### **Failure of the Solid-State Relay**


A shorted solid-state relay would cause the bath to heat to an unintentional high temperature. Either the soft cutout would disable the heater by switching off the heater relay when the temperature rose 20°C above the set-point or the hard cutout would similarly disable the heater when the temperature exceeded the cutout set-point.


### **Shorted Heater**


A shorted heater typically causes unusually high current to flow and causes the safety fuses to open. A worst case possibility would be if the heater shorted in such a way that it damaged the solid-state relay, causing it to short circuit, then settled at a current just below the fusing current of the fuses. Again, both the soft and hard cutouts would provide protection under this situation, preventing thermal runaway. If the bath happened to be wired in such a way that enough current could flow from the low line to ground to heat up the bath the cutouts would still be effective as both legs of the heater are switched open by the relay.


### **Open Thermocouple**


If the thermocouple used for the hard cutout was to open the cutout would immediately activate, disabling the heater, and it would remain in this condition until the thermocouple was replaced.


### **Temperature Controller Power Supply Failure**


The heater relay is normally open. Failure of the temperature controller's power supply would cause the relay to return to its normal position which would disable the heater.


### **Accidental High Set-point**


A possible situation is the user setting the bath to a temperature beyond the safe limit of the bath fluid or devices under test. The hard cutout is designed to offer protection against this. The cutout set-point should be set to limit the maximum temperature depending on the application. For extra protection the maximum set-point of the bath may be reduced by reducing the H parameter or the maximum cutout set-point can be set at the factory to a lower temperature.


### **Caution**


These safety features notwithstanding, Fluke Calibration baths cannot detect and self-adjust for the type of fluid used in the bath. (Nor can they compensate for misuse or abuse.) Users must understand the properties of the fluid being used, including its flash point, boiling point, freezing point, thermal expansion characteristics, etc. Accordingly, users must set cutout temperatures to accommodate the fluid they select. Serious risk of damage and injury exist when a fluid is used at temperatures outside of its usable range.


For safest operation, Fluke Calibration baths should be used according to guidelines provided in their accompanying user manual. Users should understand and adjust for all the characteristics of the fluid and instruments (or other objects) used in the bath.


## You might also be interested in


Article


### [Infrared Temperature Calibration 101](https://www.fluke.com/en-us/learn/blog/temperature-calibration/infrared-temperature-calibration-101)


Article


### [How to choose a heat transfer fluid for your calibration bath](https://www.fluke.com/en-us/learn/blog/temperature-calibration/how-to-select-calibration-bath-fluid)


[Temperature calibration](https://www.fluke.com/en-us/learn/blog/temperature-calibration) ,[Calibration](https://www.fluke.com/en-us/learn/blog/calibration)


Article


### [Temperature calibration equipment: 4 key bath specifications](https://www.fluke.com/en-us/learn/blog/temperature-calibration/buying-the-right-temperature-calibration-bath-for-your-laboratory)


Article


### [Calibration bath selection guide](https://www.fluke.com/en-us/learn/blog/temperature-calibration/calibration-bath-selection-guide)


Chat with our


Fluke assistant


Clear Chat


[Contact Us](https://www.fluke.com/en-us/support/about-us/contact-us)


[Get 10% off](https://www.fluke.com/en-us/support/about-us/contact-us)
