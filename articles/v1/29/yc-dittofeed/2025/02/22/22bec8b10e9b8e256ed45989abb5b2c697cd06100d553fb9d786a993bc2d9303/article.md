---
schema_version: "1.0.0"
document_id: "22bec8b10e9b8e256ed45989abb5b2c697cd06100d553fb9d786a993bc2d9303"
company_key: "yc-dittofeed"
company: "Dittofeed"
source_id: "yc-dittofeed-news-import-a053879274d2"
canonical_url: "https://www.dittofeed.com/post/user-properties-rendering-properties-from-track-identify-events-in-email-sms-templates"
published_at: "2025-02-03T00:00:00+00:00"
first_seen_at: "2026-07-25T01:44:36.904212+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:9d69090f198761dbb579a2f360a131c6fa9af9856d8117530d7cf8fab329b56a"
---

# User Properties: Rendering Properties From Track & Identify Events In Email & SMS Templates

## What are user properties?


In Dittofeed, user properties are a feature that allow you to easily render properties from user events inside of email, SMS, and webhook templates. This is useful for personalizing automated messages to your users with data from their traits and actions. For example, you may want to send a billing update notification email after a user updates their credit card. These kinds of emails typically let the user know the last 4 digits of the new card. They may include things like the user's name and card type as well. With user properties, it's easy to render data like this inside of your message templates.


‍


## The difference between user trait properties and user performed properties


User properties rely on the events being fed to Dittofeed from your data source, whether that's Segment CDP, a data warehouse via reverse ETL, a Postgres database via one of our SDKs, or a variety of other data source options Dittofeed provides. These events can have any number of properties associated with them, which your engineering team can define in the events' structure.


If you're a nontechnical team member, event structure can get a bit into the weeds, but at a high level, there are two main categories of events that your users will end up submitting via your apps and sites: identifiy events and track events. Identify events have properties associated with user traits. These are generally more static properties like name and email address. Track events, on the other hand, have properties associated with actions the user has performed. These are generally more dynamic properties like what the last item was that a user purchased.


If, at a minimum, you know what kinds of data you'll need to render in an email template or any other kind of template– whether that's name, email, account manager, appointment time, billing info, or anything else– a technical team member will be able to help structure the properties in your app/site's identify and track events accordingly.


‍


## What user properties look like, and how to create them in Dittofeed


Below are some helpful resources for getting started with creating user properties in Dittofeed once your data is being submitted with the appropriate event structure for your templating needs. There are some helpful examples of what different event structures could look like, along with walkthroughs that show how to set up new user properties and render them in a newly created email template or SMS template.


### ‍


### ‍ **User Property Video Guides**


‍


**Trait User Properties**


‍


**Performed User Properties**


‍


### **User Property Documentation**


1. [Trait User Property Docs](https://docs.dittofeed.com/resources/user-property-types/trait)
2. [Performed User Property Docs](https://docs.dittofeed.com/resources/user-property-types/performed)
