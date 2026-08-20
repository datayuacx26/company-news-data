---
schema_version: "1.0.0"
document_id: "ed1ae3997dfc0ed3a50f116e7ceb834b0e7497929cbe55e460bdc4e199b4e979"
company_key: "yc-popl"
company: "Popl"
source_id: "yc-popl-atom-ab76058d9d9d"
canonical_url: "https://popl.co/blogs/all/decoding-the-tech-how-do-qr-codes-work-and-why-they-matter"
published_at: "2026-06-08T20:43:54+00:00"
first_seen_at: "2026-07-24T12:58:35.597391+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:5701d6376ea58afc8ec9633a87b394d6e3d7859e59127bdd0631d0cd4bf621ab"
---

# How QR codes work: inside the code your reps scan at events

Your rep is mid-conversation with a perfect-fit buyer. They reach for a phone, frame the badge, and the lead pops up before the buyer finishes their sentence. A name. A company. A job title. That's it. The verified email, direct phone, and firmographics (everything the follow-up on the flight home depends on) never come back.


The code worked. The scan took under a second. The built-in error correction did its quiet job in the background. And the lead is already mostly useless before your rep shakes the next hand.


That's the catch with QR codes at events. It’s one reason why roughly


[80% of trade show leads](https://popl.co/blogs/all/event-lead-capture-problems-why-most-tools-fail-and-what-to-do-instead) never get a follow-up. \[1\] And the problem isn’t going away because 43% of marketers use QR codes specifically for events. \[2\]


So this is every booth, every day. The tech is reliable. The data it returns is thin. And the gap between those two things quietly eats event pipeline. See how to close it below, so your reps can start working leads before they leave the show.


## Quick Response: from assembly line to trade show floor


QR stands for Quick Response. The name hints at the design goal: the data lives in the pattern itself, so a camera can read it the moment it sees the code. No lookup required.


Masahiro Hara invented the format in 1994 at Denso Wave, a subsidiary of Denso Corporation (part of the Toyota Group), to track auto parts faster than the 1D barcodes lining the assembly line. \[3\] It was later standardized internationally as ISO/IEC 18004. \[4\]


That auto-parts origin matters more than it sounds. The original design brief was fast, reliable scanning under tough conditions: limited time, imperfect lighting, partial visibility. The same conditions describe a packed booth. Fluorescent overhead lights. Badges creased inside blazers. A buyer whose attention is already drifting. The engineering still holds up thirty years later.


QR codes weren't the first scannable thing on a badge, though. The


[linear barcode](https://popl.co/blogs/all/how-badge-scanning-technology-works-ocr-qr-and-nfc-explained) arrived first and never left: it encodes a simple ID that links to your registration database, scans quickly, and costs almost nothing to print.


But QR codes won when badges needed to carry more than a number: session access, tiered credentials, contact details written right into the code. \[5\] Both still show up on event badges in 2026.


## The anatomy of an event badge QR code


That little square is a system with parts. Walking through them explains why a badge still scans cleanly on a lanyard that's been creased inside someone's blazer all morning.


Three big squares sit in three corners. Those are the


finder patterns


. They tell the scanner where the code is and which way it's facing. There are three of them, not four. The asymmetry is the point: three corners marked and one left blank means there's only one way to read the code. With four, the scanner couldn't tell which corner was up. That's the geometry that lets a


[scanner read a badge](https://popl.co/pages/badge-scanner) dangling sideways on a lanyard or a sign tilted on an exhibit table. \[6\]


The other parts do less of the heavy lifting but matter just as much. A small


alignment pattern


sits inside the data field of larger codes, keeping the read accurate when the camera is angled. Dotted


timing patterns


run between the finder patterns to anchor the grid. The white margin around the whole thing is the


quiet zone


, the empty space that lets a scanner find the edges of the code.


The rest is data and error correction. Those tiny squares have a name:


modules


. The smallest QR code is 21 modules wide, and the largest is 177.


### How the QR code's design survives a trade show booth


Real-world scanning is brutal. Fluorescent lighting washes out contrast. Badges get creased inside blazer pockets. Cameras shoot from awkward angles while badges sway sideways on lanyards. And your rep has maybe four seconds before the prospect drifts off.


Every part of the QR code's anatomy does its job to[keep the scan working](https://popl.co/blogs/all/how-to-scan-qr-codes) . The engineering buys those four seconds.


## What happens in the under-300 milliseconds your scanner reads a badge


A booth scan feels instantaneous because it almost is. Decoding takes under 300 milliseconds on a modern smartphone. \[7\] Four distinct things just happened in that quarter of a second.


### 1. Find the code and straighten the image


The camera finds the three finder patterns in the corners. Those tell it where the code sits and how it's angled. Then the scanner straightens the image, so a tilted code reads as cleanly as a straight one.


### 2. Strip off the mask


When the code was generated, the pattern was scrambled with one of eight possible masks. A mask is like a filter that prevents long stretches of solid black or white, which would otherwise confuse the scanner. Which mask was used is recorded next to each finder pattern, so the scanner can undo it.


### 3. Read the squares


Now the scanner reads the actual squares. They aren't laid out in straight rows. They wind through the grid in a set pattern that the scanner has to follow. As the scanner reads, it sorts them into two groups: the actual data and extra data used for the next step.


### 4. Fix anything the camera misread


This is where Reed-Solomon error correction comes in. When the QR code was generated, extra "spare" data was mixed in alongside the actual data. If part of the code is damaged or misread, the scanner uses the spares to fill in what's missing. It's the same trick CDs and DVDs use to keep playing despite scratches.


There are four levels of error correction, called L, M, Q, and H. From lowest to highest, they can recover about 7%, 15%, 25%, and 30% of a damaged code. \[6\] That's why a scuffed corner or a sticker fragment doesn't kill the scan. The spare data covers the damage.


All of this happens before the lead pops up on your rep's screen. What's in the lead record depends on what the organizer chose to encode. How much could fit depends on which of the four encoding modes was used: numeric, alphanumeric, byte/binary, or kanji.


****


## What’s inside the QR code on your event badge?


Pick up a


[conference badge](https://popl.co/blogs/all/why-conference-badge-qr-codes-are-a-scam-and-how-to-break-free?) , and the QR on it is usually about an inch across, with squares small enough that a fingerprint covers several. That sizing isn't arbitrary, and it explains a lot about the lead it returns.


### In theory: a QR code can hold thousands of characters


QR codes come in 40 standard sizes. The smallest is a 21-by-21 grid (441 squares total), big enough that each square is easy to see. At its biggest, one QR code can hold over 7,000 digits, or roughly 4,300 letters and digits. \[8\] That's enough to fit dozens of full-contact records.


In practice, almost nobody encodes that much on a badge.


### In practice: most of the lead data lives in a database


Two things shrink what badges actually hold. The first is the error correction we just covered: badges run at higher levels by default because they have to withstand lanyard creases, fluorescent lighting, and being tucked under a sleeve. Higher error correction means more spare data and less room for real content: easily a third less capacity, sometimes more.


The bigger reason is that most event platforms don't put your contact details into the code at all. The QR holds a unique attendee ID. Think of it as a serial number that points to a record in


[t](https://popl.co/blogs/all/what-is-a-badge-kit-api-a-simple-answer-for-non-technical-marketers) he organizer's system. Everything else (name, company, title, email, session access) lives in that database. When your rep scans the badge, the


[rental scanner](https://popl.co/blogs/all/popl-vs-event-badge-scanners-an-in-depth-comparison?) returns whatever fields the organizer chose to expose, which usually isn't much. \[9\]


One field marketer described what they actually need from a badge scan: "My direct phone number is the most important piece of data a platform needs to capture." That's exactly the field a bare badge scan won't give you, because it's almost never the field the organizer chose to encode or expose.


So the code is small on purpose, and the lead it returns is small for the same reason. The thin record isn't an oversight. It's the design.


****


## Static vs dynamic QR codes: which one is on the badge vs the booth?


This distinction is the easiest to get wrong, and the cleanest to fix once you see it.


### How static vs dynamic QR codes are different


A static QR code has the data baked in. Whatever the code points to is locked into the pattern itself, so you can't change where it leads after it's printed. You also can't track scans because the scan never has to check in with a server.


A dynamic QR code works differently. The pattern encodes a short URL (one you control), which then redirects to wherever you want it to go. Because every scan passes through your server first, you can change where the code points to without reprinting it, see who scanned it and when, and tie scans back to a specific campaign. \[6\]


### Where each QR code belongs at an event


Attendee badges almost always use static codes. The badge has to work even when conference Wi-Fi doesn't. It can't be edited after it's printed. And it only needs to point to one record. Static wins. \[9\]


Booth banners, demo signs, and printed collateral should generally be dynamic. You want to edit the destination mid-event if a session changes rooms. You want day-by-day scan data for


[ROI reporting](https://popl.co/blogs/all/top-trade-show-metrics-to-measure-for-success-in-2026) . You want to A/B test which signage actually drives booth traffic and which is just expensive wallpaper.


One operational note for dynamic codes: use a branded short URL, such as qr.yourbrand.com. It makes the code recognizable to prospects and harder for someone to spoof with a fake sticker. \[10\]


Popl's free


[QR code generator](https://popl.co/pages/qr-codes) builds dynamic codes you can edit and track.


****


## Quishing at events: what your reps need to know


Quishing is QR code phishing. It occurs when a QR code points to a malicious website. The problem is that most sales reps don’t discover the risk until it’s too late: after they’ve tapped the link.


### Why event teams are a target


QR phishing is a real and growing problem. Phones tend to have weaker phishing protection than corporate PCs, making QR codes a favored attack vector. HP Wolf Security has tracked near-daily campaigns since October 2022 \[11\], and the FTC has issued a public alert. \[12\]


The event-specific version is already a threat. In January 2026, the FBI's Internet Crime Complaint Center issued a flash alert about a 2025 North Korean Kimsuky campaign that used fake conference invitations as the lure: QR codes in the invites routed targets to spoofed Google login pages. \[13\] A fake conference invite is the exact email an event marketer might open without thinking twice.


The physical attack pattern exists, too. New York City's DOT warned drivers in June 2025 about fraudulent QR stickers placed over legitimate parking meter codes. \[14\] Security firms list event signage in the same overlay-target category, though high-profile attacks on physical conference badges aren't widely documented yet.


### Precautions your reps can take at events


If your team is scanning QR codes at a show, confirm the URL preview before tapping, use scanners and apps you trust, and verify that unfamiliar booth-floor signage represents real brands.


If your team is deploying codes: use dynamic codes so a compromised destination can be redirected, use a branded short domain so codes are easier to recognize and harder to imitate, and set expiration dates on time-limited event codes so they can't be repurposed after the show closes. \[10\]


## Why a perfect scan can still hand you a dead lead


This is where the QR code capacity story from the last section pays off.


### Why badge scans return incomplete lead data


Event organizers encode badges in one of two ways. The contact details are embedded directly in the code, so the scan returns them instantly, even without Wi-Fi. Or the code can hold a unique ID that the system looks up in the organizer's database. This requires connectivity.


Either way, you get only what the organizer decided to share. Usually, that's a name and a company delivered in a CSV weeks after the show.


### What turns the scan into a workable lead


Turning a scanned badge into a workable lead takes four steps:


-


Enrich:


Add a


[verified email, phone number, and company firmographic](https://popl.co/pages/our-data)[s](https://popl.co/pages/our-data)


to the thin record from the badge.


-


Qualify:


Score the lead against your ICP to know if they’re worth pursuing.


-


Sync:


Push the enriched lead into


[your CRM](https://popl.co/pages/integrations) with the right campaign tags.


-


Route:


Send the lead to the right rep for rapid follow-up.


Doing all four in real time, while your rep is still on the floor, is the trick. As one field marketer put it: "I want on-the-spot enrichment so my team can review the data at dinner and craft the whole narrative for tomorrow's meetings." That's the bar.


B2B contact data decays roughly 22.5% per year \[15\], so even records that come back complete won't stay complete for long.


[Enrichment](https://popl.co/blogs/all/how-popls-list-enrichment-engine-achieves-95-match-rates) isn't optional. It's the compounding interest you owe every quarter.


### The scan isn't the gap. The data organizers share is


****


A QR code is a small marvel of engineering. But the scan only ever returns what the organizer chose to share: usually just a name and a company.


The edge goes to the first event team to close the gap.


That’s what Popl was built to do: every badge scan becomes an enriched CRM record in seconds.


[Request a demo](https://popl.co/pages/request-a-demo) to see the complete workflow in action.


****


## Frequently asked questions


### Who invented QR codes, and when?


QR codes were invented in 1994 by Masahiro Hara at Denso Wave, a subsidiary of Denso Corporation (part of the Toyota Group), to track parts on the assembly line faster than barcodes allowed. \[3\] They were later standardized internationally as ISO/IEC 18004, which has been revised several times to keep up with new encoding modes and use cases. \[4\]


### What does QR stand for?


Quick Response. The name reflects the design goal: return the encoded data the instant a camera reads the code, with no database lookup required to recover what's stored in the grid. The original use case (scanning auto parts as they pass a workstation) required sub-second reads.


### How do QR codes work?


A QR code stores data in a 2D grid of black-and-white modules. A scanner locates the three finder patterns in the corners, corrects the perspective, applies a mask, reads the modules in a set order, and uses Reed-Solomon error correction to recover the data even when part of the code is damaged or partially obscured.


### How much data can a QR code hold?


At the largest version (40) and lowest error correction, up to 7,089 numeric characters, 4,296 alphanumeric characters, 2,953 bytes of binary data, or 1,817 kanji characters. \[8\] Higher error correction lowers that ceiling, and most real-world codes (including event badges) store a small fraction of the maximum.


### Can a damaged QR code still be scanned?


Yes. Error-correction levels L, M, Q, and H rebuild missing data, up to roughly 30% of the code at the highest level. \[6\] That's why a scuffed corner, a sticker fragment, or a lanyard-sleeve crease usually doesn't stop a scan. Beyond about a 30% loss, the code becomes unrecoverable.


### What's the difference between static and dynamic QR codes?


Static code stores fixed data you can't change or track. A dynamic code points to a redirect URL you control, so you can edit the destination later and measure scans by time, place, and device. Event badges nearly always use static codes; booth signage usually benefits from dynamic codes.


### QR code vs barcode: what's the difference?


A barcode stores data in one dimension (width only) and typically holds an ID linked to a database. A QR code stores data in two dimensions, holds far more data than a one-dimensional code, and includes error correction so it can withstand physical damage. Both still appear on event badges; QR won where richer encoding mattered. \[5\]


### Are QR codes safe to scan?


The code itself is just data; risk comes from where it points. Preview the URL before tapping, prefer HTTPS and trusted scanners, and for codes you deploy, prefer dynamic codes you control so a compromised destination can be redirected. \[10\] Both the FTC and the FBI have issued recent alerts about QR phishing campaigns targeting consumers and businesses. \[12\] \[13\]


## Sources


1.


CEIR (Center for Exhibition Industry Research). (2018). "Trade Show Lead Management Statistics."


Exhibit Associates


. https://exhibitassociates.exhibit-design-search.com/trade-show-tips/trade-show-lead-management-69/


2.


Bitly. (2025). "From Scans to Strategy: The 2025 State of QR Codes Report."


Bitly


. https://bitly.com/pages/qr-code-survey


3.


Denso Wave. (n.d.). "History of QR Code and Standards."


QRcode.com


. https://www.qrcode.com/en/about/standards.html


4.


International Organization for Standardization. (2024). "ISO/IEC 18004:2024 Information technology: QR Code bar code symbology specification."


ISO


. https://www.iso.org/standard/83389.html


5.


Choose2Rent. (2026). "Which Event Badge Scanner is the Best."


Choose2Rent


. https://choose2rent.com/blog/which-event-badge-scanner-is-the-best/


6.


Wikipedia contributors. (2026). "QR code."


Wikipedia


. https://en.wikipedia.org/wiki/QR_code


7.


Supercode. (2026). "How do QR codes work?"


Supercode


. https://www.supercode.com/blog/how-do-qr-codes-work


8.


Denso Wave. (n.d.). "Information capacity and versions of the QR Code."


QRcode.com


. https://www.qrcode.com/en/about/version.html


9.


Wikipedia contributors. (2026). "Lead retrieval."


Wikipedia


. https://en.wikipedia.org/wiki/Lead_retrieval


10.


Supercode. (2026). "Real talk: just how safe and secure are QR codes?"


Supercode


. https://www.supercode.com/blog/real-talk-just-how-safe-and-secure-are-qr-codes


11.


HP Wolf Security. (2023). "Threat Insights Report Q4 2022."


HP Threat Research


. https://threatresearch.ext.hp.com/hp-wolf-security-threat-insights-report-q4-2022/


12.


Federal Trade Commission. (2023). "Scammers hide harmful links in QR codes to steal your information."


FTC Consumer Advice


. https://consumer.ftc.gov/consumer-alerts/2023/12/scammers-hide-harmful-links-qr-codes-steal-your-information


13.


Federal Bureau of Investigation, Internet Crime Complaint Center. (2026). "Kimsuky quishing campaign using fake conference invitations."


IC3 flash alert, January 2026


. https://www.ic3.gov/CSA/2026/260108.pdf


14.


New York City Department of Transportation. (2025). "NYC DOT Issues Parking Meter Scam Advisory."


NYC.gov press release, June 6, 2025


. https://www.nyc.gov/html/dot/html/pr2025/nyc-dot-issues-parking-meter-scam-advisory.shtml


15.


HubSpot. (n.d.). "Database Decay Simulation." HubSpot. https://www.hubspot.com/database-decay
