---
schema_version: "1.0.0"
document_id: "4b02d9e71ac3e203af780a52b878f567c9bfebffd965faabef3b326be68ed892"
company_key: "yc-korrai"
company: "KorrAI"
source_id: "yc-korrai-news-import-a686cf049d8d"
canonical_url: "https://www.korrai.com/blog/supercharging-insar-the-smallsat-opportunity"
published_at: null
first_seen_at: "2026-07-22T01:41:37.374471+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:85541426a0705d848674447a31e2a9ab4099145aaa8ec1aa72dc45a558cddfbc"
---

# Supercharging InSAR: The Small-Sat Opportunity

Satellite Interferometric Synthetic Aperture Radar (InSAR) has evolved dramatically since the early 1990s. What began as a niche scientific tool is now a trusted, globally deployed method for remotely measuring ground deformation across infrastructure, natural terrain, critical assets, and more.


Its elegance lies in its ability to detect subtle changes (down to millimeters) from hundreds of kilometers above Earth. Yet despite its remarkable precision, InSAR has long been constrained by several limitations that have held back its full potential.


So with the rapid growth and transformation of the space sector, a question emerges: *are small-sat SAR constellations poised to unlock the full potential of InSAR?*


## It’s Just a Phase


Across some three decades or so, InSAR has progressed through several defining phases. In the 1990s, Differential InSAR (which, at the time, was primarily used by the academic community), shed new light on deformation associated with natural hazards such as seismic events and volcanic activity. The early 2000s ushered in Persistent Scatterer Interferometry (PSI), enabling more reliable and precise time series monitoring which has been particularly useful in measuring subtle changes across urban areas and other manmade assets. The launch of Sentinel‑1(A) in 2014 fundamentally changed the landscape from a satellite imaging perspective, democratizing access to SAR data through free, routine, global acquisitions - a genuine inflection point in the recent history of the industry. More recently, cloud computing has slashed processing times from weeks to hours, and AI is accelerating how we process, classify, query, and interpret data.


But beyond these recent advances, might the next wave of innovation come from a rapidly growing segment of the satellite industry: a maturing small-sat SAR ecosystem.


The Evolution of SAR Interferometry


## The Limitations of Traditional SAR


To understand the opportunity, it’s important to review the constraints of today’s established SAR systems and architectures.


Most public and commercial SAR satellites operate in Sun‑Synchronous Orbits (SSO), traveling roughly north–south around Earth and imaging perpendicular to their direction of travel. This geometry introduces a limitation: InSAR is largely insensitive to deformation that trends in a north–south direction. For assets such as open‑pit mine slopes or natural terrain oriented in these directions, this creates an InSAR blind spot which even the combination of ascending/descending imaging can only partially mitigate.


