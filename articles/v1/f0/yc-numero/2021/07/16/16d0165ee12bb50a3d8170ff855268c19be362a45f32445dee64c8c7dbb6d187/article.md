---
schema_version: "1.0.0"
document_id: "16d0165ee12bb50a3d8170ff855268c19be362a45f32445dee64c8c7dbb6d187"
company_key: "yc-numero"
company: "Numero"
source_id: "yc-numero-rss-2718f20b56fd"
canonical_url: "https://www.numero.ai/blog/how-to-leverage-tech-to-verify-phone-numbers-and-supercharge-your-connect-rate/"
published_at: "2021-07-01T04:00:00+00:00"
first_seen_at: "2026-07-25T16:42:35.962761+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:2b47144f8d4a18d025f88af4382b1cc99d864b498286b8de1c6fadb10a79e108"
---

# How to Leverage Tech to Verify Phone Numbers and Supercharge your Connect Rate

This post was originally published by Kalani Tissot on[his blog](https://www.tissotsolutions.com/post/how-to-verify-phone-numbers-and-supercharge-your-connect-rate) and has been reposted here with his permission.


It may sound obvious, but your candidates should only call verified phone numbers during call time. Wrong numbers are a waste of their time and lower the number of “actual” phone calls your candidate can make per call session.


To verify a phone number, campaign staff would historically dial every number by hand and try to reach the donor in question. Either the donor would pick up, and you’d be able to verify who they are by simply asking, or you would get lucky, and the donor will have a custom voicemail identifying who they are. Oftentimes, you won’t get either, and instead, you’ll just be sent to an anonymous voicemail with no real clue who the owner of the number is.


**This is an important process, but it’s an inefficient one.** Instead, campaigns should leverage cheap and simple tech tools to make the above process only necessary in select cases.


Here’s how our firm utilizes Airtable, Zapier, and two APIs (Twilio & Real Phone Validation) to verify phone numbers without having to call them (most of the time).


**To start, we keep all of our Nevada donor data in Airtable.** Information about our client’s donors and prospects is stored in one central database that we maintain and improve regularly. (You can signup for Airtable here:[https://airtable.com/invite/r/nCqZhSYk](https://airtable.com/invite/r/nCqZhSYk) )


This is separate from a client-specific CRM, such as NGP or[Numero](https://www.numero.ai/) , where we maintain & store all of their donor & donation data.


**Here’s a sneak peek at what this looks like:**


*(We have about 60 additional fields “columns” that are hidden in this view)*


There are four fields in our Airtable that we use to verify phone numbers. These are:


1.


**Phone – Mobile – Carrier – Type** (Displays whether the number is a landline, mobile, or voip)


2.


**Phone – Mobile – Caller – Type** (Shows whether the number is a consumer or business line)


3.


**Phone – Mobile – Caller – Name** (Returns the owner of the said phone number)


4.


**Phone – Mobile – Connection** (Tells us whether the number is connected or disconnected)


These four fields are automatically updated by two automations we set up (one for each API) that are triggered once a new mobile phone number is added to an Airtable record. Zapier will take that phone number, run it against each database, and return the corresponding values.


**These two APIs work like magic.**


At the cost of 1.5 cents per number, Twilio will return the line type, carrier, caller name, and caller type. By leveraging their phone number database, we will often immediately see the true owner of a number and know that it is verified. More information here:[https://www.twilio.com/lookup](https://www.twilio.com/lookup)


To filter out disconnected numbers from our call sessions, we use the Real Phone Validation API. At the cost of 1.9 cents per number, their API will tell us the likelihood of whether a phone number is connected or disconnected. Here’s how to signup for their service:[https://realphonevalidation.com/resources/api-documentation/](https://realphonevalidation.com/resources/api-documentation/)


*To save on cost, we recommend using their RPV Turbo API service over RPV Scrub.*


To connect these API services, we use Zapier to automatically input the returned API values back into our Airtable database.


Here are the two Zaps we use to connect Airtable with both of these APIs:


**Twilio API Zap:**[https://zapier.com/shared/a7552c4a81f8607228474bc7625b791ebe087423](https://zapier.com/shared/a7552c4a81f8607228474bc7625b791ebe087423)


**Real Phone Validation API Zap:**[https://zapier.com/shared/5405781fab7c906629c896cc7572d926eaa282f5](https://zapier.com/shared/5405781fab7c906629c896cc7572d926eaa282f5)


I’d also note that you could use Zapier to connect these APIs to a Google Sheet instead of Airtable.


**You’ll notice up above in the embedded Airtable that the Twilio API did not return the owner of my phone number.** That’s likely because I’ve only owned that number for a few months. The majority of the time, Twilio will return an owner of the number and help you quickly verify whether it belongs to the intended donor. But when it doesn’t, that’s when you can call that number and verify it the old-fashioned way.


By leveraging tech tools like Twilio, Zapier, and Airtable, you can save a ton of time on verifying phone numbers and ensure that your candidates are only calling the actual donors that you’d like them to speak with.


Do you use any API services to enrich donor data for your firm? I’d love to hear more about it. Please send me a note at[\[email protected\] .](https://www.numero.ai/cdn-cgi/l/email-protection#7a311b161b14133a2e130909150e0915160f0e1315140954191517)
