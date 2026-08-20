---
schema_version: "1.0.0"
document_id: "9f31cdef0a348b85250266beabbe0c6466da37102a26b261fab81f95e93e8d14"
company_key: "yc-kodex"
company: "Kodex"
source_id: "yc-kodex-news-import-34a2493f70b7"
canonical_url: "https://www.kodexglobal.com/blog/eu-e-evidence-live-today-what-actually-changed"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T19:39:28.283799+00:00"
fetched_at: "2026-08-18T19:39:30.272185+00:00"
content_hash: "sha256:ae80d69b51ad8ad06e130921d0cf52354a8404dfe8e948c7febb583adc4e4bb3"
---

# EU e-Evidence is live today. Here's what actually changed.

As of this morning, competent judicial authorities in participating EU member states can issue European Production Orders directly to service providers. You have ten days to answer, or eight hours in an emergency. Non-compliance can carry penalties of up to 2% of worldwide annual turnover.


On our August 6 practitioner session we opened with a hypothetical: an emergency order arriving at 2am on a Sunday. From today, that can actually happen.


Most of the coverage of this deadline is about whether Europe is ready. The legal framework is. The technical infrastructure isn't, at least not everywhere. I'll go through the details below. But those readiness problems will resolve themselves over the next few quarters, and the regulation will still be here.


I think the more useful thing to explain is what kind of change this is, because it's a bigger one than the compliance date suggests.


## The infrastructure is still catching up


The regulation applies starting today, even though the decentralised IT system built on e-CODEX, JUDEX (the Justice Digital Exchange), isn't operational everywhere. The regulation anticipated this and provides for alternative means when the system can't be used.


Germany's federal police wrote to service providers in June and described the practical reality directly.


> "It is highly likely that JUDEX is not available in every country on August 18th, 2026."
> Bundeskriminalamt, letter to service providers, June 29, 2026


There's no published list of which member states are starting inside the system and which aren't. We've asked for one in the working sessions we're part of, and no member state has committed to producing one. Technical implementation also varies significantly by country.


Part of the reason, as Ike Anude on our team put it on the session, is that "everyone is kind of waiting for everyone else to use the API before they use the API."


If you registered to receive orders through an API, that doesn't mean authorities will be able to reach you that way on day one. But orders can still be issued starting today.


## This has happened before


GDPR took effect in 2018 while regulators were still hiring. The DSA and the OSA both went live before their enforcement machinery did. James Clark, who works with our EU customers, described the pattern on the same session: "the implementation of the regulation deadline slips, but the expectation on the service providers to be able to comply with it often doesn't."


The regulation anticipated that the technical infrastructure might not be available everywhere on day one. Article 19(5) provides for alternative means of transmission when communication through the decentralised system isn't possible. Those alternatives must be swift, secure and reliable, and allow the recipient to establish authenticity. Kodex can serve as one of those alternative channels, providing a secure and auditable path for orders when the decentralised system isn't available.


Here's what that means in practice: the regulation became applicable this morning, and orders can reach service providers through alternative channels while the common technical infrastructure comes online.


The German letter I quoted above goes on to ask each provider which alternative channel it prefers. That's not a workaround. It's a scenario the regulation expressly anticipated.


## The actual change


Until yesterday, cross-border access to electronic evidence often depended on slower mechanisms such as European Investigation Orders or mutual legal assistance, or on voluntary cooperation with service providers.


From today there's a new formal instrument with a standard format, a deadline, a penalty, and eventually an API behind it.


Twenty-six EU countries just adopted a common framework for cross-border production and preservation orders for electronic evidence.


Other governments are moving in a similar direction: the CLOUD Act agreements, the UK's COPO regime, Australia and India. What I see across jurisdictions is the same direction of travel: more standardized, direct and increasingly machine-readable processes.


Volumes were already growing before the regulation existed. Across our customers, EU request volume has grown 15% to 30% a year for the past two years, measured over hundreds of thousands of requests.


The exact impact of e-Evidence is impossible to predict, but nobody I've spoken to is forecasting a decrease. The mechanism is straightforward: investigators now have a direct legal instrument with a defined response deadline.


## The gap that's left


Most of the preparation work I've seen over the past six months went into scope analysis, appointing representatives, filing notifications, and updating policies. That work was necessary, and most companies we work with have done it.


The gap is operational: what happens in the first twenty minutes after an order arrives.


Who gets notified? Can they tell it's an emergency? Can they see if you hold responsive data? Do they know if enforcing-authority approval is needed?


And then there's automation. **Preservation workflows and user lookups are two of the clearest places to start.** For companies processing meaningful volume, these are repeatable workflows that shouldn't require someone to manually work through the same steps every time. The goal should be to automate them end to end where the requirements are clear and the appropriate controls are in place.


Eight hours isn't enough time to figure any of this out from scratch. You can't assemble an intake process at two in the morning while the clock is running.


We've spent this year on that gap.


We run the 2am scenario with customers as a drill: inject an emergency order, walk the intake end to end, see what breaks. Since no member state system was open for testing, we built a simulated one and test integrations against it.


If you want the mechanics, scope, the clocks, the channels, and what an EPOC looks like when it arrives,[our e-Evidence page](https://www.kodexglobal.com/eu-e-evidence) covers them in detail.


## What happens now


Nothing dramatic today.


The first orders will trickle in, some will be defective, and many may arrive through alternative channels while the decentralised infrastructure comes online.


If you do three things this week: decide which alternative channel you'd nominate if an authority asked, run one emergency drill before a real one arrives, and identify the repeatable work, starting with preservation workflows and user lookups, that you can automate.


[The readiness guide](https://www.kodexglobal.com/eu-e-evidence-ebook) covers the rest in about ten minutes.


We'll write up what the first real orders look like once they've moved.
