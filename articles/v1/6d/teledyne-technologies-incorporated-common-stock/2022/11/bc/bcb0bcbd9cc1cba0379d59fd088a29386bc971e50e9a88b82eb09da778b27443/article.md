---
schema_version: "1.0.0"
document_id: "bcb0bcbd9cc1cba0379d59fd088a29386bc971e50e9a88b82eb09da778b27443"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2022/11/usb-serial-decode-software-usb-eye.html"
published_at: "2022-11-11T23:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:d055df5db53afe4139e28914f908ca648ea310488bc51377fc3cdb85038b4cf7"
---

# Serial Trigger, Decode, Measure/Graph & Eye Diagram (TDME) Software: Oscilloscope Eye Diagrams for Debug

Figure 1. Two eye diagrams generated from


three active USB serial decoders.
Click any image to enlarge it.


The eye diagram is a general-purpose tool for analyzing the signal integrity of serial digital communications signals. It shows the effects of additive vertical noise, horizontal jitter, duty cycle distortion, inter-symbol interference, and crosstalk on a serial data stream. The vertical opening of the eye is affected by these elements, as well as gain differences between devices on the bus, so that the more problems with signal integrity, the more “sleepy” the eye appears. A wide open eye is indicative of good signal integrity.


It is commonplace to use an oscilloscope with decoder software to analyze the health of serial data streams, where the combination of the electrical waveform and the link layer decoding shows if and where the protocol breaks down at the physical layer, but an eye diagram can better show the *degree* of signal interference that may be impacting the serial logic—especially if it could be generated for particular devices or packets.


### What's in a Name?


Teledyne LeCroy has adopted the convention of using a key in the name of our[serial data trigger and decode products](https://teledynelecroy.com/options/default.aspx?categoryid=12&groupid=88&capid=102&mid=506) that tells you what capabilities they offer. The “M **E** ” or “M **P** ” in the name of a Teledyne LeCroy serial decode option (e.g.,[CAN FDbus TDM E](https://teledynelecroy.com/options/productseries.aspx?mseries=474&groupid=88) or[USB4-SB TDM P](https://teledynelecroy.com/options/productseries.aspx?mseries=684&groupid=88) ) refers to " **M** easure/Graph and **E** ye Diagram" or " **M** easure/Graph and **P** hysical Layer Tests." These options can quickly generate and measure an eye diagram of the decoded input signal for basic signal integrity testing.


### Serial Decoders with Measure/Graph and Eye Diagram Capability


All serial decode oscilloscope options create eye diagrams in accordance with the governing standards. They can be generated from NRZ or PAM signals and will show either single or multiple eyes, accordingly.


The basic concept for all the standards is to acquire a long data record and determine the clock rate of the acquired data so that the edge locations can be accurately located. For data protocols that use non-continuous bit streams in the form of data packets, the clock is determined as the mean clock period for each packet. Using the recovered clock period, the long record is sliced into segments that are slightly longer than the clock period, or Unit Interval (UI). These UIs are overlapped in a persistence display to create the eye (Figure 2).


Figure 2. To create an eye diagram, a long acquisition is segmented into short records with a duration slightly longer that the clock period of the serial data stream. The short records are overlaid into a persistence display forming the eye.


The persistence display may use color-graded or analog (monochrome) persistence, according to the user's selection.


With color-graded persistence, each pixel is given a color based on the pixel's relative population and the selected Eye Saturation user setting. The color palette ranges from violet (least often occurring) to red (most often occurring).


With analog persistence, all pixels are the same color, but the pixel intensity (brightness) is based on the pixel’s relative population and the eye saturation setting. This mimics the relative intensity that would be seen on an analog oscilloscope.


Although you can generate only one eye diagram per decoder, up-to-four decoders of the same (or different) sources can be activated at once, enabling you to generate and display multiple eye diagrams. Figure 1 above shows three active decoders, including two eye diagrams generated from USB4 Gen3 decodes and a USB4-SB (sideband) decode.


### Eye Mask Testing


Figure 3. The eye diagram for a USB-PD decode
showing a mask test. Mask violations, or ‘hits’,
are marked with red circles.


Basic eye mask testing is available, showing the number and placement of eye hits. Standard masks for most protocols are included, or you can upload and use your custom mask files. Mask testing gives you an immediate, visual indicator of whether the signal is compliant with the standard. Figure 3 shows an eye diagram generated from a USB-PD decode with an eye mask test.


###


### Eye Diagram Measurements


Several standard eye diagram measurement parameters provide a way of quantifying the eye opening. The two foremost measurements, included with all "ME" decoders, are Eye Height and Eye Width.


**Eye Height** is a measurement of the minimum vertical eye opening; basically, a determination of the signal to noise ratio. The measurement is made by taking the difference of the means of the one and zero levels then subtracting three times the standard deviation of each level.


**Eye Width** gives an indication of the total horizontal jitter in the signal. Like the Eye Height, the measurement is statistical in nature. The horizontal histograms of two adjacent crossing points are used to determine the mean and standard deviations of crossing times. Three times the standard deviation of each distribution is subtracted from the difference of the two mean values.


For multi-level PAM signals, these measurements can be applied to any eye individually.


### Filtering Eye Diagrams and Eye Measurements


Figure 4. Eye Height and Eye Width are the foremost parameters used to measure the eye opening, assuring that logical 1 and 0 can be easily discerned.


All Teledyne LeCroy decoder software features an interactive table of decoder results. Individual columns of this table can be filtered to show only results from a particular device ID or containing a particular data value.


Because the eye is based on a decoding of the packetized signal, when the table is filtered, the eye diagrams are regenerated from only those UIs that match the filter criteria, enabling you to immediately see the signal integrity associated with particular types of packets. Any eye measurements that have been applied to the eye are also recalculated to show the results for only those UIs.


The "Apply to Zoom" feature also can be used to redraw the eye and recalculate eye measurements using only those UIs that are included in region of the source zoom. Simply touch a row of the table to "zoom" to that packet, and immediately you will see the eye and measurement results for only the highlighted area of the table representing the selected packet.


Figure 5. "Apply to Zoom" redraws the eye and recalculates measurements as
different packets within the same acquisition are selected from the table.


### Serial Decoders with Physical Layer Tests


Serial decoders sold with “M **P** ”— **M** easure/Graph and **P** hysical Layer Test—capability feature more robust eye diagramming and measurements designed to meet the specifications of the PHY tests prescribed by the standard. You will have many more eye measurements and options for how the eye is generated. For example, in some cases, the eye can be generated as seen at either the transmitter or receiver, but as with the “ME” options, only one eye can be generated at once per decoder.


Although no serial decoder can measure eye diagrams with sufficient rigor for compliance testing (our[QualiPHY® (QPHY) products](https://teledynelecroy.com/options/default.aspx?categoryid=12&groupid=140&capid=102&mid=506) do that), they are an excellent pre-compliance test of signal integrity, and viewed alongside the decoding make quick work of debugging signal integrity problems.


#### Also see:


[Oscilloscope Testing of 10Base-T1S Automotive Ethernet Signal Integrity](https://blog.teledynelecroy.com/2022/10/oscilloscope-testing-of-10base-t1s.html)


[TDME Primer: Serial Trigger and Sequence Mode Sampling](https://blog.teledynelecroy.com/2021/03/tdme-primer-serial-trigger-and-sequence.html)


[TDME Primer: Selecting Sample Rate for Serial Bus Analysis](https://blog.teledynelecroy.com/2021/03/tdme-primer-selecting-sample-rate-for.html)
