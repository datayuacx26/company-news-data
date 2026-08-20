---
schema_version: "1.0.0"
document_id: "f2f0c2d58e3e908098fe7b903a04b41e380f5d41aa3c3e46bd356640083d368b"
company_key: "yc-vapi"
company: "Vapi"
source_id: "yc-vapi-news-import-8dd0a49247bf"
canonical_url: "https://vapi.ai/blog/composer-webinar-faq"
published_at: "2026-03-20T00:00:00+00:00"
first_seen_at: "2026-07-24T05:51:02.715878+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:1e171afc609e11ff57a2df4f0d3957f632770d09b67f4455f4fca575661c2137"
---

# Composer Webinar: Your Most-Asked Questions, Answered - Vapi AI Blog

## **The webinar demo**


We recently hosted a live webinar where Nick Robin, a Principal Product Manager at Vapi, demoed Composer in real time.


Nick built a fully functional inbound customer service agent for a fictional small business: Nick's Sunglass Shack, a California-inspired sunglasses brand based in Redwood City that makes small-batch eyewear. The agent needed to handle customer inquiries, answer product FAQs, provide store information, book appointments, and sound like it actually belonged to the brand. From start to finish, backed by Composer, Nick had a voice agent in production within a 30-minute demo.


He started with a single sentence:


"Please help me create an assistant for my business, Nick's Sunglass Shack. Your sunglasses outfitter, based in Redwood City, we make sunglasses in small batches, heavily inspired by California culture."


Composer asked for more context—specifically, store hours and FAQs. After you pasted an FAQ export from the website into Composer, it built the assistant: writing the system prompt, setting a welcome message, provisioning a phone number and scheduling. When the assistant's name was too long, Composer flagged the issue, explained why, and requested a shorter name.


Self-correction occurs throughout the process. Tools may fail, but Composer identifies issues, explains them, and proposes fixes. You approve or deny, keeping the feedback loop tight.


## **What Composer is**


Composer sits inside the Vapi dashboard as an in-product assistant. It helps you build, debug, and analyze voice agents without code, using plain-text prompts. It has direct access to the Vapi account context and can create assistants, add tools, update prompts, provision numbers, and analyze call data—all without leaving the dashboard.


It is currently in alpha, and the team is releasing improvements based on how builders use it.


---


The session generated many questions from attendees. Below are the ones that came up most, answered directly.


---


## **Does it work for building both inbound and outbound assistants?**


Yes. Composer can help build any assistant supported in Vapi. For inbound, it can help you build agents that handle customer support lines, appointment booking, FAQ answering, and live agent escalation. For outbound, it can set up agents for sales calls, lead qualification, appointment reminders, and campaign dialing.


The workflow is the same either way: describe your use case, answer a few clarifying questions, and Composer scaffolds the full agent. The distinction between inbound and outbound mostly shows up in how you configure call handling and what tools the agent needs, Composer handles that configuration once you tell it what you're building.


## **Can I connect it to my CRM, calendar, or external data?**


This is where Composer does a lot of the heavy lifting. For CRM integrations, tell Composer which CRM you're using and what data you need to capture, and it will generate the integration code for you.


Composer sets up scheduling, rescheduling, cancellations, and availability lookups. You can connect multiple calendars if needed.


Composer can conduct web and documentation searches to research a business and automatically build agent knowledge. For example: 'Research \[company name\] at \[URL\] and build a customer service agent.' Composer searches, summarizes, and configures the agent.


## **What about knowledge bases and FAQs?**


For smaller or stable knowledge sets, such as FAQs, scripts, and product info, the simplest approach is pasting the content directly into the system prompt. That is what Nick did in the demo for his company's FAQ. It is fast, and it works well for content that does not change often.


For frequently updated data, use the API Request or Code tools to fetch information in real time during a call. Composer builds these tools for you once you specify the needed data and its location.


## **Can I use Composer to edit agents I've already built?**


Yes. Give Composer an assistant ID, and it loads the configuration so you can make changes through conversation. This works for agents built via the API, the dashboard, or Composer.


Be specific with instructions. Asking to 'improve the prompt' may cause a full rewrite, which is not intended. Instead, state exactly what to change and where. Example: 'Load assistant ID \[abc123\] and change only the first sentence of the intro. Show me the change before applying.'


## **How do I handle call routing and human handoff?**


Both are natively supported. To transfer to a human agent, ask Composer to add the transfer call tool and provide the destination number. For IVR (Interactive Voice Response) menus, press 1 for sales and press 2 for support. Vapi supports DTMF (Dual-Tone Multi-Frequency) detection, and Composer configures routing logic based on your menu structure.


For complex routing where an agent hands off to a specialist, Composer helps you build Squads: multiple assistants with defined handoff conditions.


## **What about voice quality and multilingual support?**


On voice quality, the key lever is trying different voice models. Providers vary in expressiveness. What works for support may not fit sales. Ask Composer to recommend and configure voices for your agent.


Shape tone through the system prompt. Include pacing instructions—'speak with varied pacing, use brief pauses before key points' to improve delivery without changing the model.


Vapi assistants support most major languages: French, German, Spanish, Hindi, Arabic, Dutch, and more. Configure by asking Composer in your preferred language or prompting it directly, such as: "Make this agent respond in French." Composer automatically sets up STT and TTS providers. For bilingual agents, set the caller language detection and response language.


## **How do I choose the right LLM and voice model?**


Composer can surface recommendations based on your use case. The more context you give it, such as the type of call (for example, sales or support), the persona (the role or profile of the caller), the expected call length, and any compliance requirements (regulations or rules the call must follow), the better its suggestions will be. Start with Composer's recommendations, run calls, and refine from there. Model selection is often an iterative process once you have real call data.


## **Does Composer have an added cost?**


During Alpha phase, Composer is included at no extra cost. Pricing is still under review and you can access it directly from the Vapi dashboard.


## **How do I give feedback?**


Composer's built-in tool activates if you try to do something Vapi doesn't support, it prompts you to send feedback directly. You can also thumbs-down any Composer response to submit it to the product team.


---


The 28-minute demo was a deliberate stress test, resulting in a working agent with a provisioned number, a system prompt, a welcome message, and a connected knowledge base, all without coding.


That's the standard Composer aims to meet. Try it yourself, the dashboard is the starting point.
