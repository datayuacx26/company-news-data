---
schema_version: "1.0.0"
document_id: "7f7960c8ce26afde46acf7429be0abfb7b1216d419cc6b5e53cadbc09a128139"
company_key: "lightpath-technologies-inc-class-a-common-stock"
company: "LightPath Technologies Inc."
source_id: "lightpath-technologies-inc-class-a-common-stock-news-import-80e4a1dafb5b"
canonical_url: "https://www.lightpath.com/blog/drone-thermal-camera-spec-checklist-netd-resolution-frame-rate-swap"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T22:53:14.173880+00:00"
fetched_at: "2026-07-31T22:53:16.051735+00:00"
content_hash: "sha256:376b7288f2dd567588a70e613ab2a081d92b1c63406f06cde6da8c80260de90b"
---

# Drone Thermal Camera Spec Checklist: NETD, Resolution, Frame Rate & SWaP

**The best drone thermal camera specifications are matched to your mission, platform, and export requirements.**


- **Resolution and field of view set your detail and coverage, but a well-matched lens often beats raw pixel count for detection at range.**
- **NETD tells you how small a temperature difference a camera can see, and uncooled cores in the 30 to 60 mK range cover most drone missions.**
- **Frame rate, sensitivity, and resolution thresholds can trigger U.S. export controls, so lock compliance in before you finalize a spec.**
- **SWaP is the first filter, not the last: size, weight, and power decide which cameras your platform can even carry.**


**Treat the spec sheet as a starting point, then pressure-test every number against your actual platform and mission before you commit.**


---


Choosing a thermal camera for a drone program looks simple on a spec sheet and gets complicated in the field. Two cameras can list identical resolution and perform nothing alike once you factor in sensitivity, frame rate, optics, and how much payload your platform can carry. That gap between the datasheet and the mission is where most drone thermal camera specifications decisions go sideways.


The


