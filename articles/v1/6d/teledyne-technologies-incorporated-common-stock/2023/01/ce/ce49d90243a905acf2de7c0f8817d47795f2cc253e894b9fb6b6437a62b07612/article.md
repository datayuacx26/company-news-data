---
schema_version: "1.0.0"
document_id: "ce49d90243a905acf2de7c0f8817d47795f2cc253e894b9fb6b6437a62b07612"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2023/01/eliminating-dc-resistively-coupled.html"
published_at: "2023-01-25T21:39:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:a3530815745c23f40ed01014d5423860ba7535756661cdaf4fe07a21f6d2d5f5"
---

# Eliminating DC Resistively Coupled Noise: A Signal and Power Integrity Tutorial

Figure 8. The measured voltage noise on the victim
trace, on the other side of the ground plane gap,
showing no resistively coupled cross talk on the
order of 10 uV, the noise floor of the measurement.


The following is excerpted from Professor Eric Bogatin's article in the Signal Integrity Journal,[The Case for Split Ground Planes](https://nam10.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.signalintegrityjournal.com%2Farticles%2F2982-the-case-for-split-ground-planes&data=05%7C01%7Cwtyler%40horizonhouse.com%7C5ed90a35003747eb05e508db09e31fe2%7C85f2fc98a3c44064815d1efc67da1928%7C0%7C0%7C638114643337382115%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=j%2BfqAZUuEKOAzlr5Vd5bkNCXxXh03S%2FQNQ1vOSN9YuM%3D&reserved=0) *.* Reprinted by permission of Signal Integrity Journal.


This section continues from the discussion on[Inductively Coupled Noise and Resistively Coupled Noise](https://blog.teledynelecroy.com/2023/01/inductively-coupled-noise-and.html) .


. . .


When we cut a gap in the return plane, there will be no DC current flow across the gap. There will be magnetic field coupling across the gap which is why we still see significant mutual inductance coupling between the aggressor and victim across the gap. The gap has only a small impact on this noise.


However, we would expect there would be no resistively coupled noise on the victim trace on the other side of the ground plane gap. In Figure 8, the resistively coupled noise is measured with the same scale and averaging as the noise on the victim line with no gap. The noise floor of this measurement is about 10 uV. To this level, there is no measurable resistively coupled noise, a significant reduction.


There is still inductively coupled switching noise which lasts for the rise time of the square wave, about 9 nsec. This noise is just barely visible with this time base of 100 usec/div. It is the initial spike at the edges of the square wave.


It is this small amount of resistively coupled cross talk which a gap in the ground plane would prevent. It keeps the DC currents, which will spread out from the signal paths, from flowing in the ground plane to induce a DC offset noise in the return path of other signals.


Generally, this amount of noise will be on the order of 100 uV, corresponding to 100 mA flowing through 1 mohm of coupled resistance. In an ADC with a 5 V reference and 15 bit resolution (plus sign), 1 bit would be a voltage level of about 5 V/32,000 = 150 uV. The DC coupled ground plane voltage noise could contribute about 1 least significant bit (LSB) level. Fluctuations in the 100 mA of ground currents, could be just at the sensitivity level of a 16 bit ADC in some cases. It would be noticeable in a 24-bit ADC.


One solution to reduce this noise would be to isolate either the high current paths or the sensitive signal paths using an isolating gap in the return plane, parallel with the signal conductors, making sure no signals crossed this isolating gap. This is the problem solved by a gap in the return plane.


If there are 100 A of DC current flowing, this ground plane noise could be 100 mV or more, but then other design considerations such as thicker copper, more ground planes and placement of the VRM in proximity to the load might be employed.


While a gap in the return plane would dramatically reduce the resistively coupled noise on analog signals where voltage noise on the order of 100 uV was important, there is a more effective way of reducing this sort of common noise on sensitive signals that is also robust and does not run the risk of inadvertently having signals cross the isolation gap.


### Differential Signaling also Eliminates the Resistive Coupled DC Cross Talk


Most applications which are sensitive to 100 uV of low frequency noise involve measuring low level signals from sensors or microphones. An important design guideline when measuring these sorts of voltage sources is to use a differential measurement.


If the sensor itself generates a differential signal or even a single ended signal, you would route a separate trace for both the high and low ends of the sensor, back to a differential receiver, such as an instrumentation amplifier. Even if the sensor is single ended, the ground connection to the sensor’s low side could be connected at only one point, either at the sensor or at the input to the differential receiver, rather at both ends.


The voltage difference between the high and low side of the sensor is brought back to the input of the differential receiver, without using the ground plane, which might have the common DC voltage drop coupled noise in it. This separate dedicated low trace would not have the DC current of the return plane traveling in it.


This principle is illustrated in a simple experiment. A TMP36, a voltage sensitive temperature sensor, was used as the sensor. At room temperature, it generates a DC voltage of about 730 mV. It is a single ended signal.


The output of this sensor was measured with a differential amplifier and a 16-bit ADC using the ADS1115. It was measured in two configurations, with the low side connected to the ADC using a common ground path and with a separate return line connecting the low side of the sensor to the low side input of the ADC.


While these measurements were being made, a 1 Hz square wave of current was passed through the common return path. The common resistance was increased to accentuate the coupled noise. When the 100 mA of ground current was sent through the ground path, a noise level of about 2 mV was generated in the ground path from the sensor to the ADC. This voltage noise appears in series with the low-level sensor voltage when the return path is used to connect the low side reference.


When the low side reference is carried in a separate, isolated trace, there is no impact on the differential signal from the DC noise in the return path. This result is shown in Figure 9.


Figure 9. The circuit set up for the measured analog voltage using the ground as the low side reference or a separate trace to the differential input. The measurements on the right show the ground noise on the signal ended signal, but no impact on the differential measurement.


The differential signal path from the sensor to the differential receiver, does not have the voltage drop of the ground plane in its path. The measured signal does not show any of the DC resistive cross talk in its signal. This is the way to route sensitive analog signals so they are not sensitive to very slight resistive cross talk from low level signals.


### Conclusion


The problem a split in the ground plane solves is reducing the very small low frequency resistive cross talk of return currents which spread out. This typically arises at frequencies below 10 kHz and is equivalent of a common, shared resistance on the order of a few squares of sheet resistance, on the order of 1 mohm.


If your application has very low-level analog signals which must be routed across a board and might be sensitive to these low-level sources of low-frequency noise, a better solution is to use differential signal routing and a differential receiver.


The risk of adding a split in the ground plane to fix this very small problem is the possibility of higher bandwidth signals inadvertently crossing this gap. This can result in a pathological problem which will easily cause a board to fail in a number of ways.


Except in the simplest of boards, the risk from including a gap in the return path, strongly outweighs the potential benefit. Carefully consider your engineering rationale to add a split in the ground plane and to reduce risk, consider alternative solutions.


. . .


#### Also see:


[Inductively Coupled Noise and Resistively Coupled Noise](https://blog.teledynelecroy.com/2023/01/inductively-coupled-noise-and.html)


[Return Current at Low Frequency](https://blog.teledynelecroy.com/2023/01/return-current-at-low-frequency-signal.html)


[Signal Return Paths](https://blog.teledynelecroy.com/2023/01/signal-return-paths-signal-and-power.html)


[Transmission Lines for Oscilloscope Users, Part 1](https://blog.teledynelecroy.com/2022/01/transmission-lines-for-oscilloscope.html)


[Go Back to School on Signal Integrity](https://blog.teledynelecroy.com/2014/08/go-back-to-school-on-signal-integrity.html)
