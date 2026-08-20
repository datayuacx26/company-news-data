---
schema_version: "1.0.0"
document_id: "bfeed28a941a3d12fc7ca7396f4babea27fe3738f4ae1670d40c0819035e475f"
company_key: "yc-vybe"
company: "Vybe"
source_id: "yc-vybe-news-import-452c5ac9be13"
canonical_url: "https://www.vybe.build/blog/how-to-set-up-ashley-belfort-ai-sales-assistant"
published_at: "2026-06-18T18:14:00+00:00"
first_seen_at: "2026-07-24T07:18:06.647302+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:2e22d4016c308ff7f2413833251890b96c24ea8b2635f3a79d693c8c94c72477"
---

# How to Set Up Ashley Belfort, Your AI Sales Assistant on Vybe

## How to Set Up Ashley Belfort, Your AI Sales Assistant on Vybe


You have 8 sales calls this week. No time to research each prospect, your call notes are scattered across three apps, and the follow-up emails you meant to send Tuesday are still in drafts on Thursday.


[Ashley Belfort](https://www.vybe.build/agent/ashley) exists for exactly this. She is Vybe's AI Sales Assistant, built to handle the full sales call loop: pre-call research, live note-taking, follow-up drafts, and CRM updates. Set her up once and she runs it on every call from there.


This guide walks you through setup from scratch -- which integrations to connect, in what order, and what Ashley actually does with each one.


## What Ashley actually does


She covers four stages of the sales workflow:


**Before the call:** Ashley monitors your Google Calendar, spots upcoming sales meetings, and sends a pre-call brief to your inbox before each one. Company background, recent news, key people on the call, talking points, and risks to watch for.


**During the call:** She joins the meeting directly (Google Meet, Zoom, Teams, or Webex), records it, and produces structured notes: what was discussed, objections raised, commitments made, next steps. You talk. She writes.


**After the call:** She drafts a follow-up email based on the call transcript and drops it into your Gmail as a draft. Plain language, no corporate filler, ready to send in one click.


**In your CRM:** She logs a note after every call with the outcome, key topics, and agreed next steps. She also pulls deal history and contact data from your CRM before calls to make the briefs sharper over time.


If you want to see what a fully autonomous sales workflow looks like across a team, the[AI sales assistant playbook](https://www.vybe.build/blog/build-ai-sales-assistant-closes-deals) covers that in more depth.


## The integrations Ashley uses


Ashley works across four integrations. Here is what each one does and why it matters.


**[Google Calendar](https://www.vybe.build/integrations/google_calendar)** is the core trigger. Ashley watches your calendar for meetings and uses them to kick off prep and follow-up workflows automatically. Without it, nothing runs on its own -- you would have to prompt her manually every time.


**[Gmail](https://www.vybe.build/integrations/gmail)** is where follow-up drafts land. After each call, Ashley writes the email and places it directly in your drafts folder. You review it, tweak anything you want, and hit send. She never sends anything without your review.


**Meeting recording** (Google Meet, Zoom, Webex, or Teams) is how she gets into your calls. You either add her agent email as a guest on a calendar event, or paste a meeting link in chat and she joins immediately. She records the session and produces structured notes automatically.


**CRM** --[Salesforce](https://www.vybe.build/integrations/salesforce_rest_api) ,[HubSpot](https://www.vybe.build/integrations/hubspot) , or[Pipedrive](https://www.vybe.build/integrations/pipedrive) -- is where she logs call outcomes and pulls pre-call context. This one is optional to get started, but it is what makes the briefs genuinely useful over time rather than just generic web research. Once connected, she can pull deal history, past call notes, and contact context into every brief automatically.


Vybe connects to[over 3,000 integrations](https://www.vybe.build/integrations) in total, so if your stack is different, chances are she can still plug in.


## How to set up Ashley: step by step


### Step 1: Create the agent


Go to[Ashley Belfort's agent page](https://www.vybe.build/agent/ashley) on Vybe. Click to create her. She will introduce herself and walk you through onboarding in about five minutes. If you are already in the chat, type "let's set you up" and she will start from there.


### Step 2: Connect Google Calendar


This is the first thing Ashley asks for. Connect the Google account that has your sales calls on it. Once it is linked via the[Google Calendar integration](https://www.vybe.build/integrations/google_calendar) , she can see your upcoming meetings, trigger pre-call research before each one, and know when calls wrap so the follow-up workflow starts automatically.


### Step 3: Run your first pre-call brief


After Calendar is connected, Ashley scans your next seven days, finds the most relevant upcoming sales call, and runs live web research on the company. The brief lands in your inbox: company snapshot, recent news, key people on the call, suggested talking points, and a note on anything to watch for.


This happens without you prompting her. That is the point.


### Step 4: Connect Gmail for follow-up drafts


Ashley will ask for[Gmail](https://www.vybe.build/integrations/gmail) access so she can drop follow-up drafts directly into your inbox. In practice, this is the feature that saves the most time per week. The email is there when the call ends, it sounds like a person wrote it, and all you have to do is decide whether to hit send.


If you skip this step for now, she will send the draft to your Vybe inbox so you can still see what it would have looked like.


### Step 5: Get Ashley into your calls


Add her agent email address as a guest on any calendar event. She will get the invite, see the meeting link, and join when it starts. You can also paste any meeting URL in chat at any point and she will join immediately, no calendar invite needed.


### Step 6 (optional): Connect your CRM


Connect[Salesforce](https://www.vybe.build/integrations/salesforce_rest_api) ,[HubSpot](https://www.vybe.build/integrations/hubspot) , or[Pipedrive](https://www.vybe.build/integrations/pipedrive) and Ashley will start logging call outcomes automatically after each meeting. She will also pull deal history and contact data into your pre-call briefs, so instead of generic company background you are getting context specific to your actual pipeline. If you are thinking about building a lightweight custom CRM on top of this, the[build a custom CRM with AI guide](https://www.vybe.build/blog/build-custom-crm-with-ai-one-prompt) is worth reading.


### Step 7 (optional): Set up automated daily routines


Once the core integrations are in place, Ashley can run two routines every day without being asked:


- A morning routine that pulls all your sales calls for the day, researches each prospect, and emails you a pre-call brief for every meeting before it happens.
- An end-of-day routine that reviews every call from the day and drops a follow-up draft in your Gmail for each one.


You tell her what time your day starts and ends. She handles the scheduling.


## A few things worth knowing before you start


You do not have to connect everything at once. Calendar and web research work on their own from day one. Gmail and CRM add more leverage, but they are not blockers.


Ashley does not send emails without your review. Every follow-up lands in your Gmail drafts first. Nothing goes out without you seeing it.


The briefs get better over time. Before the CRM is connected, she works from web research alone. Once it is linked, she pulls deal history, past call notes, and contact context into the brief. The difference is noticeable from the first call where it kicks in.


You can talk to her in plain language anytime: "What calls do I have today?", "Pull up the deal history for Acme before my 3pm", "What did we discuss with Sarah last week?" She handles all of it in chat, no specific commands needed.


If you are a founder running sales solo alongside everything else, the piece on[how founders use AI agents for sales and recruiting](https://www.vybe.build/blog/ai-agents-for-startup-founders) covers how to think about delegation at that stage.


## Ready to get started?


Set up Ashley on Vybe and stop doing sales prep manually. Your calls are happening either way. The research, the notes, and the follow-ups do not have to be on you.


[Try Vybe and get Ashley running today.](https://www.vybe.build/?utm_source=blog&utm_medium=cta&utm_campaign=agents&utm_content=how-to-set-up-ashley-belfort-ai-sales-assistant)
