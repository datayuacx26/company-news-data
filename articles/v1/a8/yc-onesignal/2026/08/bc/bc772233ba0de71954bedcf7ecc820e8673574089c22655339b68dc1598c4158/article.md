---
schema_version: "1.0.0"
document_id: "bc772233ba0de71954bedcf7ecc820e8673574089c22655339b68dc1598c4158"
company_key: "yc-onesignal"
company: "OneSignal"
source_id: "yc-onesignal-rss-a77922638bdd"
canonical_url: "https://onesignal.com/blog/what-we-shipped-in-july/"
published_at: "2026-08-03T16:32:14+00:00"
first_seen_at: "2026-08-03T19:20:06.156485+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:b216f705417bee6d354c7fa86685ba0325c93efb3a2602f6b82174f0b447899a"
---

# What We Shipped in July

Most engagement platforms are very good at telling you what happened to your messages. Delivered. Opened. Clicked. Fewer are good at telling you what happened to your business.


That gap is where most of this month's work landed. **Conversion Metrics are now generally available to every account, including free plans** . You can set a goal on a conversion metric directly inside a Journey and watch each message node report against it. And on the email side, a **new Deliverability Insights beta breaks your inbox placement down by provider** , so a Gmail problem can't hide behind a healthy Yahoo average.


Alongside that, we made OneSignal easier to reach from wherever you already work (including any AI assistant that speaks MCP) and cleaned up a handful of things that have been quietly annoying people for a while.


Here's the full list:


### Conversion metrics are now live for all customers


Conversion Metrics have moved to GA, and every account now gets them on the dashboard and in message reports. That includes free plans.


- **Free plans** see clicks and unattributed sessions on the conversions chart.
- **Paid plans** add omni-channel session attribution and custom conversion metrics built from your own custom events.


The practical difference: instead of comparing click rates across four channels and guessing which one moved the needle, you can see how messages across every channel drive sessions and conversions in one place. And with custom metrics built on your events, you can tie messaging directly to the outcomes you actually report on — purchases, signups, subscription starts.


> **Availability:** GA for all customers ([Learn more](https://documentation.onesignal.com/docs/en/conversion-metrics#conversion-metrics) )


### Set goals on conversion metrics inside Journeys


Conversion Metrics tell you what happened. Goals tell you whether it was good enough.


You can now set a goal on a conversion metric directly inside a Journey; right on the message nodes themselves. Pick any metric and give it a greater-than or less-than threshold: a raw count, a percentage, a conversion value, or value per delivery.


Progress shows up in each node's report. The goal badge turns green when the goal is met and yellow when it isn't, so a glance at the canvas tells you which messages in the Journey are pulling their weight and which need work.


> **Availability:** Live for all paid plans ([Learn more](https://documentation.onesignal.com/docs/en/goals#goals) )


### Email Deliverability Insights (Beta)


Blended deliverability averages are comfortable and misleading. A 94% delivered rate can be a 99% Yahoo number carrying a Gmail number you'd want to know about.


The new **Deliverability Insights** tab on your Email index shows exactly where your email lands, broken down by inbox provider — Gmail, Outlook, Yahoo, Apple, and more. You get sender reputation, provider-by-provider performance, and trends over time, filterable by sending domain.


Two things we're especially glad to ship here:


- **An insights banner that flags problem providers first.** You don't have to go hunting for the outlier. It tells you what to fix and where.
- **Honest complaint data.** Google and Apple don't report complaints back, which means blended complaint rates have always understated the real picture. Breaking it out by provider closes that blind spot.


You can also ask[OneSignal AI](https://onesignal.com/blog/the-future-of-lifecycle-marketing-is-autonomous/) any deliverability question and it will answer from this data — useful when you want the "why did Gmail change last week" answer without building the report yourself.


The point is to catch and repair problems while they're still small, before they become a sender reputation problem.


> **Availability:** Beta, available to all OneSignal Mail customers today ([Learn more](https://documentation.onesignal.com/docs/en/email-deliverability-insights) )


### Connect the OneSignal MCP to any AI client


When we launched the[OneSignal MCP Server](https://documentation.onesignal.com/docs/en/model-context-protocol#what-is-mcp) , connecting it meant copying API keys and App IDs into a config file. That worked, but it was a barrier and we’re pleased to remove that.


Now any AI assistant that supports MCP can connect to OneSignal over OAuth.


- **Any MCP client works.** Beyond our Cursor and ChatGPT marketplace listings, you can connect Claude, GitHub Copilot, and any other MCP-compatible AI tool.
- **One-click sign-in.** Authenticate through a browser sign-in with your OneSignal account and approve access. No keys to copy, no App ID to look up.
- **Revoke anytime.** Every connected AI tool shows up in your account settings, and you can cut off access whenever you want.


This is a better experience and a more secure default: scoped, revocable access instead of a long-lived key pasted into a config file. More importantly, it means we meet you inside whatever AI tool you already use, not just the ones with a marketplace listing.


> **Availability:** Live now for all customers ([Documentation](https://documentation.onesignal.com/docs/en/model-context-protocol) )


### Cursor + OneSignal MCP


We’re live in the Cursor Marketplace. Easily install the OneSignal MCP to work in Cursor.


Your developers get OneSignal tooling inside their editor without going looking for it — sending pushes, building segments, and querying data from the same window they're already working in.


> **Availability:** Live in the Cursor marketplace now ([Learn more](https://cursor.com/marketplace/onesignal) )


### Test RCS sends before carrier approval


Carrier approval for an RCS sender can take weeks. Until now, those were weeks of waiting rather than building.


You can now send test RCS messages to your own devices while your RCS sender is still pending carrier approval, and compose and preview real RCS content in the editor from day one. So you can design the carousel, check how the rich cards render on an actual phone, and get your content finished — instead of discovering rendering surprises the week you go live.


> **Availability:** Available to all customers contracted for RCS ([Learn more](https://share.zight.com/QwumK952) )


### **Quality-of-life improvements**


These three fix things that have been quietly costing people time.


### **Search users by IP address**


There's a new **IP Address** option in the search dropdown on your Users page (Audience > Users). It works with both IPv4 and IPv6, uses exact match, and trims leading and trailing whitespace automatically.


This matters most for push-only users. If someone has no email, no phone number, and no external ID, IP is often the only identifier you have to work with. Now you can find a specific device or test user in seconds instead of hitting a dead end.


### **Deterministic pagination on the Users index**


The Users index now pages through your audience predictably. No more records repeating or shifting as you click next and previous.


If you've ever tried to audit an audience after an import — or walk a new teammate through their users during onboarding — you've run into this. Every page now shows each user exactly once.


### **Refreshed default in-app messages**


The default in-app message has been modernized in both the HTML editor and the Block editor. You get a polished starting point the moment you create your first in-app message, rather than a dated template to fight your way out of.


### We’ll Keep Shipping


We’re happy to deliver a bunch of new features and updates for our customers in July. We continue to ship faster and faster each month, delivering more value to you.


You can always check the[changelog](https://documentation.onesignal.com/changelog) for more real time updates before our next month’s roundup.


If you're new to OneSignal, you can[start for free](https://dashboard.onesignal.com/signup) or[talk to our team](https://onesignal.com/contact) about what makes sense for your business.


[Get Started for Free](https://app.onesignal.com/signup)
