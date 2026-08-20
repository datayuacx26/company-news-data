---
schema_version: "1.0.0"
document_id: "a54549d446d2cb26cf75b40dc334ffe9706524ccd88916c1e21301473910db65"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/introducing-templates"
published_at: "2025-11-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:d674c9d05ba8ed42a69fe9adb7d6deeb1fbef5f89c70725b3eeebb7b988b2f68"
---

# Introducing Templates

Today, we're launching **Templates** : a new way for your team to personalize emails.


Anyone on your team, from designers to marketers to developers, can create templates with our modern email builder.


Define the structure and style of your template to communicate your brand. Then, add variables to personalize the email.


## Creating a new Template


One way to create a new Template is through the dashboard.


You can also import your existing HTML or React Email code. Alternatively,[create a Template via the API](https://resend.com/docs/api-reference/templates/create-template) .


All Templates are stored in your Resend account on the[Templates](https://resend.com/templates) page.


## Template variables


You can create **template variables** to personalize email content. When you send the email, Resend will replace the variables with the actual values you've attached to your Contacts.


Variables can be` strings` or` numbers` . You can set a fallback value, which will be used if a value is not provided.


Use these variables in your Template's body, subject, reply to, and preview text fields.


When creating a Template using the API, use the` variables` parameter to define the template variables.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
await   resend  .  templates  .  create  (  {           name  :     'order-confirmation'  ,        from  :     'Wayback Store <hi@orders.waybackstore.com>'  ,        subject  :     'Thanks for your order!'  ,        html  :     "<p>Name: {{{PRODUCT}}}</p><p>Total: {{{PRICE}}}</p>"  ,           variables  :     [             {               key  :     'PRODUCT'  ,               type  :     'string'  ,               fallbackValue  :     'item'             }  ,             {               key  :     'PRICE'  ,               type  :     'number'  ,               fallbackValue  :     20             }           ]     }  )  ;


```


## Collaboration


Your entire team can collaborate in realtime to craft the perfect email template together. Anyone can create variables and use them in the visual editor.


When you send a test email, you can add example values to see how the email will look to a real user.


## Versioning


Templates are living documents, so we've introduced a versioning system. As you refine your voice, update your design, or tweak your copy, you can freely iterate and collaborate with your team with confidence.


We track changes and keep previous versions, in case you need them.


Once you've finalized your Template, publish it to make it available for sending.


This publishing experience puts you in control. Let's imagine your team is going through a rebrand. You can make edits to your Templates in production without the risk of leaking your new logo. When you're ready, publish the new version without any additional code changes.


## Send emails with Templates


Now that your Template is stored on Resend, just pass the template ID and variables. We'll handle the rest.


The` template` parameter replaces the` html` or` react` parameters that you'd normally use for sending.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
await   resend  .  emails  .  send  (  {        from  :     'Wayback Store <hi@orders.waybackstore.com>'  ,        to  :     'customer@example.com'  ,           template  :     {             id  :     'order-confirmation'  ,             variables  :     {               PRODUCT  :     'Vintage Macintosh'  ,               PRICE  :     499             }           }     }  )  ;


```


## Get started today


The new Resend templating experience streamlines the handoff between marketing and development. No more waiting for a teammate to pick up a ticket and push a code update.


We can't wait to see how this will enable your team to move faster, together.


Visit the dashboard or[API reference docs](https://resend.com/docs/api-reference/templates) to get started.
