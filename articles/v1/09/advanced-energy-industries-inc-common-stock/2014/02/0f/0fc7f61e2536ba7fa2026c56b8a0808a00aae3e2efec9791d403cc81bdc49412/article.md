---
schema_version: "1.0.0"
document_id: "0fc7f61e2536ba7fa2026c56b8a0808a00aae3e2efec9791d403cc81bdc49412"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/increasing-flow-to-increase-power-how-voltage-control-works/"
published_at: "2014-02-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T22:27:47.856975+00:00"
content_hash: "sha256:2cacb4ef77f968e60762b725eaa21f2e70dfeb03ebb5573507c3f1ed7a09cc43"
---

# Increasing Flow to Increase Power: How Voltage Control Works

Controlling reactive sputtering processes is challenging, and there are several options. Voltage control offers a way to reliably achieve a higher deposition rate at the same power, or to consume less power at the same deposition rate, with a stable process and good film quality. The voltage controller can be integrated into the system power supply, with no special additional sensors required.


A family of control curves is shown in the figure below. Voltage control provides a way to control operation in the transition region with control integrated into the power supply. This is an illustrative example created to explain the concept. At point A, there is no oxygen flow. There is just the argon process gas flow normally used for sputtering, to achieve the desired sputtering pressure. At the selected voltage, the process operates at 20 kW. As the oxygen flow is increased, the process moves to point B, at 40 kW. When oxygen flow is increased even more, the process moves to operating point C, at 60 kW, and then to point D, at 80 kW. Finally, when oxygen flow is increased even further, point E is reached. Point E is on the 100 kW curve, so the power supply delivers 100 kW to the process. At a given flow, there is a unique point on the control curve for each voltage. And, at a given voltage, there is a unique power for each value of flow at desired transition region operating points. This enables stable control of the process operating point, by controlling voltage with the power supply, and oxygen flow with a mass flow controller. Power is monitored with the instrumentation built into the power supply, and oxygen flow is adjusted to achieve the desired power.


*Illustrative example of a family of constant power control curves for a reactive sputtering process. Power increases with oxygen flow when voltage is held constant in the transition region*


Voltage control has been integrated into a power supply designed for driving DMS magnetron pairs with mid-frequency (MF) AC power. This power supply is shown in the picture below. It is capable of fast arc detection and fast shutdown, with very low arc energy and short shutdown time.


There are some key requirements for stability of reactive processes operated in the transition region. One is short shutdown time when an arc is detected. Shutdowns that are too long can result in disruptions to the process much longer than the actual shutdown. This has been studied both empirically and with numerical simulations \[1\], \[2\]. The power supply shown here has sufficiently short shutdown time for stable operation in the transition region.


References
\[1\] U. Krause, et al., “Requirements for the System Power Supply Sputter Source for High Power Pulsed Magnetron Sputtering,” Proceedings of the 3rd International Conference on Coatings on Glass, p. 173, 2000.
\[2\] D.J. Christie, E.A. Seymour, “Power System Requirements for Enhanced Mid-Frequency Process Stability,” Society of Vacuum Coaters 46th Annual Technical Conference Proceedings, p. 257, 2003.
