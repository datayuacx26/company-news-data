---
schema_version: "1.0.0"
document_id: "cda20ff61a0c4fb633b33f4a1984d2063f31e44ba831d1f5a97436b90b3bb866"
company_key: "advanced-energy-industries-inc-common-stock"
company: "Advanced Energy Industries Inc."
source_id: "advanced-energy-industries-inc-common-stock-rss-7c5b1a0c72ca"
canonical_url: "https://www.advancedenergy.com/en-us/about/news/blog/dual-magnetron-sputtering-delivering-high-power/"
published_at: "2015-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:13.689408+00:00"
fetched_at: "2026-07-28T22:27:44.458928+00:00"
content_hash: "sha256:021598c7684d9ee392ac18493890b2a246e53458ac209355bde32bb7647aeb8f"
---

# Dual Magnetron Sputtering: Delivering High Power*

Current source supplies are ideal for pulsed dual magnetron sputtering (DMS). They have the advantage of little to no current rise when an arc occurs, and they enable arc detection by voltage fall due to high incremental impedance.


Process stability is another advantage. The system tends to be self-stabilizing during the pulse; a sudden decrease in plasma impedance causes power to decrease, which tends to return the plasma impedance to its initial value. Current sources add naturally in parallel, enabling combination of units to achieve higher delivered power, with master/slave (M/S) mode operation.


The figure below is a block diagram of a current-fed pulsed power supply with voltage limiting. A current source feeds the plasma, so the plasma determines its discharge voltage; the result is increased plasma stability compared to a voltage source approach.


The switch controls the power to the plasma. The switch is opened to initiate current flow to the plasma, and closed to end the flow of current to the plasma. A voltage limiter clamps the peak voltage of the output. Notably, the voltage limiter only consumes, or sinks, current. It does not source power or provide any current to the load. A current source will try to provide whatever voltage is required to make the current flow. This is beneficial at the beginning of the pulse, where the cabling inductance and also the equivalent inductance from the electron current in the magnetron racetrack must be overcome to establish the full desired current level in the pulse. In this transition period, the plasma process cannot accept the full current from the current source. If the current source tries to provide too high a voltage, damage to the sputtering magnetron, cables, connectors, or power supply could occur. That is why it is necessary to limit the voltage with a circuit to “catch” the current until the plasma process is able to accept the full current; that is the role of the voltage limiter.


In a future post, I’ll explain in more detail how this circuit works to drive the plasma through realistic industrial cables.


*This content refers to AE-proprietary technology.
