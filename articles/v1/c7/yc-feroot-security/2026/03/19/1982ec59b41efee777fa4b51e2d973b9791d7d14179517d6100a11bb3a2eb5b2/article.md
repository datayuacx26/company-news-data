---
schema_version: "1.0.0"
document_id: "1982ec59b41efee777fa4b51e2d973b9791d7d14179517d6100a11bb3a2eb5b2"
company_key: "yc-feroot-security"
company: "Feroot Security"
source_id: "yc-feroot-security-news-import-5cc226b40d32"
canonical_url: "https://feroot.com/blog/hipaa-baa-checklist-website-technology-vendors/"
published_at: "2026-03-10T14:14:51+00:00"
first_seen_at: "2026-07-22T07:46:19.024825+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:ac9fb614ddb873915a75449f4059b2a6bf95436f0c8761509aa1b3dfef7da508"
---

# Third-Party BAA Checklist: HIPAA Requirements for Website Technology Vendors

For most of HIPAA’s history, PHI moved through known systems, between known parties, for known reasons. You provisioned access and audited behavior. The data flows remained observable, and so did the vendor relationships built around them.


EHR vendors, billing platforms, and transcription services, you knew what each one touched because you handed it to them.


Then the website became part of the care journey. With it came appointment schedulers, symptom checkers, patient portals, and intake forms. And sitting inside those pages came a layer of third-party scripts, pixels, and tag managers that observe whatever happens on the page, in real time, before your own systems write a single log entry.


