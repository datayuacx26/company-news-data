---
schema_version: "1.0.0"
document_id: "0ab5976a08ba563d876ddea8e1faf9c11573ab783b5fe36c222fbaa6ab80f5e6"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2023/01/inductively-coupled-noise-and.html"
published_at: "2023-01-23T13:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:63589c7b0da6d17c3852ece65fb25cc5fb370c9608cf865ee3ebe2b225fc6755"
---

# Inductively Coupled Noise and Resistively Coupled Noise: A Signal and Power Integrity Tutorial

Figure 6. Measuring the inductively coupled noise
on the victim trace adjacent to the aggressor
signal with no gap and separated by a gap.
The inductively coupled noise is reduced by
about 40% on the victim trace separated by a gap.
This is a small impact.


The following is excerpted from Professor Eric Bogatin's article in the Signal Integrity Journal,[The Case for Split Ground Planes](https://nam10.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.signalintegrityjournal.com%2Farticles%2F2982-the-case-for-split-ground-planes&data=05%7C01%7Cwtyler%40horizonhouse.com%7C5ed90a35003747eb05e508db09e31fe2%7C85f2fc98a3c44064815d1efc67da1928%7C0%7C0%7C638114643337382115%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=j%2BfqAZUuEKOAzlr5Vd5bkNCXxXh03S%2FQNQ1vOSN9YuM%3D&reserved=0) *.* Reprinted by permission of Signal Integrity Journal.


This section continues from the discussion on[Return Current at Low Frequency](https://blog.teledynelecroy.com/2023/01/return-current-at-low-frequency-signal.html) .


. . .


### Inductively Coupled Noise


In a plane, at frequencies below about 10 kHz, return currents will not flow under the signal path, but will spread out in the return plane. Above 10 kHz, the return currents are localized under the signal paths.


When we have two adjacent signal paths that are over a wide, continuous plane, they will show inductive cross talk at high frequency. Even with minimal overlap of the return currents, there is still loop mutual inductance between the two signal-return paths. This inductive noise is driven by the changing current, the dI/dt, in the aggressor signal-return path, which will get smaller at lower frequency.


An aggressor signal with a transient, short rise time current edge, will result in a noise signature on an adjacent victim line with a derivative of the aggressor current. The inductive noise would only appear synchronous with the switching current edge. This is why we call this sort of inductively generated noise “switching noise” since it only occurs when signals switch transition levels. As the current change drops off, with a lower slope, the inductive cross talk drops off until it is below a measurement threshold.


This behavior was demonstrated in a simple board. In a two-layer board, we constructed six parallel, identical microstrip traces. One was the aggressor. Its far end was shorted to ground. A 2 kHz square wave of 120 mA peak to peak current was transmitted down the aggressor. The rise time was about 9 nsec, but the current was at a constant value for the rest of the period.


There were two victim traces symmetrically on either side of the aggressor. Between the aggressor and one of the victim lines, a gap in the return plane was cut. This isolated the return currents from the aggressor. They were unconstrained to one victim line, be eliminated from flowing under the other victim trace.


We expect to see switching noise on the adjacent victim trace that lasts only during the 9 nsec of the rise time. The rest of the period should show no switching noise. The measured switching noise on the two victim traces shows the impact from the gap in the return plane. Figure 6 shows the measurement set up of the two configurations and the measured inductively coupled cross talk on the two victim traces.


We see the signature of the switching noise as the derivative of the current edge. From the measured peak crosstalk, on the order of 5 mV in this example, the rise time, and the current peak, we can estimate the loop mutual inductance between the aggressor and the victim. With no gap, this is about 0.4 nH of loop mutual inductance. On the other side of the gap, it is reduced to about 0.25 nH. The gap redistributed the return currents and did reduce the loop mutual inductance to the victim trace on the other side of the gap. But it was by a small amount.


### Low Frequency Resistive Coupled Cross Talk


At low frequency, when the return currents spread out, they will create a voltage drop distribution in the return plane due to the resistance in the plane. With a typical resistance in the return path on the order of 1 mohm, and currents on the order of 100 mA, this is a voltage drop between one region of the return plane and another on the order of 100 uohms, for example. This voltage drop due to the low frequency, DC currents on the plane, would appear as a voltage difference between the signal and local return plane on the victim path. It would show up at low frequency and last into DC. However, the magnitude of the resistively coupled noise might be orders of magnitude lower than the inductively coupled noise.


If there were a parallel gap in the return plane between the aggressor and victim traces, the impact on the inductive cross talk would be small. However, the parallel gap would prevent DC currents from the aggressor’s return current from flowing under the victim trace and would eliminate the already small resistively coupled cross talk.


Figure 7. The measured voltage noise on the
victim trace with a scale of 20 uV/div. The
raw measurement in one acquisition shows
the oscilloscope amplifier noise which is
reduced with 250 acquisitions averaged
together. Note the switching noise which
occurs at the edges of the square wave
is just visible on this time base scale.


The resistive cross talk can be measured during the part of the square wave when the current is constant. Figure 7 shows the same measurement of the voltage on the victim trace with no gap between it and the aggressor signal, during the entire square wave of current, but on a much higher resolution voltage scale.


This is a very difficult measurement because the resistively coupled cross talk is so small. With no averaging, the cross talk is smaller than the 100 uV rms amplifier noise of the Teledyne LeCroy WavePro HD, 12-bit scope. To reduce the random noise, we have to average consecutive acquisitions, triggering the scope with the function generator’s square wave. The random noise decreases with the square root of the number of averages, but the cross talk synchronous with the function generator, stays the same.


We see the very clear DC signature of the resistively coupled noise on the victim trace. Its magnitude is about 120 uV peak to peak. The small offset of about 20 uV is the DC offset of the scope’ amplifier. This 120 uV of resistively coupled noise for a peak-to-peak current of 120 mA corresponds to a coupled resistance in the ground plane of about 120 uV/120 mA = 1 mohm of resistance, or the resistance in about 2 squares of ground plane for this board.


This 120 uV of resistive noise is due to the overlap of the return currents of the two conductors, roughly 1 inch apart, with 120 mA of aggressor current, passing through the 1 mohm of overlapping plane resistance. This is the cross-talk noise which we would want to eliminate with a split ground plane.


#### Also see:


[Return Current at Low Frequency](https://blog.teledynelecroy.com/2023/01/return-current-at-low-frequency-signal.html)


[Signal Return Paths](https://blog.teledynelecroy.com/2023/01/signal-return-paths-signal-and-power.html)


[Transmission Lines for Oscilloscope Users, Part 1](https://blog.teledynelecroy.com/2022/01/transmission-lines-for-oscilloscope.html)


[Go Back to School on Signal Integrity](https://blog.teledynelecroy.com/2014/08/go-back-to-school-on-signal-integrity.html)
