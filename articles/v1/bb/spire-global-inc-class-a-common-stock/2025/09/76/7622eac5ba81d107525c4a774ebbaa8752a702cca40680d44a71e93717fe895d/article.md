---
schema_version: "1.0.0"
document_id: "7622eac5ba81d107525c4a774ebbaa8752a702cca40680d44a71e93717fe895d"
company_key: "spire-global-inc-class-a-common-stock"
company: "Spire Global Inc."
source_id: "spire-global-inc-class-a-common-stock-news-import-ebaf5653be69"
canonical_url: "https://spire.com/blog/space-reconnaissance/gnss-interference-report-black-sea-romanian-airspace/"
published_at: "2025-09-24T13:48:06+00:00"
first_seen_at: "2026-07-22T14:30:50.712658+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:e89264f321cd82dd0fea6e430dd5bf599cc25e369d84b58803db42036147743f"
---

# GNSS interference report: Russia - Part 4 of 4: Black Sea & Romanian airspace - Spire Global

Through a combination of electronic warfare systems, offshore interference zones, and strategic positioning in Crimea, Russia has turned the Black Sea into one of the most consistently degraded GNSS zones in Europe.


This four-part series delivers concise, data-backed snapshots of GNSS interference activity in and around Russia. Each installment pairs open-source reporting with Spire’s LEO-based constellation, providing decision-grade insight that goes well beyond public jamming maps like gpsjam.org.


Part 4 focuses on the Black Sea, where Russian GPS jamming and spoofing have expanded into international waters and NATO airspace. Through a combination of electronic warfare systems, offshore interference zones, and strategic positioning in Crimea, Russia has turned the Black Sea into one of the most consistently degraded GNSS zones in Europe. Open-source reporting from scientific teams, ship operators, and military officials confirms a multi-layered interference campaign affecting civilian navigation, flight safety, and maritime commerce.


## The Black Sea: A live fire zone for GNSS interference


While Russia’s electronic warfare systems have long targeted Ukrainian military operations, the Black Sea has increasingly emerged as a gray zone for GNSS denial, impacting civil aviation, international shipping, and regional infrastructure.


