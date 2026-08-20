---
schema_version: "1.0.0"
document_id: "79a80e9e6743ef5568790017a11a91ed23a4033bad61e3be766f82e10ca15421"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2023/01/return-current-at-low-frequency-signal.html"
published_at: "2023-01-18T13:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:d16fd8ee5486e2b5d5b7a18df113103bf3eea7316ab8cce28545c5fb62418eb1"
---

# Return Current at Low Frequency: A Signal and Power Integrity Tutorial

Figure 3. Specially configured coax cable with
the front and back of the shield shorted together.


The following is excerpted from Professor Eric Bogatin's article in the Signal Integrity Journal,[The Case for Split Ground Planes](https://nam10.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.signalintegrityjournal.com%2Farticles%2F2982-the-case-for-split-ground-planes&data=05%7C01%7Cwtyler%40horizonhouse.com%7C5ed90a35003747eb05e508db09e31fe2%7C85f2fc98a3c44064815d1efc67da1928%7C0%7C0%7C638114643337382115%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=j%2BfqAZUuEKOAzlr5Vd5bkNCXxXh03S%2FQNQ1vOSN9YuM%3D&reserved=0) *.* Reprinted by permission of Signal Integrity Journal.


This section continues the discussion in[Signal Return Paths](https://blog.teledynelecroy.com/2023/01/signal-return-paths-signal-and-power.html) of the equation,


Z = R + j𝛚L


Where:


Z is the loop impedance of the current loop path,


R is the series resistance of the loop and


L is the loop inductance of the path.


. . .


At low frequency, when the loop impedance is dominated by the R term, the current distribution in the return plane is NOT driven by the loop impedance, it is driven by the loop resistance. In the signal path, the current will spread out uniformly as any filament path in the signal conductor will have roughly the same resistance.


But the current filaments in the return path with the lowest R will be those which are shortest. This means that return currents will take the shortest paths, independent of the signal paths. As frequency increases, the return current will redistribute to transition from the path of lowest R to the path of lowest L.


This was demonstrated in a simple experiment. A coax cable was shorted at the far end so that a DC current loop, driven by a function generator, would flow from the signal conductor and back through the return. At the front of the coax, the shield, which carries the return current, was shorted between the front of the coax and the back end of the coax. This is shown in Figure 3.


Figure 4. Measured return current in the
shunt path dropping off above 10 kHz,
measured with a Teledyne LeCroy
WavePro® HD oscilloscope.


At DC, the return current will flow through the shunt between the front and back of the shield, which is a lower resistance path, instead of flowing all the way down the shield to the far end where the signal and return currents are shorted together.


To measure the current that flows through this path, a Hall effect current clamp was placed around this shunt path. This measures the current flowing through this specific path. The function generator was used to drive a constant 60 mA amplitude of sine wave current through the coax cable and the frequency swept from 1 kHz to 10 MHz. The currents were measured with the Hall effect current probe and a[Teledyne LeCroy WavePro HD](https://teledynelecroy.com/waveprohd/) 12-bit, 8 GHz bandwidth scope.


At low frequency, all the return current flowed through the shunt path. But, as frequency increased, less current flowed through the shunt and more current flowed along the higher resistance, but lower loop inductance path of the coax shield, with the return current in close proximity to the signal current. Figure 4 shows the measured current amplitude in the signal-return loop at the far end, flat with frequency, and the measured current amplitude in the shunt, which drops off with a 1-pole response, above about 10 kHz.


Figure 5 Measured step current response through
the shunt path with a 10-90 rise time of 32 usec,
measured with a Teledyne LeCroy
WavePro HD oscilloscope.


This illustrates that above about 10 kHz, all the return current will always flows in the path directly adjacent to the signal current to reduce the loop inductance of the signal-return path. But, equally important, the return currents below about 10 kHz will always flow in the path for lowest resistance.


The transient step response of the current flowing through the shunt path will be a 1-pole response with a pole frequency about 10 kHz. This is an effective RC time constant of about 16 usec. This would result in a 10-90 rise time of about 32 usec. This is what is measured in the step response of the current through the shunt path, shown in Figure 5.


#### Also see:


[Signal Return Paths](https://blog.teledynelecroy.com/2023/01/signal-return-paths-signal-and-power.html)


[Transmission Lines for Oscilloscope Users, Part 1](https://blog.teledynelecroy.com/2022/01/transmission-lines-for-oscilloscope.html)


[Go Back to School on Signal Integrity](https://blog.teledynelecroy.com/2014/08/go-back-to-school-on-signal-integrity.html)
