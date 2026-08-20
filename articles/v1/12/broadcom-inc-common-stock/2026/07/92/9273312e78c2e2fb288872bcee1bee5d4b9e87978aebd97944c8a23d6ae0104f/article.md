---
schema_version: "1.0.0"
document_id: "9273312e78c2e2fb288872bcee1bee5d4b9e87978aebd97944c8a23d6ae0104f"
company_key: "broadcom-inc-common-stock"
company: "Broadcom Inc."
source_id: "broadcom-inc-common-stock-rss-6823269edf1e"
canonical_url: "https://news.broadcom.com/mainframe-software/chefs-kiss-how-xcoms-transfer-control-xtc-runs-your-data-kitchen"
published_at: "2026-07-20T18:26:55+00:00"
first_seen_at: "2026-07-20T23:21:13.318217+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:0ab2b0bf044f0e64702184ff74659f315e82c191c1a1167c69e865e87072b6cf"
---

# Chef’s Kiss: How XCOM’s Transfer Control (XTC) Runs Your Data Kitchen

-
-
-


Picture this: It's Saturday night at the fanciest restaurant in town. The kitchen is absolutely popping, pans sizzling, timers dinging, chefs yelling "yes, chef!" like their lives depend on it. Every single dish has to come out in the right order, at the right time, or the whole table's night is ruined. Mess up the timing? Chaos. Nail it? Pure, beautiful, Michelin-starred magic.


Well, buckle up, because that's EXACTLY how XCOM's Transfer Control (XTC) works. And honestly? It's pretty delicious.


**The Problem: Your Files Are a Needy Little Tasting Menu**


Here's the thing about enterprise file transfers: they don't like being independent. They're clingy. They have *needs* .


A payroll extract needs to land on the mainframe *before* the overnight batch job kicks off. A database file needs to transfer successfully *before* the reporting system triggers its load process. If you've ever had to manually babysit a sequence of transfers, watching one finish just so you can kick off the next... yeah. We've all been there. It's not fun. It's not glamorous. And it definitely doesn't pair well with a nice Bordeaux.


XTC swoops in like a seasoned maître d' and says, *"Don't worry, I've got the whole menu handled."* You define the dependencies once, and the system takes it from there.


**The Executive Chef's Magical Order Ticket**


Think of XTC as the executive chef's all-knowing, never-sleeping order ticket system, one that doesn't just *track* what's cooking, but actively holds every station accountable. No coffee breaks. No "oops, I forgot." Just perfectly sequenced data.


Here's the full menu of how it maps:


**Fine Dining Concept** **XTC Concept**


The full tasting menu for a table (e.g., "Table 7 Service") **XTCNET** — your transfer group's name


An individual course (e.g., "Amuse-bouche", "Entrée") **XTCJOB** — one specific transfer in the group


A course passing (or flunking) the chef's quality check Transfer success or failure outcome


Chef barking instructions when a course is done Action flags like **XTCGOODREL** , **XTCERRDECR**


A dish sitting in the warmer Transfer in **HOLD** status


Chef shouting "FIRE!" to start the next course **Releasing** a held transfer


Scratching a course from the menu entirely **Purging** a transfer request


**How It Works: Dinner Service, But Make It Data**


When the amuse-bouche leaves the pass without incident, the chef doesn't sit around waiting for an email about it; they immediately call fire on the first course. Perfect conditional execution: one outcome, one trigger, automatic next step.


XTC is exactly that chef, except it never gets tired, never burns the hollandaise, and never accidentally fires two courses at once. When a file transfer completes (or fails), it can trigger up to **eight downstream actions** automatically:


- **Release** a waiting transfer *"Amuse-bouche is away! Fire the first course!"*
- **Hold** a transfer that's not ready yet *"Don't you dare plate that fish until the beurre blanc is done!"*
- **Purge** a transfer that's no longer needed *"Guest has a shellfish allergy — scratch the bisque"*
- **Increment or decrement** a hold counter *"We're waiting on the garnish AND the sauce, don't fire until BOTH are ready!"*


You set all this up using **action flags** , snazzy little parameters like XTCGOODREL (if this transfer succeeds, release the next one) or XTCERRDECR (if this transfer flops, tick down the hold counter on the next job). It's like giving your data pipeline its very own Gordon Ramsay. Except hopefully less cursing.


**Setting Up Your Tasting Menu (a.k.a. Your Transfer Network)**


Every course at a table belongs to the same dinner service. In XTC terms, that shared identity is your XTCNET name (up to 8 characters) like a reservation name on the host stand. Every individual transfer gets its own XTCJOB name, so the kitchen (system) always knows exactly which course is which and who's waiting on whom.


