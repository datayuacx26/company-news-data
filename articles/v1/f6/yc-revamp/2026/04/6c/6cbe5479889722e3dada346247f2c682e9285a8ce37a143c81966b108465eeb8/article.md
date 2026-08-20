---
schema_version: "1.0.0"
document_id: "6cbe5479889722e3dada346247f2c682e9285a8ce37a143c81966b108465eeb8"
company_key: "yc-revamp"
company: "Revamp"
source_id: "yc-revamp-news-import-b840498b5578"
canonical_url: "https://www.getrevamp.ai/blog/introducing-campaigns"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-25T21:29:53.496213+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:6e27b955d81f010f052167a6235c508fb29907ac3b78c5f8eedd0b7142d7a03c"
---

# Introducing Campaigns: AI-Personalized Email at Scale

Today, we're announcing Revamp Campaigns, a new way to generate deeply personalized email content for every customer in your audience, powered by real-world data signals and AI. Campaigns transforms what was once a manual, segment-by-segment process into a single workflow that produces unique content for every profile in your Klaviyo account.


Since we launched Revamp, teams have used our flow builder to create sophisticated marketing automations that respond to customer behavior in real time. Those flows handle millions of triggered events across abandoned carts, browse activity, and lifecycle moments. But one gap has remained stubbornly manual: campaign emails, the one-to-many sends that make up the bulk of most brands' email volume.


The challenge is straightforward. Personalization beyond {{first_name}} requires real data, what someone bought, what they browsed, what the weather is where they live, and the creative judgment to weave that data into content that feels human. Doing that for 50,000 profiles isn't a design problem. It's an infrastructure problem.


Campaigns solves it. Select your audience, configure your content blocks and data sources, preview a handful of profiles to verify quality, then generate and sync. All from one screen, in minutes.


Campaigns dashboard showing multiple campaigns with mixed statuses and profile counts


## The personalization gap in campaign email


Most email platforms offer two flavors of personalization: merge tags and conditional blocks. Merge tags insert a name or a city. Conditional blocks show or hide entire sections based on segment membership. Both are useful. Neither gets you to content that actually reflects the individual.


Consider the difference:


> **Merge tag:** "Hi Sarah, check out our new arrivals."


> **Revamp Campaign:** "Hi Sarah, it's going to be rainy in Portland this week. Here's a waterproof jacket we think you'll love based on the trail runners you picked up last month, plus a few items other outdoor enthusiasts in the Pacific Northwest are rating highly."


That second email requires knowing Sarah's location, checking a weather API, looking up her order history, finding relevant product reviews, and generating a sentence that ties it all together naturally. Doing that once is feasible. Doing it for every profile in a segment is what Campaigns automates.


## How it works


Campaigns follows a four-step workflow designed to give teams full control at every stage.


### 1. Audience


Select one or more Klaviyo lists and segments as your target audience. Campaigns pulls profile counts in real time and supports combining multiple audiences, useful when a campaign spans VIP customers from one segment and re-engagement targets from another.


You also set a property prefix at this stage, which namespaces all the Klaviyo profile properties that Campaigns will create. This keeps your Klaviyo data clean and makes it easy to reference generated content in your templates later.


### 2. Personalization


This is where Campaigns gets interesting. You configure two things: content blocks and data sources.


Content blocks are the pieces of content you want generated for each profile: subject line, preheader, headline, body copy, button text, or any combination. Each block gets its own natural language instructions that guide the AI's output. You can work in two modes:


- Content-only mode for teams that want standalone personalized fields to drop into existing Klaviyo templates
- Template mode for teams that want to import a Klaviyo template directly and mark specific text blocks for AI rewriting, preserving the design while personalizing the copy


Data sources are the real-world signals that feed into generation. Toggle on the ones relevant to your campaign:


Category Data Source What it provides


Location Location Customer city, region, country


Location Weather Current conditions and optional 5-day forecast


Location Air Quality Local air pollution index


Product Previous Orders Recent purchase details with configurable time windows


Product Browsing History Recently viewed products


