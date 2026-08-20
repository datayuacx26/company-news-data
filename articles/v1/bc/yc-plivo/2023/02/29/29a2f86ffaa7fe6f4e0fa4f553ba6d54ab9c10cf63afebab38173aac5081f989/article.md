---
schema_version: "1.0.0"
document_id: "29a2f86ffaa7fe6f4e0fa4f553ba6d54ab9c10cf63afebab38173aac5081f989"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/voice-calls-stir-shaken/"
published_at: "2023-02-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:83b456714d627c375243da55d9aebbb34f6c90f36d40ddc5046bd3f9aadcd02c"
---

# STIR/SHAKEN: Everything You Need to Know About Trusted Calling

The number of unwanted and illegal robocalls in the US continues to rise. According to[YouMail](https://robocallindex.com/history/time) , Americans were hit by more than 50 billion robocalls in 2021, with about 40% of those calls thought to be fraud-related. And as annoying as these calls are for people who receive them, they’re even more detrimental for businesses that are trying to reach people with pertinent information. Many of these robocalls use caller ID spoofing to make recipients think they might know the caller. Caller ID spoofing hurts legitimate businesses by making call recipients less likely to pick up any calls.


*UC Today interviews Plivo’s Tony Graham to learn what’s shaking with STIR/SHAKEN.*


## Understanding STIR/SHAKEN Standards: Trusted Calling with Shake Stir


While historically, telephony was highly regulated, technical innovations such as computerized dialers and inexpensive IP-based calling on the public telephone network has turned robocalling into an everyday nuance. As a result, the US agency in charge of protecting consumers from communication scams, the Federal Communications Commission (FCC), directed carriers to implement robust call authentication by adopting STIR/SHAKEN standards targeting by June 30, 2021.


## What is STIR/SHAKEN?


STIR/SHAKEN are acronyms for the Secure Telephone Identity Revisited (STIR) and Signature-based Handling of Asserted Information Using toKENs (SHAKEN) standards. They’re technical frameworks that measure trust in the displayed caller name and number by authenticating the calling number.


Together they work in a way similar to attesting to the identity of the caller with a digital certificate. In the STIR/SHAKEN framework, a secure telephony identity (STI) governance authority issues digital certificates to carriers, or others who own or are assigned dedicated telephone numbers. The private key associated with a digital certificate is then used to sign a VoIP call, thereby indicating that the calling party number is who they claim to be.


## Levels of Attestation in STIR/SHAKEN: Shake Stir Caller Trust


Attestation provides the mechanism for carriers to communicate about a calling phone number’s legitimacy. A Secure Telephony Identity (STI) authentication service assigns an attestation level to a call that represents how confident a service provider is that the number’s owner is truly the one placing the call:


- **Full attestation (A)** - The service provider has authenticated its relationship with the customer making the call and the customer is authorized to use the calling number.
- **Partial attestation (B)** - The service provider has authenticated its relationship with the customer making the call, but cannot verify that the customer is authorized to use the calling number.
- **Gateway attestation (C)** - The service provider has authenticated that it has placed the call on its network, but has no relationship with the originator of the call (for example, a call received from an international gateway).


## How STIR/SHAKEN Notifications Enhance Trusted Calling?


When someone receives an authenticated call, they may be notified with a verification keyword or symbol on the incoming call display. If a call cannot be verified (attestation C or no attestation), it may be blocked or the consumer may be warned on their caller ID screen of a potential scam call. The purpose of notifications is to allow people who receive calls to decide whether they want to answer, ignore, or block a number.


If you’re a business, these changes should help you feel more empowered and increase the chances of your calls being answered by recipients.


We have more[details about STIR/SHAKEN in our documentation](https://www.plivo.com/docs/voice/concepts/stir-shaken/) .


## Implementing STIR/SHAKEN


### Without Plivo:


Businesses that implement STIR/SHAKEN themselves (typically within a private cloud environment) are held accountable with near-instant traceback by regulatory groups and law enforcement if STIR/SHAKEN is abused, including faking attestation levels.


### With Plivo:


If you’re a direct customer of Plivo’s, we can sign outgoing calls on your behalf and ensure calls get the right attestation so that call recipients feel confident in answering them. We can also validate the attestation levels on incoming calls received on the[Plivo Voice API](https://www.plivo.com/voice) platform, providing customers with the necessary information so that they and their end users can decide whether to answer the calls or not.


To ensure the right level of attestation,[Plivo customers](https://console.plivo.com/accounts/request-trial/) should submit to us their business information and the phone numbers they own and use as caller IDs so they can be verified. We’ll determine the appropriate level of attestation depending on the results of the verification and thus the level of confidence[Plivo](https://www.plivo.com/) has in the caller ID used on an outgoing call.


Download our free eBook on Best Practices for Voice Calling with the Plivo Voice API and elevate your communication strategy!


Plivo’s compliance operations team makes this process as seamless as possible for our customers. We believe that STIR/SHAKEN is crucial in preventing illegal or deceptive behavior like caller ID spoofing, and we’re excited to be part of the fight against unwanted robocalls.
