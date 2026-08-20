---
schema_version: "1.0.0"
document_id: "c8daa3de6bbb2bcdc621860d94fde918364ece19e128e319adfe4fa3d21cea79"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/digital-control-of-power-supplies/"
published_at: "2021-09-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:035f4853be2f46db22501f44984ff2f76e2b0b0649602251bfbc9f04a3fd7a5d"
---

# Digital Control of Power Supplies

### Preface


The progress of innovation driven by technology in past decades creates a vast number of applications to support our needs for electrical equipment with standard or advanced functions. The overwhelming majority of devices are powered from an AC power sources, even those with batteries need to be recharged. New concepts of smart systems are to be as intelligent as possible. This provided design engineers opportunities to use internal components with expanded features e.g. telemetry, fault logging, external control or easy adjustment. Integration of digital control into the AC-DC power supply is an under-utilized next step to capitalize on the opportunities mentioned above. Digital control of a power supply can be broken down into two perspectives, external communication and control of the power supply (On/Off, setting adjustments etc.) and internal control (feedback loop, dynamic behavior, response to AC mains changes and output load changes etc.) of the power supply to enhance performance and capability of the “Black Box” power supply. This application note will discuss both perspectives and highlight the considerations and trade-off made in the product definition phase.


### What is EFT in real life?


A typical AC-DC power supply consists of building blocks using the most suitable technology depending on the power level along with required regulatory and industry compliance standards. So for example while for a simple home accessory a low power fly-back converter without power factor correction (PFC) with acceptable output voltage ripple can be used a high precision measurement device must withstand extreme levels of protection within industrial environments according to IEC61000-4-x, lighting units are sensitive to line harmonic emissions and must comply to IEC61000-3-2 or flicker IEC61000-3-3 and medical devices might require extended isolation and very low earth leakage current for the assemblies. The general blocks of the switching AC-DC power conversion unit are:


1. Input filter + protection.
2. Rectifier + PFC + Intermediate energy storage.
3. Isolated DC-DC conversion and filter.
4. Output voltage/current control with feedback controls.
5. Fault protection.


The input filter and protection are responsible to a protect power supply and it’s loads from disturbances or fault conditions on the electrical utility network. The level of protection is defined by standards such as EN/IEC61000-x according to its area of use. Other functions of this block are to comply to standards such as EN55022/550032/55015 i.e. to remain within the given limits of electromagnetic emissions.


The next part in the chain is the so called front-end block. It contents a rectification stage and an active power factor correction stage to minimize AC mains current distortion and generate a stable energy storage point for further power conversion. This is the block to look at for wide input AC voltage range along with good power factor correction, low harmonics distortions and high efficiency. The controller on the primary side of AC-DC power supply monitors voltage and current changes depending on network and load condition and control power switching devices as part of the power conversion process.


Specific safety standards usually require the output voltage to be isolated from hazardous electrical circuits. This type of protection is achieved by adhering to creepage and clearance distances between primary front-end circuitry and secondary output circuitry after the isolation circuitry of the power supply. By controlling the switching frequency or pulse width of power devices across the transformer, while using several techniques, the isolated DC-DC conversion allows power transmission to the load.


The load regulation block is responsible for precise output voltage and/or output current control. Sensing deviations from the desired voltage or current values and delivering this in a closed feedback loop back to the controllers on primary and secondary sides of power supply allows both sides to adopt the power transmission and regulation. Monitoring of fault conditions including overcurrent, overvoltage, short circuit or overtemperature events are additional functionality that this block offers.


### Typical Analog Design


To provide the desired functionality of the power supply the controllers whether on primary or secondary side of the AC-DC converter are well adjusted by surrounded electronic components. Based on simple RC networks to control the timing of the startup circuit and allow capacitive loads to charge correctly or to delay the DC-DC conversion before the primary side is stabilized for smooth load regulation is the simplest and inexpensive way to set up the power supply. Once the complexity of regulation rises, the number and type of the tuning components also increase. The feedback regulation becomes even more complicated if not just the output voltage, but also current limit and fault conditions must be monitored. This adds unwanted poles and zeros into the transfer function and affect often the stability of the unit. The design cycle of a new power supply may last much longer as the market can wait for and the regulatory tests must be often repeated if anything is changed on safety related parts of the power supply.


### Why Digital?


Microcontrollers and processors become more and more powerful allowing with every new generation to manage higher amount of operations per time unit. The digital control of a power supply opens new opportunities for chip manufacturers to serve customers with newer features. Market trends observation over the last 20 years shows that the end customer has challenges during new system design which are price, time to market, product size and in some applications ease of re-certification and additional telemetry with the power supply. Going into the deep bill of materials comparison between digital or analog design of a power supply demonstrates clear advantages of digital control. The component count of housekeeping components is now replaced by internal algorithms and the functional blocks such as PFC and modulation may be integrated into one chip. Revision changes during and even after design phases are made by firmware updates. This allows an engineering team shorter development cycles as the layout of a circuit board doesn’t have to be reworked and so no time lost on additional prototype builds. The same advantage over analog control is to mention re-certification of a power supply whenever additional features or light modifications in functionality can be just enabled or disabled by firmware updates without re-spin of hardware. Advanced digital design may offer state changes logging for better quality control. Moreover, it is common to find power supplies with potentiometers for output voltage tuning. Setting the output voltage in using digital control eliminates possible drift and sensitivity of these parts to mechanical shock.


