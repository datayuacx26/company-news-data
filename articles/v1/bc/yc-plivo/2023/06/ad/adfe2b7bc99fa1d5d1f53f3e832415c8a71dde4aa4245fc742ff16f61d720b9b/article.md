---
schema_version: "1.0.0"
document_id: "adfe2b7bc99fa1d5d1f53f3e832415c8a71dde4aa4245fc742ff16f61d720b9b"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/number-lookup-intro/"
published_at: "2023-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:51.132542+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:e3b0ad9e0c3b3bf4a54a006bb7517537a59965feb915d9625fb6c21303b6b40f"
---

# How to Get the Most Out of Number Lookup

If you’re planning a high-volume marketing campaign, you can save money by ensuring that the numbers you’re trying to reach are active and able to receive text messages. Number lookup can uncover deactivated numbers, landline numbers, and mobile numbers that can’t receive SMS.


Plivo[introduced](http://plivo-webflow.webflow.io/blog/lookup-phone-number-validation) the[Lookup API](https://plivo-webflow.webflow.io/lookup-api) in 2020 to allow customers to programmatically determine information for any phone number worldwide. The Lookup API can also properly format numbers for use with our other APIs; improperly formatted numbers can cause calls and messages to fail. Number lookups also make economic sense in many regions, where the cost of a number lookup is less than the cost of sending a text.


## What is number lookup?


You might be familiar with number lookup in the context of a reverse directory lookup, where, instead of finding a phone number for a name, you use the phone number to find out details about the person or business using it.


In the context of[Plivo](https://www.plivo.com/) APIs, the Lookup API returns the phone number country, the number format, its carrier, and its type: fixed (landline), mobile, VoIP, or toll-free. Type is especially useful, as most landline numbers and many VoIP and toll-free numbers can’t accept SMS messages. CPaaS platforms charge a fraction of a cent for every message sent, whether a number can accept SMS or not, so it’s worthwhile to eliminate invalid numbers from your target list.


For mobile numbers, the Lookup API relies on the Home Location Register, a global database that contains information about authorized subscribers to GSM (Global System for Mobile communication) networks. Fields returned include the mobile country code (MCC), mobile network code (MNC), and carrier name. These values map to fields of a unique ID for all mobiles phones — an International Mobile Subscriber Identity (IMSI), which is associated with the SIM in the phone. The first three digits of the IMSI represent the MCC. The next two or three numbers represent the MNC. The remaining digits represent the Mobile Subscription Identification Number (MSIN) within the mobile operator’s customer base, a unique number that the carrier uses to identify a user’s mobile phone number.


Though it sounds as if the MSIN should be a subscriber’s mobile phone number, it’s not necessarily. When a subscriber changes their phone number, for example, they keep their original MSIN.


## Lookup use cases


Number lookup can keep your business from wasting time and money sending text messages or making voice calls to invalid numbers or ones that have no value for your organization.


- **Marketing** — By looking up numbers and purging invalid ones from your database, you can save money by not contacting invalid numbers, and get higher response rates by eliminating the bad numbers before you reach out.
- **Shipping and delivery** — Number lookups confirm the validity of a customer’s contact phone, and while it can’t confirm a delivery address, it can serve as a first line of defense against fraud.
- **Security** — Many organizations use SMS to send one-time passwords to improve login security through two-factor authentication (2FA). It’s prudent to make sure that the numbers customers provide for this critical step are both valid and reachable. A similar argument applies to numbers supplied for logins to mobile apps.


## Lookup on the Plivo console


While most people[access it via code](https://www.plivo.com/docs/lookup#overview) as part of their applications, you can do one-off lookups from the[console](https://console.plivo.com/lookup/dashboard/) . In the spirit of the console tours we did for[SMS](http://plivo-webflow.webflow.io/blog/console-tour-sms) and[voice](http://plivo-webflow.webflow.io/blog/console-tour-voice) , here’s a similar look at Lookup.


The Lookup Overview page is the fourth icon on the Plivo console menu. It displays a single screen.


A usage summary section tells you how many API requests you made in the last 7, 30, and 90 days.


Under that, another box lets you enter a phone number. Once you click **Submit** , the API retrieves information about the number, including its country, the number in various formats, and the carrier and number type, and displays it. Each lookup, whether executed in code or submitted via the console, costs 0.4 cents (US).


That’s all there is to it; we try to keep things simple around here.


## Get started with Lookup


To get started with Plivo’s Lookup API, check out our[documentation](https://www.plivo.com/docs/lookup#overview) or read the[FAQ page](https://support.plivo.com/hc/en-us/articles/360049279691-FAQ-Number-Lookup-API) in our support portal.
