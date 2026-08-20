---
schema_version: "1.0.0"
document_id: "3c062585404a5e65284d0e0b3fbb146883983bf546aec568df89c367149da676"
company_key: "yc-chatfuel"
company: "Chatfuel"
source_id: "yc-chatfuel-news-import-f3b6504a20fb"
canonical_url: "https://chatfuel.com/blog/chatfuel-vs-tintim"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T00:43:29.079521+00:00"
fetched_at: "2026-08-18T00:43:33.106969+00:00"
content_hash: "sha256:8974a866c43f1270425c6d5cc2c5246107da5b74137998f670a4beba6a24add3"
---

# Chatfuel vs Tintim: How They Actually Compare

Looking for an honest Tintim review before committing to a plan? This guide covers how Tintim works, what its free trial actually gives you, how its pricing and cancellation terms read in the fine print, and how it compares to Chatfuel for WhatsApp Conversions API tracking. If you're searching for Tintim alternativas or trying to decide whether Tintim is the right tool for your Meta Ads attribution, this is the comparison to read first.


Tintim is the name that comes up most often when agencies talk about proving WhatsApp sales to Meta. It's the market leader in Brazil, reportedly past R$1M in monthly revenue. This comparison is based on a real 7-day trial of Tintim's top plan and its own terms of service, not just what each product claims about itself.


## At a glance


What Tintim does


What Chatfuel does


Reads the conversation and reports outcomes back to Meta, doesn't run any part of it


Runs the automation and reports the event from inside it


Detects a sale by trigger phrase, manual drag, or an AI batch that runs once a day and needs next-morning approval


Detects a sale, booking, or any event you define, using rules you set yourself


Reads typed text only, a sale closed by voice note or a photo is invisible to it


Reads voice notes and images, not just text


Official API and Coexistence are paid-plan-only, even on the free trial


Connects only through the official WhatsApp Business API, no paid gate on it


## The core difference: watching a conversation vs. running one


Tintim describes itself plainly in its own FAQ: "Tintim is not a CRM, but it can be used as one. It is an intelligence tool." It reads WhatsApp conversations and reports outcomes back to Meta. It doesn't send messages, qualify leads, or run any part of the conversation itself.


Chatfuel runs the automation and reports the Conversions API event from inside it. That's not a values statement, it's a structural difference: one product watches a conversation that's happening somewhere else, the other is the thing having the conversation.


## What is Tintim?


Tintim is a Brazilian SaaS tool built specifically to prove that WhatsApp conversations generate sales for Meta Ads campaigns. It connects to existing WhatsApp chats, reads the message history, detects conversion signals, and sends those events to Meta via the Conversions API (CAPI). It does not send messages, run chatbots, or replace a CRM — its own FAQ describes it as an "intelligence tool," not a conversation platform. Agencies use it when they want to show Meta that WhatsApp-driven campaigns are producing revenue without rebuilding their existing chat infrastructure.


## How each tool detects a sale


This is where the products genuinely diverge, and it's worth being specific instead of vague about it.


Tintim currently detects a sale three ways. An agent can type a pre-configured trigger phrase into the chat, which the system matches (its own documentation tells users to save the phrase as a WhatsApp quick reply "to avoid typing errors," because the match breaks on an emoji or exclamation mark). An agent can manually drag a card in a Kanban board. Or, since late July 2026, an AI feature called "Inspetor de Vendas" can flag a probable sale, but it runs once a day overnight, shows a confidence score with a few messages of context, and still requires a human to approve it the next morning. That approval can't be undone, and the feature only reads text, so a sale closed by voice note or a photo of a receipt is invisible to it.


Chatfuel's approach starts from a wider set of signals than "did someone say the trigger word": a booking confirmed on a calendar, a status change in the pipeline, a handoff to a human agent, a property update, or a rule you define yourself in plain language, not locked to one fixed "sale detected" trigger. And because qualification runs inside the same conversation Chatfuel is already having, it isn't limited to typed text. Voice notes and images get read too.


## How Tintim integrates with Meta Ads and WhatsApp


Tintim connects to Meta through the Conversions API, sending conversion events directly to Meta's servers rather than relying on browser-based tracking. It also supports Meta Pixel dispatch and Google Ads offline conversions, covering the full standard events set on the Meta side. The integration works as an external observer: Tintim reads the chat, identifies a trigger, and fires the event. It does not require changes to your existing WhatsApp setup beyond connecting the number. The practical limitation is that event quality depends entirely on how reliably the trigger is detected — and as covered above, that detection has meaningful gaps when conversations happen via voice note, image, or informal language that doesn't match the trigger phrase exactly.


## What the free trial actually offers


This is the most concrete difference, and it's the one you'd only find by actually going through Tintim's signup flow rather than reading its landing page.


During the trial, Tintim presents three ways to connect WhatsApp: Official API, Coexistence, and an unofficial connection. The first two, the ones that don't put your number at risk, are both marked as paid-plan-only. On a free trial meant to build confidence in the product, the only option you can actually pick is the unofficial one: a QR-code pairing, the same mechanism as WhatsApp Web, with a warning not to send a screenshot of the code to anyone else.


Chatfuel connects only through the official WhatsApp Business API, as a Meta Business Partner. There's no paid gate on the trustworthy connection method, because there's no other kind offered in the first place.


## Tintim pricing and cancellation terms


Tintim's self-serve plans run R$197 to R$297 per month, with agency discounts advertised from 65% off for resellers.


The cancellation terms are worth reading carefully before committing to an annual plan. Canceling mid-year means owing the full remaining balance, not a prorated fee. Tintim's own documentation uses the example of canceling in month three of an annual plan and owing the other nine months in full. This is an unusually strict cancellation policy compared to most SaaS tools in this category.


## Who each tool actually fits


Tintim fits someone who already knows Meta Ads, already has WhatsApp automation running through something else, and wants a bolt-on layer purely for attribution, and who's comfortable with the tradeoffs above.


Chatfuel fits someone who wants the automation and the attribution to be the same system from the start, connected officially from day one, without a paid plan standing between them and the trustworthy way to connect.


## Tintim alternatives: what to consider before choosing


If you're evaluating Tintim alternatives, the core question is whether you need attribution only, or attribution plus automation. Tools that only observe conversations always depend on how accurately an agent enters a trigger or how quickly an AI batch processes overnight data. Tools that run the conversation themselves can fire events at the exact moment a defined condition is met — no manual step, no next-morning review. Other categories worth comparing: native WhatsApp Business API platforms with built-in CAPI support, CRM tools with WhatsApp integrations that include conversion tracking, and Meta Business Partners that offer both automation and attribution in a single system. Chatfuel falls into the last category and is an official Meta Business Partner, which matters for long-term connection reliability.


## Track WhatsApp conversions with Chatfuel


If what you actually need is proof of what a WhatsApp conversation turned into, not just a keyword watcher bolted onto whatever's already running your chats, that's what Chatfuel's Conversions API integration is built for.
