---
schema_version: "1.0.0"
document_id: "bae097e7aeabfaeb2fede0d0870f037dde4e642ee759ec93019c0562f75730ef"
company_key: "teledyne-technologies-incorporated-common-stock"
company: "Teledyne Technologies Incorporated"
source_id: "teledyne-technologies-incorporated-common-stock-rss-9000605d05af"
canonical_url: "https://blog.teledynelecroy.com/2022/11/new-60-v-offset-power-rail-probes-offer.html"
published_at: "2022-11-28T13:00:00+00:00"
first_seen_at: "2026-07-20T04:36:13.437201+00:00"
fetched_at: "2026-08-20T01:30:31.660362+00:00"
content_hash: "sha256:840dae160dadfdc08a90fb56d1aae8565b41dbf27f1432a53b57cdb9466ba9d8"
---

# New 60 V Offset Power Rail Probes Offer the Capability Needed for 48 V Power Integrity Analysis

Figure 1. The RP2060 and RP4060
build on the legacy of the RP4030 power
rail probe. The new probes are ideally
suited to working with the new 48 Vdc
power structures.


In 2016, Teledyne LeCroy first offered the RP4030 Power Rail Probe, which was designed to enable engineers to probe a low-impedance, low-voltage DC power/voltage rail signal without loading the device under test (DUT). It provided ±30 V of probe offset to allow a DC power/voltage rail signal to be displayed in the vertical center of the oscilloscope regardless of the gain (sensitivity) setting.


Recently, we released two, new power rail probes that build on those capabilities—the 2 GHz[RP2060](https://teledynelecroy.com/probes/active-voltage-rail-probe/rp2060) and 4 GHz[RP4060](https://teledynelecroy.com/probes/active-voltage-rail-probe/rp4060) . Both probes feature:


- ±60 V Offset Capability
- ±800 mV Dynamic Range
- 50 kΩ DC Input Impedance (for low loading of low-impedance power rails)
- 1.2:1Attenuation (for low additive noise)
- MCX-terminated cable with a variety of board connections: 4 GHz*-rated MCX PCB mount;
4 GHz* solder-in; 3 GHz* coaxial cable to U.FL PCB mount; optional 500 MHz browser


* Bandwidths listed are for the 4 GHz RP4060. Maximum bandwidth when used with RP2060 is 2 GHz.


### Why the New Probes?


One driver of the new release is the increase in the number and size of data centers needed to support cloud computing and other data-intensive applications, and the new power architectures they require. The new rail probe is designed to ideally meet the needs of engineers working with power rails rated up to 48 Vdc.


To power individual servers in the racks that make up data centers, traditional “server farms” would step down power to the 12 Vdc needed to input to the server, where within the server it would be further stepped down to the 5 V, 3 V, 1 V or less needed to power the many individual chips. In the process, much energy is lost to heat, which in turn requires more energy to cool, raising costs. The average data center today uses an average of 3 kW to 5 kW of electricity per rack.


New, more efficient architectures use a 48 Vdc distribution voltage right at the server, where new types of power modules and digital controllers step it down to the much smaller voltages needed by other subsystems. Engineers designing such systems therefore need to probe DC voltages anywhere from 1 V or less to 48 V. Also, because it is a DC voltage,[high resolution oscilloscopes](https://teledynelecroy.com/oscilloscope/hdo.aspx) and probes with very high voltage offset are needed to see the very small amounts of ripple and other types of noise that may be present on a power rail, pointing to potential problems in the system. By increasing the 30 V offset of the legacy RP4030 Power Rail Probe to a comfortable 60 V, the new RP2060 and RP4060 Power Rail Probes provide the voltage range, offset and sensitivity needed for the new 48 V power structure.


### How We Do It


The input is through a high-bandwidth SMA connector, terminated to ground with a 50 kΩ resistor in parallel with a 0.1 μF capacitor. This provides high input impedance near DC and low input impedance at high frequencies―highly desirable for low-impedance DC power rail probing. A two- stage offset DAC provides ± 60 V range. This permits the offset value to be set with high accuracy (0.1% ±3 mV). The output is through a BNC connection to the Teledyne LeCroy ProBus interface into a 50 Ω oscilloscope termination. An Auto Zero grounding switch permits Auto Zero of the DC value at any time without having to disconnect the probe from the device under test (DUT).


#### Also see:


[Measuring Dead Time in 48 V Power Converters, Part 2: Dynamic Measurements](https://blog.teledynelecroy.com/2021/10/measuring-dead-time-in-48-v-power.html)


[Measuring Dead Time in 48 V Power Converters, Part 1: Static Measurements](https://blog.teledynelecroy.com/2021/08/measuring-dead-time-in-48-v-power.html)


[Fundamentals of Power Integrity: Characterizing PDN Noise](https://blog.teledynelecroy.com/2020/08/fundamentals-of-power-integrity_28.html)


[Fundamentals of Power Integrity: Self-aggression Noise](https://blog.teledynelecroy.com/2020/09/fundamentals-of-power-integrity-self.html)


[Fundamentals of Power Integrity: Board Pollution](https://blog.teledynelecroy.com/2020/09/fundamentals-of-power-integrity-board.html)
