---
schema_version: "1.0.0"
document_id: "994945ac76585a3ba84ec7352112c045890b44ea3a2576f010de5289b5bcbe95"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/parallel-or-series-operation-of-switched-mode-power-supplies/"
published_at: "2021-10-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:785ecb75d7f008ce5ee1bd8fd98be7ca5bdf239a635343da1f2a71935dfb4e9a"
---

# Parallel or Series Operation of Switched-Mode Power Supplies

### Abstract


A single power supply is most of the time sufficient in applications connected to an AC power source. However, there are sometimes additional boundaries set to fulfill either higher power needs, system reliability, or even mechanical constraints. In the following article, we will try to explain the typical scenarios of connecting more than one power supply in the same system and the reasons behind it.


### Redundancy


The connection of two or more power supplies for redundancy is important in critical applications, where the power source fails cannot be tolerated. The current sharing circuit or, load sharing circuit, is the ability to manage the output current evenly across all active power supplies to greatly reduce stresses on each power supply and allowing them to run cooler which results in higher reliability of the active power supplies.


A redundant sharing is the control of the power supplies internally or externally by switching only the desired number of the power supplies in parallel at the same time. In case of a power failure, the control circuit will automatically switch to another redundant power supply for continuous power delivery. Active redundant configuration allows to keep some of the connected power supplies in parallel not loaded and so the critical components are not stressed. Such a system approach extends the lifetime of the spare power supplies.


A typical selection of the power supplies for redundancy requires choosing the same type of power supplies connected in parallel to ensure identical operation no matter which unit will be connected to the load.


Although there may be more than one way to wire load sharing with parallel power supplies, the star wiring method is typically the one most recommended. In the proceeding figures herein, it is demonstrated three typical star wiring schemes that provide options for any desired level of redundancy:


This method above in figure 1 demonstrates a basic star wiring scheme that offers a basic level of redundancy without the inclusion of extra components that are not strictly necessary for a load-sharing function.


However, many safety-critical systems demand reliability that evolves an advanced redundancy that requires additional components to this load share wiring scheme as demonstrated in figure 2.


Although OR-ing diodes are a successful and redundant method in their simplicity, they may be neglected for their power losses impact. This level of redundancy is still achievable without the impact of such power losses as figure 3 exemplifies. This elaborate scheme consists of the same basic OR-ing star wiring method however, it is considerably more efficient. It demonstrates the introduction of a relevant OR-ing system with the use of MOSFETs driven by a compatible integrated circuit controller.


###
Higher current (parallel operation)


There are designs with configurations of power supplies in parallel for loads with higher current. The reasons for the connection of several power supplies in parallel instead of using higher power units can be for example modular configurations or a variety of applications in a design house with an extended wider power range, mechanical limitations, or even lack of products in the market with the desired specification. A common example in modular applications is when the system designer adds more and more power supplies in parallel as the system building blocks are added. In a wide variety of scenarios, the purchasing team might prefer to have one single power supply on their bill of materials in simple designs and stacked power supplies in parallel from the same type in more powerful ones.


The selection requirements of power supplies in parallel operation are similar to those for redundancy, but the control function differs. It is obvious, in this type of application a single unit is not sufficient to provide desired power needs, so two or more power supplies in parallel are expected to be always loaded. The control circuit responsibility shifts here to balancing the load sharing among the connected power supplies equally as possible.


The balancing circuitry implementation can be achieved both internally in the power supply or also with external control units. An example for internal control implementation would be an additional load-sharing IC such as UCC29002 from TI. Power supplies in parallel with internal control will require an additional current share signal line for this method as shown in figure 4. External sharing control, such for example offered by Analog Devices LTC4370, is achieved by modulating the MOSFET voltage drops to offset the mismatch in the supply voltages (figure 5). \[https://www.analog.com/en/technical-articles/novel-current-sharing-ic-balances-two-supplies-with-ease.html\] This circuitry allows usage of any power supplies in parallel and takes the balancing control on additional independent PCB.


In some applications, where the output voltage drop is allowed to be tolerated, the droop share method can be used. A basic understanding of such configuration is when the power supplies are designed to decrease the output voltage with increased load current. This allows two or more power supplies to “meet” with increased load current at the same voltage level and provide the power in parallel as seen in figure 6. V1 and V2 power supplies are identical but due to manufacturing tolerances often slightly differ in the output voltage. V1 has a higher output voltage and will be the first to support the load. With increasing current and so decreasing voltage V1 will meet at some point the V2 level and start sharing the load between two (or more) power sources.


The key points to consider for parallel operation of the power supplies are:


- Power supplies connected in parallel should have the same output voltage
- This type of configuration is targeted to increase the total output current
- Balancing of the output power supplies is recommended to distribute the load between the power supplies equally as possible.
- The trade-off to control the current share internally or externally should be considered.
- The trade-off to using the droop share method without a feedback loop but with a voltage drop at higher currents should be considered as well.


### Higher Voltage (series operation)


A somewhat easier technique to increase the total power is to connect the power supplies in series. The assumption is, that there are power supplies with lower voltage available to achieve the desired output voltage by stacking multiple power supplies in series. The output voltage of the overwhelming majority of power supplies available on the market is lower 60Vdc. System designs with voltage requirements higher 60Vdc might want to grip to this type of solution.


The main considerations for power supplies connected in series are listed below:


- Stacking power supplies with different voltages is possible. However, the user must review the safety standards especially if the total output voltage may rise higher 60Vdc.
- The maximum possible total current is defined by the smallest unit which will run into the overcurrent protection in case of an overload event. The designer should plan exact recovery scenarios.
- Additional features such as inhibit, or DC_OK signals are very often sharing the same output return (DC ground) of the corresponding power supply. As shown in figure 8, the output return of the stacked power supply B in series connection is elevated to the output voltage of power supply A. Connecting the control signals of both power supplies shortcuts the output of one of the power supplies.


If the control features of the power supplies in series are nevertheless required, this can be achieved with signal isolation boards such as ADuM6422A.


In case of an accident load shortcut or defective condition, the connected power supplies in series will be connected backward. If the power supply is not fitted with reverse polarity protection, additional external reverse bias diodes are recommended to be installed. (figure 9).


### Wiring considerations


With any of these optional load sharing methods, it is considered good practice to arrange the wiring from each paralleled power supply to the system/load in such lengths that match each other. Also, besides a comparable wire length, it is as important to consider its gauge vs. the load current it has to withstand as it is a ruling factor in controlling wire conductive losses. Such conductive losses can collectively creep up and develop a voltage drop level that may become out of range of the remote sensing control specified for the power supply being used.


As an accessible instrument, the following table (Table 01) below spreads wire gauges in assorted lengths versus their inherent voltage drops accrued from their copper losses at a maximum ampacity of 250 mils per amp.


The terminations of the wires and their contact connection losses are not accounted for in the figures of the table (Table 01) above as they may develop additional voltage drops to be considered. Accounting for all incurring losses in any load sharing configuration shall subsequently contribute to a successful configuration for a worthy load sharing performance.


### Summary


There are many different options available to increase the total power of the system or add a redundant power supply for reliability reasons. Whether the connection of the power supplies has to be in parallel or series, it is always recommended to consider worse case scenarios such as load short circuit or power supply failure. Feel free to contact us for further information or even just for knowledge exchange.
