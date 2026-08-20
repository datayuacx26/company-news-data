---
schema_version: "1.0.0"
document_id: "93e55ae0151d2e2512a5d87e98e661dd449f730bf1ef794ebcb2a034b37b861c"
company_key: "ceva-inc-common-stock"
company: "CEVA Inc."
source_id: "ceva-inc-common-stock-rss-438e9c80ad28"
canonical_url: "https://www.ceva-ip.com/blog/from-driveway-to-checkout-seamless-indoor-navigation-powered-by-uwb/"
published_at: "2026-03-06T14:35:39+00:00"
first_seen_at: "2026-07-20T23:21:57.347824+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:c1f4c1915c29d6c2ebba4953d486ddefe8dc49cf0add4b3d653783dce929b2b8"
---

# From driveway to checkout: seamless indoor navigation powered by UWB

4 min read


### The seamless vision


Imagine you’re driving to a busy campus. You open your preferred navigation app, and everything works as expected, until you go underground into the parking garage.


In today’s world, that’s often where guidance becomes vague or disappears. In the seamless future, it doesn’t. Navigation continues smoothly, even helping direct you to an available parking spot.


Then you step out of the car and keep walking, into a hospital, a mall, or a massive terminal. No app switching, no re-searching, no “you’ve arrived” message while you’re still outside. Your route continues across entrances, corridors, elevators, and atriums, down to a specific department, shop, gate, or service desk, and even enabling Smart Spaces, where your precise indoor location unlocks the right service at the right moment.


### The missing link: outdoor-to-indoor continuity


Satellite-based navigation is incredible when you have a good view of the sky, but indoor and underground environments are fundamentally different.


Satellite signals don’t behave kindly indoors: they’re easily blocked by concrete and steel, distorted by reflections, and weakened as they pass through structures, so accuracy quickly becomes dependent on having a clear “open-sky” view and minimal signal obstruction. That’s why indoor continuity remains a stubborn gap in the user experience.


Solving this isn’t about competing with GNSS, it’s about complementing it. The practical model is a handoff: GNSS outdoors, then a precise indoor layer where the building provides reference points. That indoor layer further enables Smart Spaces, allowing facilities to both guide and respond. That’s where Ultra-wideband (UWB) and indoor anchors come in, especially as UWB becomes more common in the broader device ecosystem, including mobile devices and automotive access solutions that use UWB for location-aware experiences.


### The real deployment blocker: interoperability at scale


UWB indoor positioning has been technically feasible for years, but mass deployment needs more than a good demo. It needs an ecosystem where a phone from vendor A can work in a location deployed by vendor B, with infrastructure from vendor C, reliably, repeatably, and at global scale.


That’s exactly the problem FiRa is organized to solve: turning “works” into “works everywhere” through specifications and certification focused on interoperability. When indoor navigation becomes a standardized, certified capability, site owners and operators can deploy with confidence and product teams can ship with fewer unknowns.


### What’s new: FiRa’s momentum on TDoA-based indoor location


FiRa has long highlighted DL-TDoA (Downlink Time-Difference-of-Arrival) as a strong foundation for “untracked” indoor navigation, where the user device can determine its position by listening to synchronized anchors, without needing to transmit. \[1\] That maps naturally to wayfinding use cases: you walk through a facility, and your device continuously figures out where you are.


The latest acceleration point is FiRa Core 4.0, announced December 3, 2025, which expands certification coverage to include UL-TDoA (Uplink Time-Difference-of-Arrival), opening the door wider for interoperable, infrastructure-driven asset tracking at scale. \[2\] In other words, FiRa’s scope is growing to support both major “seamless indoor” pillars: navigation for people (often downlink-centric) and tracking for assets (often uplink-centric).


### DL-TDoA vs. UL-TDoA in plain terms


DL-TDoA is the most natural fit for consumer wayfinding: the site infrastructure transmits; your phone (or device) listens. This tends to scale well because many users can “listen” at once, and it can support privacy-forward designs where the user device computes its own position. FiRa explicitly describes untracked indoor navigation using DL-TDoA in this direction.


UL-TDoA flips the flow: the tag transmits; the infrastructure listens and computes location. This often maps best to operational use cases: tracking medical equipment in hospitals, carts in airports, tools in factories, where the facility operator needs a live view of many items. FiRa Core 4.0 specifically calls out UL-TDoA as enabling asset tracking by an infrastructure of UL-TDoA anchors.


Adoption signals: devices and infrastructure providers are committing


On the device side, the message is clear: platforms are treating indoor positioning as a first-class capability.


Apple introduced DL-TDoA support starting with iOS 26, a recent platform step that signals growing OS-level readiness for anchor-based indoor navigation. \[3\]


On Android, UWB entered the platform starting with Android 13, and the roadmap didn’t stop there. Public platform updates show continued additions after Android 13 for both downlink- and uplink-based indoor positioning methods, an important signal of sustained investment as FiRa-based deployments mature.


