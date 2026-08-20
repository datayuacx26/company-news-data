---
schema_version: "1.0.0"
document_id: "b4a2c507a1bcbd71f8aa768633172e29285ca156f0da1441103ee90e3de49fa7"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/suppression-list-support"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T02:04:05.066719+00:00"
fetched_at: "2026-07-29T02:04:06.978250+00:00"
content_hash: "sha256:da5252d477fb5da874b72dc87e3f63064c99f34783a691b29623c733df3facab"
---

# Email Suppressions

When a mailbox provider hard-bounces a recipient, or someone marks your email as spam, continuing to send to that address harms your reputation.


Resend automatically suppresses addresses that hard bounce or complain, and today we're introducing **managed suppression lists** .


Take full control to view, add, and remove entries to your team's suppression list from both **the dashboard** and **the API** .


## How suppression works


An address is added to the suppression list for one of three reasons:


- ` bounce` : the recipient's mail server permanently rejected the email.
- ` complaint` : the recipient marked your email as spam.
- ` manual` : you add the recipient's address yourself.


## Add suppressions


You can add individual email addresses or bulk import a suppression list for your team.


Or add an email address to your team's suppression list programmatically.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  suppressions  .  add  (  {        email  :     'steve.wozniak@example.com'  ,     }  )  ;


```


If you're moving from another provider or have a larger list, you can also import a CSV.


Or bulk import up to 100 addresses via the API.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  suppressions  .  batch  .  add  (  {        emails  :     [  'steve.wozniak@example.com'  ,     'susan.kare@example.com'  ]  ,     }  )  ;


```


Suppressions apply to your **entire team** . A bounce or complaint on any domain suppresses that address across every one of your domains and subdomains.


Remember that adding a Suppression will *skip all sending* to that recipient until it is removed from your team's suppression list.


## View your suppression list


Within the Dashboard, access[Suppressions in the Email tab](https://resend.com/emails/suppressions) . Each row shows the suppressed address and its origin, links to the related contact, and offers an inline remove action.


You can also programmatically retrieve[a single Suppression](https://resend.com/docs/api-reference/suppressions/get-suppression) or[your entire suppression list](https://resend.com/docs/api-reference/suppressions/list-suppressions) , which returns key data about the Suppression.


```text
{        "object"  :     "suppression"  ,        "id"  :     "e169aa45-1ecf-4183-9955-b1499d5701d3"  ,        "email"  :     "steve.wozniak@example.com"  ,        "origin"  :     "bounce"  ,        "source_id"  :     "4ef9a417-02e9-4d39-ad75-9611e0fcc33c"  ,        "created_at"  :     "2026-10-06T23:47:56.678Z"     }
// `source_id` references the email id that triggered the suppression (is `null` for `manual` origin suppressions).
```


## Removing suppressed emails


If you've resolved the underlying issue that caused the Suppression, you can remove the address from the[Email tab](https://resend.com/emails/suppressions) , from the Contact's timeline, or from email's event.


You can also remove items from Suppressions individually by either` email` or suppression` id` or[bulk remove 100 at a time](https://resend.com/docs/api-reference/suppressions/remove-suppressions) .


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
// Remove by suppression id     const     {   data  ,   error   }     =     await   resend  .  suppressions  .  remove  (  'e169aa45-1ecf-4183-9955-b1499d5701d3'  )  ;
// Remove by email     await   resend  .  suppressions  .  remove  (  'steve.wozniak@example.com'  )  ;


```


Removing an address from the suppression list does not guarantee delivery. If it bounces or is marked as spam again, it will be suppressed again automatically. Remember that repeated bounces harm your sender reputation.


## Full webhook support


For maximum visibility, we've added two new webhook events you can track:


- ` suppression.added` : triggered when an address is added.
- ` suppression.removed` : triggered when an address is removed.


Listen to these events to respond in real-time within your own application.


The existing[email.suppressed](https://resend.com/docs/webhooks/emails/suppressed) fires whenever you send an email that is suppressed because it matches a suppression list entry.


## What you can build with it


- **Manual do-not-send control** : add or remove addresses yourself, individually or in bulk.
- **Sync with your own systems** : listen for the` email.suppressed` webhook to mirror suppressions in your database.
- **Audit and export** : review why an address was suppressed and export the full list from the dashboard.


## Conclusion


Suppression lists protect your sender reputation automatically, while keeping you in full control from both the dashboard and the API.


If you have any questions, please reach out to us and we'll be happy to help.
