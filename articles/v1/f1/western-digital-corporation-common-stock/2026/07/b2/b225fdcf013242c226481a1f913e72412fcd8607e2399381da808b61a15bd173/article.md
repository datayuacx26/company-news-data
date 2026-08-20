---
schema_version: "1.0.0"
document_id: "b225fdcf013242c226481a1f913e72412fcd8607e2399381da808b61a15bd173"
company_key: "western-digital-corporation-common-stock"
company: "Western Digital Corporation"
source_id: "western-digital-corporation-common-stock-rss-a1b03adbb2f1"
canonical_url: "https://blog.westerndigital.com/future-of-enterprise-hdds-32tb-100tb/"
published_at: "2026-07-14T18:17:52+00:00"
first_seen_at: "2026-07-20T23:21:38.045244+00:00"
fetched_at: "2026-07-22T04:42:20.385484+00:00"
content_hash: "sha256:493a0a408b16bb13cf605d136372581fb4bd69906d485d310d528dabb065147e"
---

# The Future of Enterprise HDDs: From 32TB to 100TB

*TL;DR: Enterprise HDD storage is at an inflection point*


Conventional, non-energy-assisted PMR has already reached a practical ceiling around 28TB. ePMR, an energy-assisted recording technology, reaches 26TB on its own and 32TB paired with UltraSMR, itself a capacity lever worth adopting, not just a bonus feature. HAMR is the next leap beyond it, advancing through hyperscale qualification toward a 100TB milestone WD has targeted for 2029.


## Key takeaways


- **Conventional PMR recording has already reached its practical ceiling at roughly 28TB per drive.** The limit isn’t commercial demand: non-energy-assisted write heads simply can’t generate a field strong enough to write to smaller, thermally stable grains.
- **ePMR and HAMR are WD’s two paths to 60TB and 100TB+ per spindle.** ePMR can reach 26TB on its own and 32TB paired with UltraSMR; HAMR is in hyperscale qualification, with ramp production targeted for 2027 and 100TB targeted for 2029.
- **UltraSMR is a capacity driver in its own right, not a feature riding along on ePMR.** It’s a drive format, not a software layer, that pairs shingled track geometry with OptiNAND’s flash-based metadata offload, and already underpins a 40TB ePMR drive in hyperscale qualification. Organizations that invest now in the host-side software and firmware changes it requires help unlock meaningfully more capacity from the same physical platform.
- **AI training pipelines and petabyte-class data lakes have made the “add more drives” approach unsustainable.** Floor space and power per cabinet are fixed; consolidating 40PB across 800 drives at 50TB each instead of 2,000 drives at 20TB cuts rack space, cooling load, and labor overhead per petabyte.
- **Organizations deploying 50TB+ drives should evaluate erasure coding and distributed RAID before the transition, not after.** Extended rebuild windows are the main risk for those still on traditional RAID at ultra-high capacity; modern protection architectures can solve this directly.


## **The storage ceiling is an AI infrastructure problem**


Enterprise data isn’t just growing, it’s compounding: AI infrastructure, video analytics, IoT telemetry, and compliance retention are outpacing what incremental upgrades can absorb. Organizations on 16TB to 32TB drives today will exhaust rack space and power within two to three refresh cycles, making the real question how to move to higher-capacity drives on a planned timeline, not in crisis mode.


## **From conventional PMR to energy-assisted recording: A necessary evolution**


Perpendicular Magnetic Recording (PMR) underlies every modern enterprise HDD. CMR and SMR are track geometries, not recording technologies in their own right, and can be paired with non-energy-assisted PMR, ePMR, or HAMR alike. ePMR and HAMR are what push density further, but through different mechanisms: ePMR applies energy at the write head to stabilize its field, while HAMR uses localized heat to temporarily reduce media coercivity during the write process.


