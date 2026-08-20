---
schema_version: "1.0.0"
document_id: "44ec423dfcd3ef1ac650ec7bccfcaaf19b11773161b827b57e21f538e4f97ab7"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/introducing-custom-tracking-domain"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:7688c43eb66fe4de62201e54263296ac679e04ee33c555fd3e53cece8a3c4d3d"
---

# Custom Tracking Domains

Tracking can give you a clear picture of how your emails are performing.


But email tracking typically relies on shared domains, meaning reputation is shared as well. If others using your domain are not sending emails responsibly, it can impact your deliverability.


Today, we're launching[custom tracking domains](https://resend.com/domains) , free for all Resend users.


## How it works


To get started, go to[Domains](https://resend.com/domains) and select the **Configure** tab for your domain.


Add the required record to your DNS settings and Resend will automatically route all tracking through your custom domain.


Both features are configured at the domain level, and both are disabled by default.


## Two types of tracking


On Resend, you can enable two different types of tracking:


### 1. Click tracking


When you enable click tracking, Resend sets up a **redirect for each link** in your email. When a recipient clicks, the action is recorded and they are immediately forwarded to the original URL.


### 2. Open tracking


When you enable open tracking, Resend **embeds a 1x1 pixel transparent image** in your emails. When a recipient's mail client downloads it, Resend records an open event.


## Why a custom domain matters


Inbox providers like to see consistency between your sending domain and the content of your emails.


When tracking links pass through a domain you own, rather than a shared one, inbox providers notice. And that alignment is better for deliverability.


It also means your recipients never see an unfamiliar domain in the links they click.


## Set up custom tracking via the API


Of course, you can also enable custom tracking when creating a domain via the API.


```text
import     {   Resend   }     from     'resend'  ;
const   resend   =     new     Resend  (  're_xxxxxxxxx'  )  ;
const     {   data  ,   error   }     =     await   resend  .  domains  .  create  (  {        name  :     'example.com'  ,        openTracking  :     true  ,        clickTracking  :     true  ,        trackingSubdomain  :     'links'  ,     }  )  ;


```


You can also verify the[tracking domain via the API](https://resend.com/docs/api-reference/domains/verify-domain) .


## Pair with webhooks


Custom tracking is most powerful when combined with[webhooks](https://resend.com/docs/webhooks/introduction) . Every open and click fires an event in real time, so you can:


- Measure performance
- Segment your audience
- Refine your emails


## Get started today


Custom tracking domains are free for all Resend users.


Visit the[custom tracking domain docs](https://resend.com/docs/dashboard/domains/tracking#custom-tracking-domain) to get started.
