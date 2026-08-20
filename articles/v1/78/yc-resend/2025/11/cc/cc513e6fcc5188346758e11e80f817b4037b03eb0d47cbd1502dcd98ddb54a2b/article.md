---
schema_version: "1.0.0"
document_id: "cc513e6fcc5188346758e11e80f817b4037b03eb0d47cbd1502dcd98ddb54a2b"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/new-contacts-experience"
published_at: "2025-11-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:e3849af1d024b8e955e89de1aa588ca1a22f1b05294ca62beba1ee9a168de63b"
---

# New Contacts Experience

Today, we're excited to announce some new features for[Contacts](https://resend.com/docs/dashboard/audiences/contacts) . These improvements will supercharge your ability to personalize and segment your Contacts on Resend.


## Custom Contact Properties


In addition to the standard fields you can already set on a Contact, such as` FIRST_NAME` and` LAST_NAME` , you can now store custom properties on your Contacts.


These properties can be used to personalize your Broadcasts across all Segments.


Properties can have a fallback value, which will be used when a Contact doesn't yet have an explicit value for the property.


You can create Contact properties[via the API or SDKs](https://resend.com/docs/api-reference/contact-properties/create-contact-property) .


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  contactProperties  .  create  (  {          key  :     'company_name'  ,          type  :     'string'  ,          fallbackValue  :     'Acme Corp'  ,        }  )  ;


```


## Assign Custom Values to Contacts


Once you've created a property, you can assign values for it to any Contact.


Of course, you can also assign values to Contact properties[using the API or SDKs](https://resend.com/docs/api-reference/contacts/create-contact) .


## Use Contact Properties in Broadcasts


Properties can then be used to personalize your broadcasts. If you do not provide a Contact property value, the fallback value will be used.


Properties can also be used in[Broadcasts sent using the API](https://resend.com/docs/api-reference/broadcasts/create-broadcast) .


## Contact Activity History


We've introduced a new activity history view for Contacts to give you a comprehensive timeline of all events related to a Contact, including when it was created, unsubscribed or received an email.


## Segmenting Contacts


Previously, each Contact you created could only be part of a single Audience. This meant if you wanted to segment a Contact into multiple Audiences, you had to create a new Contact with the same email address each time.


Now, we're making it more flexible. A Contact can now be a part of zero, one, or multiple Audiences as needed. A Contact that is in more than one Audience now counts as a single Contact in your quota.


And to make this distinction clearer, we've renamed Audiences to Segments.


Now that Contacts don't belong to a single Segment, you can reference them directly in your API calls. We have a new` /contacts` endpoint that allows you to see and manage your contacts, and our SDKs have been updated to not require the` audience_id` parameter for these requests.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  contacts  .  list  (  )  ;


```


## Global Contact Model


Starting today, Contacts are now global entities, identified by their email address.


How does this help you?


- You get a more accurate view of your interaction with each Contact
- You can personalize your broadcasts to each Contact
- You can segment your Contacts as you need
- You can assign a Contact to zero, one, or multiple Audiences
- We count your Contacts once, even if they are in multiple Audiences


## Conclusion


We can't wait to see how you use these new features to send more personalized emails to your contacts. We have more updates planned for Contacts, so stay tuned!


For more information on how to use Contacts, including a[migration guide](https://resend.com/docs/dashboard/segments/migrating-from-audiences-to-segments) for those who are already using Audiences for segmentation, check out the[new Contacts documentation](https://resend.com/docs/dashboard/audiences/introduction) .
