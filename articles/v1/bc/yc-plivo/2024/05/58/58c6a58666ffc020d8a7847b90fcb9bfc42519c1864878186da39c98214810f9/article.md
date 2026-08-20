---
schema_version: "1.0.0"
document_id: "58c6a58666ffc020d8a7847b90fcb9bfc42519c1864878186da39c98214810f9"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/verified-caller-id/"
published_at: "2024-05-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:db3c03ec7f41f2491ec8e0008bdfc9feda507d87a5cf24bc497f32ca9071ffaf"
---

# Verified Caller IDs: Trusted, Secure Outbound Calling with Plivo

Call spoofing is a growing challenge for businesses worldwide. This type of fraud involves the perpetrator using technology to make it look like they’re calling their target from a local number, a well-known company, a government agency, or even a real contact. The scammer then tricks their target into answering their calls and steals their personal information or money.


Call spoofing is a global problem: it’s estimated that Americans alone[lost an estimated $87 billion](https://www.prnewswire.com/news-releases/robokiller-releases-2022-mid-year-phone-scam-report-301620493.html) to phone scams last year. It’s also damaging to legitimate businesses who often suffer reputational and financial losses when a scammer impersonates their brand.


Plivo’s introduction of the Verified Caller ID feature is designed to tackle security challenges by bolstering the platform’s security measures. The Verified Caller ID feature plays a pivotal role in enhancing the authenticity of outbound calls made within Plivo’s ecosystem. By authenticating and verifying caller numbers, businesses can establish a more credible presence through Plivo. This feature is especially beneficial for resellers, adding an extra layer of authentication that strengthens security throughout the communication landscape.


## How do Verified Caller IDs work?


A spoofed caller ID is when someone alters the information displayed on their caller ID to make it look like they’re calling from a different number or even a different name than they are. It’s like wearing a disguise for their phone number.


With Verified Caller ID, Plivo ensures that calls are placed using authorized numbers and caller IDs, thereby preventing any calls originating from unknown caller IDs. Notably, if the number is rented or ported to Plivo, caller ID verification is bypassed as Plivo already authenticates these numbers. However, this verification step becomes essential only if a customer wishes to use their own non-Plivo number for making outbound calls. Here’s how the verification process works at a high level.


1. Add Number: Include a number you intend to utilize as an outbound caller ID within the verified caller ID screen on the Plivo Console or through API integration.
2. Authentication: Plivo will send a one-time passcode using SMS or voice.
3. Confirmation: Enter the code to prove you have control of the number.
4. Verification: Once your phone number is verified, you can use it as the caller ID when you make calls


Plivo makes it easy to verify your number. You can verify your numbers with Plivo using the Plivo console, APIs, and SDKs.


## Get started with Plivo Verified Caller ID


Plivo’s Verified Caller ID is built to seamlessly integrate into your existing workflow. We make it easy to verify your numbers quickly and reliably using the console, REST APIs, and SDKs. Learn more about the verification process in our[FAQs](https://support.plivo.com/hc/en-us/sections/30514576504089-Verified-Caller-ID?__hstc=44907643.07d767de4f3641363e5c35423841beff.1717226014872.1720505076251.1720511860769.130&__hssc=44907643.4.1720511860769&__hsfp=137727427) ,[guide](https://www.plivo.com/docs/voice/concepts/verified-caller-id/) and[API reference](https://www.plivo.com/docs/voice/api/verified-caller-id/) .
