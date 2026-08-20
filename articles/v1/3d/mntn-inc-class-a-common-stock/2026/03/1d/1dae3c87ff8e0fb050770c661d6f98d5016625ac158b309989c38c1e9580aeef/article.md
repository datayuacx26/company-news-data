---
schema_version: "1.0.0"
document_id: "1dae3c87ff8e0fb050770c661d6f98d5016625ac158b309989c38c1e9580aeef"
company_key: "mntn-inc-class-a-common-stock"
company: "MNTN Inc."
source_id: "mntn-inc-class-a-common-stock-rss-3c333be474a8"
canonical_url: "https://mountain.com/tech/how-mntn-measures-performance-without-clicks-a-deep-dive-into-the-verified-visits-model/"
published_at: "2026-03-23T16:27:48+00:00"
first_seen_at: "2026-07-25T01:12:17.961662+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:fc1879aae6640a3cc4e81b69b0e5d7358f10ea182e13119a8066bdd6d5de9b3f"
---

# How MNTN Measures Performance Without Clicks: A Deep Dive Into the Verified Visits Model

# How MNTN Measures Performance Without Clicks: A Deep Dive Into the Verified Visits Model


Mar. 2026


Connected TV has redefined performance marketing. The living room screen drives measurable business outcomes, yet its interface doesn’t allow users to click through to a landing page. Without a click trail, many third‑party analytics platforms struggle to connect an ad impression on the largest screen in the house to the surge of site visits and conversions that often follow. MNTN solved this TV attribution blind spot with Verified Visits™, a patented methodology that links impression, viewer, household, and outcome with scientific precision — so advertisers can measure CTV’s true performance alongside their other marketing efforts. This article expands on the original technical draft, weaving in additional research about how the model works and why it has become the industry benchmark for accountable CTV.


### Why traditional click‑based analytics undervalue CTV


Last‑touch web analytics tools like Google Analytics assign credit to the final referring channel in a user’s journey to an advertiser’s site. Since a television commercial can’t be clicked, it rarely shows up as the “source” of a session — even when it’s the reason a viewer visits an advertiser’s website directly.\[1\] Verified Visits was designed to restore that missing line of sight. It observes the moment a streaming ad is served, follows the consumer across household devices, validates that no other paid channel caused the visit, then credits the campaign if the pathway is clear. The model deduplicates competing traffic sources in real time and gives CTV credit only when it deserves it.


### 1. The Impression to Verified Visits Workflow:


1. **Viewer Engagement:** A consumer begins watching TV on their device, such as a Smart TV or OTT device.
2. **Ad Request via Real-Time Bidding:** In response to the content view, Real-Time Bidding (RTB) requests are sent to MNTN’s Ad-Service for a CTV creative. The Ad-Service returns a Video Ad Serving Template (VAST) file that includes both the ad, itself, and associated identity information.
3. **Ad Serving and Impression Trigger:** After a RTB bid is won, the CTV video player executes the VAST file at the time of the impression. This triggers the CTV ad to play.
4. **Impression Event Sent to MNTN:** Upon execution of the VAST file and commencement of the ad playback, a VAST impression event containing the ad and identity information is sent to the MNTN Event Service.
5. **Event Processing by Verified Visits Service:** The Event Service forwards the contents of the VAST Impression event to the Verified Visit Service (VVS) for processing.
6. **Event Storage in Scylla:** The Verified Visit Service records the impression event to a data store called Scylla. Each event is stored with a Time To Live (TTL) value that aligns with the advertisers’ configured attribution window. This ensures impression data is retained only for the duration necessary to support attribution logic.
7. **Real-Time Analytics Notification:** If a user visits the advertiser’s website after the impression, the Verified Visit Service first applies the attribution filtering logic (outlined below), then notifies third-party analytics platforms (e.g. Google Analytics) of the visit in real time.
8. **Attribution Filtering Logic for Deduplication:** Before sending the notification, MNTN applies filtering logic to ensure accurate CTV attribution: the visit is only reported if **no other marketing event** from any other source — such as paid search, paid social, marketing emails, or other advertiser-driven campaigns — occurred between the impression and the visit. This ensures compliance with attribution rules defined by Google Analytics and similar analytics providers and also prevents MNTN from being credited for last-click visits driven by other channels.


*Fig A: Impression origination diagram (specifically for a CTV ad). Triggering event: user watches TV*


