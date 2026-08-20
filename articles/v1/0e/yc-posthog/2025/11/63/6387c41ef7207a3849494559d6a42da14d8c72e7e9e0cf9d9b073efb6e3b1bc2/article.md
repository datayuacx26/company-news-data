---
schema_version: "1.0.0"
document_id: "6387c41ef7207a3849494559d6a42da14d8c72e7e9e0cf9d9b073efb6e3b1bc2"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/workflows-alpha"
published_at: "2025-11-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:b6d33bdef8d9a7cea963018291e1071e78c93c2a87e2f37700b5cdb00c1d0238"
---

# Workflows are now in Alpha and I already broke mine

# Workflows are now in Alpha and I already broke mine


- [Sara Miteva](https://posthog.com/community/profiles/35224)


Nov 03, 2025


- [Product](https://posthog.com/blog/product)


#### Contents


-
-
-
-
-
-


We've been building **[Workflows](https://posthog.com/docs/workflows)** for a while now: this is our Zapier-style automation builder you can use to trigger actions based on live user behavior. And, while it’s still in its early days, it’s already packing some serious muscle.


You can define triggers (like “user viewed this page” or “performed this event”), add logic (delays, splits, or exit conditions), and finish with actions — such as sending an email, updating a property, or firing an event to another tool.


Everything you automate runs on live product data — who clicked what, which feature they used, and how recently. Because it’s built into PostHog, there’s no syncing or connectors to manage.


This direct link to behavioral data is[what customers say makes the biggest difference](https://posthog.com/customers/grantable)


:


> “PostHog Workflows just lives on top of the event data and the amazing user data you already have. The setup was incredibly easy.”
> — Evan Rallis, Head of Product & Growth at Grantable


Channel-wise you’re already live with **email** (and Slack, Twilio, etc. coming soon) – and you can tie in any real-time destination you have set up.


No fragile API scripts or custom backend logic required – just build visually, publish, and let it run. It's so simple even a marketer can build with it, so I decided to try it out.


##


What I built


As a product marketer working on this product, I immediately started thinking about how we could dogfood it, so I set up an automation that resulted in some unexpected outcomes.


Here’s what my very simple test campaign looked like:


###


Trigger


This is where you choose who gets to enter your automation. I chose a Pageview event – meaning everyone who comes to the URL that contains workflows will enter this flow.


The issue was that I hadn't limited how often someone could enter it. The default was once every 30 minutes. We added this capability after I had created this workflow, and totally missed to edit it.


So, if someone refreshed the page a few times, the workflow happily sent them another *“test test test.”*


###


Delay


I added a delay of four days before sending the email — a friendly reminder for users who’d visited the page recently.


You can add conditions or fallback actions here if something fails, but I didn’t (because what could possibly go wrong?).


###


Email


The email editor is a simple, visual builder for creating and testing automated emails triggered by user behavior.


You can personalize fields, add dynamic properties, and drag in elements like text, buttons, or images while previewing the result on desktop or mobile.


Here's the email I sent:


##


When it broke


On Monday morning after setting up the campaign, I was happy and caffeinated, walking to my coworking space, feeling productive and ready for the week.


I was genuinely excited to see how users would react to my campaign from the previous week. Maybe I'd get some quick replies or some feedback!


And… I got replies all right - but not quite the kind I expected.


So, yes, my workflow worked – but a little too well.


Luckily, only a few users experienced the mini spam storm, and most took it as a joke (thankfully reducing my panic level from “delete everything” to “mild existential dread”). Once I had stopped laughing/crying, I fixed it and learned something useful in the process.


**Lesson learned:** always double-check your entry conditions. And maybe… don’t test live workflows before your second coffee.


##


What we learned


This mistake actually turned into great feedback.


People loved how easy it was to build automated flows using their own PostHog data – sending messages, triggering logic, or running any action you can imagine.


This even helped me identify users who were real fans of Workflows. We invited them for interviews, got some use cases from them, and they provided some very useful feedback. *So, it turned out to be a win after all.*


Since it’s in alpha, coming to beta soon, it’s free for you to try now, and we’d love to have you[kick the tyres as we refine it](https://app.posthog.com/workflows)


.


Just maybe set your entry limits first.


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
