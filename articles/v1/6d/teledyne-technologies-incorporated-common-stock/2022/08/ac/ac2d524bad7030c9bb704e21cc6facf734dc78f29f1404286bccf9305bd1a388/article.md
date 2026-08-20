---
schema_version: "1.0.0"
document_id: "ac2d524bad7030c9bb704e21cc6facf734dc78f29f1404286bccf9305bd1a388"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2022/08/using-tf-usb-c-hs-to-debug-usb-32-phy.html"
published_at: "2022-08-08T12:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:6c10401d5fd2008b20580bf87ca6d9de6369d16170394bb5b9473783f9067a04"
---

# Using TF-USB-C-HS for USB 3.2 PHY-Logic Layer Debug

Figure 1. USB 3.2 electrical decoding with ProtoSync
view of protocol packets, captured using TF-USB-C-HS.
Click on any image to enlarge it.


In a USB-C connector, link training for USB 3.1/3.2 is negotiated using an LTSSM (Link Training and Status State Machine) through electrical signaling on the[TX1/RX1 and TX2/RX2 connector pins](https://blog.teledynelecroy.com/2022/06/what-happens-when-you-connect-usb-c.html) . Link training must be completed on the link before high-speed data transactions can occur. One problem you might encounter during link training is a failure to train to USB 3.2 Gen 2 specifications. Teledyne LeCroy customers report that most system-interoperability problems are caused by either link-training or sideband-negotiation failures, which in turn can result from an electrical problem, a digital problem or a combination of both.


[TF-USB-C-HS](https://teledynelecroy.com/options/productdetails.aspx?modelid=11678&categoryid=11&groupid=170) enables you to probe all points on the USB-C connector to measure and analyze live links. The insertion-loss profile of the included cable and coupon is tuned to be the equivalent of a golden 0.8-m USB Type-C cable, so you can replace a 0.8-m cable with the coupon and not experience any difference in link performance. The coupon also has a loop to allow a current probe to make load-current measurements, and the HS version is compatible with Teledyne LeCroy[DH Series](https://teledynelecroy.com/probes/dh-series-differential-probes) probes for making high-speed differential measurements.


We'll show how to trigger, acquire and decode to find problematic link training packets synchronous with the physical-layer electrical waveforms, so you can tell if the source of your interoperability problem is electrical, logical or both.


### Equipment


Required are:


- 4 Ch, ≥16 GHz, 40 GS/s real-time oscilloscope such as SDA/[WaveMaster 8 Zi-B](https://teledynelecroy.com/oscilloscope/wavemaster-sda-dda-8-zi-b-oscilloscopes) or[LabMaster 10 Zi-A](https://teledynelecroy.com/oscilloscope/labmaster-10-zi-a-oscilloscopes)
- [USB3.2 D](https://teledynelecroy.com/options/productseries.aspx?mseries=334&groupid=88) decoder software option
- 16 or 20 GHz differential probe (2 each), such as[DH16-PL](https://teledynelecroy.com/probes/probemodel.aspx?modelid=11369&categoryid=3&mseries=608&capid=102&mid=508) or[DH20-PL](https://teledynelecroy.com/probes/probemodel.aspx?modelid=11370&categoryid=3&mseries=608&capid=102&mid=508)
- [Voyager M310P, M310e or M4x](https://teledynelecroy.com/protocolanalyzer/usb) USB Protocol Analyzer


Recommended are:


- [ProtoSync option](https://teledynelecroy.com/options/productseries.aspx?mseries=287&groupid=88) for USB (requires installation of USB Protocol Suite software)
- [SDAIII-CompleteLinQ option](https://teledynelecroy.com/sdaiii/) for eye diagram analysis of the live link


### Probing with the TF-USB-C-HS Test Coupon


The TF-USB-C-HS is connected between the DUT and the Exerciser/Analyzer ports of a Voyager Analyzer (M310e/M310P or M4x), using the included USB-C cable (Figure 2).


Figure 2. Test setup for USB 3.x PHY-logic debug using protocol trigger.


Signals are input to the oscilloscope by way of the test coupon:


- TX1 is input to A-row Upper Deck C1 using a DH Series differential probe.
- TX2 is input to A-row C2 using same.
- Optionally, sideband signals CC1/CC2 and Vbus can be monitored using passive probes on the B-row connectors of the oscilloscope.
- The Voyager trigger out signal is connected to oscilloscope Ext In.


### Protocol Analyzer Triggering


Figure 3. Voyager protocol analyzer view of the polling state
packets of the USB 3.2 LTSSM.


Link training issues usually first show up while doing Link Layer or USB Type-C-specific tests using a Voyager protocol analyzer. The protocol analyzer has a rich set of Link Training Packets (LTP) and higher layer analysis of the USB 3.2 LTSSM (Figure 3). In order to trigger an oscilloscope on the area of interest, is necessary to first identify where in the protocol trace the problem is occurring, then set up the protocol analyzer to send a trigger pulse to the oscilloscope at that time.


For instance, during Link training, you can send a trigger pulse on SCD1, SCD2, LBPM or an LMP (Link Management Packet). As each trigger event occurs in the signal, the event is marked on the protocol trace and a pulse is sent from the Trigger Out connector of the protocol analyzer to the Ext In trigger input of the oscilloscope, enabling it to capture the electrical signals at virtually the same time.


### Oscilloscope Triggering and Decoding


Set up the oscilloscope for an Edge trigger using the Ext In Source.


Set up the USB 3.2 D software for Gen2x2 decoding, with One Differential Probe selection. (Note that our example uses acquisitions of Lane0 and Lane1 saved to memories M1 and M2. When probed live, these signals would be on C1 and C2.)


Figure 4. Setup for USB 3.2 Gen2x2 decoder.


After acquiring, use the decoder Search or Filter feature to find the packet type of interest, which appears in the Type column of the decoder result table. Clicking that row of the table will zoom to the same time in the electrical trace. If you also have the ProtoSync display open (as in Figure 1), you'll also see exactly which protocol packet is involved.


### Eye Measurements


Figure 5. Eye diagrams show signal integrity of live link
during negotiations.


A possible source of link training errors is poor signal quality coming from the transmitter. The TF-USB-C-HS provides a convenient way to perform serial data analysis measurements on the transmitters while in a live link. This is not a substitute for physical layer compliance testing, but it can be used to verify that the expected signal quality is coming from the two USB 3.2 transmitters, TX1 and TX2.


If you have installed the SDAIII-CompleteLinQ oscilloscope option, a complete set of signal integrity tools is available, including jitter and eye diagram measurements and plots. You can use the eye diagrams to look at the electrical performance of the live link. If the signal is not optimized for the cable it is driving, you may see an eye diagram that has too much or too little equalization. The ISI Plot allows you to evaluate the effect of different pre-emphasis presets on the transmitted eye.


Multi-lane testing with SDAIII-CompleteLinQ also allows you to see both transmitter signals side-by-side in order to determine if there are lane-dependent SI issues that might be contributing to poor link performance.


You can download these instructions in our PDF application note, "Using TF-USB-C-HS to Debug USB 3.1/3.2 PHY-Logic and Link Training."


#### Also see:


[What Happens When You Connect a USB-C Cable](https://blog.teledynelecroy.com/2022/06/what-happens-when-you-connect-usb-c.html)


[USB4 Alt-Mode Testing: DP-AUX and USB-PD](https://blog.teledynelecroy.com/2021/12/usb4-alt-mode-testing-dpaux-and-usb-pd.html)


[Testing DisplayPort 2.0 vs. USB4 over USB Type-C](https://blog.teledynelecroy.com/2021/12/testing-displayport-20-vs-usb4-over-usb.html)
