---
schema_version: "1.0.0"
document_id: "914f66a7891a0a920dca8f7d6fb06f6bafbddf995e6940bc05723078ca5c4301"
company_key: "yc-farel"
company: "Farel"
source_id: "yc-farel-rss-64b3b2dac96a"
canonical_url: "https://farel.io/company/blog/dcs-common-use-certification/"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:17.436446+00:00"
fetched_at: "2026-07-28T20:48:41.890548+00:00"
content_hash: "sha256:107ae0ca9d474c554f7ee66621e71a8f2ad430e950c4ef8ea8a39f9a514e9b02"
---

# What DCS common-use certification actually takes

## Key takeaways


- A departure control system (DCS) can't run on shared airport check-in and boarding hardware until it's certified against the common-use platform that hardware speaks to.
- Farel, a cloud-native airline operating system, passed RESA's CREWS CUPPS (CUTE) certification - the Paris lab on 1 June 2026, then live validation at Tallinn Airport (TLL).
- Common-use certification is an invisible barrier to entry, and part of why the list of real DCS vendors stays short.
- The certified piece is the integration layer, pinned to a fixed runtime, so Farel recertifies on a 2–3 year cycle and ships every airline partner the latest version at no extra cost.
- Watch the airport network: it's often run by a third party (SITA or ARINC), not the airport, with a separate agreement and a monthly fee airlines have to budget for.


Most software only has to satisfy a screen. A departure control system has to satisfy a wall of printers, scanners, and boarding-gate readers it has never met, at an airport it has never seen, speaking protocols it doesn't get to argue with.


That wall is the real reason a DCS is hard to build. A DCS checks passengers in, prints boarding passes and bag tags, and boards the aircraft. At most airports it doesn't get its own hardware - it shares the desks and gates with every other airline through a common-use platform. And that platform doesn't care how good your software is until you prove you speak its protocols, exactly.


Farel is a cloud-native airline operating system, and our DCS just cleared that bar. We passed RESA's CREWS CUPPS (CUTE version) certification in two steps: the lab in Paris, next to CDG, on 1 June 2026, then a live test at Tallinn Airport. Here's what each step took, the one airport cost nobody warns you about, and why it matters to airlines more than the word "certification" lets on.


## What is common-use certification, and why does every DCS need it?


Common-use means the check-in desks and boarding gates are shared infrastructure. Instead of every carrier bolting its own kit to every station, airlines run their software on a common-use platform that drives the shared peripherals. RESA's platform, CREWS, runs check-in and boarding at more than 300 airports.


Two standards govern it. CUTE (Common Use Terminal Equipment) is the old one, and still everywhere. CUPPS (Common Use Passenger Processing Systems, IATA Recommended Practice 1797) is its successor. Plenty of airports still run CUTE, so certifying for the CUTE interface is what gets you onto real desks today.


This is the invisible barrier to entry. A booking engine you can judge on a screen. A DCS only counts once it drives physical hardware correctly, every time, with nobody stranded at a gate. That's a big part of why the list of vendors who can actually run airport departure control is so short.


## Why does certification happen in two steps?


Because a lab and an airport test different things. The lab proves protocol conformance - your software talks to the platform and its devices correctly in every state the standard defines. The airport proves it on the real hardware, the live airport network, with real agents at the desk. Lab first, then the airport.


## What actually broke in the lab?


We booked the slot in early March 2026. The test ran at the end of May. You don't just walk into one of these. Thanks to RESA for the slot - and, accidentally, for the timing: late May in Paris is Roland Garros. As an active tennis player and lifelong fan, I won't pretend I minded. I caught a few of the greats between builds.


Then the actual work. Over the week of 25-29 May we shipped 18 builds. Build 18 is the one that passed.


It's deliberately bare-metal. You get an SDK to reach the platform and the AEA 2009/2012 specs for the devices, and after that the platform is the only source of truth. Every loop looked the same: upload a build, ask RESA for the logs, find exactly where we'd offended the protocol, fix it, go again. No simulator to hide behind.


None of the hard part is the happy path. It's locking and unlocking devices, handling each one online, offline, and powered off, paper jams and empty trays, connection time-limits, and running a stack of devices at once. Some are only allowed at check-in, like passport and document scanners; some only at the gate, like boarding-gate readers - and the software has to know the difference. The final configuration covers bag-tag and boarding-pass printers, document and magnetic-stripe readers, barcode scanners, and boarding-gate readers, up to 16 at once, on Windows 10/11.


The most expensive lesson was the most ordinary one. Early on we did the modern thing and updated our frameworks, the bundled Chromium included, to the latest versions. Then nothing ran. RESA's environment is 32-bit, and the latest Chromium isn't. We rolled back to Chromium 116, the build that still ships 32-bit, and that's exactly what the certificate pins. One line that sums up the whole exercise. The environment doesn't bend to your stack; your stack proves it fits the environment.


Jérôme Elineau and Sophan Quach at RESA ran us through it - sharp on the hardware, patient with our edge cases. Sophan signed the certificate.


## Step two: did it hold up at a live airport?


Yes, and faster than the lab. We validated at Tallinn Airport (TLL) with the airport's ground-handling team, a RESA engineer on site, and NYX Air on a test flight. Check-in and boarding ran on the real common-use hardware, on the real network, with the certified build doing exactly what it did in Paris. Quick and clean.


A lab can't reproduce everything a working airport throws at you. That's the point of step two. And the one real surprise had nothing to do with our software.


## Who actually provides the network at the gate?


Here's the cost nobody puts in the headline quote: the network at the desk and gate often isn't the airport's. At a lot of airports it's run by a third party, usually SITA or ARINC (now Collins Aerospace), and the airline signs a separate contract and pays a monthly fee to use it.


At Tallinn, that's SITA. The number that stopped us was about $1k a month for a 128 Kbps line, before setup and a multi-year minimum. For one airport. That's less bandwidth than a home DSL line from 2005, at four figures a month per station.


It's a small line item that says something true about the business. Airlines run on thin margins, and the costs hide in places like this. If you're planning a new station, price the connectivity before you sign - it's rarely in the airport's headline number.


## What does common-use certification mean for airlines?


This is the part that outlives the milestone. On legacy departure control systems, recertification and upgrades are a recurring, usually billable cost, and airlines often end up several versions behind because of it. On Farel it works the other way, by design.


The certified thing is the common-use integration layer - the piece pinned to that fixed runtime, down to the exact browser and platform versions. Touch that layer and you recertify. Everything else, the rest of the DCS and the wider platform, ships continuously and leaves it alone.


Legacy DCS (e.g. Amadeus Altéa, Sabre SonicW) Farel


Recertification cadence Recurring, vendor-driven Every 2-3 years, Farel-run


Who pays for recert and upgrades Typically the airline Included


Version airlines run Often several releases behind Always the latest


New product features Tied to the upgrade/recert cycle Ship continuously; don't touch the certified layer


So we recertify roughly every 2-3 years, mostly to refresh backend frameworks and close vulnerabilities - the deliberate, tested kind of update the Chromium rollback shows you can't skip. Airline partners don't pay for recertification or upgrades, and they're always on the current version instead of waiting on a release cycle. The certification covers Farel's[departure control](https://farel.io/platform/departure-control-system/) module - one part of the wider operating system airlines run on Farel.


Share
