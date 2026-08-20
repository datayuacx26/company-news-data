---
schema_version: "1.0.0"
document_id: "8646f12b3c69c47ec84bc60ae488b56076c3d419ce9de12aa4d2223ba36dcccc"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/products/whatsapp-just-changed-how-customers-identify-themselves"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T13:57:06.755216+00:00"
fetched_at: "2026-07-30T13:57:07.788964+00:00"
content_hash: "sha256:a091c86bea201ebcaee20506f1bf1b81ba03683445ed67a3841127c167c819b7"
---

# WhatsApp just changed how customers identify themselves. Here's what to do about it

## WhatsApp just changed how customers identify themselves. Here's what to do about it


For nearly two decades, the phone number has been the “key” for customer conversations. It's how your CRM identifies who's messaging, how your bots route, and how your campaigns connect a click to a person. And now WhatsApp is telling its 3 billion users they don't have to share it anymore.


New generations are already using other identifiers that replace the phone number in other ecosystems. Platforms are shifting toward identifiers that users control, and away from ones businesses have always assumed would be there. The phone number's grip on trusted communication is loosening, and WhatsApp Usernames is a clear signal businesses need to adapt.


## What's technically changing


When users adopt a username, businesses receive a Business-Scoped User ID (BSUID),a stable, privacy-first identifier Meta provides in place of the phone number for new conversations. Usernames are optional, existing conversations retain the phone number, and authentication messages still use phone numbers. But for net-new inbound conversations from username-enabled users, businesses that haven't adopted BSUID will have no way to identify or retain continuity with that customer.


## The impact to businesses


Users have been expecting more control on who has access to their private information such as the phone number. A platform where users feel safer is one where they will engage more and start more conversations. That can only be good for business.


The thing is, if businesses aren't prepared for it, those new relationships can break.


Imagine a customer messages you for the first time, and no phone number arrives. Your system can't match them to anything. They come back a month later with a question and get treated like a stranger. This creates two conversations with zero continuity.


Moving forward, businesses need to work from the basis that no phone number will show when establishing new user relationships. With no phone numbers some examples are:


- CRM matching: customers become unknown
- Bot routing: lose signal that determines context, language and priority
- Campaign attribution: ads attribution go dark as they cannot match users
- Service continuity: returning users are treated as new.


Businesses need to update their systems to work with a new identifier (BSUID) so that user records can be leveraged appropriately


## Twilio is built for this


As Meta's platform evolves, Twilio handles those changes so your team doesn't have to. We absorb the complexity so you can keep your systems running without a rebuild and your customer data connected even when one channel rewrites its rules.


For Usernames specifically, here's how that plays out:


- BSUID surfaced automatically in webhook payloads, no custom parsing required
- Cross-channel continuity preserved: because Twilio runs WhatsApp alongside SMS, RCS, Apple Messages for Business, Voice, and Email on one platform, identity resolution doesn't break when one channel changes its rules
- Migration guides for CRM, bot, automation, and Click-to-WhatsApp workflows
- Console updates so username-based conversations appear alongside phone-number-based ones


Usernames is just the latest update, more will follow. Twilio stays ahead of those changes so a platform shift never becomes a breaking change.


## Build for it now


The phone number as the universal customer identifier is changing across platforms. Usernames is just the latest update, and more will follow. If brands want to avoid patching updates, they need to start building now so that they can scale successfully. Learn more about[WhatsApp Usernames here](https://www.twilio.com/docs/whatsapp/key-concepts#whatsapp-usernames) .
