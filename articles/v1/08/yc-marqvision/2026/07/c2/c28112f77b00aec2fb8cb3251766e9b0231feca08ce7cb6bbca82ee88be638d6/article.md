---
schema_version: "1.0.0"
document_id: "c28112f77b00aec2fb8cb3251766e9b0231feca08ce7cb6bbca82ee88be638d6"
company_key: "yc-marqvision"
company: "MarqVision"
source_id: "yc-marqvision-news-import-8b424d70c0ae"
canonical_url: "https://www.marqvision.com/blog/smart-rules-activity-log-brand-impersonation"
published_at: null
first_seen_at: "2026-07-22T03:25:43.979461+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:a525323d0a7a7f2dff904f84d11893d841db33e7f3bfcf9097ccf5f19dc8829f"
---

# Introducing Smart Rules and Activity Log for Brand Impersonation

In April, we launched[Rapid Takedowns for Brand Impersonations](https://www.marqvision.com/blog/rapid-takedowns-for-brand-impersonation) . The promise was simple: Marq AI finds fake domains, scam ads, and impersonation content in hours, not days, across the channels where your revenue actually lives.


‍


We delivered on speed. Detection that used to take days now happens in single-digit hours.


‍


Finding the fake wasn't the hardest part. Marq AI already detects impersonation across domains, paid ads, website content, and social media in hours. The hard part was everything between finding it and removing it -- the review queue, the wait for someone to confirm, the manual sign-off before any enforcement could begin. And while a flagged threat sat waiting, it kept stealing clicks and trust.


‍


Detection was never the bottleneck. The decision to act was. Today, that changes. This release adds two capabilities that enable fast detection to become rapid takedown.


### **Smart Rules**


Smart Rules let you automate enforcement on your terms. You define the conditions —which threat types, which channels, and the confidence level at which a flagged threat should be automatically enforced — and Marq AI carries out the approved enforcement the moment a threat meets those conditions. The clear-cut cases stop waiting in a queue.


‍


‍


This is automation you can actually configure and control, not a black box. Threats above it are enforced automatically; everything below it continues to be monitored and surfaced for your team to review. You can adjust your rules, review any action, or step in at any time.


‍


During beta, some of our best customers have turned on Smart Rules to automatically enforce and monitor:


‍


- Enforce any critical threats flagged by an AI-determined Severity Score ≥ 8
- Add non-actionable domains to the Watchlist, based on AI classification (Inactive, Parked, Clean)


‍


For our beta customers, the biggest win came during the client-confirmation stage, which has historically been the most manual step in the entire process.


‍


Once their Smart Rules were live, that step ran in **under an hour** for the threats their rules covered. For those teams, it was the difference between a fake coming down the same morning and a fake spreading for a week. Here's what one of our beta customers, Joe Zorovich, VP Operations at AS Beauty Group shared with us:


‍


> MarqVision's advanced AI-powered detection and takedown services have been crucial in identifying and removing threats across our key digital channels — consistently and at scale.
>
>
> — Joe Zorovich, VP Operations at AS Beauty Group


### The Results: What it adds up to


‍


‍


We measured our results with real incidents across domains, fake ads, and website content channels.


‍


- **End-to-end median response time is now under 9 hours across Domains, Fake Ads, and Website Content** -- from detecting a threat to filing enforcement -- up to roughly an order of magnitude faster than manual review.
- **The client-confirmation stage runs in under an hour** when handled by Smart Rules, removing the bottleneck that used to stretch takedowns into days or weeks.
- **Accuracy holds where it counts:** 99.8% across 48,253 domain impersonation incidents and 86.3% across 3,315 paid-ad incidents.


‍


And the numbers keep improving as more teams switch on Smart Rules. In early production use, with automated enforcement handling the clear-cut cases, we are already seeing end-to-end blended median response times fall below **three** hours -- pulling the same morning takedown even further out of reach for impersonators.


### **Activity Log**


Automating enforcement only works if you can see exactly what happened. The Activity Log is a complete record of every enforcement action: what was flagged, why, when action was taken, and the outcome. For legal, IP, and brand teams, that audit trail makes automated enforcement defensible -- a single, transparent log of every decision and result, with human oversight at every step.


‍


‍


That record does double duty. When our AI agent automatically detects, classifies, and reports, your team sees exactly what the AI did. And when your CEO, partner, or regulator asks why a specific takedown was filed, the answer is one click away instead of a reconstruction from email threads and screenshots. The same log that proves the AI acted correctly also gives your team the trail to defend, refine, and report on every action it takes.


### 5 New Threat Classification Types


‍


We've also added five new threat classification types for Marq AI agents to automatically detect and classify:


‍


- Copyright (Artwork)
- Publicity Rights
- Unauthorized Sales
- Design Infringement
- Scam


### Built for the full range protection of brand threats — and beyond


Fakes do not stay in one channel, so neither does Marq AI. It protects your[brand against impersonation](https://www.marqvision.com/impersonation) across domains, paid ads, website content, social media, and online marketplaces -- the full surface where coordinated phishing and fraud campaigns play out. The same AI-powered platform protects your brand against counterfeits in marketplaces and pirated content across the web. One platform, end-to-end — so your team is not stitching together solutions and manual escalations to cover the places your customers actually encounter your brand.


‍


This is a meaningful step toward a full agent transformation in brand protection: a future where AI acts on threats while keeping humans in control. Brand impostors now run coordinated campaigns across multiple channels — domains, ads, content, and social media. Protecting a brand today means having systems that can respond across the entire digital ecosystem at machine speed.


‍


Rapid Takedowns for Brand Impersonation is now generally available.[See Marq AI in action](https://www.marqvision.com/request-demo) .


‍


‍


## Frequently Asked Questions


### 1. What are Smart Rules in Marq AI?


Smart Rules are rule-based triggers you configure in advance. You define the criteria, for example, a specific threat type with a severity score of 8 or higher, and any incident Marq AI detects that matches is enforced automatically without waiting for manual sign-off. Lower-risk items that do not match a rule are surfaced for your team to review.


### 2. Which channels does automated enforcement now cover?


Smart Rules now cover Digital Risk Protection channels -- domains, scam ads, and website content -- in addition to the anti-counterfeit module, where a similar rule system was already available.


### 3. How accurate is Marq AI's detection?


Marq AI held **99.8% accuracy across 48,253 domain impersonation incidents** and **86.3% accuracy across 3,315 paid-ad incidents** during the beta period. Detection that previously took days now takes single-digit hours, with no sacrifice in precision or accuracy.


### 4. What is the Activity Log, and why does it matter?


The Activity Log is a complete audit record of every AI-driven decision: what was flagged, why it was flagged, when action was taken, and the outcome. For legal and IP teams, it provides the documentation needed to make automated enforcement defensible and auditable -- the difference between "the AI did something" and "here is exactly what the AI did, why, and what happened."


### 5. Can my team still review and override automated enforcement?


Yes. Smart Rules operate within boundaries you define. Your team sets the criteria, reviews lower-risk flagged items, and you can toggle the switch off and override any rule at any point.
