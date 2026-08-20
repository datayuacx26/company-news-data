---
schema_version: "1.0.0"
document_id: "58eaf4a4ddb3b73b4cd385781028f9b3b079caeb174c19e42dbc7675d89f7731"
company_key: "yc-govly"
company: "Govly"
source_id: "yc-govly-news-import-a238b411d40a"
canonical_url: "https://www.govly.com/blog/where-the-real-federal-opportunities-hide"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-21T22:05:24.108423+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:bd624d94d0ebeb4b2f409649e0337f6c88d2da9476e42272dd3a8a858d0c8ab6"
---

# Where the Real Federal Opportunities Hide and Why SAM.gov Will Never Show Them to You

A few weeks ago, a BD lead at a federal contractor told us this on a discovery call:


‍


*"It's on ITES. But here's the deal, they put it on an RFI. I couldn't find it. I've never once been told a government person put something out for purchase and it was not public data."*


‍


We hear some version of that line at least once a week.


‍


Here's what's actually happening when a contracting officer "puts something out for purchase" and you can't find it: the opportunity is real, the dollars are real, and a small set of contract holders can see it. You just aren't one of them and SAM.gov isn't going to fix that for you.


‍


If you're a capture manager or BD exec who has a nagging sense that opportunities are being awarded in your service line without ever crossing your desk, this article is for you. We'll cover where federal opportunities actually live, why the big public sources miss most of them, and how to start hunting where the real pipeline is.


‍


## The myth of the "single front door" for federal opportunities


The pitch every new contractor hears is simple: register on SAM.gov, set up some saved searches, and you'll see "all" the federal opportunities. It is one of the most expensive misconceptions in our industry.


‍


SAM.gov is the front door to a specific slice of federal procurement: open-market solicitations, sources sought notices, and announcements that contracting officers are required to publicize. It is essential, but it is incomplete by design. The Federal Acquisition Regulation actively encourages, and in many cases requires, agencies to use existing multiple-award contracts and indefinite-delivery vehicles instead of running open competitions. When they do, the opportunity gets posted inside the vehicle holder's private portal, not on SAM.gov.


‍


In practice, that means an Army office buying laptops won't post on SAM.gov, they'll post on CHESS. A DoD office buying IT services often goes to ITES instead. A civilian agency buying off the GSA Schedule will post on eBuy. A Navy program buying engineering services lives in SeaPort-NxG. And so on.


‍


If you aren't a contract holder on the vehicle, you don't get an automatic seat at the table. The opportunity exists, but it isn't visible to you. That is what the Galvanick BD lead was describing on our call.


‍


## The 10 federal portals SAM.gov won't fully show you


Here are the contract vehicles and portals where opportunities go up and rarely (if ever) reach SAM.gov. Vehicle holders compete for these task orders or delivery orders behind a login screen.


‍


**1. Army CHESS (Computer Hardware, Enterprise Software and Solutions)** The Army's mandatory source for commercial IT hardware and software. Task orders run through ITES-SW2, ITES-3H, and similar vehicles inside CHESS. If you're selling IT to the Army and you're not watching CHESS, you're guessing.


‍ **2. ITES (Information Technology Enterprise Solutions)** A family of Army vehicles — ITES-3S (services), ITES-SW2 (software), ITES-3H (hardware). Multi-billion-dollar ceilings, and the task orders within them rarely surface on SAM.gov.


‍ **3. DLA ITAS (Internet-Based Tools for Acquisition Services)** The Defense Logistics Agency's procurement portal for everything from commodities to professional services across DLA's six supply chains. A massive, often-overlooked spend stream.


‍ **4. GSA eBuy** The buy-side companion to GSA Advantage. Civilian and DoD agencies use eBuy to issue RFQs against the GSA Multiple Award Schedule. Most eBuy opportunities never appear on SAM.gov — they're scoped to Schedule holders only.


‍ **5. GSA STARS III** Governmentwide 8(a) IT services vehicle. Task orders run inside the vehicle, not on SAM.gov.


‍ **6. MDA Shield** The Missile Defense Agency's program-specific vehicle. Highly classified and highly specialized — and if you've won a seat, you know there's daily activity inside that the rest of the contractor base will never see.


‍ **7. SeaPort-NxG** The Navy's primary services vehicle. Task orders worth tens of millions of dollars regularly post inside the portal with short response windows.


‍ **8. DISA Encore III / SETI** The Defense Information Systems Agency runs multiple IDIQs (Encore III, SETI, ENCORE IV when stood up) where IT and engineering task orders cycle without ever touching SAM.gov.


‍ **9. NASA SEWP V** A governmentwide IT products and services vehicle used heavily across DoD and civilian agencies. Quote requests on SEWP are sent to a subset of contract holders, not posted publicly.


‍ **10. NIH CIO-CS and CIO-SP4** Health and Human Services' major IT acquisition vehicles. Task orders against CIO-CS and CIO-SP4 are routed inside the program offices.


