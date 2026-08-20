---
schema_version: "1.0.0"
document_id: "827529f8c4bf465798c65fa1ecb1dcea98427b986d173b93448d9e49856e5f5f"
company_key: "yc-bird"
company: "Bird"
source_id: "yc-bird-news-import-1ae5a03d7866"
canonical_url: "https://bird.com/en-us/blog/what-does-sms-mean"
published_at: "2026-05-24T00:00:00+00:00"
first_seen_at: "2026-07-21T10:23:31.194838+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:d1be1f15c00c2bf9ee817ca40699d6ad49ae72ca53504910b728fc10c0111b9a"
---

# What Does SMS Mean?

SMS stands for Short Message Service: the standard for sending short text messages between phones over carrier networks. A single SMS is limited to 160 characters of GSM-7 encoded text, it is addressed by phone number, and it is delivered through the mobile network rather than the internet. It is the most universal messaging channel there is, supported by essentially every phone in use.


That universality is the whole point. SMS has no app to install, no account to create, and almost no compatibility surface to worry about. The trade-off is that it is deliberately minimal.


## Why 160 characters?


The 160-character figure is not arbitrary. SMS messages travel in a small fixed-size payload, and with the default GSM-7 character set (the standard Latin alphabet plus common punctuation) that payload holds 160 characters. Go one character over and the message is split into segments and reassembled on the receiving phone, a process called concatenation. Each concatenated segment carries a small header, which is why segments after the first hold about 153 characters rather than 160.


Encoding changes the math. If your text includes characters outside GSM-7, for example emoji or many non-Latin scripts, the message switches to UCS-2 (Unicode), and the per-segment limit drops to 70 characters (about 67 per segment once concatenated). A single stray character can quietly turn a one-segment message into a multi-segment one, which matters because carriers bill per segment. It is worth checking the encoding of your templates before you send at volume.


## How is SMS different from MMS, RCS, and OTT apps?


SMS sits at one end of a spectrum that runs from minimal-and-universal to rich-but-fragmented.


Reach Media Transport


SMS Universal Text only Carrier network


MMS Near-universal Images, audio, video Carrier network


RCS Wide, growing Rich media, buttons, receipts IP, falls back to SMS


OTT apps (WhatsApp, etc.) App users only Rich media Internet


MMS (Multimedia Messaging Service) is the older way to attach media to a text; it uses the carrier network like SMS but carries pictures and video. RCS is the modern successor that adds read receipts, typing indicators, and verified business branding while keeping phone-number addressing; the longer version is in[what is RCS messaging](https://bird.com/en-us/blog/what-is-rcs-messaging) . Over-the-top (OTT) apps such as WhatsApp run on the internet and reach only people who have the app installed, trading universal reach for richer features and their own account system.


## How does business SMS (A2P) work?


When a person texts a friend, that is person-to-person (P2P) messaging. When an application sends a verification code or a shipping update, that is application-to-person (A2P) messaging, and it works differently. A2P traffic goes through a messaging provider that connects to carriers on your behalf, using a sender identity (a long code, a short code, a toll-free number, or in some countries an alphanumeric sender ID) and routing rules that carriers apply to bulk and automated traffic.


The practical implication is that A2P messaging is governed more tightly than the texts you send from your own phone. Carriers care about who is sending, with consent, and at what volume, and they enforce it.


## What is SMS used for?


Three patterns cover most A2P sending:


- **One-time passcodes and 2FA.** SMS is the most common second factor because nearly everyone can receive a text. If you are building this, see[what does OTP mean](https://bird.com/en-us/blog/what-does-otp-mean) for how one-time codes work, and[Bird Verifications](https://bird.com/en-us/products/verifications) for the delivery side.
- **Transactional alerts.** Order confirmations, shipping updates, appointment reminders, and account notifications. People read these because they are short, timely, and expected.
- **Marketing.** Promotions and campaigns sent to recipients who have opted in. This is the most regulated use and the easiest to get wrong.


## What about deliverability and compliance?


SMS deliverability is less about content filtering (as in email) and more about following the rules carriers set for A2P traffic. The basics worth knowing:


- **Opt-in.** Sending to people who agreed to hear from you is both a legal requirement in many places and the single biggest factor in whether your traffic keeps flowing. Honour opt-outs (commonly STOP) immediately.
- **Sender identity.** The number or sender ID you send from affects throughput, trust, and where you can send. Different identities suit different volumes and regions.
- **Local registration regimes.** Several markets require registering your business and your traffic before you can send A2P at scale. In the United States, application-to-person traffic over standard local numbers runs through a registration framework known as 10DLC, where you register your brand and your campaigns so carriers can apply the right throughput and filtering. The specifics vary, so treat registration as a step to plan for rather than an afterthought.


None of this is exotic, but it is the difference between messages that arrive and messages that get filtered.


## FAQ


### Does SMS support links and images?


SMS supports links as plain text (the recipient's phone makes them tappable). It does not carry images; for that you need MMS, RCS, or an OTT app. Bear in mind that some carriers scrutinise links in A2P traffic, so use a consistent, recognisable domain.


### How long can an SMS actually be?


One segment is 160 GSM-7 characters, or 70 if the message uses Unicode characters. Longer messages are split into segments and reassembled on the recipient's phone, and you are typically billed per segment, so length affects cost.


### Is SMS secure enough for one-time codes?


SMS codes are widely used and far better than no second factor, but SMS is not encrypted end to end and codes can be intercepted or phished. For higher-assurance flows, treat SMS as one channel among several and consider stronger factors where the risk warrants it.


## Where Bird fits


Bird sends[SMS](https://bird.com/en-us/products/sms) through carrier connections worldwide, handles the sender-identity and routing details, and exposes the same surface for richer[RCS](https://bird.com/en-us/products/rcs) where it is supported. For verification flows specifically,[Bird Verifications](https://bird.com/en-us/products/verifications) manages code generation, delivery, and channel selection so you are not wiring that logic by hand.
