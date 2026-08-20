---
schema_version: "1.0.0"
document_id: "b537f4f969dba9a0d502ba86bff9b3d9b08d5c67d0da11efbb33117ceb064ea1"
company_key: "yc-superunit"
company: "Superunit"
source_id: "yc-superunit-news-import-57b6d5b65af6"
canonical_url: "https://www.superunit.com/blog/how-to-scale-medical-canvassing-without-adding-siu-headcount"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-24T02:52:52.663509+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:97c669f54cbf5beb14080526206ff82da78fab5295deaf98f0171b42a1c5b169"
---

# How to Scale Medical Canvassing Without Adding SIU Headcount

## TL;DR


- Human canvassing throughput is capped by headcount and business hours. A canvasser can only work so many facilities per day, so volume spikes stretch turnaround across every file in the queue.
- Three constraints break first under scale: geographic scoping radius, simultaneous outreach capacity, and documentation consistency.
- AI canvassing removes all three without adding staff. Human canvassers still win on relationship-based access, complex escalation, and MSAs that restrict AI outreach.
- Scaling a program means negotiating specific SLA terms: a guaranteed turnaround ceiling, batch capacity, documentation format, and BAA chain of custody.


## Why Human Canvassing Can't Scale Past a Certain Volume


A human canvassing program hits its ceiling at the point where daily call capacity stops keeping pace with incoming files. One canvasser dials facilities in sequence, and each call takes as long as it takes. A facility in Phoenix cannot be reached at 8 a.m. Eastern, so a program spread across time zones loses working hours on both ends of the day. That capacity is fixed by how many people you employ and how many hours they can dial, which means throughput plateaus even as you add tools around it.


Volume spikes make the ceiling visible fast. When a batch of auto bodily injury referrals lands the same week as a routine workers' comp load, every file in the queue waits longer, not just the new ones. Your turnaround does not degrade gracefully but all at once, because the same canvassers now split their fixed hours across more work. Adding headcount is the standard response, but hiring and onboarding a canvasser takes weeks, and the surge that triggered the hire is usually over before that person is productive.


The cost shows up in the reserve decision, not the canvassing invoice. A canvass exists to tell an adjuster whether a treatment pattern holds up before reserves are set and a claim moves toward resolution. A result that arrives after that decision has already been made no longer changes the reserve. It becomes documentation for the file rather than research that drives action. A canvass landing two weeks late typically arrives after the reserve is posted, which turns useful research into a paper trail.


That lag compounds into time-to-close. Every file waiting on late canvassing intelligence sits open longer, and open files carry adjuster attention, reserve exposure, and the risk that a questionable treatment pattern goes unchallenged. The problem is not that your canvassers work slowly but that a serial, headcount-bound process cannot absorb a volume spike, and the delay lands squarely on the decisions the program exists to inform.


## The Three Bottlenecks a Scaling Program Must Solve


Three constraints break before anything else does. Facility scoping — how many providers a single file pulls in as you widen the geographic radius. Outreach throughput — how many of those providers you can actually contact at once. Documentation consistency — whether every canvass across a growing pile of files produces the same defensible record. Fix one without the others and the program still breaks, just at a different point.


### Bottleneck 1: Facility Scoping at Wider Geographic Radii


Widening the geographic radius on a file does not add facilities linearly. Widening it multiplies them, and a human canvassing model absorbs every added facility as another sequential call. A canvasser working a 25-mile radius might contact eight or ten facilities. Push that radius to 50 or 75 miles for a thorough soft-tissue sweep, and the same file now lists thirty or forty facilities, each requiring its own call, its own hold time, and its own callback if nobody answers. The canvasser handles them one after another, so turnaround grows in direct proportion to radius.


AI outreach breaks that proportion because it contacts every facility on the list at the same time. A 60-facility order across a wide radius completes in the same window as a 15-facility order across a narrow one, because phone, email, and fax calls fire in parallel rather than in sequence. The radius stops driving turnaround. You scope the file for investigative completeness instead of scoping it down to whatever a canvasser can finish before end of day.


