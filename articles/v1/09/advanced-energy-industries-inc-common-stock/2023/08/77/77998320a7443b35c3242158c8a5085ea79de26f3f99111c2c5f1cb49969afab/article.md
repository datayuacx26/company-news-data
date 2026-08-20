---
schema_version: "1.0.0"
document_id: "77998320a7443b35c3242158c8a5085ea79de26f3f99111c2c5f1cb49969afab"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/digital-power-for-medical-applications/"
published_at: "2023-08-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:e4ac56e7b3b5252daa9f541c8d3ea02a5681eae6f2ac6fe8e6672a4c5e3ebdf3"
---

# Digital Power for Medical Applications

**Digital communications and control deliver significant benefits and flexibility**


*An aging population and a recognition that early detection of ailments enhances life expectancy and reduces costs mean that the number of healthcare applications is growing rapidly. While powering medical technology can be challenging, new digital configurable power solutions offer a number of benefits by enabling control and communication with medical PSUs ‘on the fly’.*


*In this blog we consider advanced power solutions and how they deliver benefits during both design and operation.*


Medical devices and, in particular large medical systems, often have complex power challenges requiring power schemes with multiple rails and, sometimes, non-standard voltages in areas such as specialist sensors and magnetic control. For this reason, configurable power supplies that offer significant design flexibility have become highly popular in these applications. These units have a mains-fed front end and the capability to have multiple output modules to provide different voltages. It is also possible to combine the modules in series or in parallel for the most challenging of applications.


When deployed for healthcare applications, the front end of the configurable power supply will be designed and certified to meet relevant medical standards such as EN/IEC 60601. This provides the necessary safety isolation for patient, ensures operator safety and limits parameters such as leakage current within acceptable ranges. This gives designers confidence that, once configured, the power supply will not impede certification of the entire system.


**Digital Power for Medical Applications**


Increasingly, configurable power solutions are leveraging digital technology, both for control of the power supply and for communication with the rest of the system. This can be invaluable during the design of a power scheme, while engineers are making their trade-off decisions. With the power supply able to monitor key parameters such as voltages, currents and temperatures, the need for expensive test equipment is reduced, as is the set-up time.


Once the design is completed and the system is deployed, continuing to monitor these parameters can provide early warning of issues within the system. For example, an increase in current draw on a particular module, or a rapid rise in temperature could be an early warning of a forthcoming failure elsewhere, which can be investigated and addressed before it happens.


As well as enabling communication, PMBus™ also allows many aspects of the power supply to be configured and controlled, honing it for a particular application. By compensating within the power supply, external components can be reduced or eliminated, minimizing the size and cost of the overall solution.


For example, if it is known that an application has a large complex impedance, high levels of inrush current can be expected. Minor firmware modifications can be used to reduce the rise time of the power supply; this inrush current can be controlled with nothing more than a software modification.


In many applications, the current capability of an output module may be significantly more than the application requires. To enhance safety (for example when a rail requiring 3 A is powered by a 20 A-capable module), PMBus may also be used to reduce the current limit of the module to suit the application / use case.


**Configurable Technology**


Advanced Energy’s CoolX®1800 (part of the Excelsys product line) is an 1800 W-rated configurable modular power supply for medical applications with efficiency levels as high as 91%. Intended for use in a wide variety of applications such as clinical diagnostic equipment, medical lasers, dialysis and radiology, CoolX1800 medical units are approved to IEC60601-1 3rd Edition offering 2 x MOPP (Means of Patient Protection) and IEC60601-1-2 4th Edition for EMC.


Housed in a compact 1U high package, each unit has six slots available for output modules, giving the ability to be configured with up to twelve separate outputs, all of which are isolated from the input (1,850 VAC). An additional 23.5 W always ON auxiliary power output is provided for system standby functions.


A completed power supply measures just 262 mm x 127 mm x 41 mm (10.5” x 5” x 1U). MTBF exceeds 200,000 hours and a five-year warranty is standard. A total of 13 different output modules are available offering single and dual outputs, high power, high voltage and wide trim capabilities.


***Figure 1: Thirteen modules are available for the CoolX1800 modular configurable power supplies***


Source:[https://www.advancedenergy.com/globalassets/resources-root/data-sheets/en-lv-coolx1800-data-sheet.pdf](https://www.advancedenergy.com/globalassets/resources-root/data-sheets/en-lv-coolx1800-data-sheet.pdf)


The flexibility of the CoolX1800 is significantly enhanced by the inclusion of analog and digital management via the monitoring and control capability of PMBus.


**Addressing Real-World Challenges with Digital Power**


Let’s consider a couple of examples of using configurable power supplies.


The first example is a heater in a medical system, which requires multiple fixed resistances to be driven with a constant (but adjustable) current to ensure the temperature remains as needed. The challenge with this customer’s application was the need for 180 VDC with the ability to adjust the current down to zero. While this requirement would normally require multiple power modules connected in series, one CoolX CmK module was able to provide 180 VDC. However, in its standard form, the current would not reach zero. To address this, the onboard firmware was customized. As this was a heater application, the slight increase in ripple voltage was not an issue. The firmware was deployed and tested on customer site by AE application engineers, ensuring a rapid and convenient solution for the customer.


In another design, a different customer was developing a mobile scanning application where the CoolX1800 was being used for 250 V battery charging. The customer needed the ability to set custom current limits as well as to be able to control the CoolX variable fan so that they could provide system cooling even in an idle state.


***Figure 2: CoolX1800 was used in a mobile scanning application where firmware tweaks enhanced usability***


Source: Material supplied by client


To allow the customer to store their own default current limits, a ‘store all’ command was added to the CoolX firmware. This allowed the storage of other PMBus set parameters as well. An additional PMBus command was added to allow the fan speed to be controlled, even when in idle mode. These changes met the needs of this customer application, without any change to the hardware whatsoever.


**Summary**


The flexibility of medically certified modular configurable power supplies makes them an ideal choice for the complex needs of healthcare applications. Adding digital capability further extends the flexibility of these solutions.


During the development phase, communicating with the power supply can speed up debugging, while eliminating complex test equipment. The same communications can monitor the power supply once deployed and provide early warning of impending system failure.


Alongside the communications, PMBus based digital control allows for custom functionality do be introduced via firmware tweaks, thereby modifying performance to meet the needs of specific customer applications.
