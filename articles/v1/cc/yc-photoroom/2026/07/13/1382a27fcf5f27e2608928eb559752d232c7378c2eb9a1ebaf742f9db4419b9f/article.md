---
schema_version: "1.0.0"
document_id: "1382a27fcf5f27e2608928eb559752d232c7378c2eb9a1ebaf742f9db4419b9f"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/amplitude-event-governance-slack-bot-and-ai"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:0ae8861128cc2df2011750b8b7114eaaf6e0ab09fbeb0ebb52258606dafc9b34"
---

# How we automated Amplitude event governance with a Slack bot and AI

At Photoroom, we introduced a strict Amplitude event naming convention in January 2026. The idea was simple: consistent event names mean better data quality, better onboarding, and a tracking plan that people can actually trust. In practice, enforcing that convention turned out to be harder than expected.


This article is the story of how we went from a manual, error-prone review process to a fully automated Slack bot that validates event names, enforces naming conventions, and creates Amplitude Data branches on behalf of the requester, using browser automation powered by an AI agent.


## **1. The problem: conventions don’t enforce themselves**


For a long time, Amplitude event creation at Photoroom had no real structure. Names were inconsistent, there was no agreed-upon convention, and events would appear in our tracking plan without anyone in the data team knowing. We would discover them through an Omni dashboard we built for observability, which pulls all Amplitude events daily via the API. Observability is great, but we would catch issues after the fact: an event created without our knowledge, a name that did not follow any pattern, a property with no description.


The first step was to **create a convention** . We aligned with marketing and content design to define a fixed list of approved objects, settled on Proper Case throughout, and decided that all action verbs should be in past tense. Events would follow the format` Object: Action` , with an optional source prefix (` \[Backend\]` or` \[Engine\]` ) depending on who sends the event. This is not cosmetic. Knowing who sends an event is critical for debugging: if something breaks, you immediately know which team owns it and where to look. It also makes ownership explicit in the tracking plan itself, so there is no ambiguity about whether a given event is an app, backend, or engine concern.


But a convention without a process is just a document. At the time, there was no formal way to create events. Sometimes a PM would tag us in a spec Notion page, sometimes not. So I created a first process: a Notion form using the fixed list of objects, which would send a notification to a dedicated Slack channel. The data team would then review the proposed name in the thread, approve or request changes, and the requester would create the event once validated.


It was better, but still not enough. People would sometimes implement events differently from what had been agreed. Descriptions would be missing. There was no enforcement mechanism, only trust. On top of that, reviews depended on a human being available; and between time zones, competing priorities, and review fatigue, feedback could take days. We would still find issues in our Omni dashboard after the fact.