Radius matters most on the claim types where treatment fans out across many small providers. A workers' comp or soft-tissue sweep typically has to reach chiropractors, pain management clinics, imaging centers, physical therapy practices, mail-order pharmacies, and cash-based practices that never bill through a carrier. Those cash-based and mail-order providers are exactly where a narrow radius misses relevant treatment, because a claimant will travel for a provider who does not generate a paper trail through normal claims channels. When adding those facilities costs nothing in turnaround, you can afford to canvass the full picture instead of the convenient one.


### Bottleneck 2: Simultaneous Outreach Throughput (and How Superunit Solves It)


A human canvasser works one call at a time. Each contact runs in sequence, so a 60-facility order takes four times as long to work as a 15-facility order, and a queue of a hundred such orders takes a hundred times as long as one. Throughput scales linearly with headcount and hours, which means the only way to double capacity is to double the payroll or the workday.


AI outreach runs in parallel instead of series. A single system contacts every facility on a file at once across phone, email, and fax, so a 60-facility order does not take longer than a 15-facility order. Volume behaves the same way at the batch level. A thousand simultaneous 60-facility orders complete at roughly the same speed as one, because the constraint is no longer how many calls a person can place in a day ([superunit.com](https://www.superunit.com/blog/best-medical-canvassing-companies-2026) ).


Parallel outreach decides whether you can absorb a spike. Auto bodily injury volume does not arrive on a predictable schedule. A multi-vehicle event or a wave of represented claims can triple a queue in a week, and you cannot hire and onboard qualified canvassers fast enough to meet demand that has already landed. A serial workforce forces you to choose between staffing ahead of demand you may never see and letting turnaround degrade for every file in the queue when the surge hits.


Parallel outreach removes that choice. The same system that handles a slow week handles the surge week, because capacity is not tied to headcount. Batch size stops being a variable you manage.


### Bottleneck 3: Consistent Documentation Across High File Volume


Documentation quality is where a scaling human program quietly accumulates risk. Every canvasser writes up a file in their own style, records outreach in their own words, and captures whatever detail they judged relevant that day. At ten files a month, that variance is invisible. At a thousand, it produces a body of reports where no two look alike, and where the gaps only surface when someone goes looking.


That variance becomes a problem at the worst possible moment. A canvass result matters most when it lands in litigation or a formal SIU review, and both scrutinize the same things. Who did you contact, when, through what channel, and what did they say. A report missing timestamps, a call with no recording, or an outreach log that says "attempted contact" with no method attached cannot support a fraud determination when opposing counsel picks it apart. Inconsistent audit trails do not just look sloppy, they create file-level exposure precisely where the file needs to hold up.


AI canvassing removes the variance because the record is a byproduct of the outreach itself, not a task a person remembers to complete. Every facility contacted generates a call recording, a transcript, a timestamp, and the specific channel used, applied identically across every file in the batch. A defensible canvass report needs exactly those four elements, and an automated system produces them the same way on file one and file ten thousand. The human canvasser who documents thoroughly on a light week and cuts corners during a volume spike is a structural weakness. A system that captures the full record on every outreach attempt, regardless of load, is not.


## When to Keep Human Canvassers Instead of Switching to AI


AI canvassing removes the headcount ceiling, but it does not replace human judgment or access in three situations that show up in real programs.


The first is relationship-based facility access. A canvasser who has worked a region for years and knows the office manager at a specific pain management clinic often gets a callback the same day, while a cold outreach attempt sits in a voicemail queue. That relationship is not something an AI system can manufacture, and on files where a single facility is stonewalling everyone else, a known human voice can break the logjam faster.


The second is complex escalation. When a facility gives an evasive answer, hedges on whether it treated a claimant, or raises a question the script did not anticipate, a skilled canvasser reads the hesitation and adjusts in real time. That kind of live interpretation matters most on the small number of files where the canvass result will be contested, and it is exactly where automated outreach reaches its limit.


The third is contractual. Some carrier and TPA master service agreements now include clauses that explicitly restrict AI in canvassing work ([Superunit](https://www.superunit.com/blog/best-medical-canvassing-companies-2026) ). When your MSA carries that language, AI canvassing is off the table for those files regardless of how well it would perform, and human staffing remains the only compliant path. Check the agreement before you route any batch to an automated vendor.


None of these are edge cases to dismiss. They define the boundary of where AI canvassing belongs, and a program that ignores them will misroute exactly the files that need a person. Use AI for the high-volume core and reserve human canvassers for relationship access, escalation, and MSA-restricted files.


## Which SIU Programs Benefit Most from AI Canvassing Like Superunit


Four program types account for most of the volume that pushes canvassing past a human staffing model. If your program matches one of these profiles, the parallel-outreach argument applies directly to your throughput problem.


High-volume workers' comp programs exhaust human staffing first. Once monthly canvass volume climbs into the thousands, you either hire ahead of demand or watch turnaround slip on every file in the queue. AI batch capacity holds turnaround flat as volume grows, because adding files to a batch does not add sequential calls for a person to work through. The queue depth stops mattering.


Auto bodily injury programs face a timing problem instead. Litigation waves and claim surges arrive without warning, and you cannot staff for a spike you did not forecast. Hiring and onboarding canvassers takes weeks, so by the time new staff are productive, the surge has already delayed your reserve decisions. AI parallel outreach absorbs the spike the day it hits, with no hiring cycle in between.


Soft-tissue claim sweeps are the clearest fit for wide-radius parallel outreach. A single sweep can span chiropractors, pain management clinics, mail-order pharmacies, podiatrists, fitness centers, and cash-based practices across a large geographic area ([Superunit](https://www.superunit.com/blog/best-medical-canvassing-companies-2026) ). A human canvasser works that list one call at a time. AI contacts every facility type at once, so the facility count and the radius stop driving turnaround.


TPA batch referrals match AI canvassing on pricing structure, not just throughput. TPAs send work in batches, and the human staffing model scales cost roughly in step with volume, since more referrals mean more canvasser hours. Subscription and volume-tiered pricing decouples cost from headcount, so a batch of 500 referrals does not cost five times what 100 did in labor ([Superunit](https://www.superunit.com/blog/best-medical-canvassing-companies-2026) ). For a TPA operations lead measuring cost per file across a book of business, that difference compounds quickly.


Programs that don't fit one of these four profiles — low volume, high relationship dependency, or MSA restrictions across the whole book — are better served by a hybrid arrangement than a full AI switch.


## What to Demand in a Medical Canvassing Vendor SLA


Turnaround commitment is the first term to pin down, and you need two numbers rather than one. Demand a published average and a guaranteed ceiling written into the contract. The ceiling is what protects you, because a canvass that lands two or more weeks late arrives after the reserve decision is already made. By then the result documents a file instead of shaping it. A vendor quoting only an average is quoting its best case, not its worst.


Batch capacity is where a vendor's real limits show. Ask directly whether turnaround holds when you send a large batch, and get the answer in writing. Human-staffed firms scale cost and time roughly in step with volume, so a batch of 400 files takes longer than a batch of 40. An AI vendor should be able to state that a batch of hundreds completes at the same speed as a single file, and you should hold them to it during a pilot.


Documentation format is easy to overlook until a file gets scrutinized. Require call recordings, transcripts, timestamps, and the outreach method used for each facility. Then request a sample report before you sign anything, because human documentation varies by individual canvasser while an automated system produces the same record every time. A vendor that cannot show you a sample report is telling you something.


BAA terms carry the compliance risk, and the default template is rarely enough. A HIPAA-compliant BAA must flow chain of custody down to any subcontractor that touches PHI, such as a cloud host or transcription service ([HHS](https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/index.html) ). Negotiate a breach notification timeline shorter than the regulatory default, and require return or destruction of all PHI at termination. Self-insured employers stay on the hook even when a TPA runs the plan, so the diligence is yours.


Some firms publish no turnaround SLA at all — Ontellus is one. Scaling with a vendor like that means every term above has to be negotiated into the contract explicitly, because nothing binds them by default.


## How Superunit Scales Medical Canvassing Without Adding Headcount


Superunit's outreach runs across phone, email, and fax simultaneously — no queue, no per-facility human step. A wider geographic radius adds facilities to the file, but it doesn't add time. A 60-facility scope and a 15-facility scope close on the same clock, so you scope for investigative completeness rather than for whatever a canvasser can finish before end of day.


Throughput carries no staffing ceiling. A thousand simultaneous 60-facility orders complete at the same speed as a single small order, which means an auto bodily injury surge or a workers' comp batch in the thousands does not push turnaround out for every other file in the queue. We hold a[24-hour average turnaround regardless of volume](https://www.superunit.com/blog/best-medical-canvassing-companies-2026) for that reason. Adding files does not add days.


Documentation follows the same logic. Every file produces call recordings, transcripts, timestamps, and the outreach method used per facility, generated automatically rather than assembled by whichever canvasser handled the case. Consistent records matter most at high volume, when a single missing timestamp or absent recording becomes the weak point an opposing expert probes during litigation.


Before committing, two steps matter. First, confirm the BAA covers subcontractors with data access — cloud hosts, transcription services, anyone touching claimant PHI. Superunit provides a[HIPAA-compliant BAA](https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/index.html) and the chain of custody flows down. Second, request a sample report and read it the way defense counsel would. If the audit trail on a single file holds up under that reading, it holds up across a batch of hundreds.


## Conclusion


Scaling a canvassing program without adding staff comes down to solving three constraints at once. You need to scope more facilities per file as radii widen, you need to reach all of them without waiting for a human to work through a call queue, and you need every file to produce the same defensible record no matter how many land in the same week. Human staffing solves any one of these by hiring, but the cost climbs in step with volume and turnaround still degrades under a spike. AI canvassing is the only model that removes all three at the same time, because its capacity does not depend on headcount. If you are still weighing specific providers against each other, the[vendor comparison](https://www.superunit.com/blog/best-medical-canvassing-companies-2026) breaks down turnaround, pricing, and documentation capabilities across the field.


## FAQs


### What does a HIPAA-compliant BAA need to cover for a canvassing vendor?


A[Business Associate Agreement](https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/index.html) must define permitted uses of PHI, require Security Rule safeguards, and mandate breach reporting under 45 CFR 164.410. A canvassing vendor's BAA also has to flow down to any subcontractor that touches claimant data, such as a cloud storage or transcription provider. Negotiate a breach notification timeline stricter than the default and require return or destruction of all PHI when the contract ends.


### Can AI canvassing be used if our MSA restricts automated outreach?


No. Some carrier and TPA master service agreements now include clauses that explicitly restrict AI in canvassing, and those terms control regardless of vendor capability. Check the MSA language before contracting an AI provider. Where a clause bars automated outreach, human canvassers remain the only compliant option for files governed by that agreement.


### How does turnaround time hold up when we send a batch of hundreds of files at once?


With human-only firms, a large batch competes for the same limited call capacity, so turnaround stretches for every file in the queue. AI parallel outreach does not degrade under batch size. Superunit completes 1,000 simultaneous 60-facility orders at the same speed as a single 15-facility order, holding a 24-hour average turnaround regardless of volume.


### What documentation does a canvass result need to include to be defensible in litigation?


A defensible canvass report needs call recordings, transcripts, timestamps, and the outreach method used for each facility. Human documentation varies by individual canvasser, which creates gaps that surface exactly when a file is scrutinized. AI systems generate this record automatically on every file. Request a sample report from any vendor before you sign.