Product Product Reviews Review data for products the customer has interacted with


Product Recommendations AI-powered product suggestions based on behavior


Data First-Party Data Custom Klaviyo profile properties you select


Every combination produces a different kind of email. A wellness brand might use weather + air quality + product recommendations. A fashion brand might use browsing history + previous orders + location. The AI synthesizes whatever signals you enable into content that reads naturally, not like a spreadsheet was pasted into a paragraph.


### 3. Preview


Before generating at scale, Campaigns pulls a sample of real profiles from your audience and generates preview content for each one. You can see exactly what the AI produces for specific customers: a subject line tailored to a shopper in Austin who bought running shoes last Tuesday, next to one for a customer in Montreal who browsed winter coats yesterday.


If something feels off, go back and adjust your instructions or data sources. Iteration happens here, not after you've generated content for 40,000 profiles.


### 4. Generate & Sync


When you're satisfied with the previews, start generation. Campaigns processes every profile in your audience, and you can watch progress in real time through a terminal-style log that shows each profile as it completes, including timestamps, success or failure status, and error details.


Once generation finishes, you review the results and sync to Klaviyo. This pushes the generated content as custom profile properties, where they become available as dynamic variables in any Klaviyo template or flow. If you built your campaign in template mode, you can also save the result as a new Klaviyo template with one click.


The entire pipeline, from audience selection to synced profile properties, typically completes in minutes, even for audiences of tens of thousands of profiles.


## Why profile properties, not pre-rendered emails


A deliberate design choice in Campaigns is that we generate content, not emails. The output is a set of custom properties on each Klaviyo profile (a personalized subject line, a tailored body paragraph, a contextual CTA) not a finished HTML email.


This matters because it gives you full control over how and when the content is used. Reference it in a one-time campaign send. Pull it into a triggered flow. Use it in an A/B test. Combine AI-generated blocks with your own static content. The personalized data lives on the profile, ready to be used wherever Klaviyo's template engine can reach it.


It also means your existing Klaviyo workflows don't need to change. If your team already has email designs, approval processes, and sending schedules dialed in, Campaigns slots into that workflow. It doesn't replace it.


## Built for iteration


We learned from our flow builder that marketing teams need to iterate quickly. Campaigns reflects that in several ways:


- Undo and redo across all personalization settings. Block configurations, data source toggles, and instructions all support stepping backward and forward through changes.
- Per-block data source control. Disable specific data sources for individual content blocks when they're not relevant.
- Cancel and retry. Stop a generation mid-run if you spot an issue, then retry only the failed profiles instead of re-running the entire audience.
- Browse generated content. After generation, search and filter through individual profile results to audit quality before syncing.


## Pricing that starts at zero


Campaigns includes 1,000 free profile generations per month with no credit card required. After the free tier, pricing is usage-based at $0.02 per profile, billed monthly. There are no seat fees, no tier gates, and no minimum commitments.


For a brand sending a personalized campaign to 10,000 profiles, that's $180 per month. For context, the lift in engagement from genuinely personalized content, not just {{first_name}}, typically pays for itself within the first send.


## What's next


Campaigns is the foundation for a broader vision: making every customer interaction feel like it was crafted by someone who actually knows the customer. Today, that starts with campaign email. But the same architecture (audience selection, data source configuration, AI generation, platform sync) extends naturally to SMS, push notifications, landing pages, and in-app experiences.


We're also investing in closed-loop optimization. As campaigns are sent and engagement data flows back into Klaviyo, future versions of Campaigns will use that signal to improve generation quality over time, learning which data sources, content styles, and personalization strategies drive the best results for your specific audience.


For now, we're excited to put this in your hands. If you're already using Revamp, Campaigns is available today in your dashboard. If you're new to Revamp, we'd love to show you what personalization at scale looks like.


Get Started


Revamp Campaigns is available to all Revamp users. Visit your dashboard to create your first campaign, or reach out to our team for a walkthrough.


[Book a Demo](https://www.getrevamp.ai/book-demo)
