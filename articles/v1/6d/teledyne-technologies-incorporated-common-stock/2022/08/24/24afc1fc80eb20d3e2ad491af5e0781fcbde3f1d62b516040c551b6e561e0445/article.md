---
schema_version: "1.0.0"
document_id: "24afc1fc80eb20d3e2ad491af5e0781fcbde3f1d62b516040c551b6e561e0445"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2022/08/only-measuring-board-level-power-rail.html"
published_at: "2022-08-01T12:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:d9eded459488abec82803e1f10bcde15d39ec689c3d01d8e46506583094acf29"
---

# Signal and Power Integrity Tutorial: Power Rail Probing for Rail Compression

Figure 3. Equivalent circuit of a typical CMOS I/O
showing the connection from the on-die rails
and the board-level test points.


By Prof. Eric Bogatin,
Teledyne LeCroy Fellow


Excerpted by permission from the Signal Integrity Journal article,[Measuring Only Board-level Power Rail Noise May Be Misleading](https://www.signalintegrityjournal.com/articles/2790-measuring-only-board-level-power-rail-noise-may-be-misleading)


Continued from[Part 1](https://blog.teledynelecroy.com/2022/07/signal-and-power-integrity-tutorial-how.html) .


### Measuring Rail Compression on the Die


In most applications, we do not have access to the bare die when the chip is assembled on the circuit board. If the IC package has not been instrumented with special pass-through features connecting the rails on the die to board pins, we have to rely on a special trick. \[The use of a[quiet HIGH and quiet LOW](https://blog.teledynelecroy.com/2018/08/more-on-quiet-low-io-drivers-and-ground.html) \]


When the I/Os of a chip all share the same power and ground rails, which is often the case in small microcontroller devices, designated I/Os can be used as sense lines to measure externally the power rails on the die.


In digital CMOS outputs, the driver connects its output to either the Vdd or the Vss rails when its output is set to a HIGH or a LOW, respectively. This means that an I/O set as a HIGH or a LOW provides a direct connection to elsewhere on the board from which the Vdd or Vss rails can be measured, relative to the local ground on the board.


We call these special I/Os a quiet HIGH and a quiet LOW line. When they are set as a fixed value, their output voltage should not change. Any voltage variation on these lines is only due to noise either on the rail to which they connect or picked up somewhere along their signal-return paths. If these connections are designed as uniform transmission lines with a solid return plane, with other signals far away, the voltage measured at these test points will be dominated by the voltage of the on-die rail relative to the local board ground of the test point.


All the measurements of the test points on the board are done as single-ended. This means the measured voltage is the voltage of the test point relative to the local ground location where the oscilloscope probe’s ground makes contact with the board ground plane.


The voltage measured at the Vdd test point is not actually the Vdd on the die. It is the Vdd on the die relative to the board ground. If there is ground bounce noise on the Vss rail, its voltage, and the entire chip’s Vss rail, may bounce relative to the local board ground. This bounces the Vdd rail the same amount, relative to the board ground.


What we really care about is the voltage rail on the die between the Vdd and Vss rails. This is Vdd – Vss, with each of them measured relative to the same board ground reference point. We sometimes refer to this value as rail compression. It is how the voltage rail on the die compresses due to currents switching.


What we actually measure is the voltage on the test point relative to the local board ground plane. As long as there is no additional noise picked up on the test lines from where they leave the package to where they reach the test point, the measured voltage difference between the Vss and Vdd test points would be equal to the rail compression on the die.


The process is then to measure the quiet HIGH and quiet LOW test points when other I/Os switch. When the oscilloscope is triggered by one of the I/Os switching, the rail compression can be measured from the Vdd and Vss quiet lines....The rail compression can be displayed on the screen using an oscilloscope math function calculating Vdd – Vss.


Figure 4. The rail compression is
the difference between the
measured voltage of the


quiet Vdd and quiet Vss lines
relative to the local ground.


In this example, the Vdd power rail was 3.3 V. The measured rail compression during the time at which the I/Os were switching was as much as 1.2 V. This is huge. It is so large that it affects the output signal rise time and would have caused false triggering on other I/O lines. If this rail was also used by the core logic, bit errors would definitely be generated.


### On-die Rail Noise and Board-level Rail Noise


Using the quiet LOW and quiet HIGH sense lines gives a direct measure of the rail compression noise on the die itself. This is what is important, since it is this noise which will affect the correct functioning of the die and the impact from noise on false triggering.


But, suppose you had not instrumented the chip and did not know the voltage noise on the die itself. Suppose you only measured the voltage noise on the power rail on the board, relative to the local board ground. What would you see?


Also on this test board is a test point connected to the 3.3 V output of the LDO. At this point, we measure the voltage noise on the board. It is exactly the same net as the Vdd rail on the die, but it is located some distance away from the on-die test point, and on the other side of the low-pass filter from the source of the noise.


The on-die rail compression was a dip of 1.2 V, while the on-board voltage level changed by less than 0.025 V. All voltages are displayed at the same scale.


Figure 5. The on-die rail compression was a dip
of 1.5 V, while the on-board voltage level
changed by less an 0.025 V.
All voltages are displayed at the same scale.


On the oscilloscope trace in this figure, the measured on-board 3.3 V rail is about 0.025 V, a very small amount of noise when the I/Os switch while there is 1.2 V of compression on the die. This is because the noise on the die, the aggressor signal, has to get through the low-pass filter composed of the package lead inductance and the local decoupling capacitors to get to the LDO, the victim location. The pole frequency of the low-pass filter is about 1 MHz in this board. Since the rise time of this noise is about 1 ns, its bandwidth is about 350 MHz. Very little of this noise gets through the 1 MHz low-pass filter.


If we had measured the voltage noise on the board and seen just 0.025 V amplitude noise, we would have concluded that the power rail noise was very low and insignificant, not to worry. In fact, the actual on-die compression was more than one-third the entire power rail, large enough to affect robust functioning of the chip.


Without this sort of on-die measurement, you would have no idea your product was so close to failure, until some customer ran it through some operation that was sensitive to this large of an on-die noise level. Without the diagnostic of knowing how large the on-die power rail noise was, finding the root cause of the problem would be as difficult as hunting a snipe.


### Summary


It is important to think about an equivalent circuit model of your PDN so you can see in your engineering mind’s eye the low-pass filters between different nodes on the same net of your PDN. Measuring the power rail noise on the board is no indication of the noise on the die. Without a way of actually measuring the power rail voltage noise on the die, you have no idea how much noise is present.


If you use measurements of the noise on the board as an indication of the goodness of various design decisions, be careful you are not drawing false conclusions.


Implementing the technique of using a quiet HIGH and quiet LOW I/O pin is a simple way of opening up a small window onto what is happening with the power rail of your die.


***********************************************************************************


For more information, watch these two free webinars by Prof. Eric Bogatin:


[Practical On-die Power Integrity Measurements](https://go.teledynelecroy.com/l/48392/2021-05-03/874z78?&utm_source=website&utm_medium=blog&utm_campaign=21-12-07-power-integrity-masters-series-8&utm_content=__)


[Secrets To Successful Power Rail Measurements](https://go.teledynelecroy.com/l/48392/2018-12-18/79zd6q)


#### Also see:


[Situational Awareness: The Impact of the Interconnect](https://blog.teledynelecroy.com/2021/01/situational-awareness-impact-of.html)


[More on Quiet-Low I/O Drivers and Ground Bounce](https://blog.teledynelecroy.com/2018/08/more-on-quiet-low-io-drivers-and-ground.html)


[About Ground Bounce and How to Measure It](https://blog.teledynelecroy.com/2018/08/about-ground-bounce-and-how-to-measure.html)


[Making On-Die Power-Rail Measurements (Part III)](https://blog.teledynelecroy.com/2018/01/making-on-die-power-rail-measurements_24.html)


[Making On-Die Power-Rail Measurements (Part II)](https://blog.teledynelecroy.com/2018/01/making-on-die-power-rail-measurements_88.html)


[Making On-Die Power-Rail Measurements (Part I)](https://blog.teledynelecroy.com/2018/01/making-on-die-power-rail-measurements.html)


[Measuring Shared On-Die Power Rails](https://blog.teledynelecroy.com/2018/01/measuring-shared-on-die-power-rails.html)


[Setting the Stage for On-Die Power-Rail Measurements](https://blog.teledynelecroy.com/2018/01/setting-stage-for-on-die-power-rail.html)