On the infrastructure side, we’re also seeing meaningful commercial moves: Cisco’s Catalyst 9800 Any Locate capabilities now include UWB DL-TDoA for indoor wayfinding, with IOS XE 17.18.2 (dated December 21, 2025), a strong signal that UWB DL-TDoA is entering mainstream enterprise release cadence. \[4\]\[5\]


### Turning UWB standards into shippable silicon


For SoC teams, indoor location is a major opportunity, but shipping it at scale is hard because the silicon must deliver consistent results under real product constraints. That means meeting aggressive power targets, keeping area (and cost) under control, reducing bring-up effort, and aligning with interoperability standards.


As the ecosystem evolves, teams increasingly look to turnkey UWB IP platforms that reduce integration risk while aligning with industry certification programs. Accordingly, Ceva-Waves UWB is widely positioned as a leading end-to-end UWB IP platform, compliant with IEEE 802.15.4ab, combining optimized MAC/PHY hardware IP with tightly integrated software stacks targeting FiRa and CCC Digital Key requirements \[6\]. With industry adoption now spanning both DL-TDoA and UL-TDoA, product developers need a UWB foundation like Ceva-Waves UWB that can scale from battery-powered tags to always-on infrastructure without sacrificing security or robustness.


That’s how seamless becomes practical: one uninterrupted journey, from parking garage to point of sale.


**References**


\[1\] FiRa Consortium – “Untracked Indoor Navigation”:[https://www.firaconsortium.org/resource-hub/blog/untracked-indoor-navigation](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.firaconsortium.org%2Fresource-hub%2Fblog%2Funtracked-indoor-navigation%3Futm_source%3Dchatgpt.com&data=05%7C02%7CHadas.Amir%40ceva-ip.com%7Ce0bb84b38d9942cc54a808de6c80d61e%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639067497806147120%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=JjZtNRa9d%2FkHTrPmwE71SP15YqLJeU5tqWBLYpdXTpg%3D&reserved=0)
\[2\] FiRa Consortium – FiRa Core 4.0 press release:[https://www.firaconsortium.org/sites/default/files/2025-12/FiRa%204.0%20Press%20Release%20-%20Final.pdf](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.firaconsortium.org%2Fsites%2Fdefault%2Ffiles%2F2025-12%2FFiRa%25204.0%2520Press%2520Release%2520-%2520Final.pdf%3Futm_source%3Dchatgpt.com&data=05%7C02%7CHadas.Amir%40ceva-ip.com%7Ce0bb84b38d9942cc54a808de6c80d61e%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639067497806186686%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=JuaHrFd8HCFaUYOzbWiqCwJEL2dD%2FgZwRuZS7rkY%2FRU%3D&reserved=0)
\[3\] Apple Developer Documentation – DL-TDoA ranging overview:[https://developer.apple.com/documentation/nearbyinteraction/dl-tdoa-ranging](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdeveloper.apple.com%2Fdocumentation%2Fnearbyinteraction%2Fdl-tdoa-ranging%3Futm_source%3Dchatgpt.com&data=05%7C02%7CHadas.Amir%40ceva-ip.com%7Ce0bb84b38d9942cc54a808de6c80d61e%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639067497806208974%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=Lgx43xLwCRIfvjjCQnKRlo6mm92gVRXrfPDKKHTzwxk%3D&reserved=0)


\[4\] Cisco – Catalyst 9800 AnyLocate capabilities:[https://www.cisco.com/c/en/us/td/docs/wireless/controller/9800/17-18/config-guide/b_wl_17_18_cg/m_ss-location-anylocate-capabilities.html](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.cisco.com%2Fc%2Fen%2Fus%2Ftd%2Fdocs%2Fwireless%2Fcontroller%2F9800%2F17-18%2Fconfig-guide%2Fb_wl_17_18_cg%2Fm_ss-location-anylocate-capabilities.html%3Futm_source%3Dchatgpt.com&data=05%7C02%7CHadas.Amir%40ceva-ip.com%7Ce0bb84b38d9942cc54a808de6c80d61e%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639067497806229009%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=l0oWGQ9XSnQUs0osK9KyYjqwQ8ms39b7pvXE5WEfReM%3D&reserved=0)
\[5\] Cisco – IOS XE 17.18.2 release notes:[https://www.cisco.com/c/en/us/td/docs/wireless/controller/9800/17-18/release-notes/rn-17-18-2-9800.html](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.cisco.com%2Fc%2Fen%2Fus%2Ftd%2Fdocs%2Fwireless%2Fcontroller%2F9800%2F17-18%2Frelease-notes%2Frn-17-18-2-9800.html%3Futm_source%3Dchatgpt.com&data=05%7C02%7CHadas.Amir%40ceva-ip.com%7Ce0bb84b38d9942cc54a808de6c80d61e%7C02f96adbc1ff4245afbfe8427616047b%7C0%7C0%7C639067497806247867%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=bPdGEcqrYtTknuiJTPwg0Yl11XczrJow9kNBMFRrg10%3D&reserved=0)
\[6\] Ceva –[Ceva-Waves-UWB](https://www.ceva-ip.com/product/ceva-waves-uwb)