‍


That's ten. The actual count of federal vehicles that work this way runs into the dozens. Add OASIS+, the upcoming Polaris, Alliant 2, the agency-specific BPAs and BOAs, and the picture gets more complete and more unmanageable.


‍


## Why "scraping SAM.gov" isn't enough and never will be


The standard mid-market BD stack is some combination of SAM.gov, an aggregator like GovWin or GovSpend, and a couple of saved Google alerts. That stack catches the opportunities the government is *required* to publicize. It misses the opportunities the government is *encouraged* to route through pre-competed vehicles.


‍


The difference matters because the percentage of federal IT and services dollars flowing through multiple-award contracts has been climbing for over a decade. By the most recent FPDS data, well over half of professional services spend at DoD now moves through IDIQs and BPAs rather than open-market awards. That means if your tooling only looks at SAM.gov, you are systematically missing the majority of your addressable opportunity set.


‍


The Virtualitics BD exec we spoke with put the question this way:


‍


"Are you guys pulling this stuff directly as it gets released on these different solicitations or contracts? Or are you guys using something else where you guys are getting this before it even gets released on these other contracts?"


‍


That's the right question. The answer depends on the vehicle, the data source, and the relationships behind the data. SAM.gov gives you a small, fully public window. Aggregators give you a slightly larger window on top of forecasts and FPDS history. Neither one gives you the inside of the walled gardens.


‍


## How Govly surfaces what's inside the walled gardens


Govly was built specifically to solve this gap. Here is the high-level approach.


‍


- ‍ **Direct portal ingestion.** Govly ingests RFI, RFQ, and sources-sought activity directly from the major vehicle portals (CHESS, ITES, eBuy, SeaPort-NxG, ITAS, NASA SEWP V, and others). Where direct ingestion isn't possible, Govly captures the data through contract-holder partnerships — the vehicle holders themselves contribute the opportunities so the broader contractor and reseller ecosystem can see and team on them.
- ‍ **Normalization across portals.** Each portal has its own taxonomy, its own definition of "active," and its own quirks. Govly normalizes the data so a search for "warehouse management system, Army, $5M–$25M" returns matches across CHESS, ITES, GSA eBuy, and SAM.gov in one place, not five.
- ‍ **Contact enrichment.** Each opportunity is enriched with the contracting officer, technical POC where available, agency office, and prior awards on the same line. So when you find an opportunity that *isn't* on SAM.gov, you also find the human you need to call.
- ‍ **Pre-RFP signal.** A large share of federal opportunities exist as RFIs and sources sought before they become solicitations. Govly surfaces those early signals so a capture team can shape the requirement, not just respond to it.


‍


## Quick-start: which portals matter for your service line


You don't need to monitor every portal. You need to monitor the right ones for what you sell.


‍


**If you sell to the Army:**


- CHESS, ITES-3S/SW2/3H, GSA eBuy (for non-CHESS-eligible commodities), SAM.gov, Army.mil contracting opportunities pages.


‍


**If you sell to the Navy or Marines:**


- SeaPort-NxG, NAVSEA and NAVAIR program office portals, GSA eBuy, SAM.gov, USFFC small business pages.


‍


**If you sell to DoD broadly (cross-service IT or services):**


- ITES, NASA SEWP V, GSA Schedule eBuy, DISA Encore III, OASIS+ when fully stood up, SAM.gov.


‍


**If you sell to the Intelligence Community:**


- IC-specific vehicles you've been read into, agency BPAs, SeaPort-NxG (DIA uses it), classified portals you have access to via a prime, SAM.gov for unclassified components.


‍


**If you sell to civilian agencies:**


- GSA eBuy, NASA SEWP V, NIH CIO-CS and CIO-SP4, GSA STARS III (if 8(a)), agency-specific portals (DHS EAGLE NextGen, VA T4NG follow-on, etc.), SAM.gov.


‍


**If you're a small business or 8(a):**


- GSA STARS III, your SBA-set-aside-eligible Schedules, the small-business windows inside each large IDIQ above, SAM.gov set-aside filters.


‍


Map your service line to the matching portals before you spend another dollar on tooling. Then ask whoever sold you your current BD platform whether they index those portals and if they say "we pull from SAM.gov plus public forecasts," you have your answer about why your pipeline feels thin.


‍


## The bottom line


The federal contracts market is bigger, more active, and more accessible than SAM.gov makes it look. But the doors to the rest of it are behind portal logins, vehicle-holder relationships, and the daily work of capture professionals who know where to look.


‍


If your team has the nagging sense that opportunities are slipping past you, the answer is almost never to search SAM.gov harder. The answer is to start watching the vehicles that actually carry the spend.


‍


That is the thing we built Govly to do. If you want to see which portals matter for your service line and how many opportunities you've been missing,[book a 20-minute walkthrough](https://www.govly.com/) and we'll show you live data on your accounts.


‍
