---
schema_version: "1.0.0"
document_id: "2bbc09fdfeb9785f574c59a4ec307ecd6186d324b61a4c593fc02d3337d340ea"
company_key: "yc-autumn-labs"
company: "Autumn Labs"
source_id: "yc-autumn-labs-news-import-b84a0821a8a2"
canonical_url: "https://autumnlabs.io/blog/manufacturing-data"
published_at: null
first_seen_at: "2026-07-22T23:46:11.218193+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:fb8cc296c809b115272a5908e334da46b02bf82e9fb3fba1a4714f28086e6b04"
---

# Manufacturing Data

In our earlier post on the[Manufacturing Spectrum](https://autumnlabs.io/blog/manufacturing-spectrum) , we explored how factories span a wide range of ownership and responsibility models, from fully in-house lines to outsourced partnerships such as CMs, JDMs, or ODMs. This spectrum directly influences a deceptively simple question: *how is data collected?* The answer depends on station types, vendor ecosystems, and the simple fact that no two stations look the same—whether in electronics, automotive, aerospace, or medical devices.


## Different Types of Stations


Not all stations are built the same, and that diversity makes data collection a hard problem:


- **Test Stations** : Precision-driven environments where measurement accuracy matters as much as throughput. They log detailed results, often using custom-built software stacks.
- **Assembly Stations** : Robotics, conveyors, and automation cells, often supplied by third-party equipment vendors. Their logs tend to be structured, but highly vendor-specific.
- **Automation Equipment** : PLCs, robotics controllers, SMT lines, CNC machines, etc., that communicate using protocols like OPC-UA, Modbus, or vendor-proprietary APIs.
- **Manual Workstations** : Operator-led stations is where variability is highest — entries come via handheld scanners, tablets, or even paper transcriptions that're digitized later.


💡 Insight


Each station type generates data in a different format, cadence, and level of detail — but factories can’t afford to treat them as isolated silos.


### The Vendor Ecosystem and Station Ownership


Stations aren’t just technically diverse — they’re also organizationally fragmented.


- OEM-owned stations (common in vertically integrated factories) tend to have custom-built software and deeper IT integration.
- ODM/JDM/CM partners often bring their own stations, with software stacks tied to their internal quality systems.
- Third-party test and equipment and automation vendors (e.g. Keysight, Teradyne, NI, Siemens) deliver turnkey stations, each with its own data format.


All of these scenarios lead to one truth: the company that owns the product doesn’t always own the station software.


This creates tension: engineering teams want clean, consistent data, while vendors often have little incentive to re-engineer their station software.


💡 Insight


Rather than asking vendors to change, Autumn Labs meets stations as they are.


### How Software Collects Data


If you peek behind the curtain, station software is as fragmented as the vendor ecosystem:


- LabVIEW, TestStand, or custom C# apps for functional test
- Python or C++ frameworks for validation and automation
- Vendor APIs and GUIs for robotics, PLCs, or CNC systems
- Excel or Access-based tools still running in some manual contexts


The challenge is not just technical — it’s organizational. One partner may refuse to alter its station scripts; another may run proprietary systems you’re not allowed to touch.


Without a unifying approach, manufacturers end up with CSV dumps, PDFs, or screenshots instead of usable, structured data.


### Autumn Labs SDK: A Common Language


Autumn Labs solves this by introducing a lightweight SDK that integrates into station software with minimal disruption:


- Cross-platform: Runs on whatever OS the station is running on: Windows, macOS or Linux.
- Schema consistency: Every station speaks the same language for metrics, logs, and events.
- Automated ordering: Built-in timestamping ensures events are sequenced correctly across the line.
- Realtime streaming: Data flows directly into the Autumn Labs client (installed on the station) -> ClickHouse, powering live dashboards.


Instead of mandating a single software stack, we provide a thin layer that harmonizes outputs. See the python example below for how to use the SDK to collect data from a station.


Note: We support other languages such as C#, with support for more in active development. You can also find the SDK documentation[here](https://autumnlabs.io/docs/reference/sdk/python) .


#### Example SDK Usage


python


```text
#!/usr/bin/env python3
# Import the AutumnLabs SDK - that's it for integration!
from   autumnlabs.station   import   AutumnLabsStation


# Initialize the station SDK - one line!
station   =   AutumnLabsStation()


# Start testing a unit
unit_sn   =   "SN12345"
station.track_unit_in(unit_sn,   slot  =  "TestStation_1"  )


# Capture and send test metrics
voltage_reading   =   12.1    # Your measurement function would go here
station.push_metric(  "voltage_test"  , voltage_reading,   value_unit  =  "V"  ,
lower_limit  =  "11.5"  ,   upper_limit  =  "12.5"  ,
unit_serial_number  =  unit_sn,   result  =  "pass"  )


temp_reading   =   23.5
station.push_metric(  "temperature_test"  , temp_reading,   value_unit  =  "C"  ,
lower_limit  =  "20"  ,   upper_limit  =  "30"  ,
unit_serial_number  =  unit_sn,   result  =  "pass"  )


# Report results of functional tests
test_passed   =   True
station.push_metric(  "functional_test"  ,   1  ,   value_unit  =  "count"  ,
unit_serial_number  =  unit_sn,   result  =  "pass"   if   test_passed   else   "fail"  )


# Complete the test cycle to automatically get cycle times and yield calculations
station.track_unit_out(unit_sn,   slot  =  "TestStation_1"  ,   result  =  "pass"  )


print  (  "SDK integration complete - just 3 method calls!"  )
```


*For engineers: It feels like adding a logging library. For operations teams, it unlocks a real-time observability layer across stations, vendors, and geographies.*


💡 Insight


Because of the friction in changing station software, Autumn Labs prioritizes ease of integration over strict adherence to a single standard.


### Why Realtime Matters


Factories in every industry operate in tight feedback loops. If a line is producing thousands of units per hour, even a short blind spot can mean hundreds of defective units before an issue is caught.


With realtime collection:


- Line managers detect idle or blocked stations within minutes, not days.
- Engineers can correlate trends across multiple units in the same batch.
- Executives see yield and throughput in live dashboards, instead of waiting for delayed reports.


Below is an example of the impact of realtime data collection on yield and throughput. With realtime data collection, you can see the impact of a problem immediately and take action to fix it. Without it, you may not notice the problem until it's too late.


Realtime isn’t a luxury — it’s the difference between firefighting problems after they hit the customer and preventing them at the source.


### Bringing It All Together


From electronics to automotive to aerospace, the common challenge is the same: how do you unify data across heterogeneous, uniquely built stations into something actionable?


Autumn Labs answers that by:


- Meeting each station where it is
- Harmonizing data formats with an SDK-first approach
- Streaming everything into a centralized, scalable backend


Data collection isn’t just about technology — it’s about navigating the messy realities of factory operations and vendor relationships. Autumn Labs makes that complexity invisible, managing integrations end to end so your teams can focus on what matters most: building better products, faster.


---


Want to try the realtime SDK? Request access to see how Autumn Labs can transform your manufacturing operations.