The situation became very concrete during one of **Photoroom’s twice-yearly engineering cleaning week.** I spent an entire week renaming non-compliant events using[Amplitude’s CSV bulk import feature](https://amplitude.com/docs/data/csv-import-export) , which is not the most stable tool. Working through events and properties one by one, at scale, is tedious and error-prone. That week made one thing very clear: **prevention is infinitely better than correction.**


## **2. Thinking about the right solution**


Before building anything, we asked: what is the smallest intervention that would actually change the behavior?


The ideal solution had to be:


-


synchronous: validation needs to happen in real time, not hours later


-


frictionless: if it adds too much overhead, people will bypass it


-


integrated: it had to live where engineers and PMs already work


-


automated: the data team spending time on mechanical checks is not a good use of anyone’s time


Slack was the obvious answer. Everyone uses it, threads map naturally to individual event reviews, and[Slack’s Block Kit API](https://docs.slack.dev/block-kit/) supports interactive buttons which make the flow smooth.


For the validation logic, we had everything we needed: a Notion database of approved objects, a clear set of rules, and Python to wire it together.


The harder part was branch creation. Amplitude does not expose branch creation through their REST API or MCP. (Edit June 2026: their MCP does now allow branch creation). This meant any automation would require browser automation or convincing Amplitude to add an endpoint (which we also asked for,). We chose browser automation via[Twill](https://twill.ai/photoroom-data-team/home) , an AI agent built on Claude, which already had access to our codebase and could execute browser tasks. The key insight was that if we gave it the right instructions (in the form of a skill file) it could navigate the Amplitude UI reliably enough for our needs.


The goal was not to add gates or create more friction. It was to build **a golden path** : a process so smooth and integrated into existing workflows that following it becomes the default, not the exception. If validating an event name takes less time than bypassing the process, people will validate.


## **3. The architecture**


The bot is a single Python Cloud Function deployed on Google Cloud Run, triggered by Slack events. Here is the full stack:


-


Google Cloud Functions: serverless HTTP endpoint that receives Slack events and button interactions.


-


Slack Block Kit: interactive buttons for the confirmation flow.


-


Notion API: paginated query against our official objects database, cached in memory per instance.


-


Twill + Claude: AI agent that receives a task via API and creates the Amplitude branch through browser automation.


-


Google Secret Manager: stores all credentials (` SLACK_BOT_TOKEN` ,` NOTION_API_TOKEN` ,` TWILL_API_KEY` ,` AMPLITUDE_COOKIES` ).The same` AMPLITUDE_COOKIES` and` TWILL_API_KEY` are also configured directly in Twill. Note that the Amplitude session cookie must be refreshed manually in both places whenever it expires; a known limitation of browser automation.


-


Cloud Build: continuous deployment triggered on every push to` main` .


Why Slack interactive buttons? The bot needs to remember your event name.


The validation flow spans multiple messages: the user submits an event name, the bot replies with buttons, the user clicks one, the bot acts on it. The problem is that each interaction is a separate request, and the bot has no memory between them.


Storing that context in a database would work, but it would mean extra infrastructure to build and maintain. Instead, we encode the event name directly in the button’s` value` field when we send it. When the user clicks, Slack sends that value back automatically. The button itself carries the context, no backend needed.


## **4. The validation flow**


When someone mentions` @amplitude-events-helper` with an event name, the bot runs seven checks in sequence:


1.


Prefix Proper Case:` \[Backend\]` not` \[backend\]` .


2.


Body Proper Case: every word capitalized, acronyms preserved.


3.


Single colon: exactly one` :` separating object and action.


4.


Object check: the object part must exist in our Notion database.


5.


Action presence: something must come after the` :` .


6.


Past tense: the last word of the action should end in` ed` , with a whitelist of irregular past verbs (` sent` ,` shown` ,` hit` ,` built` , etc.) for legitimate exceptions.


7.


Source confirmation: a soft check asking the user to confirm whether the event comes from the apps, backend, or engine.


Checks 1 through 5 are hard blocks. Check 6 is soft: the bot asks for confirmation rather than blocking, to avoid false positives on irregular verbs. For the Notion object check, we paginate through the full database on first load (Notion returns a maximum of 100 results per page) and cache the result in memory. An` objects refresh` command lets anyone invalidate the cache on demand without redeployment.


## **5. The branch creation challenge**


Branch creation was the most technically interesting part of the project.


Since there is no API for it, we used Twill to open the Amplitude Data UI via browser automation, authenticate using session cookies, and click through the branch creation flow. The challenge was getting Twill to do this reliably. The Amplitude UI has a few quirks that made naive automation fail:


-


Cookie authentication: the` -cookie` flag on the browser agent did not persist cookies properly. We had to set them via` document.cookie` in JavaScript after the first navigation, then reload.


-


Custom React input: when Twill scans the Amplitude branch creation dialog to find interactive elements, it returns empty — the input field is wrapped in a custom React component that browser automation tools struggle to detect. The workaround is to locate it directly via JavaScript (` document.querySelectorAll('input')\[2\]` ) and simulate typing with a` dispatchEvent` so React registers the change.


This is fragile: if Amplitude restructures their dialog or updates their React implementation, the selector will silently break. A known tradeoff we accept.


-


Two “Create” buttons: the dialog’s Create button and the navbar’s Create button share the same text. Clicking generically triggered the wrong one. The fix was to target the button by its bounding box position (` y > 300` ) using` dispatchEvent` instead of a generic` click()` .


Each of these lessons is now documented directly in the skill file that guides Twill’s behavior, so future runs benefit from prior debugging without starting from scratch.


## **6. What we learned**


-


Statelessness matters from the start. We initially used in-memory dictionaries to track the state of each conversation thread. This broke silently when Cloud Run routed requests to different instances. Encoding state in button values was the right fix: explicit, reliable, and requiring no external storage.


-


Soft checks are better than hard blocks for ambiguous rules. The past tense check initially blocked events with irregular verbs. Replacing the block with a confirmation button preserved the guardrail without adding friction for legitimate edge cases.


-


Browser automation is fragile, but documentable. Every quirk Twill encountered became a line in the skill file. Over time, the skill accumulates institutional knowledge that makes each run faster and more reliable than the last.


-


Notion pagination is easy to miss. Our object database has more than 100 entries. The first version of the Notion fetch only retrieved the first page, silently missing objects that came later alphabetically. Adding pagination fixed it completely.


## **Conclusion**


Bad event names are a data quality problem. But also a knowledge one. And a trust one.


This bot does not solve everything. Branch creation still takes 3 to 4 minutes. Session cookies expire and need manual refresh. The branch review step is still human. But it moves validation to the right moment - before the event is instrumented - and removes most of the mechanical back-and-forth from the data team’s plate.


The most satisfying part of this project was that it was built almost entirely with tools we already had: Python, Slack, Notion, Google Cloud, and an AI agent that knows how to navigate a UI. No new vendor, no complex infrastructure: just the right wiring between the right tools.


Thanks for reading, you can find other articles from the Photoroom data team[here](https://www.photoroom.com/inside-photoroom/category/category-4) !
