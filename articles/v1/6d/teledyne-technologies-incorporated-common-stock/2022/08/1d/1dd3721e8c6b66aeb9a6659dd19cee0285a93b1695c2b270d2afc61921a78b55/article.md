---
schema_version: "1.0.0"
document_id: "1dd3721e8c6b66aeb9a6659dd19cee0285a93b1695c2b270d2afc61921a78b55"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2022/08/how-to-choose-best-high-voltage.html"
published_at: "2022-08-29T12:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:c0e5e5e568d33c657e75466f15933c4a297ae12698aa0975d4a1af0c83bf112c"
---

# How to Choose the Best High-voltage Oscilloscope Probe in 5 Minutes

Figure 1: The High-voltage Probe Selection Guide
color codes better or worse probe selections based on
your answers to three, simple questions.
Click any image to enlarge.


Probing high-voltage (HV) circuits for analysis with an oscilloscope presents unique challenges due to the potential for injury or equipment damage, as well as the demands of the materials used in HV semiconductors. HV floating measurements are extremely dangerous and difficult to make. Conventional passive probes are not the answer, but isolated and high-voltage differential probes are options. Yet, with many possible choices in these categories, how can you decide which is actually the best HV oscilloscope probe for *your* application?


Teledyne LeCroy offers this new, easy way to help you select a high-voltage oscilloscope probe based on your specific application—the High-voltage Probe Selection Guide—available on the Teledyne LeCroy website at:[teledynelecroy.com/powerprobes](https://teledynelecroy.com/powerprobes)


### The Three, Most Basic Questions


When you go to the High-voltage Probe Selection Guide, you will be asked three, basic questions that determine the rightness or wrongness of any probe for a given application.


#### What Is the DC Bus Voltage?


The DC bus voltage will determine the maximum voltage rating required for the probe to be used in the measurement. For AC line signals, this is the peak-to-peak voltage of the AC line. In a switched mode power device, the bus voltage is most often either the full wave rectified peak voltage AC line or the amplitude of the pulse width modulated (PWM) signals from the driver/inverter circuits.


#### What Is the Semiconductor Device Material?


Silicon (Si), Silicon Carbide (SiC) and Gallium Nitride (GaN) are all popular materials for semiconductor devices, each with its unique requirement on rise times for the switching signals:


- Si devices typically cannot handle rise times faster 10 ns
- Rise times on SiC devices are 3 to 5 ns or slower
- Rise times on GaN devices are on the order of 1 to 3 ns


The larger the voltage swing, the slower the rise times get to help keep EMI in check.


To measure fast rise times, as well as some harmonics, the probe needs to have sufficient bandwidth. For example, to measure gate drive signals on a GaN device, the probe bandwidth required might be closer to 1 GHz, whereas to measure output signals on the same GaN device, the bandwidth required could be 700 MHz or even as low as 350 MHz.


#### What Are the Applications?


The High-voltage Probe Selection Guide asks you the intended measurement application, offering a choice of power semiconductor test, floating sensor or system inputs /outputs measurements. This high-level categorization determines the relative importance of many possible probe specifications, including the voltage range, bandwidth, attenuation and isolation.


Power semiconductor testing refers to measurements made on individual devices. This includes capturing MOSFET/IGBT gate drive and output signals, then analyzing them. Analysis includes dead time verification and switching loss measurements. Depending on which semiconductor device is being tested, ideal probe features could include wide voltage range, offset capability, very good CMRR and higher bandwidth.


Floating sensor measurements include probing series or shunt resistors, current or temperature sensors, or discrete components. This type of application usually determines the isolation requirements of the probe, as the signals involved are generally small with large voltage offsets.


System input/output measurements include the line-side AC voltage, DC/DC converter high- or low-voltage inputs or outputs, DC bus or link, and inverter drive PWM outputs. Wide voltage range and common mode are typical features associated with probes for this application.


### Using the High-voltage Probe Selection Guide


Open the High-voltage Probe Selection Guide, answer the three basic questions, and you will get a recommendation for a high-voltage probe with notes on our reasoning behind the choice.


The selection guide rates the appropriateness of each probe using a simple, color-coded scheme:


**Black:** The probe should absolutely not be used for this application, as damage to the probe, oscilloscope or device under test (DUT) may occur, or harm may come to the operator.


**Red:** The probe may be safe to use for this application, but it will probably not provide a good measurement result.


**Yellow:** There are some compromises in performance of the probe in this application, though some users may find the probe works fine for them.


**Green:** This is the perfect probe. There are few issues with its use, and it has been optimized in price and performance for the application. Sometimes, it may be the only safe choice.


The principal reason for the code is shown right on the screen, with more more information behind the "i" button.


Figure 2. HV probe selections for measuring a 260 Vdc GaN transistor.


Figure 2 shows the selections for probing a 260 Vdc GaN transistor:


- Single-ended passive probes (Black) should *absolutely not* be used for any measurement of this device, because it poses a safety hazard due to the floating nature of the signal.
- A high-voltage differential probe (Yellow) may be *used with caution* , but there may be performance trade offs due to circuit loading, and it may not have sufficient bandwidth for higher speed GaN signals.
- The *best option* (Green) is the single-ended fiber-optic isolated probe, the[DL-ISO](https://teledynelecroy.com/probes/high-voltage-optically-isolated-probes) , because only it has the isolation, CMRR *and* bandwidth required for GaN device measurements.


In a future post, we'll go over more of the reasoning behind this selection when we discuss probing requirements for GaN power semiconductor measurements.


There are many considerations to selecting the best high-voltage probe. The High-voltage Probe Selection Guide on the Teledyne LeCroy website is the starting point for choosing the best high-voltage probe for your application. Based on your needs, it provides usefully documented recommendations for any of the Teledyne LeCroy high-voltage probe offerings.


To try the High-voltage Probe Selection Guide, visit:[teledynelecroy.com/powerprobes](https://teledynelecroy.com/powerprobes)
