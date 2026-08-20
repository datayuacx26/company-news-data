---
schema_version: "1.0.0"
document_id: "678bc303aa4e5e2759f05d1473ddd96b02f6b4f6be86b90325cb9ee65d0ccb70"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2022/11/sdaiii-and-qualiphy-software.html"
published_at: "2022-11-14T13:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:34d893b3b16b6e04080205737e32cc8ab8598ddc9d3daca2e9980cb3dff9febc"
---

# SDAIII and QualiPHY Software: Oscilloscope Eye Diagrams for Compliance and Debug

Figure 1. SDAIII enables eye diagrams and eye
measurements of four lanes of streaming data.


Besides the[serial TDME and TDMP options](https://blog.teledynelecroy.com/2022/11/usb-serial-decode-software-usb-eye.html) discussed earlier, there are other ways to generate eye diagrams on your Teledyne LeCroy oscilloscope for compliance testing and debug.


### SDAIII Serial Data Analysis Software


[SDAIII](https://teledynelecroy.com/options/productseries.aspx?mseries=405&groupid=103) offers the most comprehensive eye diagram capabilities for Teledyne LeCroy oscilloscopes, with tools for optimizing the displayed eye that are especially useful to high-speed serial data analysis.


SDAIII eye diagrams are created without using a trigger. This is done using a phase locked loop (PLL) to implement software clock recovery to learn the clock period. Once the clock period is determined, the data record is divided into segments somewhat larger than the clock period. The samples are positioned relative to the recovered clock edge times, and the segments are overlayed in a persistence display by aligning the clock delimited boundaries. Since the unit interval is determined from the recovered clock and not from the oscilloscope's trigger position, this method does not include jitter from any trigger circuit.


For serial data protocols supporting spread spectrum clocking to reducing EMI levels, the PLL clock recovery technique has the advantage of being able to track the small shifts in the clock period and compensate for it. This results in the cleaner eyes in SDAIII.


With the multi-lane[SDAIII-CompleteLinQ](https://teledynelecroy.com/options/productseries.aspx?mseries=405&groupid=103) option, you can simultaneously generate up-to-four eye diagrams to compare upstream and downstream lanes of data, or the signal at both the transmitter and receiver, as shown in Figure 1.


The[Eye Doctor II](https://teledynelecroy.com/options/productseries.aspx?mseries=234&groupid=103) capabilities included with SDAIII-CompleteLinQ allow you to see the eye with or without different equalization, emphasis or deemphasis settings, or following the de-embedding of cables, probes and fixtures. Be aware that signal processing for these functions utilizes tapped delay line filtering and requires minimum signal lengths to implement. The number of unit intervals required is either two or eight UI, varying with specific settings. If the signal length is too short, a warning appears in the oscilloscope message field.


### QualiPHY Compliance Test Software


Figure 2: A sample QualiPhy report showing the
total jitter and eye diagram calibration
for USB4-Tx-Rx compliance testing.


QualiPHY® test suites automate the creation of eye diagrams for each test that requires them as specified by CTS for the standard. Usually, an installation of SDAIII is required to provide the eye diagramming capabilities, while the QualiPHY software communicates the proper settings to SDAIII for the eye diagram views and measurements. For standards like PCI Express® that use SigTest or other third-party software to generate and measure eye diagrams for compliance testing, QualiPHY will interface with those applications as needed.


Optional settings within QualiPHY enable you to pause the test sequence to view the resulting eye diagrams and measurements, or you can simply allow the tests to proceed to the final report of Pass/Fail results, interacting with the software only as required to change cables or the test mode of the DUT.


A typical QualiPHY test report showing eye diagram calibration results for USB4 is shown in Figure 2.


Teledyne LeCroy oscilloscope eye diagram capabilities offer a simple way to measure the signal integrity of serial data communications, whether the eye diagrams and measurements of packetized data in the serial TDME options, or the more sophisticated analysis available in SDAIII.


#### Also see:


[Serial Trigger, Decode, Measure/Graph and Eye Diagram (TDME) Software: Oscilloscope Eye Diagrams for Debug](https://blog.teledynelecroy.com/2022/11/usb-serial-decode-software-usb-eye.html)
