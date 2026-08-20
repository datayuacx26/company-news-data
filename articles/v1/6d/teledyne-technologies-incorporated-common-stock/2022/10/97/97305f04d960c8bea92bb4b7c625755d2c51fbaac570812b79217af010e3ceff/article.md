---
schema_version: "1.0.0"
document_id: "97305f04d960c8bea92bb4b7c625755d2c51fbaac570812b79217af010e3ceff"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2022/10/oscilloscope-measurements-of-10base-t1s.html"
published_at: "2022-10-05T16:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:167b7766ce897e42a8194aecc089c512ffb14b2c2224c536d8daa22408951241"
---

# Oscilloscope Measurements of 10Base-T1S Automotive Ethernet PLCA Cycle Timing

Figure 1. Color-coded decoding of 10Base-T1S
stream makes it easy to measure timing between
signal elements. Click on any image to enlarge.


The[10Base-T1S Trigger-Decode](https://teledynelecroy.com/options/productseries.aspx?mseries=680&groupid=88) (TD) and[10Base-T1S Trigger, Decode, Measure/Graph & Eye Diagram](https://teledynelecroy.com/options/productseries.aspx?mseries=681&groupid=88) (TDME) options enable Teledyne LeCroy oscilloscope users to trigger on and decode Ethernet control and payload data from 10Base-T1S Automotive Ethernet signals. The decoding is color-coded to provide fast, intuitive understanding of the relationship between message frames and other time-synchronous events. Knowing the location of the various protocol elements makes it easy to measure Physical Layer Collision Avoidance (PLCA) cycle timing using either standard oscilloscope tools, or special serial bus measurements included with the TDME options.


PLCA cycle timing is measured to assure interoperability of the attached nodes in a 10Base-T1S mixed-segment, multidrop bus. This class of tests measures the timing between events on the bus relative to a specific bus event, usually the BEACON signal initiated by the Master node.


Let’s look at a simple example of a 10Base-T1S network with two nodes, the Master (Node 0) and a device (Node 3). The acquired waveform is shown in Figure 2, decoded using the 10Base-T1S TDME option. The top grid shows the complete acquisition, which consists mostly of BEACON signals over a record of twenty-five million samples. Toward the end of the acquisition are two packets from the other nodes. The table at the bottom of the screen lists all the elements decoded in the full acquisition.


### Measuring BEACON Duration


Figure 2. Cursors on zooms precisely measure
duration of BEACON signal.


Once the desired entry is located in the table, clicking on that row will bring up a horizontally expanded zoom trace showing the selected entry in detail. In Figure 2, the packet from the Node ID # 3 is selected and expanded in zoom trace Z1, the second grid from top, making it easier to read the decoding. The duration of the BEACON signal can be estimated by using the Time column of the table, which reports the time at the beginning of the decoded element. Taking the difference between the start of the BEACON and the start of the following Silence, the duration of the BEACON is 2.1 µs.


Horizontal relative cursors can also be used to measure timing on the decoded signal with better precision. A good way to assure the best accuracy in a cursor measurement is to first zoom the trace being measured. Cursors are initially placed about the BEACON signal on the decode trace Z1 using either the front panel Cursor button or the Cursor drop-down menu. Then, further zooming the Z1 trace into Z3 allows the cursors to be moved and placed with much greater accuracy at the beginning and end of the BEACON. Ideally, the cursors should be placed at the logic switching threshold. The cursor time readout appears in the lower right corner of the screen under the timebase annotation box, reading 2.0820 µs. This is consistent with the 10Base-T1S specification that the BEACON duration be 20 clock cycles. At 10 MHz, this works out to 2 µs.


### Measuring BEACON to Packet Timing


Figure 3. Cursors on two zooms used to measure
time delay from end of BEACON to start of packet.


A more commonly required measurement is the time from the end of the BEACON to the beginning of a packet sent by a node. This represents the time latency to the Transmit Opportunity (TO) for the node, in our example Node ID# 3. Figure 3 shows the measurement setup.


The Time column of the table can be used to estimate the delay from the end of the BEACON to the beginning of the Node 3 packet. This is the difference between the start of the Silence that follows the BEACON and the Start of Stream Delimiter (SSD). The Silence begins at -84.2 µs and the SSD begins at -72.91 µs, a difference of 11.29 µs.


The cursors can be used as before, only this time, due to the length of the packet, two zooms are used to see both the end of the BEACON (Z3) and the beginning of the packet (Z4) with sufficient resolution to place cursors accurately. The significant measurement points on the zoom traces are aligned with the measurement points on zoom trace Z1. The measured time difference is 11.2128 µs.


Time differences between any fields of the decoded signal can be measured with cursors in this manner.


### Measuring Time Between BEACONs


Figure 4. Applying filters and serial bus measurement
parameters to measure time between BEACONs.


The 10Base-T1S TDME option includes 10 serial bus measurement parameters that can be applied to decoded serial data streams. Of particular interest for 10Base-T1S is the Delta Message parameter, which can be used to measure time between BEACONs. To do this, first filter the decoder table to show only Type BEACON by clicking the down-arrow symbol in the Type column header, then entering your filter criteria (e.g., Contains BEACON).


Once the table is filtered, open the Measure/Graph tab and set up the Delta Message parameter, using the active Decode *N* as the source. In Figure 4, the time measured between BEACON transmissions is a minimum of 1.2608 ms, maximum of 1.3228 ms, and mean value of 1.28738 ms.


The TDME Measure/Graph tab also allows you to optionally graph measurement results as a track, trend, or histogram. In this example the track function is selected and routed to math trace F1. Each measured delay value is plotted time synchronously with the acquisition in the F1 math trace.


#### Also see:


[Physical-Layer Collision Avoidance in 10Base-T1S Automotive Ethernet](https://blog.teledynelecroy.com/2022/08/physical-layer-collision-avoidance-in.html)


[10Base-T1S Automotive Ethernet vs. 10Base-T1L Industrial Ethernet](https://blog.teledynelecroy.com/2022/08/10base-t1s-automotive-ethernet-vs.html)
