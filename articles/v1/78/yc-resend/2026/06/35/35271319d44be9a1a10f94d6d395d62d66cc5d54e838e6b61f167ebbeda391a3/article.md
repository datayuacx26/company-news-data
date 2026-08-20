---
schema_version: "1.0.0"
document_id: "35271319d44be9a1a10f94d6d395d62d66cc5d54e838e6b61f167ebbeda391a3"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/csv-contacts-import"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:dc232b66ddb0bd32dffd928cb74f3b3dc36a5171d905c6f606063086b318ddf7"
---

# Import Contacts from CSV

For many, the entry point for Contacts is bulk uploading data from a previous provider or a third-party tool like a CRM.


While we've long supported CSV imports, the experience was limited. Today, we're announcing a new CSV importer that's **faster** , more **flexible** , and includes **full API coverage** .


- 300% increased file size
- 10x import speed improvement
- Full API or SDK support
- Intelligent column matching


## New dashboard experience


We've rebuilt the dashboard experience from the ground up.


1. Upload a CSV as large as 200MB.
2. Columns are automatically mapped to properties.
3. Confirm your import.


The dashboard uses AI to automatically match your CSV columns to existing properties and suggest new custom properties, so you spend less time wiring up fields by hand.


## Programmatic imports


You can now create and track imports programmatically with the[Contacts Import API](https://resend.com/docs/api-reference/contacts/create-contact-import) using the API or your preferred SDK.


```text
import     {   readFile   }     from     'node:fs/promises'  ;     import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const   file   =     new     Blob  (  [  await     readFile  (  'contacts.csv'  )  ]  ,     {        type  :     'text/csv'  ,     }  )  ;
const     {   data  ,   error   }     =     await   resend  .  contacts  .  imports  .  create  (  {      file  ,        columnMap  :     {          email  :     'Email'  ,          firstName  :     'First Name'  ,          lastName  :     'Last Name'  ,          properties  :     {            plan  :     {              column  :     'Plan'  ,              type  :     'string'  ,            }  ,          }  ,        }  ,        onConflict  :     'upsert'  ,        segments  :     [  {     id  :     '78e7a5c6-9a91-4c63-9d1f-3b9c0b5b9ab6'     }  ]  ,     }  )  ;


```


We've also added full support for imports to our MCP and CLI.


## Automatic contact properties


When importing Contacts via the dashboard or API, we will create Contact Properties for you automatically if they don't yet exist for your team.


In the dashboard, you can reject suggestions or provide your own.


In the API, provide your` properties` in the column map and new Contact Properties will be created on import.


```text
const     {   data  ,   error   }     =     await   resend  .  contacts  .  imports  .  create  (  {      file  ,        columnMap  :     {          email  :     'Email'  ,             properties  :     {               plan  :     {                 column  :     'Plan'  ,                 type  :     'string'  ,               }  ,             }  ,        }  ,     }  )  ;


```


## Update or skip existing contacts


Choose what happens when a Contact already exists via the API.


Set` on_conflict` to` skip` to leave existing contacts untouched, or` upsert` to update them, which makes bulk updates as easy as a re-import.


```text
const     {   data  ,   error   }     =     await   resend  .  contacts  .  imports  .  create  (  {      file  ,        columnMap  :     {          email  :     'Email'  ,          firstName  :     'First Name'  ,        }  ,           onConflict  :     'upsert'  ,     }  )  ;


```


## Import process tracking


Imports run in the background. Fetch any import programmatically to watch its status and get a live breakdown of what was created, updated, skipped, or failed.


```text
import     {   Resend   }     from     "resend"  ;
const   resend   =     new     Resend  (  "re_xxxxxxxxx"  )  ;
const     {   data   }     =     await   resend  .  contacts  .  imports  .  get  (        "479e3145-dd38-476b-932c-529ceb705947"  ,     )  ;


```


The response includes a` status` along with specific` counts` for more details.


```text
{        "object"  :     "contact_import"  ,        "id"  :     "479e3145-dd38-476b-932c-529ceb705947"  ,        "status"  :     "completed"  ,        "created_at"  :     "2026-05-15 18:32:37.823+00"  ,        "completed_at"  :     "2026-05-15 18:33:42.916+00"  ,        "counts"  :     {          "total"  :     1200  ,          "created"  :     800  ,          "updated"  :     300  ,          "skipped"  :     75  ,          "failed"  :     25        }     }
```


## Get started


We've continued to invest in our Contacts experience, adding[Contact Properties](https://resend.com/docs/dashboard/audiences/properties) ,[Segments](https://resend.com/docs/dashboard/segments/introduction) , and[Topics](https://resend.com/docs/dashboard/topics/introduction) to enable control, personalization, and better organization.


We hope this enhanced experience for bulk importing makes it easier to trust and use Resend.[View the docs](https://resend.com/docs/dashboard/audiences/contacts) for more details.