### 2. The Tracking Pixel and The Advertiser’s Website:


The Tracking Pixel is a snippet of JavaScript code that exists on every page of the advertiser’s website. Using its Smarter Pixel technology, it collects page-level attributes and identities information in real time.


The information gathered by the pixel is distributed to multiple services for processing these signals:


- **Identity Sync Service stores** a history of identity attributes in Scylla for identity resolution
- **Verified Visits Service (VVS)** evaluates whether the page view marks the start of a new session, which is necessary for visit verification


*Fig B: Tracking Pixel Data Collection Diagram – Triggering event: A user in a household that was served an impression enters the advertisers website*


### 3. The Verification Decision Tree Inside VVS


When a page view reaches the VVS, the system decides whether it is the **start of a session** and whether that session should be marked as a Verified Visit. This evaluation happens in three stages.


1. **Eligibility Screening:** VVS checks the identity attributes associated with the page view against advertiser-created **allow‑lists** and **block‑lists** . Traffic that is obfuscated by Apple Private Relay, VPNs, or other privacy tools is disqualified at this stage.
2. **Impression Matching:** For each remaining identity attribute, VVS looks back in **ScyllaDB** for a corresponding impression within the configurable look‑back window. If no matching impression is found, the visit is considered **organic** or driven by another channel. The device is then marked **ineligible** for future Verified Visits until a new MNTN impression is served.
3. **Competing‑Source Audit.** If a valid impression is found, VVS performs a final audit by parsing the page URL for UTM parameters (e.g. gclid, utm_source, etc.) to detect other potential sources of traffic. If the visit arrived through a **competing channel** , such as a paid search click, VVS labels it a “competing first‑touch” and withholds credit. This prevents MNTN from claiming attribution for traffic driven by other media.


**Interaction Between MNTN Attribution and Other Competing Marketing Events**


MNTN’s **Verified Visit Attribution Service** is designed to provide third-party analytics platforms information about ad impressions and resulting user behavior. These platforms typically fall into two categories:


- **Mixed Media Modeling (MMM) Platforms** , which require a feed of impression data. MNTN streams this data to MMM platforms in real-time as impressions as served.
- **Multi-touch Attribution (MTA) Platforms** , which started as web analytics tools, require visit-level data rather than impression data.


**MNTN’s Conservative Attribution Approach**


When integrating with multi-touch attribution platforms, MNTN takes a conservative approach to attribution, often under-reporting visits to avoid conflicts with signals from other marketing channels. A classic example of that is the interaction between CTV and Paid Search. The following example occurs, on average, to 25% of visits.


1. A consumer sees a brand’s TV commercial streamed by MNTN.
2. The consumer opens a browser on their phone, tablet or computer and types the name of the brand in the commercial without appending the .com suffix.
3. Because the full domain isn’t typed, the browser returns search engine results instead of going directly to the brand’s site.
4. The consumer clicks the first link — often a branded search ad— which takes the user to the brand’s site.


Although the CTV ad drove the user’s action, branded search gets credit for the visit. MNTN could inform MTA platforms of this visit, but choose not to, in order to avoid conflicting with the explicit action the user took. The logic for this is simple: any visit to a brand’s site that includes a UTM parameter is ignored by MNTN’s Verified Visit Service when determining last-touch attribution and when sharing data with MTA platforms.


The attribution model distinguishes **same‑device** matches (where the viewer engages with an ad and visits the site on the same screen) from **cross‑device** matches (the more common pattern in which a household sees an ad on one device, such as connected TV, but visits the site on another, like a phone or laptop). Cross‑device accuracy relies on MNTN’s proprietary identity graph, which connects deterministic and probabilistic signals across 99% of US households.


*Fig C: High level overview of cross device attribution*


### 4. First‑Touch vs. Last‑Touch: How MNTN Speaks the Language of GA4


Marketers rely on Google Analytics 4 (GA4) and other Multi-Touch Attribution platforms for a unified view of digital performance. To support this, MNTN mirrors its visit data into advertisers’ GA4 properties using the Measurement Protocol API. However, only **non‑competing, last‑touch** **Verified Visits** are forwarded.