Here's what a simple two-course service looks like under the hood:


` Transfer A:
XTCNET=PAYROLL1
XTCJOB=EXTRACT
XTCGOODREL=LOAD ← "If I nail it, release the LOAD course — fire away!"


Transfer B:
XTCNET=PAYROLL1
XTCJOB=LOAD
(starts in HOLD — patiently waiting in the warmer for Transfer A)`


Transfer A finishes like a pro? XTC releases Transfer B automatically, no human needed, no instant message required, no one hovering over a terminal at midnight. Transfer A crashes and burns? Transfer B stays on hold. No bad data sneaks downstream. The kitchen does not send out burnt steak.


**Where XTC Runs (Spoiler: It's Not Just the Mainframe Kitchen)**


Good news XTC is part of XCOM Data Transport's whole ecosystem, and it runs on:


- **z/OS** (the classic, fancy, French Laundry of computing environments)


- **Linux, AIX, and Windows** (the hip modern bistros of the distributed world)


So whether you're moving data from a mainframe to a Linux analytics platform, or orchestrating transfers across a whole mixed environment, XTC's got the menu covered.


**A Word of Caution: Don't Leave Dishes in the Warmer**


Imagine a dish goes into the warmer to hold... and then the ticket disappears. Nobody knows it's there. It just sits. And sits. Getting sadder and sadder and definitely not improving with time. Meanwhile, the whole station is blocked and nobody can figure out why Table 7 never got their entrée.


That's what happens when an XTC transfer gets stuck in **HOLD** limbo. If a logic error or unexpected failure leaves a transfer hanging, **it won't free itself** . Someone has to go in, find it, and manually release or delete it via the XCOM server.


So before you send your tasting menu to production, ask yourself the hard questions:


- *What happens if Transfer A fails, does Transfer B know to stand down?*


- *Is there a purge condition to clean up the ticket if things go sideways?*


- *Are there any transfers that could get stuck in the warmer forever?*


**Plan for the messy nights, not just the perfect service.** Even the best restaurants have fire drills.


## **The Bottom Line: Your Data Deserves a Michelin Star**


XCOM's Transfer Control feature turns a sloppy, "fingers-crossed-and-watch-it-manually" chain of file transfers into a **sleek, self-orchestrating data tasting menu** . Name your network with` XTCNET` , give each transfer its` XTCJOB` identity, wire up your action flags, and let XTC do the expediting while you go touch some grass.


It's automated. It's elegant. It's conditional. It's... *chef's kiss.*


Just don't leave anything sitting in the warmer.


*XTC parameters can be configured via XCOM` .CNF` files or submitted directly to the XCOM server. For platform-specific implementation details, check out the*[Broadcom XCOM documentation](https://techdocs.broadcom.com/us/en/ca-mainframe-software/traditional-management/ca-xcom-data-transport-for-z-os/12-0/using/the-batch-interface/sysin01-parameters/transfer-control-xtc-parameters.html) *.*


Now that you are one step closer to being an expert, prove your expertise on XCOM by earning a Credly Badge:


**XCOM™ Data Transport® for Distributed Environments - Digital Badge Assessment**


- Credly Link:[Credly Badge](https://www.credly.com/org/broadcom/badge/xcom-data-transport-for-distributed-systems-fundame)


- [XCOM Course List](https://ent.box.com/s/8bydjgs3l727ebnzeh31fl50axoc16d1)


- Course Code: 06XCM20210E


**XCOM™ Data Transport® for z/OS - Fundamentals**


- [Credly Badge](https://www.credly.com/org/broadcom/badge/xcom-data-transport-for-z-os-fundamentals)


- [XCOM Course List](https://ent.box.com/shared/static/xzvt4580roehnac2asp67iq5zgtims3l.pdf)


- Course Code: 06XCM20200E


[HOW TO ACCESS TRAINING IN LEARNING@BROADCOM](https://app.sw.broadcom.com/e/er?s=3805888&amp;lid=14052&amp;elq=~~eloqua..type--emailfield..syntax--recipientid..encodeFor--url~~&amp;elqak=8AF5FEED2E0E5A55FD0EBAE374FB633864A5B39FB4C75AF02BC4C934F023E428125F)


[LEARN MORE](https://engage.broadcom.com/mainframe-xcom)


About the Author


About the Author


[Kenneth Vollmer](https://news.broadcom.com/author/kenneth-vollmer)


Software Engineer, MFD Mainframe R&D
