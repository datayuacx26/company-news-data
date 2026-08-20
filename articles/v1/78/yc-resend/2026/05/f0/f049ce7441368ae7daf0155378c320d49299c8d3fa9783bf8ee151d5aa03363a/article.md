---
schema_version: "1.0.0"
document_id: "f049ce7441368ae7daf0155378c320d49299c8d3fa9783bf8ee151d5aa03363a"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/auth0-integration"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:1fd25891021d1a67c02b867cd8ce8911e378aaba2469a27cff09ee5b088cbb6f"
---

# Auth0 Integration

Authentication at scale is a difficult challenge, and Auth0 has made it approachable for teams of every size. Auth0 includes a built-in email provider, but also supports custom providers.


Today, **Auth0 adds Resend as a supported email provider** .


> We’re excited to bring the power and simplicity of Resend to our customers. This partnership makes it incredibly easy for developers to level-up their email delivery, whether they need a quick five-minute setup or a fully customized solution.
>
>
> Ian Hassard
>
>
> VP, Product Management, Auth0


Crucial emails, like verification codes, password resets, and more, need to be delivered to inboxes quickly and reliably. With the Resend connector, you can start sending these emails in just a few minutes.


Prefer a video walkthrough?


## Getting started


Before connecting Auth0 to Resend, you'll need two things:


1. A **verified domain** in your[Resend dashboard](https://resend.com/domains) . Auth0 will send from this domain, so make sure it matches your Auth0 custom domain.
2. An **API key** from your[API Keys page](https://resend.com/api-keys) .


## Basic configuration


In the Auth0 Dashboard, go to **Branding > Email Provider** , toggle on **Use my own email provider** , and select **Resend** .


- Set the **From** address to match your Resend verified domain.
- Enter your Resend API key.


To test that it's working, click **Send Test Email** . The email should be sent and will also appear in your Resend dashboard.


## Using Auth0 Actions


For more control over delivery, like per-organization sender addresses, conditional routing, or custom logic, you can use an Auth0 Action instead.


In the Auth0 Dashboard, go to **Actions > Library** , create a new Custom action, and add` resend` as a dependency. Store your API key as a secret named` RESEND_API_KEY` .


Here's a working example that sends a security alert on first login:


```text
const     {   Resend   }     =     require  (  'resend'  )  ;
exports  .  onExecutePostLogin     =     async     (  event  ,   api  )     =>     {        const   resend   =     new     Resend  (  event  .  secrets  .  RESEND_API_KEY  )  ;
if     (  event  .  stats  .  logins_count   ===     1  )     {          await   resend  .  emails  .  send  (  {            from  :     'Security <security@yourdomain.com>'  ,            to  :   event  .  user  .  email  ,            subject  :     'New login detected'  ,            html  :     `  <p>Hi   ${  event  .  user  .  name  }  , a new login was detected from   ${  event  .  request  .  ip  }  .</p>  `  ,          }  )  ;        }     }  ;
```


Deploy it by adding the action to the **Post Login** trigger under **Actions > Triggers** .


## Customizing Email Templates


To customize the email templates that Auth0 sends out, go to **Branding > Email Templates** in the Auth0 Dashboard and make your changes to the existing templates.


For more advanced templating, consider our guides on[React Email](https://resend.com/docs/send-with-auth0#react-email) or[Resend Templates](https://resend.com/docs/send-with-auth0#resend-templates) within Auth0.


## Start today


Check out[the Auth0 docs](https://auth0.com/docs/customize/email/smtp-email-providers/resend) for the full setup guide.


We're excited to see what you build with Resend and Auth0!