OCR is clear on this.[Any vendor that receives PHI requires a BAA](https://www.feroot.com/blog/hipaa-tracking-pixels-without-vendor-baa/) . It’s straightforward for an EHR vendor. A different problem entirely for a tracking pixel that silently reads your appointment booking page.


In this article, we’ll discuss how to discover all the third-party scripts that require BAA, what’s needed in a BAA with them, and what to do when these vendors refuse to sign the BAA.


## When does a website vendor require a BAA?


It depends on context. The same vendor, offering the same technology, may require a BAA on one page and not another. The difference isn’t the vendor. It’s what their code can see.


What is the page about? Does it surface anything sensitive? What input fields are present? What does the script actually read? All of that matters.


### How does context determine BAA obligations?


OCR distinguishes between general unauthenticated pages and authenticated portals. But high-risk unauthenticated flows, such as appointment scheduling, symptom checkers, and intake forms that ask why you’re coming in, fall under scrutiny too.


So do content pages that carry implicit health context. A blog specifically talking about a disease, a condition, or a treatment path isn’t a general health page. It’s a signal about what a visitor is dealing with.


The 2024 revision, as notified by OCR, further added that an IP address tied to a general health information page isn’t automatically PHI. But the moment that same identifier connects to care-seeking behavior, a specific appointment path, a symptom entry, a condition-specific article, the math changes.


Put simply, the same pixel that raises no flags on your general page may require a BAA, or complete removal, on a chemotherapy intake form or virtual visit scheduler.


### What does it mean for tracking technologies?


Under 45 CFR 164.502(e), the BAA threshold is crossed the moment a vendor’s script sends PHI from the user’s browser to the vendor’s servers, even if your own systems never logged it first.


That means that a tracking tag that collects appointment details, symptoms, portal URLs tied to named patients, or device identifiers linked to telehealth sessions has “received” PHI on your behalf.


That puts it in business associate territory, unless the data is properly de-identified.


## Common website technologies and BAA considerations


Most technologies embedded on healthcare websites were built for advertising and product analytics, not regulated health data. And it’s reflected in their business models, data-collection flows, and contracts as well.


So when they encounter HIPAA and PHI, the typical response is to refuse BAAs for browser-based tracking, pushing responsibility to the customers.


Because of that, compliance teams face a challenge. They now need to evaluate each tool twice, once to see if the PHI is reaching the tool, and a second time to see if the BAA can cover the risk.


Let’s zoom in on these technologies


### Google Analytics ( GA4 )


GA4 collects data like page views, events, referrers, device and browser details, IP-derived geolocation, and URL paths and query parameters. Such information helps product and marketing teams understand their audience, run targeted campaigns, and bring more business.


But on healthcare sites, since that also includes appointment IDs, specific references, or even search terms that directly tie to a condition, that comes under PHI.


However, the end responsibility of implementing GA4 with pages that display/collect health context falls on the customer, not Google. And that’s stated in their documentation, that[HIPAA-regulated entities are not to use Google Analytics](https://www.feroot.com/blog/google-analytics-4-hipaa-compliance/) in ways that would create HIPAA obligations, and explicitly state that[Google “does not offer Business Associate Agreements in connection with this service.](https://www.feroot.com/blog/ga4-baa-wont-fix-your-hipaa-problem/) ”


### Adobe Analytics and Experience Cloud


Unlike other ad-tech and audience analytics platforms, Adobe offers some HIPAA-ready services, acting as a business associate under a BAA that spans across Adobe Customer Journey Analytics and several Experience Cloud components.


For analytics, this means some Adobe Experience Cloud configurations, typically enterprise-grade deployments built on Adobe Experience Platform, can operate under a BAA, but standard, marketing-oriented analytics configurations may not be covered unless explicitly designated HIPAA-ready.


### Meta pixel


Meta Pixel is JavaScript code that embeds in your webpages’ HTML code, specifically inside the headers. So it loads with your page and can access what the DOM can, including page URLs, referrer, device and browser information, page content, and even IP address.


On top of it, it can also track button clicks, form submissions, and even appointment identifiers.


Meta CAPI, server-side conversions, extend this model by allowing backend systems to send matched events tied to hashed identifiers, further enriching Meta’s profiling for ad optimization.


The sensitive nature of the tag has been cited in multiple OCR investigations, like Healthline, Aurora Healthcare, and more, inviting clear notifications from OCR about restricting or regulating the use of such tags on pages with healthcontext.


Meta refuses to enter into BAAs with healthcare companies and treats health-related data sent through its tools as prohibited sensitive information, placing the compliance burden on customers to avoid transmitting PHI.


### Ad tags from Google and LinkedIn


Advertising platforms like Google Ads and LinkedIn Ads rely on tags and pixels that function similarly to Meta’s, combining user identifiers like cookies, device IDs, and authentication-based metadata with page URLs, referrers, and custom parameters to measure conversions and build audiences.


HHS’s tracking bulletin implicitly brings these technologies into scope whenever they receive individually identifiable health information linked to care-seeking behavior on landing pages.


### Chat widgets


Chat widgets operate differently from pixels. A pixel reads the page. A chat widget waits for the person to speak.


And when somebody speaks into that window, on a healthcare site, they often mention what they’re feeling, what they’ve been diagnosed with, what medication they’re on, and why they’re looking for a specific type of care.


That content, combined with the email address or phone number they enter to start the conversation, is about as individually identifiable as health information gets.


The chat transcripts capture intent, context, and in many cases, clinical detail that a structured form would never surface. Which is what makes chat different, and in some cases, even a more urgent compliance issue to solve than most tracking technologies. The data isn’t inferred from a URL or an event parameter. It’s direct, written by the patient themselves.


## Third-party BAA evaluation checklist


Not every website technology in your stack requires a BAA. The question is knowing which ones do, and what to do once you’ve made that determination.


The checklist below walks through that evaluation in sequence, from page context down to vendor posture. Work through it and document your reasoning at each step.


**Evaluation Question** **If Yes** **If No**


Does this technology operate on pages containing PHI or health-related content, including condition-specific content, scheduling flows, or authenticated portals? Proceed to the next question. BAA is likely not required. Document the determination and the pages evaluated.


Can this technology access, capture, or transmit data from these pages, including URLs, query parameters, form inputs, or device identifiers? Proceed to the next question. BAA is likely not required. Document what access was assessed and how it was ruled out.


Does the accessible data include both health information and individual identifiers sufficient to constitute PHI under HHS guidance? BAA required. Proceed to the next question. BAA is likely not required. Document the de-identification or exclusion basis.


Does the vendor offer a BAA for this specific product and implementation? Execute BAA and document the scope and covered services. Proceed to the next question.


Can the technology be configured to reliably exclude PHI access through parameter stripping, field masking, or conditional loading? Implement controls, verify effectiveness, and document. Proceed to the next question.


Can the technology be removed from PHI-accessible pages entirely? Remove, document the decision, and validate through testing. Consult legal counsel. Continuing use without a BAA or verified controls is a direct Privacy Rule exposure.


## What to do when vendors refuse BAAs


A vendor’s reluctance to sign the BAA doesn’t become your defense. In fact, the opposite happens. OCR has clarified on multiple occasions that regulated entities remain responsible for impermissible disclosures even when vendors decline business associate status.


So when that happens, the question that comes next is how to design your setup in a manner that PHI never reaches the vendors that you won’t hold a BAA with. And if you can’t ensure it, do you need to discard the vendor entirely?


### Technical scoping and filtering


You limit the technology strictly to non-PHI contexts, top-of-funnel marketing pages only. Then you implement controls like URL parameter stripping, field blocking, or server-side proxies that prevent health-related data from ever reaching the vendor.


### Compensating controls with reduced data


You configure the tool to minimize what it sees. Remove user IDs, mask IPs, and disable granular event capture. Then you document a formal determination that what remains no longer meets the PHI threshold under HIPAA and HHS guidance.


### Removal or replacement


You take the vendor out of PHI-bearing journeys entirely. Either replace it with a HIPAA-ready alternative operating under a BAA, or accept reduced measurement in exchange for regulatory certainty.


None of these are one-time fixes. Scoping requires ongoing verification as new pages are added. Replacement may mean migrating to more complex stacks. The work doesn’t end at the decision.


## How DXComply supports BAA compliance


Your BAA inventory documents the relationships you know about, but it doesn’t see the scripts your tag manager loaded last week, the pixel your agency added to the scheduling flow, or the session recorder quietly capturing typed symptoms on your intake form.


By the time your periodic reviews reveal those gaps, the damage gets done.


DXComply solves that. It continuously monitors your browser-side surface to reveal all the third-party scripts and tags across the pages on the website. Then, it helps you map each of those to the PHI context, and continuously monitors those scripts to flag any anomalies, automatically detect changes, and block PHI from exiting the browser in real-time when BAAs don’t exist.


### Complete tracking stack discovery


DXComply scans all pages and domains, surfacing every script, tag, and third-party call across your sites and patient portals, including those loaded dynamically through tag managers, embedded iframes, or nested widgets. If it runs in the browser, it’s visible.


### PHI access analysis at the data flow level


The platform identifies where PHI appears in page content and forms, observes which scripts can read those elements, and maps the resulting data flows to specific vendor domains. That means BAA decisions are made based on what technologies actually capture in patient journeys, not on vendor category or intended use.


### Continuous BAA status monitoring


DXComply tracks which vendors have executed BAAs and flags mismatches where PHI reaches vendors without them. When new technologies appear on PHI-bearing pages, compliance teams are alerted before exposure compounds. And when OCR comes asking, the audit trail is already there, with time-stamped evidence of what ran, what it accessed, and where it sent data.


## The bottom line


BAA compliance for website technologies is a visibility problem you manage continuously, because the scripts on your pages change faster than any static inventory can track.


The starting point is knowing what’s actually running on your site and what it can see.


Evaluate your current technology stack against BAA requirements, and[explore what DXComply](https://www.feroot.com/demo-healthdata-shield-ai/)[surfaces](https://www.feroot.com/demo-healthdata-shield-ai/) when it looks at your pages the way OCR would.
