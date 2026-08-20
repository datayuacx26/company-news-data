---
schema_version: "1.0.0"
document_id: "a414939ea36bfb4402710f69ca161af9c35ca003bd782ff10863f069ab79b577"
company_key: "yc-hotglue"
company: "hotglue"
source_id: "yc-hotglue-news-import-0ffff35ff4c1"
canonical_url: "https://hotglue.com/blog/why-unified-apis-dont-work-for-crm-integrations"
published_at: null
first_seen_at: "2026-07-21T23:12:28.131979+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:9bb4d215698a64ffde8794c7cd5e838eae332f915dc58433aba0b1da5c956c37"
---

# Why Unified APIs don't work for CRM integrations

Unified APIs like Merge.dev or Vessel.dev are great in theory, but they can quickly fall apart when dealing with CRM integrations like Salesforce. Read on to learn why.


# First, what the heck are unified APIs?


Unified APIs are a simple concept: instead of dealing with the unique data models each vendor in a specific vertical provides, you map the vendor-specific data models to **unified models** that can be reused across all of them.


For example, when integrating with an HR system (like Workday, Gusto, Rippling, or Zenefits – to name a few), you may want to get **Employee** data. An **Employee** would be considered one data model, with some standard attributes that we can unify:


- First Name, Middle Name, Last Name
- Home Address
- Phone Number
- Email Address


Generally the fields we’ve listed above are always going to be part of an **Employee** data model, so it would be straightforward to map them from every HR system and serve it as part of a unified API. That way an engineer doesn’t have to go through all that work themselves!


This concept was popularized by the approach[Plaid](https://plaid.com/) took to unifying bank-level data, as bank transaction data generally has standard attributes as well and the work mostly lay in extracting that data from different bank websites and APIs.


# Why don’t Unified APIs work for CRMs?


CRMs like Salesforce are designed to be highly customizable and flexible – you can create custom objects, custom fields, and even custom relations between those objects. Unified APIs shine in verticals where data is more standard and similarly structured, but given the customizable nature of a CRM, it proves impossible to unify.


Here’s a simple scenario to illustrate this point. Imagine you’re working with CRM integrations and you want to get **Deal** data, specifically:


- how much each deal is worth (in dollars), and
- who the relevant contact on the deal is


In Salesforce the standard object to represent that is an **Opportunity** , whereas HubSpot calls it a **Deal** . A unified API such as Merge or Vessel can handle supporting each of these and mapping their fields it into a unified **Deal** model.


## Oh no, a custom field! 🫣


Enter our first hurdle: Company A uses Salesforce and instead of using the standard field to represent the dollar value of an Opportunity, their sales team use a custom field called` OpportunityLTV__c` . How can a unified API handle this?


The approaches vary, but essentially the best they can do is surface these custom fields to you, and allow you to handle “re-mapping” the data for Company A back to a standard model that works for you.


This quickly becomes difficult to scale: each user will have their own set of custom fields, and therefore will need their own unique adjustments made to the way data is being mapped. The complexity only grows exponentially as new users are added – and worse, if users change their custom fields (which they regularly do), you are forced to modify these mappings all over again.


This begins to defeat the purpose of having a unified API and a standard data model in the first place: you’re now building mappings yourself all over again.


## The big kahuna: custom objects


The real kicker with CRMs (especially ultra-customizable ones like Salesforce) are that they support using custom objects. There are *a lot* of companies that entirely rely on a set of custom objects and don’t use the standard CRM objects at all! In fact, there’s a whole world of Salesforce administrators whose entire job centers around managing custom objects, relationships, and fields (along with permissions & access to those).


In fact, Salesforce offers entire *packs* of objects for certain use-cases including their popular[Non-Profit Success Pack (NPSP)](https://www.salesforce.org/products/nonprofit-success-pack/) designed to add some objects that non-profits will need to use such as **Donations** (things a normal business wouldn’t need).


This is where a unified API really falls short – since these non-standard objects fall outside of the list of standard objects they support they don’t offer much support for integration use-cases where custom objects are central.


In our example above where we’re trying to get **Deals** and their dollar value, if we’re actually dealing with a non-profit whose deals are really **Donations** , a unified API is really going to hinder the integration process rather than make it simpler.


While some Unified APIs do give you a way to “pass-through” a GET request to query these custom objects, the work of mapping and consuming that data is left to you – and if you want to write custom objects you’re out of luck.


## Other considerations


The last important note for CRM integrations are when dealing with **relations** – they connect objects to each other. For example, a **Deal** typically has an **Owner** which is the sales rep managing a specific deal. CRMs allow for creating custom relation fields that link other objects (including custom objects), and this information can be vital to an integration: both when reading and writing data. Support for these may be extremely limited with unified APIs.


# Are there alternatives to unified APIs?


Yes of course! There are two other approaches outside of unified APIs that tend to work better for CRMs:


- Embedded ETL tools like[hotglue](https://hotglue.com/) provide a code-first approach that allow you (or your users) to select which custom objects/custom fields are relevant and map that data however you want. They handle both read and write, and support custom relations! This can be a great choice if you’re searching for an iPaaS that will grow with you and handle any use-case that gets thrown your way.
- Workflow Builders like[Tray.io](http://tray.io/) and Workato handle custom objects/fields better than unified APIs but are more focused on events-driven use-cases. For example a workflow you could setup would be: when a new Demo is booked in HubSpot, send a Slack message to our sales team. Keep in mind that workflow builders can be restrictive in terms of the depth of an integration.


# Conclusion


When developing a CRM integration, it’s important to ask yourself how deep your integration needs to be. Common considerations should be:


- How simple is our use-case? Are we really sure we just need to read standard objects/fields, or is there a chance we’ll need to grow beyond that?
- Will our users be leveraging custom fields? If so, is it going to be manageable to handle mapping those ourselves?
- Do our users need us to read or write data to custom fields? If so, a unified API might not be the best solution here


Regardless of what you choose, integration infrastructure is difficult to change once you’re locked in and have live users. Be sure to consider the questions above before committing to a solution that might not cover everything you need to do.


Best of luck with building and thanks for reading this article! I hope it helps your integration journey in some way :)


If you’re interested in learning more about hotglue, you can book a demo[here](https://hotglue.com/demo) .
