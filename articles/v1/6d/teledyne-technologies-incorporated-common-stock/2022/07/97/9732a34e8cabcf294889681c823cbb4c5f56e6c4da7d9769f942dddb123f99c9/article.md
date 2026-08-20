---
schema_version: "1.0.0"
document_id: "9732a34e8cabcf294889681c823cbb4c5f56e6c4da7d9769f942dddb123f99c9"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2022/07/a-tale-of-two-calibrations-vna-vs.html"
published_at: "2022-07-05T12:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:da1bfcdb63cdccbc50ef885ab71d385c8ec673b03ac075352fd6a85d699249ed"
---

# A Tale of Two Calibrations: Vector Network Analyzer vs. WavePulser 40iX

Figure 1: This sequence diagram of the
classic SOLT 2-path calibration shows
the order of connections required.


It was the best of S-parameter measurements, it was the worst of S-parameter measurements…and the difference was in the calibration. Calibrating a vector network analyzer (VNA) before making any measurements is required in order to reduce errors from imperfect channel matching, less than optimal directivity in the directional couplers and cable response issues. While VNAs are precisely calibrated at the factory, that calibration only extends to the front panel measurement ports. There will inevitably be drift on the internal paths over time. Also, any cables, adaptors or fixtures connected to the measurement ports must be characterized and de-embedded in order to make exact measurements of the device under test (DUT).


There are many possible calibration methods depending on the number of ports and paths being measured. For simplicity, let’s consider the common 2-port, 2-path calibration. This calibration method will yield a full set of S-parameters for the two ports: S11, S12, S21 and S22. It requires the use of a short, open, load and through (SOLT) calibration reference standard, along with the cables used in the test setup, as shown in Figure 1.


The diagram shows the sequence of connections made from the SOLT standard to the VNA for calibrations before the final connection of the DUT. Both ports are connected to a short, an open, a load (normally 50 Ohms) and a through path in sequence, and a measurement is taken. The large number of manual connections (at least 10 for a 2-port measurement) takes time and creates opportunity for error due to improper connections.


Figure 2: ECAL modules simplify the
VNA calibration process by reducing the
number of manual connections required.


In order to simplify this calibration process, some VNA manufacturers have designed electronic calibration (ECAL) systems that combine all the SOLT standards into single package controlled by a computer or the VNA itself. This reduces the number of connections that must be made for the SOLT calibration, as shown in Figure 2.


The ECAL module is connected only once, reducing the number of connections significantly, and the calibration process is now semi-automated, which reduces the total calibration time. However, ECAL modules are expensive “add ons” to the VNA, already quite an expensive instrument.


Compare this to calibration for the Teledyne LeCroy[WavePulser 40iX](https://teledynelecroy.com/wavepulser) . The WavePulser 40iX is a 2- or 4-port TDR instrument specifically designed for high-speed analysis of interconnects in serial data cables, channels, connectors, vias, backplanes, PCBs and ICs. The WavePulser uses an innovative, automatic calibration process that results in faster and easier measurements. The internal calibration structure of the WavePulser is illustrated in Figure 3.


Figure 3: The WavePulser 40iX uses internal relays to step through
the SOLT standards for each port during the automatic calibration.


The pulser/samplers of the WavePulser are connected to single-pole, 6-throw (SP6T) relays. The relay outputs connect to the internal short, open and load calibration standards. They also connect to the other port(s) implementing the through connection required during calibration. One of the relay outputs connects to the bulkhead output connector on the front panel—this is the *instrument reference plane* . This entire calibration subassembly is factory calibrated by making S-parameter measurements of each path from DC to 40 GHz. The cables matched to these ports are also characterized and marked with unique serial numbers. The S-parameters for the cables are part of the factory calibration and are automatically de-embedded every measurement to move the *measurement reference plane* to the point of connection to the DUT. Tools are included for de-embedding any other adapters or fixtures.


When you want to make a measurement, the DUT is connected to the cables matched to Port 1 and Port 2, and the WavePulser is powered on and allowed to reach its operating temperature. When the measurement is started, the instrument calibrates itself by alternately switching the relays through the short, open, load and through standards, obtaining a set of raw S-parameters. Two acquisitions are made: one with the Port 1 pulser active and the other with the Port 2 pulser active. These raw values are combined with the factory-measured values of the standards using traditional calibration algorithms to yield measurements corrected to the measurement reference plane. After being corrected by the stored calibration parameters, a calibrated measurement of the DUT is obtained. All this occurs automatically with only the DUT having to be connected once, quite an improvement over the traditional SOLT calibration. It is even simpler than the ECAL calibration, which still requires connecting to the external ECAL module before connecting the DUT.


The WavePulser 40iX also allows for alternative manual user calibrations and includes a calibration kit for this purpose. The automatic calibration calibrates to the internal reference plane then de-embeds the remainder of the output path, including the cables. The cables can change over time, especially due to mechanical flexure. A manual calibration corrects for any of these changes.


The WavePulser also supports a user second-tier calibration that dovetails with the automatic calibration to produce even greater accuracy. An application note available on the Teledyne LeCroy website, “[WavePulser 40iX Second Tier Calibration](https://teledynelecroy.com/doc/second-tier-calibration) ”, provides more information about the user second-tier calibration.


The WavePulser 40iX calibration is automated, simple and fast. Just connect cables to the DUT, then press “Go”. It does not require the purchase of additional external calibration standards or a laborious calibration process.


#### See also:


[When You Need a TDR and When You Need a WavePulser](https://blog.teledynelecroy.com/2022/03/when-you-need-tdr-and-when-you-need.html)


[Automotive Ethernet MDI S-Parameter Testing](https://blog.teledynelecroy.com/2021/06/automotive-ethernet-mdi-s-parameter.html)
