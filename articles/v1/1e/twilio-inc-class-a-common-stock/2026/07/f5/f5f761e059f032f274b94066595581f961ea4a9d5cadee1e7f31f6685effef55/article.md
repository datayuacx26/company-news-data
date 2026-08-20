---
schema_version: "1.0.0"
document_id: "f5f761e059f032f274b94066595581f961ea4a9d5cadee1e7f31f6685effef55"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/products/launches/lookup-line-status-public-beta"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:df7a4cafaf95c1345e6efd5698bed5a71ce1dc4629cdb69ea497a795414b9dd9"
---

# Announcing Twilio Lookup Line Status Public Beta: Verify Number Deliverability Before You Send

Time to read:


-
-
-
-
-


July 14, 2026


**Written by**[Catie Kolander](https://www.twilio.com/en-us/blog/authors/author.ckolander) Twilion


**Reviewed by**[Kelley Robinson](https://www.twilio.com/en-us/blog/authors/author.krobinson) Twilion


[Paul Kamp](https://www.twilio.com/en-us/blog/authors/author.pkamp) Twilion


---


## Announcing Twilio Lookup Line Status Public Beta: Verify Number Deliverability Before You Send


Ever start a One-Time Passcode (OTP) flow or a massive marketing SMS campaign, only to see your bounce rates jump and your budget disappear into the void?


Often, businesses didn't validate numbers, they audited failures. They paid for a message, waited for it to bounce, and then tasked engineering with parsing carrier codes like 30003 or 30005 to figure out why. By the time the business got that data, the capital was spent, carrier networks flagged their traffic, and the customer they were trying to authenticate had already walked away.


With[Twilio Lookup Line Status](https://www.twilio.com/docs/lookup/v2-api/line-status) now officially in Public Beta, it’s time to start putting real-time carrier insights to work. You can now tap directly into mobile operator networks, querying the Home Location Register (HLR) where available, to know instantly if a number is active, inactive, reachable, or unreachable before you ever hit send. Stop wasting resources on dead ends and start using Line Status today to protect your deliverability and optimize your messaging spend.


## Why Line Status matters for your business


By moving phone number validation from a post-delivery audit to a pre-send check, businesses can optimize their communication workflows:


- **Maximize campaign deliverability** : Ensure your marketing messages and authentication traffic are directed to valid and reachable recipients.
- **Reduce wasted spend** : Stop wasting budget on deactivated lines, invalid numbers, or fake sign-up attempts.
- **Protect your sender reputation** : Keep bounce rates low to maintain strong standings with carrier networks across all your outreach campaigns.
- **Streamline database hygiene** : Regularly scrub your contact lists or CRMs to eliminate stale, recycled, or deactivated customer data.


## Supercharge accuracy


We know that number intelligence is only as valuable as the data behind it. That's why we have revamped our underlying models for the US market.


By integrating advanced real-time data, we have boosted our US accuracy to 99.1% for both active and inactive phone numbers. This unparalleled level of precision minimizes false positives and negatives, giving your team data you can actually trust to fuel automated workflows.


## How it works


Integrating Line Status into your existing registration, onboarding, or messaging pre-flight check is incredibly straightforward. A simple` GET` request to the Lookup v2 API retrieves the live status of any phone number:


Copy code


```text
curl -X GET 'https://lookups.twilio.com/v2/PhoneNumbers/+447772000001?Fields=line_status' \ -u $TWILIO_ACCOUNT_SID:$TWILIO_AUTH_TOKEN
```


The API returns a clean, actionable JSON response indicating whether the phone number is currently reachable on the carrier network:


Copy code


```text
{
"calling_country_code": "44",
"country_code": "GB",
"phone_number": "+447772000001",
"national_format": "07772 000001",
"valid": true,
"validation_errors": null,
"caller_name": null,
"carrier": null,
"sim_swap": null,
"call_forwarding": null,
"line_status": {
"status": "reachable",
"error_code": null
},
"url": "https://lookups.twilio.com/v2/PhoneNumbers/+447772000001"
}
```


The possible statuses are:


-


Active


-


Reachable


-


Unreachable


-


Inactive


-


Unknown


Learn more about[each status in the documentation.](https://www.twilio.com/docs/lookup/v2-api/line-status)


## Choosing the right Lookup package


Twilio Lookup offers an array of specialized identity packages. Depending on your exact journey stage, you can leverage Line Status alongside our other packages to build a comprehensive fraud and delivery defense:


1. **Choose Line Status** when your priority is list hygiene, maximizing delivery rates, pruning deactivated numbers, or confirming a device is live right now.
2. **Choose Line Type Intelligence (LTI)** if you primarily need number routing metadata (distinguishing mobile vs. landline vs. VoIP) or want a low-latency, first-pass filter for channel gating.
3. **Choose Reassigned Number** if you are texting US consumers and require strict TCPA/regulatory compliance. Line Status checks network reachability, but it does not query the FCC Reassigned Numbers Database (RND). You must use a Reassigned Number with a last-consent date for legal compliance safety.


## Availability and pricing


Lookup Line Status supports phone numbers worldwide. Because the package triggers an authoritative, live query straight to mobile operator networks, you can expect an average lookup response latency of 1.2 seconds, with a P99 latency of 4.1 seconds.


[Pricing](https://www.twilio.com/en-us/user-authentication-identity/pricing/lookup) starts at a flat rate per call, with volume-tiered discounting built into the API to automatically lower your costs as your operations scale.


## Get started today


Stop texting and calling into the dark. Clean up your databases, secure your user onboarding, and optimize your delivery spend with authoritative, live carrier signals.


Explore the[Twilio Lookup Line Status API Documentation](https://www.twilio.com/docs/lookup/v2-api/line-status) to start building with the Public Beta today!


*Catie is a Principal Product Marketing Manager at Twilio where she works with the User Authentication & Identity Team. Catie has worked with a variety of SaaS and tech companies across telecommunications and identity intelligence industries.*