Non-energy-assisted PMR has carried the industry to roughly 28TB (24TB in CMR) through incremental platter stacking and track density gains, and has now hit a wall WD’s own engineering team calls the[recording trilemma](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/collateral/tech-brief/tech-brief-recording-technologies.pdf) : smaller, denser grains need higher-anisotropy media to stay thermally stable, but writing to that media takes a stronger field than a non-energy-assisted head can generate. ePMR and HAMR each likely solve this directly. WD introduced ePMR years ago at 18TB and has already extended it to 26TB on its own, and to 32TB paired with UltraSMR.


UltraSMR works alongside ePMR and HAMR as a separate, complementary capacity lever: not a software add-on, but a shingled write format that unlocks meaningfully more capacity from the same physical platform once a customer’s storage stack supports it. WD’s 40TB UltraSMR-paired ePMR drive is already in hyperscale qualification, with volume production planned for H2 2026. Because it depends on sequential, host-managed SMR writes, it suits sequential-write workloads better than random-write-heavy ones, and that host-side compatibility work is worth starting now, since it’s the gating factor on those capacity gains, not the drive itself.


The transition is already underway: WD’s roadmap shows ePMR scaling to 60TB and HAMR reaching[100TB by 2029, with HAMR in hyperscale qualification today and ramp production targeted for 2027](https://www.westerndigital.com/company/newsroom/press-releases/2026/2026-02-03-western-digital-accelerates-storage-innovation-for-ai-era) .


## **Where the 32TB HDD fits in the transition**


The 32TB[Ultrastar DC HC690](https://www.westerndigital.com/products/internal-drives/data-center-drives/ultrastar-dc-hc690-hdd) is WD’s highest-density ePMR/UltraSMR platform in broad production today, with validated compatibility across HBA ecosystems. It’s a reliable option for organizations mid-refresh cycle that delivers strong TCO now while preserving runway for what comes next. For storage architects building a longer term, it’s the current step in an ongoing capacity progression: each generation, including the higher-capacity ePMR and HAMR drives ahead, builds on the same validation and ecosystem work already in place, so deploying it now means structuring rack density, cabling, and power budgets to keep future upgrades incremental rather than disruptive.


## **WD’s approach: Purposeful, customer-centric innovation**


WD has consistently prioritized qualification rigor and customer-validated deployment. That same[customer-centric approach](https://blog.westerndigital.com/purposeful-progress-storage-innovation/) shapes its HAMR and ePMR transition:


- **Parallel investment across recording architectures,** backed by direct collaboration and validation cycles with hyperscale and enterprise OEM partners, reduces single-path risk and confirms real-world performance before drives ship in volume
- **Direct roadmap visibility,** shared with customers and ecosystem partners, gives procurement teams the lead time to plan refresh cycles with confidence


## **Addressing rebuild risk at scale**


At hyperscale and large-enterprise deployment, traditional hardware RAID is no longer the default. Many organizations on 30TB+ drives have already moved to erasure coding or distributed RAID built for this scale. For the subset still on RAID-5 or RAID-6, a single drive failure at ultra-high capacities can stretch rebuilds to days, not hours, raising the risk of a second failure during recovery. That’s exactly why organizations should evaluate erasure coding or distributed RAID configurations that spread rebuild load across many devices, rather than waiting until after the transition.


## **Planning for the next decade of storage**


The HDD capacity roadmap from 32TB toward 100TB is a multi-year transition, not a single product launch: one that will shape storage economics and operational complexity for years. Organizations that start planning now, from data protection to energy-assisted recording readiness, will be better positioned to absorb AI data growth instead of facing repeated expand-and-migrate cycles.


The drives are advancing. The question is whether your infrastructure strategy is ready to use them effectively.


For more on how CMR, SMR, ePMR, and HAMR relate to one another, see WD’s[Recording Technologies Tech Brief](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/collateral/tech-brief/tech-brief-recording-technologies.pdf) ; for more on UltraSMR specifically, see the[UltraSMR Technology Brief.](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/collateral/tech-brief/tech-brief-ultrasmr-technology.pdf)