Once a visit is deemed eligible, VVS publishes it to a Kafka topic. From there, the Attribution‑Consumer micro‑service formats each payload with key identifiers — ga_client_id, session_id, and measurement_id — along with a rich set of meta‑fields including source, medium, campaign, referrer, user IP, timestamps, and flags. The structured payload is then posted to GA4. Because GA4’s attribution model remains click‑centric, a **25% delta** between MNTN Verified Visits and GA sessions is expected and healthy.


*Fig D : Flow of events in GA4 integration*


### The Premise of First-Touch vs. Last-Touch Reporting


As established, in analytics platforms like Google Analytics, the source that is usually credited with a specific site visit (or session, in GA terms) is the one that brought the user **directly** to the site. For example, if a user enters the site by clicking on a sponsored Google ad, that visit would be attributed to Google. This is considered a last-touch attributed visit.


In the case of CTV ads, however, there is no way for a user to click directly from the ad to the advertiser’s website. A typical user journey is shown in the flow below:


In this example, the role of the MNTN CTV ad in influencing the user’s decision to visit the site is not acknowledged by the third-party analytics platform. Due to the non-clickable nature of CTV ads and the inherent biases in the third-party attribution models (e.g.GA4 will always prioritize Google for source credit), the true impact of CTV advertising on site traffic tends to be underestimated.


To address this issue, MNTN is moving to a First-Touch model when it comes to our platform’s own reporting. If a user enters an advertiser’s site as depicted in the flow above, and the URL contains a UTM parameter signifying they clicked on a Google ad (utm_source = google, or gclid), then: VVS will attribute as a competing / first-touch Verified Visit


If no such parameters are present in the URL, VVS will attribute this as non-competing / Last touch Verified Visit.


Since our Google Analytics integration continues to rely on the Last-Touch model outlined in the first three sections of this piece, users can compare Last-Touch results in GA4 with First Touch results in MNTN’s reporting to gain a more complete picture of CTV’s influence.


### Beyond The Visit: Conversion Attribution Without Shortcuts


Whether you’re reporting in Google Analytics or MNTN’s dashboard, once VVS marks a session as Verified, every downstream conversion inside the advertiser‑defined window becomes eligible for credit. The Conversion Pixel logs raw orders to the data warehouse, where a suite of eight SQL‑based functions run in three‑hour batches to match each order to its originating Verified Visit. These matches may be same‑device, across devices, or linked through Google click identifiers when paid search is the final marketing touchpoint. Conversions are no longer bound to a one‑to‑one relationship with impressions; a single Verified Visit may generate multiple attributed orders, as long as they fall within the attribution window.


Please note:


- **GA4 only reports same‑session conversions** for MNTN, so a gap between MNTN and GA conversion counts is normal.
- **Attribution may differ by platform:** For example, if a user sees an ad from another channel, then watches a CTV ad, then visits the advertiser’s website and converts, GA4 will attribute the conversion to the first-touch it recorded (the other channel). In contrast, MNTN will credit the conversion to CTV (its own first touch) — offering a more complete view of its influence across the funnel.


*Fig E: Same session conversion event*


*Fig F: divergence of conversion event attribution in MNTN vs GA4*


# Putting It All Together: The Measurable Halo of CTV


Verified Visits replaces the missing click with a chain of deterministic observables: impression delivery, identity resolution at household scale, server‑side validation of visit provenance, and disciplined synchronization with Mixed Media Modeling and Multi-touch Attribution tools, like Google Analytics 4. Marketers benefit in three ways:


- **Total transparency.** Dashboards expose every Verified Visit, every touch that follows, and the exact impression that set the journey in motion.
- **Custom control.** Advertisers set both the Verified Visit window and the conversion window to mirror real‑world buying cycles.
- **Cross‑channel clarity.** Because results sync into GA4 and other analytics platforms, teams can evaluate CTV alongside search, social, email and affiliate within a single canvas.


# Conclusion


The television screen may be unclickable, but its power to drive commerce is now fully measurable. MNTN’s Verified Visits treats every ad view as the start of a scientific experiment, linking exposure to outcome with a rigor that rivals lower‑funnel digital channels.


This framework captures the cumulative influence of CTV across devices and over time, safeguards credit for other media when appropriate, and equips marketers with a trustworthy compass for optimization. In an era when accurate attribution can lift efficiency by double‑digit percentages, Verified Visits is not simply a reporting feature — it is the operating system that makes streaming television a first‑class performance marketing channel.