[global drone thermal camera market](https://growthmarketreports.com/report/drone-thermal-camera-market) reached $2.66 billion in 2025 and is projected to hit $7.86 billion by 2034, so more OEMs and integrators are specifying complete systems instead of bolting on whatever module happens to fit. The imaging subsystem shapes detection range, flight time, integration effort, and compliance posture all at once, and a short checklist keeps you from optimizing one number at the mission's expense. You can see how these pieces come together across a


[full range of infrared imaging solutions](https://lightpath.com/) , but first, here's how to read the specs that matter.


## What Do Drone Thermal Camera Specifications Tell You?


Every


[drone thermal camera specifications](https://lightpath.com/thermal-imaging-solutions/drone-uav) sheet is a set of promises made under lab conditions. Resolution, NETD, frame rate, and sensitivity get measured on a bench, and then your platform adds vibration, temperature swings, limited power, and a gimbal envelope the sensor has to fit inside. The numbers matter as a system rather than as a scoreboard.


Resolution is the number everyone leads with, and it rarely decides whether a mission succeeds. Field performance comes from how the spectral band, optics, sensitivity, and interface work together on your aircraft. As


[routine long-range drone operations](https://www.faa.gov/newsroom/beyond-visual-line-sight-bvlos) move toward broader approval, more programs are specifying thermal payloads up front, so reading these specs correctly is a competitive advantage.


## Resolution and Field of View: How Much Detail Does Your Mission Need?


Resolution is the detector's pixel count, and more pixels mean more detail across the frame. The field of view, set by the lens, decides how much ground you cover and how far you can resolve a target. The two pull against each other: a wide field of view scans a large search area, while a narrow one concentrates pixels on a distant target for identification at range. Getting drone thermal resolution right means matching both to the mission rather than maximizing one in isolation.


A 640x512 detector is the practical default for most UAV payloads because it balances detail against weight. Still, raw pixel count is a trap on its own. A well-designed lens assembly can match the detection performance of a higher-resolution sensor at a lower weight, which is why optics and sensor matching often matter more than the resolution figure itself. For work like


[counter-drone detection and tracking](https://www.lightpath.com/blog/thermal-imaging-drone-technology-for-cuas-2026-oem-guide) , where you're pulling a small, low-signature target out of clutter, that optical matching often separates a system that works from one that only looks good on paper. When you weigh UAV thermal camera specs, treat drone thermal resolution and field of view as one decision.


## NETD: What Thermal Sensitivity Do You Require?


NETD (noise-equivalent temperature difference) measures the smallest temperature difference a camera can distinguish. Lower is more sensitive, so a lower drone thermal NETD figure means the camera picks out subtler heat contrasts. Sensitivity lets a thermal payload separate a warm body from a cool background in low-visibility conditions like darkness, fog, and smoke.


You rarely need the lowest NETD available. Uncooled long-wave infrared cores in the 30 to 60 mK range cover the large majority of drone missions, from surveillance and search and rescue to inspection and counter-drone work. Cooled systems push sensitivity lower and earn the added weight and power for long-range identification and targeting, where the temperature contrast is faint. Chasing an ultra-low drone thermal NETD you don't need adds cost, weight, and export complications without improving outcomes.


**Detector type**


**Typical NETD**


**Best suited for**


Uncooled LWIR


~30 to 60 mK


Most drone missions: surveillance, CUAS, SAR, inspection


Cooled MWIR


Lower than uncooled


Long-range detection and targeting where subtle contrast matters


Ask for complete-system NETD figures, including the actual lens f-number, rather than the bare sensor number, because the two aren't the same, and the difference shows up in the field.


## Frame Rate, Latency, and the Export-Control Catch


Frame rate determines how smooth your thermal video looks and how well you can track motion. For dynamic missions like tracking moving targets, aim for 30 Hz or higher with low latency so operators get a responsive feed. For static monitoring, 9 Hz can be perfectly adequate. Watch latency as closely as frame rate, since a delayed feed undermines tracking even when the frame rate looks fine.


Frame rate is also where compliance enters the conversation. Certain combinations of frame rate, resolution, and sensitivity can classify a camera under U.S. export controls, and the


[federal rules governing camera exports](https://www.federalregister.gov/documents/2024/02/23/2024-03661/revision-of-license-requirements-of-certain-cameras-systems-or-related-components) treat higher frame rates as one trigger point. If your program has any international footprint, decide early whether an export-friendly configuration meets your needs, since many surveillance and SAR missions perform well within less-restricted thresholds. Building that judgment into your spec beats discovering a constraint after design freeze. It's worth reviewing the


[thresholds around ITAR-controlled thermal cameras](https://www.lightpath.com/insights/complete-guide-to-itar-compliant-thermal-cameras) alongside your performance targets.


## SWaP: Why Size, Weight, and Power Filter Your Options First


Size, weight, and power are not a footnote on a UAV payload. For most platforms, they're the first filter that eliminates options before any spec comparison begins. Every gram of camera weight displaces battery capacity or structural margin, and on small and medium drones, a 100 to 200 gram difference can meaningfully change mission duration. A camera with excellent detection that drains your battery in 18 minutes isn't a solution for an endurance-driven mission.


Uncooled LWIR cameras are light and low-power, suiting everything from compact multirotors upward. Cooled MWIR systems deliver superior range and sensitivity but add weight, power draw, and cooldown time, so they land on larger platforms where the mission justifies the penalty. The smarter approach evaluates the SWaP thermal camera tradeoff at the system level rather than judging a single component. Payloads engineered specifically for unmanned platforms tend to outperform terrestrial cameras adapted after the fact, and working through the


[full set of SWaP tradeoffs for OEM programs](https://www.lightpath.com/blog/evaluating-drone-thermal-imaging-solutions-for-oems) early keeps your platform from a late redesign. In short, your SWaP thermal camera budget belongs at the front of a drone thermal camera specifications review, not the end.


**Attribute**


**Uncooled LWIR (8 to 14 µm)**


**Cooled MWIR (3 to 5 µm)**


Weight and power


Light, low power


Heavier, higher power, cooldown time


Best for


Surveillance, CUAS, SAR, inspection


Long-range ISR and targeting


Platform fit


Small to large platforms


Medium to large platforms


Sensitivity


Strong (typically 30 to 60 mK)


Highest


## Radiometric Output and Integration: The Specs Buyers Forget


The specs above get most of the attention, but two more decide whether your system delivers usable data and fits the aircraft. Check both before you finalize your UAV thermal camera specs.


### Radiometric vs Non-Radiometric: Do You Need Temperature Data?


A radiometric camera outputs a calibrated temperature value for every pixel rather than a relative heat image. If your mission produces inspection reports, maintenance logs, or anything where the exact temperature matters, radiometric output is non-negotiable, and you'll want adjustable emissivity and reflected-temperature compensation with it. If you only need to detect and track heat contrast, a non-radiometric system is lighter and simpler.


### Gimbal and Payload Integration: Will It Fit and Talk to Your Stack?


A stable 3-axis gimbal isolates the sensor from airframe vibration, which matters because small motions blur a heat boundary only a few pixels wide. Beyond stabilization, confirm the physical envelope, mounting, power input, and communication protocol match your platform. A camera that fits your weight budget but not your flight stack still fails integration. Validate against your actual aircraft rather than trusting a plug-and-play claim.


## Your 5-Point Thermal Camera for Drone Selection Checklist


The thermal camera for drone selection comes down to five decisions. Work them top to bottom because each one narrows the field for the next.


1. **Spectral band and cooling.** Start with uncooled LWIR (8 to 14 µm) for SWaP-efficient surveillance, CUAS, SAR, and inspection, or cooled MWIR (3 to 5 µm) when long-range sensitivity justifies the payload penalty.


2. **Resolution and field of view.** Match drone thermal resolution and coverage to the mission, and remember that optical design often beats raw pixel count for detection at range.


3. **NETD sensitivity.** Specify the sensitivity your detection task actually needs, typically 30 to 60 mK uncooled, rather than reflexively chasing the lowest number.


4. **Frame rate and compliance.** Pick 30 Hz or higher for tracking, 9 Hz where static is fine, and confirm your configuration against export-control thresholds early.


5. **SWaP and integration.** Confirm the SWaP thermal camera budget, gimbal envelope, interface, and radiometric output all fit your platform before you commit.


Run every candidate through those five items, and a spec sheet becomes a decision you can defend to your program team.


## Frequently Asked Questions


**What resolution do I need for a thermal camera for drone selection?** A 640x512 detector offers a strong balance of detail and weight for most UAV missions, and many search and surveillance tasks run well on smaller cores. Prioritize the lens-and-sensor match over raw pixel count, since good optics can deliver comparable detection at lower weight.


**Is a lower NETD always better?** Not necessarily. Lower drone thermal NETD means higher sensitivity, but you pay for it in cost and sometimes export complexity. Uncooled cores in the 30 to 60 mK range handle the majority of drone missions, so specify the sensitivity your task requires, not the lowest figure on the market.


**Do drone thermal camera specifications affect export compliance?** Yes. Certain frame rate, resolution, and sensitivity combinations can place a camera under U.S. export controls, so if your program has an international footprint, factor compliance into your UAV thermal camera specs from the start.


**Should my UAV payload be radiometric or non-radiometric?** Choose radiometric if you need calibrated per-pixel temperature data for inspection or reporting. Choose non-radiometric if you only need to detect and track heat contrast, since it's lighter and simpler to integrate.


## Spec the Mission, Not the Spec Sheet


Successful teams treat the thermal payload as a system decision made early, not a component picked late. Every spec on this checklist connects to the next, and the right answer is always the one that fits your platform, mission, and compliance posture rather than the biggest number available. With the right sequence, the rest of your platform design gets easier.


LightPath designs and manufactures complete cooled and uncooled UAV thermal payloads, from proprietary optical materials through finished camera assemblies.


[Start the conversation with our team](https://lightpath.com/contact) with your platform, detection targets, and compliance questions to pressure-test your drone thermal camera specifications against real engineering constraints.