The more parameters are sensed, provided and regulated as variables by the microcontroller the more complex functions can be added. So, for example the overload fault state is not activated immediately at a sudden high load transient but lets the digital control to manage this event several cycles before switching into protection mode. The same variables can be shared with the outer system to have an option to read out the parameters for intelligent information sharing between the building blocks. Communication interfaces such as PM-BUS, I²C, CAN, LIN and others offer to set, adjust or for example limit the values to a desired setpoint according to application’s requirements. The digital controller may be programmed to skip pulses and operate in burst mode for better low load conditions. It can also disable operation if the input voltage is out of range or to allow the EMI filter to be designed for certain bandwidth by accommodating non-linear adjustments to line and load conditions. A hidden important benefit of a power supply with internal digital interface is how it assures reliability over the years of use. Monitoring and adjusting of internal processes based on environmental temperature changes or aging of components helps to predict risks and so makes the design more robust.


### Challenges with Digital


Digital measurement of any electrical parameter is a sampling process to be done by Analog to Digital Converters (ADC). The higher the bit count of ADC, the better is the resolution of the measured signal. The numeric results that are stored for processing is yet a discrete finite value which is still not as accurate as of the traditional analog design. Unnecessary oscillations on power path can occur due to signal ripples between two digital states while trying to adjust several functions at once. Furthermore, the nature of digital interface is to follow an algorithm which could be interrupted due to other higher priority instructions. The worst-case situation here may happen when the feedback correction must react exactly between the instructions cycles and so might be delayed or missed. Such a scenario could be negligible for load transients, but it is crucial in fault conditions. This means that the tasks of a digital signal processor (DSP) must have significantly faster sampling rates and instruction cycles to be able to react on time. It makes the digital design even more complex when the switching frequency of DC-DC conversion must be considered too. For higher power density, power supply designers are challenged to reduce the size of the main core which is inversely proportional to the switching frequency of converter. With higher switching frequency, the sampling speed of error correction controller must be increased accordingly to remain the effective resolution of the feedback loop during each power conversion cycle. The maximum no load power consumption of the power supply is also affected by a digital controller as this one is active even in stand-by mode. Assuming that the controller located on the back-end stage is powered by small AC-DC auxiliary supply and can reach up to 100 mA at 3.3 V, the margin to the common limit of 500 mW is not sufficient enough under all input voltage and thermal conditions.


### System Designer Cost-Benefit Trade-Off Considerations


Monitoring complexity is a function of the number of parameters to monitor and whether they are located on the output side or input (isolated) side of the power supply. The type of parameters to be monitored is also a significant consideration. Voltage, temperature and digital signals are the more common and less complex parameters to monitor. Current signals require a sensor to develop a voltage signal proportional to current however and this becomes more costly and difficult with higher currents. Measuring specific component temperatures can also add complexity if the temperature sensor needs to be attached to a device and may require a level or isolation.


Monitoring parameters on the input side of an isolated converter requires signals be isolated from the system interface. Monitoring the AC input voltage and current, power and power factor correction require a more complex processor and signal conditioning circuits and sensors. A manner to communicate the data to the main processor on the output side of the power supply, assuming that is where the system interface is situated, is also needed. System designer really need to understand the benefit of having the data and whether to capture the data and process it at the system level or leave that intelligence to the power supply and reduce the interface to the system.


Storing of data is also a consideration. The power supply can provide a regular update of operating parameters and let the end system do the computation and storing of data. This reduces of the cost of the power supply but requires the system handle the data storage and computation. This may be a better option for system that already have the resources and hardware to do this by virtue of the application.


Control sophistication of power supply for end customer is therefore a major decision to consider while choosing the most suitable control interface of the power supply within the system, each has benefits and trade-offs. Some systems require a power supply with just digital monitoring of parameters such as voltage, current or temperature. Other applications often need an option to modify quickly the reference setpoint of the feedback loop that can be done digitally with same hardware samples from existing stock. Highest level of complexity is to replace analog feedback loop by a digital controller, but the end customer will see the difference only if obvious specifications of the power supply like for example size reduction will be presented.


### Our Approach Today


Serving medical markets for over 60 years SL Power Electronics has developed the know-how and expertise for high quality high performance power supplies. Expansion to test & measurement and to lighting markets follows the clear mission to save and sustain lives, enable precision and enhance the experience of end users. Nowadays the digital control is still a new technological step for any power supply manufacturer.


With new technology come unknows and initially higher costs. Therefore, the first power supplies to emerge on the market are addressing the most valuable features to system designers which are on the DC side of the power supply, hence the design is a hybrid solution. The functionality of the front-end circuitry is a stable building block which is required to protect the power supply and more important, to provide best immunity to the system. To make further gains in size reduction and efficiency improvements will require the PFC controller to operate at high frequencies and behave in a very non-linear manner. Its main job can not be fulfilled by analog controllers effectively to achieve this level of performance. The down converter on secondary side should have the ability to read out input stage condition of input voltage and bulk cap voltage to use it for telemetry with the system. Monitoring of a front-end block adds the possibility to deactivate the DC/DC converter stage while voltage dips on input occur as those are harder to handle. Digital control has much to offer in addressing the challenges to achieve further size, power loss and perhaps cost reduction as well as offering fault protection prediction.


While internal digital control of the power supply operation and functionality offers new advantages to reduce power size, power losses and potentially cost, “external” digital control can offer benefits for end applications also by providing monitor of power supplies operating parameters and control of protection and operating set points.
