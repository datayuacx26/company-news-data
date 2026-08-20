---
schema_version: "1.0.0"
document_id: "393f4da008a85e3c1b5aacd806092db8b1e7a65995b7e1c195b7e650fc31ab70"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/product-tutorial-human-handoff"
published_at: "2024-02-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:71efa15ed849a043c689b7c7cc695490590f433ea0bf1d3e3e7af14e02b31725"
---

# Product tutorial: Human Handoff

## ‍What is Human Handoff?


Human Handoff is a feature that automatically detects when the conversation should be handed off to a human agent and then transfers it to your team. This may include situations beyond your AI Agent’s capabilities, such as questions outside the Knowledge Base’s scope or when a customer specifically requests to be connected with your customer support team.


After setting it up, it **doesn’t require any further action** on your part to recognize the intent to transfer a user to a human agent.


However, if you want certain keywords to **always** trigger Human Handoff, you can set them as triggers.


We’ll walk through every setting in the tutorial below.


If you prefer a **video version** and want to see it in action, head straight to YouTube:


## Example use case


**‍ ‍** Let’s use the example of Helpful Macintosh, an AI Agent from MacShop, an online Apple products reseller.


The owner of MacShop was going through conversations between Mac and her customers and she noticed that her users frequently ask about invoices, shipment status, and refunds.
‍
Since these queries require access to internal systems, which Helpful Macintosh doesn’t have access to, it actually can’t help users with their queries.
‍
Thus, the owner aims to integrate Helpful Macintosh more seamlessly into her existing customer service team. The goal is for it to act not just as a standalone Assistant but as a team member that can seamlessly hand off cases it can’t handle to human team members.
‍
So how can we actually achieve that with Quickchat AI?


Let’s move on to the step-by-step tutorial.


## How to set up Human Handoff


1. **Go to Settings and scroll down to the Human Handoff section**


1. **Enable Human Handoff**


The first thing that we need to do is to enable Human Handoff, which is going to enable us to alter all of the other settings.


1. **Select Working Hours and Days**


After we enable it, we can change the Working Hours. Let’s change them from 9 to 4.30 PM. We can change the time zone as well.


And next up, we can change the Working days. Let’s add Saturday.


We’re only going to have a day off on Sundays then!


1. **Write your Message outside Working Hours‍**
‍
Next, we can set a message for outside Working Hours, informing users when human agents are unavailable.


1. **Specify the Question you want to ask before the Handoff** ‍
‍
This question will be asked by your AI Agent to confirm whether a user should be handed off to a human agent.


1. **Specify the Confirmation message‍**
‍
Next, we specify the confirmation message to be sent after Human Handoff is triggered.


1. **Turn on AI Summary‍**
‍
If we turn it on, each conversation which ended in Human Handoff will be annotated with **a short note** summarising what happened in the conversation so far.


1. **Optional: Set trigger Keywords‍**


Here, you can input keywords that will **always** trigger Human Handoff.


Setting them is **not required** , as Human Handoff automatically detects the user’s intent and triggers the Handoff if necessary.


However, if you know for sure that there are certain keywords which should always trigger Handoff, you can specify them here.
‍
Let’s add *invoice* , *receipt* , and *delivery status* , because we know that these are the cases that our AI Agent doesn’t have permission to handle.


1. **Set up email notifications‍**


You can set up email notifications to always receive one when Human Handoff is triggered in one of your AI Agent’s conversations.


And that’s it!
‍
Human Handoff ensures that your customers always receive the best support possible, whether it’s an AI Agent or a human agent on the other side of the screen.


We take care of the heavy lifting, detecting when to trigger Human Handoff, and we provide an easy way to take over the conversation, so you’re always in control.