A second constraint is revisit frequency. Traditional SAR missions operate as standalone satellites or small fleets, meaning days of delay (sometimes more) between each acquisition at the same location on Earth. For rapidly evolving hazards or non-linear deformation, these time gaps can lead to under-sampling and reduced measurement fidelity.[InSAR excels when the environment it is imaging remains coherent between acquisitions](https://www.korrai.com/blog/introduction-to-insar-guide-for-geotechnical-engineers-risk-analysts-and-safety-engineers#:~:text=targets%20and%20coherence.-,Coherence%20%26%20Target%20Suitability,-InSAR%20tracks%20features) ; long time intervals can introduce noise, signal decorrelation, and uncertainty driven by changes in ground features (natural or manmade). The shorter the time gap, and[the right satellite and acquisition strategy](https://www.korrai.com/blog/insar-in-practice-choosing-the-right-data-for-your-project) for the application at hand, the better.


## The Small‑Sat Revolution


A new generation of SAR satellites (small-sats) from established companies such as[Capella Space](https://www.capellaspace.com/) ,[ICEYE](https://www.iceye.com/) ,[PierSight](https://piersight.space/blog/release-piersight-to-provide-sovereign-sar-backbone-for-india-s-ppp-earth-observation-constellation) ,[Umbra](https://umbra.space/) and[Synspective](https://synspective.com/) , is redefining what’s possible in the SAR domain. Their constellations introduce several advantages that could potentially address long‑standing InSAR limitations:


- **Higher resolution:** Modern SAR small-sats deliver meter‑ to sub‑meter‑scale imagery, with Umbra achieving resolutions down to 16 cm. For InSAR, this could offer much denser measurement networks, improved phase unwrapping, and the ability to resolve spatial features that are currently indistinguishable.
- **Increased scale & revisit:** Constellations of satellites enable daily or even sub‑daily acquisitions at the same location on Earth. Faster revisit rates reduce temporal decorrelation (changes between images), improve measurement reliability, and allow measurement time series to build up at a much faster rate, allowing monitoring programmes to commence earlier than is possible today.
- **Expedited monitoring programmes:** InSAR techniques such as PSI require a minimum number of images (typically around 15) before deformation time series can be reliably extracted. At a 6–12 day revisit cycle, traditional satellites take three to six months to reach that threshold. Small-sat constellations have the ability to achieve the same image count in weeks, allowing monitoring programmes to yield their first results much earlier than it is possible today.
- **New orbit geometries** : Some satellite operators, such as Capella Space, have deployed satellites into Mid‑Inclined Orbits (MIO), not just SSO which is a typical industry standard. As a result, MIO imaging geometries open the possibility to north–south deformation sensitivity.
- **Improved measurement quality & fidelity** : Higher resolution and shorter acquisition intervals reduce the time available for ground-cover, or other features, to change, preserving signal coherence and enabling more detailed, spatially contiguous measurement networks. More frequent image acquisitions have the potential to improve measurement fidelity (e.g. associated with capturing non-linear deformation trends), bringing us a step closer to real-time monitoring.


Combined, these factors would represent a fundamental shift, not just incremental improvement, in how InSAR could evolve and be more widely applied.


## The Big Hurdle: Precise Orbits


For all their promise, small-sat SAR systems face one major challenge: orbital precision.


InSAR requires satellites to maintain extremely consistent, well‑characterized orbits, every orbit. Even small deviations can introduce phase errors that impact the ability to derive an accurate deformation signal – something that is even more crucial when building out measurement time series. Traditional SAR satellite missions achieve this through large satellite buses and propulsion systems, high‑precision GNSS receivers, and ample fuel reserves.


Small-sats, by contrast, operate under tighter mass, power and fuel constraints.


Operating SAR satellites at the level required for interferometry comes down to maintaining extremely tight orbital repeatability — ensuring that every pass falls within a very narrow corridor in space. That demands two things: very accurate orbit determination, enabled by precise GPS and time synchronization with onboard systems, and the ability to manoeuvre effectively using onboard propulsion.


For small satellites, the hardest trade-off is that propulsion, mass, and power are all constrained, so every manoeuvre has to be both precise and fuel-efficient to preserve mission life.


Gaurav Seth


CEO & Cofounder of PierSight


PierSight is building an upcoming SAR constellation focused on maritime surveillance, designed to deliver persistent, all-weather visibility over oceans and coastal regions. The company is also part of a PPP consortium working to develop and deploy sovereign SAR satellite capabilities, helping expand access to strategic radar EO infrastructure.


## Rising to The Challenge


Despite these constraints, some enticing progress has been recently demonstrated. ICEYE and Capella Space have publicly showcased SAR interferograms generated from well‑matched image acquisitions (see examples below). Some of these have been opportunistic (the right geometry, the right timing, the right conditions) but they signal capability, progress and glimmer of future potential.


Industry intel suggests that small-sat operators are now dedicating more energy and resource to enabling routine interferometric capability. And that’s the key: InSAR-compatible data *must* be achievable on every orbit for the life of the satellite. Downstream users depend on predictable, consistent measurements for operational monitoring, and ad hoc interferometric opportunities, whilst useful, aren’t sufficient for robust mission-critical applications and monitoring programmes.


*Deformation pattern of the Fagradalsfjall Volcano, Iceland immediately* ***before*** *eruption. Each fringe represents a deformation of ½ of a wavelength (~1.5 cm). Copyright*[ICEYE](https://www.iceye.com/blog/beyond-change-detection-measuring-the-changes-that-matter) *.*


*Deformation pattern of the Fagradalsfjall Volcano, Iceland immediately* ***after*** *eruption. Each fringe represents a deformation of ½ of a wavelength (~1.5 cm). Copyright*[ICEYE](https://www.iceye.com/blog/beyond-change-detection-measuring-the-changes-that-matter) *.*


InSAR data across Paris, France (Eiffel Tower shown in zoom). Copyright[Capella Space](https://www.capellaspace.com/blog/advancements-in-interferometric-synthetic-aperture-radar)


## The Opportunity on The Horizon


Closing the orbital‑precision gap would unlock a step‑change for the entire InSAR ecosystem. With the ability to observe a wider range of deformation phenomena (more frequently, at higher resolution, and with new geometric sensitivity) we should witness wider adoption across a range of industries, fewer blind spots in measurement detection, more robust and tactical monitoring strategies, and deeper scientific understanding of complex deformation phenomena and natural hazard processes.


Small-sat InSAR isn’t just about faster or more visually appealing measurements, it represents the potential for a fundamental shift in how we monitor the planet: from critical infrastructure and mining operations, to natural hazards and urban environments.


> Small-sat SAR can change what we measure and inform at infrastructure scale. When you revisit the same asset daily (or faster) and keep resolution high, you stop treating SAR as occasional context and start treating it as an opertional data layer of risk. For hyper-focused infrastructure monitoring, especially those under construction or daily operations, there is potential earlier risk detection, and tighter feedback loops.
>
>
> - Rahul Anand, Cofounder of KorrAI


If the engineering and economic challenges can be overcome (we haven’t touched on the cost of such data in this article!), the payoff for the InSAR community, and for global decision makers, could be profound.


To learn more about the state-of-the-art in InSAR today and how it can meet your remote monitoring needs, please reach out to us at contact@korrai.com