From mid-2024 onward, reports of[GPS jamming and spoofing](https://spire.com/aviation/gnss-interference/) persisted in the Romanian Flight Information Region (FIR), over the Danube Delta, and across the maritime zones surrounding Crimea, and these effects are not confined to the battlefield. Civilian aircraft have reported false positioning, maritime AIS systems have shown ships “teleporting” inland or spinning in circles, and scientific missions have directly recorded spoofing signals at high altitude.


The scale and consistency of this interference suggest that the Black Sea is no longer just a border zone – it is a live, contested arena in Russia’s wider information warfare campaign.


### Summary


- **Location:** Black Sea coastlines and maritime FIRs, including Romanian airspace, western Crimea, and the western Black Sea
- **Activity:** Persistent GNSS spoofing and jamming affecting aircraft, vessels, and scientific sensors
- **Date highlight:** Spring 2025 – consistent GNSS degradation observed by commercial pilots, scientific missions, and national defense authorities
- **Impact zones:** Southeast Romanian FIR, Constanța coast, Danube Delta, Snake Island corridor, offshore platforms, maritime traffic lanes


### Real-world incident


In late 2024 and into spring 2025, reports of GPS interference in the Black Sea region surged. Romanian officials, aviation observers, and civilian monitoring platforms all documented increasing spoofing and jamming activity, most of which originated from Russian-controlled Crimea.


#### Scientific confirmation of high-altitude spoofing (Aug 2024 – Feb 2025)


In August 2024, the Romanian firm[InSpace Engineering launched a high-altitude balloon](https://roinspace.com/mission-in-the-stratosphere-documentation-of-gps-jamming-over-the-black-sea/) from Constanța to monitor GNSS spectrum quality over the Black Sea. The payload recorded persistent jamming across all GNSS bands and a definitive spoofing event at ~11 km altitude, where the reported position abruptly shifted toward Simferopol in Russian-occupied Crimea.


Flight data confirmed the balloon never physically deviated from its path, pointing to external signal manipulation. The team later published spectrograms showing full-band interference across L1, L2, and L5, marking the first scientific confirmation of high-altitude GNSS spoofing in NATO airspace.


#### Regional escalation and independent monitoring (August 2024 – April 2025)


Between late summer 2024 and spring 2025, GNSS spoofing in the region reached new levels of scale and severity. According to[OPSGROUP’s 2024 GPS Spoofing Report](https://ops.group/blog/gps-spoofing-final-report/) , the global aviation community observed a 500% increase in spoofing incidents, peaking at an average of 1,500 spoofed flights per day.


While the report did not attribute activity to a specific region, Romanian and Bulgarian pilots began filing real-time reports in February of signal degradation, false GNSS positioning, and sudden navigation dropouts while flying along the Black Sea coast. These incidents aligned with public telemetry and scientific data showing consistent spoofing vector patterns that resolved eastward, often pointing toward Simferopol in Russian-occupied Crimea.


In May 2025, Romania’s Chief of Defense[publicly confirmed that GNSS spoofing and jamming “occur weekly” along the country’s coast](https://odessa-journal.com/chief-of-the-romanian-general-staff-russians-jam-gps-signals-in-romania?utm_source=chatgpt.com) , describing the interference as part of a broader “hybrid warfare” strategy that often coincides with naval movements and drone activity.


#### Maritime impact and persistent risk (April – May 2025)


By spring 2025, spoofed GNSS signals over the Black Sea had reached near-daily frequency, with merchant vessels east of Constanța and south of Snake Island among the most consistently affected.[Civilian AIS systems displayed erratic positioning](https://www.gpsworld.com/russian-jamming-creates-bermuda-triangle-in-baltic/) – ships appearing inland, spinning in circles, or drifting far off established maritime routes.


These spoofing patterns echoed those seen in Russia’s “Baltic Bermuda Triangle”, and were captured in[MarineTraffic screenshots throughout the region](https://www.marinetraffic.com/en/ais/home/centerx:28.659/centery:60.375/zoom:13) .


While some signal distortion may appear random, analysts believe much of the interference is intentional, designed to obscure military activity or mask strategic movements. The consequences extend well beyond navigational confusion: ships risk regulatory violations, insurance disputes, and collision hazards, particularly in congested maritime corridors near the Danube Delta and Romanian offshore platforms.


Notably, this maritime interference often coincides with Russian electronic warfare activity in Crimea, reinforcing the pattern of GNSS denial as a tactical smokescreen for broader hybrid operations.


Yet while public reports and operational data confirm the scale and risk of interference, most civilian platforms lack the resolution or visibility to trace signal origin or detect manipulation in real time. This is where Spire’s satellite-based RF monitoring system adds critical value – not only validating interference, but mapping its scope, directionality, and operational impact from orbit.


## Spire Satellite Validation


### What Spire saw


The following satellite-confirmed incidents showcase how[GNSS spoofing](https://spire.com/aviation/gnss-interference/) activity escalated inland and along the Romanian coast through fall 2024 and into spring 2025. These Spire Aviation snapshots provide concrete telemetry evidence supporting the open-source observations outlined in earlier reporting.


#### Persistent spoofing over Buzău, Romania (October 2024)


Throughout October 2024, Spire Aviation’s satellite-based constellation detected a cluster of GNSS spoofing anomalies over central-eastern Romania, specifically along the flight corridor of the August 2024 high-altitude balloon mission launched by InSpace Engineering.


In three adjacent hexes spanning east-west from Buzău to Ploiești, GNSS integrity metrics showed severe signal degradation that is consistent with spoofing:


- **nacp_q05 = 0 in all three zones** : this indicates that the lowest 5th percentile of ADS-B messages received in each region reported a NACp value of 0, meaning at least 5% of messages showed complete GNSS position degradation. In practice, this likely reflects GNSS loss affecting multiple aircraft, not just isolated messages.
- **nic_q10 = 7 or 8 in all three zones** : this means that 90% of ADS-B messages reported a NIC value of 7 or higher, indicating high signal integrity. This pattern of strong signal confidence despite degraded or incorrect positioning is a classic signature of GNSS spoofing, where the spoofed signal appears reliable to receivers but delivers false location data.


One hex captured data from up to 399 aircraft, reinforcing that this was not an isolated anomaly, but a persistent regional interference event.


While the spoofing signal itself likely originated from a local or regional source, the false positions resolved eastward near Simferopol, in Russian-occupied Crimea – mirroring the directional spoofing vector recorded by InSpace. During descent, the balloon’s receiver abruptly snapped to a spoofed position near Ayvazovskogo Airport in Crimea, despite remaining physically over Romanian airspace.


Together, this spatial correlation and spoofed positional alignment confirm that the August spoofing incident was not a one-off anomaly. Instead, it was part of a broader, ongoing GNSS interference pattern affecting NATO airspace through fall 2024.


**Figure 1** : Spire Aviation GNSS integrity metrics near Boboc Airfield, Romania – spoofing indicators affecting at least 89 aircraft (October, 2024)


**Figure 2** : Hex ~50 km west of Boboc – spoofing indicators in 114 aircraft (October, 2024)


**Figure 3** : Hex ~150 km west of Boboc – spoofing indicators in data from 399 aircraft (October, 2024)


#### High-confidence spoofing over Danube Delta, Romania (August 2024 – April 2025)


Satellite validation from Spire Aviation confirms a persistent GNSS spoofing pattern across southeastern Romania and the Danube Delta region, aligning with pilot-reported anomalies, OPSGROUP alerts, and military assessments from late summer 2024 through spring 2025.


Spire’s satellite-based monitoring system detected recurring spoofing signatures across multiple high-traffic air corridors near Galați, Brăila, and Tulcea – three strategic nodes within Romanian[FIR airspace](https://spire.com/aviation/global-flight-and-aviation-insights/) and near NATO’s eastern flank.


### Spoofing events confirmed by Spire telemetry


**On October 8, 2024** , a spoofing incident was recorded over the Tulcea–Babadag region. Spire Aviation telemetry showed that at least 10% of aircraft in this airspace had no valid positional accuracy (nacp_q10 = 0 and nacp_q05 = 0), while simultaneously reporting moderate signal integrity (nic_q10 = 6, nic_q05 = 6). This combination is a textbook spoofing signature: aircraft systems continue to trust the GNSS signal because its integrity rating remains acceptable, but the positional data being delivered is either missing or false.


The result is a navigation environment where aircraft unknowingly operate with degraded spatial awareness—one of the core risks of GNSS spoofing.


**Figure 4:** Tulcea–Babadag region (October 8, 2024)


**Eleven days later, on October 19** , a second spoofing event was detected in the exact same hex. In this instance, 5% of aircraft again experienced total position failure (nacp_q05 = 0), while most reported high positional confidence (nacp_q10 = 8) and consistent signal integrity (nic_q10 = 6).


The persistence of this pattern within the same airspace indicates a sustained interference campaign rather than an isolated anomaly. It also reinforces concerns about the repeatability and reliability of GNSS signals in this corridor during periods of heightened regional tension.


**Figure 5:** Tulcea–Babadag region (October 19, 2024)


**By October 26** , spoofing indicators appeared in an adjacent hex to the northeast, directly over the Galați–Reni corridor, near the Romanian–Ukrainian border. GNSS metrics again showed position failure in at least 5% of aircraft (nacp_q05 = 0) with strong positional confidence (nacp_q10 = 8) and stable signal integrity (nic_q10 = 6).


With 17 aircraft contributing to the dataset, the data confirms that spoofing was active even with moderate[air traffic volume](https://spire.com/aviation/global-flight-and-aviation-insights/) . The shift eastward also suggests possible expansion of spoofing coverage or repositioning of the signal origin – both of which hold implications for cross-border risk.


**Figure 6:** Galați–Reni corridor (October 26, 2024)


Together, these incidents reveal a high-confidence spoofing pattern targeting Romanian airspace in fall 2024. The consistency of the metrics, recurrence across time, and movement across neighboring hexes paint a clear picture of deliberate GNSS interference along NATO’s eastern edge.


These signals not only disrupt air navigation, but they also create dangerous operational blind spots in one of Europe’s most sensitive geopolitical regions.


### Spoofing expansion along Bulgaria’s Black Sea coast (May 2025)


In late May 2025, Spire Aviation’s satellite-based monitoring system confirmed ongoing spoofing activity in NATO airspace along the Bulgarian coast, including two key events in the weeks leading up to the now-public characterization of the region as a “Black Sea Bermuda Triangle.”


In both cases, the spoofing signatures followed the same pattern:


- **nacp_q05 = 0** : At least 5% of received ADS-B messages reported no positional accuracy, likely indicating GNSS spoofing affecting multiple aircraft, though the exact number may vary.
- **nacp_q10 = 8 and nic_q10 = 7** : Majority of aircraft reported excellent GNSS signal quality
- **nic_q05 = 4 (May 24) and 1 (May 31)** : A small percentage showed degraded signal integrity
- **Large sample sizes:** 441 and 385 aircraft, respectively


This is a textbook spoofing profile, with pilots and onboard systems reporting a high level of confidence in a GNSS signal that is delivering false or absent location data. The illusion of signal health masks the reality of manipulated positioning, creating serious risk in congested air and sea corridors.


**Figure 7** : Spire Aviation GNSS spoofing data along the Bulgarian Coast – Black Sea (May 24, 2025)


**Figure 8** : Spire Aviation’s GNSS spoofing data along the Bulgarian Coast – Black Sea (May 31, 2025)


Just four days after these satellite-confirmed events, civilian monitoring platforms and ship operators publicly described the Black Sea as exhibiting “Baltic Bermuda Triangle” behavior, where commercial vessels were seen spinning in circles, drifting inland, or teleporting across maritime zones on platforms like MarineTraffic. While initially anecdotal, this[Spire Aviation telemetry](https://spire.com/aviation/gnss-interference/) provides hard evidence that GNSS spoofing was active, frequent, and escalating along Bulgaria’s coast during the final weeks of May 2025.


Together with high-altitude spoofing confirmed by InSpace Engineering, pilot reports submitted to OPSGROUP, and official Romanian defense statements, this coastal spoofing campaign illustrates a multi-domain interference strategy – degrading GNSS integrity across the maritime, aviation, and scientific sectors in one of Europe’s most strategically sensitive theaters.


## The impact of Spire Aviation’s satellite data


While traditional public platforms like gpsjam.org offer broad visuals of interference zones, they rarely provide directionality, signal behavior, or attribution confidence. Spire’s LEO-based constellation fills that gap by capturing real-time GNSS telemetry from aircraft, including signal quality, positioning confidence, and interference signatures at scale.


This isn’t just validation. It’s insight.


In the Black Sea region, Spire Aviation’s data didn’t just show where spoofing occurred – it revealed how signals were behaving, how frequently spoofing reoccurred, and how GNSS confidence remained deceptively high even in corrupted zones. That level of resolution allows civil and defense operators to assess operational risk, not just anomaly presence.


## Interference pattern and attribution


Spoofing incidents detected by Spire Aviation in fall 2024 and spring 2025 consistently showed directional resolution eastward toward Russian-occupied Crimea. High-altitude spoofing recorded by scientific teams matched flight-based telemetry captured by Spire, with false GNSS positions often snapping toward Simferopol or the vicinity of Ayvazovskogo Airport.


This directional spoofing, coupled with the persistent presence of mobile and sea-based EW systems observed by open-source analysts, suggests a strategic campaign emanating from Crimea and its surrounding waters. Rather than isolated bursts, the interference reflects a layered GNSS denial strategy designed to obscure military movement, mask drone activity, and challenge NATO situational awareness across air and sea domains.


### Pinpoint GNSS interference. Protect flight operations.


Access real-time global visibility into GNSS jamming using Spire’s satellite-based ADS-B data. Stay informed, stay safe.


[Talk to our Aviation data expert](https://spire.com/aviation/get-started/)


## Operational impact


The risks in this story go well beyond “GPS glitching.” GNSS spoofing in the Black Sea region affects:


- **Civil aviation:** False positions, navigation dropouts, and corridor drift
- **Maritime commerce:** Regulatory violations, collision hazards, and AIS spoofing
- **Scientific missions:** Data loss, corrupted baselines, and safety threats
- **NATO readiness:** Degraded ISR and unreliable GNSS across eastern flank FIRs


In contested or congested airspace, false confidence in location is more dangerous than signal loss. When aircraft or vessels believe they are in the right place, but are not, the result is blind operation under false assumptions.


Spire Aviation’s telemetry-based detection system helps uncover that illusion, alerting operators to invisible threats that traditional RF monitoring can’t capture in time.


**Get in touch to explore Spire’s GNSS‑interference data feed or request a demo:**


---


### Continue reading our GNSS interference report series


01: **[GNSS interference report: Russia – Part 1 of 4: Kaliningrad & the Baltic Sea](https://spire.com/blog/space-reconnaissance/gnss-interference-report-russia/)**
02:[GNSS interference report: Russia – Part 2 of 4: Crimea and the Black Sea Region](https://spire.com/blog/space-reconnaissance/gnss-interference-report-crimea-and-the-black-sea-region/)
03: **[GNSS interference report: Russia – Part 3 of 4: Moscow and major urban zones](https://spire.com/blog/space-reconnaissance/gnss-interference-report-moscow-and-major-urban-zones/)**
04: **GNSS interference report: Russia – Part 4 of 4: Black Sea & Romanian airspace (current)**


---
