---
schema_version: "1.0.0"
document_id: "388a115e3cf81eba169ab2b1e82090ddfb36fe53bac792c0014bba539b5067a0"
company_key: "yc-surface-labs"
company: "Surface Labs"
source_id: "yc-surface-labs-news-import-ffea4c1e1d4e"
canonical_url: "https://withsurface.com/blog/how-to-run-a-full-inbound-campaign-from-setup-to-booked-meeting"
published_at: "2026-08-11T20:40:14.373+00:00"
first_seen_at: "2026-08-11T22:45:21.865194+00:00"
fetched_at: "2026-08-11T22:45:23.958134+00:00"
content_hash: "sha256:6f57b2d278b498b2f08a817d512a6185038a95d3410edc2f748f9687cd232109"
---

# How to Run a Full Inbound Campaign From Setup to Booked Meeting

Most inbound campaigns are half-built. Marketing creates the ad, writes the landing page, and launches the form. Then the lead enters a gap — CRM workflows, Zapier automations, manual routing, and a rep who may or may not see the notification today. The campaign was designed to capture leads. Nobody designed the system to convert them.


A complete inbound campaign covers every step from first click to booked meeting — and every step should be configured before the campaign goes live. Here's the full setup.


## The five components of a complete campaign


Component What it covers What most teams do What they should do


**1. Capture** The form on the landing page 4-field form: name, email, company, "how can we help?" Multi-step form with qualifying questions, conditional logic, and contextual design that matches the ad's promise


**2. Qualification** Determining lead quality at the moment of capture Nothing — leads enter the CRM unscored. SDR qualifies on the phone. Form responses + enrichment data evaluated in real time. Lead is scored and segmented before a human touches it.


**3. Routing** Assigning the lead to the right rep Round-robin or manual assignment from a CRM queue Attribute-based routing — territory, segment, product interest, availability — triggered at the moment of form submission


**4. Response** First contact with the lead Rep gets a notification and follows up when they see it. Average: 42 min – several hours. Automated personalized response within 60 seconds — rep name, lead context, scheduling link


**5. Scheduling** Getting the meeting on the calendar Rep sends a Calendly link in a follow-up email, hours later Embedded scheduling in the form flow — lead books on the matched rep's calendar before they close the tab


Most campaigns handle component 1 and stop. Components 2–5 are left to existing infrastructure — which usually means a patchwork of CRM workflows and manual processes that weren't designed for this specific campaign.


The result: the campaign generates leads, but the leads don't convert to meetings at the rate the ad spend justifies.


## Setting up each component


### Component 1: The form


The form is the campaign's conversion point. It needs to do three jobs: collect contact info, gather qualification data, and feel like a natural extension of the ad that brought the visitor here.


**Match the form to the ad's promise.** If the ad says "Get a custom demo," the form should feel like the path to a custom demo — not a generic contact form. The first question should reference what the visitor came for: "What are you most interested in seeing?" with options that map to your product.


**Add 3–4 qualifying questions.** Company size, budget range, timeline, and use case. Use conditional logic — if they select "just researching," show a shorter path. If they select "evaluating this quarter, budget approved," show the full qualification flow plus embedded scheduling.


**Use multi-step design.** Split the form into 2–3 steps. Step 1: what they're looking for. Step 2: qualifying details. Step 3: contact info + scheduling. Each step should feel like progress, not interrogation.


### Component 2: Qualification rules


Before the campaign launches, define what "qualified" means for this specific campaign. Write it out explicitly:


**Qualified for immediate sales conversation:** 100+ employees AND timeline is this quarter AND budget is in range. Route immediately to AE.


**Qualified for SDR follow-up:** 50–100 employees OR timeline is next quarter. Route to SDR team same-day.


**Nurture track:** Under 50 employees OR no timeline OR no budget. Enter automated nurture sequence.


**Disqualify:** Student, competitor, spam. Filter out before CRM entry.


These rules should be configured in your lead ops system before the first ad dollar is spent. If they're configured after launch, every lead that arrives before configuration gets the wrong treatment.


### Component 3: Routing


Map the campaign's leads to your sales structure. For a product-specific campaign, leads should route to the team that handles that product. For a geo-targeted campaign, leads should route to the territory team.


Define the primary routing rule, the fallback (if the primary rep is unavailable), and the default (if nothing matches). Test by running 10 hypothetical leads through the rules and confirming each one lands in the right place.


### Component 4: Response templates


Write the first response message before launch — personalized with dynamic fields:


"Hi \[first name\], thanks for requesting a demo of \[product they selected\]. Based on your team size and interest in \[use case\], I've connected you with \[matched rep name\] on our \[segment\] team. \[Rep name\] works with companies like \[company name\] and can walk you through exactly how this works for your use case. Here's a link to book directly: \[scheduling link\]."


This message fires automatically when routing completes — within 60 seconds of form submission. The lead gets a specific, personal response while they're still on your site.


### Component 5: Scheduling


Connect the form to your reps' calendars so the lead can book in the same flow. The scheduling step should appear after qualification and routing — so the lead is booking on the specific rep's calendar, not a generic team calendar.


If embedded scheduling isn't possible, the automated response message (Component 4) must include a direct scheduling link for the matched rep. Don't make the lead wait for a separate email.


## The pre-launch checklist


Before turning on ads:


- Form is live with qualifying questions and conditional logic
- Qualification rules are configured and tested
- Routing rules match the campaign's target audience to the right reps
- Response template is written with dynamic fields
- Scheduling is connected to routed reps' calendars
- Speed-to-lead monitoring is active
- Fallback and default rules are in place for edge cases


If any of these are incomplete, the campaign will generate leads that don't convert — and you'll spend money driving traffic into a broken pipeline.


## Where Surface fits


Surface handles all five components in one system — smart forms, real-time qualification, attribute-based routing, automated personalized response, and embedded scheduling. Configure the campaign once, and every lead that arrives gets the full treatment from form submit to meeting booked.


If your campaigns consistently generate leads but underperform on meetings, the issue isn't the ads. It's that Components 2–5 weren't set up before launch. Surface makes the full campaign setup the default, not an afterthought.
