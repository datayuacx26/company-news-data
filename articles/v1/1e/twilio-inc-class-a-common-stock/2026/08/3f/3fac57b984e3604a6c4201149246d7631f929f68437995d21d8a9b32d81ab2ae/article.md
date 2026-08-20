---
schema_version: "1.0.0"
document_id: "3fac57b984e3604a6c4201149246d7631f929f68437995d21d8a9b32d81ab2ae"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/products/launches/branded-calling-general-availability"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T13:18:34.340146+00:00"
fetched_at: "2026-08-06T13:18:35.399283+00:00"
content_hash: "sha256:6ea48cfc829c114352ad73f72a367e77af65a8aa291356a6efbb0a82c6d1e0f7"
---

# The architecture of enterprise trust: Twilio US Branded Calling is now generally available

## The architecture of enterprise trust: Twilio US Branded Calling is now generally available


## The death of the unbranded call: Why 86% of your outbound budget is ignored


Spam calls have made it harder for businesses to reach customers in a meaningful way. Most people send unknown numbers to voicemail, so real conversations and important connections are missed. What used to be a simple phone call is now a gamble that leads to missed appointments, stalled sales, and wasted outreach.


That’s why today, we’re announcing that **Branded Calling is now generally available in the US.**


It comes as businesses face a growing challenge: customers increasingly do not trust unknown calls.[Hiya’s 2026 State of the Call report](http://hiya.com/state-of-the-call) found that 86% of consumers do not answer unknown phone numbers. 73% say businesses should identify themselves in caller ID, and 29% say seeing the verified business name is the top thing that would make them more likely to answer. At the same time, 34% say trust in phone calls has decreased, and 31% say they have received a deepfake voice call.


Branded Calling helps businesses make important outbound calls more recognizable by displaying a verified business name, logo, and, where supported, reason for calling directly on supported mobile devices. This GA release also makes Branded Calling easier to onboard, manage, and scale for the teams behind the calls.


## The outbound trust deficit is real


The voice channel still matters for high-value moments. But it is harder to earn attention when an incoming call looks unfamiliar.


That creates real operational friction for teams running appointment reminders, fraud alerts, delivery coordination, account servicing, and follow-up outreach. If a customer does not recognize the call, the workflow can stall before the conversation even starts.


It is not just a customer experience issue. It is also an operational one. Many teams still manage identity, onboarding, and rollout across fragmented systems, which adds extra work for operations, legal, and IT teams before a program ever goes live.


## The solution: recognizable calls, with a faster path to launch


Twilio Branded Calling is designed to help legitimate outbound calls look more recognizable at the moment the phone rings.


Instead of asking teams to stitch together a separate branded-calling workflow, Twilio connects branded identity to the same platform they already use to build, launch, and manage voice communications. That means businesses can improve how calls appear to recipients while also giving internal teams a more centralized way to onboard and operate the experience.


### How it works


Branded Calling lets eligible business identity information travel in the native voice path so supported carriers can verify and display it directly on supported mobile devices. In plain English: when a verified branded call reaches a supported device, the recipient can see who is calling with more context than a phone number alone.


For teams that want the technical detail: Branded Calling uses Rich Call Data in the STIR/SHAKEN signing flow so supported carrier paths can verify and render branded caller identity without requiring a third-party app..


### What's new in the GA release


This GA launch is about more than what appears on the call screen. It also gives teams a better operational experience when rolling out Branded Calling.


**With GA, customers get:**


-


A self-service Console experience for onboarding and setup


-


An onboarding API to automate number registration at scale


-


An improved logo upload process with easier asset handling


-


Trust Hub consolidation for more centralized business profile and trust workflows


-


Better operational control and supportability for teams managing Branded Calling in production


In short: the recipient gets a more recognizable call experience, and the business gets a simpler path to launch and manage it.


### Technical and data specifications to know


Branded Calling still depends on carrier and device requirements, so there are a few implementation details teams should know:


-


**Display Name** is capped at **35 characters**


-


**Call Reason** is capped at **64 characters**


-


Both fields support **ASCII characters only**


-


Logos must be uploaded as **32-bit BMP files at 256x256**


-


Logo validation includes a **base64-encoded SHA256 digest**


-


Console supports logo **crop and resize** during onboarding


-


**Self-hosted logo URL** support is available


-


**Call reason is not currently supported on iOS devices** due to an Apple limitation


Availability and rendering depend on supported carriers, devices, and configuration.


## Why it matters: Recognition can improve the chances a call gets answered


Recognition does more than improve appearance on the call screen. It can help legitimate outreach break through when timing matters most.


Hiya’s findings help explain why:


-


86% do not answer unknown calls


-


73% want businesses to identify themselves in caller ID


-


29% say verified business name is the top factor that would make them more likely to answer


Healthcare provider **CareSignal saw a 21% increase in call answer rates** after implementing Branded Calling and achieved a **48% pick-up rate** . Twilio Branded Calling can reach **more than 265 million mobile devices in the US** . And Forrester research found that branded calling helped calls get answered more frequently and reduced the number of calls mistaken for spam.


That does not guarantee every call gets answered. But it can give businesses a better chance to reach customers in the moments that matter.


## The future of outreach is programmatic trust


With[Twilio Trust Hub](https://www.twilio.com/en-us/trust-hub) , you centralize your brand identity and shift from fragmented, unverified channels to a unified, trusted communications layer. Your brand appears authentically and securely across voice, messaging, and email.


While this launch focuses on voice, the need for recognizable and consistent outreach spans every channel. Twilio’s Branded Communications vision is to help businesses establish trust everywhere without managing separate identity and onboarding processes for each channel.


Branded Calling is the starting point. It lets you present a verified identity for outbound calls today and lays the foundation for extending trust to messaging and email in the future. Enterprise trust is no longer optional, it is essential for customer engagement.


Branded Calling is now generally available in the US. Make every call count. Explore the[Branded Calling Blueprint](https://www.twilio.com/en-us/lp/measuring-the-business-impact-of-enhanced-branded-calling) ,[get started in the Twilio Console](https://www.twilio.com/console) or visit our[Branded Calling Docs](https://www.twilio.com/docs/voice/branded-calling/us-enhanced) to brand your first number.
